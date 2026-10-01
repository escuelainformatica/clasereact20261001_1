# pruebas de codigo

Estrategia de pruebas del proyecto basada en las librerias definidas en (requerimientos.md)[requerimientos.md]: Vitest (runner), React Testing Library (componentes) y MSW (mock del servicio REST).

## alcance

| Tipo | Objetivo | Herramientas |
|------|----------|--------------|
| Unitarias | Probar componentes y funciones aislados | Vitest + React Testing Library |
| Integracion | Probar el flujo completo UI + servicio REST mockeado | Vitest + RTL + MSW |

## estructura de archivos

```
src/
  __tests__/
    modelos/
      photo.test.ts
    servicios/
      photoService.test.ts        (integracion con MSW)
    componentes/
      PhotoItem.test.tsx          (unitaria)
      PhotoHeader.test.tsx        (unitaria)
      PhotoList.test.tsx          (unitaria + integracion)
    paginas/
      PhotoPage.test.tsx          (integracion)
  mocks/
    handlers.ts                   (handlers de MSW)
    server.ts                     (setup del servidor MSW)
```

## casos de prueba

### pruebas unitarias

#### modelo Photo (`photo.test.ts`)
- [ ] Crea una instancia de `Photo` con todos los campos (id, title, url, thumbnailUrl, albumId).
- [ ] TypeScript valida el tipado: un objeto con campos faltantes genera error de compilacion.

#### PhotoHeader (`PhotoHeader.test.tsx`)
- [ ] Renderiza el titulo recibido en props.
- [ ] Renderiza el contador de fotos (ej: "500 fotos").
- [ ] El titulo usa la variante tipografica `h4`.

#### PhotoItem (`PhotoItem.test.tsx`)
- [ ] Renderiza el `title` de la foto.
- [ ] La imagen usa `thumbnailUrl` en el atributo `src`.
- [ ] La imagen incluye `alt` con el titulo de la foto.
- [ ] Al hacer clic en la tarjeta, abre el `Dialog` con la imagen en tamano completo (`url`).
- [ ] Al cerrar el dialog, la imagen completa desaparece del DOM.

#### PhotoList (`PhotoList.test.tsx`) - estados puros
- [ ] Con `loading=true`, muestra el `CircularProgress` y no renderiza items.
- [ ] Con `error` con valor, muestra el `Alert` de error con el mensaje.
- [ ] Con `photos=[]` y sin error, muestra el mensaje "No hay fotos disponibles".
- [ ] Con fotos, renderiza un `PhotoItem` por cada foto recibida.

### pruebas de integracion

#### PhotoService (`photoService.test.ts`) - con MSW
- [ ] `obtenerFotos()` resuelve con un arreglo de `Photo` cuando el endpoint responde 200.
- [ ] Los datos retornados mapean correctamente los campos del modelo (id, title, url, thumbnailUrl, albumId).
- [ ] `obtenerFotos()` rechaza con mensaje de error cuando el endpoint responde 500.
- [ ] `obtenerFotos()` rechaza cuando la respuesta no es JSON valido.
- [ ] La peticion se realiza contra `https://jsonplaceholder.typicode.com/photos` (verificar URL del handler).

#### PhotoList + PhotoPage (`PhotoList.test.tsx`, `PhotoPage.test.tsx`) - con MSW
- [ ] Mientras carga, muestra el spinner; al resolver MSW, renderiza las fotos mockeadas.
- [ ] Si MSW responde con error, la pagina muestra el `Alert` de error.
- [ ] El `PhotoHeader` muestra el conteo correcto segun las fotos retornadas por el servicio.
- [ ] Flujo completo: cargar -> renderizar lista -> clic en item -> dialog con imagen completa.

## configuracion MSW

- [ ] Crear `src/mocks/handlers.ts` con el handler `http.get('https://jsonplaceholder.typicode.com/photos', ...)` que retorna fotos de prueba.
- [ ] Crear `src/mocks/server.ts` con `setupServer(...handlers)`.
- [ ] Configurar `beforeAll`/`afterEach`/`afterAll` para iniciar, resetear y detener el servidor en el setup global.
- [ ] Incluir un fixture de 2-3 fotos de prueba reutilizable.

## criterios de aceptacion

- [ ] Todas las pruebas pasan con `npm test` (o `npx vitest`).
- [ ] Cobertura minima objetivo: 80% en componentes y servicios (`npm run test:coverage`).
- [ ] Ninguna prueba depende de la red real: todo el REST va por MSW.
- [ ] Las pruebas usan queries accesibles (`getByRole`, `getByAltText`, `getByText`), no selectores de implementacion.
