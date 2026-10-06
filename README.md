# TFI Frontend — Tienda y panel administrativo

Interfaz académica de comercio electrónico desarrollada con React. Permite
consultar productos, utilizar un carrito, registrarse e iniciar sesión. Incluye
un panel para administrar productos y consultar pedidos y sus estados.
Los datos y la autenticación dependen de la API del backend.

**Repositorios:** [Frontend](https://github.com/raguirre453525/TFI-frontend) ·
[Backend ASP.NET Core](https://github.com/raguirre453525/TFI-backend).

## Tecnologías y estructura

Versiones resueltas por `package-lock.json`:

| Tecnología | Versión |
| --- | --- |
| React | 19.1.1 |
| Vite | 7.1.12 |
| React Router | 7.9.4 |
| Axios | 1.13.2 |
| Tailwind CSS | 4.1.14 |
| React Hook Form | 7.65.0 |

```text
package.json                  Dependencias y comandos
package-lock.json             Versiones reproducibles para npm ci
src/modules/auth/             Registro, inicio de sesión y contexto de autenticación
src/modules/products/         Catálogo y administración de productos
src/modules/cart/             Carrito
src/modules/orders/           Creación y consulta de pedidos y actualización de estado
src/modules/home/             Inicio, navegación y panel
src/modules/shared/           Componentes y cliente Axios compartido
src/modules/templates/        Estructura del panel administrativo
public/                       Recursos públicos
vite.config.js                React, Tailwind y proxy de desarrollo
.env.development              URL pública del backend local
```

## Requisitos

- Node.js 22.12 o posterior y npm. Vite también admite Node.js 20 desde 20.19.
- Backend ejecutándose con su base de datos configurada, según su propio README.
- Certificado HTTPS de desarrollo del backend confiable en el navegador.

## Ejecutar localmente

Desde la raíz de este repositorio:

```powershell
npm ci
npm run dev -- --host localhost --port 5173 --strictPort
```

Abrir **http://localhost:5173**. La API permite este origen exacto mediante CORS;
`http://127.0.0.1:5173` u otro puerto no son equivalentes. `--strictPort` evita
que Vite cambie silenciosamente a un puerto que el backend no permite.

La configuración pública incluida en `.env.development` es:

```dotenv
VITE_BACKEND_URL=https://localhost:7138/
```

Si cambia la dirección de la API, configurar `VITE_BACKEND_URL` en el entorno o
crear un archivo local no versionado, como `.env.development.local`, y reiniciar Vite.
Mantener la barra final y verificar que el backend permita el origen del frontend.

El cliente de `src/modules/shared/api/axiosInstance.js` usa esa URL absoluta;
las solicitudes actuales no pasan por el proxy `/api` de `vite.config.js`.
Abrir la API en el navegador y comprobar el certificado antes de iniciar sesión.
No colocar claves JWT, contraseñas ni otros secretos en variables `VITE_*`:
sus valores quedan expuestos al navegador.

Registro, inicio de sesión y operaciones sobre productos o pedidos requieren
la API disponible. Las cuentas administrativas se preparan en el backend;
no existe una cuenta predeterminada documentada en este repositorio.

## Compilación y comprobaciones

El build usa el modo `production`, por lo que no carga `.env.development`.
Definir la URL pública de la API antes de compilar. Para una vista previa local:

```powershell
npm run lint
$env:VITE_BACKEND_URL = 'https://localhost:7138/'
npm run build
npm run preview -- --host localhost --port 5173 --strictPort
```

Para otro entorno, reemplazar esa URL antes del build. Vite incorpora su valor
en la compilación; cambiarlo al ejecutar `preview` no modifica los archivos generados.

- `build` genera `dist/`; no debe versionarse. `preview` sirve esa compilación
  localmente y no sustituye un servidor de producción.
- El proyecto no tiene un script `npm test` ni una suite de pruebas automatizadas.
- El lint tiene deuda de formato registrada. Su resultado es independiente del
  build: compilar no demuestra que el lint ni los flujos funcionales pasen.
- Esta separación conserva la aplicación existente; no implica nuevas verificaciones
  de compilación o ejecución ni resuelve su deuda funcional y de autorización.

Es un proyecto académico, no una aplicación preparada para producción. La
autorización debe mantenerse en la API; ocultar una pantalla no protege los datos.
