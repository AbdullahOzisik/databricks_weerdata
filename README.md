# 🌦️ Real-time Weerdata Analyse met Databricks 🚀

## 📌 Over dit project
Dit project haalt **real-time weerdata** op via de **OpenWeather API** en verwerkt deze in **Databricks Community Edition**. De data wordt opgeslagen in **Databricks File Store (DBFS)** en geanalyseerd met **PySpark**.

✅ **Data ophalen met OpenWeather API**  
✅ **Opslag in Databricks File Store (DBFS) in Parquet-formaat**  
✅ **Data blijft behouden, nieuwe metingen worden toegevoegd (append-modus)**  
✅ **Steden netjes onder elkaar gesorteerd per run**  
✅ **Geen verwijdering van oude gegevens**  

---

## 📊 Voorbeeld Data
| Stad      | Temperatuur (°C) | Vochtigheid (%) | Weerbeschrijving | Timestamp             |
|-----------|----------------|----------------|------------------|-----------------------|
| Amsterdam | 11.9           | 59             | Onbewolkt        | 2025-03-08 20:42:05   |
| Amsterdam | 12.3           | 60             | Licht bewolkt    | 2025-03-08 21:42:05   |
| Rotterdam | 12.03          | 54             | Onbewolkt        | 2025-03-08 20:40:10   |
| Rotterdam | 12.5           | 56             | Licht bewolkt    | 2025-03-08 21:40:10   |
| Utrecht   | 9.41           | 58             | Onbewolkt        | 2025-03-08 20:40:10   |

---

## 🛠️ Hoe werkt het?
1. **OpenWeather API** haalt actuele weergegevens per stad op.  
2. **PySpark** converteert de gegevens naar een **DataFrame**.  
3. **Data wordt opgeslagen in DBFS (Parquet-formaat)** voor analyses.  
4. **Elke nieuwe run voegt extra metingen toe zonder oude te verwijderen.**  
5. **Data wordt netjes gesorteerd per stad en timestamp.**  

---

## 🖥️ Installatie & Gebruik
Wil je dit project zelf draaien? Volg deze stappen:

### 🔹 **1. Open Databricks Community Edition**
Registreer gratis op **[Databricks Community](https://community.cloud.databricks.com/)**.

### 🔹 **2. Open een nieuw Notebook en plak de code**
- Kopieer de **Python-code** uit `weerdata_notebook.dbc`.
- Stel je eigen **OpenWeather API-sleutel** in.

### 🔹 **3. Run het script**
Druk op **Shift + Enter** in Databricks om het script uit te voeren.

---

## 📄 Volledige Code
```python
import requests
import pandas as pd
from datetime import datetime
import pytz
from pyspark.sql import SparkSession
from pyspark.sql.functions import col

# 🔹 API KEY (Registreer gratis op OpenWeather voor een API Key)
API_KEY = "JOUW_API_KEY_HIER"  # <-- Vervang dit met je eigen OpenWeather API Key

# 🔹 OpenWeather API Base URL
BASE_URL = "http://api.openweathermap.org/data/2.5/weather"

# 🔹 Haal één uniforme timestamp op (zodat alle steden dezelfde tijd krijgen)
amsterdam_tz = pytz.timezone("Europe/Amsterdam")
uniform_timestamp = datetime.now(amsterdam_tz).strftime("%Y-%m-%d %H:%M:%S")

# 🔹 Functie om weerdata op te halen
def get_weather(city):
    params = {
        "q": city,
        "appid": API_KEY,
        "units": "metric",
        "lang": "nl"
    }
    
    response = requests.get(BASE_URL, params=params)
    
    if response.status_code == 200:
        data = response.json()
        
        weer = {
            "stad": data["name"],
            "temperatuur": data["main"]["temp"],
            "vochtigheid": data["main"]["humidity"],
            "beschrijving": data["weather"][0]["description"],
            "timestamp": uniform_timestamp  # ✅ Uniforme tijd voor alle steden
        }
        return weer
    else:
        return None

# 🔹 Stedenlijst
steden = ["Lelystad", "Amsterdam", "Rotterdam", "Den Haag", "Utrecht", "Eindhoven"]

# 🔹 Haal weerdata op voor alle steden
weer_data = [get_weather(stad) for stad in steden if get_weather(stad) is not None]

# 🔹 Zet de data in een Pandas DataFrame
df = pd.DataFrame(weer_data)

# 🔹 Converteer naar een Spark DataFrame (voor Databricks)
spark = SparkSession.builder.getOrCreate()
spark_df = spark.createDataFrame(df)

# 🔹 Opslaglocatie in DBFS
storage_path = "dbfs:/mnt/weather_data/weerdata.parquet"

# 🔹 Sla nieuwe metingen op zonder te overschrijven (append)
spark_df.write.mode("append").parquet(storage_path)

# 🔹 Laad en sorteer de opgeslagen data op tijd en stad
df_loaded = spark.read.parquet(storage_path).orderBy(col("timestamp").desc(), col("stad").asc())

# 🔹 Toon de gesorteerde data in Databricks
display(df_loaded)

print("✅ Nieuwe weerdata succesvol toegevoegd en gesorteerd in DBFS!")

