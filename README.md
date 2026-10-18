# Landmine Victims in Colombia: A Reproducible Data Workflow

A reproducible Python workflow that loads, cleans, explores and charts Colombia's official register of people killed or injured by landmines and unexploded ordnance (12,390 records, 1990–2023).

I chose this register because the subject is close to my heart. I have worked in humanitarian mine action and arms control for 25 years, as an explosive ordnance disposal technician and a chemical weapons inspector, and I want to find ways artificial intelligence can help save lives in the field and help war-torn countries become prosperous places. This is the first project of my AI capstone on humanitarian mine action, and later projects reuse its cleaning functions.

## What I built
- `data_workflow.ipynb`: setup; ingestion with a checksum check; a six-step cleaning pipeline with a data dictionary, a log and checks; exploratory analysis; four charts with interpretations; and a summary.
- `victimas_minas_antipersonal_clean.csv` and `figures/`: the cleaned table and the charts, both written by the notebook.

## Dataset
*Situación Víctimas Minas Antipersonal en Colombia*, Presidencia de la República de Colombia, Portal Nacional de Datos Abiertos: https://www.datos.gov.co/Inclusi-n-Social-y-Reconciliaci-n/Situaci-n-V-ctimas-Minas-Antipersonal-en-Colombia/yhxn-eqqw

- Snapshot: `victimas_minas_antipersonal.csv`, downloaded on 2026-10-18 from the portal's CSV export; SHA-256 `6ac13df53d0024bc9d96f8b48d7622934b5bd1a334d40cf4155bf4cea804a84d`.
- Licence: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The raw and cleaned CSV files in this repository are shared under the same licence.

## How to run
Needs Python 3.13 and Git. `requirements.txt` was generated on Python 3.13.0.

```
git clone https://github.com/travisrcarlson/ai-programming-foundations-project.git
cd ai-programming-foundations-project
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook data_workflow.ipynb
```

On macOS or Linux, activate with `source .venv/bin/activate`. If PowerShell blocks the activation script, run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` once. In Jupyter, choose **Kernel > Restart Kernel and Run All Cells**. After adding or upgrading packages, record the environment again:

```
pip freeze > requirements.txt
```

## Reflections
- **Where poor cleaning could introduce bias:** mistakes here mainly change how big the problem looks. If victims are mislabelled or under-counted, a region can look safer than it is, and funding and clearance resources can go to the wrong places. Land release itself is a detailed, deliberate process that doesn't depend on these exact numbers, but the way resources are shared between regions does. In this file the risks are deleting the 3,858 repeated rows (31%) as duplicates, which would remove 37.4% of security-force victims but only 22.0% of civilians; reading the swapped `estado` and `genero` labels at face value; counting "Sin información" as a category; and matching municipalities by name, since 21 names belong to more than one department. The notebook keeps the repeated rows, checks the labels against their values, keeps unknowns visible and uses the official codes.
- **How the workflow would change for machine learning:** a model could help decide where to target humanitarian mine action and where to send medical equipment, supplies and funding. Before training one, I would split the data by year or by place rather than at random, so repeated rows can't appear in both the training and the test data; judge the model with measures suited to a rare outcome, such as precision and recall, since only 19% of victims were killed; add other data by municipality code, such as recorded hazardous areas and population, so the model learns more than reporting patterns; and compare Colombia with other countries and war-torn areas.
- **Preparing the data for a neural network:** most columns are categories, so I would encode them as numbers: one-hot columns for small sets such as condition and age group, and learned embeddings for places, which have many values. I would keep "not recorded" as its own category or flag, so the network can tell an unknown value from a known one.
- **Where agentic automation could help:** an agent could watch the portal for new releases, download them and compare the checksum, rerun the cleaning pipeline and its checks, and report what changed. It could also compare the register with other sources and flag municipalities whose numbers look low, because data are hard to capture in war-torn areas and under-reporting keeps problems from surfacing. A person should approve any label change, new category or published result. In future, the register could be matched with mapping software to help target drone surveys.
