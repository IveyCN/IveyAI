# Otodom_Data_Analysis
### About Otodom

> Otodom is the most popular website in Poland supporting the purchase, sale and rental of real estate. Thanks to many years of experience, huge data resources and the synergy of the OLX Group's real estate services, we understand the users of our website. We also help them make important decisions regarding residence, investment or business.

### Goal of the Project

> The goal of this project is make informed decisions in the real estate market, costumers need to have appropriate knowledge and be up to date. One of the most important pieces of information, regardless of whether costumers are buying, selling or renting, is the price.

### Data preprocessing

> To achieve the goal of the project we need, first, preprocessing the data make it clean enough that we can easy to understand and analysis. We need to use snowflake, python and google sheet to preprocess the whole dataset, you can get more info about the data cleaning process in Otodom_analysis.py

__From Web data Scraper__ we have data in Json Format we need to tranfer data into multiple columns so that we can apply aggregage function or transfer data into different datatype 


__After we change the data to flatten table the two most significant challenges we face when performing data preprocessing are respectively.__

* __Address__: Since the Address of asset from the dataset is (longitude，latitude) format, therefore, we cannot locate assets in city, suburb.
* __Title__: The title of the assets in dataset is in Polish, we cannot understand the meanning, so that we need to translate the title into English

__Finnally we combine all of the three table to get the final table we can easily analysis with__

| __Column name__ | __Value__ |
| :--- | :--: |
| ADVERTISER_TYPE | private |
| BALCONY_GARDEN_TERRACE | Ask |
| DESCRIPTION | Przedstawiamy Pastwu ofert sprzeday obiektu inwestycyjnego, w okazyjnej cenie. |
| HEATING | Ask |
| IS_FOR_SALE | TRUE |
| LIGHTING | ASK |
| LOCATION | Longitude: 21.00817, Latitude: 52.23614 |
| PRICE | PLN 2,799,999.00 |
| REMOTE_SUPPORT | NO |
| SURFACE | 160 M^2 |
| TIMESTAMP | 2023/1/20 |
| TITLE | Gociniec Wierzbinowy  - Szukamy Inwestora! |
| URL | https://www.otodom.pl/pl/oferta/gosciniec-wierzbinowy-szukamy-inwestora-ID4jGul |
| FORM_OF_PROPERTY | NULL |
| NO_OF_ROOM | 4 |
| PARKING_SPACE | parking |
| PRICE_NEW | 2799999 |
| SURFACE_NEW | 160 |
| SUBURBAN | ródmieście |
| CITY | ŚWarszawa |
| COUNTRY | NULL |
| TITLE_ENG | Gościniec Wierzbinowy - we are looking for an investor! |
| APARTMENT_FLAG | APARTMENT |

### Market Report
>We need to gerneral analyze the market while we can use the data to satisfy the specific requirement for unique costumers.

__1. What is the average rental price of 1 room, 2 room, 3 room and 4 room apartments in some of the major cities in Poland?__

|CITY|AVG_RENT_1R                  |AVG_RENT_2R|AVG_RENT_3R                                  |AVG_RENT_4R|
|----|-----------------------------|-----------|---------------------------------------------|-----------|
|Gdańsk|2345.56                      |3367.61    |5150.28                                      |10851.79   |
|Łódź|2569.66                      |3294.89    |6106.71                                      |10265.63   |
|Warszawa|2569.98                      |3451.10    |5168.98                                      |9211.72    |
|Kraków|2536.87                      |3292.87    |5320.09                                      |9096.36    |
|Wrocław|2291.78                      |3373.89    |4644.95                                      |8566.28    |
|Katowice|2142.38                      |3691.25    |5386.60                                      |7768.75    |




__2. Costumers want to buy an apartment which is around 90-100 m2 and within a range of 800,000 to 1M, display the suburbs in warsaw which can fit costumers need.__

|SUBURB|NUM_OF_UNITS                 |AVG_PRICE|
|------|-----------------------------|---------|
|Praga-Południe|6                            |888633.16666667|
|Ursus |5                            |896531.40000000|
|Bielany|5                            |893200.00000000|
|Mokotów|4                            |916087.50000000|
|Włochy|3                            |917086.66666667|
|Wola  |3                            |929333.33333333|
|Bemowo|3                            |932666.66666667|
|Białołęka|3                            |929333.33333333|
|Targówek|2                            |907000.00000000|
|Wilanów|2                            |895000.00000000|
|Ursynów|2                            |878210.00000000|
|Praga-Północ|1                            |997663.00000000|
|Wawer |1                            |928000.00000000|

__3. What size of an apartment can I expect with a monthly rent of 3000 to 4000 PLN in different major cities of Poland?__

|CITY|AVG_AREA                     |
|----|-----------------------------|
|Wrocław|52.415762712                 |
|Kraków|52.640703518                 |
|Gdańsk|53.001879699                 |
|Łódź|53.486714286                 |
|Warszawa|54.355676692                 |
|Katowice|55.952                       |


__4. What are the most expensive apartments in major cities of Poland? Display the ad title in english along with city, suburb, cost, size.__

|RN |TITLE_ENG                    |CITY    |SUBURB               |PRICE_NEW  |SURFACE_NEW|URL                                                                                 |
|---|-----------------------------|--------|---------------------|-----------|-----------|------------------------------------------------------------------------------------|
|23764|2pok in Śródmieście, very close to the old town|Gdańsk  |Śródmieście          |15000000.00|350        |https://www.otodom.pl/pl/oferta/dom-350-m-warszawa-ID4eb1H                          |
|17151|2-room apartment 42m2 + loggia without commission|Katowice|Koszutka             |17720000.00|226        |https://www.otodom.pl/pl/oferta/apartament-fort-cze-z-tarasem-na-dachu-500m2-ID4jNhn|
|36524|3-room apartment 62m2 + balcony directly|Kraków  |Czyżyny              |20000000.00|195.7      |https://www.otodom.pl/pl/oferta/mieszkanie-warszawa-srodmiescie-ID4hSfg             |
|55351|Delux apartment in the heart of Powiśle|Warszawa|Wesoła               |20000000.00|577        |https://www.otodom.pl/pl/oferta/topowa-lokalizacja-apartament-z-basenem-ID4iZEj     |
|32257|3-room apartment 55m2 + 2 gardens|Wrocław |Przedmieście Oławskie|9450000.00 |349        |https://www.otodom.pl/pl/oferta/ks-witolda-cale-10-pietro-penthouse-350mkw-ID4im01  |
|25793|3 rooms 2 20m2 balconies ideal for investments.|Łódź    |Łódź-Polesie         |7500000.00 |570        |https://www.otodom.pl/pl/oferta/ekskluzywna-willa-obok-lasku-wolskiego-ID4iLkz      |

__5. What is the percentage of private & business ads on otodom?__

|BUSINESS_ADS_PERC|PRIVATE_ADS_PERC|
|-----------------|----------------|
|89.99%           |10.01%          |


__6. What is the avg sale price for apartments within 50-70 m2 area in major cities of Poland?__

|CITY  |AVG_SALE_PRICE|
|------|--------------|
|Gdańsk|658834.01     |
|Katowice|655172.85     |
|Warszawa|652473.26     |
|Wrocław|643644.85     |
|Łódź  |640945.75     |
|Kraków|639578.23     |



__7. What is the average rental price for apartments in warsaw in different suburbs? Categorize the result based on surface area 0-50, 50-100 and over 100.__

|SUBURB|AVG_PRICE_UPTO_50|AVG_PRICE_UPTO_100|AVG_PRICE_OVER_100|
|------|-----------------|------------------|------------------|
|Bemowo|2472.32          |5312.85           |13187.79          |
|Białołęka|2677.74          |4646.86           |12197.02          |
|Bielany|2906.48          |5460.17           |10748.55          |
|Mokotów|2489.54          |5455.00           |13313.08          |
|Praga-Południe|2922.76          |4956.36           |13052.50          |
|Praga-Północ|2715.15          |4460.63           |10352.70          |
|Rembertów|                 |5460.00           |6565.00           |
|Targówek|2514.04          |5594.00           |11776.00          |
|Ursus |2738.84          |4738.21           |13995.95          |
|Ursynów|2794.08          |5609.15           |15488.05          |
|Wawer |1825.00          |5452.14           |16994.50          |
|Wesoła|3050.00          |5628.50           |21000.00          |
|Wilanów|2587.50          |4884.08           |18180.80          |
|Wola  |2778.99          |4958.11           |10275.08          |
|Włochy|2839.80          |5407.59           |10539.35          |
|Śródmieście|2869.22          |4461.19           |9629.41           |
|Żoliborz|2087.40          |4116.67           |5000.00           |


__8. Which are the top 3 most luxurious neighborhoods in Warsaw? Luxurious neighborhoods can be defined as suburbs which has the most no of of apartments costing over 2M in cost.__

|SUBURB|LUXURIOUS_APARTMENTS|
|------|--------------------|
|Mokotów|34                  |
|Wola  |30                  |
|Praga-Południe|22                  |
|Włochy|22                  |

__9. Most small families would be looking for apartment with 40-60 m2 in size. Identify the top 5 most affordable neighborhoods in warsaw.__

|SUBURB|AVG_PRICE|NO_OF_APARTMENTS|
|------|---------|----------------|
|Ursus |2903.13  |23              |
|Praga-Północ|2916.76  |10              |
|Wawer |2982.22  |9               |
|Śródmieście|3037.86  |28              |
|Bielany|3047.84  |19              |

__10. Which suburb in warsaw has the most and least no of private ads?__

|LEAST_PRIVATE_ADS|MOST_PRIVATE_ADS|
|-----------------|----------------|
|Wesoła - 1       |Wola - 81       |

__11. What is the average rental price and sale price in some of the major cities in Poland?__

|CITY  |AVG_RENTAL|AVG_SALE |
|------|----------|---------|
|Wrocław|5894.31   |871055.22|
|Łódź  |5793.64   |806923.98|
|Kraków|5703.23   |863556.14|
|Warszawa|5536.79   |856875.33|
|Gdańsk|5460.79   |843798.05|
|Katowice|5140.32   |947155.04|
