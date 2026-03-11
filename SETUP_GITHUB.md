# 🚀 Guía de Configuración para GitHub

Sigue estos pasos para que tu addon sea completamente funcional y publicable:

## 1️⃣ Preparación Inicial

> ⚠️ **Nota importante:** GitHub usa mayúsculas en tu nombre de usuario, pero
docker/ghcr requiere que los repositorios sean *lowercase*. Si tu usuario
contiene letras mayúsculas, el proceso de build local puede fallar con un
error similar a:
>
> `invalid tag "ghcr.io/Thurmiel/example:latest": repository name must be lowercase`
>
> En tal caso, reemplaza el nombre de usuario en `repository.yaml` y en las
> URLs por su versión en minúsculas (por ejemplo, `thurmiel`). El workflow de
> GitHub Actions ya convierte a minúsculas automáticamente.

### Personaliza los archivos

1. **repository.yaml** - Actualiza con tu información:
   ```yaml
   name: Tu Nombre - Add-ons
   url: 'https://github.com/TU_USUARIO/TU_REPO'
   maintainer: Tu Nombre <tu@email.com>
   ```

2. **example/config.yaml** - Cambia referencias:
   - `url`: Apunta a tu repositorio
   - Otras personalizaciones según necesites

3. **README.md** - Reemplaza:
   - `TUSUARIO` → Tu usuario de GitHub
   - `TUREPO` → Tu nombre de repositorio

## 2️⃣ Configuración de GitHub

### Paso A: Configurar Permisos de Actions

1. Ve a tu repositorio en GitHub
2. **Settings > Actions > General**
3. En "Workflow permissions":
   - ✅ Selecciona **"Read and write permissions"**
   - ✅ Marca **"Allow GitHub Actions to create and approve pull requests"**
4. Presiona **Save**

### Paso B: Configurar GitHub Container Registry

1. Ve a **Settings > Actions > Secrets and variables > Actions**
2. Asegúrate que ya existe `GITHUB_TOKEN` (creado automáticamente)
3. No necesitas crear secrets adicionales

## 3️⃣ Preparar el Primer Push

### En tu máquina local:

1. **Inicializa Git** (si no lo hiciste):
   ```bash
   cd tu_carpeta_workspace
   git init
   git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
   ```

2. **Crea rama main y haz el primer commit**:
   ```bash
   git checkout -b main
   git add .
   git commit -m "Initial commit: setup addon repository"
   git push -u origin main
   ```

## 4️⃣ Después del Primer Push

### La Action se ejecutará automáticamente:

1. Ve a **Actions** en tu repositorio
2. Verás al menos uno de los workflows:
   - **Build Home Assistant Add-ons** (construcción manual simple)
   - **Builder** (usa el builder oficial de Home Assistant)

> Nota: el archivo `builder.yaml` ya está incluido y emplea la acción oficial
> `home-assistant/builder`. Es el método recomendado para proyectos que siguen
> las guías de Home Assistant. Puedes conservar o eliminar `build.yml` según tu preferencia.

3. Espera a que termine (puede tomar 10-15 minutos)

### Si el build falla:

- Revisa los logs del workflow
- Los errores más comunes:
  - Dockerfile tiene sintaxis incorrecta
  - Faltan dependencias en apt-get
  - Permisos de lectura/escritura insuficientes

## 5️⃣ Hacer la Imagen Pública

Después del primer build exitoso:

1. Ve a tu perfil de GitHub
2. **Packages and registries > Container registry**
3. Busca tu addon (ej: `example`)
4. Abre el paquete
5. **Package settings** (esquina superior derecha)
6. En "Danger zone", cambia a **Public**

## 6️⃣ Compartir tu Addon

### Para que otros lo instalen:

Tu URL será: `https://github.com/TU_USUARIO/TU_REPO`

Los usuarios pueden:
1. Agregar tu repositorio manualmente en Home Assistant
2. O usar el badge (actualiza el README)

### Badge de instalación:

```markdown
[![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2FTU_USUARIO%2FTU_REPO)
```

## 7️⃣ Flujo de Actualización Continuo

Para cada actualización:

1. **Modifica** los archivos del addon
2. **Actualiza versión** en `example/config.yaml`:
   ```yaml
   version: "2.0.1"  # Incrementa aquí
   ```
3. **Actualiza** `example/CHANGELOG.md`
4. **Commit y push**:
   ```bash
   git add .
   git commit -m "Update: descripción de cambios"
   git push
   ```
5. El workflow se ejecuta automáticamente
6. La nueva versión está disponible para instalar

## ⚠️ Checklist Previo a Producción

- [ ] `repository.yaml` personalizado
- [ ] `example/config.yaml` con URLs correctas
- [ ] GitHub Actions tienen permisos de escritura
- [ ] Primer build completó exitosamente
- [ ] Imagen está marcada como pública
- [ ] README actualizado con tu información
- [ ] Tested la instalación en Home Assistant

## 🆘 Solución de Problemas

### El build falla en GitHub pero funciona localmente

- Verifica que Dockerfile no tenga paths absolutos
- Asegúrate de usar `COPY` y `ADD` correctamente
- Algunos comandos pueden no estar disponibles en Debian

### El addon no aparece después de agregar el repositorio

- Asegúrate que `config.yaml` esté bien formado (YAML válido)
- Verifica que `slug` sea único
- Espera 5 minutos y recarga en Home Assistant

### No puedo subir a GitHub Container Registry

- Verifica que `GITHUB_TOKEN` tenga permisos de lectura/escritura
- Revisa los logs del workflow para más detalles
- Asegúrate que el repositorio sea público

---

¡Ahora tu addon está listo para ser compartido con el mundo! 🎉
