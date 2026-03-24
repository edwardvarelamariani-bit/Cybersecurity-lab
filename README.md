#🛡️ Cybersecurity Home Lab

Laboratorio personal de ciberseguridad montado sobre **Apple Silicon M1** con UTM.  
El objetivo es practicar detección de amenazas, análisis de incidentes y técnicas de ataque/defensa en un entorno controlado.

---

## 🖥️ Infraestructura

| Componente | Detalle |
|-----------|---------|
| Host | MacBook Air M1 8GB RAM |
| Virtualización | UTM |
| VM principal | Ubuntu 25.10 ARM64 |
| SIEM | Wazuh 4.14.4 (nativo ARM64) |
| Máquina atacante | Kali Linux VM |
| Gestión contenedores | Docker + Portainer |

### Aplicaciones vulnerables (targets)

| Contenedor | Puerto | Descripción |
|-----------|--------|-------------|
| DVWA | 8081 | Damn Vulnerable Web Application |
| WebGoat | 8082 | Aplicación web vulnerable de OWASP |
| JuiceShop | 3000 | OWASP Juice Shop |
| Nagios | 8080 | Monitorización de red |

### Agentes Wazuh

| ID | Nombre | Target |
|----|--------|--------|
| 000 | edward-ubuntu | Servidor local |
| 001 | 6553dcab1cfb | DVWA |
| 002 | feccc59e41d9 | WebGoat |

---

## 🗺️ Roadmap

| Fase | Contenido | Estado |
|------|-----------|--------|
| Fase 1 | SSH Brute Force + Port Scanning → Detección Wazuh | ✅ Completada |
| Fase 2 | SQLi + XSS en DVWA → Detección Wazuh + OpenVAS | ✅ Completada (OpenVAS pendiente) |
| Fase 3 | Wazuh Dashboard, alertas email, Prometheus + Grafana | 🔄 Pendiente |
| Fase 4 | Movimiento lateral, threat hunting, informe pentest | 🔄 Pendiente |

---

## 📁 Estructura del repositorio

```
cybersecurity-lab/
├── README.md
└── fases/
    ├── fase1-fase2-informe.md
    └── ...
```

---

## 🔗 Recursos relacionados

- [Wazuh ARM64 — Guía de instalación nativa](https://github.com/edwardvarelamariani-bit/Wazuh-arm64-lab)

