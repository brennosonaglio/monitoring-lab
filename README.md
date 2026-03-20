# Lab de Monitoramento de TI — Prometheus + Grafana + Zabbix

Ambiente completo de monitoramento de infraestrutura com simulação de incidentes reais.

## Stack

| Ferramenta | Versão | Função |
|---|---|---|
| Prometheus | 2.51.0 | Coleta e armazenamento de métricas |
| Grafana | 9.5.20 | Visualização e dashboards |
| Zabbix Server | 6.4.21 | Monitoramento e alertas |
| Node Exporter | 1.7.0 | Métricas do sistema |
| PostgreSQL | 15 | Banco de dados do Zabbix |

## Pré-requisitos

- Docker Desktop instalado e rodando
- Windows 10/11, Mac ou Linux
- Mínimo 4GB de RAM disponível

## Como subir o ambiente

```bash
git clone <seu-repositorio>
cd monitoring-lab
docker-compose up -d
```

## Acesso aos serviços

| Serviço | URL | Login |
|---|---|---|
| Grafana | http://localhost:3000 | admin / monitoring123 |
| Prometheus | http://localhost:9090 | - |
| Zabbix | http://localhost:8080 | Admin / zabbix |
| Node Exporter | http://localhost:9100 | - |

## Incidentes Simulados

### Incidente 1 — Serviço fora do ar

```bash
# Derrubar o serviço
docker stop node-exporter

# Restaurar
docker start node-exporter
```

**Alerta configurado:** `up{job="node-exporter"} < 1`

### Incidente 2 — Sobrecarga de CPU

```bash
# Gerar carga
docker run --rm -d --name cpu-stress containerstack/cpustress --cpu 8 --timeout 300s

# Parar manualmente se necessário
docker stop cpu-stress
```

**Alerta configurado:** `100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[2m])) * 100) > 80`

## Dashboards

- **Node Exporter Full** — ID 1860 (importar via Grafana)
- **Zabbix System Status** — nativo do plugin Zabbix
- **Zabbix Server Dashboard** — nativo do plugin Zabbix

## Arquitetura

```
Node Exporter → Prometheus → Grafana
                                ↑
Zabbix Agent → Zabbix Server ──┘
                    ↑
              PostgreSQL
```

## Aprendizados

- Configuração de stack de monitoramento com Docker Compose
- Escrita de queries PromQL para métricas de CPU e disponibilidade
- Integração Grafana + Prometheus + Zabbix
- Ciclo completo de incidente: detecção → alerta → resolução
- Simulação de falhas em ambiente controlado

## Autor

Brenno — Estudante de TI com foco em Monitoramento e Infraestrutura
