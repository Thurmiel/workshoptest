# 🔧 Comandos Útiles y Referencia Rápida

## 🐙 Comandos Git

### Configuración Inicial
```bash
# Configurar nombre y email
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# Verificar configuración
git config --list
```

### Primer Push
```bash
# En la carpeta del addon
git init
git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
git branch -M main
git add .
git commit -m "Initial commit: Home Assistant addon"
git push -u origin main
```

### Actualizaciones Futuras
```bash
# Ver qué cambió
git status

# Agregar cambios
git add .

# Crear commit
git commit -m "Update: descripción de cambios"

# Subir a GitHub
git push origin main

# Ver historial
git log --oneline
```

### Deshacer Cambios
```bash
# Descartar cambios sin staged
git checkout -- archivo.txt

# Deshacer último commit (keep changes)
git reset --soft HEAD~1

# Deshacer último commit (lose changes)
git reset --hard HEAD~1
```

---

## 🐳 Comandos Docker

### Verificar Estado
```bash
# Ver containers corriendo
docker ps

# Ver todos los containers
docker ps -a

# Ver imágenes
docker images
```

### Logs y Debug
```bash
# Ver logs del addon
docker logs -f addon_local_example

# Últimas 50 líneas
docker logs --tail 50 addon_local_example

# Logs con timestamps
docker logs -f --timestamps addon_local_example

# Entrar en el container
docker exec -it addon_local_example /bin/bash

# Ver archivos en el container
docker exec addon_local_example ls -la /

# Ejecutar comando en el container
docker exec addon_local_example cat /etc/os-release
```

### Limpieza
```bash
# Eliminar container
docker rm addon_local_example

# Eliminar imagen
docker rmi mi_imagen:tag

# Limpiar todo no usado
docker system prune -a

# Ver espacio usado
docker system df
```

---

## 📜 Comandos Home Assistant (ha)

### Addon Management
```bash
# Listar addons
ha addons list

# Ver info de un addon
ha addons info local_example

# Crear addon (si estás desarrollando)
ha addons create local_example

# Detener addon
ha addons stop local_example

# Iniciar addon
ha addons start local_example

# Reiniciar addon
ha addons restart local_example

# Reconstruir addon (fuerza rebuild)
ha addons rebuild --force local_example

# Ver logs del addon
ha addons logs local_example
```

### Supervisor Management
```bash
# Info del supervisor
ha supervisor info

# Status general
ha supervisor status

# Restart supervisor
ha supervisor restart
```

---

## 📝 Ediciones Rápidas en Archivos

### Actualizar Versión en config.yaml
```yaml
# Cambio semántico: MAJOR.MINOR.PATCH
# 1.0.0 = primera versión
# 1.0.1 = bug fix
# 1.1.0 = nueva feature
# 2.0.0 = breaking change

version: "2.0.1"  # Incrementa aquí
```

### Plantilla para CHANGELOG.md
```markdown
## [2.0.1] - 2024-03-11
### Added
- Nueva característica X

### Fixed
- Corregido bug en Y

### Changed
- Mejora de rendimiento en Z

## [2.0.0] - 2024-03-10
### Breaking Changes
- Cambio importante en A
```

---

## 🔍 Búsquedas Útiles en el Código

### Encontrar todos los placeholders
```bash
# PowerShell
grep -r "TU_" . --include="*.md" --include="*.yaml"

# Linux/Mac
grep -r "TU_" . --include="*.md" --include="*.yaml"
```

### Validar YAML
```bash
# Con Python (si lo tienes instalado)
python -m yaml < example/config.yaml

# Ver si hay errores de indentación
cat example/config.yaml | grep "^  " # debe tener exactamente 2 espacios
```

---

## 🚀 Workflows Rápidos

### Workflow: Desarrollar Cambio Pequeño
```bash
# 1. Comedia la línea de imagen
# 2. Haz cambio en archivo
ha addons start local_example
docker logs -f addon_local_example  # Verifica
# 3. Si OK, descomenta imagen y push
git add .
git commit -m "Update: descripción"
git push
```

### Workflow: Agregar Nueva Dependencia
```bash
# 1. Edita Dockerfile (agrega apt-get package)
# 2. Rebuild
ha addons rebuild --force local_example
docker logs -f addon_local_example  # Verifica
# 3. Si funciona, push
git add .
git commit -m "Chore: add package X"
git push
```

### Workflow: Depuración Profunda
```bash
# 1. Ver logs
docker logs addon_local_example

# 2. Entrar en container
docker exec -it addon_local_example /bin/bash

# 3. Dentro del container
cd /
ls -la
cat /etc/os-release
ps aux  # ver procesos
```

---

## 📊 Monitoring y Verificación

### Verificar que todo está bien
```bash
# ¿Está running Home Assistant?
docker ps | grep ha

# ¿Está configurado el git?
git config user.name

# ¿Está el repositorio listo?
git remote -v

# ¿Hay cambios sin commitar?
git status

# ¿Qué vamos a subir?
git diff --cached
```

### Después de un Push
```bash
# Ver si llegó a GitHub
# En GitHub Actions > Build > Logs

# Verificar que la rama está actualizada
git branch -v

# Ver últimos commits
git log --oneline -5
```

---

## 🎯 Aliases Útiles (PowerShell)

Si usas PowerShell frecuentemente, puedes crear aliases:

```powershell
# Crear aliases temporales (para esta sesión)
Set-Alias -Name gs -Value "git status"
Set-Alias -Name gc -Value "git commit -am"
Set-Alias -Name gp -Value "git push origin main"
Set-Alias -Name dlog -Value "docker logs -f addon_local_example"

# Para hacerlos permanentes, edita tu perfil:
notepad $PROFILE
```

---

## 🐛 Troubleshooting Común

### "docker: command not found"
```bash
# Docker no está instalado o no en PATH
# Instala Docker Desktop
# O asegúrate que está en tu PATH
```

### "Permission denied"
```bash
# En Linux/Mac, probablemente necesitas sudo
sudo docker ps
# O agregarte al grupo docker
sudo usermod -aG docker $USER
```

### "Build failed in GitHub"
```bash
# 1. Revisa los logs en GitHub Actions
# 2. Los errores más comunes son:
#    - Dockerfile sintaxis incorrecta
#    - Falta apt-get package
#    - Permisos insuficientes
```

### "Addon no aparece después de agregar repo"
```bash
# 1. Espera 5 minutos
# 2. Recarga en Home Assistant
# 3. Verifica que config.yaml sea YAML válido
# 4. Comprueba que 'slug' sea único
```

---

## 📚 Recursos Oficiales

```bash
# Documentación de Home Assistant Add-ons
https://developers.home-assistant.io/docs/add-ons

# Documentación de Home Assistant CLI
https://github.com/home-assistant/cli

# S6-Overlay (sistema init usado)
https://github.com/just-containers/s6-overlay

# Bashio (librería bash)
https://github.com/home-assistant/bashio
```

---

## 💡 Pro Tips

1. **Usa `.gitignore`** - Excluye archivos grandes antes de commitear
2. **Commits descriptivos** - "Update: fix bug in startup script" es mejor que "fix"
3. **Tagea versiones** - `git tag v2.0.0 && git push --tags`
4. **Revisa logs antes de push** - `docker logs addon_local_example`
5. **Testea después de build** - No confíes solo en que compila
6. **Documentación actualizada** - CHANGELOG y DOCS.md son importantes
7. **Versionado semántico** - MAJOR.MINOR.PATCH

---

**🎯 Más dudas?** Revisa los archivos:
- `DEVELOPMENT.md` - Guía completa de desarrollo
- `SETUP_GITHUB.md` - Configuración GitHub
- `QUICK_START.md` - Checklist paso a paso
