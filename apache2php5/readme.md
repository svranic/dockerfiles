
docker build -t apache2_php5.6 . 

For each project/web app we should run a separate container
Each container should have its own configuration on the host and www directory is mounted also on the host to enable uploading web app to the server.
Each container should run on different port 20080, 20081, 20082, etc.
docker run --detach -p 20080:80 --name=operandum_apache2_php5.6 apache2_php5.6 \
  -v /opt/docker/operandum/apache2_php5/sites-enabled:/etc/apache2/sites-enabled \
  -v /opt/docker/operandum/apache2_php5/www:/var/www