# k8_graw
git clone https://github.com/vivstorage/k8_graw.git  
cd k8_graw  
kubectl create -f nginx.yml  

it will create 2 contaners nginx and phpfpm    
>kubectl get po  
NAME                      READY     STATUS    RESTARTS   AGE  
nginx-3614435126-2mvjh    1/1       Running   0          2m  
phpfpm-2800297442-b8078   1/1       Running   0          2m   
  
phpfpm-2800297442-b8078 in our case, 
then run git clone and grav install there  
kubectl exec -ti phpfpm-2800297442-b8078 -- chown www-data /app  
kubectl exec -ti phpfpm-2800297442-b8078 -- sudo -u www-data bash -c 'git clone https://github.com/getgrav/grav.git /app'  
kubectl exec -ti phpfpm-2800297442-b8078 -- sudo -u www-data bash -c 'cd /app && bin/grav install'  
  
**Testing**  
minikube service nginx  