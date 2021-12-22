# 9 - Controlling Access to K8S with RBAC

In the last lab, we have reached this scenario, when trying to access pos resources as user "dave":  

    > kubectl get pods
    NAME                            READY   STATUS    RESTARTS      AGE
    my-deployment-56474dbc6-gxpbd   1/1     Running   2 (34h ago)   3d6h
    my-deployment-56474dbc6-jln9h   1/1     Running   2 (34h ago)   3d6h
    my-deployment-56474dbc6-shfpn   1/1     Running   2 (34h ago)   3d6h
    > 
    > kubectl get pods --kubeconfig .kube/dave-config 
    Error from server (Forbidden): pods is forbidden: User "dave " cannot list resource "pods" in API group "" in the namespace "default"
    > 


- [Control plane upgrade steps](#Control-plane-upgrade-steps)
- [Worker node upgrade steps](#Worker-node-upgrade-steps)

## Create and apply the role

- Look at the **pod-reader-role.yaml** file in this lab.  
- It will allow get, waych and list operation on pods and pod logs.
- Let's create this role:  
**kubectl apply -f pod-reader-role.yaml**  
The pod should have been created.

## Create and apply the role binding

- Look at the **pod-reader-role-binding.yaml** file
- Notice that the user name is in quotes, and contain one more space.  When I created the user certificate I had enterred a nother space, so now I have to make sure that the user name matches the binding.
- Let's apply it:  
**kubectl apply -f pod-reader-role-binding.yaml**

Now it works:

    > kubectl get pods --kubeconfig .kube/dave-config 
    NAME                            READY   STATUS    RESTARTS      AGE
    my-deployment-56474dbc6-gxpbd   1/1     Running   2 (34h ago)   3d6h
    my-deployment-56474dbc6-jln9h   1/1     Running   2 (34h ago)   3d6h
    my-deployment-56474dbc6-shfpn   1/1     Running   2 (34h ago)   3d6h
    > 

