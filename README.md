# Weather-Forecast-and-Historical-Trends-for-Indian-Capitals
Turn 45 years of Indian weather (historic) +  14 days live forecast into an interactive Power BI dashboard, powered by Python + Open-Meteo.

✨ What’s inside

✅ Python pipeline to download historical (1980–present) and 14 day forecast for all Indian state capitals

✅ Robust retry, caching, batching, and resume logic (no wasted API calls)

✅ Clean, analysis-ready CSVs (daily granularity)

✅ Power BI model with state/city slicers, trend lines, maps, heatmaps

✅ Step-by-step setup + troubleshooting

📁 Project structure

├─ data/

│  ├─ capital_weather_historic.csv        # final historic data (daily)

│  ├─ capital_weather_forecast.csv        # final forecast data (daily)

│  ├─ indian_capital_geodata.csv          # State, Capital, Latitude, Longitude, Timezone

│  ├─ failed_cities.csv                   # auto-generated (for retries)

│  └─ cache/                              # HTTP cache (requests-cache)

├─ notebooks/

│  ├─ 01_collect_historic.ipynb

│  ├─ 02_collect_forecast.ipynb

│  └─ 03_qc_and_exports.ipynb

├─ scripts/

│  ├─ collect_historic.py

│  ├─ collect_forecast.py

│  ├─ retry_failed_cities.py

│  └─ make_failed_cities.py

├─ powerbi/

│  ├─ India's_Weather_Report_Analysis.pbix 

├─ README.md
└─

🧰 Tech Used

Python: pandas, requests, requests-cache, retry-requests

API: Open-Meteo (no API key)

BI: Power BI Desktop

🔗 Data sources

Open-Meteo Archive API – daily historic weather (1980→present)

Open-Meteo Forecast API – daily forecast (7–16 days)

Indian capitals geodata – State, Capital, Latitude, Longitude, Timezone (CSV included)

Units: rain_sum = millimeters (mm); temps in °C.

⚙️ Setup

Install Library

<img width="664" height="233" alt="Screenshot 2025-08-16 105820" src="https://github.com/user-attachments/assets/bd14efed-cd79-4da6-9ee0-0cf64f48d79d" />

📊 Power BI setup

Load data


Import capital_weather_historic.csv & capital_weather_forecast.csv

Import indian_capital_geodata.csv as a dimension

Create lookup tables (star schema)

State_Lookup = distinct of State

City_Lookup = distinct of (State, Capital)

Relationships

indian_capital_geodata[State] → capital_weather_historic[State] (1:*), single direction

indian_capital_geodata[State] → capital_weather_forecast[State] (1:*), single direction

<img width="1368" height="685" alt="Screenshot 2025-08-16 110832" src="https://github.com/user-attachments/assets/86cbded1-6533-460c-88f8-a65efe29aa09" />

Visuals

<img width="1436" height="737" alt="Screenshot 2025-08-16 111421" src="https://github.com/user-attachments/assets/23059ed5-7a94-439f-9b31-97b02b4867b2" />

<img width="1445" height="740" alt="Screenshot 2025-08-16 111443" src="https://github.com/user-attachments/assets/f86d3dae-8428-4588-85e9-b87d9d45e243" />

<img width="1528" height="735" alt="Screenshot 2025-08-16 111507" src="https://github.com/user-attachments/assets/a859e781-9c41-4244-b159-f6f6efe05d6d" />

🧪 Quality checks


Duplicate guards: (State, Capital, Date) composite key

Unit sanity: Rain_Sum in mm; convert to cm if needed (/10)

Missing days: check for gaps per city/date and log

🛟 Troubleshooting

Rate limit: Script prints “Rate limit hit… waiting 60s” and retries. Use retry_failed_cities.py between batches.

Permission denied on CSV: Close Excel/Power BI (file lock) or write to a temp file, then replace.

Many-to-many warning: Use State_Lookup / City_Lookup and 1:* relationships.

Month order wrong: Ensure MonthName is sorted by MonthNumber.

Map blank: Set State column Data Category = State or Province; use Color saturation for values.
