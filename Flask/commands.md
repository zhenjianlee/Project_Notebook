# Flask Commands and ETC

## Operations

1. Running dev server ```flask run --reload --debugger```


## DB Operations Flask-Migrate (Library integration of Alembic with Flask-SQLAlchemy)

1. Initialize the database for the first time ``` db init ```

2. Make migration files based on changes detected in models ``` db migrate ```

3. Execute migration files to db  (upgrade the schema) ``` db upgrade ``` 