# UK_food_search_analysis-NoSQL
The module focus on analysis known United Kingdom restaurants as an editor of a known UK food magazine. The evaluation is used to forsee what artciles should be written based on the restaurant's quality. The process revolves using pymongo through python (jupyter notebook) and MongoCLient in order to perform search queries for known restaurant with the json as the reference. The reason why MongoDB was imported into Python was due to ease of use and understanding the versitility of python where the code applies the MongoDB database and transfer to Panda table.

### file contents
In the repository, there are two codes, which is edited through Jupyter notebook, and one folder which contains the json. <br/>
- Json - main raw files containing details about every known United Kingdom restaurant with details from location to Scoring on the quality
- NoSQL_setup_starter.ipynb - python code which updates a known database by changing details 
- NoSQL_analysis_starter.ipynb - python code that perform searches within the json to recieve statistical results


### Development process
In both coding scripts, the first half contains codes which creates a setup sequence that connects the MongoClient to the UK_food json. this is achieved through Anaconda which the library database contained the installed Pymongo and the mongoimport is shown below: <br/>
**json -d uk_food -c establishments --drop --jsonArray establishments.json** <br/>
```

from pymongo import MongoClient
import json
import requests 
from pprint import pprint
from bson import ObjectId

```
With the installed python library, a MongoClient can be created for the connections with the MongoDB database into the python, from there the code establish the database with a name and into a variable.
  ![image](https://github.com/EricTran99/UK_food_search_analysis-NoSQL/assets/134130254/c87fdb58-02ef-46dc-8522-b91e1449de5d)

  <br/>
In the setup script, the code adds an additional restaurant into the exisitng database. This was done by creating a dictionary for the new restaurant (the detail was given manually) which is inserted into the database and checked if it was succefully added. <br/>

![image](https://github.com/EricTran99/UK_food_search_analysis-NoSQL/assets/134130254/68bba5f5-b730-43ca-b802-18981060fc30)

![image](https://github.com/EricTran99/UK_food_search_analysis-NoSQL/assets/134130254/7e351dbd-ce10-47b9-bcc7-b2eceb52edab)
 <br/>
while the new restaurant detail has been added, it's still missing important information. One missing detail is the **Business Type ID**. By using the find command, the ID can be found among the established restaurant as the type ID will be identical. This code is shown below along with the result: <br/>
![image](https://github.com/EricTran99/UK_food_search_analysis-NoSQL/assets/134130254/26e8194d-78e7-4aeb-b342-d86e2bc8dfb4)
 <br/>
With the known Business Type ID, the update command is used on the new restaurant (with the known variable as inserted_id) for the new details, which is shown below: <br/>
 ![image](https://github.com/EricTran99/UK_food_search_analysis-NoSQL/assets/134130254/a03df2c1-9eee-497b-b6fd-030fb449134c)
 <br/>
The remaining code removed all restaurant that have the LocalAuthorityName as **Dover** and edit the data type for the latitude and longitude from String to Decimals
```
db.establishments.delete_many({'LocalAuthorityName':'Dover'})

```
 <br/>
```

db.establishments.update_many({},[{'$set':{'geocode.longitude':{'$toDouble': "$geocode.longitude"}}}])
db.establishments.update_many({},[{'$set':{'geocode.latitude':{'$toDouble': "$geocode.latitude"}}}])

```
 <br/>
The second code script examines the database with query as filters to find specific restaurant, along with the command to display the first known document. The filtered results are converted into Panda DataFrame in order to display ten more restaurant in a more visual clear way rather through pretty print command.  Below is an example of one of the analysis prompt. <br/>
This is the script with the query and field descriptions included: <br/>
```

query = {'scores.Hygiene': 20}
fields = {'BusinessName': 1, 'BusinessType': 1, 'BusinessTypeID': 1, 'Distance': 1,'AddressLine2':1, 
          'LocalAuthorityName': 1, 'LocalAuthorityWebSite': 1, 
          'RatingValue': 1, 'scores.Hygiene': 1}

results = list(establishments.find(query, fields))
count_results = db.establishments.count_documents(query)
# Use count_documents to display the number of documents in the result
print("number of documents: ", count_results)
print(" ")
# Display the first document in the results using pprint
pprint(db.establishments.find_one({'scores.Hygiene': 20}))

```
Below is the result of the search command <br/>
```

number of documents:  41
 
{'AddressLine1': '5-6 Southfields Road',
 'AddressLine2': 'Eastbourne',
 'AddressLine3': 'East Sussex',
 'AddressLine4': '',
 'BusinessName': 'The Chase Rest Home',
 'BusinessType': 'Caring Premises',
 'BusinessTypeID': 5,
 'ChangesByServerID': 0,
 'Distance': 4613.888288172291,
 'FHRSID': 110681,
 'LocalAuthorityBusinessID': '4029',
 'LocalAuthorityCode': '102',
 'LocalAuthorityEmailAddress': 'Customerfirst@eastbourne.gov.uk',
 'LocalAuthorityName': 'Eastbourne',
 'LocalAuthorityWebSite': 'http://www.eastbourne.gov.uk/foodratings',
 'NewRatingPending': False,
 'Phone': '',
 'PostCode': 'BN21 1BU',
 'RatingDate': '2021-09-23T00:00:00',
 'RatingKey': 'fhrs_0_en-gb',
 'RatingValue': '0',
 'RightToReply': '',
 'SchemeType': 'FHRS',
 '_id': ObjectId('64e48a0054f73c0d0c9a06eb'),
 'geocode': {'latitude': 50.769705, 'longitude': 0.27694},
 'links': [{'href': 'https://api.ratings.food.gov.uk/establishments/110681',
            'rel': 'self'}],
 'meta': {'dataSource': None,
          'extractDate': '0001-01-01T00:00:00',
          'itemCount': 0,
          'pageNumber': 0,
          'pageSize': 0,
          'returncode': None,
          'totalCount': 0,
          'totalPages': 0},
 'scores': {'ConfidenceInManagement': 20, 'Hygiene': 20, 'Structural': 20}}

```
Below is the conversion from Mongo filtered results into Panda DataFrame <br/>
```

results_df1 = pd.DataFrame(results)

print("number of row in Dataframe", len(results_df1))
print(" ")

results_df1.head(10)

```
Below is the Panda Dataframe display <br/>
![image](https://github.com/EricTran99/UK_food_search_analysis-NoSQL/assets/134130254/0847ac68-4d01-41f4-91c4-7266c6d1984e)

