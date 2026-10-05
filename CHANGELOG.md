# Historial de versiones

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
