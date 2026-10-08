# Historial de versiones

## Sin publicar

## 2.4.11 — 08/10/2026

- **Cluster Editor: «Consultar cuadro» decía un SKC falso** («SKC según el cuadro: 00014»). El
  analizador se quedaba con la última línea del terminal que contuviera «SKC», y desde la 2.4.8
  esa línea es el cronómetro de la propia app («⏱ GetSKC: 14.0 s»). Solo vale ya la línea exacta
  «SKC: nnnnn» de kw1281test; si no está, se dice que no se pudo extraer.
- **Cluster Editor: el byte 0x10C pasa a contador vivo.** En el ensayo de clonado en banco con un
  1J5 920 846 C (ROM VWK501MH 01.00) se grabó el valor que el propio cuadro tenía y la relectura
  devolvió otro. Es el primer byte del bloque 0x10C-0x11B, los contadores del intervalo de servicio
  (se ponen a cero con el reset de servicio y acumulan solos). Ya no dispara «NO COINCIDE».
- Cluster Editor: tras una grabación verificada se avisa de **quitar y dar contacto**: el cuadro
  lee el idioma (0x147) y los ajustes al arrancar. Comprobado en el 846 C: el FIS seguía en alemán
  con 05 ya grabado hasta rearrancar.
- Cluster Editor: la etiqueta ROM enseña la **versión completa** declarada por el cuadro tras
  «Consultar cuadro» (antes solo la familia; la versión exacta es la que decide si un parche de
  barrido de agujas es compatible).
- Cluster Editor: averías 00779 (G17), 01321 (airbag sin comunicación) y 01336 (CAN confort por un
  solo hilo) documentadas.

- **Funciones rápidas VAG** (pantalla VAG y «Codificación y adaptaciones»): las recetas de
  siempre por su nombre, para coches de línea K con el cable KKL. Cierre y confort
  (centralita 46, o 35 con elevalunas manuales): autocierre al iniciar la marcha, apertura
  al sacar la llave, confirmación de apertura y cierre con bocina o intermitentes, sonido de
  la bocina de la alarma, emparejado de mandos y apertura selectiva o de todas las puertas.
  Cuadro (17): idioma, corrección de consumo y avisos de cinturón, pastillas y
  limpiaparabrisas. Reglas: **sin «Leer estado» no hay «Guardar»**, solo valores
  documentados, una codificación que no esté en la tabla no se toca, y tras guardar **se
  relee y se compara** (una centralita que contesta bien y no guarda queda en evidencia).
  El confort se busca a 10400 y a 9600 baudios y se recuerda la velocidad que funcionó.
  Cada receta enseña sus fuentes y lo que nos fiamos de ella; las cuatro confirmaciones
  (canales 06-09) van marcadas como dudosas porque la página antigua de Ross-Tech las
  numera 05-08. **Nada del 46/35 está probado todavía en un coche.**
- **Guía por modelo VAG**: 114 modelos de VW, Audi, Seat/Cupra y Skoda con su generación de
  diagnóstico (línea K, CAN de primera generación con TP2.0, UDS, UDS con SFD), el cable
  que piden y lo que la aplicación puede y no puede hacer con ellos hoy. Buscador («golf 4»,
  «leon 5f»), salto a la herramienta que toca y enlace a las recetas del modelo en
  vag-coding.net. No conecta con nada: es para saberlo antes de enchufar.
- De dónde sale: revisión de vag-coding.net del 06/10/2026 (unas 6.300 recetas para VCDS y
  OBDeleven). No se copia su contenido: el catálogo es propio, contrastado entrada a entrada
  con la wiki de Ross-Tech, y lo demás se enlaza. Detalle y pendientes en
  `docs/HOJA_DE_RUTA_MULTIMARCA.md`, punto 7.

## 2.4.10 — 05/10/2026

- **La prueba de actuadores del cuadro (agujas y testigos) se quedaba clavada en el primer paso**
  («Tachometer»): ni «Siguiente» ni «Terminar» hacían nada y el cuadro se quedaba en modo prueba.
  Mientras espera la tecla, el motor manda al cuadro un ACK tras otro (keep-alive) y escribe un
  punto por cada uno, sin salto de línea, varias veces por segundo; el lector de la pseudo-consola
  encolaba una línea vacía por cada punto y el bucle, ocupado en vaciarlas, no llegaba nunca a
  enviar las teclas. Ahora las teclas y el fin del proceso se miran en cada vuelta, los puntos no
  generan líneas y «Terminar» corta el proceso a los 5 s si el motor no sale solo. Reproducido con
  un programa .NET con la misma estructura y con el simulador de pruebas, que ahora también
  escribe los puntos.
- Los comandos interactivos del cuadro (bloques de medición en vivo y prueba de actuadores)
  arrancan al instante: ConPTY preguntaba al «terminal» qué es (`ESC[c`) y, sin respuesta, se
  quedaba 3 s parado antes de lanzar el motor. Se le contesta como un VT100.
- Si el cuadro se quedó en modo prueba por la versión anterior: quitar y dar el contacto.

## 2.4.9 — 05/10/2026

- **Las actualizaciones saltan solas.** Hasta ahora se comprobaban «una vez al día» y el día se
  marcaba en la primera apertura: una versión publicada después no se ofrecía hasta el día siguiente,
  y en el usuario del modo terminal (el programa no se reinicia nunca) no se ofrecía jamás: había que
  ir a Ajustes → «Buscar ahora». Ahora se comprueba en cada arranque y cada 4 horas mientras siga
  abierto (nunca dos veces en menos de 15 minutos). Ajustes enseña «Última comprobación automática»
  con la hora y el resultado, también cuando falla por red.
- Motor `kw1281test 1.0.2-ocr3`: si no puede crear `KW1281Test.log` en el directorio de trabajo, lo crea
  en `%TEMP%` y, si tampoco, sigue solo por consola. Era la causa real del cuadro que «no se leía» en el
  taller: en el usuario del **modo terminal** la app arranca como shell de Windows con el directorio de
  trabajo en `System32`, el motor moría con `UnauthorizedAccessException` antes de hablar con el coche
  (`⏱ DumpEeprom: 1.5 s`) y no quedaba ningún `.log`. La 2.4.8 ya lo evita desde la app (directorio de
  trabajo fijo por usuario); esto es el cinturón por si el motor se lanza desde cualquier otro sitio.
  Recordatorio: el instalador es por usuario, cada usuario de Windows tiene su copia y su versión.

## 2.4.8 — 05/10/2026

- **Cluster Editor: red de seguridad para el motor rápido.** Un intento fallido en el taller con el
  846 C (al final el cable había pasado de COM4 a COM2 para VAG EEPROM Programmer y la app seguía
  en COM4; con el puerto bueno la 2.4.7 lo lee) sirvió para añadir una protección que faltaba:
  si un comando de **solo lectura** al cuadro
  falla por comunicación, lo repite primero por el puerto COM (si iba por acceso directo D2XX, que
  deja 2 ms entre bytes en vez de 16) y después en **modo compatible** del motor, que son los
  tiempos de la versión original de kw1281test (los de hasta la 2.4.6). Lo que funciona se queda
  puesto el resto de la sesión y el registro lo dice (`[modo compatible]` en la línea del comando y
  «✔ Así ha funcionado…»). Los comandos que escriben y las demás centralitas no se repiten solos.
- Motor `kw1281test 1.0.2-ocr2`: `KW1281TEST_COMPATIBLE=1` (reloj de Windows sin tocar) y
  `KW1281TEST_R6=<ms>` (pausa entre bytes, 2 ms de serie) por variable de entorno. El `.csproj`
  del motor ya va en git (el `.gitignore` original lo dejaba fuera).
- **Registro de sesión del Cluster Editor** en `%LOCALAPPDATA%\OCR IDS Tools\registros\cluster_<fecha>.log`
  (todo lo que pasa por el terminal, con hora) y kw1281test lanzado con esa carpeta como directorio de
  trabajo, para que su `KW1281Test.log` esté siempre ahí. Motivo: el intento fallido del taller no dejó
  ningún `.log` que encontrar (instalado, el directorio de trabajo dependía del acceso directo).

## 2.4.7 — 03/10/2026

- **Batería y tensión en la cabecera**: arriba a la derecha, la batería del portátil (porcentaje,
  rayo si carga; en un PC de sobremesa no aparece) y la tensión del coche por OBD. Verde, ámbar
  o rojo según el nivel, con el aviso de no programar ni grabar centralitas con la batería
  baja. La tensión sale del adaptador (ELM327 `ATRV`, J2534) o, con cable KKL, del PID 42 de
  la centralita. Se lee cada 5 s solo si el cable está libre (nunca a la vez que los datos en
  vivo). Sin conexión muestra «— V»; pulsándola se abre la conexión.
- **Cluster Editor al día (03/10)**: lectura y grabación del cuadro mucho más rápidas, con
  kw1281test compilado por nosotros (reloj de Windows a 1 ms, sin pausas de arranque y
  grabación en una sola conexión que solo reescribe lo que cambia). **Sin probar en el coche.**
- **Datos en vivo con gráfica**: debajo de la tabla, cada parámetro a su propia escala, con
  último valor y mínimo/máximo en la leyenda, ocultar pulsando en la leyenda y lectura de
  valores al pasar el ratón. En vivo enseña el último minuto.
- **Abrir CSV…** en Datos en vivo: lee registros de este programa, de VCDS (un TIME por grupo,
  columna Marker ignorada) y de ME7Logger/NefMoto o cualquier tabla con una fila TIME.
  El CSV exportado lleva ahora la unidad en una cuarta columna.
- **Cluster Editor al día (02/10)**: con un cable KKL de chip FTDI, kw1281test se abre por
  acceso directo D2XX (latencia 2 ms en vez de los 16 ms del COM virtual: cada byte de
  KW1281 espera ese temporizador, así que las lecturas de EEPROM van bastante más rápidas).
  El selector de puerto ofrece primero el acceso rápido y después el COM; si el rápido no
  abre el cable, se repite por el COM solo. El escaneo de centralitas reintenta a 9600
  baudios las que contestan pero no sincronizan a 10400 (módulo de confort y otras).
  Ideas tomadas de KKL Kombajn 4 (Ch4ist0). **Sin probar aún con cable real.**

## 2.4.6 — 01/10/2026

- **Pantalla de inicio**: el logo de fondo, un poco más marcado (16 % en claro, 22 % en oscuro).

## 2.4.5 — 01/10/2026

- **Actualizaciones**: en PCs cuyo almacén de certificados de Windows no tiene aún el emisor de
  GitHub, buscar actualizaciones fallaba con «CERTIFICATE_VERIFY_FAILED». El programa lleva ahora
  su propio paquete de raíces (certifi) además de las de Windows. La verificación no se desactiva.
- `--diagnostico` comprueba también si el canal de fábrica es alcanzable desde ese PC.

## 2.4.4 — 01/10/2026

- **Actualizaciones**: un «Canal» guardado en blanco en Ajustes anulaba la dirección de fábrica y
  el programa no buscaba nunca (así se quedó la 2.4.2 del taller). Ahora vacío = canal de
  fábrica, Ajustes lo muestra, y guardar en blanco o con la de fábrica no deja nada grabado.

## 2.4.3 — 01/10/2026

- **Pantalla de inicio**: el logo de OCR IDS Tools aparece de fondo, difuminado y muy tenue,
  detrás de los cuadros (en los dos temas).
- **Cluster Editor** al día con el original del 01/10: avisos del asistente de clonado para el
  cuadro 1J5 920 846 C (FULL FIS), nota del testigo del cinturón en la codificación e idioma
  del MFA/FIS en el panel del vehículo.
- Primera versión que llega por el **canal de actualizaciones** (la 2.4.2 debe ofrecerla sola).

## 2.4.2 — 29/09/2026

- **Mis programas**: corregido el error «startfile() argument 'arguments' must be str, not
  None» de la 2.4.1, que impedía abrir los programas cuyo acceso directo no lleva argumentos
  (INPA, VCDS…). Los que sí los llevan (CLIP) ya abrían.
- Nuevo modo `--lanzar` para probar un mosaico desde la línea de órdenes y ver el error exacto.

## 2.4.1 — 29/09/2026

- **Arranque 3-4 veces más rápido**: el programa pasa de un exe único (que descomprimía
  ~150 MB en la carpeta temporal en cada arranque) a una carpeta instalada. Primera pantalla
  en 0,3 s y ventana principal en 1,5 s.
- La pantalla de carga ya no va a saltos: los módulos se cargan en segundo plano.
- **Mis programas**: VCDS, CLIP, INPA y cualquier programa añadido a mano arrancan con su
  carpeta de trabajo correcta (antes «no encontraban su exe» nada más abrirse). Los accesos
  directos se resuelven en el propio programa.
- **Canal de actualizaciones de fábrica**: este repositorio. Ya no hace falta escribir la
  dirección en Ajustes.
- Nuevo modo `--resolver` para diagnosticar mosaicos de «Mis programas» que no abren.

## 2.4.0 — 27/09/2026

- Suite completa: diagnóstico OBD-II/UDS multimarca (ELM327, Carly, OBDLink, ENET, J2534),
  VAG por línea K, BMW K+DCAN y DS2, Renault con DDT2000, Cluster Editor y TPMS Tools bajo
  una pantalla de mosaicos.
- Base de 18.818 averías (9.415 genéricas en español).
- Mosaicos de «Mis programas» con detección automática en el menú Inicio.
- Modo terminal (sustituir el escritorio de Windows) y actualizaciones firmadas.
