# FortiLab-GNS3 — Firewall y segmentación en práctica

Plataforma de laboratorio para implementar **FortiGate** en **GNS3** con
zonas LAN, DMZ y WAN, y un **analizador web** que revisa configuraciones
y genera un informe de buenas prácticas.

---

## Características Principales

### 1. Laboratorio FortiGate (GNS3)

- **Topología**: FortiGate, 2 switches, servidor web en DMZ, PCs de LAN
  y un router que simula Internet.
- **Funciones configuradas**:
  - Interfaces y zonas (`LAN`, `DMZ`, `WAN`).
  - Políticas de firewall con el principio de mínimo privilegio.
  - `NAT` de salida y *Virtual IP* para publicar el servidor DMZ.
  - Túnel `IPsec` sitio a sitio (opcional).
  - Registro de logs y perfiles de seguridad básicos.

### 2. Analizador de configuración (FastAPI)

- **Carga de archivos** `.conf` exportados desde el FortiGate.
- **Reglas de revisión**:
  - Políticas con origen/destino `all` y servicio `ALL`.
  - Administración abierta por HTTP/Telnet.
  - Falta de logs en políticas.
  - Contraseñas o usuarios por defecto.
- **Informe** con nivel de riesgo (Alto / Medio / Bajo) y recomendación.

### 3. Interfaz web ligera

- Panel simple para subir el archivo y ver hallazgos en una tabla.
- Descarga del informe en `Markdown` o `JSON`.

---

## Arquitectura del Proyecto

FortiLab-GNS3/
├── lab/
│   ├── topologia.gnsproject      # Proyecto GNS3 exportado
│   ├── fortigate-base.conf       # Configuración inicial
│   └── fortigate-segura.conf     # Políticas endurecidas
├── backend/
│   ├── main.py                   # API FastAPI
│   └── rules/
│       ├── policies.py           # Reglas de políticas de firewall
│       └── management.py         # Reglas de administración y acceso
├── frontend/
│   ├── index.html                # Subida de archivo y resultados
│   ├── styles.css
│   └── app.js
├── docs/
│   ├── guia-laboratorio.md       # Pasos para armar el lab
│   └── direccionamiento.md       # Tabla de IPs y zonas
├── tests/
│   └── test_rules.py             # Pruebas con pytest
├── requirements.txt
└── README.md

---

## Ejecución Local y Pruebas

### 1. Laboratorio

1. Importa `lab/topologia.gnsproject` en GNS3.
2. Carga `fortigate-base.conf` en el FortiGate y verifica la conectividad.
3. Aplica `fortigate-segura.conf` y prueba que LAN→DMZ y WAN→DMZ
   funcionen solo en los puertos permitidos.

### 2. Analizador

pip install -r requirements.txt
uvicorn backend.main:app --host 127.0.0.1 --port 8000

Abre `http://127.0.0.1:8000` y sube un archivo de `lab/`.

### 3. Pruebas

pytest -v

---
