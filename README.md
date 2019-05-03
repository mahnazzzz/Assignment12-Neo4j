# Assignment 12, Neo4J

This assignment is about the analysis of twitter data from the British Islands. 

I have downloaded data from here: 
https://github.com/datsoftlyngby/soft2019spring-databases/blob/master/data/some2016UKgeotweets.csv.zip

You can find task description [here](https://github.com/datsoftlyngby/soft2019spring-databases/blob/master/assignments/assignment12.md)

### Make a docker container and load now4j image 

* docker run \ -d --name neo4j \ --rm \ --publish=7474:7474 \ --publish=7687:7687 \ --env NEO4J_AUTH=neo4j/123 \ neo4j

### Load csv filen in docker container
    
* docker cp some2016UKgeotweets.csv neo4j:/var/lib/neo4j/import.

### Access to our database in this browser

* http://localhost:7474/browser/

## Exercise 1
If you give your neo4j docker container this flag '-v $(pwd):/var/lib/neo4j/import', you should be able to load the data using this query:

```cypher
LOAD CSV WITH HEADERS FROM "file:///some2016UKgeotweets.csv" AS row 
    FIELDTERMINATOR ";"
return row
LIMIT 1
```
```
LOAD CSV WITH HEADERS FROM "file:///some2016UKgeotweets.csv" AS row 
    FIELDTERMINATOR ";"
CREATE (tweet:Tweet {
    username: row.`User Name`,
    nickname: row.Nickname,
    location: row.`Place (as appears on Bio)`,
    lat: row.Latitude,
    long: row.Longitude,
    content: row.`Tweet content`,
    mentions: extract( m in 
                filter(m in split(row.`Tweet content`," ") where m starts with "@" and size(m) > 1) 
                | right(m,size(m)-1))
               
})


```

Some of the columns in the csv file we have to use>
"User Name", "Nickname","Place (as appears on Bio)", "Latitude", "Longitude" and "Tweet content"

* Creates a number of objects labeled "Tweet" with the columns "User Name", "Nickname","Place (as appears on Bio)", "Latitude", "Longitude" and "Tweet content" renamed as something nice (single lowercase name), and which adds a list of mentions to each Tweet.

## Exercise 2
* Use the mentions list of each tweet to create a new set of nodes labeled "Tweeters", whith a "Mentions" relation.
* Create a relation "Tweeted" between Tweeters and Tweet.

It is not done!

## Exercise 3
* Find the top 10 list of tweeters whose tweets are the furtherst apart.

## Handins
Link to cypher queries and the results obtained from each query

