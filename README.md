# Weather Data ETL Pipeline with Apache Airflow

## Overview
This project implements an automated ETL (Extract, Transform, Load) pipeline using Apache Airflow to collect weather data from Open-Meteo API for London and store it in PostgreSQL. The pipeline runs daily and demonstrates a practical implementation of data engineering concepts.

## Pipeline Architecture

```mermaid
graph LR
    A[Open-Meteo API] -->|Extract| B[HTTP Hook]
    B -->|Transform| C[Process Weather Data]
    C -->|Load| D[PostgreSQL]
```

## Features
- **Extract**: Fetches current weather data using Airflow's HttpHook
- **Transform**: Processes raw JSON into structured weather metrics
- **Load**: Persists data to PostgreSQL with timestamp tracking
- **Scheduling**: Runs daily via Airflow scheduler
- **Error Handling**: Built-in error handling for API and database operations

## Technical Stack
- Apache Airflow
- PostgreSQL
- Open-Meteo API
- Python Libraries (see requirements.txt)

## Project Structure
```
ETL weather data/
├── learning-airflow/
│   ├── dags/
│   │   └── etlweather.py      # Main DAG file
│   ├── img/                   # Screenshot directory
│   └── requirements.txt       # Project dependencies
└── README.md                 # This file
```

## DAG Details

### Weather Data Collection
- **Location**: London, UK
- **Coordinates**: 
  - Latitude: 51.5074°N
  - Longitude: 0.1278°W
- **Metrics Collected**:
  - Temperature
  - Wind Speed
  - Wind Direction
  - Weather Code

### Database Schema
```sql
CREATE TABLE weather_data (
    latitude FLOAT,
    longitude FLOAT,
    temperature FLOAT,
    windspeed FLOAT,
    winddirection FLOAT,
    weathercode INT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Setup Instructions

### 1. Prerequisites
- Python 3.x
- PostgreSQL
- Astro CLI (for Astronomer Runtime)

### 2. Dependencies Installation
```bash
cd "ETL weather data/learning-airflow"
pip install -r requirements.txt
```

### 3. Airflow Connections Setup
Configure two required connections in Airflow:

1. **Open-Meteo API Connection**
   ```yaml
   Connection Id: open_meteo_api
   Connection Type: HTTP
   Host: https://api.open-meteo.com
   ```

2. **PostgreSQL Connection**
   ```yaml
   Connection Id: postgres_default
   Connection Type: Postgres
   Host: your_host
   Schema: your_database
   Login: your_username
   Password: your_password
   Port: 5432
   ```

### 4. Running the Pipeline
Using Astro CLI:
```bash
astro dev start
```

## Monitoring and Verification

### DAG Status

![DAG Run Status](img/run_success.png)
<!-- imgs\run_success.png -->
- Screenshot shows successful DAG execution


### Task Logs
You can monitor task execution through:
1. Airflow UI - Navigate to DAG view
2. CLI - Using `astro dev logs` command

## Troubleshooting

Common issues and solutions:

1. **API Connection Failed**
   - Verify internet connection
   - Check Open-Meteo API status
   - Confirm HTTP connection configuration

2. **Database Issues**
   - Verify PostgreSQL is running
   - Check connection credentials
   - Ensure database user has required permissions

## Dependencies
```text
duckdb==0.10.3
tabulate==0.9.0
pyarrow
apache-airflow-providers-http
apache-airflow-providers-postgres
requests
pandas
psycopg2-binary
pendulum
```

## Development

### Local Development
1. Clone the repository
2. Create a virtual environment
3. Install dependencies
4. Set up Airflow connections
5. Run the DAG

### Testing
Test individual tasks using Airflow CLI:
```bash
airflow tasks test weather_etl_pipeline extract_weather_data 2025-01-01
airflow tasks test weather_etl_pipeline transform_weather_data 2025-01-01
airflow tasks test weather_etl_pipeline load_weather_data 2025-01-01
```

## Future Enhancements
1. Add more weather metrics
2. Implement data validation
3. Add multiple cities support
4. Create data visualization dashboard
5. Add email notifications for task failures

## Contributing
1. Fork the repository
2. Create a feature branch
3. Commit changes
4. Push to the branch
5. Create a Pull Request
