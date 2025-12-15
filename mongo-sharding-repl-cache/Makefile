init-config-server:
	@echo "Initializing config server replica set..."
	docker exec -i config_srv mongosh --port 27017 --eval " \
	rs.initiate( \
		{ \
			_id: \"config_server\", \
			configsvr: true, \
			members: [{ _id: 0, host: \"config_srv:27017\" }] \
		} \
	)"
	@sleep 2

init-shard1:
	@echo "Initializing shard 1..."
	docker exec -i shard1 mongosh --port 27017 --eval " \
	rs.initiate( \
		{ \
			_id: \"rs1\", \
			members: [ \
				{ _id: 0, host: \"shard1:27017\" }, \
				{ _id: 1, host: \"shard1-1:27017\" }, \
				{ _id: 2, host: \"shard1-2:27017\" } \
			] \
		} \
	)"
	@sleep 2

init-shard2:
	@echo "Initializing shard 2..."
	docker exec -i shard2 mongosh --port 27017 --eval " \
	rs.initiate( \
		{ \
			_id: \"rs2\", \
			members: [ \
				{ _id: 0, host: \"shard2:27017\" }, \
				{ _id: 1, host: \"shard2-1:27017\" }, \
				{ _id: 2, host: \"shard2-2:27017\" } \
			] \
		} \
	)"
	@sleep 2

init-router:
	@echo "Initializing router..."
	docker exec -i router mongosh --port 27017 --eval " \
	sh.addShard( \"rs1/shard1:27017\"); \
	sh.addShard( \"rs2/shard2:27017\"); \
	sh.enableSharding(\"somedb\"); \
	sh.shardCollection(\"somedb.helloDoc\", { \"name\": \"hashed\" });"
	@sleep 2

init-cluster: init-config-server init-shard1 init-shard2 init-router

dummy-data:
	@echo "Generating dummy data..."
	docker exec -i router mongosh --port 27017 --eval "\
	var db = db.getSiblingDB('somedb'); \
	for (var i = 0; i < 1000; i++) { \
		db.helloDoc.insertOne({ age:i, name:'ly'+i }); \
	}"
