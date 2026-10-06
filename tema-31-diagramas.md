# Tema 31 — Catálogo de Diagramas

> **Título oficial**: Paradigmas de computación distribuida y Servicios en Cloud. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.
>
> **Versión**: v1.0
> **Fecha**: 2026-08-26
> **Autor**: ETRIVIUM
> **Formato**: SVG inline (zero-dependencias, escalable, imprimible, accesible con role/aria-label)
> **Paleta**: Ayuntamiento de Madrid #0055a0 (primario) + #d13c3c (alertas) + #2d8659 (ventajas) + #e89822 (callouts)
> **Nota técnica**: las clases CSS de cada SVG llevan sufijo numérico único (`.t1`, `.h1`…) para evitar colisiones de estilos entre los 17 diagramas embebidos en la misma página.

---

## Índice de diagramas

| ID | Título | Sección | Tipo | Formato |
|---|---|---|---|---|
| D1 | Centralizado, paralelo y distribuido: las dos ausencias | §1.1.1 | Comparativa | 680×323 |
| D2 | Las ocho falacias de la computación distribuida | §1.1.2 | Tabla visual | 680×337 |
| D3 | De una a N capas: cliente-servidor y multicapa | §1.2.1 | Capas | 680×340 |
| D4 | SOA frente a microservicios: dónde vive la lógica | §1.2.2 | Comparativa | 680×340 |
| D5 | P2P y arquitectura dirigida por eventos | §1.2.3 | Esquema de red | 680×331 |
| D6 | Comunicación síncrona frente a asíncrona | §1.3.2 | Flujo comparado | 680×353 |
| D7 | Teorema CAP, ACID y BASE | §1.3.3 | Triángulo | 680×341 |
| D8 | Las cinco características esenciales del NIST | §2.1.1 | Bloques | 680×327 |
| D9 | Máquina virtual, contenedor y función | §2.3.1 | Capas comparadas | 680×330 |
| D10 | La pila de responsabilidad: local, IaaS, PaaS y SaaS | §3 | Matriz | 680×405 |
| D11 | Escalabilidad y elasticidad: la curva de capacidad | §3.1.2 | Gráfico | 680×341 |
| D12 | Multitenencia: silo, puente y agrupado | §3.3.2 | Comparativa | 680×331 |
| D13 | El mapa del XaaS sobre los tres modelos del NIST | §3.4 | Mapa | 680×339 |
| D14 | Los cuatro modelos de despliegue del NIST | §4 | Comparativa | 680×343 |
| D15 | Responsabilidad compartida y los errores típicos | §4.1.1 | Esquema | 680×351 |
| D16 | Marco normativo del uso de la nube en la Administración | §5.1 | Capas | 680×373 |
| D17 | Los 7 pilares de la Estrategia de nube híbrida | §5.2.1 | Bloques | 680×361 |

---

## D1 · Centralizado, paralelo y distribuido: las dos ausencias

**Sección**: §1.1.1 — Definición, objetivos y transparencias
**Propósito**: Fijar qué distingue exactamente a un sistema distribuido —ausencia de memoria compartida y de reloj global— y por qué de ahí sale el fallo parcial.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 323" role="img" aria-label="Comparativa entre sistema centralizado, sistema paralelo y sistema distribuido, indicando para cada uno si hay memoria compartida, si hay reloj global, qué ocurre cuando falla un componente y cuál es su objetivo principal">
  <style>.t1{font:700 11px system-ui,sans-serif;fill:#fff}.s1{font:9px system-ui,sans-serif;fill:#fff}.d1{font:9px system-ui,sans-serif;fill:#333}.h1{font:700 13px system-ui,sans-serif;fill:#0055a0}.k1{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f1{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h1">Centralizado, paralelo y distribuido</text>
  <rect x="20" y="34" width="205" height="36" rx="5" fill="#888"/><text x="122" y="50" text-anchor="middle" class="t1">CENTRALIZADO</text><text x="122" y="63" text-anchor="middle" class="s1">un solo nodo hace todo</text>
  <rect x="238" y="34" width="205" height="36" rx="5" fill="#e89822"/><text x="340" y="50" text-anchor="middle" class="t1">PARALELO</text><text x="340" y="63" text-anchor="middle" class="s1">muchos procesadores, un sistema</text>
  <rect x="456" y="34" width="204" height="36" rx="5" fill="#0055a0"/><text x="558" y="50" text-anchor="middle" class="t1">DISTRIBUIDO</text><text x="558" y="63" text-anchor="middle" class="s1">nodos independientes en red</text>
  <text x="20" y="88" class="k1">MEMORIA</text>
  <rect x="20" y="94" width="205" height="26" rx="4" fill="#eef3f8"/><text x="122" y="111" text-anchor="middle" class="d1">Compartida</text>
  <rect x="238" y="94" width="205" height="26" rx="4" fill="#eef3f8"/><text x="340" y="111" text-anchor="middle" class="d1">Compartida o acoplada</text>
  <rect x="456" y="94" width="204" height="26" rx="4" fill="#fdecec"/><text x="558" y="111" text-anchor="middle" class="d1">Privada de cada nodo</text>
  <text x="20" y="138" class="k1">RELOJ</text>
  <rect x="20" y="144" width="205" height="26" rx="4" fill="#f5f5f5"/><text x="122" y="161" text-anchor="middle" class="d1">Único</text>
  <rect x="238" y="144" width="205" height="26" rx="4" fill="#f5f5f5"/><text x="340" y="161" text-anchor="middle" class="d1">Común o sincronizado</text>
  <rect x="456" y="144" width="204" height="26" rx="4" fill="#fdecec"/><text x="558" y="161" text-anchor="middle" class="d1">Uno por nodo</text>
  <text x="20" y="188" class="k1">SI FALLA UN COMPONENTE</text>
  <rect x="20" y="194" width="205" height="26" rx="4" fill="#eef3f8"/><text x="122" y="211" text-anchor="middle" class="d1">Cae todo el sistema</text>
  <rect x="238" y="194" width="205" height="26" rx="4" fill="#eef3f8"/><text x="340" y="211" text-anchor="middle" class="d1">Suele caer todo</text>
  <rect x="456" y="194" width="204" height="26" rx="4" fill="#e8f4ec"/><text x="558" y="211" text-anchor="middle" class="d1">Fallo parcial: el resto sigue</text>
  <text x="20" y="238" class="k1">OBJETIVO PRINCIPAL</text>
  <rect x="20" y="244" width="205" height="26" rx="4" fill="#f5f5f5"/><text x="122" y="261" text-anchor="middle" class="d1">Simplicidad</text>
  <rect x="238" y="244" width="205" height="26" rx="4" fill="#f5f5f5"/><text x="340" y="261" text-anchor="middle" class="d1">Velocidad de cálculo</text>
  <rect x="456" y="244" width="204" height="26" rx="4" fill="#f5f5f5"/><text x="558" y="261" text-anchor="middle" class="d1">Escalar y no caerse</text>
  <rect x="20" y="280" width="640" height="22" rx="4" fill="#0055a0"/><text x="340" y="295" text-anchor="middle" class="s1">Las dos ausencias del sistema distribuido — sin memoria compartida y sin reloj global — obligan al paso de mensajes</text>
  <text x="340" y="314" text-anchor="middle" class="f1">[Fuente: TANENBAUM]</text>
</svg>
```

---

## D2 · Las ocho falacias de la computación distribuida

**Sección**: §1.1.2 — Las ocho falacias de la computación distribuida
**Propósito**: Presentar la lista cerrada de las ocho falacias enfrentando cada supuesto falso a su realidad, para memorizarlas por pares.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 337" role="img" aria-label="Las ocho falacias de la computación distribuida enunciadas por Deutsch y Gosling, cada una enfrentada a la realidad correspondiente: la red es fiable, la latencia es cero, el ancho de banda es infinito, la red es segura, la topología no cambia, hay un único administrador, el coste de transporte es cero y la red es homogénea">
  <style>.t2{font:700 10px system-ui,sans-serif;fill:#fff}.s2{font:9px system-ui,sans-serif;fill:#fff}.d2{font:9px system-ui,sans-serif;fill:#333}.h2{font:700 13px system-ui,sans-serif;fill:#0055a0}.k2{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.n2{font:700 10px system-ui,sans-serif;fill:#d13c3c}.f2{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h2">Las ocho falacias de la computación distribuida</text>
  <rect x="20" y="32" width="300" height="22" rx="4" fill="#d13c3c"/><text x="170" y="47" text-anchor="middle" class="t2">LO QUE SE SUPONE (FALSO)</text>
  <rect x="360" y="32" width="300" height="22" rx="4" fill="#2d8659"/><text x="510" y="47" text-anchor="middle" class="t2">LO QUE OCURRE DE VERDAD</text>
  <text x="24" y="76" class="n2">1</text><rect x="36" y="62" width="284" height="24" rx="4" fill="#fdecec"/><text x="46" y="78" class="d2">La red es fiable</text>
  <rect x="360" y="62" width="300" height="24" rx="4" fill="#e8f4ec"/><text x="370" y="78" class="d2">Se pierden paquetes y caen enlaces</text>
  <text x="24" y="105" class="n2">2</text><rect x="36" y="91" width="284" height="24" rx="4" fill="#fdecec"/><text x="46" y="107" class="d2">La latencia es cero</text>
  <rect x="360" y="91" width="300" height="24" rx="4" fill="#e8f4ec"/><text x="370" y="107" class="d2">La luz tiene velocidad finita</text>
  <text x="24" y="134" class="n2">3</text><rect x="36" y="120" width="284" height="24" rx="4" fill="#fdecec"/><text x="46" y="136" class="d2">El ancho de banda es infinito</text>
  <rect x="360" y="120" width="300" height="24" rx="4" fill="#e8f4ec"/><text x="370" y="136" class="d2">El caudal es finito y compartido</text>
  <text x="24" y="163" class="n2">4</text><rect x="36" y="149" width="284" height="24" rx="4" fill="#fdecec"/><text x="46" y="165" class="d2">La red es segura</text>
  <rect x="360" y="149" width="300" height="24" rx="4" fill="#e8f4ec"/><text x="370" y="165" class="d2">Hay escucha y suplantación</text>
  <text x="24" y="192" class="n2">5</text><rect x="36" y="178" width="284" height="24" rx="4" fill="#fdecec"/><text x="46" y="194" class="d2">La topología no cambia</text>
  <rect x="360" y="178" width="300" height="24" rx="4" fill="#e8f4ec"/><text x="370" y="194" class="d2">Las direcciones cambian sin avisar</text>
  <text x="24" y="221" class="n2">6</text><rect x="36" y="207" width="284" height="24" rx="4" fill="#fdecec"/><text x="46" y="223" class="d2">Hay un único administrador</text>
  <rect x="360" y="207" width="300" height="24" rx="4" fill="#e8f4ec"/><text x="370" y="223" class="d2">Intervienen terceros y proveedores</text>
  <text x="24" y="250" class="n2">7</text><rect x="36" y="236" width="284" height="24" rx="4" fill="#fdecec"/><text x="46" y="252" class="d2">El coste de transporte es cero</text>
  <rect x="360" y="236" width="300" height="24" rx="4" fill="#e8f4ec"/><text x="370" y="252" class="d2">Mover datos cuesta CPU y dinero</text>
  <text x="24" y="279" class="n2">8</text><rect x="36" y="265" width="284" height="24" rx="4" fill="#fdecec"/><text x="46" y="281" class="d2">La red es homogénea</text>
  <rect x="360" y="265" width="300" height="24" rx="4" fill="#e8f4ec"/><text x="370" y="281" class="d2">Conviven versiones y fabricantes</text>
  <rect x="20" y="294" width="640" height="20" rx="4" fill="#e89822"/><text x="340" y="308" text-anchor="middle" class="s2">La séptima es hoy la tarifa de salida de datos de la nube pública</text>
  <text x="340" y="328" text-anchor="middle" class="f2">[Fuente: DEUTSCH]</text>
</svg>
```

---

## D3 · De una a N capas: cliente-servidor y multicapa

**Sección**: §1.2.1 — Arquitectura cliente-servidor y multicapa
**Propósito**: Mostrar cómo las tres funciones lógicas —presentación, negocio y datos— se reparten en 1, 2, 3 y N capas físicas, y dónde está la frontera de cada modelo.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 340" role="img" aria-label="Reparto de las tres funciones lógicas de una aplicación (presentación, lógica de negocio y gestión de datos) entre arquitecturas de una capa, dos capas con cliente grueso, tres capas y N capas, señalando la regla de que cada capa solo se comunica con la contigua">
  <style>.t3{font:700 10px system-ui,sans-serif;fill:#fff}.s3{font:8.5px system-ui,sans-serif;fill:#fff}.d3{font:9px system-ui,sans-serif;fill:#333}.h3{font:700 13px system-ui,sans-serif;fill:#0055a0}.k3{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f3{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h3">De una a N capas</text>
  <text x="30" y="42" class="k3">1 CAPA</text>
  <rect x="30" y="48" width="130" height="66" rx="5" fill="#888"/><text x="95" y="70" text-anchor="middle" class="t3">TODO JUNTO</text><text x="95" y="85" text-anchor="middle" class="s3">presentación</text><text x="95" y="98" text-anchor="middle" class="s3">negocio + datos</text>
  <text x="190" y="42" class="k3">2 CAPAS · cliente grueso</text>
  <rect x="190" y="48" width="130" height="30" rx="5" fill="#0055a0"/><text x="255" y="61" text-anchor="middle" class="t3">CLIENTE</text><text x="255" y="73" text-anchor="middle" class="s3">presentación + negocio</text>
  <rect x="190" y="84" width="130" height="30" rx="5" fill="#2d8659"/><text x="255" y="97" text-anchor="middle" class="t3">SERVIDOR</text><text x="255" y="109" text-anchor="middle" class="s3">datos</text>
  <text x="350" y="42" class="k3">3 CAPAS</text>
  <rect x="350" y="48" width="130" height="20" rx="4" fill="#0055a0"/><text x="415" y="62" text-anchor="middle" class="s3">presentación</text>
  <rect x="350" y="72" width="130" height="20" rx="4" fill="#e89822"/><text x="415" y="86" text-anchor="middle" class="s3">lógica de negocio</text>
  <rect x="350" y="96" width="130" height="20" rx="4" fill="#2d8659"/><text x="415" y="110" text-anchor="middle" class="s3">datos</text>
  <text x="510" y="42" class="k3">N CAPAS</text>
  <rect x="510" y="48" width="150" height="16" rx="4" fill="#0055a0"/><text x="585" y="60" text-anchor="middle" class="s3">presentación web</text>
  <rect x="510" y="66" width="150" height="16" rx="4" fill="#5b8fc0"/><text x="585" y="78" text-anchor="middle" class="s3">servicios / API</text>
  <rect x="510" y="84" width="150" height="16" rx="4" fill="#e89822"/><text x="585" y="96" text-anchor="middle" class="s3">lógica de negocio</text>
  <rect x="510" y="102" width="150" height="16" rx="4" fill="#7fae8f"/><text x="585" y="114" text-anchor="middle" class="s3">integración</text>
  <rect x="510" y="120" width="150" height="16" rx="4" fill="#2d8659"/><text x="585" y="132" text-anchor="middle" class="s3">datos</text>
  <rect x="30" y="152" width="630" height="24" rx="4" fill="#eef3f8"/><text x="340" y="168" text-anchor="middle" class="d3">Regla de la arquitectura multicapa estricta: cada capa solo se comunica con la contigua</text>
  <rect x="30" y="184" width="310" height="22" rx="4" fill="#2d8659"/><text x="185" y="199" text-anchor="middle" class="t3">CORRECTO</text>
  <rect x="360" y="184" width="300" height="22" rx="4" fill="#d13c3c"/><text x="510" y="199" text-anchor="middle" class="t3">INCORRECTO</text>
  <rect x="40" y="214" width="80" height="22" rx="4" fill="#eef3f8"/><text x="80" y="229" text-anchor="middle" class="d3">presentación</text>
  <path d="M124 225 h30" stroke="#2d8659" stroke-width="2" marker-end="url(#a3)"/>
  <rect x="158" y="214" width="70" height="22" rx="4" fill="#eef3f8"/><text x="193" y="229" text-anchor="middle" class="d3">negocio</text>
  <path d="M232 225 h30" stroke="#2d8659" stroke-width="2" marker-end="url(#a3)"/>
  <rect x="266" y="214" width="64" height="22" rx="4" fill="#eef3f8"/><text x="298" y="229" text-anchor="middle" class="d3">datos</text>
  <rect x="370" y="214" width="80" height="22" rx="4" fill="#fdecec"/><text x="410" y="229" text-anchor="middle" class="d3">presentación</text>
  <rect x="488" y="214" width="70" height="22" rx="4" fill="#fdecec"/><text x="523" y="229" text-anchor="middle" class="d3">negocio</text>
  <rect x="590" y="214" width="64" height="22" rx="4" fill="#fdecec"/><text x="622" y="229" text-anchor="middle" class="d3">datos</text>
  <path d="M410 240 q106 34 212 -4" stroke="#d13c3c" stroke-width="2" fill="none" stroke-dasharray="4 3" marker-end="url(#b3)"/>
  <text x="516" y="272" text-anchor="middle" class="d3">salto directo a los datos</text>
  <defs><marker id="a3" markerWidth="7" markerHeight="7" refX="6" refY="3" orient="auto"><path d="M0 0 L6 3 L0 6 z" fill="#2d8659"/></marker><marker id="b3" markerWidth="7" markerHeight="7" refX="6" refY="3" orient="auto"><path d="M0 0 L6 3 L0 6 z" fill="#d13c3c"/></marker></defs>
  <rect x="30" y="286" width="630" height="22" rx="4" fill="#0055a0"/><text x="340" y="301" text-anchor="middle" class="s3">Si la presentación ataca directamente a la base de datos, hay tres capas dibujadas y dos de verdad</text>
  <text x="340" y="326" text-anchor="middle" class="f3">[Fuente: TANENBAUM]</text>
</svg>
```

---

## D4 · SOA frente a microservicios: dónde vive la lógica

**Sección**: §1.2.2 — Arquitecturas orientadas a servicios y microservicios
**Propósito**: Situar la diferencia real entre ambos paradigmas en dos puntos —dónde está la lógica de integración y quién posee los datos—, no en el tamaño del servicio.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 340" role="img" aria-label="Comparativa entre arquitectura orientada a servicios con bus de servicios empresarial y base de datos compartida, y arquitectura de microservicios con canal ligero y una base de datos por servicio, señalando que la diferencia está en dónde vive la lógica de integración y quién posee los datos">
  <style>.t4{font:700 10px system-ui,sans-serif;fill:#fff}.s4{font:8.5px system-ui,sans-serif;fill:#fff}.d4{font:9px system-ui,sans-serif;fill:#333}.h4{font:700 13px system-ui,sans-serif;fill:#0055a0}.k4{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f4{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h4">SOA frente a microservicios</text>
  <rect x="20" y="32" width="310" height="22" rx="4" fill="#888"/><text x="175" y="47" text-anchor="middle" class="t4">SOA CLÁSICA</text>
  <rect x="350" y="32" width="310" height="22" rx="4" fill="#0055a0"/><text x="505" y="47" text-anchor="middle" class="t4">MICROSERVICIOS</text>
  <rect x="32" y="64" width="86" height="24" rx="4" fill="#eef3f8"/><text x="75" y="80" text-anchor="middle" class="d4">Servicio A</text>
  <rect x="132" y="64" width="86" height="24" rx="4" fill="#eef3f8"/><text x="175" y="80" text-anchor="middle" class="d4">Servicio B</text>
  <rect x="232" y="64" width="86" height="24" rx="4" fill="#eef3f8"/><text x="275" y="80" text-anchor="middle" class="d4">Servicio C</text>
  <path d="M75 90 v14" stroke="#888" stroke-width="1.5"/><path d="M175 90 v14" stroke="#888" stroke-width="1.5"/><path d="M275 90 v14" stroke="#888" stroke-width="1.5"/>
  <rect x="32" y="106" width="286" height="30" rx="5" fill="#e89822"/><text x="175" y="119" text-anchor="middle" class="t4">BUS DE SERVICIOS (ESB)</text><text x="175" y="131" text-anchor="middle" class="s4">enruta, transforma y contiene lógica</text>
  <path d="M175 138 v14" stroke="#888" stroke-width="1.5"/>
  <rect x="92" y="154" width="166" height="26" rx="5" fill="#2d8659"/><text x="175" y="171" text-anchor="middle" class="t4">BASE DE DATOS COMPARTIDA</text>
  <rect x="362" y="64" width="86" height="24" rx="4" fill="#eef3f8"/><text x="405" y="80" text-anchor="middle" class="d4">Micro A</text>
  <rect x="462" y="64" width="86" height="24" rx="4" fill="#eef3f8"/><text x="505" y="80" text-anchor="middle" class="d4">Micro B</text>
  <rect x="562" y="64" width="86" height="24" rx="4" fill="#eef3f8"/><text x="605" y="80" text-anchor="middle" class="d4">Micro C</text>
  <path d="M405 90 v14" stroke="#0055a0" stroke-width="1.5"/><path d="M505 90 v14" stroke="#0055a0" stroke-width="1.5"/><path d="M605 90 v14" stroke="#0055a0" stroke-width="1.5"/>
  <rect x="362" y="106" width="286" height="20" rx="4" fill="#c9d9e8"/><text x="505" y="120" text-anchor="middle" class="d4">canal ligero: API o mensajería, sin lógica</text>
  <rect x="362" y="140" width="86" height="24" rx="4" fill="#2d8659"/><text x="405" y="156" text-anchor="middle" class="s4">BD propia A</text>
  <rect x="462" y="140" width="86" height="24" rx="4" fill="#2d8659"/><text x="505" y="156" text-anchor="middle" class="s4">BD propia B</text>
  <rect x="562" y="140" width="86" height="24" rx="4" fill="#2d8659"/><text x="605" y="156" text-anchor="middle" class="s4">BD propia C</text>
  <text x="20" y="200" class="k4">TAMAÑO</text>
  <rect x="20" y="206" width="310" height="22" rx="4" fill="#f5f5f5"/><text x="175" y="221" text-anchor="middle" class="d4">Servicios de grano grueso</text>
  <rect x="350" y="206" width="310" height="22" rx="4" fill="#f5f5f5"/><text x="505" y="221" text-anchor="middle" class="d4">Una capacidad de negocio por servicio</text>
  <text x="20" y="246" class="k4">DESPLIEGUE</text>
  <rect x="20" y="252" width="310" height="22" rx="4" fill="#eef3f8"/><text x="175" y="267" text-anchor="middle" class="d4">Coordinado, con frecuencia conjunto</text>
  <rect x="350" y="252" width="310" height="22" rx="4" fill="#eef3f8"/><text x="505" y="267" text-anchor="middle" class="d4">Independiente por servicio</text>
  <rect x="20" y="286" width="640" height="24" rx="4" fill="#0055a0"/><text x="340" y="302" text-anchor="middle" class="s4">La diferencia no es el tamaño: es dónde vive la lógica de integración y quién es dueño de los datos</text>
  <text x="340" y="326" text-anchor="middle" class="f4">[Fuente: FOWLER]</text>
</svg>
```

---

## D5 · P2P y arquitectura dirigida por eventos

**Sección**: §1.2.3 — Arquitecturas P2P y orientadas a eventos
**Propósito**: Contrastar visualmente la simetría de los nodos en P2P con el desacoplamiento entre productor y consumidores en la arquitectura dirigida por eventos.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 331" role="img" aria-label="A la izquierda, arquitectura entre pares en la que todos los nodos son iguales y actúan a la vez como cliente y como servidor; a la derecha, arquitectura dirigida por eventos en la que un productor publica un hecho en un intermediario y varios consumidores reaccionan sin que el productor los conozca">
  <style>.t5{font:700 10px system-ui,sans-serif;fill:#fff}.s5{font:8.5px system-ui,sans-serif;fill:#fff}.d5{font:9px system-ui,sans-serif;fill:#333}.h5{font:700 13px system-ui,sans-serif;fill:#0055a0}.k5{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f5{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h5">Entre pares y dirigida por eventos</text>
  <rect x="20" y="32" width="310" height="22" rx="4" fill="#2d8659"/><text x="175" y="47" text-anchor="middle" class="t5">P2P — todos los nodos son iguales</text>
  <rect x="350" y="32" width="310" height="22" rx="4" fill="#0055a0"/><text x="505" y="47" text-anchor="middle" class="t5">EDA — dirigida por eventos</text>
  <circle cx="175" cy="86" r="20" fill="#2d8659"/><text x="175" y="90" text-anchor="middle" class="s5">par 1</text>
  <circle cx="88" cy="146" r="20" fill="#2d8659"/><text x="88" y="150" text-anchor="middle" class="s5">par 2</text>
  <circle cx="262" cy="146" r="20" fill="#2d8659"/><text x="262" y="150" text-anchor="middle" class="s5">par 3</text>
  <circle cx="120" cy="212" r="20" fill="#2d8659"/><text x="120" y="216" text-anchor="middle" class="s5">par 4</text>
  <circle cx="230" cy="212" r="20" fill="#2d8659"/><text x="230" y="216" text-anchor="middle" class="s5">par 5</text>
  <path d="M160 100 L102 132" stroke="#7fae8f" stroke-width="1.6"/><path d="M190 100 L248 132" stroke="#7fae8f" stroke-width="1.6"/>
  <path d="M95 166 L114 192" stroke="#7fae8f" stroke-width="1.6"/><path d="M255 166 L236 192" stroke="#7fae8f" stroke-width="1.6"/>
  <path d="M140 212 h70" stroke="#7fae8f" stroke-width="1.6"/><path d="M104 132 L246 132" stroke="#7fae8f" stroke-width="1.6"/>
  <path d="M108 146 L242 146" stroke="#7fae8f" stroke-width="1.6" stroke-dasharray="3 3"/>
  <text x="175" y="252" text-anchor="middle" class="d5">Cada par es cliente y servidor a la vez</text>
  <text x="175" y="266" text-anchor="middle" class="d5">Sin punto único de fallo; localización por DHT</text>
  <rect x="380" y="66" width="110" height="28" rx="5" fill="#0055a0"/><text x="435" y="84" text-anchor="middle" class="s5">PRODUCTOR</text>
  <path d="M435 96 v18" stroke="#0055a0" stroke-width="2" marker-end="url(#a5)"/>
  <text x="446" y="110" class="d5">publica un hecho</text>
  <rect x="362" y="118" width="286" height="26" rx="5" fill="#e89822"/><text x="505" y="135" text-anchor="middle" class="t5">INTERMEDIARIO DE EVENTOS</text>
  <path d="M420 146 v22" stroke="#e89822" stroke-width="2" marker-end="url(#b5)"/>
  <path d="M505 146 v22" stroke="#e89822" stroke-width="2" marker-end="url(#b5)"/>
  <path d="M590 146 v22" stroke="#e89822" stroke-width="2" marker-end="url(#b5)"/>
  <rect x="372" y="172" width="96" height="26" rx="4" fill="#eef3f8"/><text x="420" y="189" text-anchor="middle" class="d5">notificación</text>
  <rect x="457" y="172" width="96" height="26" rx="4" fill="#eef3f8"/><text x="505" y="189" text-anchor="middle" class="d5">estadística</text>
  <rect x="542" y="172" width="96" height="26" rx="4" fill="#eef3f8"/><text x="590" y="189" text-anchor="middle" class="d5">archivo</text>
  <defs><marker id="a5" markerWidth="7" markerHeight="7" refX="6" refY="3" orient="auto"><path d="M0 0 L6 3 L0 6 z" fill="#0055a0"/></marker><marker id="b5" markerWidth="7" markerHeight="7" refX="6" refY="3" orient="auto"><path d="M0 0 L6 3 L0 6 z" fill="#e89822"/></marker></defs>
  <text x="505" y="222" text-anchor="middle" class="d5">El productor no sabe quién le escucha</text>
  <text x="505" y="236" text-anchor="middle" class="d5">Se añade un consumidor sin tocar al productor</text>
  <rect x="362" y="248" width="286" height="24" rx="4" fill="#fdecec"/><text x="505" y="264" text-anchor="middle" class="d5">Precio: trazabilidad difícil, consistencia eventual</text>
  <rect x="20" y="286" width="640" height="22" rx="4" fill="#0055a0"/><text x="340" y="301" text-anchor="middle" class="s5">Un comando se dirige a alguien y espera; un evento es un hecho ya ocurrido y no espera nada</text>
  <text x="340" y="322" text-anchor="middle" class="f5">[Fuente: TANENBAUM, DHT]</text>
</svg>
```

---

## D6 · Comunicación síncrona frente a asíncrona

**Sección**: §1.3.2 — Comunicación asíncrona: colas, publicación-suscripción y flujos
**Propósito**: Explicar por qué interponer un intermediario rompe el acoplamiento temporal y permite absorber un pico de carga, y fijar los tres patrones y las tres garantías de entrega.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 353" role="img" aria-label="Comparación entre comunicación síncrona, en la que el emisor se bloquea esperando la respuesta del receptor, y comunicación asíncrona con un intermediario de mensajes que absorbe el pico de carga; incluye los tres patrones de cola, publicación-suscripción y flujo de eventos y las tres garantías de entrega">
  <style>.t6{font:700 10px system-ui,sans-serif;fill:#fff}.s6{font:8.5px system-ui,sans-serif;fill:#fff}.d6{font:9px system-ui,sans-serif;fill:#333}.h6{font:700 13px system-ui,sans-serif;fill:#0055a0}.k6{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f6{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h6">Síncrono frente a asíncrono</text>
  <rect x="20" y="32" width="640" height="22" rx="4" fill="#d13c3c"/><text x="340" y="47" text-anchor="middle" class="t6">SÍNCRONO — el emisor espera y se degrada con el receptor</text>
  <rect x="40" y="64" width="120" height="26" rx="5" fill="#0055a0"/><text x="100" y="81" text-anchor="middle" class="s6">PORTAL</text>
  <path d="M164 77 h150" stroke="#d13c3c" stroke-width="2" marker-end="url(#a6)"/>
  <text x="239" y="72" text-anchor="middle" class="d6">40.000 peticiones/hora</text>
  <rect x="318" y="64" width="120" height="26" rx="5" fill="#888"/><text x="378" y="81" text-anchor="middle" class="s6">REGISTRO</text>
  <rect x="452" y="64" width="208" height="26" rx="5" fill="#fdecec"/><text x="556" y="81" text-anchor="middle" class="d6">solo admite 18.000/hora → errores</text>
  <rect x="20" y="102" width="640" height="22" rx="4" fill="#2d8659"/><text x="340" y="117" text-anchor="middle" class="t6">ASÍNCRONO — el intermediario amortigua el pico</text>
  <rect x="40" y="134" width="120" height="26" rx="5" fill="#0055a0"/><text x="100" y="151" text-anchor="middle" class="s6">PORTAL</text>
  <path d="M164 147 h50" stroke="#2d8659" stroke-width="2" marker-end="url(#b6)"/>
  <rect x="218" y="134" width="150" height="26" rx="5" fill="#e89822"/><text x="293" y="151" text-anchor="middle" class="s6">COLA (amortigua)</text>
  <path d="M372 147 h50" stroke="#2d8659" stroke-width="2" marker-end="url(#b6)"/>
  <rect x="426" y="134" width="120" height="26" rx="5" fill="#888"/><text x="486" y="151" text-anchor="middle" class="s6">REGISTRO</text>
  <rect x="556" y="134" width="104" height="26" rx="5" fill="#e8f4ec"/><text x="608" y="151" text-anchor="middle" class="d6">a su ritmo</text>
  <text x="40" y="180" class="d6">El portal responde al instante con justificante y número de referencia</text>
  <defs><marker id="a6" markerWidth="7" markerHeight="7" refX="6" refY="3" orient="auto"><path d="M0 0 L6 3 L0 6 z" fill="#d13c3c"/></marker><marker id="b6" markerWidth="7" markerHeight="7" refX="6" refY="3" orient="auto"><path d="M0 0 L6 3 L0 6 z" fill="#2d8659"/></marker></defs>
  <text x="20" y="204" class="k6">TRES PATRONES</text>
  <rect x="20" y="210" width="205" height="42" rx="5" fill="#eef3f8"/><text x="122" y="226" text-anchor="middle" class="d6">Cola punto a punto</text><text x="122" y="240" text-anchor="middle" class="d6">un mensaje → un consumidor</text>
  <rect x="238" y="210" width="205" height="42" rx="5" fill="#eef3f8"/><text x="340" y="226" text-anchor="middle" class="d6">Publicación-suscripción</text><text x="340" y="240" text-anchor="middle" class="d6">un mensaje → todos</text>
  <rect x="456" y="210" width="204" height="42" rx="5" fill="#eef3f8"/><text x="558" y="226" text-anchor="middle" class="d6">Flujo de eventos</text><text x="558" y="240" text-anchor="middle" class="d6">se conserva y se relee</text>
  <text x="20" y="272" class="k6">GARANTÍAS DE ENTREGA</text>
  <rect x="20" y="278" width="205" height="26" rx="5" fill="#f5f5f5"/><text x="122" y="295" text-anchor="middle" class="d6">Como mucho una vez: pierde</text>
  <rect x="238" y="278" width="205" height="26" rx="5" fill="#e8f4ec"/><text x="340" y="295" text-anchor="middle" class="d6">Al menos una vez: duplica</text>
  <rect x="456" y="278" width="204" height="26" rx="5" fill="#f5f5f5"/><text x="558" y="295" text-anchor="middle" class="d6">Exactamente una vez: cara</text>
  <rect x="20" y="310" width="640" height="20" rx="4" fill="#0055a0"/><text x="340" y="324" text-anchor="middle" class="s6">La garantía habitual es «al menos una vez»: el consumidor debe ser idempotente</text>
  <text x="340" y="344" text-anchor="middle" class="f6">[Fuente: AMQP, MQTT]</text>
</svg>
```

---

## D7 · Teorema CAP, ACID y BASE

**Sección**: §1.3.3 — Consistencia, teorema CAP y coordinación
**Propósito**: Corregir la lectura popular del teorema CAP —la elección solo se plantea **durante** una partición— y enfrentar los modelos de garantías ACID y BASE.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 341" role="img" aria-label="Teorema CAP representado como un triángulo con consistencia, disponibilidad y tolerancia a particiones, indicando que la tolerancia a particiones no es opcional y que la elección entre consistencia y disponibilidad solo se plantea durante una partición de red; incluye la comparación entre los modelos ACID y BASE">
  <style>.t7{font:700 10px system-ui,sans-serif;fill:#fff}.s7{font:8.5px system-ui,sans-serif;fill:#fff}.d7{font:9px system-ui,sans-serif;fill:#333}.h7{font:700 13px system-ui,sans-serif;fill:#0055a0}.k7{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f7{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h7">Teorema CAP: qué dice y qué no dice</text>
  <path d="M175 48 L272 200 L78 200 Z" fill="none" stroke="#0055a0" stroke-width="2"/>
  <circle cx="175" cy="48" r="24" fill="#0055a0"/><text x="175" y="45" text-anchor="middle" class="t7">C</text><text x="175" y="57" text-anchor="middle" class="s7">consist.</text>
  <circle cx="78" cy="200" r="24" fill="#2d8659"/><text x="78" y="197" text-anchor="middle" class="t7">A</text><text x="78" y="209" text-anchor="middle" class="s7">disponib.</text>
  <circle cx="272" cy="200" r="24" fill="#d13c3c"/><text x="272" y="197" text-anchor="middle" class="t7">P</text><text x="272" y="209" text-anchor="middle" class="s7" style="font-size:7.5px">particiones</text>
  <rect x="40" y="236" width="270" height="20" rx="4" fill="#fdecec"/><text x="175" y="250" text-anchor="middle" class="d7">P no es opcional: las particiones ocurren</text>
  <rect x="340" y="44" width="320" height="22" rx="4" fill="#d13c3c"/><text x="500" y="59" text-anchor="middle" class="t7">LECTURA INCORRECTA</text>
  <rect x="340" y="70" width="320" height="24" rx="4" fill="#fdecec"/><text x="500" y="86" text-anchor="middle" class="d7">«Elige dos de las tres»</text>
  <rect x="340" y="102" width="320" height="22" rx="4" fill="#2d8659"/><text x="500" y="117" text-anchor="middle" class="t7">LECTURA CORRECTA</text>
  <rect x="340" y="128" width="320" height="38" rx="4" fill="#e8f4ec"/><text x="500" y="144" text-anchor="middle" class="d7">Cuando hay partición, elige entre C y A.</text><text x="500" y="158" text-anchor="middle" class="d7">Cuando no la hay, puedes tener ambas.</text>
  <rect x="340" y="176" width="155" height="24" rx="4" fill="#0055a0"/><text x="417" y="192" text-anchor="middle" class="s7">Sistemas CP: rechazan</text>
  <rect x="505" y="176" width="155" height="24" rx="4" fill="#2d8659"/><text x="582" y="192" text-anchor="middle" class="s7">Sistemas AP: responden</text>
  <rect x="340" y="210" width="320" height="46" rx="4" fill="#f5f5f5"/><text x="500" y="226" text-anchor="middle" class="d7">CP: bases relacionales replicadas, consenso</text><text x="500" y="242" text-anchor="middle" class="d7">AP: almacenes clave-valor, catálogos, contenidos</text>
  <text x="40" y="278" class="k7">ACID</text>
  <rect x="80" y="266" width="230" height="20" rx="4" fill="#eef3f8"/><text x="195" y="280" text-anchor="middle" class="d7">Consistencia inmediata y fuerte</text>
  <text x="340" y="278" class="k7">BASE</text>
  <rect x="386" y="266" width="274" height="20" rx="4" fill="#eef3f8"/><text x="523" y="280" text-anchor="middle" class="d7">Disponible siempre, consistencia eventual</text>
  <rect x="40" y="294" width="620" height="20" rx="4" fill="#0055a0"/><text x="350" y="308" text-anchor="middle" class="s7">La C de CAP no es la C de ACID: aquí significa que todas las réplicas se ven iguales</text>
  <text x="340" y="332" text-anchor="middle" class="f7">[Fuente: BREWER]</text>
</svg>
```

---

## D8 · Las cinco características esenciales del NIST

**Sección**: §2.1.1 — La definición del NIST y las cinco características esenciales
**Propósito**: Fijar la lista cerrada de las cinco características y la regla de que la ausencia de cualquiera de ellas descalifica a un sistema como nube.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 327" role="img" aria-label="Las cinco características esenciales de la computación en la nube según el NIST: autoservicio bajo demanda, acceso amplio a la red, agrupación de recursos, elasticidad rápida y servicio medido, con la advertencia de que si falta una no es nube y de que ISO añade la multitenencia como sexta">
  <style>.t8{font:700 10.5px system-ui,sans-serif;fill:#fff}.s8{font:8.5px system-ui,sans-serif;fill:#fff}.d8{font:9px system-ui,sans-serif;fill:#333}.h8{font:700 13px system-ui,sans-serif;fill:#0055a0}.k8{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f8{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h8">Las cinco características esenciales (NIST SP 800-145)</text>
  <rect x="20" y="34" width="126" height="52" rx="6" fill="#0055a0"/><text x="83" y="55" text-anchor="middle" class="t8">AUTOSERVICIO</text><text x="83" y="69" text-anchor="middle" class="s8">bajo demanda,</text><text x="83" y="80" text-anchor="middle" class="s8">sin pedir permiso</text>
  <rect x="153" y="34" width="126" height="52" rx="6" fill="#0055a0"/><text x="216" y="55" text-anchor="middle" class="t8">ACCESO A LA RED</text><text x="216" y="69" text-anchor="middle" class="s8">amplio, desde</text><text x="216" y="80" text-anchor="middle" class="s8">cualquier dispositivo</text>
  <rect x="286" y="34" width="126" height="52" rx="6" fill="#0055a0"/><text x="349" y="55" text-anchor="middle" class="t8">AGRUPACIÓN</text><text x="349" y="69" text-anchor="middle" class="s8">recursos compartidos,</text><text x="349" y="80" text-anchor="middle" class="s8">multiinquilino</text>
  <rect x="419" y="34" width="126" height="52" rx="6" fill="#0055a0"/><text x="482" y="55" text-anchor="middle" class="t8">ELASTICIDAD</text><text x="482" y="69" text-anchor="middle" class="s8">crece y decrece</text><text x="482" y="80" text-anchor="middle" class="s8">rápidamente</text>
  <rect x="552" y="34" width="108" height="52" rx="6" fill="#0055a0"/><text x="606" y="55" text-anchor="middle" class="t8">MEDICIÓN</text><text x="606" y="69" text-anchor="middle" class="s8">se mide, se paga</text><text x="606" y="80" text-anchor="middle" class="s8">por lo usado</text>
  <text x="20" y="110" class="k8">QUÉ APORTA CADA UNA</text>
  <rect x="20" y="116" width="640" height="22" rx="4" fill="#eef3f8"/><text x="30" y="131" class="d8">Autoservicio: convierte semanas de tramitación en minutos de consola o una llamada a la API</text>
  <rect x="20" y="142" width="640" height="22" rx="4" fill="#f5f5f5"/><text x="30" y="157" class="d8">Acceso a la red: sin conectividad no hay servicio; la red pasa a ser tan crítica como el propio servicio</text>
  <rect x="20" y="168" width="640" height="22" rx="4" fill="#eef3f8"/><text x="30" y="183" class="d8">Agrupación: produce la economía de escala y, a la vez, los problemas jurídicos de ubicación del dato</text>
  <rect x="20" y="194" width="640" height="22" rx="4" fill="#f5f5f5"/><text x="30" y="209" class="d8">Elasticidad: capacidad percibida como ilimitada; solo ahorra si de verdad se decrece</text>
  <rect x="20" y="220" width="640" height="22" rx="4" fill="#eef3f8"/><text x="30" y="235" class="d8">Medición: hace posible el pago por uso y la imputación del gasto a quien lo genera</text>
  <rect x="20" y="252" width="310" height="26" rx="5" fill="#d13c3c"/><text x="175" y="269" text-anchor="middle" class="s8">Si falta una sola, NO es nube: es virtualización</text>
  <rect x="350" y="252" width="310" height="26" rx="5" fill="#e89822"/><text x="505" y="269" text-anchor="middle" class="s8">ISO/IEC 17788 añade una sexta: multitenencia</text>
  <rect x="20" y="284" width="640" height="20" rx="4" fill="#0055a0"/><text x="340" y="298" text-anchor="middle" class="s8">Regla mnemotécnica: A-A-A-E-M — Autoservicio, Acceso, Agrupación, Elasticidad, Medición</text>
  <text x="340" y="318" text-anchor="middle" class="f8">[Fuente: NIST145, ISO17788]</text>
</svg>
```

---

## D9 · Máquina virtual, contenedor y función

**Sección**: §2.3.1 — De la máquina virtual al contenedor y a la función
**Propósito**: Mostrar qué capa virtualiza cada unidad de despliegue y de dónde salen las diferencias de arranque, aislamiento y densidad.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 330" role="img" aria-label="Comparación en capas entre máquina virtual, que virtualiza el hardware y lleva su propio sistema operativo, contenedor, que virtualiza el sistema operativo y comparte el núcleo del anfitrión, y función sin servidor, en la que el cliente solo aporta el código; se indican tiempos de arranque, aislamiento y densidad">
  <style>.t9{font:700 10px system-ui,sans-serif;fill:#fff}.s9{font:8.5px system-ui,sans-serif;fill:#fff}.d9{font:9px system-ui,sans-serif;fill:#333}.h9{font:700 13px system-ui,sans-serif;fill:#0055a0}.k9{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f9{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h9">Máquina virtual, contenedor y función</text>
  <rect x="20" y="32" width="205" height="20" rx="4" fill="#888"/><text x="122" y="46" text-anchor="middle" class="t9">MÁQUINA VIRTUAL</text>
  <rect x="238" y="32" width="205" height="20" rx="4" fill="#0055a0"/><text x="340" y="46" text-anchor="middle" class="t9">CONTENEDOR</text>
  <rect x="456" y="32" width="204" height="20" rx="4" fill="#2d8659"/><text x="558" y="46" text-anchor="middle" class="t9">FUNCIÓN (sin servidor)</text>
  <rect x="20" y="58" width="205" height="18" rx="3" fill="#eef3f8"/><text x="122" y="71" text-anchor="middle" class="d9">Aplicación</text>
  <rect x="20" y="78" width="205" height="18" rx="3" fill="#eef3f8"/><text x="122" y="91" text-anchor="middle" class="d9">Bibliotecas</text>
  <rect x="20" y="98" width="205" height="20" rx="3" fill="#e89822"/><text x="122" y="112" text-anchor="middle" class="s9">Sistema operativo invitado</text>
  <rect x="20" y="120" width="205" height="18" rx="3" fill="#c9d9e8"/><text x="122" y="133" text-anchor="middle" class="d9">Hipervisor</text>
  <rect x="20" y="140" width="205" height="18" rx="3" fill="#ddd"/><text x="122" y="153" text-anchor="middle" class="d9">Hardware</text>
  <rect x="238" y="58" width="205" height="18" rx="3" fill="#eef3f8"/><text x="340" y="71" text-anchor="middle" class="d9">Aplicación</text>
  <rect x="238" y="78" width="205" height="18" rx="3" fill="#eef3f8"/><text x="340" y="91" text-anchor="middle" class="d9">Dependencias empaquetadas</text>
  <rect x="238" y="98" width="205" height="18" rx="3" fill="#c9d9e8"/><text x="340" y="111" text-anchor="middle" class="d9">Motor de contenedores</text>
  <rect x="238" y="118" width="205" height="20" rx="3" fill="#d13c3c"/><text x="340" y="132" text-anchor="middle" class="s9">Núcleo compartido del anfitrión</text>
  <rect x="238" y="140" width="205" height="18" rx="3" fill="#ddd"/><text x="340" y="153" text-anchor="middle" class="d9">Hardware</text>
  <rect x="456" y="58" width="204" height="18" rx="3" fill="#eef3f8"/><text x="558" y="71" text-anchor="middle" class="d9">Solo el código de la función</text>
  <rect x="456" y="78" width="204" height="80" rx="3" fill="#e8f4ec"/><text x="558" y="110" text-anchor="middle" class="d9">Todo lo demás lo pone</text><text x="558" y="124" text-anchor="middle" class="d9">y lo escala la plataforma</text>
  <text x="20" y="180" class="k9">ARRANQUE</text>
  <rect x="20" y="186" width="205" height="22" rx="4" fill="#f5f5f5"/><text x="122" y="201" text-anchor="middle" class="d9">Minutos</text>
  <rect x="238" y="186" width="205" height="22" rx="4" fill="#f5f5f5"/><text x="340" y="201" text-anchor="middle" class="d9">Segundos</text>
  <rect x="456" y="186" width="204" height="22" rx="4" fill="#f5f5f5"/><text x="558" y="201" text-anchor="middle" class="d9">Milisegundos (arranque en frío)</text>
  <text x="20" y="226" class="k9">AISLAMIENTO</text>
  <rect x="20" y="232" width="205" height="22" rx="4" fill="#e8f4ec"/><text x="122" y="247" text-anchor="middle" class="d9">Fuerte: núcleos separados</text>
  <rect x="238" y="232" width="205" height="22" rx="4" fill="#fdf3e2"/><text x="340" y="247" text-anchor="middle" class="d9">Medio: espacios de nombres</text>
  <rect x="456" y="232" width="204" height="22" rx="4" fill="#eef3f8"/><text x="558" y="247" text-anchor="middle" class="d9">Lo gestiona el proveedor</text>
  <text x="20" y="272" class="k9">DENSIDAD POR SERVIDOR</text>
  <rect x="20" y="278" width="205" height="22" rx="4" fill="#eef3f8"/><text x="122" y="293" text-anchor="middle" class="d9">Decenas</text>
  <rect x="238" y="278" width="205" height="22" rx="4" fill="#eef3f8"/><text x="340" y="293" text-anchor="middle" class="d9">Cientos</text>
  <rect x="456" y="278" width="204" height="22" rx="4" fill="#eef3f8"/><text x="558" y="293" text-anchor="middle" class="d9">Miles</text>
  <text x="340" y="316" text-anchor="middle" class="f9">[Fuente: OCI, HYPERVISORS]</text>
</svg>
```
---

## D10 · La pila de responsabilidad: local, IaaS, PaaS y SaaS

**Sección**: §3 — Modelos de servicio en Cloud Computing
**Propósito**: Es el diagrama de memorización directa del tema: nueve capas y la línea que separa lo que gestiona el cliente de lo que gestiona el proveedor en cada modelo.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 405" role="img" aria-label="Matriz de nueve capas (datos, identidades, aplicación, tiempo de ejecución, middleware, sistema operativo, virtualización, servidores y almacenamiento, y red e instalaciones) frente a los cuatro modelos local, IaaS, PaaS y SaaS, indicando en cada celda si la gestiona el cliente o el proveedor; se señala que la frontera entre local e IaaS es la virtualización y entre IaaS y PaaS el sistema operativo">
  <style>.t10{font:700 10px system-ui,sans-serif;fill:#fff}.s10{font:8.5px system-ui,sans-serif;fill:#fff}.d10{font:9px system-ui,sans-serif;fill:#333}.h10{font:700 13px system-ui,sans-serif;fill:#0055a0}.k10{font:700 9px system-ui,sans-serif;fill:#0055a0}.f10{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h10">La pila de responsabilidad</text>
  <rect x="20" y="32" width="200" height="22" rx="4" fill="#0055a0"/><text x="120" y="47" text-anchor="middle" class="t10">CAPA</text>
  <rect x="226" y="32" width="108" height="22" rx="4" fill="#888"/><text x="280" y="47" text-anchor="middle" class="t10">LOCAL</text>
  <rect x="340" y="32" width="108" height="22" rx="4" fill="#0055a0"/><text x="394" y="47" text-anchor="middle" class="t10">IaaS</text>
  <rect x="454" y="32" width="108" height="22" rx="4" fill="#e89822"/><text x="508" y="47" text-anchor="middle" class="t10">PaaS</text>
  <rect x="568" y="32" width="92" height="22" rx="4" fill="#2d8659"/><text x="614" y="47" text-anchor="middle" class="t10">SaaS</text>
  <rect x="20" y="58" width="200" height="24" rx="3" fill="#eef3f8"/><text x="30" y="74" class="d10">Datos y su clasificación</text>
  <rect x="226" y="58" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="74" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="58" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="394" y="74" text-anchor="middle" class="d10">Cliente</text>
  <rect x="454" y="58" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="508" y="74" text-anchor="middle" class="d10">Cliente</text>
  <rect x="568" y="58" width="92" height="24" rx="3" fill="#c9d9e8"/><text x="614" y="74" text-anchor="middle" class="d10">Cliente</text>
  <rect x="20" y="84" width="200" height="24" rx="3" fill="#eef3f8"/><text x="30" y="100" class="d10">Identidades y accesos</text>
  <rect x="226" y="84" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="100" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="84" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="394" y="100" text-anchor="middle" class="d10">Cliente</text>
  <rect x="454" y="84" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="508" y="100" text-anchor="middle" class="d10">Cliente</text>
  <rect x="568" y="84" width="92" height="24" rx="3" fill="#c9d9e8"/><text x="614" y="100" text-anchor="middle" class="d10">Cliente</text>
  <rect x="20" y="110" width="200" height="24" rx="3" fill="#f5f5f5"/><text x="30" y="126" class="d10">Aplicación</text>
  <rect x="226" y="110" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="126" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="110" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="394" y="126" text-anchor="middle" class="d10">Cliente</text>
  <rect x="454" y="110" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="508" y="126" text-anchor="middle" class="d10">Cliente</text>
  <rect x="568" y="110" width="92" height="24" rx="3" fill="#cfe3d6"/><text x="614" y="126" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="20" y="136" width="200" height="24" rx="3" fill="#f5f5f5"/><text x="30" y="152" class="d10">Tiempo de ejecución</text>
  <rect x="226" y="136" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="152" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="136" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="394" y="152" text-anchor="middle" class="d10">Cliente</text>
  <rect x="454" y="136" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="508" y="152" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="568" y="136" width="92" height="24" rx="3" fill="#cfe3d6"/><text x="614" y="152" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="20" y="162" width="200" height="24" rx="3" fill="#f5f5f5"/><text x="30" y="178" class="d10">Middleware</text>
  <rect x="226" y="162" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="178" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="162" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="394" y="178" text-anchor="middle" class="d10">Cliente</text>
  <rect x="454" y="162" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="508" y="178" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="568" y="162" width="92" height="24" rx="3" fill="#cfe3d6"/><text x="614" y="178" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="20" y="188" width="200" height="24" rx="3" fill="#fdf3e2"/><text x="30" y="204" class="d10">Sistema operativo</text>
  <rect x="226" y="188" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="204" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="188" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="394" y="204" text-anchor="middle" class="d10">Cliente</text>
  <rect x="454" y="188" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="508" y="204" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="568" y="188" width="92" height="24" rx="3" fill="#cfe3d6"/><text x="614" y="204" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="20" y="214" width="200" height="24" rx="3" fill="#fdf3e2"/><text x="30" y="230" class="d10">Virtualización</text>
  <rect x="226" y="214" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="230" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="214" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="394" y="230" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="454" y="214" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="508" y="230" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="568" y="214" width="92" height="24" rx="3" fill="#cfe3d6"/><text x="614" y="230" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="20" y="240" width="200" height="24" rx="3" fill="#f5f5f5"/><text x="30" y="256" class="d10">Servidores y almacenamiento</text>
  <rect x="226" y="240" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="256" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="240" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="394" y="256" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="454" y="240" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="508" y="256" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="568" y="240" width="92" height="24" rx="3" fill="#cfe3d6"/><text x="614" y="256" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="20" y="266" width="200" height="24" rx="3" fill="#f5f5f5"/><text x="30" y="282" class="d10">Red e instalaciones físicas</text>
  <rect x="226" y="266" width="108" height="24" rx="3" fill="#c9d9e8"/><text x="280" y="282" text-anchor="middle" class="d10">Cliente</text>
  <rect x="340" y="266" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="394" y="282" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="454" y="266" width="108" height="24" rx="3" fill="#cfe3d6"/><text x="508" y="282" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="568" y="266" width="92" height="24" rx="3" fill="#cfe3d6"/><text x="614" y="282" text-anchor="middle" class="d10">Proveedor</text>
  <rect x="20" y="300" width="320" height="26" rx="5" fill="#e89822"/><text x="180" y="317" text-anchor="middle" class="s10">Frontera LOCAL / IaaS: la virtualización</text>
  <rect x="350" y="300" width="310" height="26" rx="5" fill="#d13c3c"/><text x="505" y="317" text-anchor="middle" class="s10">Frontera IaaS / PaaS: el sistema operativo</text>
  <rect x="20" y="332" width="640" height="26" rx="5" fill="#0055a0"/><text x="340" y="349" text-anchor="middle" class="s10">Los datos y las identidades NUNCA cambian de dueño: son siempre del cliente</text>
  <rect x="20" y="364" width="640" height="20" rx="4" fill="#eef3f8"/><text x="340" y="378" text-anchor="middle" class="d10">Cuanto más arriba se sube, menos se gestiona y menos se controla</text>
  <text x="340" y="396" text-anchor="middle" class="f10">[Fuente: NIST145, CCN823]</text>
</svg>
```

---

## D11 · Escalabilidad y elasticidad: la curva de capacidad

**Sección**: §3.1.2 — Abstracción del hardware y elasticidad
**Propósito**: Distinguir gráficamente escalabilidad de elasticidad y visualizar los dos costes de no ser elástico: la capacidad ociosa y la demanda no atendida.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 341" role="img" aria-label="Gráfico que compara la capacidad fija de una infraestructura propia, representada como una línea horizontal, con la demanda real variable a lo largo del año, que presenta un pico en el mes de la campaña; se marcan la zona de capacidad ociosa, en la que se paga capacidad que no se usa, y la zona de demanda no atendida durante el pico; incluye las definiciones de escalabilidad, elasticidad y escalado vertical frente a horizontal">
  <style>.t11{font:700 10px system-ui,sans-serif;fill:#fff}.s11{font:8.5px system-ui,sans-serif;fill:#fff}.d11{font:9px system-ui,sans-serif;fill:#333}.h11{font:700 13px system-ui,sans-serif;fill:#0055a0}.k11{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f11{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h11">Escalabilidad y elasticidad</text>
  <path d="M62 192 h598" stroke="#999" stroke-width="1.5"/><path d="M62 40 v152" stroke="#999" stroke-width="1.5"/>
  <text x="34" y="46" class="d11">alta</text><text x="32" y="190" class="d11">baja</text>
  <rect x="72" y="112" width="258" height="40" fill="#c9d9e8" opacity="0.6"/>
  <text x="201" y="137" text-anchor="middle" class="d11">capacidad ociosa: se paga y no se usa</text>
  <rect x="372" y="58" width="36" height="54" fill="#f3c6c6"/>
  <path d="M62 152 h280 l30 -94 h40 l30 94 h218" stroke="#0055a0" stroke-width="2.5" fill="none"/>
  <path d="M62 112 h598" stroke="#888" stroke-width="2" stroke-dasharray="7 4"/>
  <text x="656" y="106" text-anchor="end" class="d11">capacidad fija comprada</text>
  <text x="390" y="52" text-anchor="middle" class="d11">demanda no atendida</text>
  <text x="350" y="208" text-anchor="middle" class="d11">tiempo (un año, con una campaña en el mes 7)</text>
  <path d="M72 224 h22" stroke="#0055a0" stroke-width="2.5"/><text x="100" y="228" class="d11">demanda real</text>
  <path d="M212 224 h22" stroke="#888" stroke-width="2" stroke-dasharray="7 4"/><text x="240" y="228" class="d11">capacidad fija</text>
  <text x="332" y="228" class="d11">Una capacidad elástica se pegaría a la línea azul: sin zona azul ni zona rosa</text>
  <rect x="20" y="238" width="205" height="50" rx="5" fill="#888"/><text x="122" y="256" text-anchor="middle" class="t11">ESCALABILIDAD</text><text x="122" y="270" text-anchor="middle" class="s11">capacidad de crecer;</text><text x="122" y="282" text-anchor="middle" class="s11">puede ser manual y lenta</text>
  <rect x="238" y="238" width="205" height="50" rx="5" fill="#2d8659"/><text x="340" y="256" text-anchor="middle" class="t11">ELASTICIDAD</text><text x="340" y="270" text-anchor="middle" class="s11">crecer Y DECRECER,</text><text x="340" y="282" text-anchor="middle" class="s11">automático y rápido</text>
  <rect x="456" y="238" width="204" height="50" rx="5" fill="#0055a0"/><text x="558" y="256" text-anchor="middle" class="t11">VERTICAL / HORIZONTAL</text><text x="558" y="270" text-anchor="middle" class="s11">más potencia a una máquina</text><text x="558" y="282" text-anchor="middle" class="s11">frente a más máquinas</text>
  <rect x="20" y="296" width="640" height="22" rx="4" fill="#e89822"/><text x="340" y="311" text-anchor="middle" class="s11">El escalado horizontal exige aplicaciones sin estado: la sesión no puede vivir en la memoria de una instancia</text>
  <text x="340" y="332" text-anchor="middle" class="f11">[Fuente: NIST145, 12FACTOR]</text>
</svg>
```

---

## D12 · Multitenencia: silo, puente y agrupado

**Sección**: §3.3.2 — Arquitectura multinquilino y licenciamiento
**Propósito**: Comparar los tres modelos de aislamiento entre inquilinos y explicitar el compromiso entre aislamiento, coste y personalización.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 331" role="img" aria-label="Comparación de los tres modelos de multitenencia: silo con aplicación y base de datos propias por inquilino, puente con aplicación compartida y base de datos propia, y agrupado con todo compartido y un identificador de inquilino en cada registro; se indican aislamiento, coste por inquilino y capacidad de personalización">
  <style>.t12{font:700 10px system-ui,sans-serif;fill:#fff}.s12{font:8.5px system-ui,sans-serif;fill:#fff}.d12{font:9px system-ui,sans-serif;fill:#333}.h12{font:700 13px system-ui,sans-serif;fill:#0055a0}.k12{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f12{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h12">Tres modelos de multitenencia</text>
  <rect x="20" y="32" width="205" height="22" rx="4" fill="#2d8659"/><text x="122" y="47" text-anchor="middle" class="t12">SILO</text>
  <rect x="238" y="32" width="205" height="22" rx="4" fill="#e89822"/><text x="340" y="47" text-anchor="middle" class="t12">PUENTE</text>
  <rect x="456" y="32" width="204" height="22" rx="4" fill="#0055a0"/><text x="558" y="47" text-anchor="middle" class="t12">AGRUPADO</text>
  <rect x="28" y="62" width="60" height="22" rx="3" fill="#eef3f8"/><text x="58" y="77" text-anchor="middle" class="d12">app A</text>
  <rect x="92" y="62" width="60" height="22" rx="3" fill="#eef3f8"/><text x="122" y="77" text-anchor="middle" class="d12">app B</text>
  <rect x="156" y="62" width="60" height="22" rx="3" fill="#eef3f8"/><text x="186" y="77" text-anchor="middle" class="d12">app C</text>
  <rect x="28" y="88" width="60" height="22" rx="3" fill="#cfe3d6"/><text x="58" y="103" text-anchor="middle" class="d12">BD A</text>
  <rect x="92" y="88" width="60" height="22" rx="3" fill="#cfe3d6"/><text x="122" y="103" text-anchor="middle" class="d12">BD B</text>
  <rect x="156" y="88" width="60" height="22" rx="3" fill="#cfe3d6"/><text x="186" y="103" text-anchor="middle" class="d12">BD C</text>
  <rect x="246" y="62" width="188" height="22" rx="3" fill="#fdf3e2"/><text x="340" y="77" text-anchor="middle" class="d12">aplicación compartida</text>
  <rect x="246" y="88" width="60" height="22" rx="3" fill="#cfe3d6"/><text x="276" y="103" text-anchor="middle" class="d12">BD A</text>
  <rect x="310" y="88" width="60" height="22" rx="3" fill="#cfe3d6"/><text x="340" y="103" text-anchor="middle" class="d12">BD B</text>
  <rect x="374" y="88" width="60" height="22" rx="3" fill="#cfe3d6"/><text x="404" y="103" text-anchor="middle" class="d12">BD C</text>
  <rect x="464" y="62" width="188" height="22" rx="3" fill="#c9d9e8"/><text x="558" y="77" text-anchor="middle" class="d12">aplicación compartida</text>
  <rect x="464" y="88" width="188" height="22" rx="3" fill="#c9d9e8"/><text x="558" y="103" text-anchor="middle" class="d12">una BD con id_inquilino en cada fila</text>
  <text x="20" y="132" class="k12">AISLAMIENTO</text>
  <rect x="20" y="138" width="205" height="22" rx="4" fill="#e8f4ec"/><text x="122" y="153" text-anchor="middle" class="d12">Máximo (físico y lógico)</text>
  <rect x="238" y="138" width="205" height="22" rx="4" fill="#fdf3e2"/><text x="340" y="153" text-anchor="middle" class="d12">Medio-alto</text>
  <rect x="456" y="138" width="204" height="22" rx="4" fill="#fdecec"/><text x="558" y="153" text-anchor="middle" class="d12">Solo lógico: depende del código</text>
  <text x="20" y="178" class="k12">COSTE POR INQUILINO</text>
  <rect x="20" y="184" width="205" height="22" rx="4" fill="#fdecec"/><text x="122" y="199" text-anchor="middle" class="d12">Alto</text>
  <rect x="238" y="184" width="205" height="22" rx="4" fill="#fdf3e2"/><text x="340" y="199" text-anchor="middle" class="d12">Medio</text>
  <rect x="456" y="184" width="204" height="22" rx="4" fill="#e8f4ec"/><text x="558" y="199" text-anchor="middle" class="d12">Mínimo</text>
  <text x="20" y="224" class="k12">PERSONALIZACIÓN</text>
  <rect x="20" y="230" width="205" height="22" rx="4" fill="#e8f4ec"/><text x="122" y="245" text-anchor="middle" class="d12">Alta</text>
  <rect x="238" y="230" width="205" height="22" rx="4" fill="#fdf3e2"/><text x="340" y="245" text-anchor="middle" class="d12">Media</text>
  <rect x="456" y="230" width="204" height="22" rx="4" fill="#fdecec"/><text x="558" y="245" text-anchor="middle" class="d12">Baja: solo configuración</text>
  <rect x="20" y="262" width="640" height="24" rx="4" fill="#d13c3c"/><text x="340" y="278" text-anchor="middle" class="s12">Riesgo del modelo agrupado: una consulta que olvide filtrar por inquilino expone datos de otra organización</text>
  <rect x="20" y="290" width="640" height="18" rx="4" fill="#eef3f8"/><text x="340" y="303" text-anchor="middle" class="d12">Y el ruido del vecino: quien consume de más degrada a los demás</text>
  <text x="340" y="322" text-anchor="middle" class="f12">[Fuente: ISO17788]</text>
</svg>
```

---

## D13 · El mapa del XaaS sobre los tres modelos del NIST

**Sección**: §3.4 — Otros modelos de servicio (XaaS)
**Propósito**: Ordenar el zoo de siglas comerciales situando cada una sobre los tres únicos modelos que reconoce el NIST.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 339" role="img" aria-label="Mapa que sitúa las denominaciones comerciales XaaS (FaaS, CaaS de contenedores y de comunicaciones, DBaaS, DSaaS, NaaS, CompaaS, DaaS de escritorio y de datos, SECaaS, IDaaS, BPaaS y MLaaS) sobre los tres únicos modelos de servicio reconocidos por el NIST: IaaS, PaaS y SaaS">
  <style>.t13{font:700 10.5px system-ui,sans-serif;fill:#fff}.s13{font:8.5px system-ui,sans-serif;fill:#fff}.d13{font:9px system-ui,sans-serif;fill:#333}.h13{font:700 13px system-ui,sans-serif;fill:#0055a0}.k13{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f13{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h13">El zoo del XaaS sobre los tres modelos del NIST</text>
  <rect x="20" y="34" width="205" height="26" rx="5" fill="#0055a0"/><text x="122" y="52" text-anchor="middle" class="t13">IaaS</text>
  <rect x="238" y="34" width="205" height="26" rx="5" fill="#e89822"/><text x="340" y="52" text-anchor="middle" class="t13">PaaS</text>
  <rect x="456" y="34" width="204" height="26" rx="5" fill="#2d8659"/><text x="558" y="52" text-anchor="middle" class="t13">SaaS</text>
  <rect x="20" y="66" width="205" height="22" rx="3" fill="#eef3f8"/><text x="122" y="81" text-anchor="middle" class="d13">CompaaS — cómputo</text>
  <rect x="20" y="92" width="205" height="22" rx="3" fill="#eef3f8"/><text x="122" y="107" text-anchor="middle" class="d13">DSaaS — almacenamiento</text>
  <rect x="20" y="118" width="205" height="22" rx="3" fill="#eef3f8"/><text x="122" y="133" text-anchor="middle" class="d13">NaaS — red</text>
  <rect x="238" y="66" width="205" height="22" rx="3" fill="#fdf3e2"/><text x="340" y="81" text-anchor="middle" class="d13">FaaS — funciones, sin servidor</text>
  <rect x="238" y="92" width="205" height="22" rx="3" fill="#fdf3e2"/><text x="340" y="107" text-anchor="middle" class="d13">DBaaS — base de datos</text>
  <rect x="238" y="118" width="205" height="22" rx="3" fill="#fdf3e2"/><text x="340" y="133" text-anchor="middle" class="d13">iPaaS — integración</text>
  <rect x="456" y="66" width="204" height="22" rx="3" fill="#e8f4ec"/><text x="558" y="81" text-anchor="middle" class="d13">SECaaS — seguridad</text>
  <rect x="456" y="92" width="204" height="22" rx="3" fill="#e8f4ec"/><text x="558" y="107" text-anchor="middle" class="d13">BPaaS — proceso de negocio</text>
  <rect x="456" y="118" width="204" height="22" rx="3" fill="#e8f4ec"/><text x="558" y="133" text-anchor="middle" class="d13">DaaS — datos como servicio</text>
  <rect x="130" y="152" width="205" height="24" rx="4" fill="#c9d9e8"/><text x="232" y="168" text-anchor="middle" class="d13">CaaS — contenedores (entre IaaS y PaaS)</text>
  <rect x="345" y="152" width="205" height="24" rx="4" fill="#cfe3d6"/><text x="447" y="168" text-anchor="middle" class="d13">IDaaS — identidad (PaaS/SaaS)</text>
  <rect x="130" y="180" width="205" height="24" rx="4" fill="#cfe3d6"/><text x="232" y="196" text-anchor="middle" class="d13">DaaS — escritorio (véase Tema 28)</text>
  <rect x="345" y="180" width="205" height="24" rx="4" fill="#cfe3d6"/><text x="447" y="196" text-anchor="middle" class="d13">MLaaS — modelos e inferencia</text>
  <rect x="20" y="216" width="640" height="24" rx="4" fill="#d13c3c"/><text x="340" y="232" text-anchor="middle" class="s13">Ninguna de estas siglas es una categoría del NIST: el NIST reconoce TRES modelos de servicio</text>
  <rect x="20" y="246" width="310" height="46" rx="5" fill="#fdecec"/><text x="175" y="264" text-anchor="middle" class="d13">CaaS ambiguo: containers</text><text x="175" y="280" text-anchor="middle" class="d13">o communications</text>
  <rect x="350" y="246" width="310" height="46" rx="5" fill="#fdecec"/><text x="505" y="264" text-anchor="middle" class="d13">DaaS ambiguo: desktop</text><text x="505" y="280" text-anchor="middle" class="d13">o data</text>
  <rect x="20" y="298" width="640" height="18" rx="4" fill="#0055a0"/><text x="340" y="311" text-anchor="middle" class="s13">En una respuesta escrita, desarrolle siempre la sigla para no dar lugar a duda</text>
  <text x="340" y="330" text-anchor="middle" class="f13">[Fuente: NIST145, ISO17788]</text>
</svg>
```

---

## D14 · Los cuatro modelos de despliegue del NIST

**Sección**: §4 — Modelos de despliegue en Cloud Computing
**Propósito**: Fijar los cuatro modelos y desmontar los dos errores clásicos: creer que privada implica «en casa» y confundir híbrida con multicloud.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 343" role="img" aria-label="Los cuatro modelos de despliegue definidos por el NIST: nube privada de uso exclusivo de una organización, nube comunitaria para un conjunto de organizaciones con intereses comunes, nube pública abierta al público general y nube híbrida que combina dos o más de las anteriores manteniéndolas como entidades separadas; incluye la distinción entre nube híbrida y multicloud">
  <style>.t14{font:700 10.5px system-ui,sans-serif;fill:#fff}.s14{font:8.5px system-ui,sans-serif;fill:#fff}.d14{font:9px system-ui,sans-serif;fill:#333}.h14{font:700 13px system-ui,sans-serif;fill:#0055a0}.k14{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f14{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h14">Los cuatro modelos de despliegue (NIST)</text>
  <rect x="20" y="34" width="155" height="50" rx="6" fill="#2d8659"/><text x="97" y="54" text-anchor="middle" class="t14">PRIVADA</text><text x="97" y="68" text-anchor="middle" class="s14">uso exclusivo de UNA</text><text x="97" y="79" text-anchor="middle" class="s14">organización</text>
  <rect x="182" y="34" width="155" height="50" rx="6" fill="#0055a0"/><text x="259" y="54" text-anchor="middle" class="t14">COMUNITARIA</text><text x="259" y="68" text-anchor="middle" class="s14">una comunidad con</text><text x="259" y="79" text-anchor="middle" class="s14">intereses comunes</text>
  <rect x="344" y="34" width="155" height="50" rx="6" fill="#e89822"/><text x="421" y="54" text-anchor="middle" class="t14">PÚBLICA</text><text x="421" y="68" text-anchor="middle" class="s14">uso abierto</text><text x="421" y="79" text-anchor="middle" class="s14">al público general</text>
  <rect x="506" y="34" width="154" height="50" rx="6" fill="#888"/><text x="583" y="54" text-anchor="middle" class="t14">HÍBRIDA</text><text x="583" y="68" text-anchor="middle" class="s14">dos o más de las</text><text x="583" y="79" text-anchor="middle" class="s14">anteriores, unidas</text>
  <text x="20" y="106" class="k14">¿QUIÉN LA POSEE Y OPERA?</text>
  <rect x="20" y="112" width="155" height="34" rx="4" fill="#eef3f8"/><text x="97" y="127" text-anchor="middle" class="d14">La organización, un</text><text x="97" y="140" text-anchor="middle" class="d14">tercero o ambos</text>
  <rect x="182" y="112" width="155" height="34" rx="4" fill="#eef3f8"/><text x="259" y="127" text-anchor="middle" class="d14">Una o varias de ellas,</text><text x="259" y="140" text-anchor="middle" class="d14">un tercero o ambos</text>
  <rect x="344" y="112" width="155" height="34" rx="4" fill="#eef3f8"/><text x="421" y="127" text-anchor="middle" class="d14">El proveedor</text><text x="421" y="140" text-anchor="middle" class="d14">(empresa o entidad)</text>
  <rect x="506" y="112" width="154" height="34" rx="4" fill="#eef3f8"/><text x="583" y="127" text-anchor="middle" class="d14">Cada una conserva</text><text x="583" y="140" text-anchor="middle" class="d14">su titularidad</text>
  <text x="20" y="168" class="k14">¿DÓNDE ESTÁ?</text>
  <rect x="20" y="174" width="155" height="28" rx="4" fill="#f5f5f5"/><text x="97" y="192" text-anchor="middle" class="d14">Dentro O FUERA</text>
  <rect x="182" y="174" width="155" height="28" rx="4" fill="#f5f5f5"/><text x="259" y="192" text-anchor="middle" class="d14">Dentro o fuera</text>
  <rect x="344" y="174" width="155" height="28" rx="4" fill="#f5f5f5"/><text x="421" y="192" text-anchor="middle" class="d14">En el proveedor</text>
  <rect x="506" y="174" width="154" height="28" rx="4" fill="#f5f5f5"/><text x="583" y="192" text-anchor="middle" class="d14">Mixto</text>
  <rect x="20" y="212" width="640" height="24" rx="4" fill="#d13c3c"/><text x="340" y="228" text-anchor="middle" class="s14">Error clásico: una nube privada NO tiene por qué estar en las instalaciones del cliente ni ser propiedad suya</text>
  <rect x="20" y="244" width="310" height="52" rx="5" fill="#e8f4ec"/><text x="175" y="262" text-anchor="middle" class="d14">HÍBRIDA</text><text x="175" y="277" text-anchor="middle" class="d14">modelos de despliegue distintos</text><text x="175" y="290" text-anchor="middle" class="d14">unidos por portabilidad real</text>
  <rect x="350" y="244" width="310" height="52" rx="5" fill="#fdf3e2"/><text x="505" y="262" text-anchor="middle" class="d14">MULTICLOUD</text><text x="505" y="277" text-anchor="middle" class="d14">varios proveedores del mismo tipo</text><text x="505" y="290" text-anchor="middle" class="d14">(no es un modelo del NIST)</text>
  <rect x="20" y="302" width="640" height="20" rx="4" fill="#0055a0"/><text x="340" y="316" text-anchor="middle" class="s14">Se pueden dar a la vez: nube privada propia más dos proveedores públicos es híbrida y multicloud</text>
  <text x="340" y="334" text-anchor="middle" class="f14">[Fuente: NIST145]</text>
</svg>
```

---

## D15 · Responsabilidad compartida y los errores típicos

**Sección**: §4.1.1 — Características, multitenencia y modelo de responsabilidad compartida
**Propósito**: Visualizar la frontera entre la seguridad *de* la nube y la seguridad *en* la nube y enumerar los errores de configuración que concentran los incidentes reales.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 351" role="img" aria-label="Modelo de responsabilidad compartida: el proveedor responde de la seguridad de la nube (instalaciones, hardware, red troncal, hipervisor y aislamiento entre inquilinos) y el cliente responde de la seguridad en la nube (datos, identidades, configuración, red virtual, sistema operativo en IaaS, código y copias); se enumeran los tres errores de configuración que concentran los incidentes">
  <style>.t15{font:700 10.5px system-ui,sans-serif;fill:#fff}.s15{font:8.5px system-ui,sans-serif;fill:#fff}.d15{font:9px system-ui,sans-serif;fill:#333}.h15{font:700 13px system-ui,sans-serif;fill:#0055a0}.k15{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.n15{font:700 11px system-ui,sans-serif;fill:#fff}.f15{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h15">Responsabilidad compartida</text>
  <rect x="20" y="34" width="310" height="26" rx="5" fill="#0055a0"/><text x="175" y="52" text-anchor="middle" class="t15">SEGURIDAD EN LA NUBE — CLIENTE</text>
  <rect x="350" y="34" width="310" height="26" rx="5" fill="#888"/><text x="505" y="52" text-anchor="middle" class="t15">SEGURIDAD DE LA NUBE — PROVEEDOR</text>
  <rect x="20" y="66" width="310" height="20" rx="3" fill="#c9d9e8"/><text x="175" y="80" text-anchor="middle" class="d15">Datos y su clasificación</text>
  <rect x="20" y="90" width="310" height="20" rx="3" fill="#c9d9e8"/><text x="175" y="104" text-anchor="middle" class="d15">Identidades, permisos y mínimo privilegio</text>
  <rect x="20" y="114" width="310" height="20" rx="3" fill="#c9d9e8"/><text x="175" y="128" text-anchor="middle" class="d15">Configuración de cada servicio contratado</text>
  <rect x="20" y="138" width="310" height="20" rx="3" fill="#c9d9e8"/><text x="175" y="152" text-anchor="middle" class="d15">Red virtual, cifrado y gestión de claves</text>
  <rect x="20" y="162" width="310" height="20" rx="3" fill="#c9d9e8"/><text x="175" y="176" text-anchor="middle" class="d15">Sistema operativo y parcheo (solo en IaaS)</text>
  <rect x="20" y="186" width="310" height="20" rx="3" fill="#c9d9e8"/><text x="175" y="200" text-anchor="middle" class="d15">Código de la aplicación y copias de seguridad</text>
  <rect x="350" y="66" width="310" height="20" rx="3" fill="#e8e8e8"/><text x="505" y="80" text-anchor="middle" class="d15">Instalaciones, energía y refrigeración</text>
  <rect x="350" y="90" width="310" height="20" rx="3" fill="#e8e8e8"/><text x="505" y="104" text-anchor="middle" class="d15">Hardware y red troncal</text>
  <rect x="350" y="114" width="310" height="20" rx="3" fill="#e8e8e8"/><text x="505" y="128" text-anchor="middle" class="d15">Hipervisor y aislamiento entre inquilinos</text>
  <rect x="350" y="138" width="310" height="20" rx="3" fill="#e8e8e8"/><text x="505" y="152" text-anchor="middle" class="d15">Disponibilidad de regiones y zonas</text>
  <rect x="350" y="162" width="310" height="44" rx="3" fill="#f5f5f5"/><text x="505" y="180" text-anchor="middle" class="d15">Y, según el modelo, el sistema operativo,</text><text x="505" y="196" text-anchor="middle" class="d15">el middleware y la propia aplicación</text>
  <rect x="20" y="214" width="640" height="22" rx="4" fill="#e89822"/><text x="340" y="229" text-anchor="middle" class="s15">La frontera sube con el modelo de servicio, pero datos, identidades y configuración son SIEMPRE del cliente</text>
  <text x="20" y="256" class="k15">LOS TRES ERRORES QUE CONCENTRAN LOS INCIDENTES</text>
  <rect x="20" y="262" width="205" height="42" rx="5" fill="#d13c3c"/><text x="30" y="278" class="n15">1</text><text x="122" y="278" text-anchor="middle" class="s15">Almacenamiento de objetos</text><text x="122" y="292" text-anchor="middle" class="s15">expuesto públicamente</text>
  <rect x="238" y="262" width="205" height="42" rx="5" fill="#d13c3c"/><text x="248" y="278" class="n15">2</text><text x="340" y="278" text-anchor="middle" class="s15">Credenciales y claves</text><text x="340" y="292" text-anchor="middle" class="s15">incrustadas en el código</text>
  <rect x="456" y="262" width="204" height="42" rx="5" fill="#d13c3c"/><text x="466" y="278" class="n15">3</text><text x="558" y="278" text-anchor="middle" class="s15">Permisos excesivos</text><text x="558" y="292" text-anchor="middle" class="s15">«por comodidad»</text>
  <rect x="20" y="310" width="640" height="18" rx="4" fill="#0055a0"/><text x="340" y="323" text-anchor="middle" class="s15">La responsabilidad jurídica del dato se delega operativamente, pero nunca se externaliza</text>
  <text x="340" y="342" text-anchor="middle" class="f15">[Fuente: NIST144, CCN823, RGPD]</text>
</svg>
```

---

## D16 · Marco normativo del uso de la nube en la Administración

**Sección**: §5.1 — Marco regulatorio y de seguridad
**Propósito**: Reunir en un solo esquema las cuatro capas normativas que condicionan la contratación de servicios en la nube por una Administración española, con sus preceptos concretos.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 379" role="img" aria-label="Cuatro capas normativas que condicionan el uso de la nube en la Administración española: seguridad con el Esquema Nacional de Seguridad del Real Decreto 311/2022 y las guías del Centro Criptológico Nacional, protección de datos con el RGPD y la sentencia Schrems II, mercado con el Reglamento de Datos y la libre circulación de datos no personales, y estrategia con la Estrategia de nube híbrida y las leyes 39 y 40 de 2015">
  <style>.t16{font:700 10.5px system-ui,sans-serif;fill:#fff}.s16{font:8.5px system-ui,sans-serif;fill:#fff}.d16{font:9px system-ui,sans-serif;fill:#333}.h16{font:700 13px system-ui,sans-serif;fill:#0055a0}.k16{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.f16{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="20" text-anchor="middle" class="h16">El marco normativo de la nube pública en la Administración</text>
  <rect x="20" y="32" width="130" height="70" rx="5" fill="#0055a0"/><text x="85" y="60" text-anchor="middle" class="t16">SEGURIDAD</text><text x="85" y="76" text-anchor="middle" class="s16">ENS · RD 311/2022</text><text x="85" y="89" text-anchor="middle" class="s16">CCN-STIC 823 y 105</text>
  <rect x="158" y="32" width="502" height="20" rx="3" fill="#eef3f8"/><text x="168" y="46" class="d16">Art. 2.3: el ENS se extiende al proveedor privado que presta servicios al sector público</text>
  <rect x="158" y="55" width="502" height="20" rx="3" fill="#f5f5f5"/><text x="168" y="69" class="d16">Art. 40 categorías BÁSICA / MEDIA / ALTA · Art. 31 auditoría al menos cada dos años</text>
  <rect x="158" y="78" width="502" height="24" rx="3" fill="#eef3f8"/><text x="168" y="94" class="d16">Anexo II: op.ext.1 a op.ext.4 (recursos externos) y op.nub.1 (servicios en la nube)</text>
  <rect x="20" y="110" width="130" height="70" rx="5" fill="#2d8659"/><text x="85" y="138" text-anchor="middle" class="t16">DATOS</text><text x="85" y="154" text-anchor="middle" class="s16">RGPD · LOPDGDD</text><text x="85" y="167" text-anchor="middle" class="s16">Schrems II · DPF</text>
  <rect x="158" y="110" width="502" height="20" rx="3" fill="#e8f4ec"/><text x="168" y="124" class="d16">Art. 28: el proveedor es ENCARGADO del tratamiento; la Administración sigue siendo responsable</text>
  <rect x="158" y="133" width="502" height="20" rx="3" fill="#f5f5f5"/><text x="168" y="147" class="d16">Sin subcontratar sin autorización · devolver o suprimir los datos al terminar</text>
  <rect x="158" y="156" width="502" height="24" rx="3" fill="#e8f4ec"/><text x="168" y="172" class="d16">Arts. 44-50 transferencias · adecuación UE-EE. UU. de 10-07-2023 para entidades certificadas</text>
  <rect x="20" y="188" width="130" height="70" rx="5" fill="#e89822"/><text x="85" y="216" text-anchor="middle" class="t16">MERCADO</text><text x="85" y="232" text-anchor="middle" class="s16">Reglamento de Datos</text><text x="85" y="245" text-anchor="middle" class="s16">y datos no personales</text>
  <rect x="158" y="188" width="502" height="20" rx="3" fill="#fdf3e2"/><text x="168" y="202" class="d16">Reglamento (UE) 2023/2854, cap. VI (arts. 23-31): derecho de cambio de proveedor</text>
  <rect x="158" y="211" width="502" height="20" rx="3" fill="#f5f5f5"/><text x="168" y="225" class="d16">Art. 29: desde el 12 de enero de 2027, PROHIBIDAS las tarifas de cambio</text>
  <rect x="158" y="234" width="502" height="24" rx="3" fill="#fdf3e2"/><text x="168" y="250" class="d16">Reglamento (UE) 2018/1807: libre circulación de datos no personales en la Unión</text>
  <rect x="20" y="266" width="130" height="66" rx="5" fill="#888"/><text x="85" y="290" text-anchor="middle" class="t16">ESTRATEGIA</text><text x="85" y="306" text-anchor="middle" class="s16">Estrategia cloud AAPP</text><text x="85" y="319" text-anchor="middle" class="s16">Leyes 39 y 40/2015</text>
  <rect x="158" y="266" width="502" height="20" rx="3" fill="#e8e8e8"/><text x="168" y="280" class="d16">Principio de nube híbrida primero · 7 pilares y 19 iniciativas · NubeSARA</text>
  <rect x="158" y="289" width="502" height="20" rx="3" fill="#f5f5f5"/><text x="168" y="303" class="d16">Soberanía: categoría ALTA solo con empresas de jurisdicción exclusivamente comunitaria</text>
  <rect x="158" y="312" width="502" height="20" rx="3" fill="#e8e8e8"/><text x="168" y="326" class="d16">Ley 40/2015: interoperabilidad, seguridad y reutilización (arts. 156-158)</text>
  <rect x="20" y="338" width="640" height="20" rx="4" fill="#0055a0"/><text x="340" y="352" text-anchor="middle" class="s16">Primero se categoriza y se analiza el dato; después se elige la tecnología</text>
  <text x="340" y="372" text-anchor="middle" class="f16">[Fuente: ENS, RGPD, DATAACT, ESTRATEGIA-CLOUD]</text>
</svg>
```

---

## D17 · Los 7 pilares de la Estrategia de nube híbrida

**Sección**: §5.2.1 — Principios de preferencia Cloud y transformación digital
**Propósito**: Presentar la estructura completa y cerrada de la Estrategia española —siete pilares y diecinueve iniciativas— tal como la formula el documento oficial.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 361" role="img" aria-label="Los siete pilares de la Estrategia de servicios en la nube híbrida para las Administraciones Públicas de diciembre de 2022: nube híbrida por diseño, catálogo de servicios creciente, política de nube híbrida primero, soberanía del dato, orientación al dato, evolución de sistemas hacia la nube híbrida y nube segura, con sus diecinueve iniciativas agrupadas">
  <style>.t17{font:700 10px system-ui,sans-serif;fill:#fff}.s17{font:8.5px system-ui,sans-serif;fill:#fff}.d17{font:9px system-ui,sans-serif;fill:#333}.h17{font:700 13px system-ui,sans-serif;fill:#0055a0}.k17{font:700 9.5px system-ui,sans-serif;fill:#0055a0}.n17{font:700 12px system-ui,sans-serif;fill:#fff}.f17{font:8px system-ui,sans-serif;fill:#777}</style>
  <text x="340" y="18" text-anchor="middle" class="h17">Estrategia de servicios en la nube híbrida para las AAPP (diciembre de 2022)</text>
  <text x="340" y="34" text-anchor="middle" class="k17">7 PILARES · 19 INICIATIVAS</text>
  <rect x="20" y="42" width="34" height="34" rx="5" fill="#0055a0"/><text x="37" y="64" text-anchor="middle" class="n17">1</text>
  <rect x="60" y="42" width="180" height="34" rx="4" fill="#0055a0"/><text x="150" y="56" text-anchor="middle" class="s17">NUBE HÍBRIDA POR DISEÑO</text><text x="150" y="69" text-anchor="middle" class="s17">i1 ampliar nube privada · i2 interoperar</text>
  <rect x="20" y="82" width="34" height="34" rx="5" fill="#0055a0"/><text x="37" y="104" text-anchor="middle" class="n17">2</text>
  <rect x="60" y="82" width="180" height="34" rx="4" fill="#0055a0"/><text x="150" y="96" text-anchor="middle" class="s17">CATÁLOGO CRECIENTE</text><text x="150" y="109" text-anchor="middle" class="s17">i3 tienda · i4 intermediar · i5 ampliar</text>
  <rect x="20" y="122" width="34" height="34" rx="5" fill="#e89822"/><text x="37" y="144" text-anchor="middle" class="n17">3</text>
  <rect x="60" y="122" width="180" height="34" rx="4" fill="#e89822"/><text x="150" y="136" text-anchor="middle" class="s17">NUBE HÍBRIDA PRIMERO</text><text x="150" y="149" text-anchor="middle" class="s17">i6 priorizar · i7 contratación</text>
  <rect x="20" y="162" width="34" height="34" rx="5" fill="#2d8659"/><text x="37" y="184" text-anchor="middle" class="n17">4</text>
  <rect x="60" y="162" width="180" height="34" rx="4" fill="#2d8659"/><text x="150" y="176" text-anchor="middle" class="s17">SOBERANÍA DEL DATO</text><text x="150" y="189" text-anchor="middle" class="s17">i8 guía de riesgos · i9 contratación</text>
  <rect x="260" y="42" width="34" height="34" rx="5" fill="#2d8659"/><text x="277" y="64" text-anchor="middle" class="n17">5</text>
  <rect x="300" y="42" width="200" height="34" rx="4" fill="#2d8659"/><text x="400" y="56" text-anchor="middle" class="s17">ORIENTACIÓN AL DATO</text><text x="400" y="69" text-anchor="middle" class="s17">i10 plataforma del dato · i11 analítica</text>
  <rect x="260" y="82" width="34" height="34" rx="5" fill="#888"/><text x="277" y="104" text-anchor="middle" class="n17">6</text>
  <rect x="300" y="82" width="200" height="34" rx="4" fill="#888"/><text x="400" y="96" text-anchor="middle" class="s17">EVOLUCIÓN A LA NUBE</text><text x="400" y="109" text-anchor="middle" class="s17">i12 transformar · i13 consolidar · i14 cargas</text>
  <rect x="260" y="122" width="34" height="74" rx="5" fill="#d13c3c"/><text x="277" y="164" text-anchor="middle" class="n17">7</text>
  <rect x="300" y="122" width="200" height="74" rx="4" fill="#d13c3c"/><text x="400" y="140" text-anchor="middle" class="s17">NUBE SEGURA</text><text x="400" y="155" text-anchor="middle" class="s17">i15 certificación ENS de la nube</text><text x="400" y="168" text-anchor="middle" class="s17">i16 capacidades · i17 centro de operaciones</text><text x="400" y="181" text-anchor="middle" class="s17">i18 red de SOC · i19 guías CCN-STIC</text>
  <rect x="516" y="42" width="144" height="154" rx="5" fill="#eef3f8"/><text x="588" y="62" text-anchor="middle" class="k17">DESAFÍOS</text><text x="588" y="82" text-anchor="middle" class="d17">Autonomía tecnológica</text><text x="588" y="100" text-anchor="middle" class="d17">Soberanía del dato</text><text x="588" y="118" text-anchor="middle" class="d17">Redundancia y resiliencia</text><text x="588" y="136" text-anchor="middle" class="d17">Interoperabilidad</text><text x="588" y="154" text-anchor="middle" class="d17">Protección de datos</text><text x="588" y="172" text-anchor="middle" class="d17">Ciberseguridad</text>
  <rect x="20" y="208" width="640" height="24" rx="4" fill="#0055a0"/><text x="340" y="224" text-anchor="middle" class="s17">El principio español no es «cloud first» sin matices, sino «nube híbrida primero» (hybrid first)</text>
  <rect x="20" y="240" width="310" height="60" rx="5" fill="#c9d9e8"/><text x="175" y="258" text-anchor="middle" class="d17">NubeSARA</text><text x="175" y="274" text-anchor="middle" class="d17">Nube PRIVADA de la AGE, desplegada</text><text x="175" y="288" text-anchor="middle" class="d17">por la SGAD en 2015 sobre la red SARA</text>
  <rect x="350" y="240" width="310" height="60" rx="5" fill="#cfe3d6"/><text x="505" y="258" text-anchor="middle" class="d17">Catálogo IaaS y PaaS con costes</text><text x="505" y="274" text-anchor="middle" class="d17">y acuerdos de nivel de servicio; evoluciona</text><text x="505" y="288" text-anchor="middle" class="d17">a una «tienda» para todas las AAPP</text>
  <rect x="20" y="308" width="640" height="22" rx="4" fill="#e89822"/><text x="340" y="323" text-anchor="middle" class="s17">No confundir la red SARA (la red de interconexión) con NubeSARA (la nube desplegada sobre ella)</text>
  <text x="340" y="352" text-anchor="middle" class="f17">[Fuente: ESTRATEGIA-CLOUD]</text>
</svg>
```
