# Tema 31 — Checklist de Validación

> **Título oficial**: Paradigmas de computación distribuida y Servicios en Cloud. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.
> **Versión**: v1.0 — Pendiente validación
> **Fecha**: 2026-08-26
> **Revisores**: María y Ana (eTrivium) · revisión técnica IAM (Jesús Cuadrado)
> **Instrucciones**: marcar cada ítem. Los cambios no se guardan en la web (imprimir o exportar a PDF si se desea fijarlos).

---

## 1. Cobertura del temario oficial

- [ ] **Fundamentos de la computación distribuida**: concepto, objetivos, transparencias y las ocho falacias — §1.1
- [ ] **Arquitecturas distribuidas**: cliente-servidor y multicapa, SOA y microservicios, P2P y dirigida por eventos — §1.2
- [ ] **Modelos de comunicación distribuida**: síncrona (sockets, RPC, SOAP, REST) y asíncrona (colas, publicación-suscripción, flujos), consistencia y CAP — §1.3
- [ ] **Definición y características esenciales** de la nube: NIST SP 800-145, ISO/IEC 17788 y los cinco actores — §2.1
- [ ] **Evolución desde la computación distribuida**: tiempo compartido, cliente-servidor, malla, computación de utilidad, virtualización, borde — §2.2
- [ ] **Tecnologías habilitadoras**: virtualización, contenedores, funciones, orquestación e infraestructura como código — §2.3
- [ ] **Ventajas y retos tecnológicos**, con la aritmética del coste — §2.4
- [ ] **IaaS**: recursos de cómputo, almacenamiento y red; abstracción del hardware, elasticidad y modalidades de facturación — §3.1
- [ ] **PaaS**: entornos de desarrollo y ejecución, variantes (CaaS, FaaS, iPaaS, DBaaS) y middleware gestionado — §3.2
- [ ] **SaaS**: distribución sobre la nube, multitenencia y licenciamiento — §3.3
- [ ] **Otros modelos (XaaS)** con su encaje en los tres modelos del NIST — §3.4
- [ ] **Nubes públicas**: multitenencia y modelo de responsabilidad compartida — §4.1
- [ ] **Nubes privadas**: infraestructura dedicada y las cuatro modalidades de gestión — §4.2
- [ ] **Nubes híbridas**: patrones, integración, interoperabilidad y portabilidad — §4.3
- [ ] **Nubes comunitarias y multicloud** — §4.4
- [ ] **Marco regulatorio y de seguridad**: ENS y cumplimiento cloud; protección de datos y soberanía — §5.1
- [ ] **Estrategia de adopción en la AGE**: principios de preferencia cloud y servicios e infraestructuras consolidadas — §5.2

## 2. Contenido teórico

- [ ] El nivel de profundidad (5 secciones, 42 epígrafes, ~17.600 palabras) es adecuado para C1 (¿hay que ampliar o recortar alguna sección?)
- [ ] El **equilibrio entre la primera mitad (§1, computación distribuida) y la segunda (§2-§5, cloud)** es el correcto: ¿debería pesar más el cloud, dado que ocupa tres de los cuatro bloques del enunciado oficial?
- [ ] Las **distinciones nucleares** quedan nítidas: distribuido / paralelo / centralizado; migración / reubicación; replicación / concurrencia; SOA / microservicios; comando / evento; síncrono / asíncrono; escalabilidad / elasticidad; vertical / horizontal; máquina virtual / contenedor / función; IaaS / PaaS / SaaS; privada / comunitaria / pública / híbrida; híbrida / multicloud; nube privada / nube privada virtual; interoperabilidad / portabilidad; virtualización / nube
- [ ] Las **cifras del NIST** son correctas: 5 características esenciales, 3 modelos de servicio, 4 modelos de despliegue; 5 actores en la SP 500-292
- [ ] Los datos del **ENS (RD 311/2022)** son correctos y están verificados contra el texto consolidado del BOE: **art. 2.3** (sector privado prestador), **art. 31** (auditoría al menos cada dos años), **art. 38** (autoevaluación para categoría BÁSICA), **art. 40** (categorías), **art. 30.4** (evaluación de implementaciones locales de servicios originariamente prestados en la nube), grupos **`op.ext`** y **`op.nub`** del Anexo II
- [ ] Las referencias al **RGPD** son correctas: arts. 5, 25, 28, 30, 32, 33-34, 35 y 44-50; **Schrems II** (C-311/18, 16-07-2020) y decisión de adecuación del **10-07-2023**
- [ ] Los datos del **Reglamento (UE) 2023/2854** son correctos: capítulo VI, arts. 23-31; art. 29 y fecha del **12 de enero de 2027**
- [ ] Los datos de la **Estrategia de servicios en la nube híbrida** (diciembre de 2022) son correctos y están verificados contra el documento oficial: **7 pilares y 19 iniciativas**, principio **«nube híbrida primero» (hybrid first)**, **NubeSARA** desplegada por la **SGAD en 2015**, los **seis desafíos** y el criterio de soberanía para **categoría ALTA**
- [ ] La frontera con los Temas 22 (cliente/servidor y servicios web), 26 (almacenamiento y copias), 28 (virtualización), 30 (redes locales), 32 (seguridad), 34-36 (TCP/IP, HTTP/TLS, VPN) y 39 (ENS/ENI) está clara y sin duplicidades innecesarias
- [ ] Los ejemplos Ayto Madrid (portal de cita previa, padrón, registro electrónico, documentación ciudadana) son verosímiles y coherentes entre secciones
- [ ] **Especialmente a validar por el IAM**: el tema **no atribuye al Ayuntamiento de Madrid ninguna infraestructura, contrato o proveedor concreto**. Todos los supuestos se declaran simplificados. Confirmar que esta cautela es suficiente o si el IAM prefiere aportar datos reales

## 3. Fuentes

- [ ] Todas las afirmaciones técnicas están respaldadas por fuente Tier 1 (NIST, ISO/IEC, ITU-T, IETF, OASIS, ENS, RGPD, Reglamento de Datos, Estrategia oficial)
- [ ] Las referencias inline se corresponden con `tema-31-fuentes.md`
- [ ] Los productos y proveedores concretos figuran como **ejemplos ilustrativos**, citados como conjunto y sin preferencia por ninguno; el tema no depende de ninguna marca

## 4. Test (60 preguntas)

- [ ] Cada pregunta tiene una sola respuesta correcta e inequívoca
- [ ] Los distractores (A/B/C) son plausibles
- [ ] La distribución de la opción correcta entre A/B/C está equilibrada (**verificada 20/20/20** por el generador)
- [ ] Las explicaciones y referencias de cada respuesta son correctas
- [ ] El reparto por bloques (P1-P15 computación distribuida, P16-P26 conceptos de nube, P27-P40 modelos de servicio, P41-P50 modelos de despliegue, P51-P60 Administración Pública) es proporcionado al peso de cada sección

## 5. Casos prácticos (3)

- [ ] Realistas y propios del Ayuntamiento (diseño distribuido y modelo de servicio del portal de ayudas; reparto híbrido y soberanía; contratación, cumplimiento y salida del proveedor)
- [ ] Soluciones orientativas técnica y jurídicamente correctas
- [ ] La puntuación de cada caso suma 10 puntos
- [ ] Los cálculos del Caso 1 (40.000 solicitudes/hora frente a 5 asientos/segundo; cola de ~22.000 y drenaje en ~73 minutos) son correctos

## 6. Diagramas (17 SVG)

- [ ] Cada diagrama es correcto y legible (también impreso en blanco y negro)
- [ ] Accesibilidad: todos tienen `role="img"` y `aria-label`
- [ ] Sin desbordes de texto ni colisiones de estilo entre SVG (clases con sufijo único `.t1`-`.t17`, QA de caja contenedora con render en navegador)
- [ ] **D10** (pila de responsabilidad) y **D14** (cuatro modelos de despliegue) son los dos diagramas de memorización directa: verificar celda a celda
- [ ] **D17** (7 pilares y 19 iniciativas) reproduce fielmente el documento oficial de la Estrategia

## 7. Referencias cruzadas a otros temas

- [ ] Validadas contra BOAM 10.032 (T11, T14, T15, T17, T22, T23, T25, T26, T28, T30, T32, T34, T35, T36, T39)
- [ ] Ninguna referencia cruzada cita un enunciado de tema incorrecto

## 8. Calidad editorial

- [ ] Ortografía verificada (tildes y ñ) — sin diacríticos perdidos, también dentro de los `aria-label` de los SVG
- [ ] Coherencia de versión (v1.0) en title, badges, banner y footer del `index.html`
- [ ] El `index.html` abre, navega entre las 8 pestañas y el motor de test funciona
- [ ] Las listas anidadas del Contenido se muestran con sus niveles (sin aplanar)
- [ ] Las tablas comparativas se muestran correctamente y sin markdown crudo filtrado

---

## Observaciones abiertas

_(Espacio para anotaciones de María, Ana y la revisión IAM.)_

- **Obsolescencia**: este es, junto al Tema 24 (desarrollo móvil), el tema **más sensible al paso del tiempo** de la serie técnica. Las secciones §1 y §2.1 son estables durante décadas; §2.3, §3 y §4.4 envejecen en meses porque dependen del catálogo de los proveedores. Se ha mitigado apoyando todo el tema en **definiciones normalizadas** y tratando los productos como ejemplos, pero conviene **revisar §2.3, §3.2 y §3.4 antes de cada convocatoria**.
- **NIS2**: la transposición española (Anteproyecto de Ley de Coordinación y Gobernanza de la Ciberseguridad) **seguía en tramitación** al cerrar esta versión. El tema lo dice expresamente. **Si se publica en el BOE antes del examen, hay que actualizar §5.1.1** con la referencia definitiva.
- **Data Act**: la fecha del **12 de enero de 2027** para la prohibición de tarifas de cambio es posterior a la previsible convocatoria, de modo que en el examen puede preguntarse tanto el régimen transitorio como el definitivo. Se han explicado los dos.
- **Estrategia cloud**: el documento vigente es el de **diciembre de 2022**, ligado al Plan de Digitalización **2021-2025**. Pendiente confirmar si existe una actualización o un plan sucesor publicado, en cuyo caso habría que revisar §5.2 completa. Esta es la pregunta más importante para el IAM de todo el tema.
- **Nivel de detalle del ENS**: se ha citado por artículos y por grupos de medidas (`op.ext`, `op.nub`) en lugar de enumerar el Anexo II medida a medida, porque el detalle exhaustivo corresponde al **Tema 39**. Pendiente confirmar si el IAM prefiere ampliarlo aquí.
- **Terminología en inglés**: se ha mantenido el término inglés entre paréntesis y en cursiva en todos los conceptos que el examen puede citar en su forma original (*resource pooling*, *multi-tenancy*, *cloud bursting*, *hybrid first*, *switching charges*, *serverless*). Confirmar que ese criterio es el deseado y no sobrecarga el texto.
- **Cifras económicas**: el ejercicio resuelto de §2.4 usa precios **inventados y redondeados** con fines didácticos, y así se declara. No proceden de ninguna oferta real ni de ningún contrato del Ayuntamiento.
