# Engineering_cours_code
DE Zoomcamp 2023 projects and homeworks. Week1.
Automation using Docker containers (file uploads, database conversions, using pgadmin for SQL queries)

**Technologies:**
-- Python 3.9
-- Docker 20.10.20
-- Docker-compose 2.12.0
-- Pandas 1.3.5
-- Psycopg2 2.9.5
-- PGAdmin


Automation using containers: file uploads, database conversions, using pgadmin for sql queries
Downloading ...:
URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-01.csv.gz"
URL="https://s3.amazonaws.com/nyc-tlc/misc/taxi+_zone_lookup.csv" (to get the database, you need to remove the format conversion to injest_data.py)

**Docker Practice:**
```
docker network create pg-network

docker run -it \
-e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
-e PGADMIN_DEFAULT_PASSWORD="root" \
-p 8080:80 \
--network=pg-network \
--name pgadmin-2 \
dpage/pgadmin4

docker run -it \
-e POSTGRES_USER="root" \
-e POSTGRES_PASSWORD="root" \
-e POSTGRES_DB="ny_taxi" \
-v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
-p 5432:5432 \
--network=pg-network \
--name pg-database \
postgres:13

docker build -t taxi_ingest:v001 .

URL = <...>

docker run -it \
     --network=pg-network \
  taxi_ingest:v001 \
     --user=root \
     --password=root \
     --host=pg-database \
     --port=5432 \
     --db=ny_taxi \
     --table_name=yellow_taxi_trips \
     --url=${URL}
```

**Docker-compose Practice:**

```
docker-compose up 

URL = <...>

python3 ingest_data.py \
     --user=root \
     --password=root \
     --host=localhost \
     --port=5432 \
     --db=ny_taxi \
     --table_name=yellow_taxi_trips \
     --url=${URL}
```