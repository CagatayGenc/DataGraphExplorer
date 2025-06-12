```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from io import StringIO
import requests
from google.colab import files

def load_csv_from_upload():
    uploaded = files.upload()
    for fn in uploaded.keys():
        df = pd.read_csv(fn)
        return df

def load_csv_from_url_input():
    url = input("Enter the URL of the CSV file: ")
    response = requests.get(url)
    if response.ok:
        df = pd.read_csv(StringIO(response.text))
        return df
    else:
        print("Failed to retrieve the file.")
        return None

def load_csv_from_hardcoded_url():
    url = "https://people.sc.fsu.edu/~jburkardt/data/csv/hw_200.csv"  # Replace with your own
    response = requests.get(url)
    if response.ok:
        df = pd.read_csv(StringIO(response.text))
        return df
    else:
        print("Failed to retrieve the file.")
        return None

def explore_dataframe(df):
    print("\nHeadings:\n", df.columns.tolist())
    print("\nFirst two rows:\n", df.head(2))
    columns = df.columns.tolist()
    return columns

def plot_columns(df, col1, col2=None):
    x = np.array(df[col1])
    
    if col2:
        y = np.array(df[col2])
        plt.scatter(x, y)
        plt.title(f"{col1} vs {col2}")
        plt.xlabel(col1)
        plt.ylabel(col2)
    else:
        plt.plot(x)
        plt.title(f"Line Plot of {col1}")
        plt.xlabel("Index")
        plt.ylabel(col1)
    
    plt.grid(True)
    plt.show()

print("Choose how to load your CSV:\n1. Upload from device\n2. Enter a URL\n3. Use a hardcoded URL")
choice = input("Enter 1, 2, or 3: ")

if choice == '1':
    df = load_csv_from_upload()
elif choice == '2':
    df = load_csv_from_url_input()
else:
    df = load_csv_from_hardcoded_url()

if df is not None:
    cols = explore_dataframe(df)
    print("\nAvailable columns:\n", cols)
    
    col1 = input(f"\nEnter the first column to plot (from above): ")
    col2 = input("Enter the second column to plot (or press Enter to skip): ")
    
    if col2.strip() == "":
        plot_columns(df, col1)
    else:
        plot_columns(df, col1, col2)
else:
    print("Dataframe not loaded properly.")
```
