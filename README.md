📖 Inhoud
📌 Over het project
⚙️ Technologieën
🚀 Installatie & Setup
🔹 Databricks: Weerdata ophalen & opslaan
💾 Azure Blob Storage: Data opslaan
📊 Power BI: Visualisaties
🔄 Automatisering & Scheduling
📈 Uitbreidingen
📎 Contact
📌 Over het project
Deze pipeline verzamelt weerdata van meerdere Nederlandse steden, slaat het op in Azure en maakt visualisaties in Power BI.
✅ Real-time data ophalen (elk uur)
✅ Data opslaan in Azure Blob Storage
✅ Automatische updates in Power BI

📊 Visualisaties in Power BI:
📌 Temperatuur per stad (lijn- en staafdiagrammen)
📌 Weerdata op een kaart
📌 Vergelijking van luchtvochtigheid

⚙️ Technologieën
Technologie	Gebruik
Databricks	Python-script voor het ophalen en verwerken van weerdata
Azure Blob Storage	Opslag van weerdata in CSV-formaat
Power BI	Visualisatie en analyse van de data
🚀 Installatie & Setup
1️⃣ Vereisten
Azure Blob Storage (Storage Account + Container)
Databricks Community Edition (gratis versie)
Power BI Desktop
OpenWeather API Key (registreer op OpenWeather)
2️⃣ Setup in Azure Blob Storage
1️⃣ Maak een Storage Account (weerdata) in Azure
2️⃣ Maak een Container (weerdata) aan
3️⃣ Genereer een SAS-token met de rechten racwdli
4️⃣ Noteer de SAS-URL om in Databricks te gebruiken

🔹 Databricks: Weerdata ophalen & opslaan
Het volgende Python-script haalt weerdata op en slaat deze op in Azure Blob Storage.

python
Kopiëren
import requests
import pandas as pd
from datetime import datetime
import pytz
from pyspark.sql import SparkSession

# 🔹 API KEY (registreer op OpenWeather)
API_KEY = "JOUW_OPENWEATHER_API_KEY"

# 🔹 OpenWeather API Base URL
BASE_URL = "http://api.openweathermap.org/data/2.5/weather"

# 🔹 Stedenlijst
steden = ["Lelystad", "Amsterdam", "Rotterdam", "Den Haag", "Utrecht", "Eindhoven"]

# 🔹 Haal weerdata op
def get_weather(city):
    params = {"q": city, "appid": API_KEY, "units": "metric"}
    response = requests.get(BASE_URL, params=params)
    if response.status_code == 200:
        data = response.json()
        return {
            "stad": data["name"],
            "temperatuur": data["main"]["temp"],
            "vochtigheid": data["main"]["humidity"],
            "timestamp": datetime.now(pytz.timezone("Europe/Amsterdam")).strftime("%Y-%m-%d %H:%M:%S")
        }
    return None

weer_data = [get_weather(stad) for stad in steden if get_weather(stad) is not None]
df = pd.DataFrame(weer_data)

# 🔹 Spark DataFrame maken
spark = SparkSession.builder.getOrCreate()
spark_df = spark.createDataFrame(df)

# 🔹 Opslaglocatie in Azure Blob Storage
storage_account = "weerdata"
container = "weerdata"
sas_token = "JOUW_SAS_TOKEN"

blob_url = f"wasbs://{container}@{storage_account}.blob.core.windows.net"
spark.conf.set(f"fs.azure.sas.{container}.{storage_account}.blob.core.windows.net", sas_token)

# 🔹 Opslaan als CSV
storage_path = f"{blob_url}/weerdata.csv"
spark_df.write.mode("overwrite").option("header", "true").csv(storage_path)

print(f"✅ Data opgeslagen in Azure: {storage_path}")
✅ Nu wordt de weerdata automatisch opgeslagen in Azure!

💾 Azure Blob Storage: Data opslaan
1️⃣ Open Azure Portal
2️⃣ Ga naar de Container "weerdata"
3️⃣ Controleer of weerdata.csv aanwezig is
4️⃣ Test of je het bestand kunt openen

📊 Power BI: Visualisaties
1️⃣ Open Power BI
2️⃣ Ga naar "Gegevens ophalen" → "Web"
3️⃣ Plak de URL van je bestand met SAS-token:

arduino
Kopiëren
https://weerdata.blob.core.windows.net/weerdata/weerdata.csv?sp=racwdli&sv=2022-11-02&sig=JOUW_SIG
4️⃣ Klik op "OK" → Selecteer "Decimaal Getal" voor temperatuur
5️⃣ Maak visualisaties:

Lijndiagram: Temperatuurtrends per stad 📈
Kaart: Weerdata per locatie 🌍
Staafdiagram: Luchtvochtigheid per stad 📊
✅ Nu worden alle weergegevens gevisualiseerd in Power BI!

🔄 Automatisering & Scheduling
📌 Laat Power BI automatisch updaten: 1️⃣ Publiceer naar Power BI Service
2️⃣ Ga naar "Schedule Refresh" → Zet een automatische update in (bijv. elk uur)
3️⃣ Nu haalt Power BI altijd de nieuwste data uit Azure!

✅ Je hebt nu een volledig geautomatiseerde pipeline! 🚀

📈 Uitbreidingen
🔹 Voorspellingen toevoegen met Machine Learning
🔹 Gebruik Google Cloud i.p.v. Azure
🔹 Meer steden en weersvoorspellingen toevoegen
🔹 Power BI Dashboard embedden in een website

📎 Contact
📌 Wil je dit project zien of samenwerken?
📧 Mail me op a.ozisik@hotmail.com
🔗 [LinkedIn-profiel hier invoegen]
