# k8_graw
git clone https://github.com/vivstorage/k8_graw.git  
cd k8_graw  
kubectl create -f nginx.yml  

it will create 2 contaners nginx and phpfpm    
kubectl get po  
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

  
I choose basic schema nginx -> php-fpm with load balancing etc, most of time I spend to find  
docker container with all php stuff there and not found it, so I made a little hack for entrypoint phpfpm container  
command: ["/bin/sh"]  
        args: ["-c", "apt-get update && apt-get install git libzip-dev sudo -y && pecl install zip && echo 'extension=zip.so' > /usr/local/etc/php/conf.d/php.ini && /usr/local/sbin/php-fpm", "--nodaemonize"]  


By default it starts pod with last nginx version  
and one with php-fpm 5.6 and they have one shared volume for source code, it will make easy autodeploy cuz we  
need just replace dir on node/nfs/s3/anywhere or just update code there. Also spent lot time with YAML config itself,  
with theyr spaces and other nice stuff.  