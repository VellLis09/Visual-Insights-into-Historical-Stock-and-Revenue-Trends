# Analyzing Historical Stock/Revenue Data and Building a Dashboard
Final Project

# Extracting Tesla Stock Data Using yfinance
Reset the index, save, and display the first five rows of the tesla_data dataframe using the head function. 

``import yfinance as yf
import pandas as pd
import requests
from bs4 import BeautifulSoup
import plotly.graph_objs as go

tesla = yf.Ticker("TSLA")
tesla_history = tesla.history(period="max")
tesla_data.reset_index(inplace=True)
tesla_data.head()``


# Extracting Tesla Revenue Data Using Webscraping
Display the last five rows of the tesla_revenue dataframe using the tail function. Upload a screenshot of the results.

url_tesla = "https://www.macrotrends.net/stocks/charts/TSLA/tesla/revenue"
html_tesla = requests.get(url_tesla).text
soup_tesla = BeautifulSoup(html_tesla, "html.parser")
tables_tesla = soup_tesla.find_all("table")

for table in tables_tesla:
    if "Tesla Quarterly Revenue" in table.text:
        df_tesla_revenue = pd.read_html(str(table))[0]
        break

df_tesla_revenue.columns = ["Date", "Revenue"]
df_tesla_revenue["Revenue"] = df_tesla_revenue["Revenue"].str.replace("$", "").str.replace(",", "").astype(float)
df_tesla_revenue = df_tesla_revenue[df_tesla_revenue["Revenue"].notna()]

# Extracting GameStop Stock Data Using yfinance
Reset the index, save, and display the first five rows of the gme_data dataframe using the head function. 

``gme = yf.Ticker("GME")
gme_history = gme.history(period="max")
gme_data = gme_history.reset_index()``

# Extracting GameStop Revenue Data Using Webscraping
Display the last five rows of the gme_revenue dataframe using the tail function.

``url_gme = "https://www.macrotrends.net/stocks/charts/GME/gamestop/revenue"
html_gme = requests.get(url_gme).text
soup_gme = BeautifulSoup(html_gme, "html.parser")
tables_gme = soup_gme.find_all("table")

for table in tables_gme:
    if "GameStop Quarterly Revenue" in table.text:
        df_gme_revenue = pd.read_html(str(table))[0]
        break

df_gme_revenue.columns = ["Date", "Revenue"]
df_gme_revenue["Revenue"] = df_gme_revenue["Revenue"].str.replace("$", "").str.replace(",", "").astype(float)
df_gme_revenue = df_gme_revenue[df_gme_revenue["Revenue"].notna()]``

# Tesla Stock and Revenue Dashboard
Use the make_graph function to graph the Tesla Stock Data, also provide a title for the graph.

``fig_tesla = go.Figure()

fig_tesla.add_trace(go.Scatter(x=tesla_data["Date"], y=tesla_data["Close"],
                               name="Tesla Stock Price"))
fig_tesla.add_trace(go.Scatter(x=pd.to_datetime(df_tesla_revenue["Date"]), y=df_tesla_revenue["Revenue"],
                               name="Tesla Revenue", yaxis="y2"))

fig_tesla.update_layout(
    title="Tesla Stock Price vs Revenue",
    xaxis_title="Date",
    yaxis=dict(title="Stock Price (USD)"),
    yaxis2=dict(title="Revenue (USD Millions)", overlaying='y', side='right'),
    legend=dict(x=0.01, y=0.99),
    width=1000,
    height=500
)
fig_tesla.show()``

# GameStop Stock and Revenue Dashboard
Use the make_graph function to graph the GameStop Stock Data, also provide a title for the graph.

``fig_gme = go.Figure()

fig_gme.add_trace(go.Scatter(x=gme_data["Date"], y=gme_data["Close"],
                             name="GameStop Stock Price"))
fig_gme.add_trace(go.Scatter(x=pd.to_datetime(df_gme_revenue["Date"]), y=df_gme_revenue["Revenue"],
                             name="GameStop Revenue", yaxis="y2"))

fig_gme.update_layout(
    title="GameStop Stock Price vs Revenue",
    xaxis_title="Date",
    yaxis=dict(title="Stock Price (USD)"),
    yaxis2=dict(title="Revenue (USD Millions)", overlaying='y', side='right'),
    legend=dict(x=0.01, y=0.99),
    width=1000,
    height=500
)
fig_gme.show()``
