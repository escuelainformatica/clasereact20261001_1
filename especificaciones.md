

# especificaciones

Necesitamos crear un proyecto en react que pueda listar los datos desde un servicio REST utilizando fetch.

## requerimientos tecnicos
[requerimientos tecnicos](requerimientos.md)

## pruebas de codigo
[pruebas de codigo](pruebas.md)

## seguridad del codigo
[seguridad del codigo](seguridad.md)

## principios de codigo
[principios de codigo](principio.md)

## diseno
El diseño visual del sitio
[diseno](diseno.md)


## estructura del proyecto

* modelos:
  - Photo: representa una foto con sus detalles (id, title, url, thumbnailUrl,albumId)
* servicios: contiene las funciones para consumir los endpoints REST del proyecto.
  - PhotoService: contiene las funciones para interactuar con el endpoint de fotos.
* pagina:
  - PhotoPage: pagina que muestra la lista de fotos obtenidas desde el servicio REST.  No olvide agregar la ruta `/` en el enrutador.  Para esta página, use el componente PhotoList
* componentes:  
  - PhotoList: componente que muestra la lista de fotos en formato de cuadrícula o lista.  Para este componente, use PhotoItem y PhotoHeader.
  - PhotoItem: componente que muestra los detalles de una sola foto.
  - PhotoHeader: componente que muestra el encabezado de la lista de fotos.

## modelo

### Photo
| Campo | Tipo | Descripcion |
|-------|------|-------------|
| id | number | Identificador unico de la foto |
| title | string | Titulo de la foto |
| url | string | URL de la foto |
| thumbnailUrl | string | URL del thumbnail de la foto |
| albumId | number | Identificador del album al que pertenece la foto |

## servicios

### PhotoService
Contiene las funciones para interactuar con el endpoint de fotos.

#### obtenerFotos
- Descripcion: Obtiene la lista de fotos desde el endpoint REST.
- Retorno: Promise<Photo[]>
- Comportamiento en caso de error:
  - Si la respuesta HTTP no es ok (ej: status 500, 404), rechaza la promesa con un `Error` cuyo mensaje es descriptivo e incluye el codigo de estado (ej: "Error al obtener las fotos: 500").
  - Si la respuesta no es un JSON valido, rechaza con un `Error` indicando que la respuesta no pudo ser procesada.
  - Si falla la red (sin conexion), envuelve el error nativo de fetch en un mensaje amigable.
  - El mensaje de error no debe exponer detalles internos como stack traces (ver seguridad A04).
  - Quien consume el servicio (PhotoPage) captura el error con try/catch y pasa el mensaje al estado `error` para mostrarlo en el `Alert` de PhotoList.

## paginas

### PhotoPage
Pagina que muestra la lista de fotos obtenidas desde el servicio REST. No olvide agregar la ruta `/` en el enrutador.

## componentes

### PhotoList
Componente que muestra la lista de fotos en formato de cuadrícula o lista. Utiliza PhotoItem y PhotoHeader.

* Props:
  - `photos: Photo[]` - lista de fotos a mostrar.
  - `loading: boolean` - indica si la carga esta en progreso.
  - `error: string | null` - mensaje de error cuando la peticion falla.
* Comportamiento:
  - Mientras `loading` sea `true`, muestra un `CircularProgress` centrado.
  - Si `error` tiene valor, muestra un `Alert` (severity="error") con el mensaje.
  - Si la lista esta vacia y no hay error, muestra un `Typography` con el texto "No hay fotos disponibles".
  - En estado normal, renderiza el encabezado (PhotoHeader) seguido de una cuadricula (`Grid2`) de items (PhotoItem).
* Componentes MUI sugeridos: `Grid2`, `Box`, `CircularProgress`, `Alert`.

### PhotoItem
Componente que muestra los detalles de una sola foto.

* Props:
  - `photo: Photo` - la foto a mostrar.
* Comportamiento:
  - Renderiza una tarjeta (`Card`) con la imagen thumbnail (`thumbnailUrl`) usando el componente `CardMedia`.
  - Muestra el `title` en `CardContent` con `Typography` (variant="subtitle2") truncado a 2 lineas (`noWrap` o `lineClamp`).
  - Al hacer clic en la tarjeta, abre la imagen en tamaño completo (`url`) en un `Dialog` con `DialogContent`.
* Componentes MUI sugeridos: `Card`, `CardActionArea`, `CardMedia`, `CardContent`, `Typography`, `Dialog`, `DialogContent`.

### PhotoHeader
Componente que muestra el encabezado de la lista de fotos.

* Props:
  - `title: string` - titulo del encabezado (ej: "Lista de Fotos").
  - `count: number` - cantidad total de fotos mostradas.
* Comportamiento:
  - Renderiza un `Typography` (variant="h4") con el titulo.
  - Muestra un `Chip` con el contador de fotos (ej: "500 fotos").
* Componentes MUI sugeridos: `Typography`, `Chip`, `Box`.

## servicios REST

A continuacion se detallan los endpoints que el proyecto debe consumir:

* URL: https://jsonplaceholder.typicode.com/photos
  - Metodo: GET
  - Descripcion: Obtiene una lista de fotos con sus detalles (id, title, url, thumbnailUrl)

Ejemplo de respuesta:
    
```json
[
  {
    "albumId": 1,
    "id": 1,
    "title": "accusamus beatae ad facilis cum similique qui sunt",
    "url": "https://picsum.photos/seed/1/600",
    "thumbnailUrl": "https://picsum.photos/seed/1/150"
  },
  {
    "albumId": 1,
    "id": 2,
    "title": "reprehenderit est deserunt velit ipsam",
    "url": "https://picsum.photos/seed/2/600",
    "thumbnailUrl": "https://picsum.photos/seed/2/150"
  }
]
```