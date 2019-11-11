#ok let's launch mongo as a docker
```
docker run -d -p 27017-27019:27017-27019 --name mongodb mongo:4.0.4
```
#let's connect
```
docker exec -it mongodb bash
```
#access mongo shell : mongo
```
show dbs

use DBNAME
#new name -> new db
```
#ok now let's kill the stuff
```
ctrl c + ctrl d
docker stop mongodb && docker rm mongodb
```
#let's share some stuff
```
docker run -d -v /code/github/ensiie-mongodb-env/share:/share -p 27017-27019:27017-27019 --name mongodb mongo:4.0.4
docker exec -it mongodb bash
```
#let's go !
```
mongo
```
#let's read db
```
show dbs
```
#To create a new database, we can use a multi-step process, the first being to define the database we wish to use:
```
use thepolyglotdeveloper
```
#We’re using the database thepolyglotdeveloper, but it doesn’t exist until we start creating collections and data. To create a collection with data, we can 
#do something like this:
```
db.people.save({ firstname: "Nic", lastname: "Raboy" })
db.people.save({ firstname: "Maria", lastname: "Raboy" })
```
#With two documents created in a new people collection in our thepolyglotdeveloper database, we can query for data using something like the following:
```
db.people.find({ firstname: "Nic" })
```
```
#let's recreate the stuff
```
ctrl c + ctrl d
docker stop mongodb && docker rm mongodb
docker run -d -v /code/github/ensiie-mongodb-env/share:/share -p 27017-27019:27017-27019 --name mongodb mongo:4.0.4
docker exec -it mongodb bash
mongo
db.people.find({ firstname: "Nic" })
```
#where are my files ???? 

#ok now let's persist the state of the db
docker stop mongodb && docker rm mongodb
docker run -d -v /code/github/ensiie-mongodb-env/state:/data/db -v /code/github/ensiie-mongodb-env/share:/share -p 27017-27019:27017-27019 --name mongodb mongo:4.0.4

#let's load our files
```
mongoimport --host localhost --port 27017 --db forum --collection profiles --file /share/profiles.json --jsonArray
```
#let's load spyder... and start the pdf stuff :D
```
from pymongo import MongoClient
client = MongoClient()
db=client.forum

res=db.profiles.find().limit(10)

for r in res:
    favorites=""
    if "favorites" in r:
        favorites=str(r['favorites'])
        print(r['name']+"\t"+r['lastname']+"\t"+favorites)
```