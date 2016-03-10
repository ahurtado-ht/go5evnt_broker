========================================
APLICACION 02: BROKER (kafka)
========================================
windows (recomendacion: no instalar en windows)

========================================
BROKER DE EVENTOS
========================================

-  tener una vm linux
-  descargar kafka
		wget http://www.apache.org/dist/kafka/0.9.0.0/kafka_2.11-0.9.0.0.tgz
   instalar kafka en linux
   - (kafka_2.11-0.9.0.0.tgz)
   - tar -xvf kafka_2.11-0.9.0.0.tgz
   cd /.work/kafka_2.11-0.9.0.0
maquina de instalacion: 
 -> 192.168.1.40 (vagrant at: /.work/kafka_2.11-0.8.2.2)
 
#actualizar para que reconozca por ip 
# 
#updating config/server.properties advertised.host.name to the public IP address of the VM.
#	esto es, adicionar lo siguiente:
#	advertised.host.name=192.168.0.13 
 
 
 
 
 
 
 
iniciar zoopeper
```
  cd /.work/kafka_2.11-0.9.0.0
  nohup ./bin/zookeeper-server-start.sh config/zookeeper.properties &
```

iniciar kafka
```
  cd /.work/kafka_2.11-0.9.0.0
  nohup ./bin/kafka-server-start.sh config/server.properties &
```

crear un topico
```
  cd /.work/kafka_2.11-0.9.0.0
  ./bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic bovedaevents
```
  
ver los topicos
```
  ./bin/kafka-topics.sh --list --zookeeper localhost:2181
```
  
ver un topico en especifico
```
 ./bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic bovedaevents
```
 
iniciar un consumer 
```
 ./bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic bovedaevents --from-beginning
```

eliminar un consumer
# adicionar la propiedad del archvo de kafka config/server.properties
# delete.topic.enable=true
# luego ejecutar
# ./bin/kafka-topics.sh --zookeeper localhost:2181 --delete --topic bovedaevents
# eliminar /tmp/zookeper
# eliminar /tmp/kafla-logs
 

#iniciar un producer [escribe por comandos]
# ./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic bovedaevents

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
		 adicionar entrada con la ip en el server
		 # localhost.localdomain 127.0.0.1 192.168.90.30
		 
		 # en los clientes adicionar la ubicacion del server
		 192.168.90.30 srvhpridentidad
		 10.0.2.15 fedora23_ht
		 # nota; si es una vm y esta en modo nat, mapear tambien 127.0.0.1 a esa maquina
		 # es decir, quedaria: 
		 127.0.0.1       localhost localhost.localdomain fedora23_ht

	en windows
		modificar el mismo archivo en %windir%/system32/drivers/etc/hosts



 
 
configurar la maquina con iptables
```
 sudo vi /etc/sysconfig/iptables
```
 
# zookeper & kafka ports
# abrir puertos / configurar iptables (si no esta instalado)
```
 sudo dnf install -y iptables-services
 sudo systemctl enable iptables
 sudo systemctl start iptables
```

#firewall configurations
```
# firewall-cmd --zone=public --add-port=2181/tcp --permanent
# firewall-cmd --zone=public --add-port=9092/tcp --permanent
#### forma 1 
-A INPUT -p tcp -m state --state NEW -m tcp --dport 2181 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 9092 -j ACCEPT
#### forma 2 
 -A INPUT -p tcp --dport 2181 -j ACCEPT
 -A INPUT -p tcp --dport 9092 -j ACCEPT
```

sudo service iptables restart 
```
 # actualziar reglas
 firewall-cmd --reload
 # confgurar puertos en virtualbox (requiere apagar la maquina y configurar http://www.howtogeek.com/122641/how-to-forward-ports-to-a-virtual-machine-and-use-it-as-a-server/)
```


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
 
