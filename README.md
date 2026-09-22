# Generador de Etiquetas con código de barras

Herramienta web para imprimir etiquetas de producto con código de barras en
impresora térmica. Una sola página, sin instalar nada, funciona sin internet
una vez cargada.

**Ábrela aquí:** https://xhibixhris.github.io/etiquetas-quantum/

## Qué hace

- **EAN-13, EAN-8, UPC-A y Code 128**, con detección automática según el código.
- Calcula el dígito verificador si le das 12 dígitos, y rechaza un EAN-13 con
  verificador inválido en lugar de imprimir una etiqueta que no escanea.
- Tamaños: 40×25 mm (por defecto), 50×25, 50×30, 58×40, 38×25, 32×19 o libre.
- El nombre del producto se auto-ajusta para caber sin cortarse.
- Lote: varios códigos con copias distintas; se puede pegar directo desde Excel.
- Guarda en **PDF vectorial** (una etiqueta o el lote completo), PNG a 300 dpi y SVG.

## Calidad del código de barras

En 40×25 mm el EAN-13 sale a módulo 0.333 mm, es decir **magnificación 101%**
(el nominal de GS1 es 0.33 mm). Las quiet zones —11 módulos a la izquierda y 7 a
la derecha— están incluidas.

La página avisa si la combinación de código y tamaño cae por debajo del **80% de
magnificación**, que es el mínimo de GS1. Por ejemplo, un EAN-13 en una etiqueta
de 32×19 mm queda en 79%: ahí conviene subir de tamaño.

## Al imprimir

1. Escala **100%**. Nunca "ajustar a la página": deforma el código y deja de escanear.
2. Márgenes en **ninguno**, sin encabezados ni pies de página.
3. El tamaño de papel del driver debe coincidir con el elegido en la página.
4. Imprime **una de prueba y pásala por el escáner** antes de tirar el lote completo.

Si el escáner no lee: sube el oscurecimiento del driver o activa "barras más
gruesas". Si aun así falla, la etiqueta es muy chica para ese código.

## Verificación

El generador se validó así:

- Tablas de codificación L/G/R comprobadas por sus propiedades estructurales
  (complemento, reverso, paridad, 4 elementos por símbolo).
- Dígito verificador contra 8 códigos reales conocidos.
- Tabla Code 128 completa: 107 patrones de 11 módulos cada uno.
- Round-trip codificar → decodificar.
- **Control end-to-end:** se renderizó el PDF final a imagen, se leyeron los
  píxeles y se decodificó como haría un escáner de línea. 12/12 barridos a
  distintas alturas devolvieron el código correcto.
