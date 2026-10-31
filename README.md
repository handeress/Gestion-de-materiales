# Gestión de materiales

App web instalable (Android) que une **Proveedores** e **Inventario del taller** en una sola. Uso personal, sin servidor: los datos viven en el celular (IndexedDB, base `materiales-db`) y funciona sin internet. El dinero (contratado, pagado, saldos) sigue solo en el Control de pagos.

Dirección: https://handeress.github.io/Gestion-de-materiales/

## Pestañas

- **Proveedores**: directorio (llamar, WhatsApp, mapa) y cotizaciones con fotos/PDF, transporte y disponibilidad.
- **Materiales**: catálogo único (foto, marca, referencia, especificaciones, unidad base, controlar uso), historial de precios con diferencia contra el más bajo vigente y aviso de alzas de más del 10 %, comparador por proveedor y precios de referencia.
- **Proyectos**: lista de materiales por proyecto, importación CSV de Revit, comparativo y mensajes de WhatsApp. Los ids son los de `proyectos.json` de Obras-base (`tibana-y-guaduas`, `silvania`, `suesca`, `fontanar`); los demás quedan como proyectos locales sin página.
- **Bodega**: inventario por ubicación, buscador de retales, muebles desarmados, movimientos y etiquetas QR (el botón QR de arriba escanea).
- **Más**: buzón, respaldo, restaurar, sumar desde archivo, repetir migración, valor por categoría, ubicaciones y borrar ítems seleccionados.

## Kardex

La existencia nunca se escribe a mano: sale de sumar movimientos (`entrada`, `recepcion`, `salida`, `traslado`, `devolucion`, `ajuste`). Un movimiento no se edita; se corrige con un ajuste con nota.

## Migración

Al primer arranque lee `proveedores-db` e `inventario-db` **en solo lectura** (nunca las borra ni las modifica), copia todo a `materiales-db`, muestra un resumen antes/después y espera la confirmación. Repetirla desde Más suma lo que falte sin duplicar.

## Buzón de Claude

`herramientas/buzon.py` cifra una carga (AES-256-GCM, PBKDF2 200.000 iteraciones) y la deja en `buzon/`. Formato `{"app":"materiales","v":1,"data":{…}}`; mientras se migran las habilidades también acepta el formato viejo de `proveedores` o `inventario`. La clave (`MATERIALES_CLAVE`) se entrega aparte y **nunca** va en el repositorio, que es público.
