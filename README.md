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