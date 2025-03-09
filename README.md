# 🌦️ Weerdata Pipeline met Databricks, Azure Blob Storage en Power BI

## 📌 Over het project
Deze pipeline verzamelt **real-time weerdata** uit de **OpenWeather API**, verwerkt het in **Databricks (PySpark)** en **slaat het automatisch op in Azure Blob Storage**.

✅ **Real-time data ophalen (elk uur met API)**  
✅ **PySpark voor data-engineering in Databricks**  
✅ **Automatische opslag in Azure Blob Storage als CSV**  
✅ **(Optioneel) Integratie met Power BI voor visualisatie**  

---

## ⚙️ Technologieën
| Technologie        | Gebruik |
|--------------------|---------|
| **OpenWeather API** | Ophalen van real-time weerdata |
| **Databricks (PySpark)** | Verwerken en opschonen van de data |
| **Azure Blob Storage** | Opslag van de weerdata als CSV |
| **Power BI (optioneel)** | Analyse en visualisatie van de gegevens |

---

# 🚀 1. OpenWeather API Instellen
Om de weerdata op te halen, gebruiken we de **OpenWeather API**.

### ✅ API Key verkrijgen
1. Registreer op **[OpenWeather API](https://openweathermap.org/api)**
2. Ga naar **API Keys** en kopieer jouw **API Key**
3. Test de API met een voorbeeld-URL:  
   ```bash
   curl "http://api.openweathermap.org/data/2.5/weather?q=Amsterdam&appid=JOUW_API_KEY&units=metric"
