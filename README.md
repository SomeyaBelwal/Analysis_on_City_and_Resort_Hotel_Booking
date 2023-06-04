# Analysis_on_City_and_Resort_Hotel_Booking
# IMPORTING THE PYTHON LIBRARIES
import pandas as pd

import matplotlib.pyplot as plt

import seaborn as sns

import warnings

warnings.filterwarnings('ignore')
# IMPORTING THE DATASET
df=pd.read_csv('hotel_bookings 2.csv')
# ANALYSING THE DATA
df.head()

df.tail()

df.shape

df.columns

df.info()

# DATA CLEANING
df['reservation_status_date']= pd.to_datetime(df['reservation_status_date'])
# GETTING INSIGHTS AND MAKING CHARTS
df.describe(include=object)

*for col in df.describe(include= 'object').columns:
    print(col)
    print(df[col].unique())
    print('..'*50)
    
*df.isnull().sum()

*df.drop(['company','agent'], axis= 1, inplace= True )

*df.dropna(inplace= True)

*df.isnull().sum()

*df['adr'].plot(kind= 'box')

*cancelled_perc=df['is_canceled'].value_counts(normalize = True)
 cancelled_perc
 plt.figure(figsize=(5,4))
 plt.title('Reservation Status Count')
 plt.bar( ['Not canceled','Canceled'],df['is_canceled'].value_counts(), edgecolor= 'k', width= 0.7)
 
*plt.figure(figsize= (8,4))
ax1 = sns.countplot(x = 'hotel', hue ='is_canceled', data= df, palette ='Greens')
legend_labels,_ =ax1. get_legend_handles_labels()
ax1.legend, bbox_to_anchor=[1,1]
plt.title('Reservation status in different hotels', size = 20) 
plt.xlabel('hotel')
plt.ylabel('number of reservations')
plt.show()

*resort_hotel= df[df['hotel']== 'Resort Hotel']
resort_hotel['is_canceled'].value_counts(normalize= True)

*city_hotel= df[df['hotel']== 'City Hotel']
city_hotel['is_canceled'].value_counts(normalize= True)

*resort_hotel= resort_hotel.groupby('reservation_status_date')[['adr']].mean()
city_hotel =city_hotel.groupby('reservation_status_date')[['adr']].mean()

*plt.figure(figsize = (20,8))
plt.title('Average Daily Rate in City and Resort Hotel', fontsize=30)
plt.plot(resort_hotel.index, resort_hotel['adr'], label= 'Resort Hotel')
plt.plot(city_hotel.index, city_hotel['adr'], label= 'City Hotel')
plt.legend(fontsize='20')
plt.show()

*df['month']=df['reservation_status_date'].dt.month
plt.figure(figsize=(16,8))
ax1= sns.countplot(x = 'month', hue = 'is_canceled', data = df, palette= 'bright')
legend_labels,_= ax1. get_legend_handles_labels()
ax1.legend(bbox_to_anchor=(1,1))
plt.title('Reservation status per month', size = 20)
plt.xlabel('month')
plt.ylabel('number of reservations')
plt.legend(['not canceled', 'canceled'])
plt.show()

*plt.figure(figsize = (15,8))
plt.title('ADR per month', fontsize =30)
sns.barplot('month','adr', data = df[df['is_canceled']].groupby('month')[['adr']].sum())

*cancelled_data = df[df['is_canceled'] == 1]
top_10_country = cancelled_data['country'].value_counts()[:10]
plt.figure(figsize = (8,8))
plt.title('Top 10 countries with reservation canceled')
plt.pie(top_10_country,autopct = '%.2f', labels = top_10_country.index)
plt.show()

*df['market_segment'].value_counts()

*df['market_segment'].value_counts(normalize= True)

*cancelled_data['market_segment'].value_counts(normalize= True)
cancelled_df_adr= cancelled_data.groupby('reservation_status_date')[['adr']].mean() 
cancelled_df_adr.reset_index(inplace=True)
cancelled_df_adr.sort_values('reservation_status_date', inplace=True)

not_cancelled_data=df[df['is_canceled'] == 0]
not_cancelled_df_adr=not_cancelled_data.groupby('reservation_status_date')[['adr']].mean()
not_cancelled_df_adr.reset_index(inplace=True)
not_cancelled_df_adr.sort_values('reservation_status_date', inplace =True)

plt.figure(figsize = (20,6))
plt.title('Average Daily Rate')
plt.plot(not_cancelled_df_adr['reservation_status_date'], not_cancelled_df_adr['adr'], label='not cancelled')
plt.plot(cancelled_df_adr['reservation_status_date'], cancelled_df_adr['adr'], label = 'cancelled')
plt.legend()

*cancelled_df_adr= cancelled_df_adr[(cancelled_df_adr['reservation_status_date']> '2016') & (cancelled_df_adr['reservation_status_date']<'2017-09')]
not_canelled_df_adr=not_cancelled_df_adr[(not_cancelled_df_adr['reservation_status_date']>'2016') & (not_cancelled_df_adr['reservation_status_date']< '2017-09')]

*plt.figure(figsize = (20,6))
plt.title('Average Daily Rate', fontsize=30)
plt.plot(not_cancelled_df_adr['reservation_status_date'], not_cancelled_df_adr['adr'], label='not cancelled') 
plt.plot(cancelled_df_adr['reservation_status_date'], cancelled_df_adr['adr'], label = 'cancelled')
plt.legend (fontsize = 28)
