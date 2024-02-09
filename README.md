![image](https://github.com/AKIAANA/Predicting-Falcon-9-First-Stage-Landing-Success-A-Data-Science-Capstone-Project.-/assets/138131607/fa463ae1-6f3e-479c-b4f6-8305e2cde53c)# Predicting-Falcon-9-First-Stage-Landing-Success-A-Data-Science-Capstone-Project.-
Data Collection and Data Wrangling
Data collection involves obtaining the Falcon 9 landing dataset, and data wrangling focuses on cleaning and preprocessing the data by handling missing values and performing necessary feature engineering. The data for this project is collected from from Wikipedia hosting falcon9 launch details (https://en.wikipedia.org/w/index.php?title=List_of_Falcon_9_and_Falcon_Heavy_launches&oldid=1027686922
![image](https://github.com/AKIAANA/Predicting-Falcon-9-First-Stage-Landing-Success-A-Data-Science-Capstone-Project.-/assets/138131607/b473f6ec-5273-46f3-8704-5af5e6f30221)

import pandas as pd
import requests
from bs4 import BeautifulSoup

# Input the URL of the Web page to collect data
wikipedia_url = 'https://en.wikipedia.org/w/index.php?title=List_of_Falcon_9_and_Falcon_Heavy_launches&oldid=1027686922'
# Send a GET request to the Wikipedia page
response = requests.get(wikipedia_url)

# Check if the request was successful (status code 200)
if response.status_code == 200:
    # Create a BeautifulSoup object from the response text content
    soup = BeautifulSoup(response.text, 'html.parser')
    # Find the table with the class 'wikitable' (you may need to inspect the HTML to find the correct class)
    table = soup.find('table', {'class': 'wikitable'})
    # Read the HTML table into a DataFrame using pandas
    df = pd.read_html(str(table))[0]
    # Data wrangling: Clean up the DataFrame
    # For example, remove unnecessary rows and columns, handle missing values, etc.
    # Drop rows with 'NaN' in the 'Date and time (UTC)' column
    df = df.dropna(subset=['Date and time (UTC)'])
    # Convert the 'Date and time (UTC)' column to datetime format
    df['Date and time (UTC)'] = pd.to_datetime(df['Date and time (UTC)'], errors='coerce')
    # Set the 'Date and time (UTC)' column as the index
    df.set_index('Date and time (UTC)', inplace=True)
    # Display the cleaned DataFrame
    print(df.head())

else:
    # If the request was not successful, print an error message
    print(f"Error: {response.status_code}")



