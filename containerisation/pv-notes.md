
# Task 3 - Kubernetes Persistent Storage

## Goal

Keep MongoDB data safe even when the database Pod is deleted and recreated.

Previously, MongoDB had no persistent storage, so data could be lost.

## What are PV and PVC?

- **PV (PersistentVolume):** Storage that Kubernetes can use.
- **PVC (PersistentVolumeClaim):** A request to use that storage.
- **Volume mount:** Connects the storage to MongoDB.

## Files

- `mongo-storage.yml` - Creates a 1Gi PV and PVC.
- `mongo.yml` - Runs MongoDB and connects it to the PVC.
- `ttt-deploy.yml` - Runs 3 TTT app Pods.
- `ttt-service.yml` - Lets users access the app.

## How It Works

1. Kubernetes creates a 1Gi PV.
2. The PVC connects to the PV.
3. MongoDB saves its data in `/data/db` using the PVC.
4. All 3 TTT app Pods share the same MongoDB database.
5. If the MongoDB Pod is replaced, the new Pod can use the same storage.

## Deploy

Create the storage:

`kubectl apply -f mongo-storage.yml`

Deploy MongoDB:

`kubectl apply -f mongo.yml`

Check the storage:

`kubectl get pv`

`kubectl get pvc`

Both should show `Bound`.

Check the Pods:

`kubectl get pods`

## Seed the Database

If the database is empty, seed it once:

`kubectl exec deployment/ttt-deployment -- node seeds/seed.js`

All 3 app Pods share the same database, so we only seed once.

**Warning:** The seed script replaces existing records in its target collection.

## Test Persistence

1. Open the TTT app.
2. Select **Persistent with Mongo DB**.
3. Check that the scoreboard has data.
4. Delete the MongoDB Pod:

`kubectl delete pod -l app=mongo`

5. Kubernetes automatically creates a new MongoDB Pod.
6. Check the Pods:

`kubectl get pods`

7. Refresh the scoreboard.

**Expected result:** The same data is still on the scoreboard because MongoDB uses persistent storage.


## Clean Up

When finished, remove MongoDB and the storage:

`kubectl delete -f mongo.yml`

`kubectl delete pvc mongo-pvc`

`kubectl delete pv mongo-pv`

**Note:** Our PV uses `Retain`, so deleting it does not automatically erase the files stored on the Kubernetes node.

## Result

MongoDB now uses persistent storage, allowing database data to survive Pod replacement on the same local Kubernetes node.
