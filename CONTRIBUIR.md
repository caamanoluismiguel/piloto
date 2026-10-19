# Cómo contribuir

Gracias por querer aportar al piloto interior del 106. Este repositorio está pensado para que cualquier par pueda revisar cada cifra en su fuente, así que también es un aporte señalar un dato que no cuadra, aunque no toques nada.

Antes de aportar, ten en cuenta que el repositorio **no tiene licencia todavía** (PENDIENTE DE LM; propuesta en [`docs/DATOS.md`](docs/DATOS.md#licencia)). Mientras tanto, se puede ver y hacer fork en GitHub, y aportar por issue o pull request.

## Formas de aportar

- **Dato o cifra incorrecta.** Abre un issue que diga qué cifra está mal, dónde aparece (página de la galería o archivo) y con qué la contrastaste.
- **Medidas en sitio.** Si puedes ir al edificio, usa la planta de medición de cada piso (`edificio/X/06-medicion/`): toma cada medida dos veces, anota las dos, y di entre qué superficies mediste, con foto. Las fotos con personas no se suben.
- **Otro edificio o más datos.** Abre primero un issue para acordar el protocolo y cómo compartir los datos ([`docs/AGENDA.md`](docs/AGENDA.md#3-cómo-puede-aportar-alguien)).
- **Código o texto.** Pull request desde una rama `tipo/descripcion` (por ejemplo `docs/metodo-colmap`), con mensajes `tipo(ámbito): descripción` en español.

## Reglas que sigue todo aporte

1. **Caras difuminadas, sin excepción.** Ninguna imagen, cuadro, splat ni video con una cara sin difuminar entra al repositorio. Una duda se resuelve mirando el cuadro completo a buen tamaño, no la miniatura. Los afiches y dibujos no se difuminan.
2. **Medido, inferido o supuesto, siempre dicho.** Toda cifra dice de dónde sale. «Medido» en este repositorio quiere decir medido en la reconstrucción (en u) salvo que diga «medido en sitio». Lo supuesto se dibuja distinto de lo medido.
3. **Sin metros sin medida.** Ninguna cifra en metros sin una medida en sitio detrás; si usas una escala supuesta, escríbelo al lado de la cifra.
4. **Cada cifra con su fuente:** el archivo del repo, el script que la reproduce o la publicación con su DOI. Las PSNR, además, con el conjunto de cuadros sobre el que se midieron.
5. **Revisión de veracidad antes de publicar.** Todo texto o cifra nueva pasa por el agente `verificador` del proyecto, que recalcula desde los archivos y mira las imágenes, antes de que LM decida publicarlo. Si aportas sin ese agente, tu aporte lo revisa el autor antes de entrar.
6. **Privacidad.** Sin rutas de tu máquina, correos ni nombres de archivo de fotos privadas. El plano esquemático del edificio no se publica.
7. **Estilo:** español con tuteo, coma decimal y punto de miles, sin guiones como separadores de frases.

## Lo que aceptas al aportar

Propuesta, sujeta a la licencia que decida LM: declaras que el aporte es tuyo o que tienes derecho a aportarlo, conservas tus derechos sobre él y le concedes al autor una licencia no exclusiva para usarlo dentro de la obra. Tu aporte queda reconocido en el historial de git. Si este material se usa en un curso, aportar no puede ser condición para aprobarlo.

## A quién escribir

Para lo que cabe en un issue, en los issues del repositorio: https://github.com/caamanoluismiguel/piloto/issues

Para colaboraciones, datos o permisos: PENDIENTE DE LM.

La publicación en el sitio la decide LM.
