MongoDB Basics:
===========================

mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  <== For BSON export

mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json  <== For JSON export

mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump  <== For BSON import

mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json  <== For JSON import

DB: 
> use sample_training;
switched to db sample_training

<br/>Updating Documents  - mongo shell

<br/>1. Find all documents in the zips collection where the zip field is equal to
   12434.
<br/>Answer: db.zips.find({"zip":"12434"})
<br/>2. Find all documents in the zips collection where the city field is equal to
   “HUDSON".
<br/>Answer: db.zips.find({"city":"HUDSON"})
<br/>3. Find how many documents in the zips collection have the city field is equal
   to “HUDSON".
<br/>Answer: db.zips.find({"city":"HUDSON"}).count()
<br/>4. Update all documents in the zips collection where the city field is equal
   to "HUDSON" by adding 10 to the current value of the "pop" field.
<br/>Answer: db.zips.updateMany({"city":"HUDSON"},{$inc:{"pop":10}})
<br/>5. Update a single document in the zips collection where the zip field is
   equal to 12534 by setting the value of the "pop" field to 17630.
<br/>Answer: db.zips.update({"zip":"12534"},{"$set":{"pop":12534}})
<br/>6. Update a single document in the zips collection where the zip field is
   equal to 12534 by setting the value of the "population" field to 17630.
<br/>Answer: db.zips.update({"zip":"12534"},{"$set":{"population":17630}})
<br/>7. Find all documents in the grades collection where the student_id is 151,
   and the class_id field is 339.
<br/>Answer: db.grades.find({"student_id":151, "class_id":339})
<br/>8. Find all documents in the grades collection where the student_id is 250,
   and the class_id field is 339.
<br/>Answer: db.grades.find({"student_id":250, "class_id":339})
<br/>9. Update one document in the grades collection where the student_id is 250,
   and the class_id field is 339, by adding a document element to the "scores"
   array.
<br/>Answer: db.grades.updateOne({"student_id":250, "class_id":339},{"$push":{"scores":{"type":"classwork","score":89}}})


Question:
How many documents in the sample_training.zips collection have fewer than 1000 people listed in the pop field?

Answer:
db.zips.find({"pop":{"$lt":1000}}).count();

Question:
Using the sample_training.routes collection find out which of the following statements will return all routes that have at least one stop in them?

Answer:
db.routes.find({ "stops": { "$gt": 0 }}).pretty()
db.routes.find({ "stops": { "$ne": 0 }}).pretty()



Question:
Which is the most succinct query to return all documents from the sample_training.inspections collection where the inspection date is either "Feb 20 2015", or "Feb 21 2015" and the company is not part of the "Cigarette Retail Dealer - 127" sector?

Answer:
db.inspections.find(
  { "$or": [ { "date": "Feb 20 2015" },
             { "date": "Feb 21 2015" } ],
    "sector": { "$ne": "Cigarette Retail Dealer - 127" }}).pretty()


Question:
How many businesses in the sample_training.inspections dataset have the inspection result "Out of Business" and belong to the "Home Improvement Contractor - 100" sector?

Answer:
db.inspections.find({"result":"Out of Business","sector":"Home Improvement Contractor - 100"}).count();

	
Question:
</br>People often confuse NEW YORK City as the capital of New York state, when in reality the capital of New York state is ALBANY.

</br>In the sample_training.zips collection add a boolean field "capital?" to all documents pertaining to ALBANY NY, and NEW YORK, NY. The value of the field should be true for all ALBANY documents and false for all NEW YORK documents.

<br/> Answer 1:
<br/> db.zips.updateMany({"state": "NY", "city":"ALBANY"},{"$set":{"capital?":true}})
<br/> db.zips.updateMany({"state": "NY", "city":"NEW YORK"},{"$set":{"capital?":false}})
	

<br/>Question:
How many zips in the sample_training.zips dataset are neither over-populated nor under-populated?

In this case, we consider population of more than 1,000,000 to be over- populated and less than 5,000 to be under-populated.

Answer:
db.zips.find({"pop":{"$lt":1000000,"$gt":5000}}).count();

Question:
How many companies in the sample_training.companies dataset were
either founded in 2004
	•	[and] either have the social category_code [or] web category_code,
[or] were founded in the month of October
	•	[and] also either have the social category_code [or] web category_code?
Copy/paste the exact numeric value (without double quotes) of the result that you get into the response field.
Answer:
db.companies.find({"$and":
			[
				{"$or":[{"founded_year":{"$eq":2004}},{"founded_month":{"$eq":10}}]},
				{"$or":[{"category_code":{"$eq":"web"}},{"category_code":{"$eq":"social"}}]}
			]
		})





MQL Syntax: { <field> : { <operator> : <value> } }
Aggregation Syntax: { <operator> : { <field>, <value> } }

Question:
What is the name of the listing in the sample_airbnb.listingsAndReviews dataset that accommodates more than 6 people and has exactly 50 reviews?

Answer:
db.listingsAndReviews.find({"accommodates":{"$gt":6},"reviews":{"$size":50}}).pretty()

Question:
Using the sample_airbnb.listingsAndReviews collection find out how many documents have the "property_type" "House", and include "Changing table" as one of the "amenities"?

Answer:
db.listingsAndReviews.find({"property_type":"House","amenities":{"$all":["Changing table"]}}).count()

Question:
Which of the following queries will return all listings that have "Free parking on premises", "Air conditioning", and "Wifi" as part of their amenities, and have at least 2 bedrooms in the sample_airbnb.listingsAndReviews collection?

Answer:
db.listingsAndReviews.find(
  { "amenities":
      { "$all": [ "Free parking on premises", "Wifi", "Air
        conditioning" ] }, "bedrooms": { "$gte":  2 } } ).pretty()

Question:
Find all documents with exactly 20 amenities which include all the amenities listed in the query array, and display their price and address:

Answer:
db.listingsAndReviews.find({ "amenities":
        { "$size": 20, "$all": [ "Internet", "Wifi",  "Kitchen", "Heating",
                                 "Family/kid friendly", "Washer", "Dryer",
                                 "Essentials", "Shampoo", "Hangers",
                                 "Hair dryer", "Iron",
                                 "Laptop friendly workspace" ] } },
                            {"price": 1, "address": 1}).pretty()

Question:
Find all documents that have Wifi as one of the amenities only include price and address in the resulting cursor:

Answer:
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()

Question:
Find all documents that have Wifi as one of the amenities only include price and address in the resulting cursor, also exclude ``"maximum_nights"``. **This will be an error:*

Answer:
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1,
                             "_id": 0, "maximum_nights":0 }).pretty()


Question:
Find all documents where the student in class 431 received a grade higher than 85 for any type of assignment:

Answer:
db.grades.find({ "class_id": 431 },
               { "scores": { "$elemMatch": { "score": { "$gt": 85 } } }
             }).pretty()


Question:
Find all documents where the student had an extra credit score:

Answer:
db.grades.find({ "scores": { "$elemMatch": { "type": "extra credit" } }
               }).pretty()


Question:
How many companies in the sample_training.companies collection have offices in the city of Seattle?

Answer:
> db.companies.find({"offices":{"$elemMatch":{"city":"Seattle"}}}).count()
117
> db.companies.find({"offices":{"$elemMatch":{"city":{"$eq":"Seattle"}}}}).count()
117

Question:
Which of the following queries will return only the names of companies from the sample_training.companies collection that had exactly 8 funding rounds?

Answer:
db.companies.find({ "funding_rounds": { "$size": 8 } },{ "name": 1, "_id": 0 })

Some Examples:
--------------
use sample_training

db.trips.findOne({ "start station location.type": "Point" })

db.companies.find({ "relationships.0.person.last_name": "Zuckerberg" },{ "name": 1 }).pretty()

db.companies.find({ "relationships.0.person.first_name": "Mark","relationships.0.title": { "$regex": "CEO" } },{ "name": 1 }).count()

db.companies.find({ "relationships.0.person.first_name": "Mark","relationships.0.title": {"$regex": "CEO" } },{ "name": 1 }).pretty()

db.companies.find({ "relationships":{ "$elemMatch": { "is_past": true,"person.first_name": "Mark" } } },{ "name": 1 }).pretty()

db.companies.find({ "relationships":{ "$elemMatch": { "is_past": true,"person.first_name": "Mark" } } },{ "name": 1 }).count()


Question:
How many trips in the sample_training.trips collection started at stations that are to the west of the -74 longitude coordinate?

Longitude decreases in value as you move west.

Note: We always list the longitude first and then latitude in the coordinate pairs; i.e.

<field_name>: [ <longitude>, <latitude> ]

Answer:
> db.trips.find({"start station location.coordinates.0":{"$lt":-74}}).count()
1928

Question:
How many inspections from the sample_training.inspections collection were conducted in the city of NEW YORK?

Answer:
> db.inspections.find({"address.city":"NEW YORK"}).count()
18279

Question:
Which of the following queries will return the names and addresses of all listings from the sample_airbnb.listingsAndReviews collection where the first amenity in the list is "Internet"?

Answer:
db.listingsAndReviews.find({ "amenities.0": "Internet" },
                           { "name": 1, "address": 1 }).pretty()

Advanced CRUD Operations Exercise Questions:
--------------------------------------------------
Query Operators - Comparison
1. Find all documents where the trip was less than or equal to 70 seconds and the usertype was not "Subscriber"
=> db.trips.find({"tripduration":{"$lte":70},"usertype":{"$ne":"Subscriber"}})

2. Find all documents where the trip was less than or equal to 70 seconds and the usertype was "Customer" using a redundant equality operator.
=> db.trips.find({"tripduration":{"$lte":70},"usertype":{"$eq":"Customer"}})

3. Find all documents where the trip was less than or equal to 70 seconds and the usertype was "Customer" using the implicit equality operator.
=> db.trips.find({"tripduration":{"$lte":70},"usertype":"Customer"})

Query Operators - Logic
1. Find all documents where airplanes CR2 or A81 left or landed in the KZN airport.
=> db.routes.find({"$and":[{"$or":[{"airplane" : "CR2"}, {"airplane" : "A81"}]},{"$or":[{"src_airport" : "KZN"},{"dst_airport" : "KZN"}]}]})

Expressive Query Operator
1. Find all documents where the trip started and ended at the same station.
=> db.trips.find({"$expr":{"$eq":["$start station id","$end station id"]}}).count()

2. Find all documents where the trip lasted longer than 1200 seconds, and started and ended at the same station.
=> db.trips.find({"$expr":{"$and":[{"$gt":["$tripduration", 1200]},{"$eq":["$start station id","$end station id"]}]}}).count()

Array Operators
1. Find all documents that contain more than one amenity without caring about the order of array elements.
=> db.listingsAndReviews.find({"$expr":{"$gte":["$amenities",1]}}).count()
2. Only return documents that list exactly 20 amenities in this field and contain the amenities of your choosing.
=> db.listingsAndReviews.find({"amenities":{"$size":20}}).count()

Array Operators and Projection
1. Find all documents in the sample_airbnb database with exactly 20 amenities which include all the amenities listed in the query array, and display their price and address.
=> db.listingsAndReviews.find({"amenities":{"$size":20}},{"price":1,"address":1}).pretty()
2. Find all documents in the sample_airbnb database that have Wifi as one of the amenities only include price and address in the resulting cursor.
=> db.listingsAndReviews.find({"amenities":{"$all":["Wifi"]}},{"price":1,"address":1}).pretty()
3. Find all documents in the sample_airbnb database that have Wifi as one of the amenities only include price and address in the resulting cursor, also exclude "maximum_nights". Was this operation successful? Why?
=> db.listingsAndReviews.find({"amenities":{"$all":["Wifi"]}},{"price":1,"address":1}).pretty()
=> db.listingsAndReviews.find({"amenities":"Wifi"},{"price":1,"address":1}).count()
4. Find all documents in the grades collection where the student in class 431 received a grade higher than 85 for any type of assignment.
=> db.grades.find({"class_id":431,"scores":{"$elemMatch":{"score":{"$gt":85}}}}).pretty()
5. Find all documents in the grades collection where the student had an extra credit score.
=> db.grades.find({"scores":{"$elemMatch":{"type":"extra credit"}}}).pretty()

Array Operators and Sub-Documents

1. Find any document from the companies collection where the last name Zuckerberg in the first element of the relationships array.
=> db.companies.find({"relationships.0.person.last_name":"Zuckerberg"}).pretty()
2. Find how many documents from the companies collection have CEOs who's first name is Mark and who are listed as the first relationship in this array for their company entry.
=> db.companies.find({ "relationships.0.person.first_name": "Mark", "relationships.0.title": { "$regex": "CEO" } }, { "name": 1 }).count()
3. Find all documents in the companies collection where people named Mark used to be in the senior company leadership array, a.k.a the relationships array, but are no longer with the company.
=> db.companies.find({ "relationships": { "$elemMatch": { "is_past": true, "person.first_name": "Mark" } } }, { "name": 1 }).pretty()


Aggregation Framework
----------------------------
-->> With MongoDB Query Language (MQL), we can filter & update data.
-->> With Aggregation Framework, we can group, compute & reshape data.


-->> cursor methods: sort(), limit(), pretty(), count()

1. Using the aggregation framework find all documents that have Wifi as one of the amenities. Only include price and address in the resulting cursor.
=> db.listingsAndReviews.aggregate([{$match:{"amenities":"Wifi"}},{$project:{"price":1,"address":1}}]).pretty()
=> db.listingsAndReviews.aggregate([{$match:{"amenities":"Wifi"}},{$project:{"price":1,"address":1}}]).count()
uncaught exception: TypeError: db.listingsAndReviews.aggregate(...).count is not a function :
2. Which countries have listings in the sample_airbnb database?
=> db.listingsAndReviews.aggregate([{$project:{"address":1,"_id":0}},{$group:{"_id":"$address.country"}}]);
3. How many countries have listings in the sample_airbnb database?
=> db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 } } }
                                	])
4. What room types are present in the sample_airbnb.listingsAndReviews collection?
=> db.listingsAndReviews.aggregate([{$project:{"room_type":1,_id:0}},{$group:{"_id":"$room_type"}}]);
5. What are the differences between using aggregate() and find()?
=> aggregate() allows us to compute and reshape data in the cursor.
=> aggregate() can do what find() can and more.
6. In what year was the youngest bike rider from the sample_training.trips collection born?
=> db.trips.aggregate([{$project:{"birth year" : 1, "_id":0}},{$group:{"_id":"$birth year"}}]); //1999
	#This is not optimal solution. We have to look manually for lowest year from the values returned.
=> db.trips.find({"birth year":{"$gt":0}},{"birth year":1}).sort({"birth year":-1}).limit(1) //1999


1. Find the least populated ZIP code in the zips collection.
=> db.zips.find().sort({"pop":1}).limit(1).pretty()
2. Find the most populated ZIP code in the zips collection.
=> db.zips.find().sort({"pop":-1}).limit(1).pretty()
3. Find the top ten most populated ZIP codes.
=> db.zips.find().sort({"pop":-1}).limit(10).pretty()
4. Get results sorted in increasing order by population, and decreasing order by city name.
=> db.zips.find().sort({"pop":1, "city":-1}).pretty()


** MongoDB Assumes that you have performed sort and then limit. So, with that said, both the below queries works same:
	db.zips.find().sort({"pop":1, "city":-1}).limit(2).pretty()
	db.zips.find().limit(2).sort({"pop":1, "city":-1}).pretty()

QUIZ:
-------
1. Which of the following commands will return the name and founding year for the 5 oldest companies in the sample_training.companies collection?
=> db.companies.find({"founded_year":{"$gt":0}},{"name":1,"founded_year":1,"_id":0}).sort({"founded_year":1}).limit(5).pretty()
=> db.companies.find({"founded_year":{"$ne":null}},{"name":1,"founded_year":1}).limit(5).sort({"founded_year":1})
=> db.companies.find({"founded_year":{"$ne":null}},{"name":1,"founded_year":1 }).sort({"founded_year":1}).limit(5)


Introduction to Indexes
-----------------------------

use sample_training
db.trips.find({ "birth year": 1989 })
db.trips.find({ "start station id": 476 }).sort( { "birth year": 1 } )
db.trips.createIndex({ "birth year": 1 })
db.trips.createIndex({ "start station id": 1, "birth year": 1 })


1. Create two separate indexes to support the following queries:
	db.trips.find({"birth year": 1989})
Ans: db.trips.createIndex({"birth year":1});
	db.trips.find({"start station id": 476}).sort({"birth year": 1})
Ans: db.trips.createIndex({"birth year":1, "start station id":1});
2. Jameela often queries the sample_training.routes collection by the src_airport field like this:
	db.routes.find({ "src_airport": "MUC" }).pretty()
   Which command will create an index that will support this query?
Ans: db.routes.createIndex({ "src_airport": -1 })

Upsert example:
---------------
db.iot.updateOne({ "sensor": r.sensor, "date": r.date,
                   "valcount": { "$lt": 48 } },
                         { "$push": { "readings": { "v": r.value, "t": r.time } },
                        "$inc": { "valcount": 1, "total": r.value } },
                 { "upsert": true })

