# Finak-Project-
import pandas as pd
import matplotlib.pyplot as plt

metro_cols = [col for col in df_encoded.columns if col.startswith('500m_metro_distance')]

stations_stats = []

for col in metro_cols:
    station_name = col.replace('500m_metro_distance_', '')
    
    apartments_near_station = df_encoded[df_encoded[col] == 1]
    
    if len(apartments_near_station) > 0:
        avg_price = apartments_near_station['price_czk'].mean()
        avg_price_m2 = (apartments_near_station['price_czk'] / apartments_near_station['usable_area']).mean()
        num_apartments = len(apartments_near_station)
        
        stations_stats.append({
            'station': station_name,
            'avg_price': avg_price,
            'avg_price_m2': avg_price_m2,
            'num_apartments': num_apartments
        })

stations_df = pd.DataFrame(stations_stats)

stations_df = stations_df.sort_values(by='avg_price_m2', ascending=False).reset_index(drop=True)

print("Top 10 most expensive stations by price per m2:")
print(stations_df.head(10))

print("\nTop 10 cheapest stations by price per m2:")
print(stations_df.tail(10))

plt.figure(figsize=(14, 6))
plt.bar(stations_df['station'][:10], stations_df['avg_price_m2'][:10], color='red')
plt.xticks(rotation=45, ha='right')
plt.title('Top 10 Most Expensive Metro Stations (price per m2)')
plt.ylabel('Average Price per m2 (CZK)')
plt.show()

plt.figure(figsize=(14, 6))
plt.bar(stations_df['station'][-10:], stations_df['avg_price_m2'][-10:], color='green')
plt.xticks(rotation=45, ha='right')
plt.title('Top 10 Cheapest Metro Stations (price per m2)')
plt.ylabel('Average Price per m2 (CZK)')
plt.show()
