export B2D_NFS_SYNC=1
export DOCKER_HOST=tcp://127.0.0.1:2376
export DOCKER_CERT_PATH=/Users/username/Sites/tls
export DOCKER_TLS_VERIFY=1
 
docker_up() {
  cd ~/Sites
  vagrant up
  echo "vagrant status: "
  vagrant status
  docker_boot
}
alias bup='docker_up'
 
boot_to_docker_down() {
    dstop
    vagrant halt
}
alias bdn='boot_to_docker_down'
 
export GHR=https://github.com/sheknows
 
if [ -f ~/.git-completion.bash ]; then
  . ~/.git-completion.bash
fi
 
vagrant-enter() {
  vagrant ssh -c "sudo docker exec -it $@ /bin/bash"
}
alias de='vagrant-enter'
 
listening() {
  sudo lsof -i -n -P | grep TCP
}
 
 
dls() {
  docker ps
}
 
dll() {
  docker ps -a
}
 
drun() {
  if [[ $# -eq 0 ]]
    then
      echo "Which container? Provide 1 argument."
      return 0
  fi 
  
  case "$1" in
    'sk' | 'sheknows' )
      docker run -d \
      -P --name sk \
      --env APP=sk \
      --env SK_ENVIRONMENT=local \
      --link db:db \
      --link redis:redis \
      -p 8080:80 \
      -v /vagrant/sheknows:/src \
      -t docker.dev.sheknows.us:5000/sheknows \
      /opt/run/start.sh
      ;;
    'admin' | 'cms' )
      docker run -d \
      -P --name cms \
      --env APP=cms \
      --env SK_ENVIRONMENT=local \
      --link db:db \
      --link redis:redis \
      --link es:es \
      --link cn:cn \
      -p 8081:80 \
      -v \
      /vagrant/cms:/src \
      -t docker.dev.sheknows.us:5000/kitchen-sink \
      /opt/run/start.sh
      ;;
    'cn' | 'connect' )
      docker run -d \
      -P --name cn \
      --env APP=cn \
      --env SK_ENVIRONMENT=local \
      -p 8082:8082 \
      --link db:db \
      --link mongodb:mongodb \
      --link redis:redis \
      --link cms:cms \
      -v \
      /vagrant/connect:/src \
      -t docker.dev.sheknows.us:5000/connect \
      /opt/run/start.sh
      ;;
  'admin2' | 'pros' )
      docker run -d \
      -P --name pros \
      --env APP=pros \
      --env SK_ENVIRONMENT=local \
      --link db:db \
      --link es:es \
      -p 8083:80 \
      -v \
      /vagrant/proscenium:/src \
      -t docker.dev.sheknows.us:5000/proscenium \
      bash
      ;;
 
    'cms-api' | 'cmsapi' )
       
      docker run -d \
      -P --name cmsapi \
      --env APP=cmsapi \
      --env SK_ENVIRONMENT=local \
      -p 8084:80 \
      --link es:es \
      --link redis:redis \
      -v /vagrant/cms-api:/src \
      -t phusion/passenger-nodejs \
      bash
      ;;
    'es' | 'elasticsearch' )
      docker run -d \
      --name es \
      --env APP=es \
      elasticsearch
      ;;
  'db' | 'mysql' )
      docker run -d \
      -P --name db \
      -e APP=db \
      -e MYSQL_ROOT_PASSWORD=D0ck3Rr00tPa55 \
      -e MYSQL_USER=sheknows_w \
      -e MYSQL_PASSWORD=88ey2phaphEspech \
      -v /vagrant/sheknows.db:/src \
      mysql
      ;;
  'redis' )
      docker run -d \
      --name redis \
      -p 6379:6379 \
      redis
      ;;
  'mongo' | 'mongodb' )
      docker run -d \
      --name mongodb \
      -p 27017:27017 \
      -v /vagrant/mongodb:/src \
      tutum/mongodb mongod \
      --smallfiles
      ;;
  'nsenter' )
      docker run --rm \
      -v /usr/local/bin:/target \
      jpetazzo/nsenter
      ;;
  * )
       
      echo "Container not recognized"
      ;;
  esac
}
 
# docker stop
dsto() {
  if [[ $# -eq 0 ]]
    then
      echo "Which container? Provide 1 argument."
      return 0
  fi 
  docker stop $1
}
   
# docker start
dsta() {
  if [[ $# -eq 0 ]]
    then
      echo "Which container? Provide 1 argument."
      return 0
  fi 
  docker start $1
}
   
# docker remove
drm() {
  if [[ $# -eq 0 ]]
    then
      echo "Which container? Provide 1 argument."
      return 0
  fi 
  docker rm $1
}
 
# docker stop all running
dsar() {
    if [[ $# -eq 0 ]]
      then
        docker ps | tail -n +2 |cut -d ' ' -f 1 | xargs docker stop
    else
        dsto $1;
    fi
}
alias dstop='dsar'
 
# docker remove all stopped
dras() {
    docker ps -a | tail -n +2 | grep Exited | cut -d ' ' -f 1 | xargs docker rm
}
 
# docker kill all
docker-killall() {
    dsar
    dras
}
alias dka='docker-killall'
 
# Docker get an ext ip
dip() {
  if [[ $# -eq 0 ]]
    then
      echo "Which container? Provide 1 argument."
      return 0
  fi
   docker inspect --format='{{.NetworkSettings.IPAddress}}' $1
}
 
docker_start() {
    dsta db
    dsta redis
    dsta es
    dsta mongodb
    dsta cn
    dsta sk
    dsta cms
    dsta pros
    dls
}
alias dstart='docker_start'
 
 
 
docker_boot() {
  drun nsenter
  drun db
  drun es
  drun redis
  drun mongodb
  drun sk
  drun cms
  drun cn
  drun pros
  docker_start
}
alias dboot='docker_boot'
 
docker_reboot() {
  dsar
  dras
  dboot
}
alias drb='docker_reboot'
 
docker_images() {
    docker images
}
alias dim='docker_images'
 