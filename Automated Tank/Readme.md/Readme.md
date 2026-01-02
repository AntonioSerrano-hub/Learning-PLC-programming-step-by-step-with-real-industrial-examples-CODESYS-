# Project: Automated Tank Control (PLC) — CODESYS (LD + ST)

Este proyecto implementa el control de un tanque con **modo Manual / Automático**, utilizando:

- **CODESYS Ladder (LD)** para el mando de salidas (bomba/válvula).
- **Structured Text (ST)** para la lógica de **máquina de estados** (AUTO), sensores derivados, interlocks y timeouts.
- Simulación simple del nivel del tanque para pruebas sin hardware real.

---

## ✅ Alcance (PLC) — Parte 1

### Modos de operación
- **Manual (bAutoMode = FALSE)**  
  - El operador controla:
    - `bManualPump` → Bomba
    - `bManualValve` → Válvula

- **Automático (bAutoMode = TRUE)**  
  - El operador usa:
    - `bStart` para iniciar el ciclo de llenado
    - `bStop` para ordenar drenado (cuando está FULL)
    - `bResetFault` para resetear fallas

### Máquina de estados (AUTO)
Estados implementados (`DUT_TANKTypes`):
- `IDLE`  → espera
- `FILLING` → llena con bomba ON
- `FULL` → tanque lleno, todo OFF
- `DRAINING` → drena con válvula ON
- `FAULT` → falla, todo OFF

**Transiciones principales:**
- `IDLE` → `FILLING` cuando `bStart` (y Auto activo)
- `FILLING` → `FULL` cuando `bLevelHigh` (≥ 90%)
- `FULL` → `DRAINING` cuando `bStop`
- `DRAINING` → `IDLE` cuando `bLevelLow` (≤ 10%)
- Cualquier estado → `FAULT` si se detecta falla

---

## 🔌 Señales principales (GVL_TankIO)

### Entradas / Comandos
- `bAutoMode` (BOOL): 1=AUTO, 0=MANUAL
- `bStart` (BOOL): inicio en AUTO
- `bStop` (BOOL): orden de drenado en AUTO (desde FULL)
- `bResetFault` (BOOL): reset de falla
- `bManualPump` (BOOL): mando manual bomba
- `bManualValve` (BOOL): mando manual válvula

### Variables de proceso
- `rLevelPercent` (REAL): nivel simulado 0..100
- `bLevelHigh` (BOOL): `rLevelPercent >= 90`
- `bLevelLow` (BOOL): `rLevelPercent <= 10`

### Comandos automáticos (los decide el ST)
- `bAutoPump` (BOOL)
- `bAutoValve` (BOOL)

### Salidas (actuadores)
- `bPump` (BOOL): salida final a bomba
- `bValve` (BOOL): salida final a válvula

### Diagnóstico
- `eState` (DUT_TANKTypes): estado actual
- `bFault` (BOOL): falla activa
- `iFaultCode` (INT): código de falla

---

## 🪜 Ladder (LD) — Lógica de salidas
El Ladder combina Manual/AUTO y aplica condiciones de seguridad:

**Rung Bomba (`bPump`)**
- Manual: `NOT bAutoMode` AND `bManualPump`
- Auto: `bAutoMode` AND `bAutoPump`
- Bloqueos típicos:
  - No activar si `bValve` está ON
  - No activar si `bLevelHigh` está activo (opcional según el diseño)
  - No activar si `bFault` está activo

**Rung Válvula (`bValve`)**
- Manual: `NOT bAutoMode` AND `bManualValve`
- Auto: `bAutoMode` AND `bAutoValve`
- Bloqueos típicos:
  - No activar si `bPump` está ON
  - No activar si `bLevelLow` está activo (opcional según el diseño)
  - No activar si `bFault` está activo

---

## 🧠 ST (PRG_MAINN) — Seguridad y fallas

### Interlock (bomba y válvula nunca juntas)
Si se detecta simultáneo:
- `bPump = TRUE` y `bValve = TRUE`
→ `bFault = TRUE`, `iFaultCode = 1`, `eState = FAULT`

### Timeouts (protección por sensores/proceso)
Timers TON:
- `tFillTimeout` → activo en `FILLING`
- `tDrainTimeout` → activo en `DRAINING`

Si vence:
- `FILLING` timeout → `iFaultCode = 2`
- `DRAINING` timeout → `iFaultCode = 3`
y pasa a `FAULT`

> Nota: En pruebas se usó PT=10s para testear rápido. En “real” se ajusta según proceso (por ejemplo 60s).

---

## 🧪 Simulación de nivel (sin hardware)
Para pruebas:
- Si `bPump = TRUE` → `rLevelPercent` sube (ej: +0.05 por ciclo)
- Si `bValve = TRUE` → `rLevelPercent` baja (ej: -0.05 por ciclo)
- Clamp final: 0..100

---

## ✅ Cómo testear (rápido)
1. Poner `bAutoMode = TRUE`
2. Forzar `bStart = TRUE` por 1 segundo y volver a `FALSE`
3. Verificar:
   - `eState` pasa a `FILLING`
   - `bAutoPump = TRUE` y en Ladder `bPump = TRUE`
4. Cuando llega a 90%:
   - `eState` pasa a `FULL` y `bAutoPump = FALSE`
5. Forzar `bStop = TRUE` por 1 segundo:
   - `eState` pasa a `DRAINING`
   - `bAutoValve = TRUE` y en Ladder `bValve = TRUE`
6. Al llegar a 10%:
   - `eState` vuelve a `IDLE`

### Test de fallas
- Setear timeout bajo (ej: PT=10s) y provocar que no llegue a umbral:
  - Debe ir a `FAULT` y setear `iFaultCode` correspondiente
- Reset:
  - Forzar `bResetFault = TRUE` → vuelve a `IDLE` y `bFault=FALSE`

---

## 📌 Próximo paso del proyecto
- **Parte 2: HMI en Ignition**
  - Pantallas de operación (Manual/AUTO)
  - Indicadores de estado, alarmas, nivel y diagnóstico
  - Botones Start/Stop/Reset Fault
  - Registro de eventos (opcional)
