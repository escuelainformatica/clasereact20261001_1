# diseño

**El diseño es responsivo (mobile-first)**: la interfaz se adapta a cualquier tamano de pantalla (movil, tablet y desktop) utilizando el sistema de breakpoints y cuadricula de MUI, sin scroll horizontal ni elementos desbordados.

Para el diseño del proyecto, se utilizará Material-UI (MUI) para los componentes de la interfaz de usuario, asegurando un diseño consistente y responsivo. Se seguirán las pautas de diseño de MUI y se aplicarán estilos personalizados cuando sea necesario.

## tema (Theme)

* Crear un tema personalizado con `createTheme` y aplicarlo globalmente con `ThemeProvider` en `root.tsx`.
* Paleta de colores:
  - Fondo principal (`background.default`): blanco (`#ffffff`).
  - Superficie de tarjetas (`background.paper`): blanco con elevacion via `BoxShadow`.
  - Color primario (`primary.main`): color primario por defecto de MUI (indigo) para elementos interactivos (botones, links, chips activos).
  - Color de error (`error.main`): para alertas y mensajes de fallo.
* Habilitar `CssBaseline` para normalizar estilos base entre navegadores.

## tipografia

* Usar la fuente por defecto de MUI (Roboto) cargada desde `@fontsource/roboto` o Google Fonts.
* Jerarquia de titulos:
  - `h4`: titulo principal de la pagina (PhotoHeader).
  - `subtitle2`: titulo de cada foto en las tarjetas (PhotoItem).
  - `body2`: textos secundarios y contadores.

## espaciado y layout

* Usar la escala de espaciado de MUI (multiplos de 8px: `spacing(1)` = 8px).
* El contenido principal tendra un ancho maximo de `lg` (1200px) centrado con `Container`.
* Cuadricula de fotos (`Grid2`):
  - Movil (`xs`): 1 columna.
  - Tablet (`sm`): 2 columnas.
  - Desktop (`md` o superior): 3 o 4 columnas.
  - Separacion entre items: `spacing={2}` (16px).

## estados visuales

* Carga (`loading`): `CircularProgress` centrado vertical y horizontalmente en un `Box` con altura minima.
* Error: `Alert` con `severity="error"` mostrando el mensaje al usuario.
* Lista vacia: mensaje centrado con `Typography` ("No hay fotos disponibles").
* Imagenes: usar `component="img"` en `CardMedia` con `loading="lazy"` para optimizar la carga.

## responsividad

El diseño debe ser completamente responsivo con enfoque **mobile-first**:

* Todos los layouts se construyen primero para movil y luego se amplian con breakpoints (`xs` → `sm` → `md` → `lg`).
* Probar en 3 breakpoints principales: movil (375px), tablet (768px) y desktop (1440px).
* El `PhotoHeader` se apila verticalmente en movil (titulo arriba, contador abajo) y se alinea horizontal en desktop.
* La cuadricula de fotos (`Grid2`) cambia de columnas segun el tamano: 1 en `xs`, 2 en `sm`, 3-4 en `md`+.
* No debe haber scroll horizontal en ningun breakpoint; usar `Container` con `maxWidth` para limitar el contenido en pantallas grandes.
* Las imagenes deben ser fluidas (`width: 100%`, `height: auto`) para escalar con su contenedor.

## accesibilidad

* Todas las imagenes incluyen el atributo `alt` con el `title` de la foto.
* Los elementos interactivos (tarjetas clickeables) deben ser alcanzables por teclado (`CardActionArea` lo provee).
* Contraste de texto conforme a WCAG AA usando los colores por defecto de MUI.
