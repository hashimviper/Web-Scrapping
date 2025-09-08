import requests
from bs4 import BeautifulSoup
import pandas as pd

# Target URL
url = "https://www.scrapethissite.com/pages/simple/"
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

countries = soup.find_all('div', class_='col-md-4 country')

# Extract data into a list of dicts
data = []
for country in countries:
    name = country.find('h3', class_='country-name').text.strip()
    capital = country.find('span', class_='country-capital').text.strip()
    population = country.find('span', class_='country-population').text.strip()
    area = country.find('span', class_='country-area').text.strip()
    data.append({
        'Country Name': name,
        'Capital': capital,
        'Population': population,
        'Area (km²)': area
    })

# Convert to DataFrame and save
df = pd.DataFrame(data)
df.to_csv('countries_data.csv', index=False)

print("✅ Data saved to countries_data.csv")
from google.colab import files
files.download('countries_data.csv')
