# 📊 Informe de Sesión — Fase 3 del Lab de Ciberseguridad
## Bloques A, B y C — Prometheus + Node Exporter + Grafana + Alertmanager

**Fecha:** 06 de mayo de 2026  
**Entorno:** Mac M1 (host) → UTM → Ubuntu 25.10 ARM64 (192.168.64.6)

---

## 📋 Entorno del Lab

| Componente | Detalle |
|-----------|---------|
| Host | Mac M1 8GB RAM |
| Virtualización | UTM |
| VM principal | Ubuntu 25.10 ARM64 (192.168.64.6) |
| SIEM | Wazuh 4.14.4 (nativo ARM64) |
| Nuevos servicios | Prometheus :9090, Node Exporter :9100, Grafana :3001 |

---

## ✅ Bloque A — Prometheus + Node Exporter

**Objetivo:** Desplegar stack de recogida de métricas del sistema sobre la VM Ubuntu.

**Versiones instaladas (binarios ARM64):**
- Prometheus `v3.4.0`
- Node Exporter `v1.9.1`

Ambos servicios configurados como unidades systemd con usuarios de sistema dedicados (`prometheus`, `node_exporter`) y arranque automático con la VM.

**Targets configurados en `prometheus.yml`:**

| Job | Target | Estado |
|-----|--------|--------|
| prometheus | localhost:9090 | ✅ up |
| node_exporter | localhost:9100 | ✅ up |

**Verificación:**
```bash
curl http://localhost:9090/api/v1/targets
# ambos targets: "health": "up"
```

---

## ✅ Bloque B — Grafana

**Objetivo:** Visualizar las métricas recogidas por Prometheus en dashboards gráficos.

**Versión instalada:** Grafana `v12.0.0` (binario ARM64)  
**Puerto:** `3001` (3000 ocupado por JuiceShop y Open WebUI)

Grafana configurado con Prometheus como datasource. Dashboard **Node Exporter Full (ID 1860)** importado desde Grafana.com.

**Métricas observadas en tiempo real:**

| Métrica | Valor |
|---------|-------|
| CPU Busy | ~11% |
| RAM Used | ~27% |
| Root Filesystem | ~54% |
| SWAP | 0% |

---

## 📖 Bloque C — Alertmanager (Teórico)

**Objetivo:** Conocer el componente de gestión de alertas del ecosistema Prometheus.

Alertmanager recibe alertas de Prometheus cuando una métrica supera un umbral definido en `rule_files`, y las enruta hacia destinos configurados (email, Slack, PagerDuty, webhooks).

**Flujo completo:**
```
Node Exporter :9100 → Prometheus :9090 → Alertmanager :9093 → Email/Slack
                                       → Grafana :3001
```

**Decisión de implementación:** No desplegado como servicio activo. El CyberLab SOC Dashboard ya cubre la monitorización de alertas Wazuh. Añadirlo sin un caso de uso concreto generaría deuda de mantenimiento innecesaria.

---

## 🔗 Integración con CyberLab SOC Dashboard

Se actualizó el dashboard Flask para incluir Prometheus y Grafana:

- **Leds de estado** en el panel SERVICIOS: check HTTP a `:9090/-/healthy` y `:3001/api/health` emitido vía SocketIO cada 10 segundos.
- **Links de acceso rápido** en el panel ACCESOS: Prometheus y Grafana añadidos junto al resto de servicios.

**Estado final del panel SERVICIOS:**

| Servicio | Estado |
|----------|--------|
| Wazuh | 🟢 up |
| Docker | 🟢 up |
| OpenSearch | 🟢 up |
| Prometheus | 🟢 up |
| Grafana | 🟢 up |

---

## 🗺️ Próximos pasos

- [ ] Fase 4 — Lateral movement, threat hunting, pentest

---

*CyberLab IFCT0109 — Edward Varela — Alicante 2026*
