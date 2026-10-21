# Inventory & Stock Management System

A Streamlit-based inventory and supply-chain dashboard backed by MySQL. The application provides summary metrics, supplier and stock views, product history, product creation, reorder placement, and reorder receiving workflows.

## Features

### Dashboard

The **Basic Information** page displays:

- Total suppliers
- Total products
- Number of product categories
- Sales value for the last three months
- Restock value for the last three months
- Products below their reorder level without a pending reorder

It also displays:

- Supplier contact details
- Products with their suppliers and current stock
- Products that need reordering

### Operational tasks

The **Operational Tasks** page supports:

- Adding a new product
- Viewing a product's inventory history
- Placing a reorder for a product
- Marking an existing reorder as received

## Technology stack

- **Python** - application language
- **Streamlit** - web dashboard UI
- **Pandas** - tabular data display
- **MySQL** - inventory database
- **mysql-connector-python** - Python-to-MySQL connection
- **Jupyter Notebook** - exploratory database analysis and development notes

## Project structure

```text
.
├── app.py
├── db_functions.py
├── Python UI for SQL Databases.sql
├── Untitled (1).ipynb
└── README.md
```

| File | Description |
| --- | --- |
| `app.py` | Streamlit application entry point and dashboard UI |
| `db_functions.py` | MySQL connection helper and database operations used by the app |
| `Python UI for SQL Databases.sql` | SQL queries, a product-insert stored procedure, an inventory-history view, and a reorder-receiving stored procedure |
| `Untitled (1).ipynb` | Notebook used for exploratory queries and testing the database functions |

## Prerequisites

Install the following before running the project:

1. Python 3
2. A running MySQL Server instance
3. A MySQL database named `dummy_project`
4. The application's required tables and routines:
   - `products`
   - `suppliers`
   - `stock_entries`
   - `shipments`
   - `reorders`
   - `product_inventory_history`
   - `AddNewProductManualID`
   - `MarkReorderAsReceived`

The current source connects to MySQL using:

```text
host: localhost
user: root
database: dummy_project
```

Before running the application, review the connection settings in `db_functions.py` and configure them for your local MySQL installation. Do not commit real database passwords or other credentials to a public GitHub repository. Prefer environment variables or another local secret-management approach when adapting the project for production.

## Installation

Open a terminal in the project directory and create a virtual environment:

### Windows PowerShell

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install streamlit pandas mysql-connector-python
```

### macOS/Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install streamlit pandas mysql-connector-python
```

The project does not currently include a `requirements.txt` file, so the packages above are the dependencies imported by the application. The notebook also imports `numpy`; install it, along with Jupyter, only if you want to run the notebook:

```bash
python -m pip install numpy jupyter
```

## Database setup

The application expects the `dummy_project` database and its inventory tables to already be available. The supplied SQL file contains application queries and database objects, but it is not a complete schema-and-seed script.

Review `Python UI for SQL Databases.sql` and adapt it to your database before executing it. It includes:

- Queries for inventory and supplier metrics
- Queries for supplier, product, and reorder information
- `AddNewProductManualID`, which inserts a product and corresponding shipment and stock-entry records
- `product_inventory_history`, which combines shipment and stock-entry history
- `MarkReorderAsReceived`, which updates a reorder and records the received stock
- Sample calls and exploratory queries

### Important SQL safety note

The SQL file contains exploratory statements, sample data-changing calls, and a `DROP TABLE products` statement. Do not execute the entire file blindly against a database containing important data. Extract and review only the schema, view, procedure, or query sections you need, and take a backup before making database changes.

After the database is prepared, verify that the configured MySQL user can:

- Read from the inventory tables
- Execute `AddNewProductManualID`
- Insert reorder records
- Execute `MarkReorderAsReceived`

## Run the dashboard

With the virtual environment activated and MySQL running:

```bash
streamlit run app.py
```

Streamlit will print a local URL, normally:

```text
http://localhost:8501
```

Open that URL in a browser. Use the sidebar to switch between **Basic Information** and **Operational Tasks**.

## Run the notebook

The notebook is optional and is intended for exploratory database work:

```bash
jupyter notebook "Untitled (1).ipynb"
```

It demonstrates connecting to MySQL, running summary queries, and testing helper functions. Make sure the notebook uses the same database credentials and that the database is available before running its cells.

## Typical usage

1. Start MySQL and make sure the `dummy_project` database is available.
2. Configure the connection values in `db_functions.py`.
3. Activate the Python virtual environment.
4. Start the Streamlit application with `streamlit run app.py`.
5. Review inventory metrics on **Basic Information**.
6. Use **Operational Tasks** to add products, inspect product history, place reorders, and receive pending reorders.

## Troubleshooting

### `ModuleNotFoundError`

Activate the virtual environment and install the application dependencies:

```bash
python -m pip install streamlit pandas mysql-connector-python
```

### MySQL connection errors

Check that:

- MySQL Server is running.
- The host, username, password, and database name configured in `db_functions.py` are correct.
- The `dummy_project` database exists.
- The MySQL user is allowed to connect from `localhost`.

### Missing table, view, or procedure errors

The application depends on the database objects listed in [Database setup](#database-setup). Create or restore those objects before starting the dashboard.

### Empty dropdowns or missing data

The dashboard reads categories, suppliers, products, and reorder records from MySQL. Confirm that the relevant tables contain data and that the configured user has `SELECT` permission.

## Security and deployment notes

- Do not commit passwords, API keys, or other secrets.
- Use a database user with only the permissions required by the application instead of using a root account.
- Review every SQL statement before running it against a shared or production database.
- Add a dependency lock or `requirements.txt` file before deploying to another environment so package versions are reproducible.

## Current limitations

- Database credentials are currently configured in Python source rather than through a dedicated configuration file or environment-variable interface.
- The repository does not include a complete database schema, migrations, or seed-data script.
- The dashboard expects MySQL routines and tables to be created before the application starts.
- There is no automated test suite or deployment configuration included.

