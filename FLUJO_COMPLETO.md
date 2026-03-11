# 🔄 FLUJO COMPLETO: Del Desarrollo a la Distribución

## 📊 Diagrama de Flujo

```
┌─────────────────────────────────────────────────────────────────┐
│                    TU COMPUTADORA LOCAL                         │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │  1️⃣ DESARROLLO LOCAL                                     │  │
│  │                                                         │  │
│  │  • Edita código en VS Code                             │  │
│  │  • Comenta: image: en config.yaml                      │  │
│  │  • Ejecuta: Start Home Assistant (task)                │  │
│  │  • Ejecuta: Rebuild and Start Addon (task)             │  │
│  │  • Prueba y debug                                      │  │
│  │  • Ver logs con: docker logs -f addon_local_example    │  │
│  │                                                         │  │
│  └─────────────────────────────────────────────────────────┘  │
│           ▼                                                     │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │  2️⃣ PREPARAR PARA PUBLICACIÓN                           │  │
│  │                                                         │  │
│  │  • Descomenta: image: en config.yaml                   │  │
│  │  • Actualiza version en config.yaml                    │  │
│  │  • Actualiza CHANGELOG.md                              │  │
│  │  • Verifica README.md                                  │  │
│  │                                                         │  │
│  └─────────────────────────────────────────────────────────┘  │
│           ▼                                                     │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │  3️⃣ COMMIT Y PUSH                                        │  │
│  │                                                         │  │
│  │  $ git add .                                            │  │
│  │  $ git commit -m "Update: descripción"                 │  │
│  │  $ git push origin main                                │  │
│  │                                                         │  │
│  └─────────────────────────────────────────────────────────┘  │
└──────────────────────────┬──────────────────────────────────────┘
                           │
                           │ (por internet)
                           ▼
        ┌──────────────────────────────────┐
        │      GITHUB REPOSITORY          │
        │                                 │
        │  • Archivos del addon           │
        │  • Triggers automáticos         │
        │  • Guardar historial            │
        └──────────────────────────────────┘
                           │
                           │ (GitHub Actions)
                           ▼
        ┌──────────────────────────────────┐
        │   🔨 GITHUB ACTIONS             │
        │      (Workflow build.yml o builder.yaml)       │
        │                                 │
        │  ✅ Checkout código             │
        │  ✅ Setup Docker Buildx         │
        │  ✅ Login a GHCR                │
        │  ✅ Build para amd64            │
        │  ✅ Build para aarch64          │
        │  ✅ Publicar a GHCR             │
        │  ⏱️  ~15 minutos                │
        └──────────────────────────────────┘
                           │
                           │ (publicado)
                           ▼
        ┌──────────────────────────────────┐
        │  📦 GITHUB CONTAINER REGISTRY   │
        │       (GHCR)                    │
        │                                 │
        │  Imagen disponible:             │
        │  ghcr.io/USUARIO/example        │
        │  - versión latest               │
        │  - versión 2.0.1                │
        │  - Para amd64                   │
        │  - Para aarch64                 │
        └──────────────────────────────────┘
                           │
                           │ (descargable)
                           ▼
┌──────────────────────────────────────────────────────────────────┐
│              USUARIOS - HOME ASSISTANT INSTANCIAS              │
│                                                                │
│  Settings > Add-ons & Automations > Add-ons > Repositories    │
│                                                                │
│  • Agregan: https://github.com/USUARIO/REPO                  │
│  • Ven: "Example add-on"                                     │
│  • Presionan: Install                                        │
│  • Se descarga desde GHCR                                    │
│  • ¡Addon funcionando!                                       │
│                                                                │
└──────────────────────────────────────────────────────────────────┘
```

---

## 📝 FLUJO PASO A PASO

### Fase 1: Desarrollo (Local)

```
┌─────────────────────────────────────────┐
│  Semana 1-N: Desarrollar Feature        │
├─────────────────────────────────────────┤
│                                         │
│  ✎ Edita código                         │
│  └─ rootfs/usr/bin/my_program           │
│  └─ example/config.yaml                 │
│  └─ ejemplo/Dockerfile                  │
│                                         │
│  🔧 Prueba localmente                   │
│  └─ Task: Rebuild and Start Addon       │
│  └─ docker logs -f addon_local_example  │
│  └─ Itera hasta que funcione            │
│                                         │
│  ✓ Versión final lista                  │
│                                         │
└─────────────────────────────────────────┘
```

### Fase 2: Preparación (Transición)

```
┌─────────────────────────────────────────┐
│  Listo para Publicación                 │
├─────────────────────────────────────────┤
│                                         │
│  ✓ Descomenta: image: en config.yaml    │
│    Antes: # image: "ghcr.io/..."        │
│    Después: image: "ghcr.io/..."        │
│                                         │
│  ✓ Incrementa versión                  │
│    Antes: version: "2.0.0"              │
│    Después: version: "2.0.1"            │
│                                         │
│  ✓ Documenta cambios en CHANGELOG.md   │
│    - Fixed: Bug en startup              │
│    - Added: Nueva feature               │
│                                         │
│  ✓ Prepara commit                      │
│    git add .                            │
│    git commit -m "Release v2.0.1"      │
│                                         │
└─────────────────────────────────────────┘
```

### Fase 3: Publicación (GitHub)

```
┌─────────────────────────────────────────┐
│  Push a GitHub                          │
├─────────────────────────────────────────┤
│                                         │
│  $ git push origin main                 │
│                                         │
│  ✓ GitHub recibe cambios                │
│  ✓ Dispara GitHub Actions               │
│  ✓ Workflow: build.yml o builder.yaml se ejecuta       │
│                                         │
│  Construye para:                        │
│  ✓ Linux/amd64                          │
│  ✓ Linux/aarch64                        │
│                                         │
│  Publica a:                             │
│  ✓ ghcr.io/USUARIO/example:latest      │
│  ✓ ghcr.io/USUARIO/example:2.0.1       │
│                                         │
└─────────────────────────────────────────┘
```

### Fase 4: Distribución (Los Usuarios)

```
┌─────────────────────────────────────────┐
│  Usuario Nuevo                          │
├─────────────────────────────────────────┤
│                                         │
│  1. Abre Home Assistant                 │
│  2. Va a Settings > Add-ons              │
│  3. Presiona "Repositories"              │
│  4. Agrega URL:                          │
│     https://github.com/USUARIO/REPO    │
│  5. Busca "Example add-on"              │
│  6. Presiona "Install"                   │
│  7. Espera descarga...                   │
│  8. Presiona "Start"                     │
│  9. ¡Addon corriendo!                   │
│                                         │
│  Lo que pasa en background:              │
│  • HA descarga imagen de GHCR           │
│  • Extrae en su Docker                  │
│  • Corre el contenedor                  │
│  • Ejecuta rootfs/etc/services.d/run   │
│                                         │
└─────────────────────────────────────────┘
```

---

## ⏱️ TIMELINE

```
SEMANA 1
│
├─ Lunes: Desarrollas Feature A
│  • Local: funciona
│  • Tests: OK
│
├─ Miércoles: Feature A + B
│  • Local: todo OK
│  • Checklist: completado
│
└─ Viernes: PUBLICACIÓN
   • Actualiza versión → 2.0.1
   • Escribe CHANGELOG
   • git push → GitHub Actions inicia
   • Espera 15 min → Build completa
   • Verifica en GHCR → Imagen pública
   • ¡Disponible para instalar!

SIGUIENTE SEMANA
│
├─ Usuarios reportan bugs
│  • Lunes: Corregiste bug X
│  • Martes: Versión 2.0.2 → push
│  • Miércoles: Build completa + disponible
│
└─ Usuarios instalan actualización automáticamente
```

---

## 🔄 CICLO DE VIDA DEL ADDON

```
v1.0.0 (Release inicial)
  │
  ├─ v1.0.1 (Bugfix)
  ├─ v1.0.2 (Bugfix)
  │
  ├─ v1.1.0 (Nueva feature)
  │
  ├─ v1.1.1 (Bugfix)
  │
  └─ v2.0.0 (Major release - breaking changes)
     │
     ├─ v2.0.1 (Bugfix)
     ├─ v2.0.2 (Bugfix)
     │
     ├─ v2.1.0 (Nueva feature)
     │
     └─ v2.1.1 (Bugfix)
        ...y así sucesivamente

Cada versión es independiente y coexiste en GHCR
```

---

## 📊 ARCHIVOS EN CADA FASE

### Fase 1: Desarrollo

```
src/
├─ example/
│  └─ rootfs/
│     ├─ etc/services.d/example/
│     │  ├─ run      (✏️ Editas aquí)
│     │  └─ finish
│     └─ usr/bin/
│        └─ my_program (✏️ Editas aquí)
│
└─ example/
   ├─ config.yaml (✏️ Editas aquí)
   ├─ Dockerfile  (✏️ Editas aquí)
   └─ CHANGELOG.md
```

### Fase 2: Preparación

```
Files to modify:
├─ example/config.yaml
│  ├─ Descomenta: image:
│  └─ Actualiza: version:
│
└─ example/CHANGELOG.md
   └─ Agrega entrada nueva
```

### Fase 3: Publicación

```
git push → GitHub Actions
├─ Lee: .github/workflows/build.yml (o builder.yaml para el flujo oficial)
├─ Construye desde: example/Dockerfile
├─ Extrae versión de: example/config.yaml
├─ Publica a: ghcr.io/USUARIO/example
└─ Etiqueta: latest + version
```

### Fase 4: Distribución

```
Usuario descarga desde:
├─ GHCR: ghcr.io/USUARIO/example
├─ Versión: latest (automática)
├─ Arquitectura: detectada automáticamente
│  ├─ Para amd64
│  ├─ Para aarch64
│  └─ Para armv6 (si lo soportas)
└─ Ejecuta: rootfs/etc/services.d/example/run
```

---

## 🎯 PUNTOS CLAVE

1. **Desarrollo Local**
   - Trabajas en tu máquina
   - Pruebas constantemente
   - Sin conectarte a internet

2. **Cambio Único**: Comentar/Descomenta `image:`
   - CON `# image:` → Build local (desarrollo)
   - SIN `# image:` → Build remoto (producción)

3. **Automatización Completa**
   - Haces push → GitHub Actions compila
   - Sin necesidad de hacer nada más
   - Automáticamente en GHCR

4. **Multi-Arquitectura**
   - Una línea de código
   - Dos builds simultáneos (amd64 + aarch64)
   - Usuarios reciben imagen correcta

5. **Versionado**
   - Cambias número en config.yaml
   - Actions lo detecta automáticamente
   - Crea tag + latest

---

## 💡 RESUMEN

```
┌────────────┐         ┌──────────────┐         ┌──────────┐
│ Desarrollo │ ──git──> │ GitHub Repo  │ ──ci──> │ GHCR     │
│ Local      │  push    │              │ builds  │ Registry │
└────────────┘         └──────────────┘         └──────────┘
                                                     │
                                                  (descarga)
                                                     ▼
                                            ┌──────────────────┐
                                            │ Home Assistant   │
                                            │ Usuarios         │
                                            │ Instalan addon   │
                                            └──────────────────┘
```

---

¡Ahora entiendes el flujo completo! De desarrollo a usuarios, todo automático. 🚀
