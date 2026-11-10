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

## Hallazgos: Centralización de Identidad Visual y Espaciado

### 1. Variables declaradas en :root

**Colores**

| Variable | Valor | Uso |
|---|---|---|
| `--color-primario` | `#0b5ed7` | Enlaces del menú |
| `--color-secundario` | `#198754` | Enlaces de filtros |
| `--color-terciario` | `#6f42c1` | Enlaces de redes sociales |
| `--color-acento` | `#fd7e14` | Botón comprar |
| `--color-destacado` | `#dc3545` | Botón destacado |
| `--color-borde-card` | `#ffc107` | Borde de las tarjetas |
| `--color-texto` | `#333333` | Texto base |
| `--color-superficie` | `#ffffff` | Fondo de las tarjetas |
| `--color-header` | `#cfe2ff` | Fondo del header |
| `--color-main` | `#fff3cd` | Fondo de `main` |
| `--color-aside` | `#d1e7dd` | Fondo de `aside` |
| `--color-footer` | `#e2e3e5` | Fondo de `footer` |

**Tipografía:** `--fuente-principal` (Inter, vinculada desde Google Fonts).

**Espaciados**

| Variable | Valor |
|---|---|
| `--espaciado-pequeno` | 12px |
| `--espaciado-medio` | 15px |
| `--espaciado-grande` | 20px |
| `--espaciado-xl` | 40px |

### 2. Ventaja de reutilizar medidas de margen y relleno

Antes, el valor `20px` estaba escrito a mano en seis reglas (body, header, main,
aside, article y footer) y `15px` en tres. Para cambiar el aire de la interfaz había
que editar cada una, con riesgo de olvidar alguna. Ahora el valor vive en
`--espaciado-grande` y cambiarlo en un solo lugar actualiza todas a la vez. Además,
el diseño usa una escala fija de medidas en vez de números inventados en cada selector.

### 3. Cómo :root previene colisiones visuales al crecer la hoja

(Redáctalo con lo que observaste en la auditoría: por ejemplo, que todos los selectores
leen del mismo origen, que no aparecen dos tonos "casi iguales" por errores de copia,
y que una regla nueva reutiliza una variable en lugar de crear otro valor.)****

## Hallazgos: Pseudoclases y Pseudoelementos

### 1. Diferencia entre pseudoclase y pseudoelemento

Una pseudoclase (`:hover`, `:active`, `:focus`) selecciona un elemento que ya existe
cuando entra en cierto estado, por ejemplo cuando el cursor pasa por encima. Reacciona
a eventos del usuario. Un pseudoelemento (`::before`, `::after`) crea una caja nueva
que no está en el HTML, dentro o junto al elemento, y se usa para inyectar contenido
decorativo. En el proyecto, `.filtro-enlace:hover` cambia colores y
`.filtro-enlace::before` agrega el símbolo "•".

### 2. Importancia de :focus para la accesibilidad

Quien navega con teclado (Tab) necesita ver qué elemento está activo. Si se quita el
contorno, no sabe dónde está. Con `:focus` y un `outline` visible en el menú lateral,
la categoría activa se identifica claramente, y es una buena práctica de accesibilidad
web.

### 3. Por qué decorar con ::before/::after y no con etiquetas vacías

El HTML debe describir contenido y estructura, no decoración. Si se agregan etiquetas
vacías solo para dibujar insignias o iconos, el HTML se ensucia, los lectores de
pantalla pueden leer elementos sin sentido y cambiar el diseño obliga a tocar el HTML
en varios sitios. Con pseudoelementos, la decoración vive en el CSS, se reutiliza con
las variables de `:root` y el HTML se mantiene semántico.

## Hallazgos: Arquitectura BEM y Modularidad

### 1. Qué significa BEM y cómo se escribe

BEM significa Bloque, Elemento, Modificador. Se escribe `bloque__elemento--modificador`.
- **Bloque:** componente autónomo, como `.card` o `.sidebar`.
- **Elemento:** parte que solo tiene sentido dentro del bloque, unida con doble guion
  bajo, como `.card__title` o `.sidebar__link`.
- **Modificador:** variación de aspecto, unida con doble guion medio, como
  `.card--destacada` o `.btn--secundario`.

### 2. Por qué dividir el CSS en varios archivos ayuda al trabajo colaborativo

Cada componente vive en su propio archivo (`tarjeta.css`, `botones.css`,
`menu-lateral.css`...). Dos personas pueden trabajar a la vez en componentes distintos
sin pisarse ni generar conflictos en Git, y es fácil encontrar dónde está cada estilo.

### 3. Cómo BEM evita la guerra de especificidad

Antes usaba selectores anidados como `header nav ul li a` (5 pts) o
`article p:last-of-type` (12 pts), y para sobrescribirlos había que usar selectores
más fuertes. Con BEM todas las reglas son de una sola clase (10 pts), y las colisiones
se resuelven por el orden: el modificador va después del bloque. No hay IDs ni
`!important`.

### Nota sobre @import

Los `@import` van al inicio de `styles.css`, antes de `:root`, porque CSS solo los
acepta antes de cualquier otra regla. Las variables de `:root` siguen disponibles en
todos los módulos.