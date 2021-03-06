simple-app:
	PORT=49160 MONGO_URL="mongodb://localhost:27017" node app/server.js

docker-build:
	docker build -t app:v1 app/.
	docker build -t mongo:v1 mongo/.

docker-network:
	docker network create --driver bridge mongo

docker-mongo:
	docker run --net=mongo --name mongo -it -p 27017:27017 mongo:v1

docker-app:
	docker run --net=mongo --name app -it -p 49160:80 -v app:/app \
		-e PORT=80 -e MONGO_URL="mongodb://mongo:27017" app:v1

docker-delete:
	docker rm app -f
	docker rm mongo -f

build:
	eval $$(minikube docker-env) && docker build -t app:v1 app/.
	eval $$(minikube docker-env) && docker build -t mongo:v1 mongo/.

create:
	kubectl create -f mongo/mongo.yaml --validate=false
	kubectl create -f app/app.yaml --validate=false

server-log:
	kubectl logs `kubectl get pods | grep app-deployment |cut -d " " -f1`

server:
	minikube service app-service

delete:
	kubectl delete -f mongo/mongo.yaml
	kubectl delete -f app/app.yaml
