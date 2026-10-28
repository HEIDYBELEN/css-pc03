# NORTE · Portafolio Interactivo

Landing page de portafolio para **NORTE**, un estudio de producto digital ficticio
radicado en Lima. El sitio presenta el estudio, tres proyectos y un formulario de
contacto, e integra **Bootstrap 5**, **transformaciones CSS** y **animaciones CSS**
como eje del diseño.

Trabajo desarrollado para la **Práctica Calificada N° 3** del curso
*Hojas de Estilo en Cascada Avanzado*.

---

## 1. Contenido del repositorio

```
.
├── index.html              Estructura semántica del sitio
├── css/
│   └── styles.css          Hoja de estilos propia (transformaciones y animaciones)
├── assets/
│   └── img/                Fotografías de los proyectos
├── docs/
│   └── Evidencias-PC3.pdf  Capturas del sitio y del código CSS
├── .gitignore
└── README.md
```

## 2. Cómo visualizar el proyecto

No requiere instalación ni compilación.

1. Descargar o clonar el repositorio.
2. Abrir `index.html` en el navegador.

También se puede servir con cualquier servidor estático:

```bash
python3 -m http.server 8000
# http://localhost:8000
```

El proyecto usa el CDN de Bootstrap 5 y Google Fonts, por lo que la primera carga
necesita conexión a internet. Las imágenes están incluidas en el repositorio.

## 3. Tecnologías

| Tecnología | Uso |
| --- | --- |
| HTML5 semántico | Estructura (`header`, `main`, `section`, `footer`) |
| Bootstrap 5.3.3 (CDN) | Grid, navbar colapsable, formulario, botones |
| CSS3 | Variables, transformaciones 2D y 3D, `@keyframes`, media queries |
| Google Fonts | Space Grotesk (titulares) y DM Sans (texto) |

---

## 4. Transformaciones aplicadas

Todas las transformaciones 2D viven en la sección **Proyectos** y se activan con
`:hover`. Cada tarjeta declara su propia `transition` para que el cambio sea
progresivo y no brusco.

| Tarjeta | Efecto | Regla CSS |
| --- | --- | --- |
| Atlas | Rotación | `transform: rotate(-2.4deg) translateY(-6px)` |
| Hilo | Escalado | `transform: scale(1.045)` |
| Mercado | Traslación + sesgado | `transform: translateY(-10px) skewY(-2.2deg)` |

La transición base es:

```css
.tx-card {
  transition:
    transform 0.55s var(--ease),
    border-color 0.55s var(--ease),
    box-shadow 0.55s var(--ease);
}
```

Además del movimiento, el hover cambia el filtro de la fotografía (de escala de
grises a un duotono cálido) y resalta el borde con el color de acento.

### Transformaciones 3D

El sitio incluye **dos** ejercicios de 3D:

**a) Tarjeta del hero.** El contenedor `.tilt` aporta la `perspective`; el elemento
interno `.tilt__inner` declara `transform-style: preserve-3d` y rota sobre los dos
ejes. Las capas internas (anillos, año y firma) se separan con `translateZ`, lo que
produce paralaje real al mover el cursor.

```css
.tilt {
  perspective: 1000px;
}

.tilt__inner {
  transform-style: preserve-3d;
  transform: rotateX(9deg) rotateY(-13deg);
  transition: transform 0.75s var(--ease);
}

.tilt:hover .tilt__inner {
  transform: rotateX(-7deg) rotateY(11deg);
}
```

**b) Tarjeta que gira (flip).** En la sección *Estudio*, `.flip` aporta la
perspectiva y `.flip__inner` conserva el 3D para que el reverso exista en el espacio:

```css
.flip {
  perspective: 1200px;
}

.flip__inner {
  transform-style: preserve-3d;
  transition: transform 0.9s var(--ease);
}

.flip:hover .flip__inner,
.flip__inner:focus-visible {
  transform: rotateY(180deg);
}

.flip__face {
  backface-visibility: hidden;
}

.flip__face--back {
  transform: rotateY(180deg);
}
```

---

## 5. Animaciones aplicadas

Todas las animaciones son CSS puro, sin JavaScript.

| Animación | `@keyframes` | Elemento | Función |
| --- | --- | --- | --- |
| Giro del spinner | `girar` | Loader y botón *Enviar* | `rotate(0 → 360deg)` infinito |
| Salida del loader | `preloader-out` | `.preloader` | Funde y oculta la capa de carga |
| Entrada del hero | `subir` | Titular, párrafo y botones | Aparecen desde abajo, escalonados con `animation-delay` |
| Banda infinita | `desplazar` | `.band__track` | `translateX(0 → -50%)` en bucle continuo |
| Latido de la señal | `latido` | `.orb` | Cambia tamaño y color de forma alterna |
| Flotación de anillos | `flotar` / `flotar-inner` | `.tilt__ring` | Escala y opacidad en los dos planos 3D |

### Control de las animaciones

La señal de disponibilidad declara las propiedades por separado para que la
duración, la iteración y la dirección se lean con claridad:

```css
.orb {
  animation-name: latido;
  animation-duration: 3.4s;
  animation-timing-function: cubic-bezier(0.45, 0, 0.55, 1);
  animation-iteration-count: infinite;
  animation-direction: alternate;
  animation-fill-mode: both;
}

@keyframes latido {
  0% {
    transform: scale(0.78);
    background-color: var(--accent);
  }

  55% {
    transform: scale(1.1);
    background-color: var(--accent-soft);
  }

  100% {
    transform: scale(0.9);
    background-color: var(--accent-deep);
  }
}
```

El **loader** de entrada no necesita JavaScript: se retira solo porque la animación
tiene `animation-delay` y `animation-fill-mode: forwards`.

```css
.preloader {
  animation-name: preloader-out;
  animation-duration: 0.7s;
  animation-delay: 0.9s;
  animation-fill-mode: forwards;
}
```

La **banda infinita** duplica su contenido en el HTML y traslada el contenedor
exactamente la mitad de su ancho, de modo que el bucle no muestra saltos.

---

## 6. Estructura con Bootstrap 5

- **Navbar** `navbar-expand-lg` con `bg-dark`: colapsa en menú hamburguesa por
  debajo de 992 px.
- **Grid**: `container`, `row` y columnas `col-sm-12`, `col-md-6`, `col-lg-4` y
  `col-lg-5` / `col-lg-7` para repartir el contenido.
- **Formulario**: `form-label`, `form-control`, `mb-3` y los campos Nombre, Email
  y Mensaje.
- **Botones**: `btn btn-primary`, `btn btn-secondary` y `btn-outline-light`,
  organizados con `d-grid gap-2`.

## 7. Responsividad

| Rango | Comportamiento |
| --- | --- |
| ≥ 1200 px | Hero a dos columnas, tres tarjetas por fila con desfase vertical |
| 992 – 1199 px | Igual, con la tarjeta 3D a 460 px |
| 768 – 991 px | Hero en una columna, menú hamburguesa, tarjetas a dos columnas |
| < 768 px | Una columna, botones a ancho completo, formulario apilado |
| < 576 px | Márgenes y tipografía reducidos |

El documento no genera desbordamiento horizontal en ninguno de esos rangos
(verificado de 320 px a 1440 px).

## 8. Accesibilidad

- Enlace *Saltar al contenido* para navegación por teclado.
- Foco visible en enlaces, botones y campos.
- La tarjeta 3D también gira con el teclado (`:focus-visible`).
- Contraste de texto acorde a WCAG AA sobre el fondo oscuro.
- Bloque `@media (prefers-reduced-motion: reduce)` que desactiva animaciones y
  transiciones, y retira el loader para no bloquear la lectura.

## 9. Autoría

Proyecto académico individual. Los nombres de los proyectos y de la marca son
ficticios y se usan solo con fines demostrativos.
