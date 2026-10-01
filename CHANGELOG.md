# Historial de versiones

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
