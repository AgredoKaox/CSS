# Catálogo de Productos

Práctica de jerarquía de estilos CSS (selectores, herencia, cascada y especificidad)
sobre un documento HTML semántico.

## Cálculo de especificidad

| Tipo de selector | Puntos |
|---|---|
| Etiqueta (`p`, `a`, `li`) | 1 |
| Clase (`.btn-comprar`) | 10 |
| ID (`#btn-destacado`) | 100 |

Se suman los puntos de todos los componentes del selector. Gana la regla con más puntos;
si hay empate, gana la declarada más tarde (cascada).

### Ejemplos del proyecto

| Selector | Cálculo | Total |
|---|---|---|
| `body` | 1 | 1 |
| `header nav ul li a` | 5 etiquetas | 5 |
| `aside ul li a` | 4 etiquetas | 4 |
| `.btn-comprar` | 1 clase | 10 |
| `#btn-destacado` | 1 ID | 100 |

## Conclusiones

1. **Herencia:** `font-family` y `color` declarados en `body` llegan a h2, p y li; los enlaces
   `<a>` requieren regla propia porque el navegador les asigna color por defecto.
2. **Selectores combinados:** anclar cada enlace a su contenedor permite cambiar un menú
   sin afectar a los demás.
3. **Especificidad vs. cascada:** `#btn-destacado` (100) vence a `.btn-comprar` (10)
   aunque esté declarado antes.
4. **Estandarización:** un sistema plano de clases (10 pts) evita IDs y `!important`;
   las colisiones se resuelven por orden de declaración.

   ## Hallazgos: El Modelo de Caja

### 1. Diferencia práctica entre Margin y Padding
El padding es el espacio **dentro** de la caja, entre el contenido y el borde, y se
pinta con el fondo del elemento. El margin es el espacio **fuera** de la caja, que
separa un elemento de sus vecinos, y siempre es transparente. En las tarjetas,
`padding: 20px` separa el texto del borde, y `margin: 15px` separa una tarjeta de otra.

### 2. Por qué es obligatorio usar box-sizing: border-box
Por defecto (`content-box`), el `width` solo mide el contenido, así que el padding y el
borde se suman por fuera y la caja termina más ancha de lo esperado, pudiendo desbordar
a su padre. Con `* { box-sizing: border-box; }` el ancho declarado ya incluye padding y
borde, y la caja crece hacia adentro, por lo que las medidas son predecibles.

### 3. Efecto de un margen grande sobre los elementos hermanos
Al darle un margen grande a una tarjeta, esta empuja a sus hermanos y los desplaza
en esa dirección. Los márgenes verticales entre hermanos se colapsan: entre dos
tarjetas con `margin: 15px` la separación real es de 15px y no de 30px, porque el
navegador toma el mayor de los dos.

### Auditoría en DevTools
En la pestaña Computed, un `<article>` mostró padding de 20px y margin de 15px,
coincidiendo con el código.