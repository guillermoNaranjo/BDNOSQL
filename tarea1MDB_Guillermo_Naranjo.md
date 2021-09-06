# Tarea 1
#### Nombre: Guillermo Naranjo
#### CU:190240

12. `db.restaurants.find({"address.coord.0":{$lt:-65.754168},"cuisine":{$not:/American/},"grades.score":{$gt:70}})`
13. `db.restaurants.find({cuisine:{$not:/[Aa]merican/},borough:{$not:/Brooklyn/},"grades.grade":"A"}).sort({cuisine:-1})`
14. `db.restaurants.find({name:/^Wil/},{name:1,borough:1,cuisine:1,restaurant_id:1})`
15. `db.restaurants.find({name:/ces$/},{name:1,borough:1,cuisine:1,restaurant_id:1})`
16. `db.restaurants.find({name:{$regex:/.*reg.*/,$options: 'i'}},{restaurant_id:1,name:1,borough:1,cuisine:1})`
17. `reviews> db.restaurants.find({borough:"Bronx",cuisine:/^American |^Chinese$/})`
18. `db.restaurants.find({borough:/(Staten Island|Queens|Bronx|Brooklyn)/},{restaurant_id:1,name:1,borough:1,cuisine:1})`
19. `db.restaurants.find({borough:{$nin:["Staten Island","Queens","Bronx","Brooklyn"]}},{restaurant_id:1,name:1,borough:1,cuisine:1})`
20. `db.restaurants.find({"grades.score":{$lte:10}},{restaurant_id:1, name:1, borough:1, cuisne:1,"grades.score":1})`
21. `db.restaurants.find({$or: [{cuisine:{$not:/^(American|Chinese)$/}},{name:/^Wil/}]},{cuisine:1,name:1,restaurant_id:1,borough:1})`
22. `db.restaurants.find({grades:{$elemMatch:{grade:"A",score:11,date:ISODate("2014-08-11T00:00:00.000Z")}}},{restaurant_id:1,name:1,grades:1})`
23. `db.restaurants.find({"grades.1.grade":"A","grades.1.score":9,"grades.1.date":ISODate("2014-08-11T00:00:00.000Z")},{restaurant_id:1,name:1,"grades":1})`
24. `db.restaurants.find({$and:[{"address.coord.1":{$gt:42}},{"address.coord.1":{$lte:52}}]},{restaurant_id:1,name:1,"address.street":1,"address.coord":1})`
25. `db.restaurants.find().sort({name:1})`
26. `db.restaurants.find().sort({name:-1})`
27. `db.restaurants.find().sort({cuisine:1,borough:-1}`
28. `db.restaurants.find({"address.street":{$exists:true}})`
29. `db.restaurants.find({"address.coord":{$type:["double"]}})`
30. `db.restaurants.find({"grades.score":{$mod:[7,0]}},{restaurant_id:1,name:1,"grades.score":1})`
31. `db.restaurants.find({name:{$regex:/.*mon.*/,$options: 'i'}},{name:1,borough:1,cuisine:1,"address.coord":1})`
32. `db.restaurants.find({name:/^Mad/},{name:1,borough:1,"address.coord":1,})`
# BDNOSQL
