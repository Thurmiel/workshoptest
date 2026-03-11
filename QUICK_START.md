# ✅ CHECKLIST DE CONFIGURACIÓN RÁPIDA

Sigue este checklist paso a paso para tener todo listo.

## 🔴 PASO 1: Personalización Inicial (5 minutos)

### Edita estos archivos en VS Code:

- [ ] **repository.yaml**
  ```yaml
  name: Tu Nombre - Add-ons
  url: 'https://github.com/TU_USUARIO/TU_REPO'
  maintainer: Tu Nombre <tu@email.com>
  ```

- [ ] **example/config.yaml** - actualiza URL:
  ```yaml
  url: "https://github.com/TU_USUARIO/TU_REPO/tree/main/example"
  ```

- [ ] **README.md** - reemplaza todos:
  - `TUSUARIO` → tu usuario
  - `TUREPO` → tu repo

---

## 🟡 PASO 2: Crear Repositorio en GitHub (3 minutos)

- [ ] Crea repositorio nuevo en GitHub (público)
  - Nombre: `TU_REPO`
  - Descripción: "Add-ons para Home Assistant"
  - Sin inicializar con archivos

- [ ] Copia la URL HTTPS: `https://github.com/TU_USUARIO/TU_REPO.git`

---

## 🟠 PASO 3: Push Inicial desde Local (5 minutos)

### En terminal PowerShell:

```powershell
cd d:\workshoptest
git init
git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
git branch -M main
git add .
git commit -m "Initial commit: Home Assistant addon repository"
git push -u origin main
```

- [ ] Verifica que todos los archivos están en GitHub

---

## 🟡 PASO 4: Configurar GitHub Actions (2 minutos)

- [ ] Ve a tu repositorio en GitHub
- [ ] **Settings > Actions > General**
- [ ] En "Workflow permissions":
  - [ ] Selecciona "Read and write permissions"
  - [ ] Marca "Allow GitHub Actions to create and approve pull requests"
  - [ ] Presiona **Save**

---

## 🟠 PASO 5: Ejecutar Build (15 minutos)

- [ ] Ve a **Actions** en tu repositorio
- [ ] Verás uno o dos workflows corriendo:
  - "Build Home Assistant Add-ons" (ejemplo manual)
  - "Builder" (usa el builder oficial, recomendado)

> El builder está configurado para ejecutarse **siempre** en cada push, no
> sólo cuando cambian ciertos archivos.
- [ ] **Espera a que termine** (10-15 minutos)
- [ ] Verifica que **"All checks passed"**

---

## 🟡 PASO 6: Hacer Pública la Imagen (2 minutos)

### Si el build fue exitoso:

- [ ] Ve a tu perfil GitHub > **Packages**
- [ ] Busca tu addon en la lista
- [ ] Abre el paquete
- [ ] **Package settings** (esquina superior derecha)
- [ ] En "Danger zone", marca **"Change package visibility"**
- [ ] Selecciona **Public**
- [ ] Presiona **Change visibility**

---

## 🟢 PASO 7: Probar Instalación (5 minutos)

### En tu instancia de Home Assistant:

- [ ] Ve a **Settings > Add-ons & automations > Add-ons**
- [ ] Presiona **Repositories**
- [ ] Agrega: `https://github.com/TU_USUARIO/TU_REPO`
- [ ] Espera a que se cargue
- [ ] Busca "Example add-on"
- [ ] Presiona **Install**
- [ ] Espera a que termine
- [ ] Presiona **Start**
- [ ] Verifica que inició sin errores

---

## ✅ ¡LISTO!

Tu addon está publicado y disponible para instalar.

### Para actualizar en el futuro:

```powershell
# Realiza cambios
# Actualiza versión en example/config.yaml
# Actualiza example/CHANGELOG.md

git add .
git commit -m "Update: descripción"
git push origin main
```

El build se ejecutará automáticamente.

---

## 🆘 TROUBLESHOOTING

### El build falla en GitHub

- Revisa los logs en **Actions > Build > Logs**
- Errores comunes:
  - Dockerfile tiene sintaxis incorrecta
  - Falta dependencia en apt-get install
  - Permisos insuficientes

### El addon no aparece después de agregar repo

- Espera 5 minutos
- Recarga la página
- Verifica que `config.yaml` esté bien formado (YAML válido)

### No puedo instalar desde mi repositorio

- Asegúrate que la imagen es **Public** en GitHub Container Registry
- Verifica que `config.yaml` tenga `image:` descomentado en production
- Comprueba que `slug` en `config.yaml` sea único

---

## 📚 Documentación Adicional

- [CAMBIOS.md](CAMBIOS.md) - Qué cambió
- [SETUP_GITHUB.md](SETUP_GITHUB.md) - Guía detallada de GitHub
- [DEVELOPMENT.md](DEVELOPMENT.md) - Desarrollo local
- [ESTRUCTURA.md](ESTRUCTURA.md) - Estructura del repositorio

---

**Necesitas ayuda?** Revisa [SETUP_GITHUB.md](SETUP_GITHUB.md) para instrucciones detalladas.
