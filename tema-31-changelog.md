# Tema 31 — Changelog

> **Título oficial**: Paradigmas de computación distribuida y Servicios en Cloud. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.

---

## v1.2 — 2026-09-06 — Marcado del apartado complementario

**Estado**: pendiente de validación por el IAM.

**Motivo**: criterio de literalidad del título fijado por el IAM (Jesús Cuadrado, 02-09-2026).

### Alcance

- El apartado final que **el enunciado oficial del tema no nombra** queda marcado como **material complementario**, en el índice y al principio del propio apartado, con la advertencia de que lo exigible es lo que enumera el título.
- **Sin cambios de contenido**: el apartado se mantiene íntegro.

---

## v1.1 — 2026-09-06 — Ficha de extensión y tiempo de estudio

**Estado**: sin cambios de contenido. Solo se añade información sobre el propio tema.

**Motivo**: petición del IAM (Jesús Cuadrado, 02-09-2026) al validar el Tema 30. Acepta la extensión de los temas «compuestos» a condición de que se informe de «su extensión en palabras y tiempo estimado de estudio». Al revisarlo se vio que ese dato solo aparecía en 16 de los 40 temas, y que faltaba justo en los más largos.

### Alcance

- Ficha bajo la cabecera del tema, y al final de la pestaña Índice donde esa pestaña existe:
  - **Extensión**: ~18.500 palabras · 17 diagramas · 60 preguntas de test
  - **Tiempo estimado de estudio**: 17-19 horas (primera vuelta completa, sin contar repasos)
- La cifra de palabras de la tabla de entregables se sincroniza con la ficha, para que el tema no muestre dos recuentos distintos.
- Las horas salen de una fórmula común a los 40 temas, para que sean comparables entre sí: contenido a 1.500 palabras/hora (ritmo de estudio activo), diagramas a una hora por cada cinco y test a dos minutos por pregunta. Se publica como intervalo de dos horas.
- Generado con `_tools-qa/ficha_estudio.py`, idempotente y reejecutable tras cualquier regeneración con `build_tNN.py`.

---

## v1.0 — 2026-08-26 — Primera versión

**Estado**: pendiente de validación por María y Ana, y de revisión técnica del IAM (Jesús Cuadrado).

**Motivo**: desarrollo del Tema 31, dentro de la serie de temas técnicos generados desde cero, replicando la estructura y el formato de los Temas 1, 11 y 17-29 ya consolidados. Con él, el bloque técnico queda publicado en **T11-T29 y T31**, con **hueco en T30** (administración de redes de área local), cuyo esqueleto está disponible en `Test_Prompting/temas agosto/30.md`.

### Alcance de la v1.0

| Entregable | Cantidad |
|---|---|
| Contenido teórico | ~17.600 palabras · 5 secciones (fieles al esqueleto oficial) con 42 epígrafes numerados |
| Diagramas SVG inline | 17 (accesibles con `role`/`aria-label`, clases con sufijo único anti-colisión) |
| Banco de preguntas tipo test | 60 preguntas A/B/C con explicación y referencia, balanceadas **20/20/20** (verificado por el generador) |
| Casos prácticos | 3 (diseño distribuido y modelo de servicio del portal de ayudas; reparto híbrido y soberanía del dato; contratación, cumplimiento y salida del proveedor) · 10 puntos cada uno |
| Fuentes Tier 1 | 46 referencias canónicas (NIST, ISO/IEC, ITU-T, IETF, OASIS, OMG, W3C, ENS, CCN-STIC, RGPD, Reglamento de Datos, Estrategia cloud de las AAPP) |

### Decisiones de generación

1. **Sin material de cliente**: solo el esqueleto `Test_Prompting/temas agosto/31.md`. Desarrollado desde fuentes canónicas, todas referenciadas.
2. **Estructura fiel al esqueleto oficial**: sus 5 secciones de primer nivel, 17 subsecciones y 12 epígrafes de tercer nivel, respetados uno a uno. Se han **desdoblado en epígrafes propios** cuatro puntos que el esqueleto dejaba dentro de una subsección sin numerar (las ocho falacias, los actores del modelo, la orquestación e infraestructura como código, y los tipos de comunicación síncrona y asíncrona), para poder citarlos con precisión desde el test. **Ninguna sección ni subsección nueva de primer o segundo nivel.**
3. **Tesis explícita desde las Convenciones**: la nube **no es una tecnología, sino un modelo económico y de consumo montado sobre tecnologías anteriores**. Es lo que explica que el enunciado oficial empiece por los paradigmas de computación distribuida, y lo que separa una respuesta buena de una respuesta de siglas.
4. **Sin fragmentos de código** (misma decisión que T26, T28 y T29): el enunciado no compara lenguajes ni plataformas de desarrollo, sino paradigmas, modelos y normativa. Lo memorizable aquí son **definiciones normalizadas, listas cerradas y preceptos**, no sintaxis. Se ha priorizado en su lugar la **densidad de tablas comparativas** y una **aritmética explícita** en los dos ejercicios resueltos.
5. **Caso de referencia único para todo el tema**: el **portal municipal de cita previa y tramitación de una campaña de ayudas** con un pico de 40.000 solicitudes en la primera hora y un registro electrónico que admite 5 asientos por segundo. Planteado como **supuesto simplificado**, no como descripción de una infraestructura real. Concentra casi todas las dificultades del tema: descomposición, acoplamiento, elasticidad, elección de modelo de servicio y de despliegue, categorización ENS, datos personales, soberanía y salida del proveedor.
6. **Prioridad al dato normalizado frente al dato de producto**, por ser este el tema más expuesto a la obsolescencia junto al T24. Todo se apoya en **NIST SP 800-145** (5-3-4), **ISO/IEC 17788**, **ISO/IEC 19941** y normativa; los proveedores se citan **como conjunto**, sin nombrar catálogos ni precios reales.
7. **Verificación normativa contra fuente primaria**. Comprobados en el texto consolidado del BOE del **RD 311/2022**: el literal del **art. 2.3**, el **art. 31** (auditoría al menos cada dos años), el **art. 38** (autoevaluación para categoría BÁSICA), el **art. 40** (categorías) y el **art. 30.4** (evaluación de implementaciones locales de servicios originariamente prestados en la nube). Comprobada también la existencia de los grupos **`op.ext`** (con `op.ext.3` y `op.ext.4` como medidas nuevas) y **`op.nub`** en el Anexo II.
8. **La Estrategia española, leída del documento oficial y no de resúmenes**. Se ha extraído el texto del PDF de la *Estrategia de servicios en la nube híbrida para las Administraciones Públicas* (diciembre de 2022, NIPO 094-23-011-5) y de ahí proceden: los **7 pilares y las 19 iniciativas** con su enunciado literal, el principio **«nube híbrida primero»** (que el documento oficial rotula literalmente *hibrid first*), los **seis desafíos**, los datos de **NubeSARA** (SGAD, 2015, 22 organismos de 11 ministerios, catálogo con costes y acuerdos de nivel de servicio, evolución a «tienda») y el criterio de **soberanía para categoría ALTA**. Este es el bloque de mayor valor diferencial del tema y el que un temario genérico no tiene.
9. **Corrección de un error frecuente en los temarios**: el principio español **no es «cloud first»**, sino **«nube híbrida primero»**. Se ha hecho explícito con su propio callout porque es exactamente el tipo de matiz que distingue una respuesta memorizada de una respuesta informada.
10. **Reglamento de Datos incorporado como contenido de primer nivel**, no como nota al pie: capítulo VI (arts. 23-31) y, sobre todo, el **art. 29** con la fecha del **12 de enero de 2027**. Es normativa reciente, muy preguntable y directamente conectada con el reto de la dependencia del proveedor que atraviesa todo el tema.
11. **NIS2 declarada como pendiente de transposición** en el momento de redactar, en lugar de darla por transpuesta. Anotado en la validación para su revisión antes de cada convocatoria.
12. **Frontera con temas vecinos cuidada**: la virtualización como tecnología al **Tema 28**; el almacenamiento y las copias al **Tema 26**; cliente/servidor, multicapa y servicios web al **Tema 22**; el desarrollo web al **Tema 23**; las bases de datos y ACID al **Tema 15**; las redes locales y su administración al **Tema 30**; la seguridad y la criptografía al **Tema 32**; TCP/IP, HTTP y TLS a los **Temas 34 y 35**; las VPN y el acceso remoto seguro al **Tema 36**; y el ENS y el ENI al **Tema 39**.
13. **Referencias cruzadas validadas contra BOAM 10.032**: T11, T14, T15, T17, T22, T23, T25, T26, T28, T30, T32, T34, T35, T36 y T39. Todas comprobadas contra el enunciado oficial de cada tema.
14. **Anti-colisión de SVG**: las clases CSS de cada diagrama llevan **sufijo numérico único** (`.t1`…`.t17`, `.h1`…`.h17`), evitando el fallo sistémico de estilos que se filtran de un SVG a otro al estar todos embebidos en la misma página (lección de T5). Los marcadores de flecha llevan también identificador único por diagrama.
15. **Distribución A/B/C fijada ANTES de redactar** (lección de T23, práctica consolidada desde T24): se predefinió la secuencia completa de 60 letras con 20 de cada una y se redactó cada pregunta contra su letra asignada. `build_t31.py` confirma **20/20/20 a la primera**, sin permutaciones correctoras.
16. **Cómputo de extensión medido, no estimado**: la cifra procede de `wc -w` sobre el `.md`.

### QA realizado antes de publicar

- **Integridad del test**: 60 preguntas, 3 opciones únicas por pregunta, texto de la respuesta correcta idéntico a la opción marcada, y referencia presente en todas. Verificado por script.
- **Atribución de los SVG**: comprobado por script que la línea `[Fuente: …]` de los 17 diagramas queda a **9 o más píxeles del borde inferior** del `viewBox` y a **12 o más del último elemento dibujado** (regla aprendida en T28); se ajustó la altura de quince diagramas.
- **Clases CSS**: comprobado que no hay ninguna clase usada y no definida en ninguno de los 17 SVG, y que todos llevan `role="img"` y `aria-label`.
- **Desbordes**: comprobado con Chrome headless y `getBBox` que **ningún texto excede el `viewBox`** ni **la caja que lo contiene** en ninguno de los 17 diagramas, y que la página no genera scroll horizontal.
- **Revisión visual por captura**: se inspeccionaron con captura de pantalla los diagramas con líneas de flujo o gráficos (D3, D5, D6, D10, D11, D16, D17), que es lo único que el `getBBox` no caza. **D11 se rehízo por completo**: la curva de capacidad elástica estaba dibujada exactamente sobre la de demanda real, de modo que se leía como una sola línea a rayas, y sobraba un rectángulo vacío. Ahora hay una sola curva, leyenda explícita de las dos líneas y una nota que explica dónde estaría la capacidad elástica.
- **Motor de test verificado sobre HTTP** (sobre `file://` los scripts no se ejecutan en este entorno headless, lección de T28): 60 preguntas, 180 opciones, corrección de 6 aciertos y 3 fallos = **5.00** con la penalización 1/3, las 60 explicaciones se muestran al corregir y el reinicio deja la nota a 0.00 y ninguna opción marcada.
- **Ortografía**: barrido con hunspell es_ES sobre la prosa y sobre los textos y `aria-label` de los SVG por separado. Sin diacríticos perdidos; todas las palabras marcadas son vocabulario técnico o inglés legítimo.
- **Markdown crudo**: comprobado que el `index.html` generado no contiene asteriscos de negrita sin convertir fuera del CSS y de los bloques de código (bug del conversor detectado en T26 y T27).

### Pendientes para QA / próxima iteración

- Confirmar con el IAM si existe una **actualización de la Estrategia cloud** posterior a diciembre de 2022 o un plan sucesor del Plan de Digitalización 2021-2025. De haberla, hay que rehacer §5.2.
- Revisar §2.3, §3.2 y §3.4 **antes de cada convocatoria**: son los epígrafes que envejecen.
- Actualizar §5.1.1 cuando se publique en el BOE la ley de transposición de **NIS2**.
- Valorar con María y Ana si el peso de §1 (computación distribuida, aproximadamente una cuarta parte del tema) es el adecuado o debe reducirse en favor de §3 y §4.
