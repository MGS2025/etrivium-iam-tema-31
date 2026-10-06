# Tema 31 — Contenido Teórico

> **Título oficial**: Paradigmas de computación distribuida y Servicios en Cloud. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.
>
> **Bloque**: Parte II — Técnico
> **Nivel**: C1 — Técnico Auxiliar TIC, Ayuntamiento de Madrid
> **Versión**: v1.0 — Pendiente validación
> **Fecha generación**: 2026-08-26
> **Fuentes**: Ver tema-31-fuentes.md · **Diagramas**: Ver tema-31-diagramas.md · **Cambios**: Ver tema-31-changelog.md
>
> *Extensión: ~17.600 palabras · 17 diagramas SVG embebidos · 4 tipos de callout transversales*

---

## Convenciones del documento

Este tema incluye cuatro tipos de **cajas callout** para facilitar el estudio:

> **[DATO CLAVE]** Información de alta densidad memorística.

> **[EJERCICIO RESUELTO]** Problema + solución paso a paso (clasificación de un servicio, cálculo de un dimensionamiento o de un coste, elección de un modelo de despliegue).

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** Aplicación real de la teoría al entorno municipal (sede electrónica, padrón, callejero, portal de datos abiertos, tramitación de expedientes).

> **[RELACIÓN CON OTROS TEMAS]** Enlace conceptual a otros temas del temario oficial.

Este tema tiene una peculiaridad que conviene entender antes de empezar, porque explica su estructura: **la nube no es una tecnología, sino un modelo económico y de consumo montado encima de tecnologías anteriores**. Por eso el enunciado oficial empieza por los *paradigmas de computación distribuida* —que son lo que hay debajo— y solo después llega a los *servicios en cloud*. Estudiar únicamente las siglas IaaS, PaaS y SaaS deja fuera lo esencial, que es precisamente lo que la nube hereda de la computación distribuida: latencia, particiones de red, consistencia, acoplamiento y fallos parciales.

Hay una segunda advertencia. Este es, junto al Tema 24, el tema **más sensible a la obsolescencia** de toda la serie técnica: los nombres de producto y los catálogos de servicios de los proveedores cambian cada pocos meses. Por eso este tema se apoya deliberadamente en **definiciones normalizadas y estables** —NIST SP 800-145, ISO/IEC 17788, ENS— y trata los productos concretos como meros ejemplos. **Lo que importa es la definición, no la marca.** Las tres cifras que hay que llevar grabadas son las del NIST: **5 características esenciales, 3 modelos de servicio y 4 modelos de despliegue** [NIST145].

Las fuentes se citan con etiquetas breves tipo `[NIST145]` o `[ENS]`; el registro completo está en `tema-31-fuentes.md`.

**Caso de referencia usado en todo el tema** (contexto Ayuntamiento de Madrid, **supuesto simplificado**, no descripción de una infraestructura real): un servicio municipal debe publicar un **portal de cita previa y tramitación** para una campaña de ayudas que se abre un día concreto a las nueve de la mañana. Se esperan **decenas de miles de solicitudes en las primeras horas** y prácticamente ninguna el resto del año. El sistema debe consultar el **padrón** para validar el empadronamiento, guardar documentación aportada por la ciudadanía, integrarse con el **registro electrónico** y conservar la trazabilidad exigida por el Esquema Nacional de Seguridad. Este supuesto atraviesa todo el tema: en §1 obliga a decidir cómo se reparte el trabajo entre componentes, en §2 y §3 a elegir modelo de servicio, en §4 a elegir modelo de despliegue y en §5 a justificar la decisión ante el marco normativo.

---

## 1. Fundamentos de la computación distribuida

### 1.1. Concepto y características de los sistemas distribuidos

#### 1.1.1. Definición, objetivos y transparencias

La definición clásica y más citada es la de Tanenbaum: **un sistema distribuido es un conjunto de ordenadores independientes que se presenta a sus usuarios como un único sistema coherente** [TANENBAUM]. La definición tiene dos mitades y ambas son igual de importantes:

- **«Ordenadores independientes»**: cada nodo tiene su propio procesador, su propia memoria y su propio reloj. **No hay memoria compartida y no hay un reloj global**. La única forma de que dos nodos se coordinen es **enviarse mensajes**, y los mensajes tardan, se pierden, se duplican y llegan desordenados.
- **«Un único sistema coherente»**: el usuario no debe percibir que hay muchas máquinas. Esa ocultación es el trabajo del *middleware*, la capa de software que se sitúa entre el sistema operativo de cada nodo y las aplicaciones.

> **[DATO CLAVE]** Las dos ausencias que definen un sistema distribuido y explican toda su dificultad: **no hay memoria compartida** y **no hay reloj global**. De ahí se derivan el paso de mensajes como único mecanismo de coordinación, la imposibilidad de saber con certeza si un nodo ha caído o solo va lento, y la necesidad de relojes lógicos para ordenar eventos [TANENBAUM] [LAMPORT].

**Distribuido, paralelo y centralizado no son sinónimos.** La distinción es la siguiente:

| Sistema | Memoria | Reloj | Fallo de un componente | Objetivo principal |
|---|---|---|---|---|
| **Centralizado** | Compartida | Único | Cae **todo** el sistema | Simplicidad |
| **Paralelo** (multiprocesador) | Compartida o fuertemente acoplada | Común o sincronizado | Suele caer todo | **Velocidad** en un solo problema |
| **Distribuido** | **Privada de cada nodo** | **Uno por nodo** | **Fallo parcial**: el resto sigue | Escalabilidad, disponibilidad, compartición de recursos |

El **fallo parcial** es la característica que más cambia el diseño. En un sistema centralizado, o funciona todo o no funciona nada, y eso simplifica el razonamiento. En un sistema distribuido, una parte puede estar caída mientras el resto sigue trabajando **sin saberlo**, y el software debe estar escrito para ese escenario. La consecuencia práctica es que en un sistema distribuido **el manejo de errores deja de ser un caso excepcional y pasa a ser parte del caso normal**.

Los **objetivos** que justifican distribuir un sistema son cinco, y conviene recordarlos porque son los mismos que después reaparecerán como ventajas de la nube:

1. **Compartición de recursos**: aprovechar datos, capacidad de cálculo, almacenamiento o dispositivos que están físicamente en otro sitio.
2. **Transparencia de la distribución**: ocultar al usuario que el sistema está repartido (se desarrolla más abajo).
3. **Apertura** (*openness*): interfaces bien definidas, publicadas y neutrales respecto del fabricante, que permitan sustituir e interoperar componentes.
4. **Escalabilidad**: crecer en número de usuarios, en volumen de datos y en dispersión geográfica sin rehacer el sistema.
5. **Disponibilidad y tolerancia a fallos**: que la caída de una parte no arrastre al conjunto, mediante redundancia y replicación.

La **transparencia** merece detalle porque tiene un listado cerrado y memorizable. El modelo de referencia **RM-ODP** (ISO/IEC 10746 = ITU-T X.901) define **ocho transparencias de distribución** [RMODP]:

| Transparencia | Qué oculta | Ejemplo |
|---|---|---|
| **De acceso** | Las diferencias en la representación de los datos y en la forma de invocar | Leer un fichero local y uno remoto con la misma llamada |
| **De ubicación** | **Dónde** está físicamente el recurso | Una URL que no revela el servidor concreto que responde |
| **De migración** | Que el recurso **se ha movido** de sitio | Una máquina virtual que cambia de anfitrión |
| **De reubicación** (*relocation*) | Que el recurso se mueve **mientras se está usando** | Un usuario móvil que cambia de antena sin cortar la llamada |
| **De replicación** | Que existen **varias copias** del recurso | Una consulta servida por cualquiera de cinco réplicas |
| **De concurrencia** | Que el recurso lo comparten **varios usuarios a la vez** | Dos tramitadores editando registros distintos de la misma tabla |
| **De fallo** | La **avería y recuperación** de un componente | Un reintento automático que el usuario no percibe |
| **De persistencia** | Si el recurso está en **memoria o en disco** | Un objeto que se recupera de disco sin que la aplicación lo sepa |

> **[DATO CLAVE]** Las dos transparencias que más se confunden: **migración** (el recurso se mueve, pero no mientras lo estás usando) frente a **reubicación** (se mueve **durante** el uso, sin interrumpirlo). Y **replicación** (hay varias copias) frente a **concurrencia** (hay varios usuarios sobre la misma copia) [RMODP].

La transparencia total, sin embargo, **no es un objetivo deseable en sí mismo**. Ocultar por completo la distribución lleva a escribir software que trata una llamada remota como si fuera local, y esa es exactamente la trampa que describen las ocho falacias del epígrafe siguiente. El criterio profesional es **ocultar lo que se puede ocultar sin mentir**, y hacer explícito lo demás: los tiempos de espera, los reintentos y los fallos.

> **[RELACIÓN CON OTROS TEMAS]** La **arquitectura de ordenadores** y los componentes internos de un equipo se tratan en el **Tema 11**; los **sistemas operativos** y sus elementos constitutivos, en el **Tema 14**. Aquí se da por conocido qué es un proceso, un hilo y una llamada al sistema, y se estudia qué ocurre cuando esos procesos están en **máquinas distintas**.

#### 1.1.2. Las ocho falacias de la computación distribuida

Enunciadas en Sun Microsystems por **L. Peter Deutsch** (las siete primeras) y completadas por **James Gosling** (la octava), las falacias son los supuestos que un diseñador da por buenos sin darse cuenta y que después arruinan el sistema en producción [DEUTSCH]. Son un contenido de estudio excelente porque forman una lista cerrada de ocho elementos y porque explican, una a una, decisiones concretas de arquitectura en la nube.

| # | Falacia | Realidad | Qué obliga a hacer |
|---|---|---|---|
| 1 | **La red es fiable** | Los paquetes se pierden; los enlaces caen | Reintentos, tiempos de espera, idempotencia |
| 2 | **La latencia es cero** | Una llamada remota es órdenes de magnitud más lenta que una local | Agrupar llamadas, caché, asincronía |
| 3 | **El ancho de banda es infinito** | El caudal es finito y compartido | Compresión, paginación, transferir solo lo necesario |
| 4 | **La red es segura** | Hay escucha, suplantación y manipulación | Cifrado en tránsito, autenticación mutua, confianza cero |
| 5 | **La topología no cambia** | Máquinas y direcciones cambian sin avisar | Descubrimiento de servicios, nombres lógicos, no IP fijas |
| 6 | **Hay un único administrador** | Intervienen operadores, proveedores y terceros | Acuerdos de nivel de servicio, contratos, observabilidad propia |
| 7 | **El coste de transporte es cero** | Serializar y mover datos cuesta CPU y **dinero** | Diseño consciente del tráfico de salida y del formato |
| 8 | **La red es homogénea** | Conviven tecnologías, versiones y proveedores distintos | Formatos e interfaces neutrales y normalizados |

> **[DATO CLAVE]** Las ocho falacias en orden: **fiable · latencia cero · ancho de banda infinito · segura · topología estable · un solo administrador · transporte gratis · red homogénea**. La séptima, «el coste de transporte es cero», es la que se materializa hoy en las **tarifas de salida de datos** (*egress*) de los proveedores de nube pública, y por eso el Reglamento de Datos ha tenido que intervenir sobre ellas [DEUTSCH] [DATAACT].

La segunda falacia merece un comentario aparte, porque es la que más frecuentemente se subestima. La latencia no se puede eliminar: está limitada por la velocidad de la luz en el medio. Un ida y vuelta entre Madrid y un centro de datos en la costa este de Estados Unidos ronda los **80-100 milisegundos** en el mejor de los casos, y ninguna optimización de software lo va a reducir. Si una pantalla de tramitación hace cien llamadas remotas encadenadas, esa pantalla tardará **varios segundos** por pura aritmética, sin que haya nada «lento» en ninguno de los dos extremos. De ahí que la elección de **región** de un servicio en la nube sea una decisión de rendimiento, no solo jurídica.

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** El portal de cita previa del caso de referencia consulta el padrón para verificar el empadronamiento. Si esa consulta se hace **una vez por solicitud**, el sistema funciona. Si se hace **una vez por cada campo del formulario** para «validar en vivo», se multiplica el tráfico contra un sistema crítico y compartido y el día de la avalancha se cae el padrón, no el portal. La falacia 2 y la falacia 3 explican por qué: cada validación parece gratis vista de una en una.

### 1.2. Arquitecturas distribuidas

Una **arquitectura distribuida** describe cómo se reparten las responsabilidades entre los componentes y cómo se comunican. Las cuatro familias que pide el enunciado —cliente-servidor y multicapa, orientada a servicios y microservicios, entre pares y dirigida por eventos— no son alternativas excluyentes: en un sistema real conviven. Lo que sí conviene tener claro es la **pregunta que responde cada una**.

#### 1.2.1. Arquitectura cliente-servidor y multicapa

En el modelo **cliente-servidor**, un proceso (**cliente**) solicita un servicio y otro (**servidor**) lo presta. La relación es **asimétrica** —el cliente inicia, el servidor responde— y **síncrona** en su forma clásica. El servidor es un recurso compartido y suele ser, por tanto, el **cuello de botella** y el **punto único de fallo** del conjunto.

La evolución del modelo se cuenta por **capas** (*tiers*), entendidas como niveles de despliegue físico o lógico. Es imprescindible distinguir dos conceptos que se confunden:

- Las tres **funciones lógicas** de toda aplicación de gestión: **presentación**, **lógica de negocio** y **gestión de datos**.
- El número de **capas físicas** en que esas funciones se reparten.

| Modelo | Reparto | Ventajas | Inconvenientes |
|---|---|---|---|
| **1 capa** (monolítico) | Las tres funciones en la misma máquina | Simplicidad máxima | No escala; no comparte |
| **2 capas, cliente grueso** | Presentación + negocio en el cliente; datos en el servidor | Aprovecha el equipo del usuario | **Despliegue en cada puesto**; lógica duplicada; el cliente accede directamente a la base de datos |
| **2 capas, cliente ligero** | Presentación en el cliente; negocio + datos en el servidor | Poco que instalar | El servidor concentra toda la carga |
| **3 capas** | Presentación / **servidor de aplicaciones** / **servidor de datos** | Cada capa escala y se asegura por separado; la lógica está en un solo sitio | Más elementos que operar |
| **N capas** | 3 capas + capas de integración, servicios, caché o presentación web | Máxima flexibilidad e integración | Complejidad, latencia acumulada |

> **[DATO CLAVE]** La regla que define una arquitectura **multicapa estricta** es que **cada capa solo se comunica con la contigua**: la presentación nunca ataca directamente a la base de datos. Si lo hace, hay tres capas dibujadas pero **dos** de verdad, y se pierden las ventajas de seguridad y de mantenimiento del modelo [TANENBAUM].

La arquitectura de tres capas sigue siendo, con enorme diferencia, **la más habitual en la Administración**, y es el punto de partida de casi todas las migraciones a la nube: la capa de presentación se convierte en un servicio web escalable, la de negocio en un conjunto de servicios de aplicación y la de datos en una base de datos gestionada.

> **[RELACIÓN CON OTROS TEMAS]** La **arquitectura de sistemas cliente/servidor y multicapa** y las **arquitecturas de servicios web** son el objeto específico del **Tema 22**, y el desarrollo web *front-end* y en servidor, el del **Tema 23**. Aquí se recorren solo como **paradigmas de distribución**, para poder contrastarlos con los microservicios y con el modelo de nube.

#### 1.2.2. Arquitecturas orientadas a servicios y microservicios

La **arquitectura orientada a servicios (SOA)** organiza el sistema como un conjunto de **servicios** con interfaz publicada, reutilizables y débilmente acoplados, que se invocan a través de la red. Sus principios canónicos son la **interfaz de contrato normalizada**, la **autonomía**, la **abstracción** (el servicio oculta su implementación), la **reutilización**, la **componibilidad**, la **ausencia de estado** y la **descubribilidad** en un registro.

En la SOA clásica de los años dos mil, la integración se apoyaba en un **bus de servicios empresarial (ESB)**, una pieza central que enrutaba, transformaba formatos, aplicaba políticas y orquestaba procesos. Ese diseño resolvió un problema real —conectar sistemas heterogéneos sin reescribirlos— pero creó otro: el bus acumulaba lógica de negocio y se convertía en un **punto único de fallo y de cuello de botella organizativo**, porque cualquier cambio pasaba por el equipo que lo mantenía.

Los **microservicios** son la reacción a ese problema. Una aplicación se construye como un conjunto de **servicios pequeños, autónomos y desplegables por separado**, cada uno organizado en torno a una **capacidad de negocio** y comunicándose por mecanismos ligeros [FOWLER]. Sus rasgos definitorios:

- **Un servicio, un despliegue**: cada servicio se libera a su ritmo, sin coordinar una entrega global.
- **Base de datos por servicio**: cada uno es dueño de sus datos y **nadie accede a la base de datos de otro**. Esta es la regla más dura de cumplir y la que de verdad separa unos microservicios de un monolito troceado.
- **Tuberías tontas y extremos listos** (*smart endpoints, dumb pipes*): la lógica vive en los servicios, no en el bus.
- **Gobierno y tecnología descentralizados**: cada equipo puede elegir su lenguaje y su almacén, con la disciplina que eso exige.
- **Diseño para el fallo**: se asume que cualquier dependencia puede no responder, y se usan **tiempos de espera**, **reintentos con retroceso** y **cortacircuitos** (*circuit breaker*) para evitar que un fallo se propague en cascada.

| Criterio | SOA clásica | Microservicios |
|---|---|---|
| Tamaño del servicio | Grande, de grano grueso | Pequeño, una capacidad de negocio |
| Integración | **ESB** con lógica | Mensajería o API ligeras, **sin lógica en el canal** |
| Datos | Suele haber base de datos **compartida** | **Una base de datos por servicio** |
| Despliegue | Coordinado, a menudo conjunto | **Independiente** por servicio |
| Protocolo típico | SOAP/WSDL sobre XML [SOAP] | REST/JSON, gRPC, mensajería [FIELDING] [GRPC] |
| Reutilización | Objetivo explícito y central | Secundaria frente a la **autonomía** |
| Gobierno | Centralizado | Descentralizado |

> **[DATO CLAVE]** El corte exacto entre SOA y microservicios no está en el tamaño, sino en **dónde vive la lógica de integración** y en **quién es dueño de los datos**. SOA: bus inteligente y datos frecuentemente compartidos. Microservicios: canal tonto, servicios inteligentes y **una base de datos por servicio** [FOWLER].

Los microservicios **no son gratis**. Trasladan complejidad del código a la **operación**: hay que descubrir servicios, equilibrar carga, versionar contratos, correlacionar trazas entre decenas de procesos y mantener la consistencia de datos **sin transacciones distribuidas** (patrón *saga*, compensaciones, eventos). La recomendación profesional más citada es la de **empezar por el monolito** y extraer servicios cuando el dolor lo justifique, no antes [FOWLER]. En una Administración con equipos pequeños y contratación por lotes, un monolito bien modularizado suele ser una decisión más defendible que veinte microservicios sin equipo que los opere.

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** En el portal de cita previa, un troceado razonable serían **tres** servicios: *validación de empadronamiento* (consulta al padrón, cacheable), *gestión de solicitudes* (alta, consulta y modificación) y *notificación* (correo y avisos). Tres, no quince: el criterio no es «cuanto más pequeño mejor», sino **qué partes tienen ritmos de cambio y de carga distintos**. La validación se dispara el día de la apertura; la notificación se dispara al día siguiente, cuando se resuelven las solicitudes.

#### 1.2.3. Arquitecturas P2P y orientadas a eventos

En una **arquitectura entre pares (P2P)** todos los nodos son funcionalmente equivalentes: cada uno actúa **a la vez como cliente y como servidor**. No hay asimetría de roles ni, en su forma pura, servidor central. Las ventajas son la **ausencia de punto único de fallo**, el **coste marginal decreciente** (cada nuevo participante aporta recursos además de consumirlos) y la resistencia a la censura y a la desconexión. Los inconvenientes son igual de claros: **localizar** un recurso sin directorio central es difícil, la seguridad y la reputación de los pares son problemáticas, y el control administrativo es escaso.

Se distinguen tres variantes por cómo resuelven la localización:

| Variante | Cómo se localiza el recurso | Ejemplo histórico |
|---|---|---|
| **P2P centralizado** (híbrido) | Un **índice central** dice quién tiene qué; el intercambio es directo entre pares | Primeras redes de intercambio de ficheros; el rastreador (*tracker*) de BitTorrent |
| **P2P puro descentralizado** | **Inundación** de consultas entre vecinos | Gnutella |
| **P2P estructurado** | **Tabla hash distribuida (DHT)**: cada clave tiene un nodo responsable determinable por cálculo | Chord, Kademlia, IPFS [DHT] |

> **[DATO CLAVE]** La **tabla hash distribuida (DHT)** es el mecanismo que permite localizar un recurso en una red P2P **sin índice central y sin inundar la red**: la clave del recurso determina matemáticamente qué nodo es responsable de él, y la búsqueda converge en un número de saltos del orden del logaritmo del número de nodos [DHT].

El modelo P2P no es una curiosidad histórica: reaparece dentro de la propia nube. Muchos sistemas de almacenamiento distribuido, bases de datos NoSQL de anillo y **cadenas de bloques** son P2P estructurado por dentro, aunque se consuman como un servicio centralizado por fuera.

La **arquitectura dirigida por eventos (EDA)** cambia el eje de la comunicación. En lugar de que un componente **pida** algo a otro, un componente **publica un hecho que ya ha ocurrido** —«solicitud registrada», «pago confirmado», «expediente resuelto»— y quien esté interesado reacciona. El emisor **no sabe ni le importa** quién le escucha: esto es lo que produce el acoplamiento más bajo posible entre componentes.

Sus elementos son cuatro: el **productor** del evento, el **evento** en sí (un hecho inmutable y pasado, con su marca de tiempo), el **canal o intermediario** (*broker*) que lo transporta y lo retiene, y el **consumidor**, que reacciona. Se distinguen dos topologías clásicas: la **mediada** (*mediator*), en la que un orquestador dirige un flujo con varios pasos, y la **encadenada** (*broker*), en la que cada componente reacciona al evento anterior sin director.

| Ventajas de EDA | Inconvenientes de EDA |
|---|---|
| Acoplamiento mínimo: se añaden consumidores sin tocar al productor | **Trazabilidad difícil**: no hay una pila de llamadas que seguir |
| Absorbe picos de carga: el intermediario amortigua | Consistencia **eventual**, no inmediata |
| Tolerancia a fallos: si el consumidor cae, el evento espera | Depuración y pruebas complejas |
| Permite reprocesar el histórico de eventos | Riesgo de eventos duplicados: exige **idempotencia** |

> **[DATO CLAVE]** Distinción entre **orden** y **evento**: un **comando** («registra esta solicitud») va dirigido a **un** destinatario concreto y espera que se ejecute; un **evento** («solicitud registrada») es un hecho **ya ocurrido**, va dirigido a **nadie en particular** y no espera nada. Confundirlos es el error de diseño más común en EDA.

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** En el caso de referencia, «solicitud registrada» es un evento perfecto: al publicarlo, reaccionan de forma independiente el servicio de **notificación** (acuse de recibo a la persona solicitante), el de **estadística** (contador del cuadro de mando) y el de **archivo** (conservación de la evidencia). Si mañana Intervención pide un cuarto consumidor para su seguimiento, se añade **sin tocar** el servicio de solicitudes.

### 1.3. Modelos de comunicación distribuida

Toda la coordinación en un sistema distribuido se reduce, en último término, a **enviar mensajes**. Lo que cambia entre modelos es **si el emisor espera**, **si emisor y receptor tienen que existir a la vez** y **quién conoce a quién**. Esas tres preguntas —acoplamiento **temporal**, acoplamiento **espacial** y acoplamiento **de sincronización**— clasifican todos los mecanismos.

#### 1.3.1. Comunicación síncrona: sockets, RPC y servicios web

En la comunicación **síncrona**, el proceso que llama **se bloquea esperando la respuesta**. Es el modelo más sencillo de programar y de razonar, y por eso es el dominante. Su precio es que el llamante **depende de la disponibilidad y del tiempo de respuesta del llamado**: si el destinatario está caído o lento, el llamante se degrada con él.

Los mecanismos, de más bajo a más alto nivel de abstracción:

**1. Sockets.** La interfaz de programación sobre TCP o UDP. El programador maneja direcciones, puertos, conexiones y la serialización de los datos. Es el sustrato de todo lo demás, pero programar directamente sobre sockets es hoy excepcional fuera del software de sistemas.

**2. Llamada a procedimiento remoto (RPC).** La idea es hacer que invocar un procedimiento en otra máquina **se parezca** a invocarlo localmente. El mecanismo tiene tres piezas: el **resguardo del cliente** (*stub*), que empaqueta los parámetros (*marshalling*); el transporte; y el **resguardo del servidor** (*skeleton*), que los desempaqueta, ejecuta y devuelve el resultado. Sobre esta idea se construyeron **ONC RPC**, **CORBA** con su protocolo IIOP [CORBA], **Java RMI**, **DCOM** y, hoy, **gRPC** sobre HTTP/2 con serialización binaria [GRPC].

**3. Servicios web SOAP.** Contrato explícito en **WSDL**, mensajes **XML** con sobre SOAP y una familia de extensiones (**WS-Security**, **WS-ReliableMessaging**, **WS-AtomicTransaction**) para seguridad, fiabilidad y transaccionalidad [SOAP]. Verboso y pesado, pero con contrato fuerte y con soporte normalizado de firma y cifrado a nivel de mensaje, razón por la que sigue muy presente en integraciones interadministrativas.

**4. Servicios REST.** No es un protocolo, sino un **estilo arquitectónico** definido por Fielding con seis restricciones: cliente-servidor, **sin estado** (*stateless*), cacheable, sistema en capas, interfaz uniforme y, opcionalmente, código bajo demanda [FIELDING]. En la práctica se materializa en HTTP + JSON, con los verbos `GET`, `POST`, `PUT`, `PATCH` y `DELETE` sobre recursos identificados por URI [RFC7231].

> **[DATO CLAVE]** Un error clásico de RPC es creer que **una llamada remota es igual que una local**. No lo es: puede fallar **por la red** —no solo por la lógica—, puede **tardar** miles de veces más y puede **ejecutarse dos veces** si el llamante reintenta tras un tiempo de espera. Por eso las operaciones remotas deben ser **idempotentes** siempre que sea posible: repetir la operación debe producir el mismo resultado que ejecutarla una sola vez [DEUTSCH].

Sobre la idempotencia conviene retener la correspondencia con HTTP: **`GET`, `PUT` y `DELETE` son idempotentes**; **`POST` no lo es** (dos `POST` crean dos recursos). De ahí la práctica de enviar una **clave de idempotencia** en las peticiones de creación que pueden reintentarse [RFC7231].

#### 1.3.2. Comunicación asíncrona: colas, publicación-suscripción y flujos

En la comunicación **asíncrona**, el emisor **deposita el mensaje y continúa**. Entre emisor y receptor se interpone un **intermediario de mensajes** (*message broker*) que almacena, encamina y entrega. Esta interposición es la que rompe el acoplamiento: emisor y receptor **no tienen que estar disponibles al mismo tiempo**, ni ir al mismo ritmo, ni conocerse.

Los tres patrones fundamentales:

| Patrón | Reparto | Consumo | Uso típico |
|---|---|---|---|
| **Cola punto a punto** | Un mensaje → **un solo consumidor** | El mensaje se **elimina** al confirmarse | Repartir trabajo entre varios trabajadores |
| **Publicación-suscripción** | Un mensaje → **todos los suscriptores** | Cada suscriptor recibe **su copia** | Notificar un hecho a varios interesados |
| **Flujo de eventos** (*log*) | Un mensaje → los consumidores que quieran | El mensaje **permanece** un tiempo; cada consumidor lleva su propio puntero | Reprocesar histórico, analítica, auditoría |

Los protocolos normalizados de referencia son **AMQP 1.0** (OASIS, = ISO/IEC 19464), orientado a mensajería empresarial con colas, intercambios y encaminamiento [AMQP], y **MQTT 5.0** (OASIS, = ISO/IEC 20922), muy ligero y de publicación-suscripción con temas jerárquicos, dominante en IoT y en el borde [MQTT]. Como plataformas, las colas clásicas se implementan con productos de tipo RabbitMQ o ActiveMQ, y los flujos con plataformas de tipo Kafka [KAFKA].

Las **garantías de entrega** son un dato clave:

- **Como mucho una vez** (*at most once*): rápido, pero **puede perder** mensajes. Válido para telemetría.
- **Al menos una vez** (*at least once*): no pierde, pero **puede duplicar**. Es el más usado y **obliga al consumidor a ser idempotente**.
- **Exactamente una vez** (*exactly once*): deseable y caro; en sistemas distribuidos solo se consigue de forma acotada, combinando desduplicación y transaccionalidad del intermediario.

> **[DATO CLAVE]** La garantía práctica dominante es **«al menos una vez»**, y su consecuencia obligatoria es que **el consumidor debe ser idempotente**: procesar dos veces el mismo mensaje no puede producir dos altas, dos cobros ni dos notificaciones. El mecanismo habitual es una **clave de desduplicación** almacenada por el consumidor.

Otros dos conceptos operativos que aparecen en supuestos prácticos: la **cola de mensajes fallidos** (*dead letter queue*), donde se aparta el mensaje que ha agotado sus reintentos para que no bloquee la cola ni se pierda; y la **contrapresión** (*backpressure*), el mecanismo por el que un consumidor saturado hace que el sistema reduzca el ritmo de admisión en lugar de desbordarse.

> **[EJERCICIO RESUELTO]** *El portal de cita previa recibe 40.000 solicitudes en la primera hora, pero el sistema de registro electrónico solo admite 5 asientos por segundo. ¿Síncrono o asíncrono?*
>
> **Solución.** 5 asientos por segundo son 18.000 por hora: el registro **no puede** absorber el pico. Si la llamada fuera síncrona, la ciudadanía vería errores de tiempo de espera y volvería a intentarlo, **multiplicando** la carga. La solución es **desacoplar con una cola**: el portal valida, guarda la solicitud, publica el evento «solicitud registrada» y **devuelve inmediatamente** un justificante con número de referencia y marca de tiempo. Un consumidor va vaciando la cola contra el registro al ritmo que este admite. La cola alcanza un máximo de unas 22.000 solicitudes pendientes y se vacía en unas cuatro horas y media (22.000 ÷ 5 por segundo ≈ 73 minutos de trabajo residual una vez cesa el pico). **Lo que se conserva es la garantía jurídica** —la fecha y hora de presentación es la de la solicitud, no la del asiento— y lo que se sacrifica es la inmediatez del número de registro definitivo.

#### 1.3.3. Consistencia, teorema CAP y coordinación

Replicar datos en varios nodos mejora la disponibilidad y el rendimiento, pero abre la pregunta central de los sistemas distribuidos: **¿qué pasa si dos réplicas no coinciden?**

El **teorema CAP**, conjeturado por Eric Brewer y demostrado por Gilbert y Lynch, formula el compromiso [BREWER]:

- **C — Consistencia** (*consistency*): toda lectura obtiene la escritura más reciente o un error. Ojo: **no es la «C» de ACID**; aquí significa que todas las réplicas se ven iguales.
- **A — Disponibilidad** (*availability*): toda petición recibe una respuesta no errónea, aunque pueda no ser la más reciente.
- **P — Tolerancia a particiones** (*partition tolerance*): el sistema sigue funcionando aunque se pierdan mensajes entre grupos de nodos.

> **[DATO CLAVE]** El enunciado popular «elige dos de las tres» es **incorrecto**. En un sistema distribuido real **las particiones ocurren**, así que **P no es opcional**: el teorema dice que, **cuando hay partición**, hay que elegir entre **C** y **A**. Cuando **no** la hay, se puede tener consistencia y disponibilidad a la vez. Esta precisión es la que distingue una respuesta correcta de una respuesta de memoria [BREWER].

De ahí la clasificación práctica en sistemas **CP** (ante la partición, rechazan operaciones para no divergir: bases relacionales replicadas, sistemas de coordinación tipo consenso) y sistemas **AP** (ante la partición, siguen respondiendo y reconcilian después: almacenes de clave-valor de alta disponibilidad) [DYNAMO].

El contraste entre los dos modelos de garantías se resume así:

| | **ACID** | **BASE** |
|---|---|---|
| Significado | Atomicidad, Consistencia, Aislamiento, Durabilidad | *Basically Available, Soft state, Eventual consistency* |
| Consistencia | **Inmediata y fuerte** | **Eventual**: converge con el tiempo |
| Disponibilidad | Se sacrifica si hace falta | Se prioriza |
| Ámbito típico | Bases de datos relacionales, operaciones económicas | NoSQL distribuido, catálogos, contenidos, analítica |
| Escalado | Más difícil horizontalmente | Diseñado para escalar horizontalmente |

> **[RELACIÓN CON OTROS TEMAS]** Los **sistemas de gestión de bases de datos relacionales, orientados a objetos y NoSQL** —incluidas las propiedades ACID y la administración de bases distribuidas— son el objeto del **Tema 15**, y el diseño lógico y la normalización, el del **Tema 17**. Aquí interesa solo el compromiso entre consistencia y disponibilidad **como problema de distribución**.

La **coordinación** entre nodos exige, además, resolver dos problemas que no existen en un sistema centralizado:

**El tiempo.** Sin reloj global, no se puede afirmar sin más que un evento ocurrió antes que otro. Lamport define la relación **«ocurrió antes»** (*happened-before*) y los **relojes lógicos**: un contador por nodo que se incrementa con cada evento y se propaga en los mensajes, de modo que se obtiene un **orden parcial** consistente aunque los relojes físicos difieran [LAMPORT]. Los **relojes vectoriales** extienden la idea para detectar eventos **concurrentes**, que es lo que permite descubrir un conflicto entre réplicas.

**El acuerdo.** Que un conjunto de nodos decida lo mismo pese a fallos y retrasos es el problema del **consenso**, resuelto en la práctica por algoritmos como **Paxos** y **Raft**, que exigen **mayoría (quórum)** de nodos vivos. De ahí que los servicios de coordinación se desplieguen en número **impar** (3, 5, 7): con cinco nodos se tolera la caída de dos y aún hay mayoría. El resultado teórico de fondo —**FLP**— establece que en un sistema **asíncrono** el consenso no puede garantizarse de forma determinista si puede fallar aunque sea un nodo; en la práctica se resuelve introduciendo **tiempos de espera**, es decir, renunciando a la asincronía pura.

> **[DATO CLAVE]** Los algoritmos de consenso (**Paxos**, **Raft**) necesitan **quórum de mayoría**: con `N` nodos toleran `(N-1)/2` caídas. Por eso los despliegues son **impares**: con 3 nodos se tolera 1 fallo; con 5, dos. Añadir un cuarto nodo a un grupo de tres **no aumenta** la tolerancia y sí el coste.

Un último concepto, imprescindible para entender la disponibilidad ofrecida por los proveedores de nube: el **dominio de fallo**. Se agrupa la infraestructura en zonas que **no comparten** alimentación, refrigeración ni red, de forma que un incidente afecte a una sola. En la nube pública esto se materializa en **zonas de disponibilidad** (centros de datos independientes y próximos, unidos por red de baja latencia) dentro de una **región** (área geográfica). Repartir réplicas entre zonas protege frente a la caída de un edificio; repartirlas entre regiones protege frente a una catástrofe territorial, a costa de latencia y, con frecuencia, de restricciones jurídicas sobre la ubicación de los datos [CSP].
---

## 2. Conceptos fundamentales de Cloud Computing

### 2.1. Definición y características esenciales

#### 2.1.1. La definición del NIST y las cinco características esenciales

La definición de referencia mundial es la del **NIST**, publicada en la **Special Publication 800-145** en septiembre de 2011 y nunca sustituida desde entonces. Su enunciado literal, traducido:

> «La computación en la nube es un modelo que permite el acceso **ubicuo, cómodo y bajo demanda** a través de la red a un **conjunto compartido** de recursos informáticos configurables (redes, servidores, almacenamiento, aplicaciones y servicios) que pueden ser **rápidamente aprovisionados y liberados** con un **esfuerzo de gestión mínimo** o una **mínima interacción con el proveedor** del servicio» [NIST145].

De esa definición el NIST extrae la estructura que hay que llevar memorizada: **cinco características esenciales, tres modelos de servicio y cuatro modelos de despliegue**.

Las **cinco características esenciales**, con su nombre en inglés porque así aparecen en los enunciados:

**1. Autoservicio bajo demanda** (*on-demand self-service*). El consumidor puede aprovisionar capacidad —tiempo de servidor, almacenamiento, red— **unilateralmente y de forma automática**, sin necesitar interacción humana con el proveedor. Esta es la característica que convierte una semana de tramitación en un minuto de consola web o en una llamada a una API, y es la que más cambia la forma de trabajar de un departamento TIC.

**2. Acceso amplio a la red** (*broad network access*). Las capacidades están disponibles **a través de la red** y se accede a ellas mediante **mecanismos estándar** que permiten el uso desde plataformas heterogéneas: ordenadores, tabletas, teléfonos, estaciones de trabajo. La consecuencia es inmediata: **sin red no hay servicio**, y la conectividad pasa a ser tan crítica como el propio servicio.

**3. Agrupación de recursos** (*resource pooling*). Los recursos del proveedor se agrupan para servir a **múltiples consumidores** mediante un modelo **multiinquilino**, con recursos físicos y virtuales asignados y reasignados dinámicamente según la demanda. Hay **independencia de la ubicación**: el cliente **no controla ni conoce** la ubicación exacta de los recursos, aunque puede especificarla a un nivel de abstracción superior (país, región, centro de datos). Es la característica que produce la economía de escala y, a la vez, la que genera los principales problemas jurídicos y de seguridad.

**4. Elasticidad rápida** (*rapid elasticity*). Las capacidades pueden aprovisionarse y liberarse **elásticamente**, en algunos casos de forma automática, para escalar rápidamente hacia arriba y hacia abajo con la demanda. Para el consumidor, las capacidades disponibles a menudo **parecen ilimitadas** y pueden apropiarse en cualquier cantidad y en cualquier momento.

**5. Servicio medido** (*measured service*). Los sistemas de nube controlan y optimizan el uso de recursos **midiéndolo** a un nivel de abstracción apropiado para el tipo de servicio (almacenamiento, procesamiento, ancho de banda, cuentas activas). El uso puede **monitorizarse, controlarse y reportarse**, aportando transparencia tanto al proveedor como al consumidor. Es la característica que hace posible el pago por uso y, en el sector público, la que permite imputar el gasto a la unidad que lo genera.

> **[DATO CLAVE]** Las cinco características son **acumulativas y necesarias**: si un servicio no las cumple **todas**, no es nube según el NIST. El error típico consiste en llamar «nube privada» a un centro de datos virtualizado que **no tiene autoservicio ni medición**: eso es **virtualización**, no nube. La virtualización es un **habilitador**, no un sinónimo [NIST145].

Una regla mnemotécnica útil en español, tomando la inicial de cada una: **A-A-A-E-M** — **A**utoservicio, **A**cceso a la red, **A**grupación, **E**lasticidad, **M**edición.

#### 2.1.2. El vocabulario normalizado ISO/IEC 17788 y los actores del modelo

La norma internacional equivalente es **ISO/IEC 17788:2014**, publicada conjuntamente con la UIT como **Recomendación ITU-T Y.3500**. Coincide en lo esencial con el NIST, pero introduce dos diferencias [ISO17788]:

1. Añade una **sexta característica clave**: la **multitenencia** (*multi-tenancy*), entendida como la asignación de recursos físicos o virtuales de forma que varios inquilinos y sus cómputos y datos permanezcan **aislados e inaccesibles** entre sí. El NIST la considera implícita dentro de la agrupación de recursos; ISO prefiere hacerla explícita.
2. Sustituye la rígida terna IaaS/PaaS/SaaS por dos conceptos: los **tipos de capacidad en la nube** (de **infraestructura**, de **plataforma** y de **aplicación**) y las **categorías de servicio**, entre las que enumera **CompaaS** (cómputo), **CaaS** (comunicaciones), **DSaaS** (almacenamiento de datos), **IaaS**, **NaaS** (red), **PaaS** y **SaaS**.

La norma **ISO/IEC 17789** completa el marco con la **arquitectura de referencia**: roles (cliente, proveedor y socio del servicio en la nube), subroles y actividades [ISO17789].

Por su parte, el NIST define en la **SP 500-292** una **arquitectura de referencia con cinco actores** que conviene memorizar porque estructura cualquier supuesto de contratación [NIST292]:

| Actor | Papel | Ejemplo en la Administración |
|---|---|---|
| **Consumidor** de la nube (*cloud consumer*) | Usa el servicio y mantiene la relación contractual | El Ayuntamiento que contrata el servicio |
| **Proveedor** de la nube (*cloud provider*) | Pone el servicio a disposición | El operador de la plataforma contratada |
| **Intermediario** (*cloud broker*) | Gestiona uso, rendimiento y entrega; **agrega, integra o mejora** servicios de terceros | Una empresa que empaqueta varios servicios en un único catálogo |
| **Portador** (*cloud carrier*) | Proporciona **conectividad y transporte** entre proveedor y consumidor | El operador de telecomunicaciones |
| **Auditor** (*cloud auditor*) | Evaluación **independiente** de servicios, operaciones, rendimiento y seguridad | La entidad de certificación de conformidad con el ENS |

> **[DATO CLAVE]** Los **cinco actores del NIST**: consumidor, proveedor, **intermediario** (*broker*), **portador** (*carrier*) y **auditor**. Los dos que se olvidan siempre son el portador —que es quien pone la red— y el auditor. Nótese que el **intermediario** puede ser de tres tipos: de **intermediación** (añade valor a un servicio), de **agregación** (combina varios) y de **arbitraje** (elige dinámicamente el mejor proveedor) [NIST292].

### 2.2. Evolución desde la computación distribuida

La nube no aparece de la nada: es el resultado de una línea de evolución de sesenta años que conviene poder narrar, porque lo que importa es precisamente la **relación** entre paradigmas.

| Etapa | Idea central | Qué aporta a la nube |
|---|---|---|
| **Tiempo compartido** (años 60-70) | Un gran ordenador central atiende a muchos terminales; se factura por tiempo de uso | El **modelo económico**: pagar por consumo, no por propiedad |
| **Cliente-servidor** (años 80-90) | El cómputo se reparte entre estaciones de trabajo y servidores | La **separación de roles** y las interfaces de red |
| **Computación en malla** (*grid*, años 90-2000) | Agregar recursos **heterogéneos y de varias organizaciones** para resolver un problema grande | La idea de **federación** y de recurso como servicio |
| **Computación de utilidad** (*utility computing*) | La informática como **suministro** medido, igual que el agua o la luz | La **medición** y la facturación por uso |
| **Virtualización masiva** (2000s) | Separar el servidor lógico del servidor físico | El **habilitador técnico** del aprovisionamiento en minutos |
| **Nube** (desde 2006) | Autoservicio + elasticidad + medición sobre recursos agrupados | El modelo completo |
| **Borde y niebla** (*edge*, *fog*) | Acercar el cómputo al punto donde se generan los datos | Reduce **latencia** y tráfico; complementa la nube, no la sustituye |

Las diferencias entre **malla** y **nube**:

| Criterio | Computación en malla (*grid*) | Computación en la nube |
|---|---|---|
| Propiedad de los recursos | **Varias organizaciones** federadas | Normalmente **un proveedor** |
| Motivación | Compartir capacidad para problemas científicos grandes | Prestar servicio comercial elástico |
| Modelo económico | Colaboración, intercambio de ciclos | **Pago por uso** |
| Heterogeneidad | Alta y asumida | Homogeneizada por virtualización |
| Aprovisionamiento | Por lotes, planificado (*batch*) | **Bajo demanda, en minutos** |

> **[DATO CLAVE]** La nube **no inventó** ninguna de sus piezas: el tiempo compartido aportó el pago por uso, la malla la federación, la virtualización el aprovisionamiento rápido y la web el acceso ubicuo. **Lo que la nube aporta es la combinación**, y en particular el **autoservicio automatizado** y la **medición**, que son las dos características que ninguna etapa anterior tenía simultáneamente [NIST145].

### 2.3. Tecnologías habilitadoras: virtualización y orquestación

#### 2.3.1. De la máquina virtual al contenedor y a la función

La **virtualización** es la tecnología que permite que un mismo servidor físico ejecute varios entornos aislados, y sin ella la nube sería económicamente inviable: es lo que permite la **agrupación de recursos** y la reasignación dinámica. El **hipervisor** es el software que crea y gestiona esas máquinas virtuales; se distingue el **tipo 1** (ejecuta directamente sobre el hardware, *bare metal*) del **tipo 2** (ejecuta sobre un sistema operativo anfitrión) [HYPERVISORS].

Sobre esa base, la nube ha ido subiendo el nivel de abstracción en tres saltos:

| Unidad | Qué incluye | Arranque | Aislamiento | Densidad por servidor |
|---|---|---|---|---|
| **Máquina virtual** | Hardware virtual + **sistema operativo completo** + aplicación | Minutos | **Fuerte** (núcleos separados) | Decenas |
| **Contenedor** | Aplicación + sus dependencias; **comparte el núcleo** del anfitrión | Segundos | Medio (espacios de nombres y *cgroups*) | Cientos |
| **Función** (*serverless*) | Solo el **código** de la función; el entorno lo pone la plataforma | Milisegundos a segundos | Gestionado por el proveedor | Miles |

> **[DATO CLAVE]** La diferencia esencial entre máquina virtual y contenedor: la **máquina virtual virtualiza el hardware** y lleva **su propio sistema operativo**; el **contenedor virtualiza el sistema operativo** y **comparte el núcleo** del anfitrión. De ahí que el contenedor arranque en segundos y sea mucho más denso, y que su aislamiento sea **menor** —un fallo del núcleo afecta a todos los contenedores del nodo— [OCI].

La normalización es lo que hace que los contenedores sean el mecanismo de **portabilidad** entre nubes más eficaz disponible: la **Open Container Initiative** define especificaciones abiertas de **imagen**, de **ejecución** y de **distribución**, de modo que una imagen construida en un sitio se ejecuta igual en otro [OCI]. Este punto es directamente relevante para el §4.3 y para la estrategia pública de evitar la dependencia de un proveedor.

> **[RELACIÓN CON OTROS TEMAS]** La **virtualización de sistemas y de puestos de usuario** —hipervisores, tipos, gestión de recursos, VDI— es el objeto completo del **Tema 28**. El **almacenamiento y su virtualización**, junto con las políticas de copia de seguridad, corresponden al **Tema 26**. En este tema la virtualización se trata solo como **tecnología habilitadora** de la nube.

#### 2.3.2. Orquestación, automatización e infraestructura como código

Virtualizar no basta. Lo que convierte un conjunto de máquinas virtuales en una nube es la **capa de orquestación y automatización**: el software que recibe la petición del usuario, decide dónde colocar el recurso, lo crea, lo conecta a la red, aplica las políticas de seguridad, lo mide y lo factura, **sin intervención humana**.

Sus funciones son cinco:

1. **Planificación y colocación** (*scheduling*): decidir en qué anfitrión físico se ubica cada carga, atendiendo a capacidad, afinidad y dominios de fallo.
2. **Ciclo de vida**: creación, arranque, parada, escalado, actualización y destrucción de recursos.
3. **Autoescalado**: añadir o quitar instancias según métricas (uso de procesador, longitud de cola, peticiones por segundo) siguiendo reglas o previsiones.
4. **Autorreparación**: detectar una instancia que no responde y reemplazarla automáticamente.
5. **Medición y cuotas**: registrar el consumo por inquilino y aplicar límites.

En el mundo de los contenedores, el orquestador de referencia es **Kubernetes**, cuyo modelo es **declarativo**: el operador describe el **estado deseado** («quiero cinco réplicas de este servicio») y un bucle de control trabaja continuamente para que el estado real coincida con él [K8S]. Ese cambio de mentalidad —de dar órdenes a declarar objetivos— es el que hace posible la autorreparación.

La **infraestructura como código (IaC)** aplica la misma idea a toda la infraestructura: las redes, las máquinas, los permisos y las bases de datos se describen en **ficheros de texto versionados** que se aplican de forma **idempotente**, de modo que aplicar dos veces la misma descripción no duplica nada [TERRAFORM]. Sus ventajas son directamente auditables y, por tanto, muy valiosas en el sector público:

- **Reproducibilidad**: el entorno de preproducción es idéntico al de producción porque procede del mismo fichero.
- **Trazabilidad**: cada cambio de infraestructura queda como una revisión en el control de versiones, con autor, fecha y motivo.
- **Reversibilidad**: se puede volver a la descripción anterior.
- **Revisión previa**: el cambio se revisa antes de aplicarse, igual que el código.

> **[DATO CLAVE]** **Idempotencia** en infraestructura como código significa que **aplicar la misma descripción n veces deja el sistema en el mismo estado que aplicarla una vez**. Es lo que diferencia una herramienta declarativa de un guion (*script*) imperativo, que al ejecutarse dos veces puede crear dos veces el mismo recurso o fallar [TERRAFORM].

### 2.4. Ventajas y retos tecnológicos

Las **ventajas** de la nube son reales, pero conviene enunciarlas con precisión y con sus condiciones.

| Ventaja | En qué consiste | Condición o matiz |
|---|---|---|
| **Agilidad** | Aprovisionar en minutos lo que antes tardaba meses | Requiere que la organización sepa **decidir** igual de rápido |
| **Elasticidad** | Ajustar capacidad al pico y al valle | Solo ahorra si **de verdad se decrece**; una instancia encendida las 24 horas no es elástica |
| **De inversión a gasto corriente** (CAPEX → OPEX) | No se compra hardware; se paga el consumo | Cambia el **capítulo presupuestario**, con implicaciones administrativas serias |
| **Escala global** | Desplegar en varias regiones | Sujeto a restricciones jurídicas de ubicación del dato |
| **Alta disponibilidad** | Zonas y regiones redundantes | **No es automática**: hay que diseñar la aplicación para usarlas |
| **Copias y recuperación** | Servicios gestionados de respaldo y réplica | La **responsabilidad de tener copias sigue siendo del cliente** |
| **Delegación de tareas rutinarias** | El proveedor parchea, sustituye discos, renueva hardware | El personal propio se libera **para tareas de mayor valor**, no desaparece |
| **Sostenibilidad** | Mayor eficiencia energética por consolidación | Depende de la solución concreta y del suministro eléctrico [ESTRATEGIA-CLOUD] |

Los **retos** son igual de concretos:

**1. Dependencia del proveedor** (*vendor lock-in*). Cuanto más se usan servicios propietarios de alto nivel, más difícil y caro es salir. Se manifiesta en tres planos: **de datos** (formatos y coste de extracción), **de aplicación** (dependencia de API propietarias) y **de conocimiento** (el equipo solo sabe operar esa plataforma). Es el reto que aborda directamente el **Reglamento de Datos** con su régimen de cambio de proveedor [DATAACT].

**2. Pérdida de control y opacidad.** El cliente no ve el centro de datos, no elige el hardware, no conoce a los administradores del proveedor y depende de sus informes de auditoría. La respuesta es contractual y de certificación, no técnica.

**3. Seguridad y superficie de exposición.** Los servicios son accesibles desde internet por diseño. La causa dominante de incidentes en nube pública **no es la vulneración del proveedor**, sino la **configuración incorrecta del cliente**: almacenamiento expuesto públicamente, credenciales en el código, permisos excesivos [NIST144].

**4. Cumplimiento normativo y ubicación del dato.** Dónde se almacena, quién puede acceder y bajo qué jurisdicción. Se desarrolla en el §5.

**5. Coste imprevisible.** El pago por uso corta en las dos direcciones. Sin control, aparecen recursos olvidados encendidos, entornos de prueba que nadie apaga, tráfico de salida no previsto y servicios sobredimensionados. La disciplina de gestión económica del consumo se conoce como **FinOps** [FINOPS].

**6. Latencia y dependencia de la conectividad.** Sin red no hay servicio, y la latencia añadida puede ser incompatible con determinadas cargas.

**7. Continuidad y salida.** Qué ocurre si el proveedor quiebra, es adquirido, cambia sus precios drásticamente o resuelve el contrato. Un **plan de reversión** con datos exportables y probados es un requisito, no una precaución opcional.

> **[EJERCICIO RESUELTO]** *Un servicio municipal necesita 20 servidores durante 6 semanas al año (campaña) y 3 el resto del tiempo. La compra de 20 servidores propios cuesta 120.000 € amortizables en 5 años, más 9.000 €/año de mantenimiento y energía. En nube, cada servidor equivalente cuesta 0,20 €/hora. ¿Qué sale a cuenta?*
>
> **Solución.** **Propio**: 120.000 ÷ 5 = 24.000 €/año de amortización + 9.000 € = **33.000 €/año**.
> **Nube, dimensionando de verdad de forma elástica**: 6 semanas × 7 días × 24 h = 1.008 h con 20 servidores → 20 × 1.008 × 0,20 = **4.032 €**; el resto del año, 8.760 − 1.008 = 7.752 h con 3 servidores → 3 × 7.752 × 0,20 = **4.651 €**. Total ≈ **8.683 €/año**.
> **Nube mal usada, dejando los 20 encendidos todo el año**: 20 × 8.760 × 0,20 = **35.040 €/año**, es decir, **más caro que comprarlos**.
> **Lectura**: el ahorro no lo produce la nube, sino la **elasticidad efectivamente ejercida**. Esta comparación es deliberadamente incompleta —no incluye tráfico de salida, licencias, copias de seguridad ni el coste del personal que opera cada opción—, y por eso en un supuesto real hay que enunciar esas partidas aunque no se cuantifiquen.
---

## 3. Modelos de servicio en Cloud Computing

Los modelos de servicio responden a una sola pregunta: **¿hasta dónde llega lo que gestiona el proveedor y dónde empieza lo que gestiona el cliente?** El NIST reconoce **tres y solo tres**: IaaS, PaaS y SaaS [NIST145]. Todo lo demás —FaaS, CaaS, DBaaS— son denominaciones comerciales que encajan dentro de esos tres o se sitúan entre ellos.

La forma más rentable de estudiarlos es la **pila de responsabilidad**: nueve capas que van del edificio a los datos, y una línea que se desplaza hacia arriba a medida que se sube de modelo.

| Capa | Local (*on-premise*) | IaaS | PaaS | SaaS |
|---|---|---|---|---|
| Datos y su clasificación | **Cliente** | **Cliente** | **Cliente** | **Cliente** |
| Identidades y accesos | **Cliente** | **Cliente** | **Cliente** | **Cliente** |
| Aplicación | **Cliente** | **Cliente** | **Cliente** | Proveedor |
| Tiempo de ejecución y bibliotecas | **Cliente** | **Cliente** | Proveedor | Proveedor |
| Middleware | **Cliente** | **Cliente** | Proveedor | Proveedor |
| Sistema operativo | **Cliente** | **Cliente** | Proveedor | Proveedor |
| Virtualización | **Cliente** | Proveedor | Proveedor | Proveedor |
| Servidores y almacenamiento | **Cliente** | Proveedor | Proveedor | Proveedor |
| Red e instalaciones físicas | **Cliente** | Proveedor | Proveedor | Proveedor |

> **[DATO CLAVE]** Las **dos fronteras** que hay que saber señalar sin dudar. Entre **local e IaaS** está la **virtualización**: en IaaS el proveedor pone el hipervisor y el hardware. Entre **IaaS y PaaS** está el **sistema operativo**: en IaaS **lo administra el cliente**; en PaaS, **no**. Y hay dos capas que **nunca** cambian de dueño en ningún modelo: **los datos** y **las identidades y accesos** [NIST145] [CCN823].

### 3.1. Infraestructura como Servicio (IaaS)

#### 3.1.1. Concepto y recursos de computación, almacenamiento y red

La definición del NIST: la capacidad que se proporciona al consumidor es **aprovisionar procesamiento, almacenamiento, redes y otros recursos informáticos fundamentales**, sobre los que el consumidor puede desplegar y ejecutar software arbitrario, **incluidos sistemas operativos y aplicaciones**. El consumidor **no gestiona ni controla la infraestructura de nube subyacente**, pero **sí controla los sistemas operativos, el almacenamiento y las aplicaciones desplegadas**, y posiblemente un control limitado de componentes de red seleccionados, como los cortafuegos de anfitrión [NIST145].

El catálogo típico de un servicio IaaS se agrupa en tres familias:

**1. Cómputo.**
- **Instancias** o máquinas virtuales, caracterizadas por número de núcleos virtuales, memoria y familia de rendimiento (uso general, optimizadas para cómputo, para memoria, para almacenamiento o con aceleradores gráficos).
- **Imágenes** de máquina para el aprovisionamiento repetible.
- **Grupos de autoescalado** que crean y destruyen instancias según reglas.
- **Servidores dedicados** (*bare metal*), sin hipervisor, para cargas con exigencias de licenciamiento o de aislamiento.

**2. Almacenamiento.** Es imprescindible distinguir los tres tipos:

| Tipo | Unidad | Cómo se accede | Uso típico |
|---|---|---|---|
| **De bloques** | Bloque de disco | Se conecta a **una** instancia como si fuera un disco | Discos de sistema, bases de datos |
| **De ficheros** | Fichero en jerarquía de carpetas | Protocolo de red compartido (NFS, SMB) | Carpetas compartidas, contenido de aplicaciones |
| **De objetos** | **Objeto** con su identificador y sus metadatos | API sobre **HTTP** | Documentos, copias de seguridad, datos abiertos, ficheros grandes |

El **almacenamiento de objetos** es el más característico de la nube: no tiene jerarquía real de carpetas, es prácticamente ilimitado, muy barato, altamente durable por replicación y accesible por HTTP, pero **no se puede modificar un objeto parcialmente** —se sustituye entero— y su latencia es mayor. Suele ofrecerse en **clases** de acceso (frecuente, esporádico, archivo) con precios y tiempos de recuperación muy distintos.

**3. Red.**
- **Redes virtuales** privadas con subredes, tablas de encaminamiento y pasarelas.
- **Grupos de seguridad** y listas de control de acceso, que son cortafuegos distribuidos aplicados a la instancia.
- **Balanceadores de carga** de nivel 4 y de nivel 7.
- **Direcciones IP públicas**, **traducción de direcciones** para salida y **conexiones privadas** hacia la red corporativa (VPN de sitio a sitio o enlace dedicado).
- **DNS** gestionado y **red de distribución de contenidos** (CDN).

> **[RELACIÓN CON OTROS TEMAS]** El **acceso remoto seguro y las VPN** se estudian en el **Tema 36**, y los **protocolos TCP/IP** en el **Tema 34**. Aquí solo interesa que una nube IaaS se conecta a la red corporativa por VPN o por enlace dedicado, y que la **segmentación** es responsabilidad del cliente.

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** La documentación que la ciudadanía adjunta a una solicitud —justificantes, escaneos, certificados— es el caso de manual de **almacenamiento de objetos**: son ficheros que se escriben una vez, se leen pocas veces, deben conservarse años y crecen sin límite previsible. Guardarlos en discos de bloques conectados a las instancias sería más caro, menos durable y obligaría a redimensionar discos continuamente.

#### 3.1.2. Abstracción del hardware y elasticidad

Lo que IaaS abstrae es el **ciclo de vida del hardware**: la compra, la instalación en el bastidor, el cableado, la sustitución de discos, la renovación por obsolescencia y la gestión del espacio, la energía y la refrigeración. Lo que **no** abstrae es la administración del sistema operativo: parcheado, endurecimiento, cuentas, servicios, registro y copias siguen siendo del cliente.

La **elasticidad** merece precisión terminológica:

- **Escalabilidad**: capacidad de un sistema de **crecer** para atender más carga. Puede ser **vertical** (*scale up*: dar más núcleos y memoria a la misma máquina; sencillo, pero con techo físico y normalmente con reinicio) u **horizontal** (*scale out*: añadir más máquinas; sin techo práctico, pero exige que la aplicación sea **apta para funcionar en varias instancias**, es decir, **sin estado en memoria local**).
- **Elasticidad**: capacidad de **crecer y decrecer automáticamente** siguiendo la demanda, en tiempos cortos y sin intervención humana.

> **[DATO CLAVE]** **Escalabilidad no es elasticidad.** Un sistema puede ser escalable (se le pueden añadir servidores) y **nada elástico** (hay que hacerlo a mano y tarda semanas). La elasticidad exige las **tres** cosas: automatismo, rapidez y **bidireccionalidad** —también hacia abajo—. Y la elasticidad horizontal exige que la aplicación sea **sin estado**: si la sesión del usuario vive en la memoria de una instancia concreta, no se pueden añadir ni quitar instancias sin romper sesiones [12FACTOR].

Los modelos de **facturación** de cómputo son también materia de supuestos prácticos:

| Modalidad | Compromiso | Descuento típico | Cuándo conviene |
|---|---|---|---|
| **Bajo demanda** | Ninguno | — | Cargas variables o impredecibles |
| **Reservada / con compromiso** | 1 o 3 años | Alto | **Base estable** de carga conocida |
| **Capacidad sobrante** (*spot*) | El proveedor puede **retirarla** con poco aviso | Muy alto | Procesos por lotes tolerantes a interrupción |
| **Anfitrión dedicado** | Servidor físico reservado | — | Requisitos de licencia o de aislamiento |

La combinación habitual —y la respuesta correcta en un supuesto de optimización— es **cubrir la base con instancias reservadas y el pico con instancias bajo demanda**, reservando la capacidad sobrante para trabajos por lotes.

Existe una **cuarta dimensión del coste** que se olvida sistemáticamente: el **tráfico de salida** (*egress*). La entrada de datos suele ser gratuita y la salida se factura por gigabyte, lo que penaliza precisamente las operaciones de **extracción masiva** —es decir, las de **salida del proveedor**—. Es la materialización económica de la séptima falacia y el motivo de que el Reglamento de Datos haya intervenido sobre estas tarifas [DEUTSCH] [DATAACT].

### 3.2. Plataforma como Servicio (PaaS)

#### 3.2.1. Concepto y entornos de desarrollo y ejecución

La definición del NIST: la capacidad que se proporciona al consumidor es **desplegar sobre la infraestructura de nube aplicaciones creadas o adquiridas por él**, desarrolladas con lenguajes de programación, bibliotecas, servicios y herramientas **soportados por el proveedor**. El consumidor **no gestiona ni controla la infraestructura subyacente**, incluidos **red, servidores, sistemas operativos ni almacenamiento**, pero **sí controla las aplicaciones desplegadas** y, posiblemente, los ajustes de configuración del entorno de alojamiento [NIST145].

Dicho en términos operativos: el cliente **entrega código** y la plataforma se ocupa de construirlo, desplegarlo, ejecutarlo, escalarlo, parchearlo y vigilarlo. Desaparece la figura del administrador de sistemas para esa carga concreta.

Lo que un PaaS aporta habitualmente:

- **Entorno de ejecución** gestionado para uno o varios lenguajes, con sus versiones mantenidas y parcheadas.
- **Canalización de construcción y despliegue**: compilar, ejecutar pruebas, publicar, con estrategias de despliegue sin corte (*blue-green*, canario).
- **Escalado automático** por métricas de aplicación.
- **Gestión de configuración y secretos** externalizada respecto del código.
- **Registro, métricas y trazas** integrados.
- **Servicios de respaldo** enchufables: bases de datos, colas, caché, correo, almacenamiento.

Las **variantes** de PaaS que conviene distinguir:

| Variante | Qué entrega el cliente | Nota |
|---|---|---|
| **PaaS de aplicación** clásico | Código fuente | La plataforma construye la imagen |
| **CaaS** (contenedores como servicio) | **Imagen de contenedor** | Frontera IaaS-PaaS; el cliente controla más |
| **FaaS** (funciones, *serverless*) | **Una función** y su disparador | Facturación por invocación y milisegundos |
| **iPaaS** (integración como servicio) | Flujos de integración | Sustituye al ESB tradicional |
| **DBaaS** | Esquema y datos | Base de datos gestionada: copias y réplica incluidas |

> **[DATO CLAVE]** **Sin servidor** (*serverless*) **no significa que no haya servidores**: significa que el cliente **no los ve, no los dimensiona y no paga por ellos cuando no se usan**. Se factura por **número de invocaciones y tiempo de ejecución**. Su contrapartida técnica es el **arranque en frío** (*cold start*): la primera invocación tras un periodo de inactividad tarda más porque hay que preparar el entorno.

> **[EJERCICIO RESUELTO]** *De las tres piezas del portal de cita previa, ¿cuál encaja mejor en FaaS?*
>
> **Solución.** La **notificación**. Es una tarea **corta**, **puntual**, **disparada por un evento** («solicitud registrada»), **sin estado** y con un volumen extremadamente desigual: cero durante meses y decenas de miles en unas horas. Pagar por invocación es exactamente el modelo adecuado. En cambio, la **validación de empadronamiento** conviene que sea un servicio permanente con caché caliente —el arranque en frío arruinaría la experiencia el día del pico— y la **gestión de solicitudes** requiere estado y transacciones, propias de un servicio de aplicación con base de datos gestionada.

#### 3.2.2. Middleware gestionado y servicios de plataforma

El **middleware** es la capa de software que se sitúa entre el sistema operativo y las aplicaciones y les presta servicios comunes: servidores de aplicaciones, gestores de mensajes, servidores web, cachés, motores de reglas y de procesos. En un modelo tradicional, alguien de la casa lo instala, lo configura, lo parchea y lo vigila. En PaaS, **el proveedor lo hace y lo garantiza mediante un acuerdo de nivel de servicio**.

Los servicios de plataforma más habituales y lo que cambia al consumirlos gestionados:

| Servicio | Lo que deja de hacer el cliente | Lo que sigue siendo suyo |
|---|---|---|
| **Base de datos gestionada** | Instalar, parchear, replicar, programar copias | **El esquema, las consultas, los índices y los datos** |
| **Cola o bus gestionado** | Operar el intermediario, escalarlo | **El diseño de los mensajes y la idempotencia** |
| **Caché gestionada** | Operar el clúster | **Qué se cachea y durante cuánto** |
| **Motor de contenedores gestionado** | Operar el plano de control | **Las imágenes, los manifiestos y los límites de recursos** |
| **Identidad como servicio** | Operar el directorio y la federación | **El modelo de roles y permisos** |
| **Análisis y aprendizaje automático** | Operar la infraestructura de cálculo | **Los datos, el modelo y su gobernanza** |

> **[DATO CLAVE]** La regla que resume PaaS: **el proveedor se hace cargo del software de base; el cliente sigue siendo responsable del diseño y de los datos**. Un servicio de base de datos gestionada hace copias, pero **no impide que el cliente borre una tabla**: la recuperación ante un error del cliente sigue exigiendo que el cliente haya definido su política de retención y haya **probado la restauración** [CCN823].

El precio de PaaS es la **pérdida de flexibilidad y el riesgo de dependencia**: se depende de las versiones de lenguaje que la plataforma soporte, de sus límites de ejecución y de sus API propietarias. Cuanto más se apoya la aplicación en servicios exclusivos del proveedor, más difícil es moverla. La mitigación práctica consiste en **preferir servicios basados en tecnologías abiertas** —una base de datos relacional estándar frente a un almacén propietario, contenedores conformes con la OCI frente a formatos exclusivos— y en **aislar el acceso a esos servicios detrás de interfaces propias** dentro del código.

### 3.3. Software como Servicio (SaaS)

#### 3.3.1. Concepto y distribución de software sobre la nube

La definición del NIST: la capacidad que se proporciona al consumidor es **utilizar las aplicaciones del proveedor** que se ejecutan sobre una infraestructura de nube. Las aplicaciones son accesibles desde diversos dispositivos cliente a través de una interfaz de cliente ligero, como un **navegador web**, o de una interfaz de programa. El consumidor **no gestiona ni controla la infraestructura subyacente** —red, servidores, sistemas operativos, almacenamiento **ni siquiera las capacidades de la aplicación individual**—, con la posible excepción de **ajustes de configuración de la aplicación específicos del usuario** [NIST145].

El cambio respecto del modelo tradicional de software es total:

| Aspecto | Software instalado | SaaS |
|---|---|---|
| Adquisición | **Licencia perpetua** + mantenimiento anual | **Suscripción** por usuario o por consumo |
| Contabilidad | Inversión (inmovilizado) | **Gasto corriente** |
| Instalación | En servidores propios o en cada puesto | Ninguna: navegador |
| Actualización | Proyecto de migración, con versiones antiguas conviviendo | **Continua y para todos a la vez** |
| Versión | Cada cliente puede tener la suya | **Una sola versión** para todos |
| Personalización | Modificación del código posible | **Configuración**, no modificación |
| Datos | En casa | **En el proveedor** |

> **[DATO CLAVE]** La consecuencia más importante y menos intuitiva de SaaS: **el cliente pierde el control del calendario de versiones**. El proveedor actualiza para todos a la vez; no hay opción de «quedarse en la versión anterior» mientras se valida. Por eso en el sector público los contratos SaaS deben incluir **preaviso de cambios funcionales**, **entornos de prueba** y **compromisos de compatibilidad de las integraciones**.

La **personalización** es el otro punto crítico. Un SaaS bien diseñado se adapta por **configuración** (campos, flujos, roles, plantillas, idioma, marca) y por **extensión** (llamadas a API, ganchos, complementos), pero **no por modificación del código**, porque el código es común a todos los inquilinos. Cuando una Administración exige un comportamiento que el producto no contempla, las salidas son tres, y solo una es buena: **adaptar el procedimiento** al producto, **pagar un desarrollo** que el fabricante incorpore a su producto estándar, o **construir una capa propia alrededor** —cara y frágil—.

#### 3.3.2. Arquitectura multinquilino y licenciamiento

La **multitenencia** es lo que hace económicamente posible el SaaS: una **misma instancia** de la aplicación sirve a **muchos inquilinos** (organizaciones cliente), cuyos datos permanecen **lógicamente aislados**. Hay tres modelos de aislamiento, y la elección tiene consecuencias directas de coste, seguridad y capacidad de cumplimiento [ISO17788]:

| Modelo | Cómo se separan los datos | Aislamiento | Coste por inquilino | Personalización |
|---|---|---|---|---|
| **Silo** | **Todo separado**: aplicación y base de datos propias por inquilino | **Máximo** | **Alto** | Alta |
| **Puente** (*bridge*) | Aplicación compartida, **base de datos o esquema propio** por inquilino | Medio-alto | Medio | Media |
| **Agrupado** (*pooled*) | Todo compartido; los registros llevan un **identificador de inquilino** | Lógico | **Mínimo** | Baja |

En el modelo agrupado, el aislamiento depende **enteramente del software**: basta con que una consulta olvide filtrar por el identificador de inquilino para que una organización vea datos de otra. Por eso los proveedores serios aplican el filtrado en la capa de acceso a datos o en la propia base de datos (seguridad a nivel de fila), y no confían en que cada consulta lo recuerde.

> **[DATO CLAVE]** El **«ruido del vecino»** (*noisy neighbour*) es el riesgo característico de la multitenencia agrupada: un inquilino que consume recursos de forma desmedida degrada el servicio de los demás. Se mitiga con **cuotas, límites de tasa y aislamiento de recursos**, y se cubre contractualmente con el **acuerdo de nivel de servicio**.

En cuanto al **licenciamiento**, las modalidades habituales son:

- **Por usuario nombrado**: se paga por cada persona dada de alta, la use o no. Predecible; caro si hay muchos usuarios esporádicos.
- **Por usuario concurrente**: se paga por el número máximo de personas conectadas a la vez. Adecuado para turnos y para usuarios ocasionales.
- **Por consumo**: transacciones, expedientes, documentos, gigabytes.
- **Por niveles de funcionalidad** (*tiers*): la misma aplicación con conjuntos de funciones distintos.
- **Gratuito con límites** (*freemium*), poco frecuente en el sector público por los requisitos de contrato y de tratamiento de datos.

En contratación pública, la modalidad elegida condiciona el expediente: un contrato **por usuario nombrado** permite un presupuesto cerrado y plurianual; uno **por consumo** exige estimar el volumen y prever un mecanismo de control del gasto, además de plantear la duda de si el objeto es un contrato de servicios de tracto sucesivo con precio variable [LCSP].

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** Una herramienta de firma electrónica en modo SaaS para 3.000 empleados municipales, de los cuales solo unos 400 firman a diario: la licencia **por usuario nombrado** obliga a pagar 3.000 aunque 2.600 firmen dos veces al año. Con **usuario concurrente** o **por firma realizada** el coste se ajusta al uso real. La elección del modelo de licencia puede mover el precio de un contrato en un factor de cinco sin cambiar ni una línea del pliego técnico.

### 3.4. Otros modelos de servicio (XaaS)

**XaaS** —*anything as a service*, «cualquier cosa como servicio»— designa la proliferación de denominaciones comerciales construidas sobre el mismo patrón. **Ninguna de ellas es una categoría del NIST**, que mantiene sus tres modelos; la mayoría son especializaciones de PaaS o de SaaS. Aun así conviene conocerlas porque aparecen en pliegos y en enunciados.

| Sigla | Nombre | Qué entrega | Dónde encaja |
|---|---|---|---|
| **FaaS** | Función como servicio | Ejecución de código por evento, sin gestionar servidores | PaaS |
| **CaaS** | Contenedores como servicio | Ejecución y orquestación de contenedores | Entre IaaS y PaaS |
| **CaaS** | Comunicaciones como servicio | Voz, vídeo, mensajería integradas | SaaS · categoría **ISO/IEC 17788** |
| **DBaaS** | Base de datos como servicio | Motor de base de datos gestionado | PaaS |
| **DSaaS** | Almacenamiento de datos como servicio | Capacidad de almacenamiento | IaaS · categoría **ISO/IEC 17788** |
| **NaaS** | Red como servicio | Conectividad y funciones de red | IaaS · categoría **ISO/IEC 17788** |
| **CompaaS** | Cómputo como servicio | Capacidad de proceso | IaaS · categoría **ISO/IEC 17788** |
| **DaaS** | Escritorio como servicio | Puesto de trabajo virtual completo | SaaS (sobre IaaS/PaaS) · **Tema 28** |
| **DaaS** | Datos como servicio | Conjuntos de datos consumibles por API | SaaS |
| **SECaaS** | Seguridad como servicio | Antivirus, filtrado, gestión de eventos de seguridad | SaaS |
| **IDaaS** | Identidad como servicio | Directorio, federación, doble factor | PaaS/SaaS |
| **BPaaS** | Proceso de negocio como servicio | Un proceso completo externalizado (nóminas, cobros) | Sobre SaaS |
| **MLaaS / AIaaS** | Aprendizaje automático o IA como servicio | Modelos y API de inferencia | PaaS/SaaS |

> **[DATO CLAVE]** Cuidado con la ambigüedad de dos siglas: **CaaS** puede significar *containers* o *communications*, y **DaaS** puede significar *desktop* o *data*. En un enunciado, el sentido lo fija el contexto; en una respuesta escrita, conviene **desarrollar la sigla** para no dar lugar a duda. Y recordar siempre que el NIST solo reconoce **tres** modelos de servicio: el resto son etiquetas de mercado [NIST145] [ISO17788].
---

## 4. Modelos de despliegue en Cloud Computing

Si los modelos de servicio responden a **«qué gestiona cada uno»**, los modelos de despliegue responden a **«para quién es la nube y quién la controla»**. El NIST define **cuatro**: privada, comunitaria, pública e híbrida [NIST145]. Nótese que los cuatro son **ortogonales** a los tres modelos de servicio: cabe una **IaaS privada**, un **SaaS público**, una **PaaS comunitaria** o cualquier otra combinación.

| Modelo | ¿Para quién? | ¿Quién la posee y opera? | ¿Dónde está? |
|---|---|---|---|
| **Privada** | **Una sola organización** (con varias unidades o consumidores) | La organización, un tercero o una combinación | Dentro **o fuera** de sus instalaciones |
| **Comunitaria** | Una **comunidad específica** de organizaciones con intereses comunes | Una o varias de ellas, un tercero o una combinación | Dentro o fuera de las instalaciones |
| **Pública** | **Uso abierto al público general** | Una empresa, una entidad académica o pública, o una combinación | En las instalaciones **del proveedor** |
| **Híbrida** | Combinación de **dos o más** de las anteriores | Cada una conserva su titularidad | Mixto |

> **[DATO CLAVE]** Dos precisiones del NIST que se confunden a menudo. **Primera**: una nube **privada no tiene por qué estar en las instalaciones del cliente** ni ser propiedad suya; puede estar alojada y operada por un tercero. Lo que la define es que **su uso está reservado a una sola organización**. **Segunda**: en la nube **híbrida**, las infraestructuras que se combinan **siguen siendo entidades separadas y distintas**, unidas por una tecnología —normalizada o propietaria— que permite **la portabilidad de datos y de aplicaciones** entre ellas [NIST145].

### 4.1. Nubes públicas

#### 4.1.1. Características, multitenencia y modelo de responsabilidad compartida

La **nube pública** es infraestructura aprovisionada para uso **abierto al público general**, existente en las instalaciones del proveedor. Sus rasgos:

- **Escala masiva** y catálogo de servicios muy amplio, del cómputo básico a la inteligencia artificial.
- **Multitenencia intensiva**: la infraestructura física se comparte con clientes desconocidos.
- **Pago por uso** sin inversión inicial, con capacidad percibida como ilimitada.
- **Presencia geográfica** organizada en **regiones** (áreas geográficas) y, dentro de ellas, **zonas de disponibilidad** (centros de datos independientes en energía, refrigeración y red, unidos por baja latencia).
- **Innovación continua**: el catálogo crece y evoluciona sin intervención del cliente.

Sus **inconvenientes** son la contrapartida exacta: menor control, dependencia del proveedor, coste variable y difícil de acotar, y la necesidad de justificar el cumplimiento normativo sobre una infraestructura que no se posee.

El **modelo de responsabilidad compartida** es el concepto vertebrador de la nube pública y aparece en prácticamente todos los supuestos prácticos. Su enunciado canónico distingue:

- **Seguridad *de* la nube**: responsabilidad del **proveedor**. Instalaciones físicas, energía, refrigeración, hardware, red troncal, hipervisor y aislamiento entre inquilinos.
- **Seguridad *en* la nube**: responsabilidad del **cliente**. Datos, clasificación, cifrado, identidades y permisos, configuración de los servicios, red virtual, sistema operativo (en IaaS), código de la aplicación y copias de seguridad.

Y la línea que separa ambas **se desplaza según el modelo de servicio**: en IaaS el cliente asume el sistema operativo; en PaaS deja de asumirlo; en SaaS solo le quedan datos, identidades y configuración. Lo que **nunca** cambia es lo esencial:

> **[DATO CLAVE]** En todos los modelos y en todos los despliegues, **el cliente conserva siempre**: (1) sus **datos** y su clasificación, (2) la **gestión de identidades y accesos** y (3) la **configuración** de los servicios que contrata. Y en el plano jurídico, la Administración conserva **siempre** la condición de **responsable del tratamiento**: la responsabilidad **se puede delegar operativamente, pero no se externaliza jurídicamente** [RGPD] [CCN823].

Los tres errores de configuración que concentran la mayor parte de los incidentes reales en nube pública, y que conviene poder enumerar en un supuesto [NIST144]:

1. **Almacenamiento de objetos expuesto públicamente** por una política de acceso mal fijada. Es la causa de la mayoría de las filtraciones masivas publicadas.
2. **Credenciales y claves incrustadas en el código** o en repositorios accesibles, en lugar de un gestor de secretos con rotación.
3. **Permisos excesivos**: usuarios y servicios con privilegios de administrador «por comodidad», en lugar de **mínimo privilegio** y roles temporales.

A ellos se añaden dos de operación: **ausencia de registro y monitorización** propios —confiar en que el proveedor «ya lo guarda»— y **falta de copias de seguridad verificadas** bajo control del cliente.

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** Si el portal del caso de referencia guarda la documentación aportada por la ciudadanía en un almacén de objetos y esa política de acceso se deja en «lectura pública», el resultado es una **brecha de datos personales** notificable a la Agencia Española de Protección de Datos en **72 horas** (art. 33 RGPD) y comunicable a las personas afectadas si el riesgo es alto (art. 34). El proveedor **no** habrá incumplido nada: la configuración es responsabilidad del cliente. Este es el ejemplo que mejor explica por qué el reparto de responsabilidades debe estar escrito **en el pliego**, no supuesto.

### 4.2. Nubes privadas

#### 4.2.1. Infraestructura dedicada y modalidades de gestión

La **nube privada** es infraestructura aprovisionada para el uso **exclusivo de una sola organización**, que puede comprender varios consumidores —por ejemplo, distintas áreas o distritos—. Puede ser **propiedad de la organización, de un tercero o de una combinación de ambos**, ser **gestionada** por cualquiera de ellos y estar **dentro o fuera** de sus instalaciones [NIST145].

De ahí salen cuatro modalidades que conviene distinguir:

| Modalidad | Ubicación | Propiedad y operación | Nota |
|---|---|---|---|
| **Interna** (*on-premise*) | Centro de datos propio | Propia | Máximo control; toda la inversión y la operación son propias |
| **Alojada** (*hosted*) | Centro de datos de un tercero | Propia (o mixta) | Se externaliza el edificio, no la gestión |
| **Gestionada** (*managed*) | Propia o de un tercero | **Operada por un tercero** | Se externaliza la operación conservando la exclusividad de uso |
| **Privada virtual** (*VPC*) | Nube **pública** | Del proveedor | Segmento **lógicamente aislado** dentro de una nube pública; **no es una nube privada del NIST**, aunque el marketing lo sugiera |

> **[DATO CLAVE]** Una **nube privada virtual** dentro de una nube pública **no convierte esa nube en privada**: el hardware sigue siendo compartido y el modelo de despliegue sigue siendo **público**. La palabra clave del NIST para «privada» es **«uso exclusivo de una sola organización»**, y eso se refiere al **uso de la infraestructura**, no al aislamiento lógico de la red [NIST145].

Las **ventajas** de la nube privada son el control físico y jurídico sobre la ubicación de los datos, el ajuste fino al cumplimiento normativo, la previsibilidad del coste y la posibilidad de atender requisitos de latencia o de integración con sistemas heredados difíciles de mover. Sus **inconvenientes** son la **inversión inicial**, la **elasticidad limitada por la capacidad instalada** —la nube privada solo es elástica hasta donde llega su hardware— y la necesidad de un equipo capaz de operar la plataforma.

Tecnológicamente, una nube privada se construye sobre un hipervisor más una capa de gestión que aporte autoservicio, catálogo, cuotas y medición; el ejemplo de código abierto de referencia es **OpenStack** [OPENSTACK], y existen equivalentes comerciales de los fabricantes de virtualización.

> **[DATO CLAVE]** El criterio para saber si un centro de datos virtualizado **es** una nube privada: comprobar si tiene **portal de autoservicio, catálogo, aprovisionamiento automático, elasticidad y medición del consumo por unidad**. Si el usuario tiene que abrir un tique y esperar a que alguien cree la máquina a mano, **es virtualización, no nube** [NIST145].

### 4.3. Nubes híbridas

#### 4.3.1. Integración, interoperabilidad y portabilidad de cargas

La **nube híbrida** es la composición de **dos o más infraestructuras de nube distintas** (privada, comunitaria o pública) que **permanecen como entidades separadas**, pero están **unidas por tecnología normalizada o propietaria que permite la portabilidad de datos y aplicaciones** [NIST145]. La definición es exigente: no basta con «tener cosas en los dos sitios»; hace falta un mecanismo real de portabilidad.

Los **patrones híbridos** habituales, que son la respuesta esperada en un supuesto de diseño:

| Patrón | En qué consiste | Cuándo se usa |
|---|---|---|
| **Desbordamiento** (*cloud bursting*) | La carga base se atiende en la nube privada y **el pico desborda** a la pública | Campañas, matrículas, plazos de presentación |
| **Reparto por sensibilidad** | Datos y procesos sensibles en privada; presentación y cargas públicas en pública | Padrón en privada, portal de consulta en pública |
| **Recuperación ante desastres** | La nube pública actúa como emplazamiento alternativo | Continuidad sin duplicar un centro de datos |
| **Desarrollo y pruebas en pública** | Los entornos no productivos se crean y destruyen en la pública | Ahorro y agilidad, con **datos anonimizados** |
| **Modernización progresiva** (*strangler fig*) | Se van sacando funciones del sistema heredado hacia la nube, una a una | Migración de aplicaciones antiguas sin corte [FOWLER] |
| **Analítica en la nube** | Los datos operativos permanecen en casa; el análisis se hace en la pública sobre copias | Cuadros de mando, datos abiertos |

Para que cualquiera de estos patrones funcione hacen falta cuatro condiciones técnicas: **conectividad** de baja latencia y ancho de banda suficiente (VPN de sitio a sitio o enlace dedicado); **identidad federada**, de modo que las mismas cuentas y roles valgan en ambos lados; **red y direccionamiento coherentes**, sin solapamientos; y **observabilidad unificada**, porque un incidente que atraviesa dos nubes es indiagnosticable con dos consolas separadas.

Conviene precisar dos conceptos que la norma **ISO/IEC 19941** separa con cuidado y que conviene estudiar como pareja [ISO19941]:

- **Interoperabilidad**: capacidad de dos sistemas de **intercambiar información y usarla**. Se descompone en interoperabilidad de **transporte**, **sintáctica** (formatos), **semántica** (significado), de **comportamiento** (mismo efecto) y de **políticas** (compatibilidad de reglas de seguridad y cumplimiento).
- **Portabilidad**: capacidad de **mover** algo de un entorno a otro. Se distingue la portabilidad **de datos** (llevarse la información en un formato utilizable) de la portabilidad **de aplicación** (que el programa se ejecute en el destino sin reescribirlo).

> **[DATO CLAVE]** **Interoperabilidad ≠ portabilidad**. Interoperar es **hablarse** estando cada uno en su sitio; portar es **mudarse**. Un sistema puede ser perfectamente interoperable y absolutamente imposible de portar, que es justamente la situación que produce la dependencia del proveedor [ISO19941].

Las palancas técnicas de portabilidad, por orden de eficacia demostrada: **contenedores conformes con la OCI** [OCI], **orquestación con Kubernetes** como capa común [K8S], **infraestructura como código** con herramientas que soporten varios proveedores [TERRAFORM], **formatos de datos abiertos** y **preferencia por servicios basados en tecnologías estándar** frente a servicios propietarios equivalentes. La palanca jurídica es el **Reglamento de Datos**, que se trata en el §5.1.

> **[EJERCICIO RESUELTO]** *Diseñe la arquitectura del portal de cita previa como nube híbrida y justifique el reparto.*
>
> **Solución.** **En nube privada municipal**: el **padrón** y el **registro electrónico**, por ser sistemas de misión crítica, con datos personales de todo el censo y con integraciones internas numerosas; y la **base de datos de solicitudes**, si su categorización ENS lo aconseja. **En nube pública**: la **capa de presentación** del portal —que es lo que recibe la avalancha—, la **CDN** y las **imágenes estáticas**, el servicio de **notificación** en modo función y los **entornos de desarrollo y pruebas** con datos anonimizados. **Unión**: enlace dedicado o VPN de sitio a sitio, identidad federada y una **cola** que amortigüe el tráfico de la parte pública hacia la privada, de modo que un pico en el portal **no se propague** al padrón. **Patrón aplicado**: desbordamiento más reparto por sensibilidad. **Lo que hay que anotar en el supuesto**: la conectividad entre ambas nubes se convierte en un **punto único de fallo** y debe tener redundancia, y el volumen de tráfico de salida ha de estimarse antes, no después.

### 4.4. Nubes comunitarias y estrategia Multicloud

La **nube comunitaria** es infraestructura aprovisionada para el uso exclusivo de una **comunidad específica de consumidores de organizaciones que comparten intereses** —misión, requisitos de seguridad, políticas o consideraciones de cumplimiento—. Puede ser propiedad de una o varias de las organizaciones de la comunidad, de un tercero o de una combinación, y estar dentro o fuera de sus instalaciones [NIST145].

Es el modelo **naturalmente adecuado al sector público**, y no es una figura teórica: comparte la lógica de los **servicios compartidos** de la Administración. Sus ventajas son el **reparto de costes** entre entidades que no podrían asumirlos por separado, la **homogeneidad** de cumplimiento —todas están sujetas al mismo marco— y el **efecto de cohesión territorial**, al permitir que entidades locales pequeñas alcancen un nivel de digitalización que no lograrían solas [ESTRATEGIA-CLOUD]. Sus dificultades son de **gobernanza**: hay que decidir quién manda, cómo se reparten los costes, cómo se priorizan las peticiones y qué ocurre cuando dos miembros quieren cosas incompatibles.

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** Los servicios comunes que la Administración General del Estado pone a disposición de las entidades locales —registro electrónico común, plataforma de intermediación de datos, identificación y firma, notificaciones— funcionan en la práctica como una **nube comunitaria del sector público**: infraestructura de uso exclusivo de un conjunto de organizaciones con requisitos y marco normativo comunes. Que un Ayuntamiento consuma esos servicios en lugar de construirlos es la aplicación directa del principio de **reutilización** del art. 157 de la Ley 40/2015 [L40-2015].

La **estrategia multicloud** consiste en usar **servicios de varios proveedores de nube**, normalmente varias nubes públicas. No es un modelo de despliegue del NIST, sino una **decisión de aprovisionamiento**.

> **[DATO CLAVE]** **Multicloud no es lo mismo que híbrida.** *Híbrida* = combinación de **modelos de despliegue distintos** (típicamente privada + pública) unidos por portabilidad. *Multicloud* = **varios proveedores del mismo tipo**, habitualmente varias nubes públicas. Ambas cosas pueden darse a la vez: una entidad con nube privada propia y servicios en dos proveedores públicos es **híbrida y multicloud** simultáneamente.

Las **motivaciones** del multicloud son cuatro: reducir la **dependencia** de un proveedor y mejorar la posición negociadora; aprovechar el **mejor servicio de cada uno**; cumplir requisitos de **resiliencia** que exigen no depender de un único operador; y responder a **exigencias regulatorias** o de soberanía. Sus **costes** son igualmente concretos: multiplicar el conocimiento necesario en el equipo, gestionar identidades y seguridad en varias consolas, pagar tráfico entre nubes y renunciar en buena medida a los servicios diferenciales de cada proveedor si se busca el **mínimo común denominador** para poder moverse.

Existen dos variantes que conviene diferenciar: el **multicloud por reparto**, en el que cada carga vive en el proveedor que mejor le encaja y no se mueve —el caso habitual y razonable—, y el **multicloud por portabilidad activa**, en el que la misma carga puede ejecutarse en cualquiera de ellos. El segundo es mucho más caro y solo se justifica cuando existe una exigencia real de continuidad frente a la pérdida de un proveedor completo. La **Estrategia** española recoge la idea en su definición de *multi-cloud*: emplear varios proveedores de nube pública para un mismo servicio **de manera que no exista dependencia de un proveedor** para un servicio concreto [ESTRATEGIA-CLOUD].
---

## 5. Cloud Computing en la Administración Pública

> **Material complementario.** El enunciado oficial de este tema no nombra este apartado. Se mantiene porque sitúa la materia en el Ayuntamiento y en la normativa que le aplica, pero lo exigible es lo que enumera el título del tema.

Esta sección es la que convierte el tema en un tema **de oposición a la Administración** y no en un tema genérico de tecnología. La idea que la vertebra es sencilla de enunciar y difícil de aplicar: **una Administración puede externalizar la infraestructura, pero no puede externalizar la responsabilidad**. Todo lo que sigue —ENS, protección de datos, soberanía, estrategia— son maneras de hacer operativa esa frase.

### 5.1. Marco regulatorio y de seguridad

#### 5.1.1. Esquema Nacional de Seguridad y cumplimiento Cloud

El **Esquema Nacional de Seguridad**, regulado por el **Real Decreto 311/2022, de 3 de mayo**, es el marco de referencia obligatorio [ENS]. Para el uso de servicios en la nube importan cinco piezas.

**1. El ámbito alcanza al proveedor privado.** El **artículo 2.3** establece que el real decreto se aplica también a los sistemas de información de las **entidades del sector privado** cuando, de acuerdo con la normativa aplicable y **en virtud de una relación contractual**, presten servicios o provean soluciones a las entidades del sector público para el ejercicio por estas de sus competencias y potestades administrativas; incluida la obligación de contar con la **política de seguridad** del artículo 12.

> **[DATO CLAVE]** El **art. 2.3 del RD 311/2022** es el precepto que hay que citar cuando un supuesto pregunte «¿puede el Ayuntamiento contratar a un proveedor de nube que no cumpla el ENS?». La respuesta es **no**: el ENS **se extiende contractualmente** al proveedor privado, y los pliegos deben exigirlo y **acreditarlo**, no darlo por supuesto [ENS].

**2. Categorización del sistema.** El **artículo 40** establece las **categorías de seguridad** —**BÁSICA, MEDIA y ALTA**—, que se determinan valorando el impacto de un incidente sobre las **cinco dimensiones de seguridad** del Anexo I: **disponibilidad, integridad, confidencialidad, autenticidad y trazabilidad**, cada una en tres niveles (bajo, medio, alto). La categoría del sistema es la que corresponde a la **dimensión más exigente**. Esta valoración es el **primer paso** de cualquier proyecto de nube: determina qué medidas del Anexo II son exigibles y, como se verá, condiciona incluso dónde pueden estar los datos.

**3. Las medidas específicas del Anexo II.** El marco operacional del Anexo II incluye dos grupos directamente aplicables:

| Medida | Contenido |
|---|---|
| `op.ext.1` | **Contratación y acuerdos de nivel de servicio** con el prestador externo |
| `op.ext.2` | **Gestión diaria** del servicio prestado por terceros: seguimiento, indicadores, incidencias |
| `op.ext.3` | **Protección de la cadena de suministro** (medida **nueva** del RD 311/2022) |
| `op.ext.4` | **Interconexión de sistemas**: control de los enlaces con sistemas ajenos |
| `op.nub.1` | **Protección de los servicios en la nube**: exigencias específicas cuando el servicio se presta en la nube |

> **[DATO CLAVE]** El grupo **`op.nub`** («servicios en la nube») es una **novedad del RD 311/2022** respecto del ENS anterior, igual que lo son `op.ext.3` (cadena de suministro) y `op.ext.4` (interconexión de sistemas). Que el ENS haya tenido que crear un grupo propio para la nube es, en sí mismo, un dato significativo: refleja que el uso de servicios en la nube dejó de ser excepcional [ENS].

**4. Auditoría y conformidad.** El **artículo 31** exige una **auditoría regular ordinaria al menos cada dos años**, además de auditorías extraordinarias cuando se produzcan modificaciones sustanciales. El **artículo 38** regula los **procedimientos de determinación de la conformidad**: los sistemas de categoría **BÁSICA** solo requieren **autoevaluación** para declarar la conformidad, mientras que los de categoría **MEDIA** y **ALTA** requieren **auditoría de certificación** por una entidad acreditada. Existe además una previsión específicamente pensada para el mundo cloud: el **artículo 30.4** contempla condiciones específicas de evaluación y auditoría para las implementaciones locales de productos, sistemas o servicios **originariamente prestados en la nube o en forma remota**.

> **[DATO CLAVE]** El binomio que hay que memorizar: **BÁSICA → autoevaluación y declaración de conformidad; MEDIA y ALTA → auditoría y certificación** por entidad acreditada, con **periodicidad de al menos dos años** (arts. 31 y 38 del RD 311/2022) [ENS].

**5. Las guías del CCN.** La guía **CCN-STIC 823, «Utilización de servicios en la nube»**, es el documento de cabecera: identifica las medidas y requisitos que debe cumplir el proveedor y **cómo se reparten las responsabilidades entre cliente y proveedor según el modelo de servicio** —cuestión decisiva, porque en SaaS el cliente apenas puede implantar medidas técnicas y debe apoyarse en la conformidad acreditada del proveedor— [CCN823]. Se complementa con la **CCN-STIC 105**, que publica el **Catálogo de Productos y Servicios de Seguridad TIC (CPSTIC)**, donde figuran los productos **aprobados** y los productos y servicios **cualificados** para su uso en sistemas sujetos al ENS, incluidos servicios en la nube [CCN105]; y con las guías **803** (valoración de sistemas), **804** (implantación) y **809** (declaración y certificación de conformidad) [CCN800].

Junto al ENS operan otras normas de seguridad aplicables al contexto cloud: la **Directiva NIS2** (Directiva (UE) 2022/2555), que incluye expresamente a los **proveedores de servicios de computación en nube** entre las entidades sujetas y cuya transposición al ordenamiento español seguía en tramitación al redactar este tema [NIS2]; y el **Reglamento (UE) 2019/881** (*Cybersecurity Act*), que crea el marco europeo de certificación de la ciberseguridad y da cobertura al futuro esquema **EUCS** para servicios en la nube [CSA-EU].

> **[RELACIÓN CON OTROS TEMAS]** Los **principios básicos del Esquema Nacional de Seguridad y del Esquema Nacional de Interoperabilidad** son el objeto completo del **Tema 39**, y los **conceptos de seguridad de los sistemas de información**, las amenazas, las técnicas criptográficas y la firma digital, el del **Tema 32**. Aquí el ENS se trata **solo** en lo que condiciona la contratación y el uso de servicios en la nube.

#### 5.1.2. Protección de datos de carácter personal y garantía de soberanía

Cuando el servicio en la nube trata **datos personales**, se superpone el **RGPD** con estas consecuencias:

**1. El proveedor es encargado del tratamiento (art. 28 RGPD).** La Administración sigue siendo **responsable del tratamiento** y el proveedor actúa **por cuenta de ella y siguiendo sus instrucciones**. El artículo 28 exige un **contrato o acto jurídico por escrito** que fije objeto, duración, naturaleza y fin del tratamiento, tipo de datos y categorías de interesados, y que imponga al encargado, entre otras: tratar los datos **solo siguiendo instrucciones documentadas**; garantizar la **confidencialidad** del personal; aplicar las medidas de seguridad del artículo 32; **no subcontratar** sin autorización del responsable; asistir al responsable en la atención de derechos y en las brechas; y, al finalizar, **devolver o suprimir** los datos a elección del responsable, incluidas las copias [RGPD].

> **[DATO CLAVE]** Las **subencargas** son el punto ciego habitual de los contratos de nube: un proveedor de SaaS se apoya casi siempre en un proveedor de infraestructura, que a su vez usa terceros. El art. 28.2 y 28.4 exige **autorización** del responsable y que el subencargado quede sujeto a **las mismas obligaciones**. Un pliego correcto exige la **lista de subencargados**, el derecho a **oponerse** a nuevas incorporaciones y un **preaviso** [RGPD].

**2. Evaluación de impacto (art. 35 RGPD).** El paso a la nube de un tratamiento a gran escala, o de categorías especiales de datos, suele exigir una **evaluación de impacto relativa a la protección de datos** previa, con consulta al **delegado de protección de datos**.

**3. Transferencias internacionales (arts. 44-50 RGPD).** Si los datos van a tratarse fuera del Espacio Económico Europeo —o si personal del proveedor establecido fuera puede acceder a ellos, lo que **también es una transferencia**— hace falta una base del capítulo V: **decisión de adecuación**, **cláusulas contractuales tipo**, **normas corporativas vinculantes** o una excepción del artículo 49. Tras la sentencia **Schrems II** (STJUE C-311/18, de 16 de julio de 2020), que anuló el *Privacy Shield*, no basta con firmar cláusulas: hay que **evaluar el marco jurídico del país de destino** y adoptar **medidas complementarias** si la protección no es equivalente [SCHREMSII]. Desde el **10 de julio de 2023** existe una **decisión de adecuación** para el **Marco de Privacidad de Datos UE-EE. UU.** aplicable a las entidades estadounidenses **certificadas** en él [DPF].

> **[DATO CLAVE]** Tres precisiones. **Primera**: el **acceso remoto** desde un tercer país por personal de soporte **es una transferencia internacional**, aunque los servidores estén en la Unión. **Segunda**: la **decisión de adecuación** UE-EE. UU. de 2023 solo ampara a las entidades **certificadas** en el Marco, no a cualquier empresa estadounidense. **Tercera**: el **cifrado con claves gestionadas exclusivamente por la Administración** es la medida complementaria más eficaz, porque un acceso al dato cifrado sin clave no revela información [SCHREMSII] [DPF].

**4. Datos no personales.** El **Reglamento (UE) 2018/1807** consagra la **libre circulación de datos no personales** dentro de la Unión y prohíbe con carácter general los requisitos de **localización** de datos, salvo por motivos justificados de seguridad pública. Es la norma que hay que citar cuando un supuesto plantea si se puede exigir que los datos «estén en España»: la respuesta genérica es que **dentro de la UE no cabe exigir localización nacional** de datos no personales, salvo justificación de seguridad pública [R2018-1807].

**5. Cambio de proveedor y portabilidad: el Reglamento de Datos.** El **Reglamento (UE) 2023/2854** (*Data Act*) dedica su **capítulo VI (arts. 23 a 31)** al **cambio entre servicios de tratamiento de datos**. Impone obligaciones de eliminación de obstáculos precontractuales, comerciales, técnicos y contractuales al cambio; exige que el contrato recoja por escrito los derechos del cliente y las obligaciones del proveedor en el proceso de cambio; y, de manera especialmente relevante, su **artículo 29** establece la **retirada progresiva de las tarifas de cambio** (*switching charges*): en el periodo transitorio los proveedores solo pueden repercutir costes reducidos directamente vinculados al cambio, y **a partir del 12 de enero de 2027 no podrán imponer ninguna tarifa de cambio** [DATAACT].

> **[DATO CLAVE]** La fecha **12 de enero de 2027** y el concepto de **tarifas de cambio** del art. 29 del Reglamento (UE) 2023/2854 son datos memorizables: es la norma que ataca directamente la **dependencia del proveedor** convirtiendo en derecho lo que hasta ahora era una barrera económica —el coste de sacar los datos— [DATAACT].

**6. Soberanía del dato.** Más allá de la protección de datos personales, la preocupación por la **soberanía** responde a que un proveedor sujeto a la legislación de un tercer país puede verse obligado por ella a entregar datos, con independencia de dónde estén almacenados. La **Estrategia de servicios en la nube híbrida para las Administraciones Públicas** lo formula con criterios explícitos: que **los datos sensibles de la Administración no se transfieran fuera de la Unión Europea**; que **los datos manejados por sistemas de categoría ALTA del ENS solo puedan ser manejados por empresas a las que se aplique de manera exclusiva la jurisdicción comunitaria**; que las autoridades de terceros países **no puedan acceder de manera incontrolada**; y que la disponibilidad de las infraestructuras pueda preservarse **incluso ante tensiones geopolíticas** [ESTRATEGIA-CLOUD].

> **[DATO CLAVE]** El criterio de la Estrategia española sobre **categoría ALTA del ENS** —solo empresas sujetas de manera **exclusiva** a jurisdicción comunitaria— es el enunciado más concreto y citable sobre soberanía del dato en la Administración española, y **combina** las dos normativas: la categorización viene del ENS y la exigencia de jurisdicción, de la política de soberanía [ESTRATEGIA-CLOUD] [ENS].

En el plano europeo, la respuesta institucional a la soberanía se articula en iniciativas de federación de infraestructuras y de servicios de nube y en proyectos importantes de interés común europeo, orientados a construir capacidad propia y reglas comunes de portabilidad y transparencia [GAIAX].

### 5.2. Estrategia de adopción Cloud en la Administración General del Estado

#### 5.2.1. Principios de preferencia Cloud y transformación digital

El documento de referencia es la **«Estrategia de servicios en la nube híbrida para las Administraciones Públicas»**, publicada en **diciembre de 2022** por el entonces Ministerio de Asuntos Económicos y Transformación Digital, dentro del **Plan de Digitalización de las Administraciones Públicas 2021-2025** —integrado a su vez en la agenda **España Digital 2026** y financiado con cargo al Plan de Recuperación, Transformación y Resiliencia—. Se vincula a la **medida 7** del Plan (Servicio de Infraestructuras Cloud) y a la **medida 9** (Centro de Operaciones de Ciberseguridad), con una inversión declarada de **854 millones de euros** para la Administración del Estado y las Administraciones territoriales [ESTRATEGIA-CLOUD] [PLAN-DIGITAL].

Su estructura es **7 pilares y 19 iniciativas**, y es un contenido excelente para memorizar porque es cerrado y verificable:

| # | Pilar | Idea central |
|---|---|---|
| 1 | **Nube híbrida por diseño** | Ampliar la nube privada existente y promover la conexión e interoperabilidad con distintos proveedores (i1, i2) |
| 2 | **Catálogo de servicios creciente** | Crear la «tienda» de NubeSARA, intermediar la oferta privada homologada y ampliar el catálogo periódicamente (i3, i4, i5) |
| 3 | **Política de provisión «nube híbrida primero»** (*hybrid first*) | Priorizar los servicios en la nube frente a la inversión en infraestructura propia y crear instrumentos de contratación adecuados (i6, i7) |
| 4 | **Soberanía del dato** | Guía de análisis de riesgos en la nube según el ENS y criterios para la contratación centralizada (i8, i9) |
| 5 | **Orientación al dato** | Integrar la plataforma del dato de la AGE con NubeSARA y proporcionar herramientas de analítica (i10, i11) |
| 6 | **Evolución de sistemas hacia la nube híbrida** | Transformar los centros hacia soluciones en nube híbrida, consolidar la nube privada y fijar criterios de distribución de cargas (i12, i13, i14) |
| 7 | **Nube segura** | Certificación ENS de las infraestructuras de nube, capacidades de ciberseguridad, evolución del Centro de Operaciones, Red Nacional de SOC y guías CCN-STIC por modelo de servicio (i15 a i19) |

> **[DATO CLAVE]** El principio español **no es «cloud first» sin matices, sino «nube híbrida primero»** (*hybrid first*): priorizar el aprovisionamiento de servicios basados en la nube frente a las soluciones tradicionales, **en un modelo híbrido** que combina la nube privada de la Administración con proveedores externos. Y la cifra a retener: **7 pilares y 19 iniciativas** [ESTRATEGIA-CLOUD].

Los **desafíos** que la propia Estrategia identifica —y que son la mejor guía para redactar un supuesto— son seis: **autonomía tecnológica**, **soberanía del dato**, **redundancia y resiliencia**, **interoperabilidad**, **protección de datos** y **ciberseguridad** [ESTRATEGIA-CLOUD].

Este marco estratégico se apoya en una base normativa previa: la **Ley 40/2015** impone a las Administraciones relacionarse entre sí por medios electrónicos y consagra los principios de **interoperabilidad, seguridad y reutilización** de sistemas y aplicaciones (arts. 156-158) [L40-2015]; la **Ley 39/2015** garantiza el derecho de la ciudadanía a relacionarse electrónicamente [L39-2015]; y el **Real Decreto 203/2021** desarrolla el funcionamiento del sector público por medios electrónicos [RD203-2021]. La contratación de estos servicios se somete, además, a la **Ley 9/2017 de Contratos del Sector Público**, cuyos pliegos son el instrumento en el que se materializan las exigencias de ENS, protección de datos, niveles de servicio y reversión [LCSP].

#### 5.2.2. Servicios consolidados e infraestructuras públicas

La pieza de infraestructura más citada es **NubeSARA**: la **solución de nube privada** de la Administración General del Estado, desplegada por la **Secretaría General de Administración Digital en 2015** sobre la red **SARA** —la red que interconecta a las Administraciones españolas y las conecta con las redes europeas—. Según la Estrategia, alberga de forma parcial la infraestructura de cómputo de **22 organismos y entidades** vinculados o dependientes de **11 ministerios**, y dispone de un **catálogo de servicios** con **coste conocido y acuerdos de nivel de servicio asociados**, con las actividades más relevantes de provisión **automatizadas**. El paso siguiente previsto es convertir ese catálogo en una **Tienda de Soluciones, Servicios y Aplicaciones**, a modo de *marketplace*, abierta a las distintas Administraciones Públicas e integrando proveedores externos [ESTRATEGIA-CLOUD].

> **[DATO CLAVE]** **NubeSARA** = nube **privada** de la AGE, **desplegada en 2015** por la **SGAD**, con catálogo de servicios **IaaS y PaaS**, costes conocidos y **acuerdos de nivel de servicio**. Evoluciona hacia una **«tienda»** o *marketplace* de soluciones para todas las Administraciones. No confundir la **red SARA** (la red de interconexión) con **NubeSARA** (la nube desplegada sobre ella) [ESTRATEGIA-CLOUD].

Junto a ella, la Administración española lleva años prestando **servicios comunes en modalidad de nube** que constituyen, de hecho, la aplicación práctica del modelo: soluciones de **registro** (ORVE/GEISER/SIR), la **Plataforma de Intermediación de Datos**, los servicios de **identificación y firma** y la **factura electrónica**, cuyo éxito la propia Estrategia atribuye precisamente al despliegue de una solución **en modalidad nube para todas las Administraciones Públicas** [ESTRATEGIA-CLOUD]. El efecto buscado es doble: **reutilización** —evitar que cada entidad construya lo mismo— y **cohesión territorial**, permitiendo que las entidades locales con menos recursos alcancen un nivel de digitalización equivalente.

La **decisión de qué se lleva a la nube y cómo** no debería ser una decisión de moda, sino el resultado de un análisis. El esquema de decisión habitual, que resume todo el tema y sirve directamente para un supuesto práctico, tiene seis pasos:

1. **Categorizar el sistema conforme al ENS** (art. 40) y valorar las cinco dimensiones. De aquí sale la primera restricción, incluida la de soberanía para categoría ALTA.
2. **Determinar si hay datos personales** y de qué categoría, y decidir si procede evaluación de impacto.
3. **Analizar la carga**: perfil de demanda (¿hay picos?), latencia tolerable, volumen de datos, integraciones con sistemas internos y ciclo de vida previsto.
4. **Elegir el modelo de servicio** aplicando la regla de subir tanto como el control exigido permita: **SaaS si existe producto adecuado; PaaS si hay que desarrollar; IaaS solo si hace falta controlar el sistema operativo**.
5. **Elegir el modelo de despliegue** con la política de **nube híbrida primero**, comprobando antes si el servicio ya existe en el **catálogo público** —principio de reutilización— antes de contratarlo fuera.
6. **Diseñar la salida antes de la entrada**: formatos exportables, plan de reversión probado, estimación del tráfico de salida y cláusulas de cambio de proveedor conforme al Reglamento de Datos.

> **[DATO CLAVE]** El orden importa: **primero se categoriza y se analiza el dato, después se elige la tecnología**. Un supuesto que empiece eligiendo proveedor y termine preguntándose si los datos podían salir de la Unión está mal resuelto aunque la arquitectura sea impecable [ENS] [ESTRATEGIA-CLOUD].

> **[EJEMPLO DE APLICACIÓN EN EL AYTO]** Aplicando el esquema al caso de referencia del tema: el **padrón** es un sistema con datos personales de toda la población, alta exigencia de integridad y de confidencialidad y numerosas integraciones internas → nube **privada** municipal, sin discusión. El **portal de cita previa**, que solo recoge y muestra datos de la persona solicitante, tiene un pico brutal y previsible y ninguna integración compleja del lado de la presentación → capa pública elástica con **PaaS**, cola de amortiguación hacia dentro y validación contra el padrón limitada y cacheada. Las **notificaciones** → función bajo demanda. Los **entornos de pruebas** → nube pública con datos anonimizados. Y antes de contratar nada: comprobar si el registro, la identificación y la notificación **ya están disponibles** como servicios comunes de la Administración, porque construir de nuevo lo que ya existe es la forma más cara de resolver el problema [L40-2015] [ESTRATEGIA-CLOUD].

> **[RELACIÓN CON OTROS TEMAS]** El tema conecta hacia atrás con el **Tema 22** (cliente/servidor, multicapa y servicios web), el **Tema 26** (almacenamiento y copias), el **Tema 28** (virtualización) y el **Tema 30** (administración de redes de área local); y hacia adelante con el **Tema 32** (seguridad de los sistemas de información y criptografía), el **Tema 34** (TCP/IP), el **Tema 35** (HTTP, HTTPS y TLS), el **Tema 36** (seguridad perimetral, acceso remoto seguro y VPN) y el **Tema 39** (ENS y ENI). La **accesibilidad y la confidencialidad en el puesto de usuario** corresponden al **Tema 25**.
