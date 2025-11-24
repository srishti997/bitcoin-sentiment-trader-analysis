#Bitcoin Sentiment vs Trader Performance Analysis
Using Hyperliquid Historical Data + Bitcoin Fear & Greed Index

**Project Overview**
This project explores how market sentiment (Fear vs Greed) influences trader performance on Hyperliquid, a Web3 derivatives exchange.
The analysis combines:
- Bitcoin Fear & Greed Index
- Hyperliquid trader execution history
to uncover:
- Which market sentiment leads to better PnL
- How trader behavior changes
- Whether certain conditions create higher risk
- Actionable insights for improving trading strategies
  
 **Dataset Description**
 
1️⃣ Bitcoin Fear & Greed Index
Columns: date, classification
Labels include Fear, Greed, Extreme Fear, Extreme Greed
Source: Provided Google Drive link


2️⃣ Hyperliquid Trader Data
Contains per-trade execution information including:
Timestamp IST
Account
Symbol
Size USD
Side
Closed PnL
Leverage
Event
Start Position


** Data Cleaning & Preprocessing**
Converted timestamps → datetime
Extracted trade date
Converted sentiment date to standard format
Renamed classification → sentiment
Merged sentiment into trades using the date key
Removed trades with missing sentiment
Added a is_win column:
merged['is_win'] = (merged['Closed PnL'] > 0).astype(int)


**Merging Logic**
Each trade was matched with the corresponding sentiment on the same day:
merged = trades.merge(sentiment, left_on='date', right_on='Date', how='left')

This attaches sentiment to every trade.

**Metrics Calculated**
Performance metrics calculated per sentiment group:
Total number of trades
Average PnL per trade
Total PnL
Win rate
Average trade size


Example groupby operation:
performance = merged.groupby('sentiment').agg(
    total_trades=('Closed PnL', 'count'),
    win_rate=('is_win', 'mean'),
    avg_pnl=('Closed PnL', 'mean'),
    total_pnl=('Closed PnL', 'sum'),
    avg_size=('Size USD', 'mean')
).reset_index()


**Visualizations**

Plots generated:
 Average PnL by sentiment
 Win rate by sentiment
 PnL distribution (symlog scale)


Examples:
sns.barplot(data=performance, x='sentiment', y='avg_pnl')
sns.barplot(data=performance, x='sentiment', y='win_rate')
sns.boxplot(data=merged, x='sentiment', y='Closed PnL')

Visuals stored in /charts


** How to Run the Analysis**
1. Install dependencies
pip install pandas numpy matplotlib seaborn gdown

2. Download the datasets
In notebook:
!gdown --id 1PgQC0tO8XN-wqkNyghWc_-mnrYv_nhSf -O fear_greed_index.csv
!gdown --id 1IAfLZwu6rJzyWKgBToqwSmmVYU6VbjVs -O historical_data.csv

3. Run the notebook
Open Assignment.ipynb and run all cells.


Contact
Srishti Gupta
Bengaluru, India
srishtigupta997@gmail.com
LinkedIn: www.linkedin.com/in/srishtigupta997
