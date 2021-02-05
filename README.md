---
###  Inlämning 1 databaser  i Linux

- [Mariadb](https://mariadb.com/)
- [Mongodb](https://www.mongodb.com)
- [MySQL](https://www.mysql.com) 



---


#### 1: Var hittar man log-filerna för databaserna ?
Enkelt svar: /var/log \
Svårt svar: \
Var man hittar log-filer är definierat i konfigurationsfilen. 
#### MariaDB/MySQL
locate my.cnf \
/etc/alternatives/my.cnf \
/etc/mysql/my.cnf \
/etc/mysql/my.cnf.fallback \
/var/lib/dpkg/alternatives/my.cnf\
vanlig plats för konfigurationsfil är "/etc/my.cnf", /etc/mysql/mysql.cnf och (ubuntu MariaDB) /etc/mysql/mariadb.conf.d/50-server.cnf \
mysql show variables ger kanske mer svar: 
#### Default options are read from the following files in the given order: \
/etc/my.cnf /etc/mysql/my.cnf ~/.my.cnf 

#### so after examinating this system the logs are in /var/log/mysql/

#### MongoDB
Från mongo show logs show log global, show log startupWarnings
Mongo har också en konfigurationsfil etc/"mongo.conf där normalt var/log/mongodb/mongodb.log står.

#### 2: Vad är skillnaden mellan en SQL databas som MySQL / MariaDB och en NoSQL databas som MongoDB?
En stor skillnad är att StructuredQueryLanguage språket används i MySQL/MariaDB vilket det inte gör i MongoDB.
I MySQL/MariaDB måste man i förväg planera lite mer eftersom det är mindre dynamiskt med tables och rows medans man i MongoDB kan välja att skicka in värden och skapa värden direkt när man märker att dessa behövs.
MongoDB är en dokument-orienterad databas använder javascript och lagrar i Json/Bson format och varje värde har en unik nyckel. 
MySQL/MariaDB är relationsdatabaser och sparar i tabeller och använder sig av SQL språket.

NOSQL 
Dokument databaser är enklare att skriva och läsa än sql databaser.
Flexibla, skalbara och kanske mindre resurskrävande för hårdvaran.

#### 3. Vad finns det för likheter mellan databaserna?

Samtliga databaser stöder CRUD(CREATE, READ, UPDATE, DELETE).
Samtliga databaser har relationer men löser de på olika sätt.

#### 4. Fundera, när vill man välja en SQL databas, och när vill man välja en NoSQL databas?
Enkelt uttryckt för att spara strukturerad data SQL och ostrukturerad data NOSQL.

NOSQL data från användare, meddelande, enhetsdata, bilder och video.
Vid utveckling känns det som att NoSQL databaser är ett bättre alternativ eftersom det är enklare att stoppa in nya värden och nycklar utan att ha en plan för hur strukturen ser ut.

SQL till det som är strukturerat.

#### 5: Om du ger en annan part tillgång till din databas, vad är då viktigt att tänka på? (Generell fråga, du behöver inte svara för båda databaserna)
Man bör se till att RBAC(Role-based access control) är aktiverat och väldefinierat i fallet mongodb så finns ju tillgång till mycket. \
help admin \
	ls([path])                      list files
	pwd()                           returns current directory
	listFiles([path])               returns file list
	hostname()                      returns name of this host
	cat(fname)                      returns contents of text file as a string
	removeFile(f)                   delete a file or directory
	load(jsfilename)                load and execute a .js file
	run(program[, args...])         spawn a program and wait for its completion
	runProgram(program[, args...])  same as run(), above
	sleep(m)                        sleep m milliseconds
	getMemInfo()                    diagnostic


#### 6. Vad för typ av information kan vara känsligt / problematiskt att spara in en databas när det kommer till tex GDPR? 
All information som kan knytas till individer är känslig information.
exempel: adress, epostadress, namn, mac-adress, ip-adress. 

#### 7. Nämn några SQL-databaser, samt några NoSQL-databaser. 
#### SQL
MYSQL, MariaDB, Microsoft SQL Server, SQLite, Oracle Database, Amazon Relational Database Service.
#### NOSQL
MongoDB, MarkLogic, InterSystems Caché, OrientDB, Apache CouchDB, IBM Cloudant & Informix, CrateDB, Azure Cosmos DB, BaseX, CouchbaSE sERVER, ExIST db. 
#### 8. Om flera företag, organisationer eller personer använder samma databas, varför är det då viktigt att sätta upp separata användare i databasen för dessa parter? 
Om flera användare använder samma databas bör man se till att ingen som inte ska ha behörighet kan läsa, skriva, uppdatera eller ta bort en tabell/collection eller databas. Viktigt att tänka på är även dbRef eller REFERENCES och vilken övrig funktionalitet som erbjuds när man ansluter till databasen se fråga 5 mongo admin help.

#### 9. Vilka delar i CRUD påverkar / gör ändringar i databasen, och vilka delar i CRUD gör inte det?
READ gör inte ändringar således allt som är relaterat till read.

#### 10. Hur ansluter man till någon annans databas från terminalen?(Visa både på MySQL och MongoDB)
#### MySQL
#### Clientside:
mysql -u jonny -p -h 8.8.8.254 (möjligtvis ett -p efter -h om portnummer är annat än 3306)

#### serverside:
mysql 
create user 'Jonny'@'%' identified by 'bad_password'; \
GRANT ALL PRIVILEGES ON * .* TO 'Jonny'@'%'; \
FLUSH PRIVILEGES;



#### MongoDB

##### Clientside: \
mongo -u jonny -p bad_password 8.8.8.254:27017 \
mongo -u <USER> -p <PASSWORD> <HOST>:<PORT>/<DB> --authenticationDatabase <AUTH_DB> \
without authorization mongo 8.8.8.254  
from mongo db = connect("8.8.8.254:27017/imdb")

##### Serverside:
mongo
use admin
db.createUser({user:'siteUserAdmin', pwd: '<secure password #1>', roles:['userAdminAnyDatabase']})
db.auth('jonny', 'bad_password')

/etc/mongod.conf

bindIp: 0.0.0.0

#depening on mongo version only one pick wisely ;)
#auth=true 

#security:
#authorization: enabled






