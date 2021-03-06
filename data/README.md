#CartoDB data tables used in this application

**This file contains the queries used to generate each of the derived tables used by the application.** We call these tables "materialized", even though they are not technically [Materialized Views](http://www.postgresql.org/docs/9.3/static/sql-creatematerializedview.html) in the PostgreSQL sense. They are simply copies.

**Why do we do this?**

1. Using these copied tables is more efficient, because the query doesn't have to be run every time a new user loads the application.
2. They allow us to make the derived table public (so the application does not require API keys) while keeping the source data private.
3. They make the application more resilient in case the source data tables are undergoing modification or development. Using materialized tables means that the application will always be using a version of the data that is known to work.

**How to use these queries:**

1. Create a new empty table in the CartoDB web interface. This table will be used only temporarily, from which to create our materialized table.
2. Paste the SQL into the CartoDB "Custom SQL query" panel. Click "Apply query".
3. Select "Dataset from query" in the "Edit" menu.
4. Click on the name of the new table to change the name from `untitled_table_NN_copy` to `site_tablename_materialized`.
5. Select "Change privacy" in the "Edit" menu, so that the table is accessible to anyone "With link".
6. (optional) You can now delete the empty table you created in step 1.

The following sections list the names of each of the tables used by the application.
The "**Tables**" section is a list of the source tables used by the query.
The "**SQL**" section documents the query used to generate the derived table.


####site_commodities_materialized
**Tables:**
`commodities`

**SQL**
```sql
SELECT * FROM commodities
```

####site_commodities_lookup_materialized
**Tables:**
`commodities_lookup`

**SQL**
```sql
SELECT * FROM commodities_lookup
```

####site_category_lookup_materialized
**Tables:**
`category_lookup`

**SQL**
```sql
SELECT * FROM category_lookup
```

####site_canal_list_materialized
**Tables:**
`canal_list`

**SQL**
```sql
SELECT * FROM canal_list
```

####site_canals_materialized
**Tables:**
`canals`

**SQL**
```sql
SELECT canal_id, name, opened, closed, length, ST_Transform(ST_SetSRID(ST_Transform(the_geom,2163),3857),4326) as the_geom FROM canals
```

####site_total_tonnage_materialized
**Tables:**
`total_tonnage`

**SQL**
```sql
SELECT * FROM total_tonnage
```
