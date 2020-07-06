# k8s-manifests-backup

In order to save all manifests, create a BASH script

```shell
#!/bin/bash
 
DIR='k8s-manifests/namespaces'
 
mkdir -p $DIR
 
for NAMESPACE in $(kubectl get -o=name namespaces | cut -d '/' -f2)
do
    for TYPE in $(kubectl get -n $NAMESPACE -o=name pvc,configmap,serviceaccount,secret,ingress,service,deployment,statefulset,hpa,job,cronjob)
    do
        mkdir -p $(dirname $DIR/$NAMESPACE/$TYPE)
        kubectl get -n $NAMESPACE -o=yaml $TYPE > $DIR/$NAMESPACE/$TYPE.yaml
    done
done
```

```shell
chmod +x backup-k8s.sh
```

Run this BASH script on node with ~/.kubeconfig
