# Tema 31 — Fuentes

> **Título oficial**: Paradigmas de computación distribuida y Servicios en Cloud. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.
>
> **Criterio**: todo dato del contenido cita un **ID** inline (p. ej. `[NIST145]`). Tier 1 = definiciones normalizadas y especificaciones canónicas (NIST, ISO/IEC, ITU-T, IETF, OASIS, OMG), normativa española y europea directamente aplicable (ENS, RGPD/LOPDGDD, Reglamento de Datos) y documentos oficiales de estrategia de la Administración. Tier 2 = documentación de productos, proyectos y prácticas concretas, citada para ilustrar sin atar el tema a un fabricante. Tier 3 = marco administrativo y organizativo de contexto, no citado como contenido técnico.

---

## Tier 1 — Definiciones normalizadas, especificaciones y normativa

| ID | Referencia |
|---|---|
| `[NIST145]` | NIST. *SP 800-145: The NIST Definition of Cloud Computing* (septiembre de 2011). **Definición canónica**: 5 características esenciales, 3 modelos de servicio (IaaS, PaaS, SaaS) y 4 modelos de despliegue (privada, comunitaria, pública, híbrida). |
| `[NIST146]` | NIST. *SP 800-146: Cloud Computing Synopsis and Recommendations* (2012). Desarrollo de cada modelo de servicio, responsabilidades del consumidor y del proveedor, y recomendaciones de adopción. |
| `[NIST292]` | NIST. *SP 500-292: NIST Cloud Computing Reference Architecture* (2011). **Cinco actores**: consumidor, proveedor, intermediario (*broker*), portador (*carrier*) y auditor de la nube. |
| `[NIST293]` | NIST. *SP 500-293: US Government Cloud Computing Technology Roadmap*. Requisitos de interoperabilidad, portabilidad y seguridad para la adopción de la nube en el sector público. |
| `[NIST144]` | NIST. *SP 800-144: Guidelines on Security and Privacy in Public Cloud Computing* (2011). Riesgos de gobernanza, cumplimiento, confianza, aislamiento y respuesta a incidentes en nube pública. |
| `[ISO17788]` | ISO/IEC 17788:2014 = **ITU-T Y.3500**. *Information technology — Cloud computing — Overview and vocabulary*. Vocabulario normalizado; añade la **multitenencia** como sexta característica clave y define las categorías de servicio (CompaaS, CaaS, DSaaS, IaaS, NaaS, PaaS, SaaS). |
| `[ISO17789]` | ISO/IEC 17789:2014 = ITU-T Y.3502. *Cloud computing — Reference architecture*. Roles, subroles y actividades; vistas funcional y de implantación. |
| `[ISO19941]` | ISO/IEC 19941:2017. *Cloud computing — Interoperability and portability*. Tipos de interoperabilidad (transporte, sintáctica, semántica, de comportamiento, de políticas) y de portabilidad (de datos y de aplicación). |
| `[ISO19086]` | ISO/IEC 19086-1:2016 y siguientes. *Cloud computing — Service level agreement (SLA) framework*. Componentes, métricas y objetivos de nivel de servicio (SLO/SQO). |
| `[ISO27017]` | ISO/IEC 27017:2015. *Code of practice for information security controls based on ISO/IEC 27002 for cloud services*. Controles adicionales para proveedor y cliente de servicios en la nube. |
| `[ISO27018]` | ISO/IEC 27018:2019. *Code of practice for protection of personally identifiable information (PII) in public clouds acting as PII processors*. |
| `[ISO27001]` | ISO/IEC 27001:2022 e ISO/IEC 27002:2022. Sistema de gestión de la seguridad de la información y catálogo de controles. |
| `[ISO20000]` | ISO/IEC 20000-1:2018. *Gestión del servicio — Requisitos del sistema de gestión del servicio*. Marco certificable de gestión de servicios TI aplicable a la operación de servicios en la nube. |
| `[RMODP]` | ISO/IEC 10746 = ITU-T X.901-X.904. *Reference Model of Open Distributed Processing (RM-ODP)*. Los cinco puntos de vista y las **ocho transparencias de distribución**. |
| `[RFC7231]` | IETF. *RFC 7231 y RFC 9110: HTTP Semantics*. Base del estilo arquitectónico REST y de las API de gestión de los proveedores de nube. |
| `[RFC6455]` | IETF. *RFC 6455: The WebSocket Protocol*. Canal bidireccional persistente sobre HTTP, usado en interfaces distribuidas en tiempo real. |
| `[RFC9846]` | IETF. *RFC 9846: The Transport Layer Security (TLS) Protocol Version 1.3* (julio de 2026). Obsoleta los RFC 5077, 5246, 6961, 7627, 8422 y 8446: es la especificación vigente de TLS 1.3 y sustituye a la de 2018. Cifrado del canal en tránsito hacia y dentro de la nube. |
| `[RFC8446]` | IETF. *RFC 8446: The Transport Layer Security (TLS) Protocol Version 1.3* (agosto de 2018). Obsoletado por el RFC 9846. Se conserva la referencia porque es la que recogen los temarios al uso. |
| `[RFC7519]` | IETF. *RFC 7519: JSON Web Token (JWT)* y *RFC 6749: The OAuth 2.0 Authorization Framework*. Autenticación y autorización delegadas entre servicios distribuidos. |
| `[RFC5280]` | IETF. *RFC 5280: Internet X.509 Public Key Infrastructure Certificate and CRL Profile*. Certificados de servidor y de cliente en las comunicaciones entre nubes. |
| `[RFC4122]` | IETF. *RFC 4122 (y RFC 9562): Universally Unique IDentifier (UUID)*. Identificación sin coordinación central, patrón habitual en sistemas distribuidos. |
| `[AMQP]` | OASIS. *Advanced Message Queuing Protocol (AMQP) 1.0* = **ISO/IEC 19464:2014**. Protocolo abierto de mensajería con colas, encaminamiento y entrega fiable. |
| `[MQTT]` | OASIS. *MQTT versión 5.0* = **ISO/IEC 20922**. Protocolo ligero de **publicación-suscripción**, referencia en IoT y en el borde (*edge*). |
| `[SOAP]` | W3C. *SOAP Version 1.2* y *Web Services Description Language (WSDL) 1.1/2.0*. Servicios web basados en contrato XML, base histórica de SOA. |
| `[CORBA]` | OMG. *Common Object Request Broker Architecture (CORBA)* e *Internet Inter-ORB Protocol (IIOP)*. Precursor normalizado de la invocación remota de objetos. |
| `[FIELDING]` | R. T. Fielding. *Architectural Styles and the Design of Network-based Software Architectures* (tesis doctoral, UC Irvine, 2000). Definición del estilo **REST** y de sus restricciones. |
| `[BREWER]` | E. Brewer. *Towards Robust Distributed Systems* (PODC 2000) y S. Gilbert y N. Lynch, *Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services* (2002). **Teorema CAP** y su demostración formal. |
| `[LAMPORT]` | L. Lamport. *Time, Clocks, and the Ordering of Events in a Distributed System* (CACM, 1978). Relación «ocurrió antes», relojes lógicos y orden parcial de eventos. |
| `[TANENBAUM]` | A. S. Tanenbaum y M. van Steen. *Distributed Systems: Principles and Paradigms*. Definición de sistema distribuido, objetivos, transparencias y clasificación de arquitecturas. |
| `[DEUTSCH]` | L. P. Deutsch y J. Gosling (Sun Microsystems). *The Eight Fallacies of Distributed Computing*. Enunciado clásico de los supuestos falsos que arruinan los diseños distribuidos. |
| `[ENS]` | Real Decreto **311/2022, de 3 de mayo**, por el que se regula el **Esquema Nacional de Seguridad**. Ámbito (art. 2, incluido el **art. 2.3** para el sector privado prestador), principios básicos, requisitos mínimos, categorización (art. 40), auditoría (art. 31) y medidas del **Anexo II**, entre ellas `op.ext` (recursos externos) y `op.nub.1` (protección de servicios en la nube). |
| `[CCN823]` | Centro Criptológico Nacional. *Guía CCN-STIC 823 — Utilización de servicios en la nube*. Requisitos y medidas exigibles al proveedor de servicios en la nube para el cumplimiento del ENS; reparto de responsabilidades por modelo de servicio. |
| `[CCN105]` | Centro Criptológico Nacional. *Guía CCN-STIC 105 — Catálogo de Productos y Servicios de Seguridad TIC (CPSTIC)*. Relación de productos **aprobados** y de productos y servicios **cualificados** para su uso en sistemas sujetos al ENS. |
| `[CCN800]` | Centro Criptológico Nacional. *Guías CCN-STIC serie 800*, en particular **803** (valoración de sistemas), **804** (implantación del ENS), **809** (declaración y certificación de conformidad) y **817** (gestión de ciberincidentes). |
| `[RGPD]` | Reglamento (UE) **2016/679** (RGPD). Arts. 5 (principios), 24-25 (responsabilidad proactiva y protección desde el diseño), **28** (encargado del tratamiento), 30 (registro de actividades), 32 (seguridad), 33-34 (brechas), 35 (evaluación de impacto) y **44-50** (transferencias internacionales). |
| `[LOPDGDD]` | Ley Orgánica **3/2018**, de 5 de diciembre, de Protección de Datos Personales y garantía de los derechos digitales. Deber de confidencialidad, delegado de protección de datos y régimen de las Administraciones Públicas. |
| `[SCHREMSII]` | Sentencia del Tribunal de Justicia de la UE de **16 de julio de 2020**, asunto **C-311/18** (*Schrems II*). Anulación del *Privacy Shield* y exigencia de evaluar el marco jurídico del país de destino y adoptar medidas complementarias. |
| `[DPF]` | Decisión de Ejecución (UE) 2023/1795 de la Comisión, de **10 de julio de 2023**, sobre la adecuación del **Marco de Privacidad de Datos UE-EE. UU.** (*EU-US Data Privacy Framework*). |
| `[DATAACT]` | Reglamento (UE) **2023/2854** (*Reglamento de Datos* o *Data Act*). Capítulo VI (**arts. 23-31**): cambio entre servicios de tratamiento de datos, retirada progresiva de las **tarifas de cambio** (art. 29) e interoperabilidad. |
| `[R2018-1807]` | Reglamento (UE) **2018/1807**, relativo a un marco para la **libre circulación de datos no personales** en la Unión Europea. Prohibición general de los requisitos de localización de datos y códigos de conducta de portabilidad. |
| `[CSA-EU]` | Reglamento (UE) **2019/881** (*Cybersecurity Act*). Marco europeo de certificación de la ciberseguridad y mandato de ENISA; base del esquema **EUCS** para servicios en la nube, aún en elaboración. |
| `[NIS2]` | Directiva (UE) **2022/2555** (**NIS2**), que incluye expresamente a los **proveedores de servicios de computación en nube** entre las entidades sujetas. Su transposición al ordenamiento español seguía en tramitación en el momento de redactar este tema. |
| `[L40-2015]` | Ley **40/2015**, de 1 de octubre, de Régimen Jurídico del Sector Público. Funcionamiento electrónico del sector público, **Esquema Nacional de Interoperabilidad** y reutilización de sistemas y aplicaciones (arts. 156-158). |
| `[L39-2015]` | Ley **39/2015**, de 1 de octubre, del Procedimiento Administrativo Común de las Administraciones Públicas. Derecho a relacionarse electrónicamente con las Administraciones. |
| `[RD203-2021]` | Real Decreto **203/2021**, de 30 de marzo, por el que se aprueba el Reglamento de actuación y funcionamiento del sector público por medios electrónicos. |
| `[ENI]` | Real Decreto **4/2010**, de 8 de enero, por el que se regula el **Esquema Nacional de Interoperabilidad**, y sus Normas Técnicas de Interoperabilidad. |
| `[LCSP]` | Ley **9/2017**, de 8 de noviembre, de Contratos del Sector Público. Pliegos, prescripciones técnicas y régimen de los contratos de servicios TIC. |
| `[ESTRATEGIA-CLOUD]` | Ministerio de Asuntos Económicos y Transformación Digital. *Estrategia de servicios en la nube híbrida para las Administraciones Públicas* (**diciembre de 2022**, NIPO 094-23-011-5), dentro del Plan de Digitalización de las Administraciones Públicas 2021-2025. **7 pilares y 19 iniciativas**; principio **«nube híbrida primero»**; NubeSARA; soberanía del dato. |
| `[PLAN-DIGITAL]` | Gobierno de España. *Plan de Digitalización de las Administraciones Públicas 2021-2025*, integrado en la agenda *España Digital 2026* y financiado por el Plan de Recuperación, Transformación y Resiliencia. |

## Tier 2 — Tecnologías, plataformas y prácticas concretas

| ID | Referencia |
|---|---|
| `[OCI]` | Open Container Initiative. *Image, Runtime and Distribution Specifications*. Formato normalizado de imagen y de ejecución de contenedores, base de la portabilidad entre nubes. |
| `[K8S]` | Cloud Native Computing Foundation. *Kubernetes Documentation*. Orquestación de contenedores: planificación, autoescalado, servicios, almacenamiento y despliegues declarativos. |
| `[CNCF]` | Cloud Native Computing Foundation. *CNCF Cloud Native Definition* y mapa del ecosistema nativo de nube (contenedores, malla de servicios, observabilidad, GitOps). |
| `[12FACTOR]` | A. Wiggins. *The Twelve-Factor App*. Doce prácticas para aplicaciones desplegables en plataformas como servicio (configuración en el entorno, procesos sin estado, paridad entre entornos). |
| `[FOWLER]` | J. Lewis y M. Fowler. *Microservices: a definition of this new architectural term* (martinfowler.com, 2014) y artículos asociados (*MonolithFirst*, *CircuitBreaker*, *StranglerFigApplication*). |
| `[TERRAFORM]` | HashiCorp *Terraform*, *OpenTofu*, AWS *CloudFormation* y *Ansible*. Herramientas de **infraestructura como código** con estado declarativo e idempotencia. |
| `[KAFKA]` | Apache *Kafka*, *RabbitMQ* y *ActiveMQ*. Plataformas de flujos de eventos y de mensajería por colas, citadas como ejemplos de comunicación asíncrona. |
| `[GRPC]` | *gRPC* y *Protocol Buffers*. Invocación remota de procedimientos binaria sobre HTTP/2, alternativa moderna a CORBA y a SOAP. |
| `[DHT]` | Trabajos sobre tablas *hash* distribuidas (*Chord*, *Kademlia*) y sistemas P2P (BitTorrent, IPFS, cadenas de bloques). Localización de recursos sin servidor central. |
| `[DYNAMO]` | G. DeCandia y otros. *Dynamo: Amazon's Highly Available Key-value Store* (SOSP 2007) y E. Brewer, *CAP Twelve Years Later*. Consistencia eventual y modelo **BASE** en la práctica. |
| `[OPENSTACK]` | *OpenStack* y *Apache CloudStack*. Plataformas de código abierto para construir nubes privadas de tipo IaaS (cómputo, red, almacenamiento y gestión de inquilinos). |
| `[HYPERVISORS]` | Documentación de hipervisores y plataformas de virtualización: KVM/QEMU, VMware vSphere, Microsoft Hyper-V, Xen, Proxmox. Tratados en profundidad en el Tema 28. |
| `[CSP]` | Documentación pública de los grandes proveedores de nube (Amazon Web Services, Microsoft Azure, Google Cloud, Oracle Cloud, IBM Cloud, OVHcloud) sobre modelos de servicio, regiones, zonas de disponibilidad y modelo de responsabilidad compartida. Citada como conjunto, sin preferencia por ninguno. |
| `[SAAS-SUITES]` | Suites de productividad y de gestión en modo SaaS (correo, ofimática colaborativa, CRM, ITSM, gestión de expedientes). Citadas como categoría, no como recomendación. |
| `[GAIAX]` | Asociación *Gaia-X* e *IPCEI-CIS* (proyecto importante de interés común europeo sobre infraestructura y servicios de nube). Iniciativas europeas de federación de nubes y soberanía digital. |
| `[FINOPS]` | FinOps Foundation. *FinOps Framework*. Prácticas de gestión económica del consumo de nube: visibilidad, asignación de costes, optimización y previsión. |
| `[SRE]` | B. Beyer y otros (Google). *Site Reliability Engineering*. Objetivos de nivel de servicio (SLI/SLO/SLA), presupuesto de error y gestión de la fiabilidad en servicios distribuidos. |
| `[EDGE]` | Documentación sobre computación en el borde (*edge computing*), redes de distribución de contenidos (CDN) y computación en la niebla (*fog computing*). |
| `[BENCHMARKS]` | Center for Internet Security. *CIS Benchmarks* y *CIS Controls*, incluidos los específicos de plataformas de nube. Líneas base de configuración segura. |

## Tier 3 — Marco administrativo y organizativo (contexto, no citado como contenido técnico)

| ID | Referencia |
|---|---|
| `[BOAM10032]` | Ayuntamiento de Madrid. *Boletín Oficial del Ayuntamiento de Madrid* núm. 10.032, de 23 de diciembre de 2025. Bases específicas de la convocatoria y **temario oficial** de Técnico Auxiliar de Informática (C1). Usado para validar las referencias cruzadas a otros temas. |
| `[ROGA]` | Reglamento Orgánico del Gobierno y de la Administración del Ayuntamiento de Madrid, de 31 de mayo de 2004. Estructura de Áreas de Gobierno y Distritos (Temas 3 y 4). |
| `[ORD-ADMIN-E]` | Ordenanza de Atención a la Ciudadanía y Administración Electrónica del Ayuntamiento de Madrid. Sede electrónica, registro y tramitación por medios electrónicos. |
| `[TRANSPARENCIA-MAD]` | Portal de datos abiertos y de transparencia del Ayuntamiento de Madrid. Origen de los ejemplos de conjuntos de datos y servicios municipales usados en los supuestos. |
</content>
