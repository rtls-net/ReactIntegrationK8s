# Docker and k8s integratin with reacts
 ======================================
## steps: 1 

      npx create-react-app simple-app

## steps: 2

     npm build --production

     npm start

## steps: 3 Dockerfile

		***************************
		FROM node:18-alpine
		WORKDIR /react-app
		COPY package.json .
		COPY package-lock.json .
		RUN npm install
		COPY . .
		EXPOSE 3000
		CMD ["npm", "start"]
		****************************



## steps: 4 

     docker build -t rtlsecs/simple-app .

##  steps: 5 
   
      docker login

## steps: 6 

     docker push rtlsecs/simple-app

## steps: 7 minikube config set driver docker

## steps: 8 minikube start

## steps:9 create deployment

	deployments.yml
	*****************************
	 
	 apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: simple-app-deployment
	spec:
	  replicas: 2
	  selector:
	    matchLabels:
	      app: simple-app
	  template:
	    metadata:
	      labels:
		app: simple-app
	    spec:
	      containers:
	      - name: simple-app
		image: victorelvis/simple-app
		resources:
		  limits:
		    memory: "256Mi"
		    cpu: "500m"
		ports:
		- containerPort: 3000
	***********************************

## steps: 10 

        kubectl apply -f deployment.yaml

## steps: 11 expose service 

	service.yaml
	*******************************

	apiVersion: v1
	kind: Service
	metadata:
	  name: simple-app-service
	spec:
	  type: LoadBalancer
	  selector:
	    app: simple-app
	  ports:
	  - port: 3000
	    targetPort: 3000
	    protocol: TCP

	********************	

## steps: 12

      kubectl apply -f service.yaml

## steps: 13 

      kubectl get deployment
	
## steps: 14 
       
       kubectl get service
	
## steps: 15 	Expose Url

     minikube service simple-app-service





## Delete Minikube if any config issue

	***********
	minikube delete
	
	