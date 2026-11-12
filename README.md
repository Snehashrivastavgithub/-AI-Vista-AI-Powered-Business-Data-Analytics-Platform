# AI VISTA

**AI-Powered Business Data Analytics and Intelligent Reporting System**

AI VISTA is a complete, local-first business analytics application built with **Python, Django, MySQL, Pandas, NumPy, SciPy, scikit-learn, Chart.js, ReportLab and python-pptx**. It turns CSV, Excel and PDF data into auditable statistics, interactive charts, grounded business insights, forecasts and downloadable PDF / PowerPoint / Excel / CSV reports.

The application is deliberately **not** a front-end mock-up. Uploads, database records, calculations, charts, forecasts, assistant answers, reports and admin actions all execute against real data in MySQL.

---

## 1. What is implemented

| Area | Implementation |
|---|---|
| Authentication | Registration, login, logout, password change, secure password hashing (scrypt), rate-limited login |
| Roles | Admin and User roles with permission flags; admin-only routes; last-admin lockout protection |
| Data management | CSV, XLSX, XLS and text-PDF imports; preview; search; filters; sort; pagination; add/edit/delete rows; CSV/Excel export |
| Statistics | Mean, median, mode, percentage, growth rate, variance, standard deviation, min, max, sum, average, correlation, linear regression |
| Visualisation | Chart.js bar, line, pie, doughnut, scatter, comparative and time-series charts; selectable chart type |
| Forecasting | scikit-learn linear trend and calendar-month regression, held-out backtest, R² / MAPE / MAE / RMSE, confidence band |
| AI assistant | Dataset-grounded Q&A, summaries, rankings, change explanations, anomalies, recommendations and forecasts; refuses unsupported questions |
| Business management | Company profiles, partners, rates, contracts, revenue contribution, performance scores, partnership history and comparison charts |
| Reports | PDF, PPTX, XLSX and CSV; overview, statistics, charts, forecast, insights, recommendations and partner performance |
| Admin | Custom admin panel, user management, role management, dataset monitoring, audit trail, security monitor, system health |
| Security | CSRF, ORM-only queries, XSS escaping, CSP/security headers, upload validation, per-user object access, audit logs, session hardening |

---

## 2. Screenshots

The project includes a premium dark navy / purple / blue SaaS interface.

| Executive dashboard | Analytics studio |
|---|---|
| ![Dashboard](docs/screenshots/dashboard.png) | ![Analytics](docs/screenshots/analytics.png) |

| AI assistant | Landing page |
|---|---|
| ![Assistant](docs/screenshots/assistant.png) | ![Landing](docs/screenshots/landing.png) |

---

## 3. Technology stack

- **Backend:** Python 3.11, Django 5.2 LTS
- **Database:** MySQL 8.0 through PyMySQL (pure-Python MySQLdb-compatible driver)
- **Data:** Pandas, NumPy
- **Statistics / ML:** SciPy, scikit-learn 1.5.2
- **Charts:** Chart.js 4 (bundled locally)
- **Frontend:** HTML5, CSS3, JavaScript, Bootstrap 5.3 (bundled locally), Bootstrap Icons
- **PDF:** ReportLab + Matplotlib
- **PowerPoint:** python-pptx
- **Excel:** OpenPyXL, XlsxWriter
- **PDF extraction:** pdfplumber, pypdf, optional pytesseract + Tesseract OCR
- **Environment:** python-dotenv, Visual Studio Code

> `scikit-learn` is pinned to **1.5.2** because newer compiled wheels were blocked by Windows Application Control on the development machine. The application code uses standard scikit-learn APIs and can run newer versions when the host policy allows it.

---

## 4. Project structure

```
AI-VISTA/
├── config/                 # Django settings, root URLs, WSGI/ASGI
├── accounts/               # Custom user, roles, login/registration, throttling
├── core/                   # Landing, health, template tags, management commands, shared signals
├── dashboard/              # Executive KPI aggregation and dashboard
├── datasets/               # Dataset models, import services, CRUD, access control, exports
├── analytics/              # Statistical engine, regression, forecast, analysis models
├── assistant/              # Grounded assistant, AI insights, conversations
├── business/               # Business profiles, partners, partnership history
├── reports/                # PDF / PPTX / XLSX / CSV generators
├── panel/                  # Custom admin panel, users, roles, audit, security
├── audit/                  # Audit log model, logging helper, security headers middleware
├── templates/              # Django templates (dark premium UI)
├── static/                 # App CSS/JS and vendored Bootstrap, Chart.js, Bootstrap Icons
├── tests/                  # Django test suite
├── scripts/                # MySQL, setup, run, smoke and screenshot helpers
├── docs/screenshots/       # README screenshots
├── media/                  # Uploaded datasets and generated reports (git-ignored)
├── logs/                   # Application logs (git-ignored)
├── manage.py
├── requirements.txt
├── .env.example
└── README.md
```

---

## 5. Quick start (Windows / VS Code)

### Prerequisites

- Windows 10/11
- Python **3.11** (64-bit) with `python` / `py -3.11` available
- MySQL Server 8.0 installed at `C:\Program Files\MySQL\MySQL Server 8.0`
- Visual Studio Code with the Python extension

### Option A — one command setup

Open a terminal in the project folder:

```powershell
.\scripts\setup.cmd
.\scripts\run.cmd
```

Then open **http://127.0.0.1:8000/**.

`setup.cmd`:

1. creates `.venv` with Python 3.11;
2. installs `requirements.txt`;
3. creates `.env` if missing;
4. starts the dedicated MySQL instance on port **3307**;
5. creates the `ai_vista` database and application user;
6. runs migrations;
7. seeds the demo workspace.

`run.cmd` starts MySQL if necessary, applies migrations and runs the Django development server.

### Option B — manual VS Code flow

1. **Open the folder** in VS Code.
2. Select the interpreter **`.venv\Scripts\python.exe`** (`Ctrl+Shift+P` → “Python: Select Interpreter”).
3. Create the environment and install dependencies:

   ```powershell
   py -3.11 -m venv .venv
   .\.venv\Scripts\python.exe -m pip install -r requirements.txt
   Copy-Item .env.example .env
   ```

4. Start MySQL (dedicated instance on 3307):

   ```powershell
   powershell -ExecutionPolicy Bypass -File .\scripts\start_mysql.ps1
   ```

5. Apply migrations and seed:

   ```powershell
   .\.venv\Scripts\python.exe manage.py migrate
   .\.venv\Scripts\python.exe manage.py seed_demo
   ```

6. Run the server:

   ```powershell
   .\.venv\Scripts\python.exe manage.py runserver
   ```

7. Open **http://127.0.0.1:8000/**.

> If PowerShell blocks local scripts, use the `.cmd` wrappers or run:
> `powershell -ExecutionPolicy Bypass -File .\scripts\setup.ps1`.

### Demo accounts

| Role | Username | Password |
|---|---|---|
| Admin | `admin` | `Admin@12345` |
| User / analyst | `demo` | `Demo@12345` |

The demo workspace includes a **1,502-row, 14-column retail sales dataset**, a business profile, six partners, grounded AI insights and a sample PDF report.

Additional samples are included in the archive:

- `docs/sample_data/shopping_trends.csv` — the 3,900-row Shopping Trends dataset used for the revenue and PowerPoint demo.
- `docs/sample_reports/Shopping_Trends_Revenue_Report.pptx` — a generated PowerPoint with revenue KPI cards, a revenue-by-category bar chart and a category-share pie chart.



Create the database and grant privileges (run as a MySQL administrator):

```sql
CREATE DATABASE ai_vista CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'ai_vista'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON ai_vista.* TO 'ai_vista'@'localhost';
FLUSH PRIVILEGES;
```

Then run `manage.py migrate`.

### Why PyMySQL?

The project uses **PyMySQL**, a pure-Python MySQLdb-compatible driver, because the native `mysqlclient` DLL was blocked by Windows Application Control on the development machine. Django's MySQL backend works through `pymysql.install_as_MySQLdb()` in `config/__init__.py`. On a normal host you may install `mysqlclient` instead if you prefer.

---

## 7. Data formats, schema detection and PDF handling

### CSV
- Encoding detection: UTF-8 with/without BOM, CP1252, Latin-1
- Delimiter detection: comma, semicolon, tab, pipe
- Header row expected; duplicate/blank column names are cleaned
- Currency symbols, commas and parentheses are parsed into numeric values where appropriate
- IDs/codes/phone/postal columns are preserved as text

### Excel
- `.xlsx` via OpenPyXL; `.xls` via xlrd
- The largest readable sheet is imported
- Native Excel dates are preserved

### PDF
- Text-based PDF tables are extracted with **pdfplumber**
- Table extraction is attempted first; a text-layout parser is used as fallback
- Scanned/image-only PDFs can be processed with **OCR** if the Tesseract engine is installed:
  1. Install Tesseract OCR for Windows.
  2. Add `tesseract.exe` to `PATH`.
  3. `pytesseract` is already in `requirements.txt`.
- If OCR is unavailable, the UI returns a clear, actionable error instead of pretending the import succeeded.

### Revenue detection
AI VISTA recognises common revenue columns automatically: `Revenue`, `Sales`, `Net Sales`, `Income`, `Turnover`, `Amount`, `Purchase Amount (USD)`, `Order Amount`, `GMV` and similar. For a retail dataset such as **Shopping Trends**, `Purchase Amount (USD)` becomes the revenue metric, so the dashboard, category charts, pie charts and PowerPoint report all populate without renaming the file. If no expense column exists, profit and margin are shown as **Not provided** rather than fabricated.

### Import limits and scalability
- Security upload limit: `MAX_DATASET_UPLOAD_MB=50` (configurable)
- No arbitrary fixed **record** limit
- Rows are stored in batches of 1,000; the table is paginated and searchable
- Statistical analysis uses at most `ANALYSIS_MAX_ROWS=250,000` rows by default to protect server memory; this is configurable. The UI reports when only a sample was analysed.
- Excel export is limited only by the Excel format limit (1,048,576 rows); use CSV for larger exports.

---

## 8. Database design

Django migrations create a normalized MySQL schema:

| Table | Purpose | Key relationships |
|---|---|---|
| `accounts_role` | Role definitions and permission flags | 1 → many users |
| `accounts_user` | Custom user, role, profile, last login IP | FK role |
| `accounts_loginattempt` | Login telemetry for security monitoring | — |
| `datasets_datasetcategory` | User-defined dataset categories | FK owner |
| `datasets_dataset` | Uploaded file, metadata, status, cached KPIs/quality | FK owner, category |
| `datasets_datasetcolumn` | Column name, type, null/unique counts, statistics | FK dataset |
| `datasets_datarecord` | One row of data as MySQL JSON + search blob | FK dataset, unique (dataset, row_number) |
| `analytics_analysisresult` | Saved analysis runs and JSON results | FK dataset, user |
| `analytics_savedchart` | Saved chart configurations | FK dataset, user |
| `assistant_aiinsight` | Grounded insight records | FK dataset |
| `assistant_conversation` / `assistant_message` | Assistant history | FK user, dataset |
| `business_businessprofile` | Company details and fiscal context | FK owner |
| `business_partner` | Partner rate, contract, revenue, score, status | FK business |
| `business_partnershiphistory` | Events and value changes | FK partner |
| `reports_report` | Report metadata and generated file | FK owner, dataset, business |
| `audit_auditlog` | Immutable activity trail | FK user (nullable) |

Constraints, indexes and foreign keys are defined in the models and verified by migrations.

---

## 9. AI and hallucination controls

AI VISTA does **not** send data to an external language model. The “AI assistant” is a **grounded analytics layer**:

1. Pandas / NumPy / SciPy / scikit-learn compute the facts.
2. Intent detection selects a supported analytical operation.
3. Answers are generated from templates using those exact facts.
4. Every answer returns `facts`, `sources`, `confidence` and an `intent`.
5. If the question cannot be mapped to a computation, the assistant **refuses to guess** and explains what it can do.
6. Forecasts expose the method, backtest R², MAPE, MAE, RMSE and a range; they are labelled as estimates.
7. The UI has a visible **Grounded mode** badge and a “Why this is safe” panel.

This directly addresses the risk of hallucination: numerical claims can only come from computed results, and unsupported questions produce an explicit limitation message rather than invented content.

---

## 10. Security and data-protection posture

- **Authentication:** Django auth, scrypt password hashing, password complexity validators, session rotation on login
- **Brute-force protection:** cache-backed failed-login throttle (5 attempts / 5 minutes by default), login telemetry and blocked-event audit entries
- **Authorization:** explicit Admin/User roles, `admin_required` route guard, object-level dataset ownership checks
- **CSRF:** all HTML forms include `{% csrf_token %}`; assistant fetch sends `X-CSRFToken`
- **SQL injection:** Django ORM only; no user-controlled raw SQL
- **XSS:** template auto-escaping; chart data passed with `json_script`; assistant responses inserted with `textContent`
- **Upload security:** extension allow-list, magic-byte checks, size limit, sanitized original filename, UUID storage path, content-type checks, no execution of uploads
- **Headers:** CSP, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`, frame denial, safe cross-origin policy
- **Sessions:** HttpOnly, SameSite=Lax, 8-hour lifetime, secure cookies when `DJANGO_SECURE_SSL=1`
- **Audit:** logins, failed logins, uploads, exports, record changes, analyses, assistant queries, reports, role changes and blocked access
- **Data isolation:** users only see their own datasets, business profiles, partners, reports and conversations; administrators can access all data through the admin panel
- **Deletion:** dataset/report files are removed from storage when their database rows are deleted (post-delete signals)
- **No third-party transfer:** all processing is local; there is no external AI API call

### Regulatory note

AI VISTA provides technical controls that support responsible handling of business data (least privilege, auditability, deletion, local storage, consent acknowledgement at registration). It is **not** a legal compliance certification. Before production use in a regulated environment, the deploying organisation must define:

- lawful basis and consent/notice;
- retention periods and deletion policy;
- data-processing agreements with hosting/backup providers;
- access reviews and incident response;
- encryption at rest, backups, TLS termination and network controls;
- any sector-specific obligations (for example financial, health or personal-data rules).

---

## 11. Running tests

```powershell
# Django system check
.\.venv\Scripts\python.exe manage.py check

# 7 automated Django tests (auth, upload, CRUD, isolation, analytics, assistant, report, throttle)
.\.venv\Scripts\python.exe manage.py test tests -v 2

# Full page/API smoke test against the demo database
.\.venv\Scripts\python.exe scripts\smoke.py

# Deep upload / CRUD / XSS / PDF / Excel / assistant test
.\.venv\Scripts\python.exe scripts\deep_test.py
```

The tests create and destroy a `test_ai_vista` MySQL database. `scripts/start_mysql.ps1` grants the application user privileges on that test database.

---

## 12. Main URLs

| URL | Description |
|---|---|
| `/` | Public landing page |
| `/accounts/register/`, `/accounts/login/` | Registration and login |
| `/dashboard/` | Executive dashboard |
| `/datasets/` | Dataset portfolio |
| `/datasets/upload/` | CSV / Excel / PDF upload |
| `/datasets/<id>/` | Dataset overview, schema, preview, insights |
| `/datasets/<id>/records/` | Paginated data browser with CRUD |
| `/analytics/` | Analytics studio |
| `/analytics/<id>/?tab=...` | Statistics, correlation, regression, categories, trend, forecast, anomalies |
| `/assistant/` | Grounded AI assistant |
| `/business/` | Business profiles and partners |
| `/reports/` | Report history and generation |
| `/panel/` | Custom admin panel |
| `/panel/audit/`, `/panel/security/`, `/panel/roles/` | Audit, security and role management |
| `/django-admin/` | Django’s built-in admin (superuser) |
| `/health/` | JSON health check (database status) |

---

## 13. Report formats

| Format | Contents |
|---|---|
| **PDF** | Cover, business overview, KPI table, data quality, statistical table, charts, forecast actual-vs-predicted, future projections, grounded insights, recommendations, partner performance |
| **PowerPoint** | 16:9 deck: cover, KPI cards, revenue bar chart + category pie/trend chart, forecast (when dated data exists), insights, recommendations |
| **Excel** | Multi-sheet workbook: Executive Summary, KPIs, Statistics, Categories, Forecast, Insights, Partners, Data appendix |
| **CSV** | Tidy metrics CSV with KPI, statistics, category, forecast and insight sections (full dataset export is available from the dataset page) |

Reports record the source dataset, row count, method notes, generation timestamp and forecast caveats.

---

## 14. Known limitations / honest notes

- **OCR is optional.** Text PDFs work out of the box; scanned PDFs require the Tesseract system binary. The app never claims OCR succeeded if it did not.
- **The assistant is deterministic, not a large language model.** It is intentionally constrained to computed facts. It will refuse general or unsupported questions.
- **Forecasting is planning-grade, not causal.** Backtest metrics and confidence labels are displayed. High-variance history can produce a low R²; this is reported rather than hidden.
- **Analysis sampling.** Very large datasets are analysed on a configurable sample (250k rows by default) for memory safety; the UI indicates sampling. This is not an upload record limit.
- **CSV reports are metrics reports.** The complete processed dataset is exported separately from the dataset page as CSV/XLSX.
- **The development server is for development.** For production use a WSGI server (Waitress/Gunicorn), HTTPS, `DEBUG=False`, strong secrets, backups and monitoring.
- **Native `mysqlclient` may be blocked by Application Control** on some managed Windows machines; PyMySQL is used instead.
- **No email delivery / scheduled reports.** These are future-scope items from the synopsis; the current application focuses on the working analytics and reporting pipeline.

---

## 15. Production checklist

1. Set `DJANGO_DEBUG=False` and a strong `DJANGO_SECRET_KEY`.
2. Set `DJANGO_ALLOWED_HOSTS` and `DJANGO_CSRF_TRUSTED_ORIGINS`.
3. Use HTTPS and set `DJANGO_SECURE_SSL=1`.
4. Use a dedicated MySQL user with only the required database privileges.
5. Configure `MEDIA_ROOT` on durable storage and back it up.
6. Run `python manage.py collectstatic` and serve static files through your web server.
7. Run the app with Waitress/Gunicorn behind IIS/Nginx; do not use `runserver`.
8. Review the audit log and security monitor regularly.
9. Change the seeded demo passwords or remove the demo accounts.
10. Define data retention, access review and incident-response procedures.

---

## 16. Troubleshooting

| Problem | Fix |
|---|---|
| PowerShell refuses `.ps1` | Use `.\scripts\setup.cmd` / `run.cmd`, or run with `powershell -ExecutionPolicy Bypass -File ...` |
| MySQL connection refused | Run `scripts\start_mysql.ps1`; check port 3307; inspect `%LOCALAPPDATA%\ai_vista_mysql\mysql.err.log` |
| `Access denied for root` in start script | Create the `ai_vista` database/user manually using a MySQL administrator and update `.env` |
| Scanned PDF import fails | Install Tesseract OCR and add it to `PATH`, or upload CSV/Excel |
| `sklearn` DLL blocked | Use the pinned scikit-learn 1.5.2 wheel; newer wheels may be blocked by Application Control |
| Matplotlib cache permission warning | `MPLCONFIGDIR` is set to `.mplconfig` by `config/settings.py`; ensure the folder is writable |
| Demo login fails | Run `.\.venv\Scripts\python.exe manage.py seed_demo`; passwords are reset to the documented demo values by the seeder |

---

## 17. Credits

Built as the AI VISTA mini-project implementation, following the product scope in the project synopsis while using the requested **Python/Django/MySQL** stack.

**Author:** Sneha Shrivastava 
**Project:** AI VISTA — Business Data Analytics & Automated Reporting System

