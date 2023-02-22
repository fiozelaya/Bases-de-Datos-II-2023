# ¿Qué es Elasticsearch?

#### ElasticSearch es un montor de análisis y búsqueda distribuida que facilita la recopilación y agregación de los datos y el almacenamiento en Elasticsearch gracias a Logstash y Beats.
#### Elasticsearch proporciona búsqueda y análisis casi en tiempo real para todo tipo de datos. Ya sea que tenga texto o datos (numéricos, geoespaciales, etc) estructurados o no estructurados, Elasticsearch puede almacenarlos e indexarlos de una forma eficiente que posibilite las búsqueda rápidas. También puede recuperar y agregar datos para reconocer tendencias y patrones en los datos.
#### Elasticsearch ofrece velocidad y flexibilidad para manejar los datos en una gran variedad de casos de uso. Algunos ejemplos son los siguientes:
- Agregar un cuadro de búsqueda a una aplicación o sitio web
- Almacenar y analizar registros, métricas y datos de eventos de seguridad
- Utilizar machine learning para modelar de forma automática el comportamiento de los datos en tiempo real
- Automatizar los flujos de trabajo empresariales utilizando Elasticsearch como motor de almacenamiento
- Administrar, integrar y analizar información espacial usando Elasticsearch como un sistema de información geográfica (GIS)
- Almacenar y procesar datos genéticos utilizando Elasticsearch como herramienta de investigación bioinformática

---

# Entrada de información: Documentos e índices

#### Elasticsearch almacena estructuras de datos complejas que se han serializado como documentos JSON. Cuando existen varios nodos de Elasticsearch en un clúster, los documentos almacenados se distribuyen por todo el clúster y se puede acceder a ellos de inmediato desde cualquier nodo.
#### Cuando se almacena un documento, se indexa y se puede buscar por completo casi en tiempo real. Elasticsearch utiliza una estructura de datos llamada índice invertido que enumera cada palabra única que aparece en el documento que identifica todos los documentos en los que aparece la palabra, lo que admite búsquedas de texto completo muy rápidas.
#### Un índice puede pensarse como una colección optimizada de documentos, donde cada documento es asimismo una colección de campos, que son los pares de clave-valor (key-value) que contienen los datos.
#### Elasticsearch también tiene la capacidad de no tener esquemas, por lo que los documentos se pueden indexar sin especificar explícitamente cómo manejar cada uno de los diferentes campos que pueden aparecer en un documento, ya que si el mapeo dinámico está habilitado, Elasticsearch detecta los tipos de datos apropiados y agrega automáticamente nuevos campos al índice. Esto facilita la indexación y exploración de los datos.
#### Asimismo, también se pueden definir ciertas reglas para controlar el mapeo dinámico y definir mapeos donde se especifique cómo se almacenan e indexan los campos. Definir asignaciones propias permite:
- Distinguir entre campos de cadena de texto completo y campos de cadena de valor exacto
- Realizar un análisis de texto específico del idioma
- Optimizar campos para coincidencias parciales
- Usar formatos de fecha personalizados
- Utilizar tipos de datos como geo_point y geo_shape que no se pueden detectar automáticamente
#### También se puede indexar un campo de diferentes maneras para diferentes propósitos. Otro detalle importante es que la cadena de análisis que se aplica a un campo de texto completo durante la indexación también se usa en el momento de la búsqueda.

---
# Salida de información: Buscar y analizar

#### Gracias a Elasticsearch, además de almacenar y recuperar documentos, se puede acceder a todas las capacidades de búsqueda integradas en la biblioteca del motor de búsqueda Apache Lucene. 
#### Elasticsearch proporciona una API REST simple para administrar el clúster e indexar y buscar los datos.

## Buscar datos

#### Las API REST de Elasticsearch admiten consultas estructuradas, consultas de texto completo y consultas complejas que combinan las dos. Las consultas estructuradas son similares a los tipos de consultas que se puede construir en SQL. Las consultas de texto completo encuentran todos los documentos que coinciden con el string de consulta y los devuelven ordenados según la relevancia.
#### También se puede realizar búsqueda de frases, similitudes y prefijos, y obtener sugerencias de autocompletado.
#### Asimismo, Elasticsearch indexa datos no textuales en estructuras de datos optimizadas que admiten consultas geográficas y numéricas de alto rendimiento.
#### Para realizar esto se utiliza el lenguaje de consulta de estilo JSON de Elasticsearch ( Query DSL ). También hay otras herramientas como los controladores JBDC y ODBC para obtener una amplia gama de aplicaciones de terceros que interactúen con Elasticsearch a través de SQL.

## Analizar datos

#### Las agregaciones de Elasticsearch le permiten crear resúmenes complejos de sus datos y obtener información sobre métricas, patrones y tendencias importantes. Permite responder preguntas más específicas y sútiles que aporten más información a lo que se está buscando. Las agregaciones son muy rápidas, ya que también utilizan las mismas estructuras de datos que la búsqueda, lo que permite ver y analizar datos en tiempo real.
#### Además, también funcionan con las solicitudes de búsqueda, por lo que se puede buscar documentos, filtrar información y realizar un análisis de los datos al mismo tiempo en una misma solicitud.
#### Y aún hay más cosas que se pueden hacer. Se puede emplear machine learning para crear líneas de base precisas sobre el comportamiento normal de los datos e identificar patrones anómalos, como anomalías en los datos o estadísticas y comportamientos inusuales de un miembro de una población.

---
# Escabilidad y Resiliencia

#### En Elasticsearch se pueden añadir servidores (nodos) en un clúster para aumentar la capacidad y también distribuye de forma automática la carga de datos y consultas en los nodos disponibles. Elasticsearch sabe como balancear los clústeres que tienen varios nodos y como proporcionar alta disponibilidad.
#### Esto funciona gracias a los fragmentos (shards) de Elasticsearch. Un índice es realmente una agrupación lógica de uno o más fragmentos físicos, donde cada fragmento es un índice autónomo. Elasticsearch garantiza redundancia mediante la distribución de los documentos en un índice en varios fragmentos y esos fragmentos en varios nodos. Esto protege contra las fallas de hardware y aumenta la capacidad de consulta.
#### Hay dos tipos de fragmentos: Primarios y réplicas. Las réplicas son copias de un fragmento primario y tienen la función de proteger contra fallas de hardware y aumentar la capacidad para atender solicitudes de lectura, como buscar o recuperar un documento. La cantidad de fragmentos primarios se definen al crear in índice, mientras que la cantidad de fragmentos réplica se puede cambiar en cualquier momento.

## El rendimiento es importante...
#### Para garantizar un buen rendimiento hay que tener en cuenta el tamaño y cantidad de fragmentos a usar, lo cual va a depender. En general, es bueno seguir el siguiente punto de partida:
- Tratar de mantener el tamaño del fragmento rpomedio entre unos pocos GB y algunas decenas del GB.
- Como regla general, la cantidad de fragmentos por GB de espacio de almacenamiento dinámico debe ser inferior a 20.
    Importante: La mejor manera de determinar la configuración óptima para un caso de uso es realizando pruebas con los datos y consultas propias.

## En caso de desastre
#### Es importante que los nodos tengan buenas conexiones entre ellos, por lo que se colocan en el mismo centro de datos o cerca, pero para mantener una alta disponibilidad, también se debe evitar cualquier punto único de falla, puesto que en caso de fallo, los servidores localizados en otro lado deben poder tomar el lugar. Para esto se usa la replicación entre clústeres (Cross-Cluster Replication), que permite sincronizar automáticamente los índices de su clúster principal con un clúster remoto secundario que puede servir como copia de seguridad. También se puede crear clústeres secundarios para atender solicitudes de lectura en proximidad geográfica a sus usuarios.

## Cuidado y alimentación
#### Algunas herramientas para asegurar, administrar y monitorear los clústeres de Elasticsearch son Kibana (centro de control) y funciones como los resúmenes de datos y la administración del ciclo de vida de los índices para administrar los datos de forma inteligente.

##### Resumen de [¿What is Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/elasticsearch-intro.html)


