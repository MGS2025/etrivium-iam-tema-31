# Tema 31 — Índice

> **Título oficial**: Paradigmas de computación distribuida y Servicios en Cloud. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.
>
> **Bloque**: Parte II — Técnico (Temas 11-40)
> **Nivel**: C1 — Técnico Auxiliar TIC, Ayuntamiento de Madrid

---

## Estructura del tema

1. **Fundamentos de la computación distribuida**
   1.1. Concepto y características de los sistemas distribuidos
   1.1.1. Definición, objetivos y transparencias
   1.1.2. Las ocho falacias de la computación distribuida
   1.2. Arquitecturas distribuidas
   1.2.1. Arquitectura cliente-servidor y multicapa
   1.2.2. Arquitecturas orientadas a servicios y microservicios
   1.2.3. Arquitecturas P2P y orientadas a eventos
   1.3. Modelos de comunicación distribuida
   1.3.1. Comunicación síncrona: sockets, RPC y servicios web
   1.3.2. Comunicación asíncrona: colas, publicación-suscripción y flujos
   1.3.3. Consistencia, teorema CAP y coordinación

2. **Conceptos fundamentales de Cloud Computing**
   2.1. Definición y características esenciales
   2.1.1. La definición del NIST y las cinco características esenciales
   2.1.2. El vocabulario normalizado ISO/IEC 17788 y los actores del modelo
   2.2. Evolución desde la computación distribuida
   2.3. Tecnologías habilitadoras: virtualización y orquestación
   2.3.1. De la máquina virtual al contenedor y a la función
   2.3.2. Orquestación, automatización e infraestructura como código
   2.4. Ventajas y retos tecnológicos

3. **Modelos de servicio en Cloud Computing**
   3.1. Infraestructura como Servicio (IaaS)
   3.1.1. Concepto y recursos de computación, almacenamiento y red
   3.1.2. Abstracción del hardware y elasticidad
   3.2. Plataforma como Servicio (PaaS)
   3.2.1. Concepto y entornos de desarrollo y ejecución
   3.2.2. Middleware gestionado y servicios de plataforma
   3.3. Software como Servicio (SaaS)
   3.3.1. Concepto y distribución de software sobre la nube
   3.3.2. Arquitectura multinquilino y licenciamiento
   3.4. Otros modelos de servicio (XaaS)

4. **Modelos de despliegue en Cloud Computing**
   4.1. Nubes públicas
   4.1.1. Características, multitenencia y modelo de responsabilidad compartida
   4.2. Nubes privadas
   4.2.1. Infraestructura dedicada y modalidades de gestión
   4.3. Nubes híbridas
   4.3.1. Integración, interoperabilidad y portabilidad de cargas
   4.4. Nubes comunitarias y estrategia Multicloud

5. **Cloud Computing en la Administración Pública (material complementario)**
   5.1. Marco regulatorio y de seguridad
   5.1.1. Esquema Nacional de Seguridad y cumplimiento Cloud
   5.1.2. Protección de datos de carácter personal y garantía de soberanía
   5.2. Estrategia de adopción Cloud en la Administración General del Estado
   5.2.1. Principios de preferencia Cloud y transformación digital
   5.2.2. Servicios consolidados e infraestructuras públicas

---

## Conceptos clave para memorizar

| Concepto | Dato clave |
|---|---|
| Sistema distribuido | Conjunto de **ordenadores independientes** que se presenta al usuario como **un único sistema coherente**. Sin memoria compartida ni reloj global: solo hay **paso de mensajes** |
| Transparencias (ISO/ITU RM-ODP) | **Ocho**: acceso, ubicación, migración, reubicación, replicación, concurrencia, fallo y persistencia. Ocultan al usuario que el sistema está repartido |
| Ocho falacias | **La red es fiable · la latencia es cero · el ancho de banda es infinito · la red es segura · la topología no cambia · hay un único administrador · el coste de transporte es cero · la red es homogénea**. Enunciadas por L. P. Deutsch y J. Gosling (Sun Microsystems) |
| Cliente-servidor / multicapa | **2 capas** = cliente grueso + servidor de datos; **3 capas** = presentación + lógica de negocio + datos; **N capas** = añade capas de integración o de servicios. La clave es que cada capa **solo habla con la contigua** |
| SOA frente a microservicios | SOA: servicios **grandes**, integrados por un **bus (ESB)** con lógica en el propio bus. Microservicios: servicios **pequeños y autónomos**, **una base de datos por servicio**, **tuberías tontas y extremos listos** |
| Arquitectura P2P | Todos los nodos son **iguales** (*peers*): a la vez cliente y servidor. No hay punto único de fallo, pero sí problemas de **localización de recursos** (DHT) y de control |
| Arquitectura dirigida por eventos | Los componentes se comunican **publicando hechos ya ocurridos**; el emisor **no sabe** quién le escucha. Acoplamiento mínimo, trazabilidad difícil |
| Síncrono / asíncrono | **Síncrono**: el llamante **espera** la respuesta (RPC, REST, gRPC). **Asíncrono**: deja el mensaje y sigue (colas, *pub/sub*). El asíncrono absorbe picos y sobrevive a caídas del receptor |
| Teorema CAP (Brewer) | Ante una **partición de red (P)** hay que elegir entre **consistencia (C)** y **disponibilidad (A)**. **No** dice «elige 2 de 3» en ausencia de particiones |
| ACID / BASE | **ACID** (atomicidad, consistencia, aislamiento, durabilidad) en bases relacionales; **BASE** (*Basically Available, Soft state, Eventual consistency*) en muchos sistemas distribuidos NoSQL |
| Definición canónica de nube | **NIST SP 800-145**: **5** características esenciales, **3** modelos de servicio, **4** modelos de despliegue |
| Las 5 características esenciales | **Autoservicio bajo demanda · acceso amplio a la red · agrupación de recursos (*resource pooling*) · elasticidad rápida · servicio medido**. Si falta una, **no es nube** |
| Sexta característica (ISO/IEC 17788) | La norma **ISO/IEC 17788 = ITU-T Y.3500** añade la **multitenencia** (*multi-tenancy*) a las cinco del NIST: **seis características clave** |
| Actores (NIST SP 500-292) | **Cinco**: consumidor, proveedor, **intermediario** (*broker*), **portador** (*carrier*) y **auditor** de la nube |
| IaaS / PaaS / SaaS | Regla del examen: **cuanto más arriba, menos gestionas y menos controlas**. IaaS entrega **máquinas, discos y red**; PaaS entrega el **entorno de ejecución**; SaaS entrega la **aplicación terminada** |
| Frontera IaaS-PaaS | En **IaaS** el cliente administra el **sistema operativo**; en **PaaS**, no. Ese es el corte exacto, y es la pregunta clásica |
| Escalabilidad / elasticidad | **Escalabilidad** = capacidad de crecer. **Elasticidad** = crecer **y decrecer automáticamente** siguiendo la demanda, en minutos. Vertical (*scale up*) frente a horizontal (*scale out*) |
| Multitenencia | Una **misma instancia** de software sirve a **varios inquilinos** con sus datos **aislados**. Modelos: **silo** (todo separado), **puente** (mezcla) y **agrupado** (*pooled*, todo compartido) |
| XaaS | FaaS (funciones, *serverless*), CaaS (contenedores), DBaaS, DaaS (escritorio **y** datos, según contexto), NaaS, SECaaS, BPaaS. **No son categorías del NIST**: el NIST solo reconoce tres |
| Nube pública / privada / comunitaria / híbrida | Los **cuatro** modelos de despliegue del NIST. **Híbrida** = dos o más nubes **distintas** que siguen siendo entidades separadas pero se unen por una tecnología que permite **portabilidad de datos y aplicaciones** |
| Multicloud ≠ híbrida | **Multicloud** = varios proveedores del **mismo tipo** (normalmente varias nubes públicas). **Híbrida** = combinación de **modelos de despliegue distintos** (típicamente privada + pública) |
| Responsabilidad compartida | El proveedor responde de la seguridad **de** la nube; el cliente, de la seguridad **en** la nube. **La responsabilidad jurídica del dato NO se externaliza nunca** |
| ENS y sector privado | **Art. 2.3 del RD 311/2022**: el ENS se aplica también a las entidades del **sector privado** que, en virtud de una relación contractual, presten servicios o provean soluciones al sector público |
| Medidas ENS del cloud | Marco operacional: **`op.ext`** (recursos externos: `op.ext.1` a `op.ext.4`) y **`op.nub.1`** (protección de servicios en la nube), grupo **nuevo** introducido por el RD 311/2022 |
| RGPD y nube | El proveedor es **encargado del tratamiento** (**art. 28**): contrato por escrito, **no subcontratación sin autorización**, devolución o supresión de los datos al finalizar. El responsable sigue siendo **la Administración** |
| Transferencias internacionales | Cap. V del RGPD (**arts. 44-50**). Tras la sentencia **Schrems II** (STJUE C-311/18, 16-07-2020), decisión de adecuación **EU-US Data Privacy Framework** de **10 de julio de 2023** |
| Reglamento de Datos (Data Act) | Reglamento (UE) **2023/2854**. Cap. VI (**arts. 23-31**): derecho de **cambio de proveedor**; desde el **12 de enero de 2027** quedan **prohibidas las tarifas de cambio** (*switching charges*) |
| Estrategia española | **Estrategia de servicios en la nube híbrida para las Administraciones Públicas** (diciembre de 2022): **7 pilares y 19 iniciativas**. Principio **«nube híbrida primero»** (*hybrid first*), **no** «cloud first» sin más |
| NubeSARA | Solución de **nube privada** de la AGE desplegada por la **SGAD en 2015** sobre la red **SARA**. Catálogo de servicios IaaS y PaaS con costes y **acuerdos de nivel de servicio**; evoluciona a una **«tienda»** (*marketplace*) de soluciones |
| Soberanía del dato | Criterio de la Estrategia: los datos de sistemas de **categoría ALTA** del ENS solo deben ser manejados por empresas sujetas **exclusivamente a jurisdicción comunitaria** |
