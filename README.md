# World_Weather_Analysis
## Purpose
In this World Weather Analysis, API's were used in conjunction with Pandas to create a weather map and travel itinerary.

## Summary
This project consisted of randomly generating multiple random latitude and longitude combinations by using numpy.
```
lats = np.random.uniform(low=-90.000, high=90.000, size=2000)
lngs = np.random.uniform(low=-180.000, high=180.000, size=2000)
lat_lngs = zip(lats, lngs)
lat_lngs
```
These coordinates were then run against citipy in order to capture actual cities within the random dataset.
```
# Create a list for holding the cities
cities = []
# Identigy the nearest city for each latitude and longitude combination
for coordinate in coordinates:
    city = citipy.nearest_city(coordinate[0], coordinate[1]).city_name
    
    # If the city is unique, then we will add it to the cities list
    if city not in cities:
        cities.append(city)
```
Then, after capturing the cities, and API call was done from open weather map to get current weather conditions. This data was then collected into a dataframe to be used in Google API calls.
![image](https://user-images.githubusercontent.com/90691846/138780682-497b9eb7-33d9-4841-8e54-cdae41527580.png)

In the second deliverable, the data gathered earlier was utilized to be able to capture hotel data for the random cities generated. In order to find suitable hotesl, code was created for user input to provide minimum and maximum temperatures for an ideal vacation spot. This was accomplished by calling Google API for nbearby search.
```
# 6a. Set parameters to search for hotels with 5000 meters.
params = {
    "radius": 5000,
    "type": "lodging",
    "key": g_key
}

# 6b. Iterate through the hotel DataFrame.
for index, row in hotel_df.iterrows():
    # 6c. Get latitude and longitude from DataFrame
    lat = row["Lat"]
    lng = row["Lng"]
    params["location"] = f"{lat},{lng}"
    
    # 6d. Set up the base URL for the Google Directions API to get JSON data.
    base_url = "https://maps.googleapis.com/maps/api/place/nearbysearch/json"

    # 6e. Make request and retrieve the JSON data from the search. 
    hotels = requests.get(base_url, params=params).json() 
    
    # 6f. Get the first hotel from the results and store the name, if a hotel isn't found skip the city.
    try:
        hotel_df.loc[index, "Hotel Name"] = hotels["results"][0]["name"]
    except (IndexError):
        print("Hotel not found... skipping.")
```
This data was put into a clean dataframe with the following map as a result:
![image](https://user-images.githubusercontent.com/90691846/138780926-2f11352a-0ede-4d89-8fea-4588fc96e1bf.png)

Finally, in deliverable 3, a vacation itinerary was created for traveling to 4 cities in Australia. Another Google API call was utilized with the following results:
![image](https://user-images.githubusercontent.com/90691846/138781145-5abffa54-094e-4ec4-bb74-ca693ef1638c.png)
![image](https://user-images.githubusercontent.com/90691846/138781183-e46f6a2c-2c4b-4962-96f7-11e59b76a163.png)

This was an excellent exercise in showcasing API calls as well as utilizing Pandas.
