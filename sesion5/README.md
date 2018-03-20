Asignatura Cloud Computing del Máster en Ingeniería Informática. 

Departamento de Ciencias de la Computación e Inteligencia Artificial.

Universidad de Granada.

<HR>

Profesor: **Manuel J. Parra-Royón**

Email: **manuelparra@decsai.ugr.es**

Tutorías: **Viernes, de 17:30 a 18:30, despacho D31 (4ª planta) Escuela Técnica Superior de Ingenierías Informática y de Telecomunicación (ETSIIT).**

Material de prácticas de la asignatura: **https://github.com/manuparra/PracticasCC**

<HR>

# Sesión : Introducción a MongoDB

Tabla de contenido:

- [Usando MongoDB](#usando-mongo)
  * [Documentos en lugar de filas/columnas](#documentos-en-lugar-de-filas-columnas)
    + [Tipos de datos](#tipos-de-datos)
- [Comenzando con MongoDB](#comenzando-con-mongodb)
  * [Conectando a mongodb:](#conectando-a-mongodb-)
  * [Selección/creación/eliminación de la base de datos](#selecci-n-creaci-n-eliminaci-n-de-la-base-de-datos)
  * [Crear una colección](#crear-una-colecci-n)
  * [Borrar las colecciones](#borrar-las-colecciones)
  * [Trabajar con documentos sobre las colecciones](#trabajar-con-documentos-sobre-las-colecciones)
  * [Seleccionando, Consultando y Filtrando](#seleccionando--consultando-y-filtrando)
  * [Actualizando documentos](#actualizando-documentos)
  * [Borrando documentos](#borrando-documentos)
  * [Importar datos externos](#importar-datos-externos)
  * [Clients MongoDB](#clients-mongodb)
- [References](#references)

# Usando MongoDB

MongoDB es una base de datos de código abierto desarrollada por MongoDB, Inc. 

MongoDB almacena datos en documentos similares a JSON que pueden variar en estructura. La información relacionada se almacena junta para un acceso rápido a la consulta a través del lenguaje de consulta MongoDB. MongoDB utiliza esquemas dinámicos, lo que significa que puede crear registros **sin definir primero la estructura**, como los campos o los tipos de sus valores. Puede cambiar la estructura de los registros (a los que llamamos documentos) simplemente añadiendo nuevos campos o borrando los existentes. Este modelo de datos le da la capacidad de representar relaciones jerárquicas, almacenar matrices y otras estructuras más complejas fácilmente. No es necesario que los documentos de una colección tengan un conjunto idéntico de campos y es frecuente la desnormalización de los datos. MongoDB también fue diseñado con alta disponibilidad y escalabilidad en mente, e incluye replicación lista para usar y auto-sharding.


**MongoDB principales características:**

* Almacenamiento orientado a documentos - Los datos se almacenan en forma de documentos de estilo JSON.
* Índice sobre cualquier atributo
* Replicación y alta disponibilidad
* Auto-sharding
* Consultas ricas

**Usando Mongo:**

* Big Data
* Gestión y entrega de contenidos
* Infraestructura Móvil y Social
* Gestión de datos de usuario
* Hub de datos

**Comparado con MySQL:**

Muchos conceptos en MySQL tienen análogos cercanos en MongoDB. Algunos de los conceptos comunes en cada sistema:

* MySQL -> MongoDB
* Database -> Database
* Table -> Collection
* Row -> Document
* Column -> Field
* Joins -> Embedded documents, linking

**Lenguaje de consulta:**

Desde MySQL:

```
INSERT INTO users (user_id, age, status)
VALUES ('bcd001', 45, 'A');
```

A MongoDB:

```
db.users.insert({
  user_id: 'bcd001',
  age: 45,
  status: 'A'
});
```

Desde MySQL:

```
SELECT * FROM users
```

A MongoDB:

```
db.users.find()
```


Desde MySQL:

```
UPDATE users SET status = 'C'
WHERE age > 25
```

A MongoDB:

```
db.users.update(
  { age: { $gt: 25 } },
  { $set: { status: 'C' } },
  { multi: true }
)
```


## Documentos en lugar de filas/columnas

MongoDB almacena registros de datos como documentos BSON. 

BSON es una representación binaria de documentos JSON, contiene más tipos de datos que JSON.

![bsonchema](https://docs.mongodb.com/manual/_images/crud-annotated-document.png)

Los documentos MongoDB se componen de pares de campo y valor y tienen la siguiente estructura:


```
{
   field1: value1,
   field2: value2,
   field3: value3,
   ...
   fieldN: valueN
}
```

Ejemplo de documento:

```
var mydoc = {
               _id: ObjectId("5099803df3f4948bd2f98391"),
               name: 
               		{ 
               		 first: "Alan", 
               		 last: "Turing" 
               		},
               birth: new Date('Jun 23, 1912'),
               death: new Date('Jun 07, 1954'),
               contribs: [ 
               				"Turing machine", 
               				"Turing test", 
               				"Turingery" ],
               views : NumberLong(1250000)
            }
```

Para especificar o acceder a un campo de un documento: utilice la notación por puntos

```
mydoc.name.first
```

Los documentos permiten documentos incrustados documentos incrustados documentos incrustados ....:

```
{
   ...
   name: { first: "Alan", last: "Turing" },
   contact: { 
   			phone: { 
   					model: { 
   						brand: "LG", 
   						screen: {'maxres': "1200x800"} 
   					},
   					type: "cell", 
   					number: "111-222-3333" } },
   ...
}
```

El tamaño máximo de los documentos BSON es de **16 megabytes!**.


### Tipos de datos

* String − This is the most commonly used datatype to store the data.
* Integer − This type is used to store a numerical value.
* Boolean − This type is used to store a boolean (true/ false) value.
* Double − This type is used to store floating point values.
* Min/ Max keys − This type is used to compare a value against the lowest and highest BSON elements.
* Arrays − This type is used to store arrays or list or multiple values into one key.
* Timestamp − ctimestamp. This can be handy for recording when a document has been modified or added.
* Object − This datatype is used for embedded documents.
* Null − This type is used to store a Null value.
* Symbol − This datatype is used identically to a string; however, it's generally reserved for languages that use a specific symbol type.
* Date − This datatype is used to store the current date or time in UNIX time format. You can specify your own date time by creating object of Date and passing day, month, year into it.
* Object ID − This datatype is used to store the document’s ID.
* Binary data − This datatype is used to store binary data.
* Code − This datatype is used to store JavaScript code into the document.
* Regular expression − This datatype is used to store regular expression.


# Comenzando con MongoDB

Conectate a ATCSTACK:

```
ssh manuparra@.........es
```

En primer lugar, compruebe que tiene acceso al sistema de herramientas mongo, pruebe este comando:

```
mongo + tab
```

mostrará:

```
mongo         mongodump     mongoexport   mongofiles    
mongoimport   mongooplog    mongoperf     mongorestore  mongostat     mongotop 
```

## Conectando a mongodb:

El puerto por defecto para instancias de mongodb y mongos es 27017. 
Puede cambiar este puerto con port o --port cuando se conecte.

Escribe:

```
mongo
```

Conectará con los parámetros por defecto: ```localhost```, puerto: ```27017``` y base de datos: ``test``.


```
MongoDB shell version: 2.6.12
connecting to: test
>
```

Para salir usa ``CTRL+C`` o ``exit``

Cada usuario tiene una cuenta en el servicio mongodb. Para conectar:

```
mongo localhost:27017/manuparra -p 
```

Preguntará el password ``password``. 

```
mongo localhost:27017/manuparra -p mipasss 
```


El servicio MongoDB se está ejecutando localmente en sistemas Docker, por lo tanto, si se conecta desde contenedores Docker o Máquinas Virtuales, debe utilizar el sistema local Docker IP:

```
mongo 192.168.10.30:27017/manuparra -p mipasss 
```

## Selección/creación/eliminación de la base de datos

El comando creará una nueva base de datos si no existe, de lo contrario devolverá la base de datos existente.

```
> use manuparra:
```

Ahora estás usando la base de datos ``manuparra`` .

Si quieres saber qué base de datos estás usando:

```
> db
```

El comando ```command db.dropDatabase()`` es usado para borrar la base de datos existente.

NO USES ESTE COMANDO:

```
db.dropDatabase()
```

Para conocer el tamaño de las bases de datos:

```
show dbs
```

## Crear una colección

La sintaxis básica del comando createCollection() es la siguiente:

```
db.createCollection(name, options)
```

donde ``options`` es Opcional y especifica opciones sobre el tamaño de la memoria y la indexación.

Recuerde que en primer lugar el mongodb necesita saber cuál es la Base de Datos donde creará la Colección. Use ``show dbs`` y luego ``use <su base de datos>``.

```
use manuparra;
```

Y luego creamos la colección:

```
db.createCollection("MyFirstCollection")
```

Ahora comprobamos que está disponible:

```
show collections
```

En MongoDB, no es necesario crear la colección. MongoDB crea la colección automáticamente, cuando usted inserta algún documento:

```
db.MySecondCollection.insert({"name" : "Manuel Parra"})
```

Ya tienes las nuevas colecciones creadas:

```
show collections
```

## Borrar las colecciones

Para borrar las colecciones desde la base de datos:

```
db.MySecondCollection.drop();

```


## Trabajar con documentos sobre las colecciones

Para insertar datos en la colección de MongoDB, necesita usar el método ``insert()`` o ``save()`` de MongoDB.

```
> db.MyFirstCollection.insert(<document>);
```

Ejemplo de documento: place 

```
{    
     "bounding_box":
    {
        "coordinates":
        [[
                [-77.119759,38.791645],
                [-76.909393,38.791645],
                [-76.909393,38.995548],
                [-77.119759,38.995548]
        ]],
        "type":"Polygon"
    },
     "country":"United States",
     "country_code":"US",
     "likes":2392842343,
     "full_name":"Washington, DC",
     "id":"01fbe706f872cb32",
     "name":"Washington",
     "place_type":"city",
     "url": "http://api.twitter.com/1/geo/id/01fbe706f872cb32.json"
}
```

Para insertar:

```
db.MyFirstCollection.insert(
{    
     "bounding_box":
      {
        "coordinates":
        [[
                [-77.119759,38.791645],
                [-76.909393,38.791645],
                [-76.909393,38.995548],
                [-77.119759,38.995548]
        ]],
        "type":"Polygon"
      },
     "country":"United States",
     "country_code":"US",
     "likes":2392842343,
     "full_name":"Washington, DC",
     "id":"01fbe706f872cb32",
     "name":"Washington",
     "place_type":"city",
     "url": "http://api.twitter.com/1/geo/id/01fbe706f872cb32.json"
}
);
```

Comprobar si el documento está almacenado:

```
> db.MyFirstCollection.find();
```

Añade multiple documentos:

```
	var places= [
		{    
	     "bounding_box":
	      {
	        "coordinates":
	        [[
	                [-77.119759,38.791645],
	                [-76.909393,38.791645],
	                [-76.909393,38.995548],
	                [-77.119759,38.995548]
	        ]],
	        "type":"Polygon"
	      },
	     "country":"United States",
	     "country_code":"US",
	     "likes":2392842343,
	     "full_name":"Washington, DC",
	     "id":"01fbe706f872cb32",
	     "name":"Washington",
	     "place_type":"city",
	     "url": "http://api.twitter.com/1/geo/id/01fbe706f872cb32.json"
	},
	{    
	     "bounding_box":
	      {
	        "coordinates":
	        [[
	                [-7.119759,33.791645],
	                [-7.909393,34.791645],
	                [-7.909393,32.995548],
	                [-7.119759,34.995548]
	        ]],
	        "type":"Polygon"
	      },
	     "country":"Spain",
	     "country_code":"US",
	     "likes":2334244,
	     "full_name":"Madrid",
	     "id":"01fbe706f872cb32",
	     "name":"Madrid",
	     "place_type":"city",
	     "url": "http://api.twitter.com/1/geo/id/01fbe706f87333e.json"
	}
	]
```

y:

```
db.MyFirstCollection.insert(places)
```


En el documento insertado, si no especificamos el parámetro ``_id``, entonces MongoDB asigna un ObjectId único para este documento.
Puede anular el valor ``_id``, usando su propio ``_id``.

Dos métodos para guardar/insertar:


```
db.MyFirstCollection.save({username:"myuser",password:"mypasswd"})
db.MyFirstCollection.insert({username:"myuser",password:"mypasswd"})
```

Diferencias:

> Si un documento no existe con el valor ```_id``` especificado, el método ``save()`` realiza una inserción con los campos especificados en el documento.

> Si existe un documento con el valor ``_id`` especificado, el método ``save()`` realiza una actualización, sustituyendo todos los campos del registro existente por los campos del documento.


## Seleccionando, Consultando y Filtrando

Muestra todos los documentos en ``MyFirstCollection``:

```
> db.MyFirstCollection.find();
```

Sólo un documento:

```
> db.MyFirstCollection.findOne();
```

Cuenta los documentos, añade  ``.count()`` al comando:

```
> db.MyFirstCollection.find().count();
```


Muestra los documentos en un formato bonito:

```
> db.MyFirstCollection.find().pretty()
```

Selecciona o busca mediante campos, por ejemplo como ``bounding_box.type``:

```
...
 "bounding_box":
    {
        "coordinates":
        [[
                [-77.119759,38.791645],
                [-76.909393,38.791645],
                [-76.909393,38.995548],
                [-77.119759,38.995548]
        ]],
        "type":"Polygon"
    },
...
```


```
> db.MyFirstCollection.find("bounding_box.type":"Polygon")
```


Filtrado:

Igualdad	``{<key>:<value>}``	 ``db.MyFirstCollection.find({"country":"Spain"}).pretty()``

Menor que 	``{<key>:{$lt:<value>}}``	``db.mycol.find({"likes":{$lt:50}}).pretty()``

Menor o igual	``{<key>:{$lte:<value>}}``	``db.mycol.find({"likes":{$lte:50}}).pretty()``

Mayor que	``{<key>:{$gt:<value>}}``	``db.mycol.find({"likes":{$gt:50}}).pretty()``

Más o igual que : ``gte`` Greater than equal, ``ne`` Not equal, etc. 

Y:

```
> db.MyFirstCollection.find(
   {
      $and: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

O:
> db.MyFirstCollection.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()

Mezclando todo:

```
db.MyFirstCollection.find(
		{"likes": {$gt:10}, 
		 $or: 
			[
			 {"by": "..."},
   			 {"title": "..."}
   			]
   		}).pretty()
```


Usando expresiones regulares en los campos, por ejemplo para buscar documentos donde el nombre  ``name`` contenga ``Wash``.


```
db.MyFirstCollection.find({"name": /.*Wash.*/})

```


## Actualizando documentos

Sintaxis:

```
> db.MyFirstCollection.update(<selection criteria>, <data to update>)
```

Ejemplo:

```
db.MyFirstCollection.update(
	 { 'place_type':'area'},
	 { $set: {'title':'New MongoDB Tutorial'}},
	 {multi:true}
	);
```

IMPORTANTE: use ``multi:true`` para actualizar todas las coincidencias.


## Borrando documentos

MongoDB tiene el método  ``remove()`` para borrar un documento de una colección. ``remove()``. El método acepta dos parametros. Uno, el método de borrado y el otro el la opción justOne.

```
> db.MyFirstCollection.remove(<criteria>)
```

Ejemplo:

```
db.MyFirstCollection.remove({'country':'United States'})
```


## Importar datos externos

Descargue este conjunto de datos en tu HOME (copie este enlace: http://samplecsvs.s3.amazonaws.com/SacramentocrimeJanuary2006.csv):

[DataSet](http://samplecsvs.s3.amazonaws.com/SacramentocrimeJanuary2006.csv) 7585 rows and 794 KB)

Usa el comando siguiente:

```
curl -O http://samplecsvs.s3.amazonaws.com/SacramentocrimeJanuary2006.csv
```

o descargalo desde [github](./datasetmongodb/SacramentocrimeJanuary2006.csv).

Para importar el fichero:

```
mongoimport -d manuparra -c <your collection> --type csv --file /tmp/SacramentocrimeJanuary2006.csv --headerline
```

Prueba las siguientes consultas a la colección:

- Cuenta el número de delitos/robos.
- Cuenta el número de delitos por hora.


## Clients MongoDB

- Command line tools: https://github.com/mongodb/mongo-tools
- Use Mongo from PHP: https://github.com/mongodb/mongo-php-library
- Use Mongo from NodeJS: https://mongodb.github.io/node-mongodb-native/
- Perl to MongoDB: https://docs.mongodb.com/ecosystem/drivers/perl/
- Full list of Mongo Clients (all languages): https://docs.mongodb.com/ecosystem/drivers/#drivers


# References 

- Getting Started with MongoDB (MongoDB Shell Edition): https://docs.mongodb.com/getting-started/shell/
- MongoDB Tutorial: https://www.tutorialspoint.com/mongodb/
- MongoDB Tutorial for Beginners: https://www.youtube.com/watch?v=W-WihPoEbR4
- Mongo Shell Quick Reference: https://docs.mongodb.com/v3.2/reference/mongo-shell/