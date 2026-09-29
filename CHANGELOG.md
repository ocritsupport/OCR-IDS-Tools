# Historial de versiones

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
