### Online YAML Lint

```
http://www.yamllint.com/
```

### Pods

```
kubectl run nginx --image=nginx

```
creates a pod ( deployment, without using a definition file)


```
kubectl get pods -o wide

```

### Replication Controller
```
kubectl create -f rc-definition.yml

kubectl get replicationcontroller

kubectl get pods
```

### ReplicaSet

selector - is the major difference between Replicaset and Replication controller
```
kubectl create -f replicaset-definition.yml

kubectl get replicaset

kubectl get pods

kubectl describe replicaset <<replicasetname>>

kubectl delete replicaset <<replicasetname>>
```
### Labels and selectors

ReplicaSet / Replication controller:
  - Can Monitor existing pods already created
  - can create pods
  - it monitors the pods and create them if any one goes down to maintain the desired no. of pods
  - scale up or scale down
```
    kubectl replace -f replicaset-definition.yml
                  or
    kubectl scale --replcas=6 -f replicaset-definition.yml
                  or
    kubectl scale --replicas=6 replicaset myapp-replicaset
    (replicaset type replicaset name)
```

### Deployments

- Rolling updates
- Rollbacks
- Upgrades
- Modify resource allocations


kubectl create -f deployment-definition.yml

kubectl get deployments

kubectl get replicaset

kubectl get pods


kubectl get all


#### rollout and Versioning

Revision 1 - first deployment

Revision 2  - new deployment

status of rollout and history
```
kubectl rollout status deployment/<<deploymentname>>

kubectl rollout history deployment/<<deploymentname>>

```
Deployment Strategy:

Recreate:
  - destroy all and create a newer versions
  Problem with this - application would be down and inaccessible to the user
  - This is a recreation strategy

Rolling update: ( Default strategy)
- Bring down one and update one 
- no downtime


```
  kubectl rollout undo deployment/myapp-deployment

```
brings up the old replicaset one by one and shuts down the pods on new replicaset one by one

observe the behaviour by running
```
kubectl get replicasets

```

- To get record the change cause ( this can be viewed on rollout history)

```
 kubectl create -f deployment/deployment-definition.yml --record

  kubectl rollout history deployment/myapp-deployment

```
#### kubectl apply

upgrading the newer version on deployment file and then
```
- kubectl apply -f deployment-definition.yml

OR

- kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1

```

Underthehood - Rolling update strategy, it creates a new replicaset and start taking down the pod one by one on the old replicaset 

- view the replicasets whilst this in progress
```
  kubectl get replicasets
```

Some examples

Updating the version -
  - kubectl apply -f deployment/deployment-definition.yml ( OR )
  - kubectl set image deployment/myapp-deployment nginx=nginx:1.12-perl
  - kubectl rollout history deployment/myapp-deployment
  - kubectl rollout undo deployment/myapp-deployment
  - kubectl rollout status deployment/myapp-deployment
  - kubectl rollout history deployment/myapp-deployment

