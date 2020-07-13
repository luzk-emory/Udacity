# Exploration of the Ford GoBike System Data

## by Zhikun Lu

Project #5: Communicate Data Findings Project - Udacity Data Analyst Nanodegree 

Data files are not uploaded to GitHub


## Dataset

This dataset is from Lyft Bay Wheel. (source: https://www.lyft.com/bikes/bay-wheels/system-data) It includes information about individual rides made in a bike-sharing system covering the greater San Francisco Bay area. Each trip is anonymized and includes:

- Start Time and Date
- End Time and Date
- Start Station ID
- Start Station Name
- Start Station Latitude
- Start Station Longitude
- End Station ID
- End Station Name
- End Station Latitude
- End Station Longitude
- User Type (Subscriber or Customer – “Subscriber” = Member or “Customer” = Casual)

and other variables.


## Summary of Findings

1. More trips occur during 7-9am and 4-6pm, i.e. the morning peak and the evening peak. 
2. There are more trips on weekdays than on weekends.
3. There are more trips in Mar, Arp, and Jul, and less trips on Dec.
4. Most trips end within 30 minutes; the average trip length is 13 minutes.
5. Subscribers ride more often during rish hours, i.e. the morning peak and the evening peak, than Customers.
6. Subscribers ride less on weekends, while weekday and weekend make no much difference for customers.
7. The demand pattern for subscribers is more stable across months , except for December. Subscribers' number of rides drops dramatically on December.
8. Subscribers on average spend less time on a ride.
9. The morning peak and the evening peak are only valid for weekdays. They disappear on weekends.
10. The Covid 19 has significant negative impact on the bike-sharing business. The usage dropped by 80% from the beginning of March to the end of March.
11. While the number of rides dropped, the average ride duration almost doubled.


## Key Insights for Presentation

1. Daily Riding Pattern
	- More trips occur during 7-9am and 4-6pm, i.e. the morning peak and the evening peak.
	- Subscribers ride more often during rish hours than Customers.
2. Weekly Riding Pattern
	- There are more trips on weekdays than on weekends.
	- Subscribers ride more on weekdays and less on weekends, while customers have a more flat pattern.
3. Covid 19 has a huge impact on the bike-sharing business.
	- The weekly number of rides dropped by 80% in March.
	- Average riding time almost doubled after the Covid 19 outbreak.

## Slides

Use `jupyter nbconvert slide_deck.ipynb --to slides --post serve --template output_toggle` to view the slides in presentation mode.