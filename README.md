# Otodom_Data_Analysis
### About Otodom

> Otodom is the most popular website in Poland supporting the purchase, sale and rental of real estate. Thanks to many years of experience, huge data resources and the synergy of the OLX Group's real estate services, we understand the users of our website. We also help them make important decisions regarding residence, investment or business.

### Goal of the Project

> The goal of this project is make informed decisions in the real estate market, costumers need to have appropriate knowledge and be up to date. One of the most important pieces of information, regardless of whether costumers are buying, selling or renting, is the price.

### Data preprocessing

> To achieve the goal of the project we need, first, preprocessing the data make it clean enough that we can easy to understand and analysis. We need to use snowflake, python and google sheet to preprocess the whole dataset

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


