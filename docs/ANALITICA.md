# Analítica de visitas

Las páginas de esta galería cuentan visitas con [GoatCounter](https://www.goatcounter.com/), de forma anónima y sin cookies. El esquema completo (rutas, marcas `?ref=`, eventos, qué no se mide y la ponderación de privacidad según la Ley 81 de 2019 de Panamá) es uno solo para este repo y para el visor del 106, y vive en [`docs/ANALITICA.md` del repo edificio-106](https://github.com/caamanoluismiguel/edificio-106/blob/main/docs/ANALITICA.md).

## Lo propio del piloto

- Se cuentan las 9 páginas de la galería: las dos portadas (`/piloto/` y `/piloto/edificio/`), `visor/`, `edificio/ideas.html`, `edificio/recorrido-tiempo/` y los cuatro recorridos (`05-recorrido/` y `edificio/A|B|C/05-recorrido/`).
- No se cuentan `edificio/preguntas/` ni `edificio/guia-fotos.html` (documentos de trabajo).
- Eventos: `piloto-mapa-abrir` (se abre el minimapa de un recorrido, una vez por visita) y `piloto-permanencia-30s`, `-2min`, `-5min` (tiempo con la página visible).
- La galería no se reparte con marcas `?ref=` mientras sea de uso interno.
- No hay cifras visibles ni enlaces al panel de GoatCounter, que es privado.

## Excluir tu navegador

Abre cualquier página con `#toggle-goatcounter` al final una vez; otra vez para volver a contarla. Vale para todo `caamanoluismiguel.github.io` en ese navegador.

Responsable: Luis Miguel Caamaño. Consultas por los Issues del repositorio.
