# Tema 31 — Casos Prácticos

> **Título oficial**: Paradigmas de computación distribuida y Servicios en Cloud. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.
>
> **Formato**: 3 casos prácticos sobre supuestos del Ayuntamiento de Madrid. Cada caso suma **10 puntos**.
> **Nivel**: C1 — Técnico Auxiliar TIC, Ayuntamiento de Madrid

Los tres casos recorren el supuesto de referencia del tema (ver tema-31-contenido.md, «Convenciones»): el **portal municipal de cita previa y tramitación de una campaña de ayudas**. El **Caso 1** trabaja el **diseño distribuido y la elección del modelo de servicio**; el **Caso 2**, el **modelo de despliegue y la nube híbrida**; y el **Caso 3**, el **cumplimiento normativo, la contratación y la salida del proveedor**. Todos los supuestos son **simplificados** y no describen la infraestructura real del Ayuntamiento de Madrid.

---

## Caso 1 — Diseño distribuido y elección del modelo de servicio

### Enunciado

El Área de Gobierno responsable debe publicar un **portal de solicitud de una línea de ayudas** que abre el **1 de octubre a las 9:00** y cierra veinte días después. Las previsiones estiman **40.000 solicitudes en la primera hora** y unas 120.000 en total; el resto del año el portal no recibe tráfico. Cada solicitud exige: (a) **validar el empadronamiento** contra el padrón municipal, (b) **adjuntar hasta cinco documentos** escaneados, (c) obtener un **justificante con número de referencia y marca de tiempo** y (d) practicar el **asiento en el registro electrónico**, que admite **5 asientos por segundo**. El equipo TIC dispone de dos meses y de tres personas.

### Cuestiones

**Cuestión 1 — Descomposición (2 puntos).** Proponga una descomposición del sistema en servicios y justifique **por qué esos y no más**, indicando qué criterio ha aplicado.

**Cuestión 2 — Comunicación (3 puntos).** Decida para cada interacción si debe ser **síncrona o asíncrona** y justifíquelo con números. Indique qué garantía de entrega asume y qué obligación impone al consumidor.

**Cuestión 3 — Modelo de servicio (3 puntos).** Asigne a cada componente el **modelo de servicio** (IaaS, PaaS, SaaS o FaaS) más adecuado y justifique cada elección. Señale expresamente **dónde queda la frontera de responsabilidad** en cada uno.

**Cuestión 4 — Almacenamiento y elasticidad (2 puntos).** Elija el **tipo de almacenamiento** para los documentos adjuntos y explique qué condición debe cumplir la capa web para poder escalar horizontalmente.

### Solución orientativa

- **C1**: (§1.2.2) **Tres** servicios: **validación de empadronamiento** (consulta al padrón, cacheable), **gestión de solicitudes** (alta, consulta, modificación y justificante) y **notificación** (acuse de recibo y avisos). El criterio no es «cuanto más pequeño mejor», sino **separar lo que tiene ritmos de cambio y de carga distintos**: la validación se dispara el día de la apertura, la notificación al resolverse las solicitudes semanas después, y la gestión de solicitudes es la que soporta el estado y las transacciones. Con tres personas y dos meses, **quince microservicios serían una decisión imposible de operar**: trasladaría toda la complejidad del código a la operación —descubrimiento, versionado de contratos, correlación de trazas, consistencia sin transacciones distribuidas— sin ningún beneficio. Un monolito modular bien separado sería también una respuesta defendible si se justifica.

- **C2**: (§1.3.1, §1.3.2, §3.1.2)

| Interacción | Modo | Justificación |
|---|---|---|
| Navegador → portal | **Síncrona** | El usuario espera la pantalla |
| Portal → validación de padrón | **Síncrona pero acotada y cacheada** | Debe resolverse en la misma pantalla, pero con tiempo de espera corto, reintento limitado y **una sola consulta por solicitud**, no una por campo |
| Portal → registro electrónico | **Asíncrona, con cola** | 40.000 solicitudes/hora frente a 5 asientos/segundo = 18.000/hora: el registro **no puede** absorber el pico |
| Solicitud registrada → notificación | **Asíncrona, por evento** | Nadie espera el correo; permite añadir consumidores sin tocar el emisor |

  **Números**: en la primera hora entran 40.000 y salen 18.000, de modo que la cola acumula unas **22.000 solicitudes**; a 5 por segundo se drenan en unos **73 minutos** una vez cesa el pico. Lo que se conserva es la **garantía jurídica**: la fecha y hora de presentación es la de la solicitud, acreditada por el justificante, no la del asiento. Se asume la garantía **«al menos una vez»**, lo que obliga a que el consumidor del registro sea **idempotente** mediante una clave de desduplicación, para que un reintento no produzca dos asientos.

- **C3**: (§3, §3.1, §3.2)

| Componente | Modelo | Justificación | Frontera de responsabilidad |
|---|---|---|---|
| Capa web del portal | **PaaS** | Hay que desarrollar, pero no hay razón para administrar sistemas operativos con tres personas y dos meses | El proveedor pone el sistema operativo y el tiempo de ejecución; el equipo mantiene **código, datos, identidades y configuración** |
| Base de datos de solicitudes | **PaaS (DBaaS)** | Copias y réplica gestionadas; el equipo conserva esquema, índices, datos y **política de retención probada** | La restauración ante un borrado propio sigue siendo del cliente |
| Notificación | **FaaS** | Tarea corta, disparada por evento, sin estado, con volumen extremadamente desigual | Solo el código de la función |
| Validación de empadronamiento | **PaaS con caché**, no FaaS | El **arranque en frío** arruinaría la experiencia justo en el pico | Código y caché |
| Padrón y registro electrónico | **Sistemas existentes**, no se rehacen | Se consumen como servicios | — |
| Ofimática y firma del personal | **SaaS** si ya existe producto | No hay nada que desarrollar | Datos, identidades y configuración |

  Si se hubiera elegido **IaaS** para la capa web, la diferencia sería que el equipo asumiría el **parcheado y el endurecimiento del sistema operativo** de todas las instancias del pico: es exactamente el trabajo que no se puede permitir en este calendario.

- **C4**: (§3.1.1, §3.1.2) Los documentos adjuntos van a **almacenamiento de objetos**: se escriben una vez, se leen pocas, deben conservarse años y crecen sin límite previsible; guardarlos en discos de bloques obligaría a redimensionar discos y sería más caro y menos durable. Se aplicará una **clase de acceso esporádico o de archivo** pasados unos meses. Para que la capa web escale horizontalmente debe ser **sin estado**: la sesión del usuario y los ficheros temporales **no pueden vivir en la memoria ni en el disco local de una instancia**, sino en una caché o almacén compartido; de lo contrario no se pueden añadir ni retirar instancias sin romper sesiones en curso.

### Criterios de evaluación

| Criterio | Puntos |
|---|---|
| Descomposición razonada con criterio explícito y dimensionada al equipo disponible | 2 |
| Decisión síncrono/asíncrono justificada con números, garantía de entrega e idempotencia | 3 |
| Modelo de servicio correcto por componente, con la frontera de responsabilidad señalada | 3 |
| Almacenamiento de objetos y condición de aplicación sin estado | 2 |

---

## Caso 2 — Modelo de despliegue: nube híbrida para el portal de ayudas

### Enunciado

Aprobado el diseño del Caso 1, hay que decidir **dónde se despliega cada pieza**. El Ayuntamiento dispone de un **centro de proceso de datos propio** con capacidad para unas 40 máquinas virtuales adicionales y una plataforma de virtualización con portal de autoservicio y medición del consumo por unidad. El **padrón** está categorizado como sistema de **categoría MEDIA** del ENS y el **registro electrónico**, como **MEDIA**. La documentación adjunta contiene datos personales, incluidos algunos datos de salud en las ayudas por dependencia, lo que eleva la categorización del sistema de solicitudes a **ALTA** en confidencialidad. La conexión de la sede a internet es de 1 Gbps.

### Cuestiones

**Cuestión 1 — Calificación de la plataforma propia (2 puntos).** ¿La plataforma de virtualización del Ayuntamiento es una **nube privada** según el NIST? Justifique la respuesta con los criterios de la definición.

**Cuestión 2 — Reparto híbrido (3 puntos).** Reparta los componentes entre nube privada y nube pública, indique **qué patrón híbrido** aplica en cada caso y qué **condiciones técnicas** deben cumplirse para que la unión funcione.

**Cuestión 3 — La restricción de la categoría ALTA (2 puntos).** ¿Qué consecuencia tiene sobre la elección del proveedor el hecho de que el sistema de solicitudes sea de **categoría ALTA** en confidencialidad? Cite el criterio aplicable.

**Cuestión 4 — Riesgos del diseño (3 puntos).** Enumere **cuatro riesgos** específicos de esta arquitectura híbrida y una medida para cada uno. Incluya al menos uno de coste y uno de disponibilidad.

### Solución orientativa

- **C1**: (§2.1.1, §4.2.1) **Sí**, siempre que cumpla las **cinco características esenciales**. El enunciado confirma **autoservicio** (portal), **medición** del consumo por unidad y, por definición, **acceso por red** y **agrupación de recursos**. La única que hay que verificar es la **elasticidad**: si el aprovisionamiento y la liberación son automáticos y rápidos, es una nube privada; si crear una máquina exige abrir un tique y esperar a un técnico, **es virtualización, no nube**. Debe señalarse además que su elasticidad **está acotada por la capacidad instalada**: 40 máquinas virtuales adicionales son su techo físico, a diferencia de la capacidad percibida como ilimitada de una nube pública.

- **C2**: (§4.3.1)

| Componente | Dónde | Patrón |
|---|---|---|
| Padrón y registro electrónico | **Privada** | No se mueven: sistemas de misión crítica con muchas integraciones internas |
| Base de datos de solicitudes | **Privada** | Reparto por sensibilidad: contiene datos de salud |
| Documentos adjuntos | **Privada**, o pública **cifrada con claves propias** si se justifica | Reparto por sensibilidad |
| Capa web del portal y CDN | **Pública** | **Desbordamiento**: es la pieza que recibe la avalancha |
| Notificación (función) | **Pública** | Desbordamiento |
| Entornos de desarrollo y pruebas | **Pública**, con **datos anonimizados** | Desarrollo y pruebas en pública |

  **Condiciones técnicas de la unión**: (1) **conectividad** de baja latencia y ancho de banda suficiente mediante VPN de sitio a sitio o enlace dedicado —debe comprobarse que 1 Gbps basta para el tráfico previsto y que **no es el único enlace**—; (2) **identidad federada**, de modo que las mismas cuentas y roles valgan a ambos lados; (3) **red y direccionamiento coherentes**, sin solapamiento de rangos; y (4) **observabilidad unificada**, porque un incidente que atraviesa dos nubes es indiagnosticable con dos consolas separadas. Debe añadirse una **cola de amortiguación** en el sentido pública → privada, para que un pico en el portal **no se propague** al padrón ni al registro.

- **C3**: (§5.1.2) La **Estrategia de servicios en la nube híbrida para las Administraciones Públicas** establece, dentro del pilar de **soberanía del dato**, que los datos manejados por sistemas de **categoría ALTA** del ENS **solo puedan ser manejados por empresas a las que se aplique de manera exclusiva la jurisdicción comunitaria**, y que los datos sensibles de la Administración no se transfieran fuera de la Unión Europea. En consecuencia: o los datos de solicitudes permanecen en la **nube privada municipal**, o el proveedor debe cumplir ese criterio de jurisdicción. Debe añadirse que, al haber **datos de salud** —categoría especial del artículo 9 del RGPD—, procede una **evaluación de impacto** previa (art. 35 RGPD) con intervención del delegado de protección de datos, y que el proveedor será **encargado del tratamiento** (art. 28 RGPD).

- **C4**: (§2.4, §4.1.1, §4.3.1) Bastan cuatro; se ofrecen cinco riesgos válidos con su medida:

| Riesgo | Tipo | Medida |
|---|---|---|
| El enlace entre ambas nubes es **punto único de fallo** | Disponibilidad | Redundancia del enlace por rutas y operadores distintos, y degradación controlada del portal si cae |
| **Tráfico de salida** no previsto al servir documentos desde la nube pública | Coste | Estimar el volumen antes, usar CDN y caché, y fijar alertas de consumo |
| **Configuración incorrecta** del almacenamiento o de los permisos en la nube pública | Seguridad | Mínimo privilegio, gestor de secretos, revisión automática de configuración y registro propio de auditoría |
| **Propagación del pico** de la capa pública hacia el padrón | Disponibilidad | Cola de amortiguación, limitación de tasa y caché de validaciones |
| **Dependencia del proveedor** de la capa pública | Continuidad | Contenedores conformes con la OCI, infraestructura como código y plan de reversión probado |

### Criterios de evaluación

| Criterio | Puntos |
|---|---|
| Aplicación correcta de las cinco características para calificar la plataforma propia, con la reserva sobre elasticidad y techo de capacidad | 2 |
| Reparto híbrido coherente, patrón identificado y las cuatro condiciones técnicas de la unión | 3 |
| Restricción de soberanía por categoría ALTA correctamente citada, con la mención de la evaluación de impacto | 2 |
| Cuatro riesgos pertinentes con medida asociada, incluyendo coste y disponibilidad | 3 |

---

## Caso 3 — Contratación, cumplimiento y salida del proveedor

### Enunciado

El Ayuntamiento va a licitar la **capa pública** del portal de ayudas y el servicio de notificación, con un contrato de **dos años prorrogable**. Durante la fase de preparación del expediente aparecen cuatro cuestiones: (1) un licitador propone un servicio SaaS **alojado en la Unión Europea pero cuya matriz está sujeta a legislación de un tercer país**; (2) otro ofrece un precio muy bajo pero **no acredita conformidad con el ENS**, alegando que «el ENS obliga a la Administración, no a las empresas»; (3) el pliego borrador no dice nada sobre **qué ocurre al terminar el contrato**; y (4) el responsable económico pregunta **cómo se presupuesta** un servicio de pago por uso.

### Cuestiones

**Cuestión 1 — La alegación del licitador (2 puntos).** Responda a la afirmación de que «el ENS obliga a la Administración, no a las empresas», citando el precepto aplicable y qué debe exigirse en el pliego.

**Cuestión 2 — Jurisdicción y datos personales (3 puntos).** Analice el caso del licitador cuya matriz está sujeta a legislación de un tercer país. Indique qué hay que comprobar, qué figura jurídica ocupa el proveedor y qué medidas complementarias caben.

**Cuestión 3 — Cláusulas de salida (3 puntos).** Redacte, en forma de lista, las **exigencias de reversión y cambio de proveedor** que deben incorporarse al pliego, citando la norma europea que las respalda y su calendario.

**Cuestión 4 — Presupuestación (2 puntos).** Explique qué implica presupuestar un servicio de pago por uso y proponga **tres mecanismos** de control del gasto.

### Solución orientativa

- **C1**: (§5.1.1) La alegación es **incorrecta**. El **artículo 2.3 del Real Decreto 311/2022** dispone que el ENS se aplica también a los **sistemas de información de las entidades del sector privado** cuando, de acuerdo con la normativa aplicable y **en virtud de una relación contractual**, presten servicios o provean soluciones a las entidades del sector público para el ejercicio por estas de sus competencias y potestades administrativas, **incluida la obligación de contar con la política de seguridad** del artículo 12. En consecuencia, el pliego debe: exigir **conformidad con el ENS en la categoría que corresponda** al sistema; exigir la **acreditación** de esa conformidad —**declaración** basada en autoevaluación si el sistema es de categoría BÁSICA, **certificación** por entidad acreditada si es MEDIA o ALTA (arts. 31 y 38)—; y prever la **auditoría regular al menos cada dos años** y la obligación de comunicar modificaciones sustanciales. Es aconsejable comprobar además si el servicio figura como **cualificado** en el CPSTIC (guía CCN-STIC 105) y apoyar el reparto de responsabilidades en la **CCN-STIC 823**.

- **C2**: (§5.1.2) Que los servidores estén en la Unión **no resuelve el problema por sí solo**. Hay que comprobar: (a) si existe **acceso a los datos desde el tercer país** —soporte, administración, copias—, porque el **acceso remoto desde un tercer país es una transferencia internacional** sometida al capítulo V del RGPD (arts. 44-50); (b) si el proveedor está **certificado en el Marco de Privacidad de Datos UE-EE. UU.**, en cuyo caso la decisión de adecuación de **10 de julio de 2023** ampara la transferencia, y si no, si se aplican **cláusulas contractuales tipo** con la evaluación del marco jurídico del país de destino exigida por **Schrems II**; (c) la **lista de subencargados**, con derecho a oponerse a nuevas incorporaciones y preaviso, conforme al artículo 28.2 y 28.4; y (d) la **categoría ENS** del sistema, porque si es ALTA opera el criterio de jurisdicción exclusivamente comunitaria de la Estrategia. **Figura jurídica**: el proveedor es **encargado del tratamiento** (art. 28 RGPD) y el Ayuntamiento sigue siendo **responsable**; la responsabilidad se delega operativamente pero **no se externaliza**. **Medidas complementarias**: cifrado con **claves gestionadas exclusivamente por el Ayuntamiento**, seudonimización, minimización de los datos que salen, restricción geográfica del soporte y compromiso de notificación de requerimientos de autoridades extranjeras.

- **C3**: (§2.4, §4.3.1, §5.1.2) Exigencias de reversión que deben constar en el pliego:

1. **Formatos abiertos y documentados** para la exportación de datos y metadatos, con esquema publicado.
2. **Derecho de extracción íntegra** en cualquier momento y a la finalización, con **plazo máximo** y **sin coste**, o con coste acotado durante el periodo transitorio.
3. **Devolución o supresión** de los datos a elección del responsable al terminar el contrato, **incluidas las copias**, con **certificado de borrado** (art. 28.3.g del RGPD).
4. **Periodo de reversión** con solapamiento y **obligación de colaboración** del proveedor saliente con el entrante.
5. **Prueba de reversión** al menos una vez durante la vigencia del contrato: un plan no probado no es un plan.
6. **Portabilidad técnica**: contenedores conformes con la OCI, infraestructura como código y ausencia de dependencias exclusivas no sustituibles.
7. **Lista de subencargados** y preaviso de cambios funcionales que afecten a integraciones.
8. **Continuidad**: qué ocurre en caso de resolución, insolvencia o cambio de control del proveedor.

  **Norma que lo respalda**: el **Reglamento (UE) 2023/2854 (Reglamento de Datos)**, cuyo **capítulo VI (arts. 23-31)** regula el cambio entre servicios de tratamiento de datos y cuyo **artículo 29** establece la retirada progresiva de las **tarifas de cambio**, prohibidas por completo **desde el 12 de enero de 2027**. Hasta entonces solo pueden repercutirse costes reducidos directamente vinculados al proceso de cambio.

- **C4**: (§2.4, §3.3.2) Presupuestar pago por uso significa que el crédito **no se corresponde con una prestación fija**, sino con un **consumo estimado**, lo que obliga a estimar volúmenes, a prever un mecanismo de control y a plantear si el objeto es un contrato de servicios de tracto sucesivo con precio variable, con las cautelas del expediente correspondientes. Tres mecanismos de control del gasto: **(1)** **presupuestos y alertas** por servicio y por unidad responsable, con umbrales de aviso y de bloqueo; **(2)** **etiquetado obligatorio** de todos los recursos por proyecto y unidad, de modo que el consumo sea imputable —lo que es posible precisamente por la característica esencial de **servicio medido**—; y **(3)** **cuotas y límites de tasa** por servicio, más apagado automático de entornos no productivos fuera del horario de trabajo. Puede añadirse una **cobertura de la base con instancias reservadas** para reducir la parte variable, y la práctica de revisión periódica del consumo conocida como FinOps.

### Criterios de evaluación

| Criterio | Puntos |
|---|---|
| Refutación de la alegación con cita del art. 2.3 y régimen de acreditación por categoría | 2 |
| Análisis de jurisdicción completo: transferencia por acceso remoto, figura de encargado y medidas complementarias | 3 |
| Lista de cláusulas de reversión pertinente y cita correcta del Reglamento de Datos con su calendario | 3 |
| Explicación de la presupuestación por consumo y tres mecanismos de control efectivos | 2 |
