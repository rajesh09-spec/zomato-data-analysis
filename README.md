# zomato-data-analysis
# Step 1: Import Required Libraries
import pandas as pd
import matplotlib.pyplot as plt
# Step 2: Load the Dataset
df = pd.read_csv("Zomato.csv", encoding='latin-1')
print("✅ Dataset Loaded Successfully!")
# Step 3: Keep Only Useful Columns
df = df[['name', 'location', 'rate', 'cuisines', 'approx_cost(for two people)', 'online_order']]
print("✅ Columns Trimmed!")
# Step 4: Clean 'rate' Column
df = df[df['rate'].notnull()]
df['rate'] = df['rate'].apply(lambda x: str(x).split('/')[0].strip())
df = df[df['rate'] != 'NEW']
df = df[df['rate'] != '-']
df['rate'] = df['rate'].astype(float)
print("✅ Ratings Cleaned!")
# Step 5: Top 5 Locations with Most Restaurants
top_locations = df['location'].value_counts().head(5)
top_locations.plot(kind='bar', color='skyblue')
plt.title("Top 5 Locations with Most Restaurants")
plt.xlabel("Location")
plt.ylabel("Number of Restaurants")
plt.show()
# Step 6: Online Order vs Rating
df_online = df[df['online_order'] == 'Yes']
df_offline = df[df['online_order'] == 'No']
avg_online_rating = df_online['rate'].mean()
avg_offline_rating = df_offline['rate'].mean()
print("📱 Average Rating (Online Orders):", round(avg_online_rating, 2))
print("🚫 Average Rating (No Online Orders):", round(avg_offline_rating, 2))
# Step 7: Most Popular Cuisines
df['cuisines'] = df['cuisines'].astype(str)
df['main_cuisine'] = df['cuisines'].apply(lambda x: x.split(',')[0])
df['main_cuisine'].value_counts().head(5).plot(kind='bar', color='orange')
plt.title("Top 5 Most Popular Cuisines")
plt.xlabel("Cuisine")
plt.ylabel("Count")
plt.show()
# Step 8: Summary
print(
    "
✅ Project Summary:\n- Top locations with highest number of restaurants visualized\n- Online ordering slightly affects average rating\n- Most common cuisines revealed (e.g., North Indian)\n✨ You can now add this to your resume or GitHub!"
)
