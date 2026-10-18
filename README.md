# Landmine Victims in Colombia: A Reproducible Data Workflow

A reproducible Python workflow that loads, cleans, explores and charts Colombia's official register of people killed or injured by landmines and unexploded ordnance (12,390 records, 1990–2023). It is the first project of an AI capstone on humanitarian mine action, and later projects reuse its cleaning functions.

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
- **Where poor cleaning could introduce bias:** 3,858 rows repeat another row exactly, because the file has no victim or event ID and several people hurt in one accident look identical. Deleting them as duplicates would remove 37.4% of security-force victims but only 22.0% of civilians, changing every comparison between the two groups. Reading the swapped `estado` and `genero` labels at face value, counting "Sin información" as a category, or matching municipalities by name (21 names belong to more than one department) would also distort the results.
- **How the workflow would change for machine learning:** choose a target, such as killed or injured; split by time or by municipality rather than at random, so repeated rows and places can't leak between training and test data; fit the cleaning and encoding steps on training data only; and report metrics suited to an imbalanced target, such as precision and recall.
- **Preparing the data for a neural network:** encode categories (one-hot or learned embeddings for department), scale numeric fields, keep missing values explicit, and use class weights or balanced batches, since only 19% of victims were killed. A table this small and categorical may suit a tree-based model better.
- **Where agentic automation could help:** an agent could check the portal on a schedule, download a new release, compare its checksum, rerun the pipeline and its checks, and hand any new categories or label changes to a person before anything is published. The pipeline's assertions give it clear points to stop.
