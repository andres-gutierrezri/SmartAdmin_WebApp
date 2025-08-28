# SmartAdmin 4.5.0 Project Structure

```
smartadmin-4.5.0/
├── README.md
├── .gitignore
├── package.json
├── gulpfile.js
├── app.config.js
├── docs/
│   ├── installation.md
│   ├── configuration.md
│   ├── components.md
│   ├── migration-guide.md
│   └── api-reference.md
├── src/
│   ├── js/
│   │   ├── app.core.js
│   │   ├── modules/
│   │   │   ├── navigation.js
│   │   │   ├── panels.js
│   │   │   ├── themes.js
│   │   │   └── utils.js
│   │   └── plugins/
│   │       ├── smartpanel.js
│   │       ├── datatable.js
│   │       ├── charts.js
│   │       └── forms.js
│   ├── scss/
│   │   ├── app.scss
│   │   ├── themes/
│   │   │   ├── light/
│   │   │   ├── dark/
│   │   │   └── custom/
│   │   ├── components/
│   │   └── utilities/
│   ├── templates/
│   │   ├── layouts/
│   │   ├── partials/
│   │   └── pages/
│   ├── assets/
│   │   ├── img/
│   │   ├── fonts/
│   │   └── icons/
│   └── i18n/
│       ├── en.json
│       ├── es.json
│       └── fr.json
├── dist/
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── build/
└── node_modules/

## Key Files Description

### package.json
- Project dependencies and scripts
- Development and production configurations
- Testing and build automation commands

### gulpfile.js
- Build automation tasks
- SCSS compilation and minification
- JavaScript bundling and optimization
- Asset optimization and copying

### app.config.js
- Application configuration
- Theme settings and customization
- Plugin initialization parameters
- Environment-specific variables

### src/js/app.core.js
- Main application entry point
- Module initialization and dependency management
- Event system and lifecycle management
- Global utility functions

### src/scss/app.scss
- Main stylesheet entry point
- Theme imports and configurations
- Component style orchestration
- Responsive breakpoint definitions

### dist/
- Production-ready compiled assets
- Minified CSS and JavaScript files
- Optimized images and fonts
- Generated documentation files
```

## Project Architecture Diagram

```
┌─────────────────────────────────────────┐
│              SmartAdmin 4.5.0           │
├─────────────────────────────────────────┤
│  Bootstrap 5 Framework                  │
├─────────────────────────────────────────┤
│  Vanilla JavaScript ES6+ Modules       │
├─────────────────────────────────────────┤
│  SCSS/Sass Preprocessing               │
├─────────────────────────────────────────┤
│  EJS Template Engine                    │
├─────────────────────────────────────────┤
│  Gulp Build System                      │
├─────────────────────────────────────────┤
│  Testing Framework (Jest + Cypress)     │
└─────────────────────────────────────────┘
```

## Development Workflow

1. **Development Setup**
   ```bash
   npm install
   npm run dev
   ```

2. **Build Process**
   ```bash
   npm run build:dev    # Development build
   npm run build:prod   # Production build
   ```

3. **Testing Commands**
   ```bash
   npm test             # Run all tests
   npm run test:unit    # Unit tests only
   npm run test:e2e     # End-to-end tests
   ```

4. **Development Server**
   ```bash
   npm run serve        # Local development server
   npm run watch        # File watching and auto-reload
   ```