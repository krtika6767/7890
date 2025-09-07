import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
fear_greed_df = pd.read_csv('fear_greed_index.csv')
historical_data_df = pd.read_csv('historical_data.csv')
fear_greed_df['date'] = pd.to_datetime(fear_greed_df['date'])
historical_data_df['Timestamp IST'] = pd.to_datetime(historical_data_df['Timestamp IST'], format='%d-%m-%Y %H:%M')
historical_data_df['date'] = historical_data_df['Timestamp IST'].dt.date
historical_data_df['date'] = pd.to_datetime(historical_data_df['date'])
historical_data_df['date'] = historical_data_df['Timestamp IST'].dt.date
historical_data_df['date'] = pd.to_datetime(historical_data_df['date'])
merged_df = pd.merge(historical_data_df, fear_greed_df, on='date', how='left')
merged_df.drop(columns=['timestamp', 'Timestamp IST'], inplace=True)
avg_pnl_by_sentiment = merged_df.groupby('classification')['Closed PnL'].mean().reset_index()
avg_pnl_by_sentiment = avg_pnl_by_sentiment.sort_values(by='Closed PnL', ascending=False)
plt.figure(figsize=(10, 6))
sns.barplot(x='classification', y='Closed PnL', data=avg_pnl_by_sentiment, palette='viridis')
plt.title('Average Closed PnL by Market Sentiment Classification')
plt.xlabel('Market Sentiment Classification')
plt.ylabel('Average Closed PnL ($)')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.savefig('avg_pnl_by_sentiment.png')
print("Average PnL by sentiment classification:")
print(avg_pnl_by_sentiment)
