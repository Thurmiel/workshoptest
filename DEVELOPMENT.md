# 🔧 Guía Local de Desarrollo

Esta guía te ayuda a desarrollar y probar el addon localmente antes de subirlo a GitHub.

## Prerequisitos

- Docker instalado y en ejecución
- Home Assistant (Supervisor o Development Container)
- VS Code con la carpeta del addon abierta

## 🚀 Pasos para Desarrollo Local

### 1. Preparar la configuración

#### Para desarrollo local:
- **Comenta** la línea `image:` en [example/config.yaml](example/config.yaml)
- Esto permite que Home Assistant construya el addon desde el código

```yaml
# image: "ghcr.io/homeassistant/{arch}-addon-example:latest"
```

#### Para producción en GitHub:
- **Descomenta** la línea `image:`
- Esto hace que instale desde la imagen pre-construida en GHCR

### 2. Usar los Tasks de VS Code

Tres tasks están configurados para facilitar el desarrollo:

#### Task 1: "Start Home Assistant"
- Inicia el supervisor de Home Assistant
- Primero que todo, ejecuta esto
- El servidor estará disponible en `http://localhost:8123`

```bash
supervisor_run
```

#### Task 2: "Start Addon"  
- Inicia el addon sin reconstruir
- Usa esto para cambios rápidos de configuración
- Útil cuando solo modificas scripts bash

```bash
ha addons stop "local_example"
ha addons start "local_example"
docker logs --follow "addon_local_example"
```

#### Task 3: "Rebuild and Start Addon"
- Reconstruye completamente el Docker image
- Luego reinicia el addon
- Usa esto cuando modificas `Dockerfile` o instalas nuevas dependencias

```bash
ha addons rebuild --force "local_example"
ha addons start "local_example"
docker logs --follow "addon_local_example"
```

### Opcional: usar el builder oficial localmente
Si quieres replicar lo que hace GitHub Actions usa la imagen del
[builder oficial de Home Assistant](https://github.com/home-assistant/builder).
Puedes ejecutarla desde Docker sin instalar nada adicional:

```bash
docker run --rm -v "${PWD}:/data" \
    homeassistant/builder:2025.11.0 build \
    --test --amd64 --addon example --target /data/example \
    --image "example" --docker-hub "ghcr.io/TU_USUARIO"
```

Ajusta las opciones (`--amd64`/`--aarch64`, `--image`, `--addon`) según tu
addon. El flag `--test` evita que intente subir imágenes.

Este comando imita exactamente el comportamiento de `builder.yaml` en CI.

### 3. Workflow típico

1. **Primera vez**: Ejecuta "Start Home Assistant"
   - Espera a que inicie (2-3 minutos)
   - Accede a `http://localhost:8123`

2. **Luego de cambios pequeños** (scripts, configuración):
   - Ejecuta "Start Addon"
   - Revisa los logs

3. **Luego de cambios grandes** (Dockerfile, dependencias):
   - Ejecuta "Rebuild and Start Addon"
   - Espera a que compile (1-5 minutos)
   - Revisa los logs

## 📝 Archivos Principales

### [example/Dockerfile](example/Dockerfile)
- Define la imagen base y dependencias
- Agrega aquí comandos `RUN apt-get install` para nuevos paquetes
- Copia archivos del addon con `COPY rootfs /`

### [example/config.yaml](example/config.yaml)
- Configuración del addon (nombre, versión, permisos)
- Define opciones que el usuario puede configurar

### [example/rootfs/etc/services.d/example/run](example/rootfs/etc/services.d/example/run)
- Script que inicia el addon (s6-overlay)
- Usa bashio para leer configuración

### [example/rootfs/etc/services.d/example/finish](example/rootfs/etc/services.d/example/finish)
- Script que se ejecuta cuando el addon se detiene
- Limpia recursos y detiene servicios

### [example/rootfs/usr/bin/my_program](example/rootfs/usr/bin/my_program)
- Programa principal que ejecuta el addon
- Reemplaza con tu código

## 🐛 Debugging

### Ver logs del addon
```bash
# En la carpeta del addon
docker logs -f addon_local_example
```

### Entrar en el contenedor
```bash
docker exec -it addon_local_example /bin/bash
```

### Ver estructura del contenedor
```bash
docker exec addon_local_example ls -la /
```

## 📦 Agregar Dependencias

### Agregar paquetes de Debian

En [Dockerfile](example/Dockerfile), busca el bloque `RUN apt-get install`:

```dockerfile
RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        bash \
        curl \
        mi-nuevo-paquete \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
```

Luego ejecuta "Rebuild and Start Addon" para que se instale.

### Agregar binarios ejecutables

1. Crea el archivo en `example/rootfs/usr/bin/mi_programa`
2. Hazlo ejecutable: `chmod +x mi_programa`
3. Cópialo en el Dockerfile (ya está hecho con `COPY rootfs /`)
4. Ejecuta desde el script de inicio

## 🔍 Verificar Configuración

### Validar YAML
Los archivos `.yaml` deben tener indentación correcta:
- 2 espacios para cada nivel
- Sin tabs
- Sin espacios finales

### Probar la instalación
1. En Home Assistant, ve a Settings > Add-ons & automations > Add-ons
2. Busca "Example add-on"
3. Instálalo y abre su página
4. Configura las opciones si es necesario

## ✅ Checklist Antes de Subir a GitHub

- [ ] Comenta la línea `image:` en `config.yaml`
- [ ] Pruba el addon localmente funciona
- [ ] Dockerfile no tiene errores
- [ ] Scripts bash están bien indentados
- [ ] No hay rutas absolutas hardcodeadas
- [ ] Versión en `config.yaml` es correcta
- [ ] `CHANGELOG.md` está actualizado

Luego:
- [ ] Descomenta línea `image:` en `config.yaml`
- [ ] Commit y push a GitHub
- [ ] Verifica que el workflow de GitHub Actions se ejecute

---

¡Felicidades! Ya estás listo para desarrollar addons localmente 🎉
