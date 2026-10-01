# seguridad del codigo

## estandar de seguridad

El proyecto sigue los estandares **OWASP**:

* **OWASP Top 10**: los 10 riesgos de seguridad mas criticos de aplicaciones web, como base minima.
* **OWASP ASVS Nivel 1**: estandar de verificacion de seguridad de aplicaciones (nivel suficiente para este proyecto educativo).

## checklist de pruebas de seguridad (basado en OWASP Top 10)

Marcar cada item cuando se verifique:

### A01 - Broken Access Control
- [ ] No existe funcionalidad de administracion accesible sin autenticacion (no aplica aun, verificar al agregarla).
- [ ] Las rutas no exponen datos sensibles en la URL.

### A02 - Cryptographic Failures
- [ ] Todo el trafico hacia servicios REST usa HTTPS (verificar `https://jsonplaceholder.typicode.com`).
- [ ] No se almacenan datos sensibles sin cifrar en el navegador (`localStorage`/`sessionStorage`).

### A03 - Injection (XSS)
- [ ] No se usa `dangerouslySetInnerHTML` en ningun componente.
- [ ] React renderiza todo contenido dinamico escapado por defecto (verificar que no se inserte HTML crudo con `innerHTML`).
- [ ] Las URLs de imagenes (`url`, `thumbnailUrl`) se usan solo en atributos `src`/`alt`, no en codigo ejecutable.

### A04 - Insecure Design
- [ ] El manejo de errores del servicio REST no expone detalles internos (stack traces) al usuario final.
- [ ] Existe un estado de error controlado (`Alert`) en la UI ante fallos del endpoint.

### A05 - Security Misconfiguration
- [ ] No hay endpoints de debug habilitados en produccion.
- [ ] Las cabeceras de seguridad basicas estan configuradas en el servidor de despliegue (CSP, X-Content-Type-Options, X-Frame-Options).

### A06 - Vulnerable and Outdated Components
- [ ] Ejecutar `npm audit` sin vulnerabilidades de severidad alta o critica.
- [ ] Todas las dependencias (`react`, `react-router`, `@mui/*`) estan en versiones soportadas y actualizadas.
- [ ] No hay dependencias sin uso en `package.json`.

### A07 - Identification and Authentication Failures
- [ ] No aplica actualmente (no hay autenticacion). Revisar al agregar login (NIST SP 800-63B: contrasenas fuertes, MFA, limitacion de intentos).

### A08 - Software and Data Integrity Failures
- [ ] No se cargan scripts desde CDNs no confiables.
- [ ] El `package-lock.json` esta versionado para asegurar instalaciones reproducibles.

### A09 - Security Logging and Monitoring Failures
- [ ] Los errores de consumo del servicio REST se registran en consola (`console.error`) para diagnostico.
- [ ] No se registran datos sensibles en los logs.

### A10 - Server-Side Request Forgery (SSRF)
- [ ] El frontend solo consume las URLs de endpoints definidas en la capa de servicios (no URLs arbitrarias del usuario).

## practicas adicionales del proyecto

- [ ] No hardcodear API keys ni secretos en el codigo fuente.
- [ ] Usar variables de entorno (`.env`) y agregarlo a `.gitignore`.
- [ ] Ejecutar `npm audit` regularmente (script `npm run audit`).
- [ ] Validar el tipado de los datos recibidos del REST contra el modelo `Photo` (TypeScript).

