# Justia

Agente legal local con una Consultoría original conservada y cinco módulos adicionales organizados en dos espacios: **Skills** y **Herramientas**. Todo funciona abriendo los archivos directamente en el navegador (`file://`): no hay build, servidor obligatorio ni dependencias npm.

## Cómo abrirlo

1. Abre **`index (3).html`** (página de inicio).
2. Desde ahí entra al portal **`justia/index.html`**, que ofrece dos entradas:
   - **`justia/skills/index.html`** → Consultoría, Análisis de documentos, Redacción.
   - **`justia/herramientas/index.html`** → Buscador, Gestor de casos, Estrategias.

También puedes abrir cualquier `index.html` de un módulo directamente. Las rutas son relativas y apuntan siempre a archivos `index.html` concretos, no a carpetas, para que funcionen con `file://`.

Opcionalmente, si prefieres servirlo por HTTP:

```powershell
npx --yes serve .
# o
python -m http.server 8000
```

## Estructura

```
.
├── README.md                 → este archivo
├── index (3).html            → página de inicio (autónoma: CSS y fuentes incrustados)
├── skill 1/                  → Consultoría Jurídica Inteligente (trabajo original intacto)
└── justia/
    ├── index.html            → portal neutral (Skills / Herramientas)
    ├── assets/
    │   ├── theme.css         → tema compartido de los hubs
    │   ├── core.js           → utilidades, almacenamiento y traspasos entre módulos
    │   ├── catalog.js        → catálogo de fuentes oficiales + adaptador externo
    │   └── consultoria-bridge.js → puente cargado por la skill 1
    ├── skills/
    │   ├── index.html
    │   ├── analisis-documentos/
    │   └── redaccion/
    ├── herramientas/
    │   ├── index.html
    │   ├── buscador/
    │   ├── casos/
    │   └── estrategias/
    ├── docs/                 → README, CONFIGURACION y PRUEBAS detallados
    └── tests/run.js          → suite automática (Node)
```

## Funcionalidades

| Módulo | Ubicación | Qué hace |
|--------|-----------|----------|
| Consultoría Jurídica | `skill 1/index.html` | Skill original, sin modificaciones de diseño ni lógica. |
| Análisis de documentos | `justia/skills/analisis-documentos/` | Lee PDF/TXT/MD/CSV/HTML/JSON; cada hallazgo cita evidencia literal; la información ausente se marca como faltante. |
| Redacción jurídica | `justia/skills/redaccion/` | Contratos, demandas y peticiones con marcadores para datos pendientes; exporta TXT y DOCX real. |
| Buscador jurídico | `justia/herramientas/buscador/` | Catálogo local de fuentes oficiales con estado de verificación; adaptador externo opcional. |
| Gestor de casos y plazos | `justia/herramientas/casos/` | Expedientes, partes, pruebas, tareas, adjuntos, agenda ICS y cálculo de plazos con regla verificable. |
| Estrategias y rutas | `justia/herramientas/estrategias/` | Alternativas con ventajas, riesgos, pasos, autoridades y advertencias. |

Los módulos se conectan mediante `JustiaCore.handoff` (clave `justia.handoffs.v1`), con destinos `buscador`, `redaccion`, `estrategias`, `casos` y `analisis-documentos`.

## Configuración opcional

Todo funciona sin configuración en modo local. Las funciones externas quedan desactivadas y lo informan; nunca se simulan respuestas.

- **Búsqueda en línea:** define `window.JUSTIA_LEGAL_API` con `endpoint` antes de cargar `justia/herramientas/buscador/js/app.js`. Sin `endpoint`, el botón de consulta externa lo explica.
- **OCR:** define `window.JUSTIA_OCR = { tesseractScriptUrl, language }` antes de usar imágenes o PDF escaneados. Sin esta configuración, el analizador muestra un error explícito.
- **Almacenamiento:** `localStorage` para traspasos y expedientes; IndexedDB para adjuntos. Si el navegador lo bloquea, se indica que el modo es memoria.
- **Recordatorios:** API `Notification` del navegador. No son notificaciones judiciales oficiales.

Detalles completos en `justia/docs/CONFIGURACION.md`.

## Compatibilidad

- La página de inicio lleva las fuentes DM Sans y Playfair Display incrustadas, por lo que mantiene la tipografía original incluso sin conexión.
- Incluye un respaldo técnico si el navegador no expone `IntersectionObserver`: el contenido se muestra igual y el resto de interacciones siguen funcionando.
- Probado en Chromium (Chrome/Edge) sobre `file://`. Safari moderno es compatible; en navegadores antiguos o in-app pueden limitarse notificaciones e IndexedDB.

## Pruebas

Sin instalar nada, desde la raíz:

```powershell
node "justia/tests/run.js"
node "skill 1/tests/engine.test.js"
```

Resultados actuales: **41/41** verificaciones de los módulos nuevos e integración, y **31/31** pruebas de la skill original. El detalle está en `justia/docs/PRUEBAS.md`.

## Límites

- No hay API legal externa ni OCR configurados por defecto.
- No hay base de datos remota, cuentas ni sincronización entre dispositivos.
- La demo de la portada es ilustrativa; para una respuesta real se usa la Consultoría.
- Justia es una herramienta de apoyo, no asesoría ni representación legal. Verifica normas, requisitos y plazos en fuentes oficiales.
