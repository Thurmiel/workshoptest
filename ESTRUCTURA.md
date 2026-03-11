# Estructura del Repositorio

```
.
├── .github/
│   └── workflows/
│       └── build.yml                 # 🔨 GitHub Actions - Construcción automática (hay también builder.yaml con el flujo oficial)
├── example/                          # 📦 Addon principal
│   ├── apparmor.txt                  # AppArmor security profile (opcional)
│   ├── build.yaml                    # Configuración de build de Home Assistant
│   ├── CHANGELOG.md                  # Historial de cambios del addon
│   ├── config.yaml                   # Configuración principal del addon
│   ├── Dockerfile                    # Definición de imagen Docker
│   ├── DOCS.md                       # Documentación del addon
│   ├── README.md                     # README del addon
│   ├── translations/
│   │   └── en.yaml                   # Traducciones (inglés)
│   └── rootfs/                       # Sistema de archivos del contenedor
│       ├── etc/
│       │   └── services.d/
│       │       └── example/
│       │           ├── finish        # Script de finalización (s6-overlay)
│       │           └── run           # Script de inicio (s6-overlay)
│       └── usr/
│           └── bin/
│               └── my_program        # Programa principal
├── .gitignore                        # Archivos a ignorar en Git
├── CAMBIOS.md                        # 📝 Resumen de cambios realizados
├── DEVELOPMENT.md                    # 🔧 Guía de desarrollo local
├── LICENSE                           # Licencia del proyecto
├── README.md                         # 📖 Documentación principal
├── repository.yaml                   # Configuración del repositorio
└── SETUP_GITHUB.md                   # 🚀 Guía de configuración GitHub
```

## Archivos Principales Explicados

### **repository.yaml**
- Define el repositorio completo en Home Assistant
- Información del mantenedor
- Punto de entrada para instalar el addon

### **example/config.yaml**
- Define el addon en Home Assistant
- Nombre, slug, versión, descripción
- Permisos requeridos
- Mapeos de volúmenes
- Opciones de configuración del usuario

### **example/build.yaml**
- Configuración de build de Docker
- Imagen base a usar
- Versiones de dependencias
- Configuraciones de seguridad (cosign)

### **example/Dockerfile**
- Define la imagen Docker del addon
- Instala dependencias de sistema
- Copia archivos del rootfs
- Se ejecuta solo en contexto de compilación

### **example/rootfs/**
- Archivos que se copian al contenedor
- Estructura de directorios Linux estándar
- Scripts de inicio/parada
- Programa principal

### **.github/workflows/build.yml**
- Automatiza builds en GitHub
- Se ejecuta en cada push a `main`
- Construye para amd64 y aarch64
- Publica en GitHub Container Registry

### **SETUP_GITHUB.md**
- Pasos para configurar GitHub Actions
- Cómo hacer pública la imagen
- Cómo compartir el addon

### **DEVELOPMENT.md**
- Cómo desarrollar localmente
- Cómo usar los tasks de VS Code
- Cómo debuggear
- Checklist antes de publicar
