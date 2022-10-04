# Working with Kubernetes Updates and Rollback

## What is Kubernetes Updates? ##

Let's say Kubernetes deployment deploys a particular instance of your application and tomorrow there is a newer version of the application made available. One needs to update the application instance to the newer version to take advantage of the latest and greatest features. Now there are two ways to do this and this is called ** Deployment Strategy **

### Deployment Strategy ###

There are two types of deployment strategies. 
1. **Recreate Strategy** Using this strategy destroy the application instances of the replicaset and redeploy with the newer version of the application instances. The problem with this strategy is that since all replicasets have to be destroyed, there is application downtime
2. **Rolling Update Strategy** Using this approach we do not destroy all instances of the replicaset at once, instead we destroy one at a time and bring up a newer updated version of the application instance. This is called rolling update hence there is no application downtime. Note: Rolling update is the default deployment strategy unless otherwise specified. 

## What is Kubernetes Rollback? ##

When you deploy using the default deployment strategy and update the application instances, Kubernetes maintains an older version of the replicaset without completely destroying it. At any point in time if the application instance has to be rolled back to an older version Kubernetes administrator can trigger a rollback operation which changes the version of the application instance to the older version. This process is called **Rollback**

### Let's now learn how to work with Kubernetes Updates and Rollback ###

