
docker build -t apache2_php5.6 . 

For each project/web app we should run a separate container

Proxy for enabling communication between http server and geoserver (i.e. tomcat) should be defined in apache http server at the host level

Each container should have the same configuration and www directory is mounted to the host to enable uploading web app to the server.

Each container should run on different port 20080, 20081, 20082, etc.

docker run -v /opt/docker/operandum/apache2_php5/www:/var/www \
  --detach -p 20081:80 --name=operandum_apache2_php5.6 apache2_php5.6
