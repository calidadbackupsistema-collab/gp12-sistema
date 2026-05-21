# Sistema GP12 — Guía de Instalación
# GP12 시스템 — 설치 가이드

---

## ARCHIVOS DEL SISTEMA

| Archivo | Descripción |
|---------|-------------|
| `gp12_dashboard.html` | Dashboard principal (supervisores / calidad / PC) |
| `gp12_registro.html` | App móvil para auditores (se abre al escanear QR) |

---

## PASO 1 — FIREBASE CONFIGURATION

Abre AMBOS archivos y reemplaza el bloque FIREBASE_CONFIG con tus datos reales:

```javascript
const FIREBASE_CONFIG = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_PROJECT.firebaseapp.com",
  projectId: "TU_PROJECT_ID",
  storageBucket: "TU_PROJECT.appspot.com",
  messagingSenderId: "TU_SENDER_ID",
  appId: "TU_APP_ID"
};
```

Obtén estos datos en: console.firebase.google.com → Tu Proyecto → ⚙️ → Configuración del proyecto → Tu aplicación web

---

## PASO 2 — CONTRASEÑA DEL SISTEMA

En `gp12_dashboard.html`, busca y cambia la contraseña:
```javascript
const SISTEMA_PASSWORD = "GP12Calidad2024";
```

---

## PASO 3 — URL PARA QR

En `gp12_dashboard.html`, actualiza la URL base para que los QR apunten al registro móvil correcto:
```javascript
const BASE_URL = 'https://TU-DOMINIO.web.app/gp12_registro.html';
```

---

## PASO 4 — PUBLICAR EN FIREBASE HOSTING

```bash
npm install -g firebase-tools
firebase login
cd carpeta-gp12
firebase init hosting
# Public directory: . (punto)
# Single page app: No
# Overwrite files: No
firebase deploy --only hosting
```

---

## REGLAS DE FIRESTORE (Recomendado después de pruebas)

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /gp12_registros/{doc} {
      allow read: if true;
      allow write: if request.time < timestamp.date(2027, 1, 1);
    }
  }
}
```

---

## FUNCIONALIDADES INCLUIDAS

### Dashboard (gp12_dashboard.html)
- ✅ Bilingüe Español / Coreano (한국어/스페인어)
- ✅ 8 KPIs en tiempo real (hoy, NG, abiertos, cerrados, cumplimiento, turnos, mensual)
- ✅ 12 Estaciones / Mesas de inspección con cards
- ✅ Turno Día 🌅 y Turno Noche 🌙 diferenciados
- ✅ Top 3 Defectos global + Top 3 por cada mesa
- ✅ Calendario con dots de color por turno
- ✅ Resumen Semanal (últimos 7 días)
- ✅ Acumulado Mensual (últimos 3 meses)
- ✅ Historial completo con filtros
- ✅ QR codes para las 12 mesas
- ✅ Exportar Excel (3 hojas: datos, dashboard, semanal)
- ✅ Protección por contraseña para modificaciones
- ✅ Charts de barras por mesa y por semana

### App Móvil (gp12_registro.html)
- ✅ Optimizada 100% para celular
- ✅ Carga mesa automáticamente desde QR
- ✅ Botones grandes para + / - de piezas
- ✅ Selector de turno visual
- ✅ Chips de tipo de defecto
- ✅ Estado Abierto / Cerrado
- ✅ Pantalla de éxito con estadísticas
- ✅ Guarda siguiente registro conservando mesa e inspector

---

## CONTRASEÑA DEFAULT
**GP12Calidad2024** (cámbiala en el archivo)

## COLECCIÓN FIRESTORE
Los datos se guardan en: `gp12_registros`

---

Desarrollado para HPM — Sistema GP12
Incluye: 12 Mesas · Turno Día/Noche · ES/KO · Firebase · Excel Export
