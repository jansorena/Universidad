# Docker
## Practica 1

```bash
docker run -it --name contenedor1 ubuntu
docker run -it --name contenedor1 ubuntu bash
```
https://hub.docker.com/

> [!NOTA] 
> versiones Alpine son versiones reducidas
## Practica 2
```python
import hashlib
import datetime
from flask import Flask, request, jsonify
from flask_jwt_extended import JWTManager, create_access_token, jwt_required, get_jwt_identity
from pymongo import MongoClient
from pymongo.server_api import ServerApi
from bson.objectid import ObjectId

app = Flask(__name__)
jwt = JWTManager(app)

app.config['JWT_SECRET_KEY']= "ALSKJDLASKJDLASKJDLKASJDLKASJDL"
app.config['JWT_ACCESS_TOKEN_EXPIRES']=datetime.timedelta(days=1)

uri = "mongodb+srv://jansorena:uOZ34cmhFKKn1uJ1@clusterudec.zkpjzmr.mongodb.net/?retryWrites=true&w=majority&appName=ClusterUdec"
client = MongoClient(uri, server_api=ServerApi('1'))
db = client["demoUdec"]
user_collection =db["users"]

@app.route("/api/v1/users", methods=["POST"]) # api siempre con versiones

def create_user():
	new_user= request.get_json()
	new_user["password"] = hashlib.sha256(new_user["password"].encode("utf-8")).hexdigest()
	doc = user_collection.find_one({"username" : new_user["username"]})
	if not doc:
		user_collection.insert_one(new_user)
		return jsonify({"status" : "Usuario creado con exito"})
	else:
		return jsonify({"status" : "Usuario ya existe"})

@app.route("/api/v1/login", methods=["POST"])

def login():
	login_details = request.get_json()
	user = user_collection.find_one({"username" : login_details["username"]})
	if user:
		enc_pass = hashlib .sha256(login_details['password'].encode("utf-8")).hexdigest()
		if enc_pass ==user["password"]:
			access_token= create_access_token(identity=user["username"])
			return jsonify(access_token= access_token),200
	return jsonify({'msg':'Credenciales incorrectas'}),401

@app.route("/api/v1/usersAll",methods=["GET"])

#@jwt_required()
def get_all_users():
	users = user_collection.find()
	data=[]
	for user in users:
		user["_id"] = str(user["_id"])
		data.append(user)
	return jsonify(data)

@app.route("/api/v1/user/<user_id>",methods=["DELETE"])
#@jwt_required()
def delete(user_id):
	delete_user = user_collection.delete_one({'_id':ObjectId(user_id)})
	if delete_user.deleted_count>0:
		return jsonify({"status" : "Usuario eliminado con exito"}),204
	else:
		return "",404

if __name__ == '__main__':
	app.run(debug=True)
```


## Practica 3
```bash
docker pull mongo
docker network create my-mongo-cluster
docker run -p 30001:27017 --name mongo1 --net my-mongo-cluster mongo mongod --replSet my-mongo-set

docker run -p 30002:27017 --name mongo2 --net my-mongo-cluster mongo mongod --replSet my-mongo-set

docker run -p 30003:27017 --name mongo3 --net my-mongo-cluster mongo mongod --replSet my-mongo-set

#--replSet my-mongo-set levanta el mongo pero que este dentro de esta replica

docker exec -it mongo1 mongosh
#exec ejecutar algo dentro del contenedor
```

```bash
db = (new Mongo('localhost:27017')).getDB('udec')

config = { "_id": "my-mongo-set", 
"members": [ {"_id":0, "host":"mongo1:27017"},{"_id":1, "host":"mongo2:27017"},{"_id":2, "host":"mongo3:27017"}]}

rs.initiate(config)
#insertar a la base de datos
db.alumnos.insert({nombre:'Jaime Ansorena'})
db.alumnos.insert({nombre:'Pedro Perez'})
db.alumnos.insert({nombre:'Rodrigo Rodriguez'})

db.alumnos.find()

db2 = (new Mongo('mongo2:27017')).getDB('udec')
db2.alumnos.find()

db3 = (new Mongo('mongo3:27017')).getDB('udec')
db3.alumnos.find()

```