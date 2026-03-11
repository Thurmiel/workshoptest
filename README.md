# Tu Repositorio de Add-ons para Home Assistant

Repositorio con add-ons personalizados para Home Assistant.

**Documentación de add-ons**: <https://developers.home-assistant.io/docs/add-ons>

## 📦 Instalación

### Opción 1: Agregar repositorio desde la UI (Recomendado)

Haz clic en este badge para agregar el repositorio automáticamente:

[![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2FTUSUARIO%2FTUREPO)

O agrega manualmente:
1. Ve a **Settings > Add-ons & automations > Add-ons > Repositories**
2. Pega esta URL: `https://github.com/TUSUARIO/TUREPO`
3. Presiona Enter y espera a que se agregue
4. Ahora verás este repositorio en la lista de add-ons disponibles

### Opción 2: Instalación desde archivo

Si clonaste el repositorio localmente:

```bash
git clone https://github.com/TUSUARIO/TUREPO.git
cd TUREPO
```

## 📋 Add-ons Disponibles

### [Example add-on](./example)

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

Add-on de ejemplo con funcionalidades básicas.

**Descripción**: Ejemplo de add-on funcional para Home Assistant

## 🛠️ Desarrollo

### Requisitos
- Docker
- Home Assistant Supervisor o Home Assistant Core

### Pasos para desarrollo local

1. **Clona el repositorio**:
   ```bash
   git clone https://github.com/TUSUARIO/TUREPO.git
   cd TUREPO
   ```

2. **Durante desarrollo, comenta la línea `image` en `example/config.yaml`**:
   ```yaml
   # image: "ghcr.io/homeassistant/{arch}-addon-example:latest"
   ```
   Esto permite que el supervisor construya el add-on localmente.

3. **Construye y prueba usando los tasks de VS Code**:
   - "Start Home Assistant" - Inicia el supervisor
   - "Rebuild and Start Addon" - Reconstruye y reinicia el add-on
   - "Start Addon" - Solo inicia el add-on sin reconstruir

### Antes de hacer push a GitHub

1. **Descomenta la línea `image` en `example/config.yaml`**
2. **Actualiza la versión** en `example/config.yaml` si hay cambios
3. **Actualiza `example/CHANGELOG.md`** con los cambios realizados
4. **Haz commit y push** a tu rama

### Configuración de GitHub Actions

Para que se construyan automáticamente las imágenes Docker:

1. Ve a **Settings > Actions > General**
2. En "Workflow permissions" selecciona **"Read and write permissions"**
3. Marca **"Allow GitHub Actions to create and approve pull requests"**

## 🚀 Construcción y Publicación

Las imágenes Docker se construyen automáticamente cuando haces push a `main`:

1. Las builds se publican en `ghcr.io/TUSUARIO/ADDON_SLUG:VERSION`
2. Asegúrate que el paquete sea **público** en GitHub Container Registry
3. El addon se instala automáticamente desde la última versión publicada

## 📝 Personalización

Para tu primer add-on, realiza estos cambios:

1. Reemplaza `TUSUARIO` con tu usuario de GitHub
2. Reemplaza `TUREPO` con el nombre de tu repositorio
3. Actualiza `repository.yaml` con tu nombre y email
4. Modifica `example/config.yaml`:
   - `name`: nombre visible del add-on
   - `slug`: identificador único
   - `url`: URL de tu repositorio
5. Renueva el directorio `example/` según sea necesario

## 📧 Soporte

Para reportar problemas o sugerencias, abre un issue en GitHub.

---

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
