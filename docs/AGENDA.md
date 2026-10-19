# Agenda para pares

Preguntas abiertas que el piloto deja y en las que otros grupos pueden aportar: datos, más edificios, código o revisión. Para cada una digo qué se sabe hoy y qué haría falta.

## 1. Preguntas abiertas del propio 106

1. **¿Cuánto se equivoca la reconstrucción en metros?** Falta lo más básico: medidas con cinta. Las plantillas por piso ya están ([`edificio/X/06-medicion/`](../edificio/index.html)) y `escalar.py` escala con unas y valida con otras.
2. **¿Se pueden unir los pisos?** Hoy no hay imágenes comunes entre los modelos. La propuesta es grabar la escalera en un clip continuo que empiece y termine de 3 a 5 m dentro de cada piso, y unir con un ajuste conjunto (`EVAL-INFORME-EXTERNO/PANEL.md`, ítems A8 y B11).
3. **¿Qué hay al final del pasillo central del testero oeste en B y C?** Dos pisos grabados en dos visitas ven un pasillo que sigue más allá de donde la maqueta pone el testero (`volumen/INFORME.md`, sección 3). Con la escala del plano esquemático, el muro que se tomó por testero podría ser uno interior (`plano-dwg/INFORME-DWG.md`, sección 4.3). Se resuelve mirando en sitio.
4. **¿Por qué la versión nueva de la planta baja sale más blanda en lo viejo?** No es apariencia ni sobre todo presupuesto de gaussianas; una parte medida es que la escena cambió entre los dos días. El resto no tiene causa probada (`pruebas-19oct/A/PRUEBAS-A.md`).
5. **¿Cuánto pesa la triangulación baja de los giros lentos parados** en la cobertura de la nube densa y en lo supuesto del paramétrico? No se midió (`diag-19oct/DIAGNOSTICO.md`, sección 5).
6. **¿Cuánto de una diferencia de ±0,3 dB entre corridas de Brush es ruido?** No hay prueba de repetición (`pruebas-19oct/C/PRUEBAS-C-000.6.md`).
7. **¿Cómo enseñar con esto?** Los ejercicios propuestos ([`edificio/ideas.html`](../edificio/ideas.html)) no se han probado con estudiantes. Medir qué aprenden, con un evaluador externo, sería un aporte propio.

## 2. Línea de investigación: documentar edificios solo con celular

Idea de LM del 17 de octubre de 2026. El investigador del proyecto revisó la literatura de 2023 a 2026 el 19 de octubre (`ESTADO-ARTE-CELULAR.md`, registro de trabajo). Las citas de abajo las comprobó contra la fuente primaria. De Fabris 2023, Uluçay Temel 2026, Remondino 2023, Oxford Spires y Beltramini 2023 leyó solo el resumen; el detalle de LaMAR (un edificio de oficinas de varios pisos) y de Nuha 2026 (un aula) sale de su informe. Su búsqueda tuvo huecos: CumInCAD (actas de eCAADe, SIGraDi, CAADRIA y ACADIA) bloqueó las consultas automáticas, así que la revisión de esos congresos quedó incompleta. «No encontré» no quiere decir «no existe».

### Qué ya está hecho

- **El celular da centímetros en objetos y exteriores.** Con fotos o video: de 0,7 a 1,6 cm en un monumento funerario dentro de una cripta (Musicco, Rossi y Verdoscia 2023, [doi:10.5194/isprs-archives-XLVIII-M-2-2023-1119-2023](https://doi.org/10.5194/isprs-archives-XLVIII-M-2-2023-1119-2023)); de 1 a 2 cm en dianas del exterior de dos edificios pequeños, con iPhone, gimbal y COLMAP o Pix4D (McNally et al. 2026, ISPRS Annals, [doi:10.5194/isprs-annals-XI-M-1-2026-31-2026](https://doi.org/10.5194/isprs-annals-XI-M-1-2026-31-2026)); de 4 a 7 cm con fotos de celular en un castillo, según el resumen (Fabris et al. 2023, [doi:10.3390/drones7020101](https://doi.org/10.3390/drones7020101)).
- **El LiDAR del iPhone, en cuartos o en un edificio pequeño:** 31,2 mm de RMSE en 32 medidas de un edificio de dos pisos, con dron afuera y un solo caso, según el resumen (Uluçay Temel y Temel 2026, [doi:10.3390/buildings16132512](https://doi.org/10.3390/buildings16132512)); 0,182 m de RMSE dimensional en un aula (Nuha et al. 2026, [doi:10.3846/gac.2026.25678](https://doi.org/10.3846/gac.2026.25678)).
- **Los splats se ven mejor y miden peor que MVS** con las mismas fotos. En McNally et al. 2026, al pasar a splats la distancia nube a nube media sube tres veces o más. Lo confirman también Clini et al. 2024 ([doi:10.5194/isprs-archives-XLVIII-2-W8-2024-93-2024](https://doi.org/10.5194/isprs-archives-XLVIII-2-W8-2024-93-2024)) y el conjunto Oxford Spires (Tao et al. 2025, [doi:10.1177/02783649251369905](https://doi.org/10.1177/02783649251369905)). La práctica que se impone es anclar los splats a una geometría medida.
- **Las fallas que vimos en el 106 ya están descritas:** giros sin desplazamiento (LighthouseGS, Han et al., arXiv 2507.06109), estructuras repetidas (Doppelgangers++, Xiangli et al., CVPR 2025, [doi:10.1109/CVPR52734.2025.02530](https://doi.org/10.1109/CVPR52734.2025.02530); Perda, Morelli y Remondino 2025, [doi:10.5194/isprs-archives-XLVIII-1-W4-2025-95-2025](https://doi.org/10.5194/isprs-archives-XLVIII-1-W4-2025-95-2025)), vidrio y paredes lisas (Remondino et al. 2023, [doi:10.3390/rs15143585](https://doi.org/10.3390/rs15143585)). Corregirlas con algoritmos es trabajo de visión por computador, no de este proyecto.
- **Las cámaras 360 tampoco resuelven solas los interiores:** desviaciones estándar de 18 a 26 cm en el interior de una iglesia (Clini et al. 2024).
- **Enseñar fotogrametría con celular ya se probó**, de forma cualitativa (Beltramini et al. 2023, [doi:10.5194/isprs-archives-XLVIII-M-2-2023-195-2023](https://doi.org/10.5194/isprs-archives-XLVIII-M-2-2023-195-2023)).
- **El conjunto de datos más cercano** es LaMAR (Sarlin et al., ECCV 2022, [doi:10.1007/978-3-031-20071-7_40](https://doi.org/10.1007/978-3-031-20071-7_40)): un edificio de oficinas de varios pisos grabado con iPhone y con láser, pero pensado para localización, sin anotación arquitectónica.

### El hueco que queda

Formulado para que un revisor lo pueda comprobar (`ESTADO-ARTE-CELULAR.md`, sección 3.2):

> No se encontró ninguna evaluación revisada por pares de edificios completos de varios pisos, con interiores y escaleras, levantados solo con video RGB de celular por operadores no expertos con un protocolo escrito, con error medido contra una referencia independiente, tanto global (entre pisos) como por elemento, y con la variación entre operadores.

Tampoco se encontró un conjunto de datos abierto que junte edificios completos de varios pisos, video de celular con un protocolo documentado, referencia láser o de estación total, medidas de chequeo independientes y anotación arquitectónica. Ni un trabajo latinoamericano que documente un edificio completo con celular y mida el error.

**Lo que no sería aporte:** la cadena de herramientas del 106 por sí misma. Es una combinación de herramientas existentes que los modelos de reconstrucción de avance directo pueden dejar obsoleta pronto. El aporte posible está en la evaluación, los datos y el protocolo.

### Qué haría falta medir

Según el mismo informe (sección 3.3):

- **Edificios:** al menos 3, idealmente 5 o 6, de 2 a 4 pisos y de tipologías distintas (escalera cerrada, mucho vidrio o paredes blancas, elementos repetidos, uno patrimonial). El 106 sirve de piloto, no de caso final, porque se grabó antes de fijar el protocolo.
- **Operadores:** por edificio, al menos 3 no expertos con un protocolo de una página y 1 experto, cada uno grabando el edificio completo.
- **Referencia:** escáner láser terrestre en al menos 2 edificios; en todos, unas 30 distancias de chequeo separadas de las que dan la escala, incluidas distancias entre pisos por la escalera.
- **Métricas:** número de modelos y si los pisos quedaron unidos; RMSE y máximo en las distancias de chequeo y deriva vertical entre pisos; error por elemento contra el láser; porcentaje de supuesto por elemento y por piso, como ya publica el 106; costo y tiempo; y, si hay splats, calidad de imagen en vistas fuera de la trayectoria.
- **Comparación:** en al menos un edificio, también con LiDAR de iPhone y, si se consigue, con cámara 360.
- **Ética:** difuminado previo y consentimiento por escrito.

### Riesgos de rechazo que el informe anticipa

Un solo caso; usar las mismas medidas para escalar y para chequear; presentar los splats como medida; no comparar con LiDAR de iPhone; llamar «no expertos» a quienes no pasaron por un estudio de usuarios; y que el docente evalúe a sus propios estudiantes con su propio protocolo, lo que pide un evaluador externo.

## 3. Cómo puede aportar alguien

- **Datos.** Grabar otro edificio con un protocolo escrito, con medidas de chequeo separadas de las de escala, y compartir los cuadros con las caras difuminadas, las poses y las medidas. Si tienes acceso a un escáner láser o a una estación total, una referencia en uno o dos edificios es lo que más vale.
- **Medidas en el 106.** Tomar las medidas de las plantillas por piso con flexómetro, dos veces cada una, y documentar entre qué superficies se midió y con foto.
- **Más edificios.** Repetir la cadena en otro edificio y reportar las mismas métricas: modelos, porcentaje registrado, cortes débiles, PSNR en cuadros apartados y porcentaje supuesto.
- **Código.** Probar emparejadores mejores sobre la escalera de C (ALIKED y LightGlue con soporte ONNX), unir pisos con un ajuste conjunto cuando haya imágenes comunes, o automatizar la revisión de caras.
- **Docencia.** Probar los ejercicios de [`edificio/ideas.html`](../edificio/ideas.html) en un curso y contar qué funcionó.
- **Revisión.** Encontrar una cifra que no cuadre con su fuente es un aporte en sí.

Cómo proponerlo: [`CONTRIBUIR.md`](../CONTRIBUIR.md). Para datos o colaboraciones que no caben en un issue, el contacto es PENDIENTE DE LM.

## Dónde publicar, según el informe

En orden: SIGraDi o eCAADe para el protocolo, la docencia y el piloto presentado como preliminar; ISPRS Annals (texto completo revisado) para la evaluación con varios edificios y láser; una revista o un artículo de datos para la versión completa. El informe no verificó fechas de convocatoria. Publicar los datos con DOI encaja con la estructura de DOI que está preparando el repo `edificio-106` (PENDIENTE DE LM).
