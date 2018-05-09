
# Gestor de incidencias - Grupo E1B

[![Join the chat at https://gitter.im/Agents_e1b](https://badges.gitter.im/InciManager_e1b.svg)](https://gitter.im/InciManager_e1b/Lobby?utm_source=share-link&utm_medium=link&utm_campaign=share-link)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/1d7cb9ab12dd4230a9a1ccdc3a723185)](https://www.codacy.com/app/jelabra/InciManager_e1b?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=Arquisoft/InciManager_e1b&amp;utm_campaign=Badge_Grade)
[![Build Status](https://travis-ci.org/Arquisoft/InciManager_e1b.svg?branch=master)](https://travis-ci.org/Arquisoft/InciManager_e1b)
[![codecov](https://codecov.io/gh/Arquisoft/InciManager_e1b/branch/master/graph/badge.svg)](https://codecov.io/gh/Arquisoft/InciManager_e1b)

**Bienvenidos a nuestro módulo: InciManagement**

Este módulo forma parte de un sistema informático de análisis gestión de incidencias, el cuál se encarga de la gestión y la carga de las mismas.

## Autores ##

- [Esteban Montes Morales (UO246305)](https://github.com/sankosk)
- [Lucía de la Granda Prieto (UO251626)](https://github.com/luciadelagranda)
- [Pablo Menéndez Suárez (UO252406)](https://github.com/mistermboy)
- [Sara Grimaldos Rodríguez (UO251782)](https://github.com/saritagrimal2)


## Dependencias del proyecto ##

- **Librería para mongoDB**

```xml
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-mongodb -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-mongodb</artifactId>
		</dependency>
```

- **Librería para apache kafka**
```xml
		<!-- https://mvnrepository.com/artifact/org.springframework.kafka/spring-kafka -->
		<dependency>
			<groupId>org.springframework.kafka</groupId>
			<artifactId>spring-kafka</artifactId>
			<version>2.1.4.RELEASE</version>
		</dependency>
```

- **Apache HTTP Components**
```xml
		<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
		<dependency>
	    		<groupId>org.apache.httpcomponents</groupId>
	    		<artifactId>httpclient</artifactId>
	    		<version>4.5.3</version>
		</dependency>
```

- **Selenium**
```xml
		<dependency>
			<groupId>org.seleniumhq.selenium</groupId>
			<artifactId>selenium-server-standalone</artifactId>
			<version>2.53.0</version>
		</dependency>
```

- **Selenium reporter plugin**
```xml
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-surefire-report-plugin</artifactId>
			<version>2.13</version>
		</plugin>
```

- **Cucumber unit testing**
```xml
		<dependency>
			<groupId>info.cukes</groupId>
			<artifactId>cucumber-junit</artifactId>
			<version>1.2.5</version>
			<scope>test</scope>
		</dependency>
```

- **Cucumber spring**
```xml
		<dependency>
			<groupId>info.cukes</groupId>
			<artifactId>cucumber-spring</artifactId>
			<version>1.2.5</version>
			<scope>test</scope>
		</dependency>
```

- **Gatling pruebas de carga**
```xml
		<dependency>
			<groupId>io.gatling.highcharts</groupId>
			<artifactId>gatling-charts-highcharts</artifactId>
			<version>2.3.1</version>
			<scope>test</scope>
		</dependency>
```

- **Gatling plugin**
```xml
		<plugin>
			<groupId>io.gatling</groupId>
			<artifactId>gatling-maven-plugin</artifactId>
			<version>2.2.4</version>
			<configuration>
				<runMultipleSimulations>true</runMultipleSimulations>
			</configuration>
		</plugin>
```

- **Scala compiler para las simulaciones de gatling**
```xml
		<plugin>
        	<groupId>net.alchim31.maven</groupId>
        	<artifactId>scala-maven-plugin</artifactId>
        	<version>3.3.2</version>
		</plugin>
```

## ¿Cómo funciona?

Una serie de agentes, porporcionados por [el módulo 2 de agentes](https://github.com/Arquisoft/Agents_e1b) pueden enviar una serie de incidencias al sistema, que las **procesara** (***apache kafka***) y **almacenará** (***mongodb***) para futuramente ser mostradas en un dashboard, para su posterior análisis.

El aspecto de la incidencia es el siguiente (siguiendo las directrices de los requisitos proporcionados por el profesorado):

```json
{
  "ident":"entidad2",
  "password":"123456",
  "kind":2,
  "name":"inc_GHKB",
  "description":"Nueva lectura de humedad",
  "location":"2919,178",
  "tags":["Nieve","Fuego","Niebla","Terremoto"],
  "additionalInformation":"http://puntoverdeleon.com.mx/wp-content/uploads/2016/09/imagen-de-prueba-320x240.jpg",
  "properties":{
    "p0":"v0","p1":"v1","p2":"v2","p3":"v3"
  },
  "state":"Abierta",
  "notification":"si",
  "expireAt":"2018-10-25 10:02:29.769579",
  "assignedTo":"oper_rUxl"
}
```
**En donde**:

Estos tres campos se encargarían de la identificación del agente:

- **Ident**: Es el identificador del agente.
- **Password**: Es su contraseña .
- **Kind**: Tipo de agente.


Y el resto serían la información relativa a la incidencia:

- **Name**: Nombre de la incidencia.
- **Description**: Descripción de la incidencia.
- **Location**: Por defecto es la localización en la que se encuentra el agente, aunque se le puede cambiar el valor por si la incidencia ocurre en un lugar distinto a dónde el agente está situado.
- **Tags**: Lista de etiquetas que describen y categorizan la incidencia.
- **AdditionalInformation**: Enlaces informativos o multimedia (fotos o videos) que porporcionan información adicional sobre la incidencia.
- **Properties**: Propiedades de la incidencia
- **State**: Estado de la incidencia, si esta se encuentra abierta, siendo procesada, cerrada o anulada.
- **Notification**: Hace referencía a si está se desea notificar en el dashboard o no.
- **ExpireAt**: Fecha de expiración, en la que la incidencia se eliminará automáticamente del sistema.
- **AssignedTo**: Hace referencia al operario al que se le asigna la incidencia.


### Servicio REST

Disponemos principalmente de dos **endpoints**:

- ``` http://localhost:8091/user```

- ``` http://localhost:8090/incidence-creator```

El segundo se comunica con el primero, para realizar la identificación del agente y comprobar si este existe o no en el sistema.

Ambos reciben y generan información en formato JSON y se comunican por método POST.



Por tanto, será este último el que se deberá utilizar, ya que se comunica con el primero. Este recibe como entrada el json mostrado anteriormente y devolverá un mensaje de error si no se pudo crear la incidencia o un mensaje que avisará de que la incidencia ha sido procesada correctamente.


### Servicio WEB

Disponemos tambien de un servicio web que estará disponible accediendo a:

``` http://localhost:8090/ ```


## Como lanzar el proyecto ##


### Lanzar apache kafka y zookeeper
Situate en el directorio de apache kafka, abre dos terminales y ejecuta:

En una:

``` bin/zookeeper-server-start.sh config/zookeeper.properties ```

En la otra:

``` bin/kafka-server-start.sh config/server.properties ```

**OJO**: Si usas windows la ruta cambia:
```bin``` por ```bin\windows``` y ```.sh``` por ```.bat```


### Lanzar Agents y InciManager ###

Abre dos terminales, en una situate en el directorio de Agents y en la otra en el directorio de InciManager. En ambas terminales ejecuta:

``` mvn spring-boot:run ```


### Generador de incidencias ####

El generador de incidencias simplemente simula ser una aplicación que envia incidencias de forma automática, esta debería ser configurada para que se enviarán cada cierto tiempo, pero por comodidad y simplicidad, puesto que va a ser una demo, esta funcionalidad no se ha implementado.

Esta programado en ***Python 2.7*** y no tiene nada que ver con el proyecto, es una aplicación de terceros que lanza peticiones contra el **endpoint** del gestor de incidencias.

Esta genera los campos de la incidencia aleatoriamente y construye el objeto json que se enviará por parámetros al creador de incidencias.

Se ejecuta de la siguiente forma:

```
python2 generadorDeIncidencias.py n
```

Donde ***n*** es el número de incidencias a generar. Hay que tener en cuenta que estamos trabajando con una base de datos MongoDB alojada de forma remota en **mlab.com** por lo que no conviene propasarse con el número de incidencias a enviar, ya que es un hosting gratuito y el tamaño máximo que nos dejan alojar son 500MB y tendrá un ancho de banda limitado.

El aspecto de una incidencia autogenerada tiene el aspecto del JSON mostrado al principio del readme.

### Pruebas de carga - gatling
Simplemente te situas en el directorio del proyecto, y con los servicios activos, abris una consola y tecleais:
```
mvn gatling:execute
```

Y se ejecutarán las pruebas de carga sobre los escenarios cargados en el directorio **/src/test/scala**, puedes revisar los resultados luego en el directorio **/target/gatling**

### Pruebas selenium
Actualmente las pruebas de selenium se encuentran en la rama del proyecto llamada "Selenium", donde se encuentra el proyecto entero con estas pruebas incluidas, importante tener el firefox portable en el C://.

Las hemos tenido que retirar de la rama principal porque da conflictos con travys CI.

### Endpoint despliegue
Despliegue: incimanagere1b.herokuapp.com
