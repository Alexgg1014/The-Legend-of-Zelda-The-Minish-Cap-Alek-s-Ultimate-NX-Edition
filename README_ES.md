<p align="center">
  <img src="docs/images/portada-aleks-version.png"
       alt="The Legend of Zelda: The Minish Cap – Alek's Ultimate NX Edition"
       width="460">
</p>

# The Legend of Zelda: The Minish Cap — Alek's Ultimate NX Edition

**v1.3.9** · Port homebrew nativo para Nintendo Switch · [English → README.md](README.md)

![Juego a doble pantalla en Nintendo Switch](docs/screenshots/hero/01_hero_dual.jpg)

## ¿Qué es esto?

Una edición centrada en Nintendo Switch del port nativo de *The Minish Cap*,
construida sobre la descompilación de código abierto y los proyectos de port
nativo listados en [CREDITS.md](CREDITS.md). El juego corre de forma nativa en
la Switch — no es un emulador — y añade una segunda pantalla de compañía,
interacción táctil y por mando, RetroAchievements, una traducción completa del
juego al portugués de Brasil, una interfaz del port localizada, un
actualizador integrado y varias mejoras de calidad de vida específicas de
Switch.

Lo hice porque quería jugar *The Minish Cap* de esta manera
concreta en mi propia Switch: con una disposición a doble pantalla al estilo
de los Zelda de DS, un modo vertical tipo Flip Grip y logros. Se comparte
públicamente por si otras personas quieren jugarlo, estudiarlo, hacer un fork
o usarlo como punto de partida para su propio trabajo.

Este proyecto **no** creó la descompilación de Minish Cap, ni el port nativo
para PC, ni el concepto original de doble pantalla. Se apoya en los proyectos
acreditados — consulta [CREDITS.md](CREDITS.md) para el linaje completo.

## Características

- **Compilación nativa para Switch** — sin emulador, funciona como homebrew (NRO).
- **Pantalla ancha (16:9)** en modo NORMAL: la cámara muestra más sala a ambos
  lados, los cuadros de texto quedan centrados y el HUD se ancla a los bordes.
  Las salas más estrechas que la vista se muestran con bandas, nunca
  estiradas. Mantiene 60 FPS con la CPU a frecuencia de serie gracias al
  renderizador por tiles.
- **Calidad de vida** (AJUSTES → GAMEPLAY → CALIDAD DE VIDA, todo con
  interruptor): movimiento de 360° con el stick, ataque giratorio dibujando un
  círculo con el stick y pulsando B, ataque de voltereta con un botón, carrera
  de botas Pegaso que gira con el stick (1.4.1), tope de conchas 9999, sin
  pista de Ezlo al cargar partida, figuras con mejores probabilidades, y
  opcionales Modo Héroe y saltar tutoriales de Ezlo.
- **Filtros de pantalla** — scanlines (fuertes / suaves) y rejilla LCD de GBA,
  dibujados en la GPU sin coste de FPS (AJUSTES → DISPLAY).
- **Tres modos de pantalla**
  - **NORMAL** — presentación clásica a una sola pantalla (Fit o 2×/3×/4× exacto).
  - **DUAL** — el juego más un panel de segunda pantalla, lado a lado.
  - **FLIP 270** — composición vertical pensada para girar físicamente la
    Switch (funciona muy bien con un soporte tipo Flip Grip; sin afiliación
    con ningún fabricante de accesorios).
- **Panel de segunda pantalla** con cuatro pestañas:
  - **MISIÓN** — misión principal actual, objetivo, pista y ubicación (Guía de
    Historia).
  - **MAPA** — mapas del mundo y de mazmorras con el marcador de Link en vivo,
    marcas de windcrests, vuelta automática de piso y cámara de seguimiento
    opcional.
  - **OBJETOS** — vista de equipo con los anillos A/B y los anillos de atajo
    X/Y/ZL/ZR (soft slots: un tercer y cuarto objeto sin abrir el menú de
    pausa). Arrastra un objeto a cualquier anillo para asignarlo, arrastra un
    anillo sobre otro para intercambiarlos, y arrastra un atajo fuera (o
    mantenlo pulsado) para vaciarlo (1.4.4).
  - **AJUSTES** — todos los ajustes del port, el actualizador y la info del build.
- **Funciona con táctil o con mando.** En modo NORMAL el panel se abre como
  superposición con **Menos** y se queda en cualquier pestaña: la cruceta
  mueve, **A / B** activan (en OBJETOS: equipar en la ranura A o B), **L / R**
  cambian de pestaña, y **A** sobre el mapa abre la región donde está Link.
- **Traducción completa del juego al portugués de Brasil** (todos los
  diálogos, carteles y descripciones — 2910 mensajes), sobre la ROM USA. Se
  elige en AJUSTES → GENERAL → IDIOMA.
- **RetroAchievements** — inicio de sesión, reconocimiento del juego, Rich
  Presence, notificaciones de desbloqueo con la insignia real, y **juego sin
  conexión**: los logros conseguidos offline se guardan con su hora original y
  se envían al reconectar (solo softcore, según la política de
  RetroAchievements). Requiere una cuenta gratuita de
  [retroachievements.org](https://retroachievements.org).
- **Partidas intercambiables con emuladores** — `tmc.sav` usa el mismo orden
  de bytes que mGBA / VBA-M / volcados de cartucho (desde la v1.3.9). Cópialo
  como `<rom>.sav` o de vuelta, sin conversión.
- **Autoguardado gestionado por el port** y **Cargar Autoguardado** —
  separados del sistema de guardado propio del juego; nunca lo sobrescriben.
- **Actualizador integrado** — busca, descarga e instala versiones nuevas desde
  la segunda pantalla, sin PC. La descarga se verifica contra un tamaño y un
  SHA-256 fijados en el manifiesto, antes se hace una copia verificada de tu
  versión actual, y cualquier fallo la restaura. Ver [Actualizar](#actualizar).
- Atajo **Hablar con Ezlo** (click del stick izquierdo por defecto,
  reasignable), entrada **Volver al Título**, y **Ayuda de Acción** contextual
  para el botón R (NO / CONTEXTUAL / SÍ).
- **Interfaz del port localizada** en English, Español, Français, Deutsch,
  Italiano y Português (Brasil). (La localización de los diálogos originales
  es de Nintendo; este proyecto localiza la interfaz añadida por el port y
  aporta la traducción PT-BR del juego como trabajo propio.)
- **Fast Boot** — arranque de unos 13–14 s en el hardware probado; el tiempo
  real depende de la tarjeta SD.
- **Motor reforzado** — la línea 1.3.x auditó todos los tipos de entidad
  contra desfases de layout de 64 bits (~130 estructuras, cada una con
  comprobación en compilación), portó las correcciones de motor del port de
  PC hasta su v0.9.3 y cerró los crashes y bloqueos reportados por la
  comunidad. Detalles en [CHANGELOG.md](CHANGELOG.md).

**Widescreen** (16:9 real con más mundo en pantalla, sin estirar) está en
desarrollo para la **v1.4** y no forma parte de la v1.3.9.

## Capturas

| DUAL | NORMAL | FLIP 270 |
|---|---|---|
| ![DUAL](docs/screenshots/gameplay/02_dual_gameplay.jpg) | ![NORMAL](docs/screenshots/gameplay/03_normal.jpg) | ![FLIP](docs/screenshots/flip/06_flip270.jpg) |

Pantalla ancha (v1.4.0), 240 columnas nativas junto a la vista 16:9 de 284:

![Pantalla ancha en Hyrule Town](docs/screenshots/widescreen/02_widescreen_town.png)
![Diálogo y HUD en pantalla ancha](docs/screenshots/widescreen/05_widescreen_dialogue_hud.png)
![Overlays BG3 en toda la vista](docs/screenshots/widescreen/04_bg3_overlay_clouds.png)
![Oclusión en puertas](docs/screenshots/widescreen/03_stairs_occlusion.png)

Ajustes (v1.4.0): la página de Calidad de vida, los atajos y pantalla ancha:

| CALIDAD DE VIDA | CONTROLES | PANTALLA ANCHA |
|---|---|---|
| ![QoL](docs/screenshots/settings/10_qol_page1.jpg) | ![Controles](docs/screenshots/settings/12_controls_softslots.jpg) | ![Pantalla ancha](docs/screenshots/settings/13_display_widescreen_on.jpg) |

Desbloqueo real de un logro, en hardware real, en el Santuario del Bosque:

![Desbloqueo de RetroAchievements](docs/screenshots/achievements/08_retroachievements_dungeon_map.jpg)

### Localización de la interfaz del port

<table>
  <tr>
    <td align="center"><b>English</b></td>
    <td align="center"><b>Español</b></td>
    <td align="center"><b>Français</b></td>
  </tr>
  <tr>
    <td><img src="docs/screenshots/localization/en.jpg" alt="Interfaz del port en inglés" width="280"></td>
    <td><img src="docs/screenshots/localization/es.jpg" alt="Interfaz del port en español" width="280"></td>
    <td><img src="docs/screenshots/localization/fr.jpg" alt="Interfaz del port en francés" width="280"></td>
  </tr>
  <tr>
    <td align="center"><b>Deutsch</b></td>
    <td align="center"><b>Italiano</b></td>
    <td></td>
  </tr>
  <tr>
    <td><img src="docs/screenshots/localization/de.jpg" alt="Interfaz del port en alemán" width="280"></td>
    <td><img src="docs/screenshots/localization/it.jpg" alt="Interfaz del port en italiano" width="280"></td>
    <td></td>
  </tr>
</table>

La captura en inglés muestra la pestaña MAPA y las otras cuatro muestran
MISIÓN — en las cinco se ve la interfaz propia del port en ese idioma. Más en
[docs/SCREENSHOTS.md](docs/SCREENSHOTS.md).

### Localización y ROM base

Alek's Ultimate NX Edition está probado principalmente con la **ROM USA** como
base recomendada. El port añade su propia capa de interfaz multilenguaje para
los menús y funciones específicas del port en inglés, español, francés, alemán
e italiano.

El contenido y las traducciones originales del juego siguen perteneciendo a los
datos originales del juego; este proyecto no se atribuye las traducciones
oficiales de Nintendo. Poner la interfaz del port en español no convierte una
ROM USA en otra edición regional — el idioma de los diálogos del juego es el
que proporcione tu propia ROM.

## Instalación

Versión corta — guía completa en
[docs/INSTALLATION_ES.md](docs/INSTALLATION_ES.md):

1. Copia el NRO en `/switch/tmc/` de tu tarjeta SD sin cambiarle el nombre
   (`tmc_aleks_ultimate_nx.nro`): ese es el archivo que reemplaza el
   actualizador integrado.
2. Copia tu ROM **propia y obtenida legalmente** de *The Minish Cap* versión
   **USA** (`.gba`) en la misma carpeta. El nombre del archivo no importa — el
   port identifica la ROM por su cabecera. `baserom.gba` es el nombre
   convencional.
3. Lánzalo desde el Homebrew Menu. Se recomienda el modo aplicación (lanzar
   sobre un título completo) para el mejor rendimiento.

**Este proyecto no incluye ninguna ROM, no proporciona enlaces de descarga de
ROMs y no distribuye contenido del juego con derechos de autor.** Debes volcar
tu propio cartucho u obtener el juego legalmente.

### Partidas guardadas

- `/switch/tmc/tmc.sav` es la partida del juego (los tres archivos internos).
  Desde la **v1.3.9** se escribe en el orden de bytes estándar de los
  emuladores, así que puedes copiarla a mGBA / VBA-M como `<rom>.sav` y de
  vuelta sin conversión. Las partidas de versiones anteriores se detectan y
  cargan solas.
- **Una partida escrita por la v1.3.9 o posterior no carga en la v1.3.8 o
  anteriores.** Si alguna vez vuelves atrás, restaura `tmc.sav.bak` (copia
  que se hace en cada arranque).
- Los autoguardados (`autosave_*.bin`) son del port y son independientes de
  `tmc.sav`.

### Tras actualizar: la carpeta `assets/`

El primer arranque extrae los assets del juego desde tu ROM a
`/switch/tmc/assets/`. **Actualizar el NRO no refresca esa carpeta.** Si tras
una actualización ves gráficos o textos raros, borra `assets/` y vuelve a
lanzar el juego — se regenera en un minuto. Las partidas no se tocan.

## Actualizar

En la segunda pantalla ve a **AJUSTES → SISTEMA** y usa la fila de
actualización:

1. **CHECK FOR UPDATES** — consulta el manifiesto de actualización del proyecto.
2. **DOWNLOAD UPDATE** — descarga la nueva versión y la verifica.
3. **INSTALL UPDATE** — hace copia de seguridad de tu versión actual e instala.
4. **Cierra el juego y vuelve a abrirlo** para ejecutar la versión nueva.

La consola necesita conexión a internet sólo para los pasos 1 y 2.

### Qué toca y qué no

El actualizador reemplaza el programa del juego
(`tmc_aleks_ultimate_nx.nro`) y nada más. Tu ROM, tus partidas, el
autoguardado, los assets extraídos y `config.json` no los lee ni los escribe.

Antes de reemplazar el juego instalado, escribe una copia verificada del mismo
en `/switch/tmc/tmc_aleks_ultimate_nx.bak`. Si la instalación falla en
cualquier punto, esa copia se restaura automáticamente. Si alguna vez quieres
volver atrás a mano, copia el `.bak` sobre el `.nro`.

### Por qué está hecho así

Los metadatos de actualización vienen de un manifiesto controlado por el
proyecto con un esquema cerrado: una clave desconocida, una clave duplicada,
una URL que no sea HTTPS, un canal incorrecto o un campo malformado hacen que
se rechace el manifiesto entero en lugar de ignorarse. El tamaño y el SHA-256
esperados de la nueva versión se fijan en ese manifiesto antes de descargar
nada, y el archivo descargado se verifica contra ellos antes de acercarse
siquiera a tu juego instalado. Las descargas son sólo HTTPS, las redirecciones
sólo HTTPS, y se verifican los certificados.

Si prefieres no usarlo, puedes seguir actualizando a mano: descarga el NRO de
la [página de releases](../../releases) y cópialo tú sobre
`/switch/tmc/tmc_aleks_ultimate_nx.nro`.

## Compatibilidad de ROM

Las versiones publicadas están construidas para la edición **USA** (cabecera
`BZME`). El volcado USA de referencia conocido tiene el SHA-1
`b4bd50e4131b027c334547b4524e2dbbd4227130` — el port no verifica este hash en
tiempo de ejecución (identifica el juego por la cabecera), pero ese es el
volcado con el que se compila y prueba cada versión. El cargador reconoce
cabeceras EU (`BZMP`), pero no es una base soportada; usa una ROM USA.

## Rendimiento

NORMAL (incluida la pantalla ancha) funciona a 60 FPS con la CPU a frecuencia
de serie desde la v1.4.0, cuando la PPU por software pasó de renderizar por
píxel a hacerlo por tile. DUAL y FLIP son más exigentes que NORMAL y también
mejoraron; en el hardware probado, 1224 MHz de CPU sigue dando en ellos los
60 FPS más estables. Es opcional y no garantiza 60 FPS fijos en todas las
escenas. El port nunca cambia la
frecuencia por su cuenta; la integración automática con gestores de reloj sigue
en evaluación. Detalles en [docs/PERFORMANCE.md](docs/PERFORMANCE.md).

## Configuración

Todos los ajustes de AJUSTES (JUEGO, CONTROLES, PANTALLA, LOGROS, SISTEMA)
están documentados en [docs/CONFIGURATION_ES.md](docs/CONFIGURATION_ES.md).

## Transparencia de desarrollo

Este proyecto se desarrolló con una asistencia significativa de IA — incluidas
Claude / Claude Code, ChatGPT y OpenAI Codex — para investigación de código,
depuración, implementación, revisión de código, documentación y preparación de
la publicación. La dirección del proyecto, las decisiones de características,
las pruebas en hardware real, la validación visual y todas las decisiones de
publicación las tomó manualmente el mantenedor. Esto describe cómo se
construyó *esta edición*; los proyectos upstream sobre los que se apoya tienen
sus propias historias de desarrollo, y aquí no se hace ninguna suposición
sobre las herramientas o los flujos de trabajo que hayan usado sus
mantenedores. La descripción completa está en
[docs/DEVELOPMENT_TRANSPARENCY.md](docs/DEVELOPMENT_TRANSPARENCY.md).

## Expectativas de mantenimiento

Este es un proyecto personal. Mi intención es seguir mejorándolo hasta que
alcance un estado que considere totalmente jugable y estable para el uso que
le quiero dar. Después de eso, las actualizaciones pueden volverse poco
frecuentes o detenerse. No prometo soporte a largo plazo, ni una hoja de ruta,
ni respuestas rápidas a los issues. Si quieres llevarlo más lejos, los forks
son bienvenidos bajo las licencias upstream aplicables — consulta
[CONTRIBUTING.md](CONTRIBUTING.md).

## Reportar un fallo

Las versiones publicadas guardan diagnósticos en la tarjeta, para que un
reporte pueda llevar pruebas. Adjunta lo que exista de esta lista, junto con la
versión que aparece en **AJUSTES → SISTEMA → BUILD INFO**:

| Archivo | Qué es |
|---|---|
| `/switch/tmc/tmc.log` | Registro de arranque: identidad del build, carga de assets, resolución de la ROM, errores. Empieza con una línea `[build]` con la versión exacta — inclúyelo siempre. |
| `/switch/tmc/startup.log` | Tiempos de arranque por fase. Útil si el juego tarda mucho o se cuelga al arrancar. |
| `/switch/tmc/crashlogs/` | Solo se escribe si el juego ha crasheado — **es el archivo que importa en un crash**; `tmc.log` solo casi nunca basta. |
| `/switch/tmc/ra.log` | Actividad de RetroAchievements, para reportes de logros. |
| `/atmosphere/crash_reports/` | El informe de crash del propio sistema, del mismo momento. |

Una captura o un vídeo corto ayuda muchísimo en cualquier fallo visual, y tu
`tmc.sav` permite reproducir el problema exactamente. Di también si borraste
`assets/` después de actualizar (ver arriba): una carpeta de assets vieja
explica muchos reportes de "gráficos raros".

## Problemas conocidos

Consulta [docs/KNOWN_ISSUES_ES.md](docs/KNOWN_ISSUES_ES.md). En escenas
exigentes DUAL y FLIP pueden bajar de 60 FPS.

## Créditos y linaje

Esta edición existe gracias a los proyectos siguientes. La versión corta:

- [zeldaret/tmc](https://github.com/zeldaret/tmc) — la descompilación de
  Minish Cap sobre la que se construye todo.
- [Project Picori (999sian/tmc)](https://github.com/999sian/tmc) — la base del
  port nativo para PC. Sus correcciones de motor se portan a esta edición con
  regularidad (hasta su v0.9.3 en la v1.3.9), y su trabajo de widescreen es la
  base de la próxima v1.4.
- [HayatoG/tmc](https://github.com/hayatog/tmc) — el port original para
  Nintendo Switch y la base directa de la que parte este árbol.
- [samyost1/tmc-android](https://github.com/samyost1/tmc-android) — el
  concepto e implementación de doble pantalla que este trabajo extiende.
- [EstebanPdN/zelda-tmc-3ds](https://github.com/EstebanPdN/zelda-tmc-3ds) —
  la adaptación a doble pantalla para 3DS; varias de sus correcciones de motor
  están portadas aquí.

Gracias a todos los que prueban en hardware real y reportan en GBAtemp y
GitHub — la mayoría de los arreglos de la 1.3.x empezaron como un reporte
vuestro.

Roles completos, bibliotecas de terceros y detalles de licencias:
[CREDITS.md](CREDITS.md) · [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## Legal

Este es un proyecto de fans no oficial, sin afiliación, respaldo ni soporte de
Nintendo. *The Legend of Zelda* y *The Minish Cap* son marcas de Nintendo. Con
este proyecto no se distribuyen recursos originales del juego, ROMs ni ningún
contenido de Nintendo con derechos de autor.
