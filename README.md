# Video-Games-Sales
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv(r"C:\Users\Dell\OneDrive\video games sales.csv")


df.dropna(inplace=True)
df.columns = df.columns.str.strip()


df['Year'] = df['Year'].astype(int)


top_publishers = df.groupby("Publisher")["Global_Sales"].sum().sort_values(ascending=False).head(10)

plt.figure(figsize=(12, 6))
sns.barplot(x=top_publishers.values, y=top_publishers.index, hue=top_publishers.index, palette="viridis", legend=False)
plt.title("Top 10 Publishers by Global Sales", fontsize=14)
plt.xlabel("Global Sales (in millions)")
plt.ylabel("Publisher")
plt.tight_layout()
plt.show()


games_per_year = df['Year'].value_counts().sort_index()

plt.figure(figsize=(14, 6))
sns.lineplot(x=games_per_year.index, y=games_per_year.values, marker='o', color='orange')
plt.title("Number of Games Released Per Year", fontsize=14)
plt.xlabel("Year")
plt.ylabel("Number of Games")
plt.grid(True)
plt.tight_layout()
plt.show()


avg_sales_by_genre = df.groupby("Genre")["Global_Sales"].mean().sort_values(ascending=False)

plt.figure(figsize=(12, 6))
sns.barplot(x=avg_sales_by_genre.values, y=avg_sales_by_genre.index, hue=avg_sales_by_genre.index, palette="coolwarm", legend=False)
plt.title("Average Global Sales per Genre", fontsize=14)
plt.xlabel("Average Global Sales (in millions)")
plt.ylabel("Genre")
plt.tight_layout()
plt.show()


region_genre_sales = df.groupby("Genre")[["NA_Sales", "EU_Sales", "JP_Sales", "Other_Sales"]].sum()

plt.figure(figsize=(10, 6))
sns.heatmap(region_genre_sales, annot=True, fmt=".1f", cmap="YlGnBu")
plt.title("Regional Sales by Genre (in millions)", fontsize=14)
plt.tight_layout()
plt.show()


platform_counts = df['Platform'].value_counts().head(10)

plt.figure(figsize=(10, 5))
sns.barplot(x=platform_counts.index, y=platform_counts.values, hue=platform_counts.index, palette="magma", legend=False)
plt.title("Top 10 Platforms by Number of Games Released")
plt.xlabel("Platform")
plt.ylabel("Number of Games")
plt.tight_layout()
plt.show()


df['Success'] = df['Global_Sales'] > 1
success_ratio = df.groupby('Publisher')['Success'].mean().sort_values(ascending=False).head(10)

plt.figure(figsize=(12, 6))
sns.barplot(x=success_ratio.values, y=success_ratio.index, hue=success_ratio.index, palette="cubehelix", legend=False)
plt.title("Top 10 Publishers by Success Ratio (Sales > 1M)")
plt.xlabel("Success Ratio")
plt.ylabel("Publisher")
plt.tight_layout()
plt.show()


plt.figure(figsize=(8, 6))
sns.heatmap(df[["NA_Sales", "EU_Sales", "JP_Sales", "Other_Sales", "Global_Sales"]].corr(), annot=True, cmap="coolwarm")
plt.title("Sales Correlation Matrix")
plt.tight_layout()
plt.show()
