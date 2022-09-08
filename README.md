# sw
#test

sudo docker build -t jenkins_enbd .

sudo docker run -d -p 8080:8080 -p 50000:50000 --name jenkins_enbd -v /data/:/data/ jenkins_enbd
