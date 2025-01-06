# Accident-Report

# This is the code which i used for creating pie chart from the Excel file and the creating the report 

import pandas as pd
import matplotlib.pyplot as plt
import os

# File path to the CSV file
file_path = r'C:/Users/laksh/OneDrive/Desktop/Research/Research topics/Road accidents/Excel/time_data.csv'

# City and specific rows to include in the pie charts
city = 'maharashtra'
rows = ['6:00 - 9:00 am', '09:00-12:00 am', '12:00-15:00 pm', '15:00-18:00 pm', '18:00-21:00 pm', '21:00-24:00 pm', '00:00-03:00 am', '03:00-06:00 am', 'Unknown Time']

if os.path.isfile(file_path):
    # Load the CSV file
    df = pd.read_csv(file_path)

    # Filter the data for the specified city
    city_data = df[df['States/UTs'].str.lower() == city.lower()]

    if not city_data.empty:
        combined_data = []

        for row in rows:
            if row in city_data.columns:
                combined_data.extend(city_data[row].values)
            else:
                print(f"Row '{row}' not found in the data for city: {city}")

        if combined_data:
            total_accidents = sum(combined_data)
            sizes = combined_data
            labels = []

            for i, size in enumerate(sizes):
                percentage = (size / total_accidents) * 100
                labels.append(f"{rows[i]} ({percentage:.1f}%)")

            # Create the combined pie chart
            plt.figure(figsize=(12, 8))  # Increased figure size
            plt.pie(sizes, labels=None, startangle=140)  # Removed autopct
            plt.legend(labels, loc="best", fontsize='small')  # Added percentages to legend

            plt.title(f'Accident Data Distribution for {city.title()} - Combined Road Type')
            plt.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.
            plt.show()
        else:
            print(f"No valid data found for the specified rows in city: {city}")
    else:
        print(f"No data found for city: {city}")
else:
    print("File not found. Please check the file path.")
