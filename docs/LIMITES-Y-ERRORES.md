# Límites y errores

## Lo que no se puede afirmar con este material

- **Ninguna medida en metros.** Todo está en u. Las cifras en metros de maqueta del volumen único son una escala supuesta, y la maqueta resultó más ancha de lo que declara ([`DATOS.md`](DATOS.md#unidades-y-escala)).
- **Ningún nivel de exactitud (LOA).** No hay control independiente. Los porcentajes del paramétrico miden ajuste contra la misma nube, no exactitud (`LECCIONES.md`, sección 7; `volumen/INFORME.md`, sección 5).
- **Que los pisos estén unidos.** Cada piso es un modelo aparte. Los tres juntos están ubicados dentro de la maqueta, con el supuesto de que la escalera principal está en el mismo sitio en los tres pisos; no están unidos por imágenes comunes ni por medidas.
- **Que un splat mida.** Los splats son para ver. Los estudios con referencia láser coinciden en que se ven mejor y miden peor que la nube de MVS con las mismas fotos ([`AGENDA.md`](AGENDA.md)).
- **Que una PSNR sea mejor que otra** si se midió sobre otros cuadros. Solo se comparan en los cuadros comunes.
- **Dónde hay vidrio.** La nube no lo distingue. Lo que la galería marca como vidrio sale de revisión visual de las fotos y falta confirmarlo en sitio.
- **Que un hueco sea un hueco.** Una zona sin puntos puede ser vidrio, pared lisa, algo oscuro o un lugar que nadie grabó.
- **Que el visor funcione en un teléfono real.** Solo se probó en teléfono emulado.
- **Que no queden caras** en el splat del piloto ni, sin una revisión más, en el splat publicado de la planta baja ([`DATOS.md`](DATOS.md#privacidad)).
- **El uso, la historia o el autor del edificio.** Este repositorio no los trata.
- **Que el resultado se repita.** COLMAP no es determinista (115 y 138 cuadros registrados en dos corridas iguales; experimento E4) y no hay prueba de repetición para ninguna versión de los tres pisos (`diag-19oct/DIAGNOSTICO.md`, sección 4).
- **Que el método sirva para otros edificios.** Es un solo caso, grabado por una sola persona.

## Errores encontrados y corregidos

### En los datos y la reconstrucción

| Error | Cómo se vio | Corrección |
|---|---|---|
| Copias fantasma: escalera de B copiada un piso arriba, salón repetido, franja de C girada 16,8°, media planta baja 0,27 u más abajo | Las de B y C las vio LM mirando las láminas, que un verificador que comprobaba contra su propio registro había dado por buenas; la de A salió en la limpieza de la nube | Limpieza de la nube y compuerta de fantasmas para todo modelo nuevo ([`METODO.md`](METODO.md#compuerta-de-fantasmas)) |
| Un cielo raso falso en A (0,5209 u) que salía del ala bajada | Al limpiar la nube | La altura pasó a 0,8081 u (galería del edificio, sección 4) |
| Puertas de vidrio de A y una ventana con lámina de colores de B modeladas como muro «medido» | Revisión de las superposiciones con las fotos | Pasaron a supuestos por revisión visual; falta confirmarlo en sitio |
| Una escalera de «3 peldaños» en B que no existía, y una regla que contaba huecos en vez de huellas faltantes | Revisión de las láminas | Regla corregida; las escaleras dudosas van en morado y no entran al paramétrico (`LECCIONES.md`, punto 14) |
| Textura de la malla con colores que no estaban en las fotos | Comparación con las fotos | Nivelación de color apagada en OpenMVS (`PILOTO-INTERIOR.md`) |
| El recorrido del piloto ordenado por identificador de COLMAP: 37,99 u en vez de 19,24 | Revisión del recorrido | Cámaras siempre por nombre (`LECCIONES.md`, punto 22) |
| La opción de quitar todos los muros invisibles rompía el visor | Prueba antes de publicar | Guarda para el caso de muros vacíos (`NOCHE.md`, 16:20) |

### En la privacidad

- **Dos cuadros con caras llegaron al entrenamiento** en la segunda visita: una cara en la esquina de un cuadro de A, descartada mirando la miniatura en vez del cuadro completo, y una segunda mujer en un sofá de B, que quedó fuera de la marca porque el seguimiento juntó a dos personas. El verificador detuvo la publicación de B; los dos splats se rehicieron y nunca se publicaron (`NOCHE.md`, 15:45 y 18:40). Después de eso, una auditoría encontró 89 marcas nuevas en B, 5 en C y 115 en la versión nueva de A (`NOCHE.md`, 18:30 y 01:10).
- **El splat del piloto con una persona** sigue publicado. No está corregido ([`DATOS.md`](DATOS.md#privacidad)).

### Nuestros, en el método y en la comunicación

- **La lista de captura de la segunda visita pidió girar parado.** Pidió «una vuelta completa de 360 grados en 30 segundos» y «nunca camines y gires a la vez: primero caminas, te detienes, y después giras». Eso contradice la práctica recomendada y la propia guía del 8 de octubre, que advertía que girar desde un solo punto no basta. Evitó los giros rápidos, pero los giros lentos en el sitio tampoco dan paralaje: la segunda visita triangula peor que la primera (10,3 contra 3,8 % de puntos con menos de 2° en A; `DIAGNOSTICO.md`, sección 2). El investigador lo advirtió la noche del 16 de octubre, antes de procesar (`LECCIONES.md`, punto 1.2), pero no se midió hasta que la evaluación del informe externo lo puso como error nuestro (`EVAL-INFORME-EXTERNO/PANEL.md`).
- **La escala del exterior se presentó mal y después se corrigió.** El 17 de octubre, al plantear el volumen único, el asistente que orquestaba le dijo a LM que la maqueta exterior daba «metros y alturas reales», y LM decidió con esa premisa. Esa misma noche el investigador encontró que la maqueta tiene una escala de ±12 % y ventanas inferidas en los pisos 2 y 3 (`NOCHE.md`, 23:00). El volumen pasó a ser un borrador rotulado «escala supuesta». El 19 de octubre el plano esquemático mostró que hasta el ±12 % se quedaba corto a lo ancho ([`DATOS.md`](DATOS.md#unidades-y-escala)). La galería todavía dice ±12 %.
- **Escalas inventadas en la primera visita.** Se llegó a escribir que A «tiene escala 2x», un número que salía de suponer 3 m de altura, y una altura de 0,52 u de A que era falsa (`LECCIONES.md`, punto 11).
- **Raúl se fue sin una sola medida con cinta**, y no se le pidió la escalera como clip continuo entre pisos, ni exposición bloqueada, ni grabar sin estabilización electrónica; la lente sí se pidió fija en 0,6×, y aun así hubo clips con otros campos de visión (`EVAL-INFORME-EXTERNO/PANEL.md`, «Errores nuestros»).
- **Un verificador que verificaba contra el propio registro** dio 27 comprobaciones bien sin ver la escalera inventada de B ni la franja de C. Desde entonces el verificador recalcula desde los archivos y mira las imágenes (`LECCIONES.md`, punto 12).
- **Una compuerta automática que revisaba fechas y no contenido**, y un control de caras que al principio solo miraba los cuadros vecinos a personas conocidas. Los señaló el verificador y se ampliaron a todos los cuadros (`NOCHE.md`, 22:45).
- **Fuentes mal atribuidas** en informes internos: dos artículos citados por algo que no dicen. Regla: abrir la fuente antes de citarla (`LECCIONES.md`, punto 19).
- **Una premisa técnica falsa del propio panel:** que la instalación de pycolmap ya traía ALIKED y LightGlue en CPU. Los nombres estaban, el soporte no (`pruebas-19oct/ALIKED-C.md`).

## La evaluación del informe externo

El 19 de octubre LM compartió un informe externo con recomendaciones para pasar de pisos partidos en u a un levantamiento unido, escalado y verificable. Su tesis: la causa es la captura, no el software. Un panel simulado (fotogrametría, visión por computador, levantamiento y patrimonio, captura de campo y un crítico) revisó cada recomendación contra lo hecho (`EVAL-INFORME-EXTERNO/RECOMENDACIONES.md`, `INVENTARIO.md` y `PANEL.md`).

**En qué acierta.** En el principio: primero la captura, después las poses, y la escala con control independiente. Lo que más vale del informe depende de volver al edificio: medidas con cinta (de control y de chequeo, separadas), la escalera en un clip continuo entre pisos, caminar en arco en vez de girar parado, exposición bloqueada y una sola lente.

**En qué se equivoca sobre el proyecto.** Trata los tres pisos como partidos, cuando con la segunda visita A y B quedaron en un solo modelo; mezcla cifras de las dos visitas en el tercer piso; propone activar una detección de bucles que el árbol de vocabulario ya cubre; da por pendiente la segunda visita, que ya había ocurrido; y no menciona que algunos métodos que recomienda tienen licencia no comercial.

**Qué se hizo después.** El 19 de octubre se corrieron las pruebas baratas que recomendó el panel: giros por clip y triangulación por zona (`diag-19oct/DIAGNOSTICO.md`), las dos pruebas de la planta baja (`pruebas-19oct/A/PRUEBAS-A.md`) y tres variantes de Brush en C (`pruebas-19oct/C/PRUEBAS-C-000.6.md`). Ninguna superó a lo publicado. ALIKED y LightGlue no se pudieron correr. La «fase de rescate en la nube» del informe (métodos aprendidos con GPU) el panel no la haría antes de una nueva captura: lo que falta en C no está en los datos de entrada: el salón 302 no llegó en la segunda visita, y el pasillo del mural se grabó en la primera pero sus cuadros quedaron fuera del filtrado de cuadros viejos y ningún clip nuevo lo regrabó (`DIAGNOSTICO.md`, sección 2).

**Lo que queda pendiente de LM:** una tercera visita corta y dirigida, de 2 a 3 horas con una persona, con la lista reescrita. Es lo único que daría metros, pisos unidos y un nivel de exactitud declarable.
