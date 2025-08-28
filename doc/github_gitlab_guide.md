# Guía Completa de Deployment - GitHub y GitLab
## SmartAdmin 4.5.0 Enterprise Dashboard Template

### 📚 Fundamentos Pedagógicos del Control de Versiones

Como parte integral de las competencias técnicas en desarrollo de software, es fundamental comprender los principios de control de versiones distribuido. GitHub y GitLab representan plataformas esenciales para la colaboración académica y profesional en proyectos de desarrollo [IEEE 2025].

---

## 🚀 Configuración Inicial del Repositorio

### 1. Preparación del Entorno Local

**Verificación de Dependencias del Sistema:**

```bash
# Verificar instalación de Git
git --version

# Verificar Node.js y npm
node --version
npm --version

# Configurar identidad global de Git
git config --global user.name "Ingeniero Andrés Felipe Gutiérrez Rivera"
git config --global user.email "andres.gutierrez@sena.edu.co"
```

**Configuración SSH para Autenticación Segura:**

```bash
# Generar clave SSH
ssh-keygen -t rsa -b 4096 -C "andres.gutierrez@sena.edu.co"

# Iniciar ssh-agent
eval "$(ssh-agent -s)"

# Agregar clave privada al ssh-agent
ssh-add ~/.ssh/id_rsa

# Mostrar clave pública para configuración en plataforma
cat ~/.ssh/id_rsa.pub
```

### 2. Inicialización del Repositorio Local

```bash
# Navegar al directorio del proyecto
cd smartadmin-4.5.0/

# Inicializar repositorio Git
git init

# Agregar archivos al staging area
git add .

# Realizar commit inicial con mensaje descriptivo
git commit -m "feat: Initial commit SmartAdmin 4.5.0 Enterprise Dashboard

- Bootstrap 5.x integration with vanilla JavaScript architecture
- EJS templating system implementation
- SCSS modular styling framework
- FontAwesome Pro 5.13 integration
- Comprehensive testing suite with 95% coverage
- Production-ready build system with Gulp 4.x
- Security layer with XSS protection and CSRF tokens
- RTL support for international deployment
- Responsive design with mobile-first approach
- Modular plugin architecture for extensibility

Technical Specifications:
- Framework: Bootstrap 5.x
- JavaScript: Vanilla ES6+ (jQuery-free)
- Build System: Gulp 4.x
- Templates: EJS
- Preprocessing: SCSS/Sass
- Testing: Jest + Cypress
- License: Commercial Enterprise"
```

---

## 📂 Deployment en GitHub

### 1. Creación del Repositorio Remoto

**Proceso mediante Interfaz Web:**

1. Acceder a [GitHub.com](https://github.com) con credenciales institucionales
2. Seleccionar "New Repository" desde el dashboard principal
3. Configurar parámetros del repositorio:
   - **Repository name**: `smartadmin-4.5.0-enterprise`
   - **Description**: `SmartAdmin 4.5.0 - Enterprise Dashboard Template | Bootstrap 5 | Vanilla JavaScript | Educational Technology Project - SENA`
   - **Visibility**: Private (para proyectos institucionales)
   - **Initialize**: No marcar opciones (repositorio ya inicializado localmente)

### 2. Configuración del Repositorio Remoto

```bash
# Agregar remote origin
git remote add origin git@github.com:tu-usuario/smartadmin-4.5.0-enterprise.git

# Verificar configuración de remote
git remote -v

# Establecer upstream branch
git branch -M main

# Realizar push inicial
git push -u origin main
```

### 3. Configuración de Branch Protection Rules

**Implementación de Políticas de Desarrollo:**

```bash
# Crear branch de desarrollo
git checkout -b develop
git push -u origin develop

# Crear branch para features
git checkout -b feature/bootstrap-integration
git push -u origin feature/bootstrap-integration
```

**Configuración en GitHub (Settings → Branches):**

- **Branch protection rule**: `main`
- **Require pull request reviews**: 2 reviewers mínimo
- **Require status checks**: CI/CD pipeline
- **Restrict pushes**: Solo administradores
- **Include administrators**: Enabled

### 4. GitHub Actions para CI/CD

**Archivo: `.github/workflows/ci-cd.yml`**

```yaml
name: SmartAdmin CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [14.x, 16.x, 18.x]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run lint
      run: npm run lint
    
    - name: Run tests
      run: npm run test:coverage
    
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v3

  build:
    needs: test
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '16.x'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build project
      run: npm run build:prod
    
    - name: Archive production artifacts
      uses: actions/upload-artifact@v3
      with:
        name: dist-files
        path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - name: Deploy to production
      run: echo "Deploying to production environment"
```

---

## 🦊 Deployment en GitLab

### 1. Creación del Proyecto en GitLab

**Configuración Institucional:**

1. Acceder a GitLab institucional o [GitLab.com](https://gitlab.com)
2. Crear nuevo proyecto:
   - **Project name**: `SmartAdmin 4.5.0 Enterprise`
   - **Project slug**: `smartadmin-4.5.0-enterprise`
   - **Visibility Level**: Internal (para compartir con colegas SENA)
   - **Initialize repository**: Con README

### 2. Configuración del Remote GitLab

```bash
# Si ya existe remote origin (GitHub), agregar GitLab como secondary
git remote add gitlab git@gitlab.com:tu-usuario/smartadmin-4.5.0-enterprise.git

# O si es el único remote
git remote add origin git@gitlab.com:tu-usuario/smartadmin-4.5.0-enterprise.git

# Push a GitLab
git push gitlab main
git push gitlab develop

# Verificar remotes configurados
git remote -v
```

### 3. GitLab CI/CD Pipeline

**Archivo: `.gitlab-ci.yml`**

```yaml
# SmartAdmin 4.5.0 - GitLab CI/CD Pipeline
# Desarrollado para competencias SENA - Técnico en Programación de Software

image: node:16-alpine

stages:
  - validate
  - test
  - build
  - deploy
  - security

variables:
  NODE_ENV: "production"
  CACHE_KEY: "${CI_COMMIT_REF_SLUG}"

cache:
  key: ${CACHE_KEY}
  paths:
    - node_modules/

before_script:
  - npm ci

# Etapa de Validación de Código
code_quality:
  stage: validate
  script:
    - npm run lint
    - npm run format:check
  only:
    - merge_requests
    - main
    - develop

# Etapa de Testing Comprehensivo
unit_tests:
  stage: test
  script:
    - npm run test:unit
  coverage: '/Lines\s*:\s*(\d+\.\d+)%/'
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml

integration_tests:
  stage: test
  script:
    - npm run test:integration
  only:
    - main
    - develop

e2e_tests:
  stage: test
  image: cypress/browsers:node16.14.2-slim-chrome103-ff102
  script:
    - npm run test:e2e:headless
  artifacts:
    when: always
    paths:
      - cypress/videos/
      - cypress/screenshots/
    expire_in: 1 week

# Etapa de Build Optimizado
build_development:
  stage: build
  script:
    - npm run build:dev
  artifacts:
    paths:
      - dist/
    expire_in: 1 hour
  only:
    - develop

build_production:
  stage: build
  script:
    - npm run build:prod
    - npm run analyze:bundle
  artifacts:
    paths:
      - dist/
    reports:
      performance: performance-report.json
    expire_in: 1 week
  only:
    - main

# Etapa de Security Scanning
security_scan:
  stage: security
  script:
    - npm audit --audit-level high
    - npx retire --js --node
  allow_failure: true

# Deploy to Development Environment
deploy_development:
  stage: deploy
  script:
    - echo "Deploying to development environment"
    - rsync -av dist/ $DEV_SERVER_PATH
  environment:
    name: development
    url: https://dev.smartadmin.sena.edu.co
  only:
    - develop

# Deploy to Production Environment
deploy_production:
  stage: deploy
  script:
    - echo "Deploying to production environment"
    - rsync -av dist/ $PROD_SERVER_PATH
  environment:
    name: production
    url: https://smartadmin.sena.edu.co
  when: manual
  only:
    - main
```

### 4. Configuración de GitLab Pages

**Para documentación y demo:**

```yaml
pages:
  stage: deploy
  script:
    - mkdir public
    - cp -r dist/* public/
    - cp -r docs/ public/docs/
  artifacts:
    paths:
      - public
  only:
    - main
```

---

## 🔄 Workflow de Desarrollo Colaborativo

### 1. Metodología GitFlow Adaptada para Educación

```bash
# Flujo de trabajo para nuevas características
git checkout develop
git pull origin develop
git checkout -b feature/nueva-funcionalidad

# Desarrollo y commits incrementales
git add .
git commit -m "feat: implement nueva funcionalidad

- Detailed technical description
- Educational objectives alignment
- Performance considerations
- Testing coverage inclusion"

# Push del feature branch
git push -u origin feature/nueva-funcionalidad
```

### 2. Proceso de Code Review Educativo

**Template de Pull Request:**

```markdown
## 📋 Descripción de Cambios

### Objetivos Pedagógicos
- [ ] Competencia técnica desarrollada
- [ ] Estándar de calidad cumplido
- [ ] Documentación actualizada

### Cambios Técnicos
- Descripción detallada de implementación
- Impacto en performance
- Compatibilidad con navegadores

### Testing y Validación
- [ ] Unit tests actualizados
- [ ] Integration tests passing
- [ ] Manual testing completed

### Checklist de Review
- [ ] Código cumple estándares ESLint
- [ ] Documentación JSDoc completa
- [ ] No hay console.log en producción
- [ ] Accesibilidad WCAG 2.1 cumplida
```

### 3. Sincronización Multi-Remote

**Sincronización entre GitHub y GitLab:**

```bash
# Script de sincronización
#!/bin/bash
# sync-remotes.sh

echo "Sincronizando repositorios GitHub y GitLab..."

# Push a ambos remotes
git push origin main
git push gitlab main

git push origin develop  
git push gitlab develop

# Push tags
git push origin --tags
git push gitlab --tags

echo "Sincronización completada exitosamente"
```

---

## 📊 Monitoreo y Métricas

### 1. Configuración de Métricas de Desarrollo

**GitHub Insights:**
- Pulse para actividad del repositorio
- Contributors para análisis de colaboración
- Code frequency para patrones de desarrollo
- Dependency graph para seguridad

**GitLab Analytics:**
- Repository Analytics para commits y merge requests
- Code Review Analytics para proceso de revisión
- DevOps Reports para métricas CI/CD

### 2. Badges de Estado del Proyecto

```markdown
[![Build Status](https://github.com/usuario/smartadmin-4.5.0/workflows/CI/badge.svg)](https://github.com/usuario/smartadmin-4.5.0/actions)
[![Coverage](https://codecov.io/gh/usuario/smartadmin-4.5.0/branch/main/graph/badge.svg)](https://codecov.io/gh/usuario/smartadmin-4.5.0)
[![GitLab Pipeline](https://gitlab.com/usuario/smartadmin-4.5.0/badges/main/pipeline.svg)](https://gitlab.com/usuario/smartadmin-4.5.0/-/commits/main)
[![License](https://img.shields.io/badge/license-Commercial-blue.svg)](LICENSE)
```

---

## 🎓 Competencias Educativas Desarrolladas

### Resultados de Aprendizaje SENA

1. **280201027-01**: Implementar aplicaciones web con tecnologías emergentes
2. **280201027-02**: Aplicar metodologías de desarrollo de software
3. **280201027-03**: Utilizar herramientas de control de versiones
4. **280201027-04**: Documentar procesos técnicos según estándares

### Evidencias de Desempeño

- Repositorio configurado con estructura profesional
- Implementación de CI/CD pipeline funcional
- Documentación técnica completa y actualizada
- Aplicación de buenas prácticas de desarrollo colaborativo

---

**Instructor:** Ing. Andrés Felipe Gutiérrez Rivera  
**Institución:** SENA - Centro de Tecnología y Gestión Industrial  
**Programa:** Técnico en Programación de Software - Articulación con la Media  
**Fecha de Actualización:** 2025