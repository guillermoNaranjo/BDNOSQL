# Tarea 2 MongoDB
## Guillermo Naranjo Muedano 190240

4.Cómo podemos saber si los tuiteros hispanohablantes interactúan más en las noches?
* Consideré que la noche era de 7 a 4 am
* Noche `db.tweets.aggregate([{$lookup: {from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},
{$lookup:{from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
{$match:{"fulllocale.languages":{$regex:/Español/},created_at:{$regex:/[a-z]{3}.[a-z]{3}.[0-9]{2}.(19|20|21|22|23|24|01|02|03|04).*/i}}},
{$group:{_id:"$fulllocale.languages",count:{$sum:1}}}])`

* Día `db.tweets.aggregate([{$lookup: {from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},
{$lookup:{from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
{$match:{"fulllocale.languages":{$regex:/Español/},created_at:{$not:{$regex:/[a-z]{3}.[a-z]{3}.[0-9]{2}.(19|20|21|22|23|24|01|02|03|04).*/i}}}},
{$group:{_id:"$fulllocale.languages",count:{$sum:1}}}])`

* Hay más tuiteros de noche

* Para comprobar que la suma de día y noche me de el total de hispanohablantes `db.tweets.aggregate([
{$lookup: {from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},
{$lookup:{from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
{$group:{_id:"$fulllocale.languages",count:{$sum:1}}}])`

5. Cómo podemos saber de dónde son los tuiteros que más tiempo tienen en la plataforma?
* `db.meses.insertMany([
	{mes:"Jan",num:"01"},
	{mes:"Feb",num:"02"},
	{mes:"Mar",num:"03"},
	{mes:"Apr",num:"04"},
	{mes:"May",num:"05"},
	{mes:"Jun",num:"06"},
	{mes:"Jul",num:"07"},
	{mes:"Aug",num:"08"},
	{mes:"Sep",num:"09"},
	{mes:"Oct",num:"10"},
	{mes:"Nov",num:"11"},
	{mes:"Dec",num:"12"}
	])`

* `db.tweets.aggregate([
	{$addFields:{hora:{$regexFind:{input:"$user.created_at",regex:/[0-9]{2}:[0-9]{2}:[0-9]{2}/}},ano:{$regexFind:{input:"$user.created_at",regex:/\s[0-9]{4}/}},dia:{$regexFind:{input:"$user.created_at",regex:/\s[0-9]{2}\s/}},mes:{$regexFind:{input:"$user.created_at",regex:/\s[a-z]{3}\s/i}},plus:{$regexFind:{input:"$user.created_at",regex:/\+[0-9]{4}/}}}},
	{$addFields:{anoF:{$regexFind:{input:"$ano.match",regex:/[0-9]{4}/}},mesF:{$regexFind:{input:"$mes.match",regex:/[a-z]{3}/i}},diaF:{$regexFind:{input:"$dia.match",regex:/[0-9]{2}/}}}},
	{$lookup:{from:"meses",localField:"mesF.match",foreignField:"mes",as:"mesesJ"}},
	{$unwind:"$mesesJ"},
	{$addFields:{fechaAux:{$concat:["$anoF.match","-","$mesesJ.num","-","$diaF.match"," ","$hora.match"," ","$plus.match"]}}},
	{$addFields:{fechaIso:{$toDate:"$fechaAux"}}},{$project:{_id:1,fechaIso:1,"user.created_at":1}}]).sort({fechaIso:1})`
* Pequeña explicación: creo primero campos auxiliares donde con $addFields y $regexFind creo campos nuevos que hacen match con la regex que busco, los segundos $regexFind son para eliminar espacios, hago $lookup con mi colección de meses para transformar de String a número, por ejemplo: Apr=04, por último creo un campo de fecha auxiliar para unir todos los "pedacitos" que forman la fecha y la transformo a ISODate para ordenarlas utilizando ese campo.

6. En intervalos de 7:00:00pm a 6:59:59am y de 7:00:00am a 6:59:59pm, de qué paises la mayoría de los tuits?
* De 7:00:00pm a 6:59:59am `db.tweets.aggregate([{$lookup:{from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},{$lookup:{from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},{$match:{created_at:{$regex:/[a-z]{3}.[a-z]{3}.[0-9]{2}.(19|20|21|22|23|24|01|02|03|04|05|06).*/i}}},{$group:{_id:"$user.time_zone",count:{$sum:1}}}]).sort({"count":-1})`

* De 7:00:00am a 6:59:59pm `db.tweets.aggregate([{$lookup:{from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},{$lookup:{from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},{$match:{created_at:{$regex:/[a-z]{3}.[a-z]{3}.[0-9]{2}.(07|08|09|10|11|12|13|14|15|16|17|18).*/i}}},{$group:{_id:"$user.time_zone",count:{$sum:1}}}]).sort({"count":-1})`

7. De qué país son los tuiteros más famosos de nuestra colección?

* Tomando más famosos como "con más seguidores" `db.tweets.aggregate([{$lookup:{from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},{$lookup:{from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},{$project:{_id:1,"user.time_zone":1,"user.followers_count":1}},{$sort:{"user.followers_count":-1}}])`
