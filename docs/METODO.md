# Método: del video de celular a la galería

Este documento describe la cadena completa, paso por paso, para que alguien la pueda repetir con otro edificio o revisar la nuestra. Para cada paso doy las herramientas con su versión, los parámetros que importan, los scripts y la métrica con que se juzgó el resultado.

**Dónde están los scripts y las fuentes.** Ninguno está publicado todavía. Los del piloto del 9 de octubre (`elegir.py`, `sfm.py`, `openmvs.sh`, `poses.py`, `psnr.py` y el visor de prueba) y sus informes (`PILOTO-INTERIOR.md`, `EXPERIMENTOS-PILOTO.md`, `LICENCIAS-PILOTO.md` y la guía `GUIA-GRABACION-ISTHMUS.md`) están en la copia de trabajo del repo `edificio-106`, sin comitear. Los de las visitas del 15 y del 16 de octubre están en la carpeta de trabajo del proyecto, junto con los registros que cito como fuente (`NOCHE.md`, `LECCIONES.md`, `DIAGNOSTICO.md` y otros). Todo lo que cito por nombre de archivo sin enlace es uno de esos documentos de trabajo. Publicarlos es PENDIENTE DE LM; mientras tanto, los nombro aquí para que se sepa qué hace cada uno. Ningún script lleva caras ni datos personales, pero sí rutas de la máquina que habría que limpiar antes.

**Equipo.** Un Apple M4 de 10 núcleos con 16 GB de memoria, sin GPU contratada (`PILOTO-INTERIOR.md`). Brush usa la GPU integrada por Metal; todo lo demás corre en CPU. Las herramientas son gratuitas; sus licencias están revisadas en `LICENCIAS-PILOTO.md`.

**Unidades.** Todas las cifras de reconstrucción están en u, la unidad propia de cada modelo de COLMAP. Ver [`DATOS.md`](DATOS.md#unidades-y-escala).

## Versiones

| Herramienta | Versión | Para qué |
|---|---|---|
| ffmpeg | 8.0.1 en la máquina hoy (la del 9 de octubre no quedó anotada) | extraer cuadros |
| pycolmap (COLMAP para Python) | 4.2.1 | poses de cámara y nube dispersa |
| Brush | 0.3.0, binario oficial para Mac | entrenar los splats |
| splat-transform (PlayCanvas) | 3.6.4 | comprimir a SOG |
| OpenMVS | 2.4.0, compilado en CPU | nube densa, malla y textura |
| Blender | 5.2.1 | modelo paramétrico y volumen |
| three.js | 0.186.1 | visores web |
| Spark | 2.2.0 | dibujar splats en la web |
| Playwright | sin versión anotada | pruebas como computador y como teléfono |
| Apple Vision (por un script en Swift) | la del sistema | detector de caras y de personas |

Las versiones salen de `LICENCIAS-PILOTO.md` y `PILOTO-INTERIOR.md` y de las páginas publicadas, que cargan three 0.186.1 y Spark 2.2.0 desde jsDelivr.

## 1. Captura

### Material

| Grabación | Fecha | Clips | Duración total | Formato |
|---|---|---|---|---|
| Piloto | 9 oct 2026 | 2 (se usó el de 78,6 s) | 127,7 s | HEVC 1920 × 1080, 60 fps |
| Primera visita | 15 oct 2026 | 7: A 2, B 3, C 2 | unos 19,7 min | HEVC 1920 × 1080, vertical salvo la primera toma |
| Segunda visita | 16 oct 2026 | 13: A 6, B 4, C 3 | unos 38,5 min | HEVC 1920 × 1080, horizontal |

Duraciones medidas con `ffprobe` sobre los videos originales; los del piloto, en `PILOTO-INTERIOR.md`. Los grabó Raúl Alejandro González con un celular, con su aplicación de cámara normal y sin LiDAR; el del piloto era un Samsung (experimento E2), y la marca de las visitas no consta en los metadatos. Los videos originales no se publican ([`DATOS.md`](DATOS.md)).

### Protocolo y cómo cambió

1. **Guía del 8 de octubre**, antes del piloto (`GUIA-GRABACION-ISTHMUS.md`, redactada con Codex): cámara principal a 1×, horizontal, misma lente en cada toma, sin zoom. Ya advertía que girar sobre el propio eje desde un solo punto no basta.
2. **Lista para la primera visita**, escrita después del piloto (sección 7 de la galería raíz, [`index.html`](../index.html)): lente fijo y anotado, cada tramo de ida y de regreso, cruzar cada puerta en los dos sentidos, mirar piso y cielo raso, no acercarse a paredes blancas ni a vidrios, la escalera completa con parada en el descanso, sin gente.
3. **Lista para la segunda visita** (galería del edificio, sección 8, [`edificio/index.html`](../edificio/index.html)): 14 puntos donde los modelos salían mal, teléfono en horizontal, gran angular 0,6×, una vuelta de 360° en 30 s, caminar a medio paso y **«nunca camines y gires a la vez: primero caminas, te detienes, y después giras»**. Se sumó una guía de fotos fijas ([`edificio/guia-fotos.html`](../edificio/guia-fotos.html)).

### Errores aprendidos en la captura

- **Giros rápidos sin desplazamiento.** En la primera visita, junto a los cortes débiles la cámara gira de 26 a 106° en 6 cuadros y se mueve de 0,07 a 0,33 u (registro `LECCIONES.md`, punto 1.2). Sin desplazamiento no hay paralaje, y COLMAP pierde la cadena o la cierra mal.
- **La lista de la segunda visita pidió lo contrario de la práctica recomendada.** Pedir vueltas de pie y prohibir caminar y girar a la vez evitó los giros rápidos, pero cambió giros rápidos sin paralaje por giros lentos sin paralaje. Registraron bien (A y B quedaron en un solo modelo), pero triangulan menos: los modelos de la segunda visita tienen 10,3 % de puntos con ángulo de triangulación menor de 2° en A y 10,1 % en C, contra 3,8 y 4,4 % en la primera (`DIAGNOSTICO.md`, sección 2). Es un error nuestro; está en [`LIMITES-Y-ERRORES.md`](LIMITES-Y-ERRORES.md).
- **Lentes distintos sin aviso.** Dentro de una misma visita hubo clips de unos 91°, 70° y 61° de campo horizontal (`DIAGNOSTICO.md`, sección 3). Por eso cada clip lleva su propia cámara en COLMAP y su propio campo de visión en el visor.
- **Paredes blancas y vidrio.** Un tramo pegado a una pared lisa o a un vidrio partió el modelo en el piloto, y lo partió distinto en dos corridas iguales (experimento E4, `EXPERIMENTOS-PILOTO.md`).
- **Sin medidas con cinta.** Ninguna de las dos visitas trajo una medida en sitio. Todo sigue en u.
- **La escalera entre pisos no se grabó como un clip continuo**, y por eso los pisos no se pueden unir con imágenes comunes.
- **Gente en el edificio.** Se pidió grabar sin gente, pero el edificio estaba en uso. Ver la sección 3.

Lo que recomienda hoy el panel que evaluó el informe externo para una próxima visita (`EVAL-INFORME-EXTERNO/PANEL.md`): caminar en arco en las esquinas en vez de girar parado, exposición y balance de blancos bloqueados, una sola lente sin estabilización electrónica agresiva, la escalera en un clip continuo que empiece y termine de 3 a 5 m dentro de cada piso, fotos fijas en las zonas críticas, horario sin gente y, sobre todo, medidas con cinta: de control y de chequeo, separadas.

## 2. Extracción y selección de cuadros

- `ffmpeg -i <video> -vf fps=15 -q:v 2` saca 15 cuadros por segundo.
- `elegir.py <crudos> <destino> <VENTANA>` se queda con el cuadro más nítido de cada grupo de VENTANA cuadros, según la varianza del laplaciano.
- VENTANA 5 da 3 cuadros por segundo (piloto, A y B de la primera visita, B y C de la segunda); VENTANA 3 da 5 por segundo (C de la primera visita); VENTANA 7, unos 2,1 por segundo (clips nuevos de A). Fuente: `EVAL-INFORME-EXTERNO/INVENTARIO.md`, ítem A2.
- Cada clip lleva una letra (`a_`, `b_`, `c_`…) y los cuadros se ordenan **por nombre**, nunca por el identificador interno de COLMAP. Ordenar por identificador hizo que el recorrido del piloto midiera 37,99 u en vez de 19,24 (`LECCIONES.md`, punto 22).

**Métrica.** Nitidez mediana de los elegidos frente a todos (piloto: 298 contra 277) y número de cuadros por clip.

**Aprendido.** Densificar a 15 cuadros por segundo en los cortes débiles de C no unió la cadena (`LECCIONES.md`, punto 1.2): más cuadros no crean paralaje. Con giros rápidos, pasar de 3 a 5 cuadros por segundo sí ayudó a registrar en C (442 de 1.325 contra 1.456 de 2.208; `LECCIONES.md`, punto 1.9).

## 3. Difuminado de caras

Regla de LM: toda cara se difumina antes de entrenar un splat o de publicar una imagen. Los afiches y dibujos no se difuminan.

- **Detector:** Apple Vision, llamado desde un script en Swift (`caras`), sobre cada cuadro.
- **Marcas a mano:** las personas que el detector no ve (pequeñas, de espaldas, detrás de un vidrio) se marcan a mano en un JSON por piso.
- **Difuminado** (`difuminar_cuadros.py`): pixelado más desenfoque gaussiano dentro de una elipse de radio horizontal r y vertical 1,25 r, con borde suave. En la segunda visita se aplica sobre los cuadros **originales, antes del undistort**, para que las marcas no dependan de la cámara que estime COLMAP. En la primera visita se aplicaba después del undistort.
- **Control fino** (desde el 17 de octubre): el detector se corre otra vez sobre los cuadros ya difuminados, en teselas ampliadas al doble, sobre **todos** los cuadros, y cada cuadro con personas se revisa a tamaño completo contando cabezas una por una. Un control positivo comprueba que el mismo detector sí encuentra las caras en el cuadro original sin difuminar.
- COLMAP sí ve los cuadros sin difuminar. Eso queda en la máquina y no se publica.

**Métrica.** Caras que el detector aún ve después del difuminado: tiene que ser 0, y cualquier detección se revisa a ojo antes de seguir (`NOCHE.md`, 17 oct, 15:45 y 18:40).

**Aprendido.** Dos veces un cuadro con una cara sin difuminar llegó al entrenamiento: una cara en la esquina de un cuadro de A que se descartó mirando la miniatura, y una segunda mujer en un sofá de B que quedó fuera de la marca porque el seguimiento juntó a dos personas en una. Los dos splats se rehicieron y nunca se publicaron. De ahí salieron la revisión a tamaño completo y el control en teselas (`NOCHE.md`; `LECCIONES.md`, sección 6).

## 4. Poses de cámara: COLMAP

- **pycolmap 4.2.1**, rasgos SIFT.
- **Una cámara OPENCV por clip**, nunca compartida: el celular cambió de lente sin aviso (60° y 91° en el piloto; experimento E2).
- **Emparejamiento:** secuencial dentro de cada clip (20 vecinos más solape cuadrático) y recuperación por árbol de vocabulario con 15 vecinas por imagen (`vocab_tree_faiss_flickr100K_words32K.bin`, de la publicación 3.11.1 de COLMAP). La recuperación es lo que unió clips distintos en el piloto: 148 de 148 y 236 de 236 cuadros en un modelo, con las cámaras de cada clip a 0,0044 y 0,0177 u RMS de su modelo propio (experimento E2, `02-computo/README.md`).
- **Mapeo incremental**; se toma el modelo con más imágenes. En la segunda visita, B y A usaron `sfm_tramo_rapido.py`, con las razones de crecimiento del ajuste global en 1,2 en vez de 1,1 para ir más rápido (`NOCHE.md`, 22:40).
- **Undistort** a cámara PINHOLE para Brush y OpenMVS.
- **Segunda visita:** por piso, los cuadros buenos de la primera visita más los nuevos en un solo COLMAP desde cero, sin los cuadros viejos a 3 o menos de un corte débil (`viejos.py`; `LECCIONES.md`, sección 2.1).

**COLMAP no es determinista.** Dos corridas iguales registraron 115 y 138 de 148 cuadros (experimento E4). Un tramo partido se corre otra vez antes de darlo por perdido.

### Compuerta de fantasmas

Las estructuras repetidas (escaleras, pasillos y cubículos iguales) crearon en la primera visita copias fantasma: la escalera de B copiada un piso arriba, una franja de C girada 16,8° y media planta baja 0,27 u más abajo que la otra mitad (`LECCIONES.md`, punto 1.1; galería del edificio, sección 3). Desde entonces todo modelo pasa esta compuerta antes de seguir (`LECCIONES.md`, sección 2.7):

- **Cortes débiles** (`podado_cortes_base.py`, solo lectura de la base): para cada punto del video se cuentan cuántos de los 25 pares posibles entre los 5 cuadros de antes y los 5 de después se emparejaron. Con 3 o menos, el corte es débil: el modelo queda pegado con muy poco y puede haberse doblado sin que se note. En las láminas, las cámaras a 3 cuadros o menos de un corte débil llevan un anillo naranja.
- **Saltos** (`podado_evaluar_modelo.py`): dos cuadros seguidos del mismo clip separados por más de 12 pasos medianos, y, contra el marco de la versión anterior, desplazamientos de más de 0,35 u en planta o 0,15 u en altura.
- **Aceptación:** al menos el 95 % de las imágenes en el modelo mayor, 0 saltos o cada salto explicado mirando los cuadros, y cada clip nuevo con puntos 3D compartidos con los cuadros viejos del mismo espacio.

### Resultados por versión

| Piso y versión | Modelos | En el mayor | Cortes débiles (base / modelo mayor) | Reproyección |
|---|---|---|---|---|
| Piloto | 1 | 236 de 236 | sin dato | 0,52 px |
| A, primera visita (publicado) | 3 | 884 de 924 (95,7 %) | 24 / 13 | 0,558 px |
| A, dos visitas (no publicado) | 1 | 3.005 de 3.007 | 14 / 14 | 0,549 px |
| B, dos visitas (publicado) | 1 | 3.170 de 3.170 | 17 / 17 | 0,643 px |
| C, primera visita (reemplazada) | 10 | 1.456 de 2.208 (65,9 %) | 100 / 26 | 0,594 px |
| C, dos visitas (publicado) | 6 | 1.729 de 2.404 (71,9 %) | 24 / 4 | 0,696 px |

Fuente: `diag-19oct/metricas_por_version.csv` y `PILOTO-INTERIOR.md`. Los cortes débiles de las bases de la segunda visita en letras viejas incluyen huecos que dejó el filtrado de cuadros viejos, así que no se comparan uno a uno con los de la primera. C de las dos visitas no pasa el 95 %; lo que queda fuera es una franja vieja que ya estaba girada y el clip de la escalera angosta, que se parte en un giro rápido en un descanso (`DIAGNOSTICO.md`, sección 1). El orquestador lo aprobó con esa nota (`NOCHE.md`, 00:04).

**Lo que se probó y no mejoró:** quitar los pares lejanos en el tiempo (17 a 42 pedazos por piso), GLOMAP sobre la cadena (pasillos curvos) y una poda dirigida en C (endereza la franja pero duplica un pasillo). Detalle en la sección 7 de la galería del edificio y en `LECCIONES.md`, punto 1.3. ALIKED y LightGlue no se pudieron probar: la rueda de pycolmap 4.2.1 instalada trae los nombres pero no el soporte ONNX compilado (`pruebas-19oct/ALIKED-C.md`).

## 5. Splats: Brush

```
brush_app <escena> --total-steps <pasos> --max-splats 1500000 \
  --eval-split-every 8 --eval-every <pasos> --eval-save-to-disk \
  --export-every <pasos> --export-path <salida> --export-name 'splat_{iter}.ply'
```

- **Pasos:** 15.000 en el piloto y en la primera visita (el splat publicado de A); 30.000 en B y C de la segunda visita, como en el 3DGS original (Kerbl et al., SIGGRAPH 2023), porque con 15.000 C quedó unos 2 dB más blando en la zona vieja (`NOCHE.md`, 07:55).
- **Tope de 1.500.000 gaussianas.** Con 16 GB de memoria, el pico fue de 13,5 a 13,7 GB. Una corrida con 2.500.000 la cortó la guardia a los 10 min 40 s, cuando el swap pasó de 3 GB (`pruebas-19oct/A/PRUEBAS-A.md`).
- **Evaluación:** 1 de cada 8 cuadros queda fuera del entrenamiento.
- Tiempos: de 41 min (piloto, 15.000 pasos) a 2 h 36 min (A de las dos visitas, 45.000 pasos), según `PILOTO-INTERIOR.md` y `NOCHE.md`.

**Métrica: PSNR mediana sobre los cuadros apartados** (`psnr_comparar.py`). Cada PSNR vale solo para su propio conjunto de cuadros; para comparar dos versiones de un mismo piso se usan los cuadros que comparten.

| Piso | Publicado | PSNR mediana (n) | En los cuadros comunes con la versión anterior |
|---|---|---|---|
| Piloto | sí | 29,94 dB (30) | no aplica |
| A, primera visita | sí | 29,19 dB (110) | no aplica |
| A, dos visitas, 45.000 pasos | no | 27,98 dB (376) | 22,74 contra 26,07 dB (25) |
| B, dos visitas | sí | 29,46 dB (397) | 26,98 contra 29,52 dB (33) |
| C, dos visitas | sí | 32,08 dB (217) | 33,04 contra 33,00 dB (15) |

Fuente: `diag-19oct/metricas_por_version.csv`, `NOCHE.md` y `PILOTO-INTERIOR.md`. La galería redondea a 29,2 · 29,5 · 32,1 dB.

**Regla de publicación** (LM): un piso nuevo reemplaza al publicado solo si no empeora lo que ya estaba. C pasó; B se publicó porque cierra huecos y la pérdida en lo viejo es sutil a la vista; A no pasó y sigue con la primera visita (`NOCHE.md`, 17:55). La parte de la brecha de A que se pudo explicar es que la escena cambió entre los dos días: en 4 de los 25 cuadros comunes el montón de objetos de la recepción es otro (`PRUEBAS-A.md`).

**Probado y descartado en C** (`pruebas-19oct/C/PRUEBAS-C-000.6.md`): el filtro de flotantes de splat-transform con sus valores por defecto (peor en 217 de 217 cuadros), arrancar desde la nube limpia (la escalera desaparece) y máscaras de personas (la persona se va pero deja una estela).

## 6. Compresión: splat-transform y SOG

```
splat-transform <splat.ply> -N <tramo.sog>
splat-transform <splat.ply> -N -H 1 --decimate-adaptive 50% <movil.ply>
splat-transform <movil.ply> -N <tramo_movil.sog>
```

La versión para teléfono deja la mitad de las gaussianas y los armónicos esféricos hasta la banda 1. Pesos publicados: 7,4 MB en el piloto y de 11,0 a 11,2 MB por piso en el edificio, en MB de 1.048.576 bytes (galerías). `poses_tramo.py` escribe `poses.json` en el orden del video.

**Visor.** three 0.186.1 y Spark 2.2.0 por WebGL 2. El splat se lleva al marco del edificio (sección 8). Los puntos de navegación van sobre el piso; unos muros invisibles, hechos donde la nube tiene puntos de muro, impiden tocar a través de las paredes. Cada punto usa el campo de visión de su propio clip.

**Métrica.** Pruebas con Playwright como computador y como teléfono emulado: errores de consola, toques que llegan, desborde y cuadros por segundo, siempre con Brush parado (`NOCHE.md`, 22:25). **No se ha probado en un teléfono real.**

## 7. Nube densa y malla: OpenMVS

```
DensifyPointCloud escena.mvs --resolution-level 1
ReconstructMesh densa.mvs
TextureMesh densa.mvs -m malla.ply --global-seam-leveling 0 --local-seam-leveling 0
```

- **Nivel 1** (imágenes a la mitad). La resolución completa costó 7,07 veces el tiempo, con un ruido de 0 a 19 % menor y la misma cobertura (experimento E3). El C de la segunda visita se corrió a nivel 2 y sin malla: 7.173.549 puntos (`NOCHE.md`, 23:55).
- **Textura con la nivelación de color apagada** y la malla al 30 %: con los valores por defecto salían parches saturados que no están en las fotos (`PILOTO-INTERIOR.md`, sección 5).
- Compilación en CPU con tres arreglos documentados en `PILOTO-INTERIOR.md`.

**Limpieza** (paso 07). Clasifica la nube por vecindad en vóxeles y quita copias fantasma, flotantes y puntos de otro nivel. Las tres nubes limpias suman 11.016.647 puntos de 19.359.531 en las densas (galería del edificio). En C quedó solo la parte fiable, el 29 % de la nube. La malla tapa los huecos con superficies inventadas: no sirve para medir.

**Métricas.** Cobertura en planta: celdas de 0,05 u con puntos entre el piso y el cielo raso. Celdas vacías: 72,1 % en A, 67,0 % en B y 48,0 % en la parte fiable de C (galería del edificio, sección 3); la fracción depende del rectángulo elegido. Y cobertura de la nube sobre el cuadro en los peores casos: 26, 23 y 13 % (sección 2).

## 8. Marco, láminas y plantilla de medidas

- **Marco del edificio** (E1, `marco.py`): z es la normal del piso, los ejes siguen las dos familias de muros, el origen es el pie de la primera cámara. Planos por picos del histograma en cada eje con reajuste libre por SVD. Nunca se usa el «arriba» de las cámaras, que en el piloto estaba inclinado 5,51°.
- **Lámina A3** (E6, `laminas.py`): planta a 0,45 de la altura libre y cortes por el recorrido y por la escalera, dato y ajuste separados, discontinua donde falta información según HABS (NPS 2008, sección 4.5.5). Una escalera es fiable si le falta como mucho una huella en todo el tramo, si sus alturas casan con una contrahuella fija y si casi todas sus huellas tienen puntos; si no, va en morado discontinuo, rotulada como dudosa, y no pasa al paramétrico.
- **Plantilla E7**: distancias entre planos paralelos enfrentados y de piso a cielo raso, con incertidumbre según la planitud de los dos planos, y marca de menor confianza si más del 30 % de los puntos de un plano está cerca de un corte débil. `escalar.py` escala con las medidas de escala y valida con las de control; con un CSV sintético detectó una medida con 10 cm de error (E7).

## 9. Paramétrico: Blender

- `extraer_parametros.py` escribe `parametros.json`, el contrato: cada valor con su origen (nube o «supuesto: motivo»), número de puntos, RMS y confianza.
- `generar.py` (Blender en modo `-b`) arma piso, cielo raso, muros con espesor y vanos, y escalera. Toda pieza con un valor supuesto va a la colección Supuestos con material rayado. Con `--estricto`, el generador se niega si una pieza pensada como medida usa un supuesto.
- Un hueco sin puntos detrás es `hueco_sin_datos` y se rellena como supuesto, nunca como muro medido. El vidrio que la nube confunde con muro se pasa a supuestos por revisión visual de las fotos.
- **Varias alturas de cielo raso:** antes de modelarlas se resta el piso local y se cruza cada altura con el tramo de cámaras. En la primera visita, las alturas raras de A y C eran errores de registro; en C de la segunda visita, las dos alturas (losa a 2,130 u y vigas a unos 1,91 u) son geometría real (`NOCHE.md`, 00:00).

**Métricas** (`evaluar.py`): porcentaje de superficie vista que es supuesta (A 36,4 %, B 53,2 %, C 47,9 %) y porcentaje de puntos de muro a menos del umbral del modelo (A 82,6 %, B 87,6 %, C 91,9 %), según la galería del edificio, sección 5. Es ajuste contra la misma nube, no exactitud: sin control independiente no se puede declarar ningún nivel de exactitud (LOA). Además, el modelo se superpone a cinco cuadros por piso desde sus poses reales y se revisa a ojo.

## 10. Volumen único

Encargo de LM: un solo volumen con los tres pisos, encajando cada uno dentro de la maqueta exterior del visor del 106.

- **Maqueta:** caras exteriores de las fachadas en X ±22,75 y Z ±11,50 m de maqueta; espesor de muro supuesto de 0,30 m; niveles de piso 0,65, 4,30 y 7,95 (`volumen/INFORME.md`).
- **Encaje** (`encaje.py`): por piso, una similitud 2D más la z del piso. El giro se fija en el múltiplo de 90° que dicen los controles; escala y traslación salen de mínimos cuadrados sobre rectas y puntos de control (fachadas, testero, puerta). El segundo y el tercer piso se ubican a lo largo suponiendo que la escalera principal está en el mismo sitio en los tres pisos.
- **Armado** (`armar_volumen.py`, Blender): los tres `modelo.glb` con su matriz, la maqueta en gris transparente, y en rojo lo que sale por los lados de la maqueta.

**Resultado** (`encaje.json`): escalas de 3,526 (A), 2,937 (B) y 1,928 (C) m de maqueta por u, con residuos RMS de los controles de 0,43, 0,28 y 0,14 m.

**Comprobación cruzada, que es lo más útil del borrador:** entre pisos todo es coherente (la cámara sale a una altura de 1,77 a 1,86 m de maqueta en los tres pisos), pero contra la maqueta no cuadra, y siempre en el mismo sentido: la escalera sube de un piso a otro de 4,17 a 4,47 m de maqueta, contra 3,65 de piso a piso en la maqueta. El 19 de octubre, un plano esquemático del edificio (2022–2023), facilitado por LM, explicó buena parte del desajuste: la maqueta es demasiado ancha (`plano-dwg/INFORME-DWG.md`, registro de trabajo). Con el factor del plano (k = 0,821), las escalas quedan en 2,896, 2,412 y 1,583 m por u, la subida de los tramos en 3,43 a 3,67 m y la altura de cámara en 1,45 a 1,53 m. Ese factor **no se ha aplicado** al volumen publicado. Ver [`DATOS.md`](DATOS.md#unidades-y-escala).

## 11. Recorrido en el tiempo

Inspirado en una visualización de Bilawal Sidhu publicada en septiembre de 2026, donde cada cuadro de un video queda en el lugar y el momento en que se grabó (`REFERENTE-4D.md`). En el 106 lo que se mueve es la cámara, así que la estela es la cámara misma.

- **Datos** (`preparar.py`): solo poses de COLMAP ya existentes, de modelos que pasaron la compuerta de fantasmas (el mayor en B y C; en A, el corregido por la limpieza), llevadas al marco del edificio. No hay ningún modelo nuevo.
- **Cuadros clave:** uno nuevo cada cierto avance o cada cierto giro desde el anterior, más el inicio y el fin de cada clip, con los umbrales ajustados por piso para quedar cerca de 400 en computador (398, 388 y 374) y menos en teléfono (`edificio/recorrido-tiempo/X/datos.json`). Donde se caminó, las pirámides quedan escalonadas; donde se giró parado, se abren en abanico.
- **Miniaturas:** de los cuadros ya difuminados, solo de cuadros sin personas marcadas ni detectadas, revisadas a ojo; los cuadros clave que caían en un cuadro con personas se corrieron a un vecino sin gente o se quitaron.
- **Los tres juntos** se apilan con el encaje del volumen único, con su escala supuesta.
- Diseño decidido por un panel simulado (`recorrido-tiempo/PANEL-DISENO.md`): cámara virtual fuera y arriba, movimiento reducido con `prefers-reduced-motion`, un solo splat cargado en teléfono.

No sirve para medir, y la página lo dice siempre a la vista.

## 12. Revisión antes de publicar

- **Compuerta automática** (`antes_de_publicar.sh` y variantes): nueve grupos de revisiones; sale con error si algo falla y nada se publica sin «TODO OK» (`NOCHE.md`, 19:20).
- **Verificador independiente:** un agente que recalcula las cifras desde los archivos y mira las imágenes, no los registros del propio trabajo (`LECCIONES.md`, punto 12).
- **Pruebas como teléfono** con Playwright, con la GPU libre.
- **Publicación** solo con autorización de LM.
