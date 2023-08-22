# Assignment-12-NoSQL
This is module 12 assignment focusing on using NoSQL (MongoDB) through Python

In the repository, there are two codes, which is edited through Jupyter notebook, and one folder which contains the json.

- Two ipynb codes that are the main part of the assigment.
- folder 'Resources' which conatins the large json (achieved through using git add/push from gitbash)

How does it work?
the first code 'NoSQL_setup_starter.ipynb' contains the codes which extracts the json data through mongodb.
In the notebook it contains the code for the mongo import. It's (mongoimport --type json -d uk_food -c establishments --drop --jsonArray establishments.json)
This can be run again as the --drop-- arrary is included.
The notebook takes the existing data and upload a new dataset, while editing the data to align with the current database.

The second code 'NoSQL_analysis_starter.ipynb focus on analysing the database and filter the data through query and sort. 
