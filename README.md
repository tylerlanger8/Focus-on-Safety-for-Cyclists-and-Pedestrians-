# Focus-on-Safety-for-Cyclists-and-Pedestrians-
Focus on Safety for Cyclists and Pedestrians 
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sb
import numpy as np

# First Data Source
crashDF = pd.read_csv('Crash.csv')
crashDF['DUI'] = crashDF['DUI'].replace({'YES': 1, 'NO': 0})
crashDF['AlcoholDrugSuspected'] = crashDF['AlcoholDrugSuspected'].replace({'YES': 1, 'NO': 0})
crashDF = crashDF.dropna()
crashDF

crashDF.columns

ropCrashDf = crashDF.dropna(subset=['Lat', 'Lon'])

import folium
dropnaDF = dropCrashDf

map_center = [dropnaDF['Lat'].mean(), dropnaDF['Lon'].mean()]  
crashMap = folium.Map(location=map_center, zoom_start=12)

pedestrianDF = dropnaDF[dropnaDF['PedestriansInvolved'] == 'YES']
for _, row in pedestrianDF.iterrows():
    folium.Marker(location=[row['Lat'], row['Lon']], icon=folium.Icon(color='red')).add_to(crashMap)

bicyclistDF = dropnaDF[dropnaDF['BicyclesInvolved'] == 'YES']
for _, row in bicyclistDF.iterrows():
    folium.Marker(location=[row['Lat'], row['Lon']], icon=folium.Icon(color='blue')).add_to(crashMap)


crashMap.save('mapMarkers.html')

from IPython.display import HTML
html_file_path = 'mapMarkers.html'
HTML(f'<iframe src={html_file_path} width=700 height=500></iframe>')
bicycleCrashes = crashDF[crashDF['BicyclesInvolved'] == 'YES'].shape[0]
pedestrianCrashes = crashDF[crashDF['PedestriansInvolved'] == 'YES'].shape[0]

# Display the counts
print("Number of Bicycle Crashes:", bicycleCrashes)
print("Number of Pedestrian Crashes:", pedestrianCrashes)

graphDF2 = crashDF[['PrmaryCollsionType', 'CrossStreet', 'StreetType', 'DUI', 'BicyclesInvolved','NumInjuredOrKilled','RoadDescription', 'Lat', 'Lon','Crash_Time']]

graphDF2['BicyclesInvolved'] = graphDF2['BicyclesInvolved'].replace({'YES': 1, 'NO': 0})

bikeDF = graphDF2[graphDF2['BicyclesInvolved'] == 1]
bikeDF

injureORkilledDF = bikeDF.sort_values(by='NumInjuredOrKilled', ascending=False)
injureORkilledDF.head(15)
bikeCrashLOC = injureORkilledDF['CrossStreet'].value_counts()
bikeCrashLOC.head(15)

bikeChartData = bikeCrashLOC.head(15)

plt.figure(figsize=(10, 6))
plt.scatter(bikeChartData.index, bikeChartData.values, color='Red')
plt.title('Deaths of Bicyclist')
plt.xlabel('Cross Street')
plt.ylabel('Deaths')
plt.xticks(rotation=45, ha='right') 
plt.show()

graphDF3 = crashDF[['PrmaryCollsionType', 'CrossStreet', 'StreetType', 'DUI', 'PedestriansInvolved','RoadDescription','NumInjuredOrKilled', 'Lat', 'Lon','Crash_Time']]

graphDF3['PedestriansInvolved'] = graphDF3['PedestriansInvolved'].replace({'YES': 1, 'NO': 0})

peopleDF = graphDF3[graphDF3['PedestriansInvolved'] == 1]
peopleDF

injuredPeopleDF = peopleDF.sort_values(by='NumInjuredOrKilled', ascending=False)
injuredPeopleDF.head(15)

peopleCrashLOC = injuredPeopleDF['CrossStreet'].value_counts()
plotdata = peopleCrashLOC.head(15)
plotdata

plt.figure(figsize=(10, 6))
plt.scatter(plotdata.index, plotdata.values, color='Black')
plt.title('Deaths of Pedestrians')
plt.xlabel('Cross Street')
plt.ylabel('Deaths')
plt.xticks(rotation=45, ha='right') 
plt.show()

import folium

broadwayAccDF = crashDF[(crashDF['CrossStreet'].str.contains('BROADWAY'))]

map1 = [broadwayAccDF['Lat'].mean(), broadwayAccDF['Lon'].mean()]  # Center the map around the average coordinates
BroadwayMap = folium.Map(location=map1, zoom_start=13)


for index, row in filtered_df.iterrows():
    folium.Marker(location=[row['Lat'], row['Lon']], popup=row['CrossStreet']).add_to(BroadwayMap)
BroadwayMap.save('accident_map.html')

len(broadwayAccDF)

from IPython.display import HTML
html_file_path = 'accident_map.html'
HTML(f'<iframe src={html_file_path} width=700 height=500></iframe>')

sns.barplot(x='DUI', y='NumInjuredOrKilled', data=crashDF)
plt.title('Number of Injured or Killed by DUI Status')
plt.xlabel('DUI Status (0: No, 1: Yes)')
plt.ylabel('Number of Injured or Killed')
plt.yticks([])
plt.show()
