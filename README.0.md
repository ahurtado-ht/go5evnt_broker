========================================
APLICACION 02: BROKER (kafka)
========================================
windows (recomendacion: no instalar en windows)

========================================
BROKER DE EVENTOS
========================================


-  tener una vm linux
-  descargar kafka
   instalar kafka en linux
   - (kafka_2.11-0.9.0.0.tgz)
   - tar -xvf kafka_2.11-0.9.0.0.tgz
   cd /.work/kafka_2.11-0.9.0.0
maquina de instalacion: 
 -> 192.168.90.30 (vagrant at: /.work/kafka_2.11-0.8.2.2)
 
iniciar zoopeper
  cd /.work/kafka_2.11-0.8.2.2
  nohup bin/zookeeper-server-start.sh config/zookeeper.properties &

iniciar kafka
  cd /.work/kafka_2.11-0.8.2.2
  nohup ./bin/kafka-server-start.sh config/server.properties &

crear un topico
  cd /.work/kafka_2.11-0.8.2.2
  ./bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic my-topic-test
  
ver los topicos
  ./bin/kafka-topics.sh --list --zookeeper localhost:2181
 
iniciar un consumer 
 ./bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic my-topic-test --from-beginning
 

#iniciar un producer [escribe por comandos]
# ./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic my-topic-test 

 kafka escuha en el puerto 9092.
 zookeeper escucha en el puerto 2181 
 zookeper almacena por defecto los datos en /tmp/zookeeper. 
 (verificar que estan escuchando  netstat -anp | grep 9092)

	requiere configurar el nombre en /etc/hosts tanto de cliente y servidor 
	para que resuelva la comunicacion
		https://www.vultr.com/docs/how-to-change-your-hostname-on-centos
		
	en linux: 	
		ejecutar 
		  hostname
		luego con el nobre dad ejecutar
		 sudo vi /etc/hosts
		 adicionar entrada con la ip
		 192.168.90.30 srvhpridentidad
	en windows
		modificar el mismo archivo en %windir%/system32/drivers/etc/hosts



 
 
configurar la maquina con iptables
 sudo vi /etc/sysconfig/iptables
 
# zookeper & kafka ports
# abrir puertos / configurar iptables (si no esta instalado)
 sudo dnf install -y iptables-services
 sudo systemctl enable iptables
 sudo systemctl start iptables

# firewall-cmd --zone=public --add-port=2181/tcp --permanent
# firewall-cmd --zone=public --add-port=9092/tcp --permanent
#### forma 1 
-A INPUT -p tcp -m state --state NEW -m tcp --dport 2181 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 9092 -j ACCEPT
#### forma 2 
 -A INPUT -p tcp --dport 2181 -j ACCEPT
 -A INPUT -p tcp --dport 9092 -j ACCEPT

sudo service iptables restart 
 # actualziar reglas
 firewall-cmd --reload
 # confgurar puertos en virtualbox (requiere apagar la maquina y configurar http://www.howtogeek.com/122641/how-to-forward-ports-to-a-virtual-machine-and-use-it-as-a-server/)



===========================
REFERENCIAS / KAFKA-CLIENTS-OTROS
===========================


otros clientes de kafka: 
-kafkacat https://github.com/edenhill/kafkacat
-kafkatool
- https://github.com/yahoo/kafka-manager

===========================
REFERENCIAS / KAFKA-EJEMPLOS/INSTALACION
===========================
- http://blog.jaceklaskowski.pl/2015/07/20/real-time-data-processing-using-apache-kafka-and-spark-streaming.html
- http://blog.jaceklaskowski.pl/2015/07/19/publishing-events-using-custom-producer-for-apache-kafka.html (envio simple de mensajes con el api raw - sin reactive)
- http://www.slideshare.net/miguno/apache-kafka-08-basic-training-verisign
- https://www.47deg.com/blog/spark-on-lets-code-part-2
- https://www.digitalocean.com/community/tutorials/how-to-install-apache-kafka-on-ubuntu-14-04 (instalacion)
- https://spring.io/blog/2015/04/15/using-apache-kafka-for-integration-and-data-processing-pipelines-with-spring (uso con spring)
 
