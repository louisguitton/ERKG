# ERKG with the ICIJ Offshore Leaks dataset

## Set up local environment

After cloning this repo, connect into the `ERKG/icij` directory and set up
your local environment:

```bash
git clone https://github.com/DerwenAI/ERKG.git
cd ERKG
git checkout kgrag
cd icij

python3.11 -m venv venv
source venv/bin/activate

python3 -m pip install -U pip wheel
python3 -m pip install -r requirements.txt 
```

We're using Python 3.11 here, although this code should run with most
of the recent Python 3.x versions.

### Set up Neo4j

Follow instructions to download and install Neo4j Desktop with the GDS
plugin added:
<https://neo4j.com/developer-blog/entity-resolved-knowledge-graphs/>

Be sure to create a local file `.env` in the `icij` subdirectory which
has your Neo4j credentials as described in the article.

### Download the ICIJ data

Follow instructions to dowload the ICIJ dataset and load it into Neo4j:
<https://offshoreleaks.icij.org/pages/database>

Then launch Jupyter:

```bash
./venv/bin/jupyter lab
```

---

## Load the ICIJ+Neo4j+Senzing prepared data

### Run the data loading notebook

Download and unzip the Senzing overlay:
<https://storage.googleapis.com/erkg/icij/ICIJ-entity-report-2024-06-21_12-04-57-std.json.zip>

Then follow the steps shown in this notebook:
`icij.ipynb`

Depending on your laptop resources and the Neo4j version, this may
take approximately 6 hours to load the Senzing entities plus ~1.6M
edges connecting nodes for the ICIJ data records.

You can double-check results by viewing in Neo4j Bloom.

---

## Rework the ICIJ connected data

Many of the connections in the ICIJ dataset were based on simplistic
string matching of names. Some of these are clearly wrong, and it's
likely there were many false negatives as well.

Prepare the data using this notebook:
`prep_data.ipynb`

To load into KùzuDB,
`load_kuzu.ipynb`
(1 min)

To laod into Neo4j Desktop via GDS:
`load_neo4j.ipynb`
(4 min)
