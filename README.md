# Introducción

Este repositorio alberga el fichero de configuración de fig para poder levantar un servicio de recolección de logs. Está basado en los contenedores Docker [luispa/base-fluentd](https://registry.hub.docker.com/u/luispa/base-fluentd/) y [luispa/base-eskibana](https://registry.hub.docker.com/u/luispa/base-eskibana/). 

La combinación de fluentd + elasticsearch + kibana permite unificar, almacenar y visualizar todos los log's de todos mis contenedores docker en mi equipo linux en un único sitio.

[Fluentd](http://www.fluentd.org/) es un recolector que unifica "logs", proyecto open source que permite aunar colecciones de datos y que sean consumidos de forma centralizada para poder entenderlos mejor. 

[Elasticsearch](http://www.elasticsearch.org/) es un motor de búsqueda (una base de datos para que nos entendamos), también es un proyecto opensource que es muy conocido por su sencillez de uso. 

[Kibana](http://www.elasticsearch.org/overview/kibana/) es un interfaz de usuario Web que permite realizar búsquedas más amigables en elasticsearch. 

Al combinar las tres herramientas (Fluentd + Elasticsearch + Kibana) conseguimos un sistema escalable, sencillo y flexible que agrega los logs en un motor de búsqueda que puede ser consumido de forma sencilla desde la web. En mi caso he decidido separar las tres herramientas en dos contenedores, uno con fluentd y el otro con elasticsearch y kibana.

Consulta este [apunte técnico sobre varios servicios en contenedores Docker](http://www.luispa.com/?p=172) para acceder a otros contenedores Docker y sus fuentes en GitHub.


## Ejecución con "fig"

Para automatizar con el programa [fig](http://www.fig.sh/index.html) y que arranquen el contenedor descarga y renombra el fichero fig-template.yml, modifica las variables y ejecuta el programa: 

    $ git clone https://github.com/LuisPalacios/servicio-log.git
    :
    $ mv fig-template.yml fig.yml   (edita a tu gusto)
    :
    $ fig up -d


# Personalización

### Variable: FLUENTD_PORT

Puerto por el que escuchará fluentd en este contenedor. Cuando configuremos rsyslogd (o cualquier otro gestor de logs) en los contenedores remotos que quieran enviarme sus logs, tenemos que darles este puerto como el de destino. 

## Puertos

Expongo los siguientes puertos: 

	- "9200" - para poder conectar con elasticsearch de  forma directa
	- "8081" - para poder conectar con kibana (http://src.dominio.com::8081)

## Volúmenes

Es importante que prepares un directorio persistente para tus datos de elasticsearch, en mi caso lo he dejado en el siguiente directorio: 

  - "/Apps/data/log/elasticsearch/data/:/data"


Directorio persistente para configurar el Timezone. Crear el directorio /Apps/data/tz y dentro de él crear el fichero timezone. Luego montarlo con -v o con fig.yml

    Montar:
       "/Apps/data/tz:/config/tz"  
    Preparar: 
       $ echo "Europe/Madrid" > /config/tz/timezone
