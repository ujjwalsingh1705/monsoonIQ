# monsoonIQ
Mumbai Waterlogging Intelligence System
# MONSOONIQ — Mumbai Waterlogging Intelligence System

Streamlit + MySQL + scikit-learn app that estimates waterlogging risk in Mumbai from **verified, user-supplied data**.
No synthetic data is included or used for training.

## Run locally
```bash
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env    # fill in MySQL + API keys
```
Create the MySQL database and user:
```sql
CREATE DATABASE monsooniq CHARACTER SET utf8mb4;
CREATE USER 'monsooniq'@'localhost' IDENTIFIED BY 'change_me';
GRANT ALL ON monsooniq.* TO 'monsooniq'@'localhost';
```
Tables are created automatically on first launch.
```bash
streamlit run app.py
```

## Workflow
1. **Data Management** → download the template, upload your verified CSV, validate, import into MySQL and/or save as a training file.
2. **Model Training** → pick dataset, target, features, split → train → review metrics → **Save model** (`models/model.joblib`, `models/preprocessing.joblib`).
3. **Risk Analysis** → select/search a location → fetch live weather → run estimate (saved to `predictions`).
4. **Risk Map / Dashboard / Analytics** visualise everything from MySQL.

The model is loaded from disk and cached; it is never retrained on page load.

## APIs
- Weather: OpenWeatherMap current weather (`OPENWEATHER_API_KEY`).
- Geocoding: Google Geocoding if `GOOGLE_MAPS_API_KEY` is set, otherwise OpenStreetMap Nominatim (keyless, respect its usage policy).
- Map tiles: CARTO dark basemap via Plotly (no key).

Risk outputs are statistical estimates, not flood forecasts.
