# Datos: procedencia, publicación, unidades y privacidad

## Procedencia

| Dato | De dónde sale | Quién lo produjo |
|---|---|---|
| Videos | Celular, 9, 15 y 16 de octubre de 2026 ([`METODO.md`](METODO.md#1-captura)) | Raúl Alejandro González |
| Cuadros | Extraídos de los videos con ffmpeg y elegidos por nitidez | Scripts de la cadena |
| Poses y nube dispersa | COLMAP (pycolmap 4.2.1) | Scripts de la cadena |
| Splats (`.sog`) | Brush 0.3.0 sobre cuadros con las caras difuminadas, comprimidos con splat-transform 3.6.4 | Scripts de la cadena |
| Nube densa, malla, láminas, plantillas | OpenMVS 2.4.0 y scripts propios | Scripts de la cadena |
| Paramétrico (`modelo.glb`) | Parámetros de la nube, generado en Blender 5.2.1 | Scripts de la cadena |
| Maqueta exterior del 106 | Modelo hecho con fotos del exterior, con escala de ±12 % declarada; es la del visor del repo `edificio-106` (`docs/HISTORIA.md`) | Repo `edificio-106` |
| Volumen único (`volumen.glb`) | Los tres paramétricos encajados en la maqueta | Scripts de la cadena |
| Plano esquemático del edificio (2022–2023) | Archivo DWG facilitado por LM | No se publica; ver abajo |
| Ubicación de la escalera al ático, del baño y sentido del tercer piso | Datos del sitio que dio el equipo, sin medir (galería del edificio, sección 5) | Equipo del proyecto |

«Scripts de la cadena» quiere decir código escrito y corrido por Claude bajo la dirección de LM ([`README.md`](../README.md#autoría-y-propósito)).

## Qué se publica y qué no

| Material | ¿Publicado? | Por qué |
|---|---|---|
| Videos originales | No | Tienen personas sin difuminar y no son necesarios para ver los resultados |
| Cuadros sin difuminar | No | Caras de personas |
| Cuadros difuminados, poses, nubes densas y limpias | No, por ahora | Pesan varios GB; publicarlos en un repositorio de datos con DOI es PENDIENTE DE LM ([`AGENDA.md`](AGENDA.md)) |
| Splats para la web, láminas, plantillas, paramétricos, volumen, imágenes de la galería | Sí | Son el resultado; las imágenes y los splats de los recorridos del edificio salen de cuadros difuminados |
| Plano esquemático del edificio | No | Es material que facilitó LM para el análisis; no sale de la carpeta de trabajo sin su permiso. Se cita solo por las medidas que se derivaron de él |
| Fotos del exterior y croquis usados para cruzar fuentes | No | Material privado del proyecto |
| Scripts de las visitas del 15 y 16 de octubre | No, por ahora | PENDIENTE DE LM ([`METODO.md`](METODO.md)) |
| Registros de trabajo (`NOCHE.md`, `LECCIONES.md`, diagnósticos, paneles) | No, por ahora | Llevan rutas de la máquina y datos de contacto; habría que limpiarlos antes. PENDIENTE DE LM |

## Unidades y escala

**La unidad «u».** COLMAP reconstruye sin escala: cada modelo tiene una unidad arbitraria, la u, y un modelo nuevo del mismo piso tiene otra u y otro marco. Por eso ninguna cifra en u se compara entre pisos ni entre versiones de un mismo piso sin una similitud medida entre los dos (`LECCIONES.md`, punto 11).

**Nada está en metros.** No hay ninguna medida tomada en el edificio. Las plantillas de medición ([`edificio/X/06-medicion/`](../edificio/index.html)) están listas para tomarlas con flexómetro: por piso, el alto y el ancho de una puerta para la escala, y dos baldosas y un peldaño para el control.

**Escala supuesta de la maqueta.** El volumen único y la vista de los tres pisos juntos del recorrido en el tiempo usan la escala de la maqueta exterior del 106, que declara un margen de ±12 % (`edificio-106`, `docs/HISTORIA.md`). Esa escala no es una medida. Las escalas del volumen son 3,526, 2,937 y 1,928 m de maqueta por u en A, B y C (`volumen/encaje.json`). En el recorrido en el tiempo, C va en otro marco con otra u, y su factor es 3,682 m de maqueta por u (`edificio/recorrido-tiempo/C/datos.json`).

**La maqueta es más ancha de lo que declara.** Según el plano esquemático del edificio (2022–2023), facilitado por LM, el exterior mide 43,30 × 18,90 m cara a cara de muro, contra 45,5 × 23,0 m de la maqueta. A lo ancho, el plano es un **17,8 % menor** que la maqueta (factor 0,822); dicho al revés, la maqueta es un 21,7 % más ancha que el plano, fuera del ±12 % declarado. A lo largo la diferencia es de 4,8 % (factor 0,952). Entre caras interiores de fachada, 18,40 m contra 22,40 m, el factor es k = 0,821, y como las escalas del volumen salieron del ancho entre fachadas, ese es el que corrige el volumen: 2,896, 2,412 y 1,583 m por u (`plano-dwg/INFORME-DWG.md`, registro de trabajo).

Tres cosas para leer bien ese resultado:

- El plano es esquemático: trae retícula, columnas, muros, tabiques, puertas y escaleras, pero no ventanas, cortes ni alturas. **No da el piso a piso.** Cruzado con la proporción de fotos del exterior, da unos 3,70 m, con un margen de ±4 a 7 % por foto (inferencia, mismo informe).
- La proporción largo/ancho del plano (2,29) coincide con la de las fotos del exterior (2,29) y con el ritmo de ménsulas de las fachadas, y la huella de OpenStreetMap menos un alero de 1,35 a 1,85 m da casi lo mismo. Son fuentes independientes entre sí.
- **El factor k = 0,821 no se ha aplicado a nada publicado.** La galería sigue diciendo «escala supuesta, ±12 %». Corregir la maqueta del visor del 106 es un cambio de fondo en otro repositorio y lo decide LM.

Con ese factor, la cámara de los videos queda a 1,45 a 1,53 m del piso y la escalera sube de 3,43 a 3,67 m por piso, cifras cercanas a la altura de cámara de 1,5 m supuesta y al piso a piso de 3,65 m de la maqueta, de las que antes se alejaban (1,77 a 1,86 m y 4,17 a 4,47 m de maqueta; `volumen/INFORME.md`, sección 3). Sigue siendo una escala derivada, no medida: la única forma de cerrarla es con cinta.

**Orientación.** El norte no se conoce en ningún piso.

## Privacidad

- **Caras.** Toda cara se difumina antes de entrenar un splat y antes de publicar una imagen ([`METODO.md`](METODO.md#3-difuminado-de-caras)). Los afiches y dibujos no se difuminan, por decisión de LM.
- **Excepción conocida en lo publicado: el splat del piloto.** El splat del 9 de octubre (`visor/piloto_movil.sog`, que usan [`visor/`](../visor/index.html) y [`05-recorrido/`](../05-recorrido/index.html)) se entrenó antes de que existiera la regla de difuminar, y en él quedó una persona sentada al fondo del pasillo (`PILOTO-INTERIOR.md`, sección 8). La galería del piloto lo advierte ([`index.html`](../index.html)); las páginas que cargan el splat (`visor/` y `05-recorrido/`) no. Retirarlo o rehacerlo con la cara difuminada es PENDIENTE DE LM, y es lo primero que habría que resolver antes de difundir este repositorio entre pares.
- **El splat publicado de la planta baja** es de la primera visita y pasó la revisión de caras de esa fecha (detector y marcas a mano). El control más estricto en teselas del 17 y 18 de octubre se aplicó a B, a C y a la versión nueva de A, que no se publicó. No consta en los registros que se haya repetido sobre el splat publicado de A. PENDIENTE DE LM.
- **Miniaturas del recorrido en el tiempo:** solo de cuadros difuminados y sin personas marcadas ni detectadas, revisadas a ojo (`edificio/recorrido-tiempo/X/datos.json`, campo `privacidad`).
- **Datos personales.** Ningún documento de este repositorio lleva correos ni rutas de la máquina. Raúl aparece nombrado por su crédito.
- **El edificio** es un espacio privado. Mostrar su interior en algo comercial pediría permiso del dueño o administrador (`LICENCIAS-PILOTO.md`, documento de trabajo).
- **Obras de terceros.** Los murales, dibujos, carteles y letreros que aparecen en las imágenes son obras de otros autores. Reproducirlas fuera del uso académico puede necesitar permiso.
- Siete de las diez páginas llevan `noindex, nofollow`; les falta a los tres recorridos de `edificio/X/05-recorrido/`. Algunas todavía dicen «uso interno»: se escribieron para el equipo antes de decidir abrir el repositorio a pares. Quitar o mantener esos avisos es PENDIENTE DE LM.

## Licencia

PENDIENTE DE LM. Hoy el repositorio no tiene archivo de licencia. La propuesta es copiar el esquema del repo `edificio-106` (su `LICENSE`, vigente desde el 6 de octubre de 2026):

1. **Qué cubre:** el código, los modelos, los textos, las imágenes y la selección de datos del repositorio. No cubre los hechos ni los datos en sí, ni las partes de terceros.
2. **Sin pedir permiso:** ver los archivos y hacer fork dentro de GitHub; copias locales para preparar un aporte; usar la galería publicada en clase, en investigación y para uso personal; citar y mostrar imágenes con la cita.
3. **Con permiso escrito:** publicar otra versión fuera de GitHub o activar Pages en un fork; usar copias locales para dar clase; integrarla en otro producto; cualquier uso comercial; presentarla como propia o como oficial de Isthmus o de Ciudad del Saber.
4. **Aportes:** quien aporta conserva sus derechos y concede al autor una licencia no exclusiva para usar el aporte dentro de la obra; aportar no puede ser condición para aprobar un curso.
5. **Sin garantía:** proyecto académico experimental; sus cifras no sustituyen una medición en sitio.

Una decisión que el esquema del 106 no resuelve y que importa aquí: si los datos se publican con DOI para que otros grupos los reutilicen (cuadros difuminados, poses, nubes), una licencia abierta para esos datos, separada de la del código y la galería, facilitaría la reutilización académica. También la decide LM.

Licencias de las herramientas: `LICENCIAS-PILOTO.md`, documento de trabajo sin publicar. Ninguna impide el uso de lo que se generó con ellas; OpenMVS es AGPL-3.0, lo que pesa solo si se distribuye el programa, o si se ofrece como servicio una versión modificada.
