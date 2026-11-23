# Kubernetes - Sparta TTT App

## Task 1 - Deploy the TTT App

**Goal:** Run 3 copies of the Sparta TTT app using Kubernetes.

### Files

- `ttt-deploy.yml` - Creates 3 app Pods using my Docker Hub image.
- `ttt-service.yml` - Creates a NodePort Service so we can access the app.

### What happens?

1. Kubernetes downloads my app image from Docker Hub.
2. It creates 3 Pods running the app.
3. The Service connects users to the app Pods.
4. Kubernetes can restart a Pod if it fails.

### Commands

Deploy the app:

`kubectl apply -f ttt-deploy.yml`

Create the Service:

`kubectl apply -f ttt-service.yml`

Check the Pods:

`kubectl get pods`

Check the Service:

`kubectl get services`

### Access the app

The app runs on port `3000`.

The NodePort Service uses port `30080`.

Try:

`http://localhost:30080`

If this does not work, use:

`kubectl port-forward service/ttt-service 3001:3000`

Then open:

`http://localhost:3001`

**Result:** The TTT app runs with 3 replicas in Kubernetes.

---

## Task 2 - Deploy the TTT App with MongoDB

**Goal:** Connect the 3 TTT app Pods to one MongoDB database.

### Files

- `ttt-deploy.yml` - Creates 3 app Pods and adds the database connection.
- `ttt-service.yml` - Lets users access the app.
- `mongo.yml` - Creates 1 MongoDB Pod and an internal Service.

### What happens?

1. Kubernetes starts 1 MongoDB Pod.
2. It creates a Service called `mongo`.
3. The 3 app Pods connect to MongoDB using this Service.
4. All 3 app Pods share the same database.

### Database connection

In `ttt-deploy.yml`, under the app container image, add:

    env:
      - name: MONGODB_URI
        value: "mongodb://mongo:27017/posts"

- `MONGODB_URI` tells the app where MongoDB is.
- `mongo` is the MongoDB Service name.
- `27017` is the MongoDB port.
- `posts` is the database name.

### Commands

Start MongoDB:

`kubectl apply -f mongo.yml`

Deploy the app:

`kubectl apply -f ttt-deploy.yml`

Create the app Service:

`kubectl apply -f ttt-service.yml`

Check everything:

`kubectl get all`

### Access the app

`kubectl port-forward service/ttt-service 3001:3000`

Open:

`http://localhost:3001`