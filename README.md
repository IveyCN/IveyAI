# Otodom_Data_Analysis
### About Otodom

> Otodom is the most popular website in Poland supporting the purchase, sale and rental of real estate. Thanks to many years of experience, huge data resources and the synergy of the OLX Group's real estate services, we understand the users of our website. We also help them make important decisions regarding residence, investment or business.

### Goal of the Project

> The goal of this project is make informed decisions in the real estate market, costumers need to have appropriate knowledge and be up to date. One of the most important pieces of information, regardless of whether costumers are buying, selling or renting, is the price.

### Data preprocessing

> To achieve the goal of the project we need, first, preprocessing the data make it clean enough that we can easy to understand and analysis.

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
