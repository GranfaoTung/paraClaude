# PANAL PRODUCTION SYSTEM
## Sistema de Gestión de Producción de Bolsas Plásticas

### 📋 Descripción
Sistema web completo para gestión de producción, aprobación y liquidación de rollos de bolsas plásticas. Implementado con Google Apps Script y Google Sheets como base de datos.

### 🏗️ Arquitectura

```
┌─────────────────────────────────────────────────────────┐
│                    PANAL PRODUCTION                      │
├─────────────┬─────────────┬─────────────────────────────┤
│  EMPLEADO   │   CAPATAZ   │           DUEÑO             │
├─────────────┼─────────────┼─────────────────────────────┤
│• Registrar  │• Aprobar    │• Ejecutar cierre semanal    │
│  producción │  producción │• Configurar precios         │
│• Ver        │• Gestionar  │• Generar reportes           │
│  estimado   │  empleados  │• Administrar usuarios       │
│  semanal    │• Dashboard  │• Acceso total               │
└─────────────┴─────────────┴─────────────────────────────┘
```

### 📁 Estructura de Archivos

**Backend (Google Apps Script):**
- `Code.gs` - Punto de entrada principal y configuración
- `Database.gs` - Capa de acceso a datos (Google Sheets)
- `Auth.gs` - Autenticación y gestión de sesiones
- `Production.gs` - Lógica de negocio de producción

**Frontend (HTML):**
- `index.html` - Estructura principal de la interfaz
- `styles.html` - Sistema de diseño CSS
- `script.html` - Lógica del cliente JavaScript

### 🚀 Instalación y Despliegue

#### Paso 1: Crear Google Spreadsheet
1. Abre Google Sheets: https://sheets.google.com
2. Crea una nueva hoja de cálculo
3. Nómbrala "PANAL Production Database"
4. Copia el ID de la hoja (está en la URL):
   ```
   https://docs.google.com/spreadsheets/d/[ESTE_ES_EL_ID]/edit
   ```

#### Paso 2: Crear Apps Script Project
1. En la hoja de cálculo, ve a: **Extensiones** → **Apps Script**
2. Borra el código por defecto (`Code.gs`)
3. Crea los siguientes archivos:

**Archivos .gs (Google Apps Script):**
- Clic en **+** junto a "Archivos"
- Selecciona "Script"
- Copia el contenido de cada archivo:
  - `Code.gs`
  - `Database.gs`
  - `Auth.gs`
  - `Production.gs`

**Archivos HTML:**
- Clic en **+** junto a "Archivos"
- Selecciona "HTML"
- Copia el contenido de cada archivo:
  - `index.html`
  - `styles.html`
  - `script.html`

#### Paso 3: Inicializar Base de Datos
1. En el editor de Apps Script, selecciona la función `initializeSpreadsheet`
2. Haz clic en **Ejecutar** (▶️)
3. Autoriza la aplicación cuando se solicite
4. Verifica que se crearon las hojas en tu Spreadsheet:
   - EMPLEADOS
   - PRECIOS
   - PRODUCCION
   - LIQUIDACIONES
   - AUDITORIA

#### Paso 4: Configurar ID del Spreadsheet
1. En el editor, selecciona la función `setSpreadsheetId`
2. Haz clic en **Ejecutar** (▶️)
3. Esto guardará automáticamente el ID en las propiedades del script

#### Paso 5: Desplegar como Web App
1. Haz clic en **Implementar** → **Nueva implementación**
2. Selecciona tipo: **Aplicación web**
3. Configuración:
   - **Descripción:** "PANAL Production v1.0"
   - **Ejecutar como:** "Yo (tu email)"
   - **Quién tiene acceso:** "Cualquier usuario"
4. Haz clic en **Implementar**
5. Copia la **URL de la aplicación web**

#### Paso 6: Acceder al Sistema
1. Abre la URL en tu navegador
2. Usa los PINs de prueba:
   - **Empleado:** 1234
   - **Capataz:** 9999
   - **Dueño:** 0000

### 🔒 Seguridad

- **PINs:** Almacenados como hash SHA-256
- **Sesiones:** Tokens UUID con expiración de 8 horas
- **Permisos:** Validación en cada operación del backend
- **Auditoría:** Log de todas las operaciones críticas

### 👥 Usuarios de Prueba

| Nombre | PIN | Rol | Permisos |
|--------|-----|-----|----------|
| Juan Pérez | 1234 | EMPLEADO | Registrar producción, ver historial |
| María Gómez | 5678 | EMPLEADO | Registrar producción, ver historial |
| Carlos Jefe | 9999 | CAPATAZ | Aprobar/rechazar, crear empleados |
| Sr. Dueño | 0000 | DUEÑO | Acceso total, cierre semanal |

### 💰 Configuración de Precios (Inicial)

| Tamaño | Precio Base | Umbral Bonus | % Bonus | Precio con Bonus |
|--------|-------------|--------------|---------|------------------|
| 30x40 | $5.00 | 50 rollos | 10% | $5.50 |
| 50x60 | $8.50 | 30 rollos | 15% | $9.78 |
| 70x90 | $12.00 | 20 rollos | 20% | $14.40 |

### 📊 Reglas de Negocio Críticas

1. **BN-01:** Ningún rollo genera pago sin aprobación explícita
2. **BN-02:** El bonus se aplica cuando se excede el umbral semanal
3. **BN-03:** Cálculo: Si Total ≥ Umbral → Precio con Bonus
4. **BN-07:** Semana = Lunes 00:00 a Domingo 23:59
5. **BN-08:** Capataz NO puede aprobar sus propios rollos

### 🔧 Funciones Administrativas

#### Crear Usuario
```javascript
// Solo CAPATAZ (empleados) o DUEÑO (cualquier rol)
Auth.createUser(sessionToken, {
  name: "Nuevo Empleado",
  pin: "1111",
  role: "EMPLEADO",
  email: "empleado@example.com"
});
```

#### Ejecutar Cierre Semanal
```javascript
// Solo DUEÑO
Production.executeWeeklySettlement(ownerId, "2026-W07");
```

### 🐛 Solución de Problemas

**Error: "Session expired"**
- Solución: Vuelve a hacer login. Las sesiones duran 8 horas.

**Error: "Spreadsheet not found"**
- Solución: Ejecuta `setSpreadsheetId()` de nuevo.

**Error: "Authorization required"**
- Solución: Ve a Apps Script → Ejecutar cualquier función → Autoriza.

**Los cambios no se reflejan**
- Solución: Crea una nueva implementación (Deploy → New deployment)

### 📈 Próximas Funcionalidades (Roadmap)

- [ ] Exportación de reportes en PDF
- [ ] Notificaciones por email
- [ ] Gestión de turnos
- [ ] Dashboard de métricas del dueño
- [ ] Aplicación móvil nativa
- [ ] Integración con sistemas contables

### 🎨 Diseño

**Tema:** Industrial Command Center  
**Paleta:**
- Primary: #ff6b2c (Safety Orange)
- Secondary: #00e5ff (Electric Cyan)
- Background: #0a0e1a (Deep Navy)

**Tipografía:**
- Display: Archivo (900)
- Mono: DM Mono (datos numéricos)

### 📝 Licencia

Propiedad privada. Todos los derechos reservados.

### 👨‍💻 Soporte

Para asistencia técnica, contacta al administrador del sistema.

---

**Versión:** 1.0.0  
**Fecha:** 13 de Febrero, 2026  
**Desarrollado con:** Google Apps Script + HTML/CSS/JavaScript
