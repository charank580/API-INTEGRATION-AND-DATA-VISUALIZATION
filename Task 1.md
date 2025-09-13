\# weather\_dashboard.py



import requests

import pandas as pd

import matplotlib.pyplot as plt

import seaborn as sns

import streamlit as st



\# -------------------------

\# 🔑 ENTER YOUR API KEY HERE

\# -------------------------

API\_KEY = "YOUR\_API\_KEY"

CITY = "Hyderabad"  # change this to any city you want

URL = f"http://api.openweathermap.org/data/2.5/forecast?q={CITY}\&appid={API\_KEY}\&units=metric"



\# Fetch Data

response = requests.get(URL)

data = response.json()



\# Convert to DataFrame

forecast\_list = data\["list"]

weather\_data = \[]



for item in forecast\_list:

&nbsp;   weather\_data.append({

&nbsp;       "datetime": item\["dt\_txt"],

&nbsp;       "temperature": item\["main"]\["temp"],

&nbsp;       "humidity": item\["main"]\["humidity"],

&nbsp;       "feels\_like": item\["main"]\["feels\_like"],

&nbsp;       "weather": item\["weather"]\[0]\["description"]

&nbsp;   })



df = pd.DataFrame(weather\_data)



\# Streamlit Dashboard

st.title(f"🌦 Weather Dashboard for {CITY}")

st.write("Data from OpenWeatherMap (5-day forecast, 3-hour intervals)")



st.dataframe(df.head())



\# Line Plot - Temperature

st.subheader("🌡 Temperature Trend")

plt.figure(figsize=(10, 4))

sns.lineplot(data=df, x="datetime", y="temperature", marker="o", color="red")

plt.xticks(rotation=45)

plt.tight\_layout()

st.pyplot(plt)



\# Line Plot - Humidity

st.subheader("💧 Humidity Trend")

plt.figure(figsize=(10, 4))

sns.lineplot(data=df, x="datetime", y="humidity", marker="o", color="blue")

plt.xticks(rotation=45)

plt.tight\_layout()

st.pyplot(plt)



\# Bar chart: Average per day

st.subheader("📊 Daily Average Temperature")

df\["date"] = pd.to\_datetime(df\["datetime"]).dt.date

daily\_avg = df.groupby("date")\["temperature"].mean().reset\_index()



plt.figure(figsize=(8, 4))

sns.barplot(x="date", y="temperature", data=daily\_avg, palette="coolwarm")

plt.xticks(rotation=45)

plt.tight\_layout()

st.pyplot(plt)



st.success("✅ Dashboard Ready!")

