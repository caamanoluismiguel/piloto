# Piloto interior del Edificio 106

Reconstrucción del interior de un edificio de tres pisos a partir de videos de celular, procesada en un computador personal de escritorio y publicada como galería web. El edificio es el 106 de Ciudad del Saber (Panamá), donde funciona Isthmus. No es un proyecto oficial de Isthmus ni de Ciudad del Saber.

Galería en línea: https://caamanoluismiguel.github.io/piloto/

## Autoría y propósito

El proyecto es idea y dirección de **Luis Miguel Caamaño** (LM). Nace de su iniciativa de explorar el uso de la inteligencia artificial para crear herramientas pedagógicas sobre el edificio: qué se puede construir con asistentes de IA, qué tan confiable sale y cómo se le muestra a estudiantes y docentes la diferencia entre lo que se midió y lo que se supuso. Este repositorio es la parte interior de esa exploración; la exterior es el visor de sol y clima del repo [`edificio-106`](https://github.com/caamanoluismiguel/edificio-106).

**Raúl Alejandro González** grabó todos los videos: el piloto del 9 de octubre de 2026 y las dos visitas del 15 y del 16 de octubre.

**Trabajo con asistentes de IA.** Casi todo el procesamiento lo hicieron asistentes de IA bajo la dirección de LM, y conviene decirlo con precisión:

- **Claude** (Anthropic), en Claude Code, escribió y corrió los scripts de la cadena (extracción de cuadros, COLMAP, difuminado de caras, Brush, OpenMVS, limpieza, láminas, modelo paramétrico, volumen y visores), hizo los análisis y los diagnósticos, redactó los textos de la galería y estos documentos, y organizó la revisión con agentes: un investigador que revisa fuentes primarias, paneles de expertos **simulados** (no son personas reales) y un agente `verificador` que recalcula las cifras antes de publicar.
- **Codex** (OpenAI) redactó la guía de grabación del 8 de octubre de 2026, entregada a LM antes del piloto. Esa guía (`GUIA-GRABACION-ISTHMUS.md`) es, por ahora, un documento de trabajo sin publicar.
- **LM decidió**: hacer el proyecto y con qué alcance; pedirle a Raúl las grabaciones; las reglas de privacidad (difuminar caras; los afiches y dibujos no se difuminan); usar flexómetro en vez de láser; armar un volumen único con los tres pisos dentro de la maqueta exterior; la regla de no reemplazar un piso publicado por una versión peor; y qué se publica, unas veces en persona y otras delegando en el equipo de agentes con sus reglas, como hizo el 16 de octubre (`NOCHE.md`). También fue LM quien detectó, mirando las láminas, las copias fantasma del segundo y del tercer piso que un agente verificador, que comprobaba contra su propio registro, no había visto (registro de trabajo `LECCIONES.md`, puntos 12 y 20).

Los asistentes se equivocaron varias veces y algunos de esos errores llegaron a LM antes de corregirse. Están en [`docs/LIMITES-Y-ERRORES.md`](docs/LIMITES-Y-ERRORES.md).

## Pregunta de investigación

¿Qué parte del interior de un edificio de varios pisos se puede documentar solo con video de celular, procesado con herramientas gratuitas en un computador personal sin GPU contratada, y con qué grado de confianza, si cada producto separa lo que sale de los datos de lo que se supone?

El piloto no responde la pregunta en general: es un solo edificio, grabado por una sola persona, sin ninguna medida en sitio. Sirve para saber dónde falla la cadena, qué hay que medir y qué protocolo de captura conviene probar en más edificios ([`docs/AGENDA.md`](docs/AGENDA.md)).

## Qué contiene

Hay dos galerías. La de la raíz es el piloto del 9 de octubre (un video de 79 s de la planta baja). La de `edificio/` son los tres pisos grabados el 15 y el 16 de octubre. Las letras A, B y C son la planta baja, el segundo y el tercer piso.

Todas las cifras de las reconstrucciones están en **unidades de reconstrucción (u)**: cada modelo tiene la suya y no se comparan entre pisos. Ninguna está en metros, porque no hay medidas tomadas en el edificio ([`docs/DATOS.md`](docs/DATOS.md)).

| Parte | Enlace | Qué es | Estado |
|---|---|---|---|
| Galería del piloto | [`index.html`](index.html) | Lo que salió del video de 79 s: splats, nube, malla, planta, cortes, paramétrico y recorrido | Reconstruido en u; sin escala |
| Galería del edificio | [`edificio/index.html`](edificio/index.html) | Los tres pisos, con todas las secciones de abajo y lo que falta | Reconstruido en u; sin escala |
| Recorrido de splats | [`edificio/A/05-recorrido/`](edificio/A/05-recorrido/index.html), [`B`](edificio/B/05-recorrido/index.html), [`C`](edificio/C/05-recorrido/index.html), y el del piloto en [`05-recorrido/`](05-recorrido/index.html) | Splat nivelado con el piso, puntos de navegación sobre el piso, minimapa sin escala | Para ver, no para medir. A es de la primera visita; B y C juntan las dos visitas |
| Láminas | `edificio/X/03-laminas/E6_lamina_A3.pdf` y [`03-laminas/`](03-laminas/E6_lamina_A3.pdf) | Planta y dos cortes en A3, con el dato (puntos grises) separado del ajuste (líneas naranjas) y línea discontinua donde falta información | Dato reconstruido; sin cotas. Las escaleras que no pasan la regla van rotuladas como dudosas |
| Plantilla de medidas | `edificio/X/03-laminas/E7_plantilla.csv` | Distancias entre planos que se podrían medir en sitio, con su incertidumbre en u | Propuesta; ninguna se ha medido |
| Planta de medición | `edificio/X/06-medicion/` | Planta, fichas con foto y tabla para medir con flexómetro | Lista para usar; sin medidas tomadas |
| Paramétrico | `edificio/X/04-parametrico/modelo.glb` y [`04-parametrico/`](04-parametrico/modelo.glb) | Modelo de volúmenes generado en Blender a partir de parámetros de la nube; cada pieza dice si sale de la nube o es supuesta | Medido en la nube (en u) o supuesto: 36, 53 y 48 % de las superficies vistas son supuestas en A, B y C ([`edificio/index.html`](edificio/index.html), sección 5) |
| Volumen único | [`edificio/volumen/volumen.glb`](edificio/volumen/volumen.glb), visible en la sección 5 de la galería | Los tres paramétricos dentro de la maqueta exterior del 106 | **Borrador interpretado.** Escala supuesta tomada de la maqueta, ubicación con supuestos; no sirve para medir |
| Recorrido en el tiempo | [`edificio/recorrido-tiempo/`](edificio/recorrido-tiempo/index.html) | Por dónde pasó el celular durante la grabación, con una pirámide y una miniatura por cuadro clave | Visualización de las poses; no sirve para medir |
| Ideas | [`edificio/ideas.html`](edificio/ideas.html) | Usos propuestos por un panel simulado y por el investigador, con su estado | Propuestas |
| Guía de fotos | [`edificio/guia-fotos.html`](edificio/guia-fotos.html) | Guía de fotos fijas preparada para la segunda visita; no consta que se tomaran fotos | Documento de campo |

Lo que falta confirmar de memoria está en la página de [preguntas para LM](edificio/preguntas/index.html): dos preguntas abiertas de la planta baja y tres confirmaciones rápidas del tercer piso, con las que ya se contestaron.

### Qué quiere decir cada estado

- **Medido en la reconstrucción:** sale de la nube de puntos o de las poses de cámara. Está en u, no en metros.
- **Medido en sitio:** con cinta o distanciómetro en el edificio. Hoy no hay ninguna medida de este tipo.
- **Inferido:** deducido con una regla a partir de los datos o de una revisión visual de las fotos; por ejemplo, las puertas de vidrio del vestíbulo, que pasaron a supuestos por revisión visual y falta confirmarlo en sitio.
- **Supuesto:** relleno sin dato (espesores, techos no vistos, huecos), dibujado rayado o en blanco translúcido.

## Qué se puede hacer con esto

**Docencia.** El material vale más por sus errores que por sus aciertos. Algunos ejercicios que ya se pueden hacer sin nada nuevo: calcar la lámina separando con dos colores lo observado de lo supuesto; superponer el paramétrico a cinco fotos y marcar dónde no coincide; dibujar de memoria la planta de un piso y compararla con la lámina; nombrar los puntos del recorrido (hoy se llaman «Punto 1», «Punto 2»). Con una sesión en sitio, los estudiantes pueden tomar las medidas de la planta de medición y darle al edificio la escala que le falta, entregando el error de las medidas de control. La lista completa, con quién la propuso y qué pide, está en [`edificio/ideas.html`](edificio/ideas.html).

**Investigación.** El piloto es un caso documentado con sus fallas medidas: giros sin desplazamiento que dejan poca triangulación, estructuras repetidas que crean copias fantasma, vidrio y paredes lisas que no reconstruyen, y la distancia entre lo que un splat muestra y lo que una nube permite medir. La línea «documentar edificios solo con celular», con el estado del arte y lo que haría falta medir, está en [`docs/AGENDA.md`](docs/AGENDA.md). El método completo para repetir la cadena está en [`docs/METODO.md`](docs/METODO.md).

**El visor del 106.** Los recorridos y el paramétrico podrían entrar al visor de sol y clima como capa interior cuando haya escala medida y los pisos estén unidos. Hasta entonces, cualquier cifra en metros sería una suposición.

## Cómo citar

Propuesta, PENDIENTE DE LM (no hay DOI todavía):

> Caamaño, Luis Miguel (dir.). 2026. *Piloto interior del Edificio 106: reconstrucción con video de celular*. Grabación de Raúl Alejandro González. Procesamiento con Claude (Anthropic); guía de grabación con Codex (OpenAI). https://github.com/caamanoluismiguel/piloto

Si citas una cifra, cita también el archivo de donde sale y la fecha de la versión (el hash del commit), porque las cifras cambian cuando se rehace un piso.

## Cómo contribuir

Lee [`CONTRIBUIR.md`](CONTRIBUIR.md). En corto: nada con caras sin difuminar, cada cifra con su fuente, y lo medido separado de lo supuesto.

## Licencia

PENDIENTE DE LM. Este repositorio **no tiene licencia todavía**, así que por defecto rigen todos los derechos de autor. La propuesta es usar el mismo esquema del repo `edificio-106`: «todos los derechos reservados» con permisos explícitos para ver, hacer fork en GitHub, usar la galería publicada en clase y en investigación, y citar y mostrar imágenes con la cita; publicar otra versión, usar copias locales para dar clase o cualquier uso comercial pedirían permiso escrito. Los hechos y los datos en sí no quedan reservados, y las herramientas de terceros conservan su licencia. Detalle y opciones en [`docs/DATOS.md`](docs/DATOS.md#licencia).

Hay dos cosas que ninguna licencia de este repositorio puede cubrir: el edificio es un espacio privado, y los murales, dibujos y carteles que aparecen en las imágenes son obras de otros autores ([`docs/DATOS.md`](docs/DATOS.md)).

## Documentos

- [`docs/METODO.md`](docs/METODO.md): la cadena completa, paso por paso, con versiones, parámetros y métricas.
- [`docs/DATOS.md`](docs/DATOS.md): de dónde sale cada dato, qué se publica y qué no, unidades y escala, privacidad y licencia.
- [`docs/LIMITES-Y-ERRORES.md`](docs/LIMITES-Y-ERRORES.md): lo que no se puede afirmar y los errores encontrados, también los nuestros.
- [`docs/AGENDA.md`](docs/AGENDA.md): preguntas abiertas para pares y la línea de investigación.
- [`CONTRIBUIR.md`](CONTRIBUIR.md): cómo proponer cambios.
