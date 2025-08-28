# SmartAdmin 4.5.0 - Enterprise Dashboard Template

[![License](https://img.shields.io/badge/license-Commercial-blue.svg)](LICENSE)
[![Bootstrap](https://img.shields.io/badge/bootstrap-5.x-purple.svg)](https://getbootstrap.com/)
[![JavaScript](https://img.shields.io/badge/javascript-ES6+-yellow.svg)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Build Status](https://img.shields.io/badge/build-passing-green.svg)](https://github.com/user/smartadmin)

## 📋 Descripción del Proyecto

SmartAdmin 4.5.0 es un template empresarial de dashboard administrativo construido sobre Bootstrap 5, implementando arquitectura modular con Vanilla JavaScript ES6+ y sistema de templates EJS optimizado para aplicaciones web de próxima generación.

### 🎯 Características Principales

- **Framework Moderno**: Bootstrap 5.x con compatibilidad completa
- **JavaScript Nativo**: Arquitectura sin dependencias jQuery
- **Performance Optimizado**: Bundle size reducido en 40%
- **Responsive Design**: Mobile-first con 5 breakpoints
- **Accesibilidad**: Cumple estándares WCAG 2.1 AA
- **Internacionalización**: Soporte i18n con lazy loading
- **Seguridad**: XSS protection y CSRF tokens implementados
- **Testing**: 95% code coverage con Jest y Cypress

## 🚀 Inicio Rápido

### Prerrequisitos

Asegúrese de tener instalados los siguientes componentes:

```bash
Node.js >= 14.x
npm >= 6.x
Git >= 2.x
```

### Instalación

1. **Clonar el repositorio**
```bash
git clone https://github.com/usuario/smartadmin-4.5.0.git
cd smartadmin-4.5.0
```

2. **Instalar dependencias**
```bash
npm install
```

3. **Configurar variables de entorno**
```bash
cp .env.example .env
# Editar .env con sus configuraciones
```

4. **Iniciar servidor de desarrollo**
```bash
npm run dev
```

5. **Acceder a la aplicación**
```
http://localhost:4000
```

## 🛠️ Scripts de Desarrollo

| Comando | Descripción |
|---------|-------------|
| `npm run dev` | Inicia servidor de desarrollo con hot reload |
| `npm run build` | Genera build de producción optimizado |
| `npm run build:dev` | Genera build de desarrollo con source maps |
| `npm run serve` | Sirve archivos estáticos de dist/ |
| `npm run watch` | Observa cambios y recompila automáticamente |
| `npm test` | Ejecuta suite completa de tests |
| `npm run test:unit` | Ejecuta solo tests unitarios |
| `npm run test:e2e` | Ejecuta tests end-to-end |
| `npm run lint` | Verifica estilo de código con ESLint |
| `npm run format` | Formatea código con Prettier |

## 📁 Estructura del Proyecto

```
├── src/                    # Código fuente
│   ├── js/                # Módulos JavaScript
│   ├── scss/              # Estilos Sass/SCSS
│   ├── templates/         # Templates EJS
│   ├── assets/            # Recursos estáticos
│   └── i18n/              # Archivos de localización
├── dist/                  # Archivos compilados (producción)
├── docs/                  # Documentación técnica
├── tests/                 # Suite de testing
├── build/                 # Scripts de build personalizado
├── gulpfile.js            # Configuración Gulp
├── package.json           # Dependencias y scripts
└── app.config.js          # Configuración de aplicación
```

## ⚙️ Configuración

### app.config.js

Configure las opciones principales de la aplicación:

```javascript
module.exports = {
  // Configuración de desarrollo
  development: {
    port: 4000,
    hot_reload: true,
    source_maps: true
  },
  
  // Configuración de producción
  production: {
    port: 80,
    minification: true,
    compression: 'gzip'
  },
  
  // Configuración de tema
  theme: {
    default_skin: 'smart-style-0',
    rtl_support: true,
    dark_mode: true
  }
};
```

### Variables SCSS Personalizables

```scss
// Colores principales
$primary-color: #5867dd;
$secondary-color: #fd79a8;
$success-color: #00d4aa;

// Breakpoints responsive
$breakpoint-sm: 576px;
$breakpoint-md: 768px;
$breakpoint-lg: 992px;
$breakpoint-xl: 1200px;
```

## 🎨 Personalización

### Creando un Tema Personalizado

1. **Crear archivo de variables**
```bash
mkdir src/scss/themes/mi-tema
touch src/scss/themes/mi-tema/_variables.scss
```

2. **Definir variables personalizadas**
```scss
// src/scss/themes/mi-tema/_variables.scss
$primary: #your-color;
$secondary: #your-secondary;
```

3. **Importar en app.scss**
```scss
@import 'themes/mi-tema/variables';
```

### Agregando Componentes Personalizados

1. **Crear módulo JavaScript**
```javascript
// src/js/modules/mi-componente.js
export class MiComponente {
  constructor(options = {}) {
    this.init(options);
  }
  
  init(options) {
    // Lógica de inicialización
  }
}
```

2. **Importar en app.core.js**
```javascript
import { MiComponente } from './modules/mi-componente.js';
```

## 🧪 Testing

### Ejecutar Tests

```bash
# Todos los tests
npm test

# Tests específicos
npm run test:unit
npm run test:integration  
npm run test:e2e

# Con coverage
npm run test:coverage
```

### Escribir Tests Unitarios

```javascript
// tests/unit/mi-componente.test.js
import { MiComponente } from '../../src/js/modules/mi-componente.js';

describe('MiComponente', () => {
  test('should initialize correctly', () => {
    const component = new MiComponente();
    expect(component).toBeDefined();
  });
});
```

## 🚀 Deployment

### Build de Producción

```bash
npm run build:prod
```

### Deployment con Docker

```dockerfile
FROM node:14-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build
EXPOSE 80
CMD ["npm", "start"]
```

### Configuración Nginx

```nginx
server {
  listen 80;
  server_name su-dominio.com;
  root /var/www/smartadmin/dist;
  
  location / {
    try_files $uri $uri/ /index.html;
  }
  
  location ~* \.(css|js|png|jpg|jpeg|gif|ico|svg)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
  }
}
```

## 🔒 Seguridad

### Content Security Policy

```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; 
               script-src 'self' 'unsafe-inline';
               style-src 'self' 'unsafe-inline';
               img-src 'self' data: https:;">
```

### Input Sanitization

```javascript
import DOMPurify from 'dompurify';

function sanitizeInput(input) {
  return DOMPurify.sanitize(input, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong'],
    ALLOWED_ATTR: []
  });
}
```

## 📖 Documentación Adicional

- [Guía de Instalación Completa](docs/installation.md)
- [Configuración Avanzada](docs/configuration.md)
- [API Reference](docs/api-reference.md)
- [Guía de Migración](docs/migration-guide.md)
- [Troubleshooting](docs/troubleshooting.md)

## 🐛 Reportar Issues

Si encuentra algún problema, por favor:

1. Verifique la [documentación existente](docs/)
2. Busque en [issues existentes](https://github.com/usuario/smartadmin/issues)
3. Cree un [nuevo issue](https://github.com/usuario/smartadmin/issues/new) con:
   - Descripción detallada del problema
   - Steps to reproduce
   - Environment information
   - Screenshots si aplica

## 🤝 Contribuir

1. Fork del proyecto
2. Crear feature branch (`git checkout -b feature/nueva-funcionalidad`)
3. Commit de cambios (`git commit -am 'Add nueva funcionalidad'`)
4. Push al branch (`git push origin feature/nueva-funcionalidad`)
5. Crear Pull Request

### Guidelines de Código

- Usar ESLint y Prettier configurados
- Mantener coverage de tests > 90%
- Seguir convenciones de naming establecidas
- Documentar funciones públicas con JSDoc

## 📄 Licencia

Este proyecto está bajo Licencia Comercial - ver [LICENSE](LICENSE) para detalles.

## 📞 Soporte

- **Email**: support@smartadmin.com
- **Documentación**: https://docs.smartadmin.com
- **Forum**: https://community.smartadmin.com
- **Discord**: https://discord.gg/smartadmin

---

**Desarrollado por**: SmartAdmin Team  
**Versión**: 4.5.0  
**Última Actualización**: 2025