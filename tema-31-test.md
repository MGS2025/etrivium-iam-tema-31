# Tema 31 — Test de Autoevaluación

> **Título**: Paradigmas de computación distribuida y Servicios en Cloud. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.
> **Formato**: 60 preguntas tipo test A/B/C (formato oficial oposición)
> **Nivel**: C1 — Técnico Auxiliar TIC, Ayuntamiento de Madrid
> **Versión**: v1.0 — Pendiente validación
> **Fecha**: 2026-08-26
> **Fuentes**: ver tema-31-fuentes.md

---

## Instrucciones

- Cada pregunta tiene **3 opciones** (A, B, C). Solo una es correcta.
- Penalización en examen real: respuesta incorrecta descuenta **1/3** del valor de una correcta.
- Tiempo orientativo: 1 minuto por pregunta.
- Distribución: Fundamentos de la computación distribuida (P1-P15), Conceptos fundamentales de Cloud Computing (P16-P26), Modelos de servicio IaaS, PaaS, SaaS y XaaS (P27-P40), Modelos de despliegue (P41-P50), Cloud Computing en la Administración Pública (P51-P60).

---

### Pregunta 1

**¿Cuáles son las dos ausencias que definen técnicamente a un sistema distribuido?**

A) La ausencia de red de comunicaciones y la ausencia de sistema operativo común
B) La ausencia de memoria compartida y la ausencia de un reloj global
C) La ausencia de virtualización y la ausencia de un servidor central

<details><summary>Respuesta</summary>

**Correcta: B) La ausencia de memoria compartida y la ausencia de un reloj global** De ambas se deriva que el único mecanismo de coordinación posible sea el paso de mensajes, y con él la imposibilidad de distinguir con certeza un nodo caído de un nodo lento.

*Referencia: §1.1.1 [TANENBAUM]*
</details>

---

### Pregunta 2

**¿Cuántas transparencias de distribución define el modelo de referencia RM-ODP?**

A) Ocho: acceso, ubicación, migración, reubicación, replicación, concurrencia, fallo y persistencia
B) Cinco: acceso, ubicación, replicación, fallo y persistencia
C) Doce, agrupadas en cuatro bloques de tres

<details><summary>Respuesta</summary>

**Correcta: A) Ocho: acceso, ubicación, migración, reubicación, replicación, concurrencia, fallo y persistencia** El objetivo de todas ellas es ocultar al usuario que el sistema está repartido, aunque la transparencia total no es un objetivo deseable en sí mismo.

*Referencia: §1.1.1 [RMODP]*
</details>

---

### Pregunta 3

**La transparencia que oculta que un recurso se está moviendo de ubicación mientras se está utilizando se denomina:**

A) De migración
B) De replicación
C) De reubicación

<details><summary>Respuesta</summary>

**Correcta: C) De reubicación** La transparencia de migración oculta que el recurso se ha movido; la de reubicación, que se mueve durante el uso sin interrumpirlo. Es la distinción que más se falla de las ocho.

*Referencia: §1.1.1 [RMODP]*
</details>

---

### Pregunta 4

**¿Cuál de las siguientes NO es una de las ocho falacias de la computación distribuida?**

A) El almacenamiento es infinito
B) El coste de transporte es cero
C) La red es homogénea

<details><summary>Respuesta</summary>

**Correcta: A) El almacenamiento es infinito** Las ocho falacias son: la red es fiable, la latencia es cero, el ancho de banda es infinito, la red es segura, la topología no cambia, hay un único administrador, el coste de transporte es cero y la red es homogénea.

*Referencia: §1.1.2 [DEUTSCH]*
</details>

---

### Pregunta 5

**Las tarifas de salida de datos que cobran los proveedores de nube pública son la materialización económica de una de las falacias. ¿De cuál?**

A) La red es fiable
B) El ancho de banda es infinito
C) El coste de transporte es cero

<details><summary>Respuesta</summary>

**Correcta: C) El coste de transporte es cero** Es la séptima falacia. Serializar y mover datos consume procesador y, en la nube pública, se factura por gigabyte, lo que penaliza precisamente la extracción masiva de información.

*Referencia: §1.1.2 [DEUTSCH] [DATAACT]*
</details>

---

### Pregunta 6

**En una arquitectura multicapa estricta, ¿cuál es la regla que la define?**

A) Que exista siempre un servidor de aplicaciones intermedio
B) Que cada capa solo se comunique con la capa contigua
C) Que la lógica de negocio resida en el cliente

<details><summary>Respuesta</summary>

**Correcta: B) Que cada capa solo se comunique con la capa contigua** Si la capa de presentación ataca directamente a la base de datos, hay tres capas dibujadas pero dos de verdad, y se pierden las ventajas de seguridad y de mantenimiento del modelo.

*Referencia: §1.2.1 [TANENBAUM]*
</details>

---

### Pregunta 7

**¿Cuál es la diferencia esencial entre una arquitectura SOA clásica y una arquitectura de microservicios?**

A) En SOA la lógica de integración vive en el bus y los datos suelen compartirse; en microservicios el canal es ligero y cada servicio posee su propia base de datos
B) En SOA los servicios son pequeños y en microservicios son grandes
C) SOA usa exclusivamente REST y los microservicios exclusivamente SOAP

<details><summary>Respuesta</summary>

**Correcta: A) En SOA la lógica de integración vive en el bus y los datos suelen compartirse; en microservicios el canal es ligero y cada servicio posee su propia base de datos** El corte no está en el tamaño, sino en dónde vive la lógica de integración y quién es dueño de los datos.

*Referencia: §1.2.2 [FOWLER]*
</details>

---

### Pregunta 8

**El principio de los microservicios que se enuncia como «tuberías tontas y extremos listos» significa que:**

A) Los mensajes deben ir siempre sin cifrar para reducir la latencia
B) Cada servicio debe delegar su lógica en el orquestador central
C) La lógica reside en los servicios y el canal de comunicación no la contiene

<details><summary>Respuesta</summary>

**Correcta: C) La lógica reside en los servicios y el canal de comunicación no la contiene** Es la reacción directa al bus de servicios empresarial de la SOA clásica, que acumulaba lógica de negocio y se convertía en punto único de fallo y cuello de botella organizativo.

*Referencia: §1.2.2 [FOWLER]*
</details>

---

### Pregunta 9

**En una red entre pares (P2P), ¿qué mecanismo permite localizar un recurso sin índice central y sin inundar la red de consultas?**

A) El bus de servicios empresarial
B) La tabla hash distribuida (DHT)
C) El registro UDDI

<details><summary>Respuesta</summary>

**Correcta: B) La tabla hash distribuida (DHT)** La clave del recurso determina matemáticamente qué nodo es responsable de él, de modo que la búsqueda converge en un número de saltos del orden del logaritmo del número de nodos.

*Referencia: §1.2.3 [DHT]*
</details>

---

### Pregunta 10

**En una arquitectura dirigida por eventos, ¿cuál es la diferencia entre un comando y un evento?**

A) El comando se dirige a un destinatario concreto y espera que se ejecute; el evento es un hecho ya ocurrido, no va dirigido a nadie en particular y no espera nada
B) El comando es asíncrono y el evento es siempre síncrono
C) El comando lo emite el intermediario y el evento lo emite el consumidor

<details><summary>Respuesta</summary>

**Correcta: A) El comando se dirige a un destinatario concreto y espera que se ejecute; el evento es un hecho ya ocurrido, no va dirigido a nadie en particular y no espera nada** Confundirlos es el error de diseño más frecuente en arquitecturas dirigidas por eventos.

*Referencia: §1.2.3 [TANENBAUM]*
</details>

---

### Pregunta 11

**¿Cuál de los siguientes métodos HTTP NO es idempotente?**

A) PUT
B) DELETE
C) POST

<details><summary>Respuesta</summary>

**Correcta: C) POST** Dos peticiones POST idénticas crean dos recursos. Por eso, cuando una creación puede reintentarse tras un tiempo de espera, se envía una clave de idempotencia que permite al servidor detectar el reintento.

*Referencia: §1.3.1 [RFC7231]*
</details>

---

### Pregunta 12

**La garantía de entrega de mensajes más habitual en sistemas asíncronos es «al menos una vez». ¿Qué obligación impone al consumidor?**

A) Confirmar la recepción antes de procesar el mensaje
B) Ser idempotente, de modo que procesar dos veces el mismo mensaje no produzca dos altas
C) Ordenar los mensajes por marca de tiempo antes de procesarlos

<details><summary>Respuesta</summary>

**Correcta: B) Ser idempotente, de modo que procesar dos veces el mismo mensaje no produzca dos altas** La garantía «al menos una vez» no pierde mensajes, pero puede duplicarlos; el mecanismo habitual de protección es una clave de desduplicación almacenada por el consumidor.

*Referencia: §1.3.2 [AMQP]*
</details>

---

### Pregunta 13

**Según el enunciado correcto del teorema CAP, ¿qué elección se plantea?**

A) Cuando se produce una partición de red, hay que elegir entre consistencia y disponibilidad
B) Hay que elegir dos de las tres propiedades en cualquier circunstancia
C) Hay que elegir entre consistencia y tolerancia a particiones, siendo la disponibilidad siempre obligatoria

<details><summary>Respuesta</summary>

**Correcta: A) Cuando se produce una partición de red, hay que elegir entre consistencia y disponibilidad** En un sistema distribuido real las particiones ocurren, de modo que la tolerancia a particiones no es opcional. Cuando no hay partición, se pueden tener consistencia y disponibilidad a la vez.

*Referencia: §1.3.3 [BREWER]*
</details>

---

### Pregunta 14

**Los algoritmos de consenso como Paxos o Raft exigen quórum de mayoría. ¿Qué consecuencia práctica tiene sobre el número de nodos del despliegue?**

A) Que conviene desplegar un número par de nodos para repartir la carga
B) Que se despliega un número impar de nodos, porque añadir un cuarto a un grupo de tres no aumenta la tolerancia a fallos
C) Que basta con dos nodos si ambos están en zonas de disponibilidad distintas

<details><summary>Respuesta</summary>

**Correcta: B) Que se despliega un número impar de nodos, porque añadir un cuarto a un grupo de tres no aumenta la tolerancia a fallos** Con N nodos se toleran (N-1)/2 caídas: con tres nodos, una; con cinco, dos.

*Referencia: §1.3.3 [BREWER]*
</details>

---

### Pregunta 15

**El modelo BASE, característico de muchos sistemas distribuidos NoSQL, se caracteriza frente a ACID por:**

A) Garantizar atomicidad y aislamiento a costa de la disponibilidad
B) Exigir bloqueo pesimista en todas las operaciones de escritura
C) Priorizar la disponibilidad y admitir consistencia eventual

<details><summary>Respuesta</summary>

**Correcta: C) Priorizar la disponibilidad y admitir consistencia eventual** BASE responde a las iniciales de básicamente disponible, estado blando y consistencia eventual, y está pensado para escalar horizontalmente, a diferencia de ACID.

*Referencia: §1.3.3 [DYNAMO]*
</details>

---

### Pregunta 16

**¿Cuántas características esenciales, modelos de servicio y modelos de despliegue define el NIST en la SP 800-145?**

A) Cinco características, tres modelos de servicio y cuatro modelos de despliegue
B) Seis características, cuatro modelos de servicio y tres modelos de despliegue
C) Cuatro características, tres modelos de servicio y tres modelos de despliegue

<details><summary>Respuesta</summary>

**Correcta: A) Cinco características, tres modelos de servicio y cuatro modelos de despliegue** Es la cifra que hay que llevar grabada: 5-3-4. La definición se publicó en septiembre de 2011 y no ha sido sustituida.

*Referencia: §2.1.1 [NIST145]*
</details>

---

### Pregunta 17

**Un centro de datos municipal está completamente virtualizado, pero para crear una máquina hay que abrir un tique y esperar a que un técnico la cree a mano. ¿Es una nube privada según el NIST?**

A) Sí, porque la infraestructura es de uso exclusivo de una organización
B) No, porque falta el autoservicio bajo demanda y la medición del consumo, y sin todas las características esenciales no es nube
C) Sí, siempre que exista un hipervisor de tipo 1

<details><summary>Respuesta</summary>

**Correcta: B) No, porque falta el autoservicio bajo demanda y la medición del consumo, y sin todas las características esenciales no es nube** Las cinco características del NIST son acumulativas y necesarias. Un centro de datos virtualizado sin autoservicio ni medición es virtualización, no nube: la virtualización es un habilitador, no un sinónimo.

*Referencia: §2.1.1 [NIST145]*
</details>

---

### Pregunta 18

**¿Qué característica añade la norma ISO/IEC 17788, equivalente a la Recomendación ITU-T Y.3500, respecto de las cinco del NIST?**

A) La portabilidad de aplicaciones
B) La interoperabilidad semántica
C) La multitenencia

<details><summary>Respuesta</summary>

**Correcta: C) La multitenencia** ISO la hace explícita como sexta característica clave, mientras que el NIST la considera implícita dentro de la agrupación de recursos.

*Referencia: §2.1.2 [ISO17788]*
</details>

---

### Pregunta 19

**Los cinco actores de la arquitectura de referencia del NIST (SP 500-292) son:**

A) Consumidor, proveedor, desarrollador, integrador y auditor
B) Consumidor, proveedor, intermediario, portador y auditor
C) Cliente, encargado, subencargado, responsable y autoridad de control

<details><summary>Respuesta</summary>

**Correcta: B) Consumidor, proveedor, intermediario, portador y auditor** El portador aporta la conectividad y el auditor la evaluación independiente; son los dos que se olvidan con más frecuencia. El intermediario puede ser de intermediación, de agregación o de arbitraje.

*Referencia: §2.1.2 [NIST292]*
</details>

---

### Pregunta 20

**¿Cuál es la diferencia principal entre la computación en malla (grid) y la computación en la nube?**

A) La malla federa recursos de varias organizaciones con motivación colaborativa y aprovisionamiento planificado; la nube es normalmente de un proveedor, con pago por uso y aprovisionamiento bajo demanda
B) La malla utiliza virtualización y la nube no
C) La malla es siempre pública y la nube es siempre privada

<details><summary>Respuesta</summary>

**Correcta: A) La malla federa recursos de varias organizaciones con motivación colaborativa y aprovisionamiento planificado; la nube es normalmente de un proveedor, con pago por uso y aprovisionamiento bajo demanda** La nube heredó de la malla la idea de federación, pero cambió el modelo económico y el tiempo de aprovisionamiento.

*Referencia: §2.2 [NIST145]*
</details>
---

### Pregunta 21

**¿Cuál es la diferencia esencial entre una máquina virtual y un contenedor?**

A) La máquina virtual comparte el núcleo del anfitrión y el contenedor no
B) El contenedor solo puede ejecutar aplicaciones escritas en lenguajes interpretados
C) La máquina virtual virtualiza el hardware y lleva su propio sistema operativo; el contenedor virtualiza el sistema operativo y comparte el núcleo del anfitrión

<details><summary>Respuesta</summary>

**Correcta: C) La máquina virtual virtualiza el hardware y lleva su propio sistema operativo; el contenedor virtualiza el sistema operativo y comparte el núcleo del anfitrión** De ahí que el contenedor arranque en segundos y ofrezca mucha más densidad, pero con un aislamiento menor.

*Referencia: §2.3.1 [OCI]*
</details>

---

### Pregunta 22

**¿Qué organización define las especificaciones abiertas de imagen, ejecución y distribución de contenedores que sustentan su portabilidad entre nubes?**

A) La Open Container Initiative (OCI)
B) El Instituto Nacional de Estándares y Tecnología (NIST)
C) La Unión Internacional de Telecomunicaciones (UIT)

<details><summary>Respuesta</summary>

**Correcta: A) La Open Container Initiative (OCI)** Gracias a esa normalización, una imagen construida en un entorno se ejecuta igual en otro, lo que convierte a los contenedores en el mecanismo de portabilidad entre nubes más eficaz disponible.

*Referencia: §2.3.1 [OCI]*
</details>

---

### Pregunta 23

**En infraestructura como código, ¿qué significa que una descripción sea idempotente?**

A) Que puede escribirse en cualquier lenguaje de programación
B) Que aplicarla varias veces deja el sistema en el mismo estado que aplicarla una sola vez
C) Que solo puede aplicarse una vez y después queda bloqueada

<details><summary>Respuesta</summary>

**Correcta: B) Que aplicarla varias veces deja el sistema en el mismo estado que aplicarla una sola vez** Es lo que diferencia una herramienta declarativa de un guion imperativo, que al ejecutarse dos veces puede duplicar recursos o fallar.

*Referencia: §2.3.2 [TERRAFORM]*
</details>

---

### Pregunta 24

**El modelo declarativo de un orquestador de contenedores consiste en que:**

A) El operador ejecuta órdenes secuenciales de creación y destrucción de recursos
B) El orquestador ejecuta un guion al arrancar y no vuelve a intervenir
C) El operador describe el estado deseado y un bucle de control trabaja para que el estado real coincida con él

<details><summary>Respuesta</summary>

**Correcta: C) El operador describe el estado deseado y un bucle de control trabaja para que el estado real coincida con él** Ese cambio de mentalidad, de dar órdenes a declarar objetivos, es el que hace posible la autorreparación de instancias que dejan de responder.

*Referencia: §2.3.2 [K8S]*
</details>

---

### Pregunta 25

**Una unidad mantiene veinte instancias encendidas las veinticuatro horas del año en la nube pública porque «así no hay sorpresas». ¿Qué está ocurriendo?**

A) Está pagando el precio de la nube sin ejercer la elasticidad, lo que puede salir más caro que comprar los servidores
B) Está aplicando correctamente el principio de alta disponibilidad
C) Está aprovechando el descuento por instancias de capacidad sobrante

<details><summary>Respuesta</summary>

**Correcta: A) Está pagando el precio de la nube sin ejercer la elasticidad, lo que puede salir más caro que comprar los servidores** El ahorro no lo produce la nube, sino la elasticidad efectivamente ejercida: una instancia encendida permanentemente no es elástica.

*Referencia: §2.4 [FINOPS]*
</details>

---

### Pregunta 26

**¿Cuál es la causa dominante de los incidentes de seguridad en nube pública?**

A) La vulneración del hipervisor del proveedor
B) La configuración incorrecta por parte del cliente
C) El fallo del aislamiento entre inquilinos

<details><summary>Respuesta</summary>

**Correcta: B) La configuración incorrecta por parte del cliente** Almacenamiento de objetos expuesto públicamente, credenciales incrustadas en el código y permisos excesivos concentran la mayor parte de los incidentes reales publicados.

*Referencia: §2.4, §4.1.1 [NIST144]*
</details>

---

### Pregunta 27

**Según la definición del NIST, en el modelo IaaS el consumidor:**

A) No controla el sistema operativo, que gestiona el proveedor
B) Controla la infraestructura de nube subyacente, incluidos los servidores físicos
C) No gestiona la infraestructura subyacente, pero sí controla los sistemas operativos, el almacenamiento y las aplicaciones desplegadas

<details><summary>Respuesta</summary>

**Correcta: C) No gestiona la infraestructura subyacente, pero sí controla los sistemas operativos, el almacenamiento y las aplicaciones desplegadas** Puede además tener un control limitado de componentes de red seleccionados, como los cortafuegos de anfitrión.

*Referencia: §3.1.1 [NIST145]*
</details>

---

### Pregunta 28

**Un servicio municipal debe conservar durante años la documentación que la ciudadanía adjunta a sus solicitudes: ficheros que se escriben una vez, se leen pocas y crecen sin límite previsible. ¿Qué tipo de almacenamiento es el adecuado?**

A) Almacenamiento de bloques conectado a cada instancia
B) Almacenamiento de objetos, accesible por HTTP y con clases de acceso según la frecuencia de uso
C) Almacenamiento de ficheros compartido por NFS entre todas las instancias

<details><summary>Respuesta</summary>

**Correcta: B) Almacenamiento de objetos, accesible por HTTP y con clases de acceso según la frecuencia de uso** Es prácticamente ilimitado, barato y muy durable. Su limitación es que un objeto no se modifica parcialmente, sino que se sustituye entero, y que su latencia es mayor.

*Referencia: §3.1.1 [CSP]*
</details>

---

### Pregunta 29

**¿Qué distingue la elasticidad de la escalabilidad?**

A) Que la elasticidad exige automatismo, rapidez y bidireccionalidad, es decir, también decrecer
B) Que la escalabilidad solo se aplica al almacenamiento y la elasticidad al cómputo
C) Que la escalabilidad es una característica esencial del NIST y la elasticidad no

<details><summary>Respuesta</summary>

**Correcta: A) Que la elasticidad exige automatismo, rapidez y bidireccionalidad, es decir, también decrecer** Un sistema puede ser escalable —se le pueden añadir servidores— y nada elástico, si hacerlo requiere intervención manual y semanas de plazo.

*Referencia: §3.1.2 [NIST145]*
</details>

---

### Pregunta 30

**Para que el escalado horizontal funcione, ¿qué condición debe cumplir la aplicación?**

A) Debe ejecutarse sobre un hipervisor de tipo 1
B) Debe estar escrita en un lenguaje compilado
C) Debe ser sin estado, de modo que la sesión del usuario no viva en la memoria de una instancia concreta

<details><summary>Respuesta</summary>

**Correcta: C) Debe ser sin estado, de modo que la sesión del usuario no viva en la memoria de una instancia concreta** Si la sesión reside en la memoria local, no se pueden añadir ni retirar instancias sin romper sesiones de usuario en curso.

*Referencia: §3.1.2 [12FACTOR]*
</details>

---

### Pregunta 31

**Ante una carga con una base estable conocida y picos puntuales, la combinación de facturación más eficiente es:**

A) Cubrir la base con instancias reservadas y el pico con instancias bajo demanda
B) Cubrir todo con instancias de capacidad sobrante para maximizar el descuento
C) Cubrir todo con instancias reservadas a tres años

<details><summary>Respuesta</summary>

**Correcta: A) Cubrir la base con instancias reservadas y el pico con instancias bajo demanda** Las instancias de capacidad sobrante pueden ser retiradas por el proveedor con poco aviso, por lo que solo son adecuadas para procesos por lotes tolerantes a la interrupción.

*Referencia: §3.1.2 [FINOPS]*
</details>

---

### Pregunta 32

**Según la definición del NIST, en el modelo PaaS el consumidor no gestiona ni controla:**

A) Las aplicaciones que él mismo despliega
B) Los ajustes de configuración del entorno de alojamiento
C) La red, los servidores, los sistemas operativos ni el almacenamiento

<details><summary>Respuesta</summary>

**Correcta: C) La red, los servidores, los sistemas operativos ni el almacenamiento** Sí controla, en cambio, las aplicaciones desplegadas y, posiblemente, los ajustes de configuración del entorno de alojamiento.

*Referencia: §3.2.1 [NIST145]*
</details>

---

### Pregunta 33

**¿Cuál es la frontera exacta que separa IaaS de PaaS?**

A) La virtualización: en PaaS la pone el proveedor y en IaaS el cliente
B) El sistema operativo: en IaaS lo administra el cliente y en PaaS lo administra el proveedor
C) Los datos: en PaaS pasan a ser responsabilidad del proveedor

<details><summary>Respuesta</summary>

**Correcta: B) El sistema operativo: en IaaS lo administra el cliente y en PaaS lo administra el proveedor** La virtualización marca la frontera entre el modelo local y IaaS. Los datos y las identidades no cambian de dueño en ningún modelo: son siempre del cliente.

*Referencia: §3, §3.2.1 [NIST145] [CCN823]*
</details>

---

### Pregunta 34

**El término «sin servidor» (serverless) significa que:**

A) El cliente no ve, no dimensiona ni paga los servidores cuando no se usan, aunque los servidores siguen existiendo
B) La aplicación se ejecuta directamente en el navegador del usuario, sin ningún servidor implicado
C) El proveedor entrega servidores físicos sin hipervisor

<details><summary>Respuesta</summary>

**Correcta: A) El cliente no ve, no dimensiona ni paga los servidores cuando no se usan, aunque los servidores siguen existiendo** Se factura por número de invocaciones y tiempo de ejecución, y su contrapartida técnica es el arranque en frío tras un periodo de inactividad.

*Referencia: §3.2.1 [CSP]*
</details>

---

### Pregunta 35

**Al contratar una base de datos gestionada, ¿qué sigue siendo responsabilidad del cliente?**

A) Aplicar los parches del motor de base de datos y sustituir los discos averiados
B) El esquema, las consultas, los índices, los datos y la política de retención y restauración
C) La replicación entre zonas de disponibilidad y la programación de las copias

<details><summary>Respuesta</summary>

**Correcta: B) El esquema, las consultas, los índices, los datos y la política de retención y restauración** El servicio hace copias, pero no impide que el cliente borre una tabla: la recuperación ante un error propio exige haber definido la retención y haber probado la restauración.

*Referencia: §3.2.2 [CCN823]*
</details>

---

### Pregunta 36

**¿Cuál es la consecuencia menos intuitiva y más relevante del modelo SaaS para una Administración?**

A) Que los datos se cifran obligatoriamente en origen
B) Que el proveedor asume la responsabilidad jurídica sobre los datos personales
C) Que el cliente pierde el control del calendario de versiones, porque el proveedor actualiza para todos a la vez

<details><summary>Respuesta</summary>

**Correcta: C) Que el cliente pierde el control del calendario de versiones, porque el proveedor actualiza para todos a la vez** Por eso los contratos deben incluir preaviso de cambios funcionales, entornos de prueba y compromisos de compatibilidad de las integraciones.

*Referencia: §3.3.1 [SAAS-SUITES]*
</details>

---

### Pregunta 37

**En un producto SaaS, la adaptación a las necesidades de una organización cliente se realiza normalmente:**

A) Por configuración y por extensión mediante API o complementos, no por modificación del código
B) Por modificación del código fuente del producto para ese cliente concreto
C) Por sustitución de la base de datos del producto por una propia

<details><summary>Respuesta</summary>

**Correcta: A) Por configuración y por extensión mediante API o complementos, no por modificación del código** El código es común a todos los inquilinos. Cuando el producto no contempla un comportamiento exigido, las salidas son adaptar el procedimiento, pagar un desarrollo que el fabricante incorpore al producto estándar o construir una capa propia alrededor.

*Referencia: §3.3.1 [SAAS-SUITES]*
</details>

---

### Pregunta 38

**En un modelo de multitenencia agrupado (pooled), ¿de qué depende enteramente el aislamiento entre organizaciones?**

A) Del hipervisor del proveedor
B) Del cifrado del canal de comunicaciones
C) Del software, ya que basta con que una consulta olvide filtrar por el identificador de inquilino para exponer datos de otra organización

<details><summary>Respuesta</summary>

**Correcta: C) Del software, ya que basta con que una consulta olvide filtrar por el identificador de inquilino para exponer datos de otra organización** Por eso el filtrado se aplica en la capa de acceso a datos o mediante seguridad a nivel de fila, y no se confía en que cada consulta lo recuerde.

*Referencia: §3.3.2 [ISO17788]*
</details>

---

### Pregunta 39

**Una herramienta en modo SaaS para 3.000 empleados de los que solo 400 la usan a diario. ¿Qué modalidad de licenciamiento ajusta mejor el coste al uso real?**

A) Por usuario nombrado, dando de alta a los 3.000
B) Por usuario concurrente o por transacción realizada
C) Por niveles de funcionalidad, contratando el nivel superior para todos

<details><summary>Respuesta</summary>

**Correcta: B) Por usuario concurrente o por transacción realizada** La elección del modelo de licencia puede mover el precio del contrato en un factor de cinco sin cambiar ni una línea del pliego técnico.

*Referencia: §3.3.2 [LCSP]*
</details>

---

### Pregunta 40

**Respecto de las denominaciones XaaS (FaaS, CaaS, DBaaS, DaaS…), es correcto afirmar que:**

A) No son categorías del NIST, que reconoce únicamente tres modelos de servicio, y algunas siglas son ambiguas, como CaaS y DaaS
B) Sustituyen a los tres modelos clásicos desde la revisión de la SP 800-145
C) Son las siete categorías de servicio que define en exclusiva la norma ISO/IEC 17789

<details><summary>Respuesta</summary>

**Correcta: A) No son categorías del NIST, que reconoce únicamente tres modelos de servicio, y algunas siglas son ambiguas, como CaaS y DaaS** CaaS puede significar contenedores o comunicaciones, y DaaS escritorio o datos: en una respuesta escrita conviene desarrollar siempre la sigla.

*Referencia: §3.4 [NIST145] [ISO17788]*
</details>
---

### Pregunta 41

**Respecto de la nube privada, según la definición del NIST:**

A) Debe estar necesariamente en las instalaciones de la organización que la usa
B) Puede ser propiedad de un tercero, estar operada por un tercero y ubicarse fuera de las instalaciones; lo que la define es que su uso está reservado a una sola organización
C) Solo puede ofrecer el modelo de servicio IaaS

<details><summary>Respuesta</summary>

**Correcta: B) Puede ser propiedad de un tercero, estar operada por un tercero y ubicarse fuera de las instalaciones; lo que la define es que su uso está reservado a una sola organización** Es el error más frecuente al estudiar los modelos de despliegue: privada no significa «en casa».

*Referencia: §4.2.1 [NIST145]*
</details>

---

### Pregunta 42

**Dentro de la organización geográfica de una nube pública, ¿qué es una zona de disponibilidad?**

A) El área geográfica completa en la que el proveedor ofrece servicio
B) Un segmento lógico de red aislado dentro de la red virtual del cliente
C) Un centro de datos independiente en energía, refrigeración y red, unido a los demás de su región por enlaces de baja latencia

<details><summary>Respuesta</summary>

**Correcta: C) Un centro de datos independiente en energía, refrigeración y red, unido a los demás de su región por enlaces de baja latencia** Repartir réplicas entre zonas protege frente a la caída de un edificio; repartirlas entre regiones protege frente a una catástrofe territorial, a costa de latencia y de posibles restricciones jurídicas.

*Referencia: §1.3.3, §4.1 [CSP]*
</details>

---

### Pregunta 43

**En el modelo de responsabilidad compartida, ¿qué conserva siempre el cliente, sea cual sea el modelo de servicio?**

A) Los datos y su clasificación, la gestión de identidades y accesos, y la configuración de los servicios contratados
B) El sistema operativo, el middleware y el hipervisor
C) La red troncal, el hardware y el aislamiento entre inquilinos

<details><summary>Respuesta</summary>

**Correcta: A) Los datos y su clasificación, la gestión de identidades y accesos, y la configuración de los servicios contratados** La línea que separa las responsabilidades se desplaza con el modelo de servicio, pero estas tres nunca cambian de dueño.

*Referencia: §4.1.1 [CCN823]*
</details>

---

### Pregunta 44

**Un almacén de objetos con documentación ciudadana queda accesible públicamente por una política de acceso mal fijada por el personal del Ayuntamiento. ¿Quién ha incumplido?**

A) El proveedor, por no impedir configuraciones inseguras
B) El Ayuntamiento, porque la configuración de los servicios es responsabilidad del cliente, y estaría además ante una brecha de datos personales notificable en 72 horas
C) Ninguno de los dos, al tratarse de un riesgo inherente a la nube pública

<details><summary>Respuesta</summary>

**Correcta: B) El Ayuntamiento, porque la configuración de los servicios es responsabilidad del cliente, y estaría además ante una brecha de datos personales notificable en 72 horas** Es el ejemplo que mejor explica por qué el reparto de responsabilidades debe constar en el pliego y no darse por supuesto.

*Referencia: §4.1.1 [RGPD] [CCN823]*
</details>

---

### Pregunta 45

**Una nube privada virtual (VPC) contratada dentro de un proveedor de nube pública:**

A) Convierte el despliegue en una nube privada según el NIST
B) Equivale a una nube comunitaria si la usan varias Administraciones
C) Sigue siendo un despliegue en nube pública, porque el hardware continúa siendo compartido y el aislamiento es solo lógico

<details><summary>Respuesta</summary>

**Correcta: C) Sigue siendo un despliegue en nube pública, porque el hardware continúa siendo compartido y el aislamiento es solo lógico** La palabra clave del NIST para «privada» es el uso exclusivo de la infraestructura por una sola organización, no el aislamiento lógico de la red.

*Referencia: §4.2.1 [NIST145]*
</details>

---

### Pregunta 46

**Según el NIST, en una nube híbrida las infraestructuras que se combinan:**

A) Se fusionan en una única infraestructura gestionada por el proveedor principal
B) Permanecen como entidades separadas y distintas, unidas por una tecnología que permite la portabilidad de datos y aplicaciones
C) Deben ser necesariamente una privada y una comunitaria

<details><summary>Respuesta</summary>

**Correcta: B) Permanecen como entidades separadas y distintas, unidas por una tecnología que permite la portabilidad de datos y aplicaciones** La definición es exigente: no basta con tener cosas en dos sitios, hace falta un mecanismo real de portabilidad.

*Referencia: §4.3.1 [NIST145]*
</details>

---

### Pregunta 47

**El patrón híbrido consistente en atender la carga base en la nube privada y desbordar el pico a la nube pública se denomina:**

A) Desbordamiento a la nube (cloud bursting)
B) Reparto por sensibilidad
C) Modernización progresiva

<details><summary>Respuesta</summary>

**Correcta: A) Desbordamiento a la nube (cloud bursting)** Es el patrón adecuado para campañas, matrículas y plazos de presentación, donde la demanda se concentra en unas horas concretas del año.

*Referencia: §4.3.1 [NIST145]*
</details>

---

### Pregunta 48

**¿Qué distingue la interoperabilidad de la portabilidad según la norma ISO/IEC 19941?**

A) La interoperabilidad se refiere a los datos y la portabilidad a las personas usuarias
B) Son términos sinónimos en el ámbito de la computación en la nube
C) Interoperar es intercambiar información y usarla estando cada sistema en su sitio; portar es mover datos o aplicaciones de un entorno a otro

<details><summary>Respuesta</summary>

**Correcta: C) Interoperar es intercambiar información y usarla estando cada sistema en su sitio; portar es mover datos o aplicaciones de un entorno a otro** Un sistema puede ser perfectamente interoperable y absolutamente imposible de portar: esa es justamente la situación que produce la dependencia del proveedor.

*Referencia: §4.3.1 [ISO19941]*
</details>

---

### Pregunta 49

**La nube comunitaria se define como infraestructura aprovisionada para:**

A) El público general, con acceso abierto y pago por uso
B) El uso exclusivo de una comunidad específica de organizaciones que comparten intereses, misión, requisitos de seguridad o cumplimiento
C) Un consorcio de proveedores privados que reparten entre sí la capacidad sobrante

<details><summary>Respuesta</summary>

**Correcta: B) El uso exclusivo de una comunidad específica de organizaciones que comparten intereses, misión, requisitos de seguridad o cumplimiento** Es el modelo naturalmente adecuado al sector público y comparte la lógica de los servicios compartidos de la Administración; su dificultad principal es de gobernanza.

*Referencia: §4.4 [NIST145]*
</details>

---

### Pregunta 50

**¿En qué se diferencian una estrategia multicloud y un despliegue de nube híbrida?**

A) Multicloud es usar varios proveedores del mismo tipo, normalmente varias nubes públicas; híbrida es combinar modelos de despliegue distintos unidos por portabilidad
B) Multicloud es un modelo de despliegue del NIST e híbrida no
C) Multicloud exige que la misma carga se ejecute simultáneamente en todos los proveedores

<details><summary>Respuesta</summary>

**Correcta: A) Multicloud es usar varios proveedores del mismo tipo, normalmente varias nubes públicas; híbrida es combinar modelos de despliegue distintos unidos por portabilidad** Ambas pueden darse a la vez: una entidad con nube privada propia y servicios en dos proveedores públicos es híbrida y multicloud simultáneamente.

*Referencia: §4.4 [NIST145] [ESTRATEGIA-CLOUD]*
</details>

---

### Pregunta 51

**¿Qué precepto obliga a que una empresa privada que presta servicios en la nube a una entidad del sector público cumpla el Esquema Nacional de Seguridad?**

A) El artículo 31 del Real Decreto 311/2022
B) El artículo 40 del Real Decreto 311/2022
C) El artículo 2.3 del Real Decreto 311/2022

<details><summary>Respuesta</summary>

**Correcta: C) El artículo 2.3 del Real Decreto 311/2022** Extiende el ENS a los sistemas de información de las entidades del sector privado que, en virtud de una relación contractual, presten servicios o provean soluciones al sector público para el ejercicio de sus competencias y potestades administrativas, incluida la obligación de contar con política de seguridad.

*Referencia: §5.1.1 [ENS]*
</details>

---

### Pregunta 52

**Las categorías de seguridad del ENS y su determinación son:**

A) BÁSICA, MEDIA y ALTA, correspondiendo al sistema la categoría de la dimensión más exigente entre disponibilidad, integridad, confidencialidad, autenticidad y trazabilidad
B) BAJA, MEDIA y CRÍTICA, correspondiendo al sistema la media aritmética de las dimensiones
C) BÁSICA y ALTA únicamente, según haya o no datos personales

<details><summary>Respuesta</summary>

**Correcta: A) BÁSICA, MEDIA y ALTA, correspondiendo al sistema la categoría de la dimensión más exigente entre disponibilidad, integridad, confidencialidad, autenticidad y trazabilidad** Las categorías están en el artículo 40 y las cinco dimensiones, en el Anexo I. Esta valoración es el primer paso de cualquier proyecto de nube.

*Referencia: §5.1.1 [ENS]*
</details>

---

### Pregunta 53

**En el Anexo II del ENS, ¿qué grupo de medidas del marco operacional se refiere específicamente a los servicios en la nube?**

A) mp.info
B) op.nub
C) org.pro

<details><summary>Respuesta</summary>

**Correcta: B) op.nub** El grupo op.nub, con la medida op.nub.1 de protección de los servicios en la nube, es una novedad del Real Decreto 311/2022, igual que op.ext.3 (protección de la cadena de suministro) y op.ext.4 (interconexión de sistemas).

*Referencia: §5.1.1 [ENS]*
</details>

---

### Pregunta 54

**Respecto de la auditoría y la conformidad con el ENS:**

A) Todos los sistemas requieren certificación por entidad acreditada, con periodicidad anual
B) La auditoría es voluntaria salvo que se traten datos personales
C) La auditoría regular ordinaria se realiza al menos cada dos años, y los sistemas de categoría BÁSICA solo requieren autoevaluación para declarar la conformidad

<details><summary>Respuesta</summary>

**Correcta: C) La auditoría regular ordinaria se realiza al menos cada dos años, y los sistemas de categoría BÁSICA solo requieren autoevaluación para declarar la conformidad** Los de categoría MEDIA y ALTA requieren auditoría de certificación por entidad acreditada. La auditoría se regula en el artículo 31 y los procedimientos de conformidad en el artículo 38.

*Referencia: §5.1.1 [ENS]*
</details>

---

### Pregunta 55

**¿Cuál es la guía del Centro Criptológico Nacional específicamente dedicada a la utilización de servicios en la nube?**

A) La CCN-STIC 823
B) La CCN-STIC 105
C) La CCN-STIC 817

<details><summary>Respuesta</summary>

**Correcta: A) La CCN-STIC 823** Identifica las medidas y requisitos exigibles al proveedor y, sobre todo, cómo se reparten las responsabilidades entre cliente y proveedor según el modelo de servicio. La 105 publica el catálogo CPSTIC de productos y servicios y la 817 trata la gestión de ciberincidentes.

*Referencia: §5.1.1 [CCN823] [CCN105]*
</details>

---

### Pregunta 56

**Cuando una Administración contrata un servicio en la nube que trata datos personales, el proveedor tiene la condición de:**

A) Corresponsable del tratamiento, junto con la Administración
B) Encargado del tratamiento, conforme al artículo 28 del RGPD, siguiendo instrucciones documentadas del responsable
C) Tercero receptor de una cesión de datos, que requiere consentimiento de las personas afectadas

<details><summary>Respuesta</summary>

**Correcta: B) Encargado del tratamiento, conforme al artículo 28 del RGPD, siguiendo instrucciones documentadas del responsable** El contrato debe prohibir la subcontratación sin autorización y obligar a devolver o suprimir los datos, incluidas las copias, al finalizar la prestación. La Administración sigue siendo responsable del tratamiento.

*Referencia: §5.1.2 [RGPD]*
</details>

---

### Pregunta 57

**El personal de soporte de un proveedor, establecido en un tercer país, accede en remoto a datos alojados en servidores situados en la Unión Europea. Desde el punto de vista del RGPD, esto es:**

A) Un tratamiento interno que no requiere garantías adicionales, al no salir los datos de la Unión
B) Una comunicación de datos sometida únicamente al deber de confidencialidad
C) Una transferencia internacional de datos, sujeta al capítulo V del RGPD

<details><summary>Respuesta</summary>

**Correcta: C) Una transferencia internacional de datos, sujeta al capítulo V del RGPD** El acceso remoto desde un tercer país es transferencia aunque los servidores estén en la Unión. Tras la sentencia Schrems II hay que evaluar el marco jurídico del país de destino y adoptar medidas complementarias, siendo el cifrado con claves gestionadas exclusivamente por la Administración la más eficaz.

*Referencia: §5.1.2 [SCHREMSII] [RGPD]*
</details>

---

### Pregunta 58

**Conforme al Reglamento (UE) 2023/2854 (Reglamento de Datos), ¿desde qué fecha no podrán los proveedores imponer tarifas de cambio a sus clientes?**

A) Desde el 11 de enero de 2024
B) Desde el 12 de enero de 2027
C) Desde el 10 de julio de 2023

<details><summary>Respuesta</summary>

**Correcta: B) Desde el 12 de enero de 2027** Lo establece el artículo 29, dentro del capítulo VI (artículos 23 a 31) sobre cambio entre servicios de tratamiento de datos. Durante el periodo transitorio solo pueden repercutirse costes reducidos directamente vinculados al proceso de cambio.

*Referencia: §5.1.2 [DATAACT]*
</details>

---

### Pregunta 59

**La Estrategia de servicios en la nube híbrida para las Administraciones Públicas, de diciembre de 2022, se estructura en:**

A) Siete pilares y diecinueve iniciativas, con el principio de «nube híbrida primero»
B) Tres ejes y doce medidas, con el principio de «cloud first»
C) Cinco objetivos y veinte acciones, con el principio de «nube pública por defecto»

<details><summary>Respuesta</summary>

**Correcta: A) Siete pilares y diecinueve iniciativas, con el principio de «nube híbrida primero»** El principio español no es «cloud first» sin matices: prioriza el aprovisionamiento en la nube frente a las soluciones tradicionales, pero en un modelo híbrido que combina la nube privada de la Administración con proveedores externos.

*Referencia: §5.2.1 [ESTRATEGIA-CLOUD]*
</details>

---

### Pregunta 60

**¿Qué es NubeSARA?**

A) La red de interconexión de las Administraciones Públicas españolas
B) El catálogo de productos y servicios de seguridad cualificados por el Centro Criptológico Nacional
C) La solución de nube privada de la Administración General del Estado, desplegada por la Secretaría General de Administración Digital en 2015, con catálogo de servicios IaaS y PaaS con costes y acuerdos de nivel de servicio

<details><summary>Respuesta</summary>

**Correcta: C) La solución de nube privada de la Administración General del Estado, desplegada por la Secretaría General de Administración Digital en 2015, con catálogo de servicios IaaS y PaaS con costes y acuerdos de nivel de servicio** No debe confundirse la red SARA, que es la red de interconexión, con NubeSARA, que es la nube desplegada sobre ella y que evoluciona hacia una «tienda» de soluciones para todas las Administraciones.

*Referencia: §5.2.2 [ESTRATEGIA-CLOUD]*
</details>
