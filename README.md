# Otodom_Data_Analysis
### About Otodom

> Otodom is the most popular website in Poland supporting the purchase, sale and rental of real estate. Thanks to many years of experience, huge data resources and the synergy of the OLX Group's real estate services, we understand the users of our website. We also help them make important decisions regarding residence, investment or business.

### Goal of the Project

> The goal of this project is make informed decisions in the real estate market, costumers need to have appropriate knowledge and be up to date. One of the most important pieces of information, regardless of whether costumers are buying, selling or renting, is the price.

### Data preprocessing

> To achieve the goal of the project we need, first, preprocessing the data make it clean enough that we can easy to understand and analysis. We need to use snowflake, python and google sheet to preprocess the whole dataset

__From Web data Scraper__ we have data in this format

|  "advertiser_type": "business",
  "balcony_garden_terrace": "Balcony",
  "description": "<p>Gdańsk Przymorze to najbardziej atrakcyjna pod względem infrastruktury i lokalizacji dzielnica Gdańska, idealne miejsce do wygodnego zamieszkania. <br><br>Dzielnica z pełną infrastrukturą handlowo - usługową, wszelkimi sklepami, punkami usługowymi, marketami, rynkiem ze świeżą, zdrową żywnością. W odległości spacerowej (500 metrów) zielone, rekreacyjne tereny Parku Reagana, a w odległości 1,5 km piaszczysta plaża i nadmorska promenada. W okolicy centrum biurowe Olivia Business Centre oraz Alchemia, szkoły, przychodnie, wszystko to, co potrzebne jest do wygodnego zamieszkania. Szybki dojazd do innych części Trójmiasta umożliwia pobliska stacja kolei SKM Gdańsk Przymorze lub Gdańsk Oliwa oraz przystanki autobusowe i tramwajowe. <br><br>Mieszkanie o powierzchni 55 m2 znajduje się na 3. piętrze apartamentowca w inwestycji „Cztery Oceany” i dzięki ekspozycji zachodniej i dużemu 9-metrowemu, słonecznemu tarasowi stanowi wyjątkowo atrakcyjne lokum. Na wnętrze apartamentu składają się: pokój dzienny (18,3 m2) z aneksem kuchennym (5,8 m2), sypialnia (13,5m2), łazienka z wanną (5,6m2), garderoba (5,6 m2) oraz przedpokój (6,4 m2). <br><br>Apartament wykończony w ponadczasowym stylu, z wykorzystaniem materiałów dobrej jakości. W salonie wygodna strefa wypoczynku oraz okrągły stół, idealny do wspólnego spędzania czasu. Aneks kuchenny w naturalnych barwach, wyposażony we wszystkie niezbędne urządzenia AGD, wyraźnie oddzielony od salonu. Sypialnia aktualnie zaaranżowana na pokój dziecięcy. Przestronna łazienka z wanną. <br><br>Bardzo wygodnie wypoczywa się na tarasie, przestronnym na tyle, aby ustawić na nim leżaki, meble wypoczynkowe czy stół obiadowy. Szeroka panorama miasta rozciągająca się za oknami daje poczucie przestrzeni. <br><br>Mieszkanie może również świetnie sprawdzić się jako inwestycja przeznaczona na najem długo lub krótkoterminowy. <br><br>Budynek jest strzeżony, monitorowany, z całodobową ochroną w eleganckiej portierni. Dla wygody mieszkańców w budynku znajdują się dwie szybkobieżne, ciche windy. <br><br>Na drugiej kondygnacji przyjemne zielone patio z placem zabaw, do wyłącznego użytku mieszkańców, a na parterze rowerownia.<br><br>Do mieszkania przynależy duża komórka lokatorska - 8,5 m2 w cenie 50 tys. zł. oraz miejsce parkingowe dodatkowo płatne 50 tys. zł. <br><br>Przedstawiona oferta ma charakter informacyjny, nie stanowi oferty handlowej w rozumieniu Art. 66 par.1 Kodeksu Cywilnego.<br><br>NABA HOUSE pomaga też w uzyskaniu kredytu hipotecznego. Mamy najlepszych specjalistów kredytowych, możliwość uzyskania kredytu z opcją gwarantowanego zadatku.</p>",
  "form_of_property": "full ownership",
  "heating": "urban",
  "is_for_sale": true,
  "lighting": "Ask",
  "location": "Longitude: 18.59598 | Latitude: 54.40964",
  "no_of_rooms": 2,
  "parking_space": "garaż/miejsce parkingowe",
  "price": "PLN 995,000.00",
  "remote_support": "No",
  "rent_sale": "No",
  "surface": "55,10 m²",
  "timestamp": "2023-01-20",
  "title": "Mieszkanie z tarasem i panoramicznym widokiem.",
  "url": "https://www.otodom.pl/pl/oferta/mieszkanie-z-tarasem-i-panoramicznym-widokiem-ID4i7Fe"|

__The first two most significant challenges we face when performing data preprocessing are respectively.__

* __Address__: Since the Address of asset from the dataset is (longitude，latitude) format, therefore, we cannot locate assets in city, suburb.
* __Title__: The title of the assets in dataset is in Polish, we cannot understand the meanning, so that we need to translate the title into English

__Location data transfer to address__
```python
geolocator = Nominatim(user_agent="otodomprojectanalysis")
with engine.connect() as conn:
    try:
        query = """ SELECT RN, concat(latitude,',',longitude) as LOCATION
                    FROM (SELECT RN
                            , SUBSTR(location, REGEXP_INSTR(location,' ',1,4)+1) AS LATITUDE 
                            , SUBSTR(location, REGEXP_INSTR(location,' ',1,1)+1, (REGEXP_INSTR(location,' ',1,2) - REGEXP_INSTR(location,' ',1,1) - 1) ) AS LONGITUDE
                        FROM otodom_data_flatten
                        ORDER BY rn  ) """
        print("--- %s seconds ---" % (time.time() - start_time))
        
        df = pd.read_sql(query,conn)
                      
        df.columns = map(lambda x: str(x).upper(), df.columns)
        
        ddf = dd.from_pandas(df,npartitions=10)
        print(ddf.head(5,npartitions=-1))

        ddf['ADDRESS'] = ddf['LOCATION'].apply(lambda x: geolocator.reverse(x).raw['address'],meta=(None, 'str'))
        print("--- %s seconds ---" % (time.time() - start_time))

        pandas_df = ddf.compute()
        print(pandas_df.head())
        print("--- %s seconds ---" % (time.time() - start_time))

        pandas_df.to_sql('otodom_data_flatten_address', con=engine, if_exists='append', index=False, chunksize=16000, method=pd_writer)
    except Exception as e:
        print('--- Error --- ',e)
    finally:
        conn.close()
engine.dispose()
```
__Title Text translate to English__
```python

```
