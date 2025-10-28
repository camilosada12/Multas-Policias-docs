# Multas-Policias-docs

## Estructura del proyecto
Este workspace contiene cinco repositorios independientes:

| Repositorio | Descripción |
|-------------|-------------|
| [Palermo-docs](./Palermo-docs) | Documentación, diagramas de arquitectura y ADRs |
| [Palermo-api](./Palermo-api) | API REST de backend y lógica de negocio |
| [Palermo-portal](./Palermo-portal) | Frontend web para usuarios finales |
| [Palermo-app](./Palermo-app) | Aplicación móvil |
| [Palermo-db](./Palermo-db) | Esquema de base de datos y migraciones |

### Clonar el workspace con submódulos

Este workspace usa **Git Submodules**. Para clonar con todos los repositorios:

```bash
# Clonar el repositorio del workspace con todos los submódulos
git clone --recursive https://github.com/MultasPalermo/Palermo.git
cd Palermo

# O si ya lo clonaste sin --recursive
git clone https://github.com/MultasPalermo/Palermo.git
cd Palermo
git submodule update --init --recursive
```

## Estrategia de ramas

Todos los repositorios siguen una estrategia de **4 entornos**:

- `dev` - Desarrollo
- `qa` - Aseguramiento de Calidad
- `staging` - Preproducción
- `main` - Producción

### Convención de nombres de ramas (HU)

Cada Historia de Usuario (HU) debe tener su propia rama por entorno:

```
HU-{NUMERO}-{ENTORNO}
```

Ejemplos:
- `HU-01-dev`
- `HU-01-qa`
- `HU-01-staging`

### Ramas de release

Solo `main` utiliza ramas de release para despliegues a producción:

```
release.{SPRINT}.{VERSION}
```

Ejemplos: `release.1.1`, `release.1.2`

## Flujo de desarrollo

### Crear rama de funcionalidad

```bash
# Partiendo desde dev
git switch dev
git pull origin dev
git switch -c HU-01-dev
```

### Desarrollar y commitear

```bash
# Realiza cambios
git add .
git commit -m "feat: agregar autenticación de usuarios"
```

### Push y creación de PR

```bash
git push origin HU-01-dev
# Crear Pull Request: HU-01-dev → dev
```

### Progresión por entornos

Tras la aprobación en cada entorno:
- De `dev` → crear `HU-01-qa`
- De `qa` → crear `HU-01-staging`
- De `staging` → incluir en `release.X.Y`

## Reglas importantes

- No commitear directamente a `dev`, `qa`, `staging` o `main`
- Cada HU debe tener ramas para cada entorno por el que pasa
- Siempre solicitar **Code Review** antes de hacer merge
- Mantener ramas **de corta vida** (2-3 días máximo)
- Actualizar **documentación** tras merges a `staging` o `main`
- Cada release debe estar **taggeado** con número de versión

## Arquitectura

Este proyecto utiliza una **arquitectura polyrepo** (múltiples repositorios) en lugar de monorepo (un único repositorio). Beneficios:

- **Despliegue independiente** por módulo
- **Ciclos de versionado** y releases aislados
- **Propiedad y límites claros** por equipo/módulo
- **Tecnologías flexibles** por módulo
- **Menor impacto** ante cambios
