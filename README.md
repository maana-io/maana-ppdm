# maana-ppdm

Create a [PPDM](https://ppdm.org/ppdm/PPDM/Standards/PPDM_Data_Model/PPDM_3_9_Data_Model/PPDM/PPDM_3.9_Data_Model.aspx?hkey=fed7573b-c57d-4909-b15a-a61880fb8d2b) GraphQL endpoint for us in Oil and Gas domain.

- Create a PostgreSQL database from: [https://github.com/rbhughes/pg_ppdm](https://github.com/rbhughes/pg_ppdm)
- Use [PostGraphile](https://www.graphile.org/) to generate a GraphQL endpoint

## Setup

### Azure Database for PostgreSQL

- [Quick Start](https://docs.microsoft.com/en-us/azure/postgresql/quickstart-create-server-database-portal)
- Database name: ppdm-postgres
- Ensure to whitelist any IP from which you are going to connect in the server firewall

### Install PostgreSQL (local)

During development, it is useful to have a local copy of the database

If using Azure, then you'll also need the tools (e.g., psql) to work with cloud database.

#### Ubuntu

Follow t6he instructions for the appropriate version:
- [Ubuntu 19.04](https://www.osradar.com/how-to-install-postgresql-on-ubuntu-19-04/)

## Connecting to Azure Database for PostgreSQL

```bash
psql "sslmode=require host=ppdm-postgres.postgres.database.azure.com user=maana@ppdm-postgres dbname=postgres"
```
