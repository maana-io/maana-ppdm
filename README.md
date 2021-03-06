# maana-ppdm

Create a [PPDM](https://ppdm.org/ppdm/PPDM/Standards/PPDM_Data_Model/PPDM_3_9_Data_Model/PPDM/PPDM_3.9_Data_Model.aspx?hkey=fed7573b-c57d-4909-b15a-a61880fb8d2b) GraphQL endpoint for us in Oil and Gas domain.

- Create a PostgreSQL database from: [https://github.com/rbhughes/pg_ppdm](https://github.com/rbhughes/pg_ppdm)
- Use [Prisma](https://www.prisma.io/) to generate a GraphQL endpoint

## Setup

### Azure Database for PostgreSQL

- [Quick Start](https://docs.microsoft.com/en-us/azure/postgresql/quickstart-create-server-database-portal)
- Database name: ppdm-postgres
- Ensure to whitelist any IP from which you are going to connect in the server firewall

### Install PostgreSQL (local)

During development, it is useful to have a local copy of the database

If using Azure, then you'll also need the tools (e.g., psql) to work with cloud database.

#### Ubuntu

Follow the instructions for the appropriate version:
- [Ubuntu 19.04](https://www.osradar.com/how-to-install-postgresql-on-ubuntu-19-04/)

## Connecting to Azure Database for PostgreSQL

```bash
psql postgresql://maana%40ppdm-postgres:[SECRET]@ppdm-postgres.postgres.database.azure.com:5432/postgres?sslmode=prefer
```

## Loading PostgreSQL with PPDM Data

Ensure you are in the PPDM data directory, `./ppdm_postgres_39/`.

Once logged into the PostgreSQL database, use the following commands to prepare to load the PPDM data:

```sql
CREATE DATABASE ppdm39 WITH OWNER maana;

\connect ppdm39 maana;

CREATE SCHEMA IF NOT EXISTS ppdm AUTHORIZATION maana;

SET search_path to ppdm;

\i PPDM39.sql
```

If all is well, then this last command will take awhile (~20m) and should produce the following output:
```bash
postgres=> \i PPDM39.sql
Creating Tables and Columns...
Creating Primary Keys...
Creating Check Constraints...
Creating Foreign Keys...
Creating Original Units of Measure (OUOM) Foreign Keys...
Creating Units of Measure (UOM) Foreign Keys...
Creating ROW_QUALITY Foreign Keys...
Creating SOURCE Foreign Keys...
Creating Table Comments...
Creating Column Comments...
Creating Unique constraints and Not Null constraints on PPDM_GUID...
```

## Create the Prisma server

### Install
```bash
npm install -g prisma2
```

### Introspect Schema
First time:
```bash
prisma2 init
```

Subsequent times:
```bash
prisma2 introspect
```

### Generate the Client (Photon)

First, we need to edit the schema file (`prisma/schema.prisma`) to include the generator directive:

```
+ generator photonjs {
+   provider = "photonjs"
+ }
```

Next, generate the Photon client:

```bash
prisma2 generate
```
