# نظام إدارة صيانة الأجهزة الطبية (CMMS)

A local-network, Arabic RTL CMMS MVP built with Python, Streamlit, SQLAlchemy, and SQLite. It is intended to run on a hospital server and does not call external services at runtime.

## Requirements

- Python 3.11+
- One PC/server on the hospital LAN; a UPS is recommended
- Internet is only needed to install Python packages initially, unless packages are installed from a local wheelhouse

## Install and run (Linux)

```bash
cd cmms
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m seed.seed_demo
streamlit run app/main.py --server.address 0.0.0.0 --server.port 8501
```

## Install and run (Windows PowerShell)

```powershell
cd cmms
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -m seed.seed_demo
streamlit run app/main.py --server.address 0.0.0.0 --server.port 8501
```

Open `http://SERVER-IP:8501` from a browser on the hospital Wi-Fi/LAN. Restrict port 8501 to the hospital network using the server firewall. For first boot, the demo seed creates these accounts:

| Username | Role | Temporary password |
|---|---|---|
| `supervisor` | Supervisor | `ChangeMe123!` |
| `engineer1` | Biomedical engineer | `ChangeMe123!` |
| `engineer2` | Biomedical engineer | `ChangeMe123!` |
| `nurse1` | Nurse — ICU | `ChangeMe123!` |
| `nurse2` | Nurse — Internal Medicine | `ChangeMe123!` |
| `nurse3` | Nurse — Emergency | `ChangeMe123!` |
| `procurement` | Procurement | `ChangeMe123!` |

All accounts require a password change at first login. Change demo credentials before using real data. Seed data is only for testing and must not be treated as a validated clinical checklist.

## Database and backups

The default database is `data/cmms.db` (override with `CMMS_DB_PATH`). SQLite foreign keys, WAL, and a 5-second busy timeout are enabled. Stop the app before making a manual copy. For a consistent online backup, use SQLite's backup API; do not copy only the main DB file while WAL is active. Store backups separately and test restore procedures.

## Tests

```bash
pytest -q
```

## Current implementation scope

The runnable starter includes the schema, login/roles, forced password change, device list/create, nurse fault reporting and checklists, work-order triage/completion, basic procurement queue, notifications, KPI overview, CSV device export, demo data, and unit tests for the priority matrix, shift windows, and status transitions.

This first generated build is **not yet a validated clinical deployment**. Remaining work before operational use includes full service-layer permission test coverage, background worker jobs for escalation/missed inspections/PM generation/backup rotation, complete user/departments/settings CRUD and report exports, audit review, Alembic initial migration, hardening of session persistence and upload handling, broader acceptance tests, and hospital staff validation of risk rules/checklists/SLA values. Do not use it as the sole safety control for medical equipment.
