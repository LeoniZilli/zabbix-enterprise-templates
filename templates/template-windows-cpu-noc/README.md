# Template - Windows CPU by NOC

## 📌 Visão Geral

Template modular responsável pelo monitoramento de utilização de CPU em servidores Windows.

Este módulo faz parte da stack Windows NOC e é focado exclusivamente em análise de consumo de processamento, utilizando coleta ativa via Zabbix Agent.

Desenvolvido por Leoni e padronizado conforme arquitetura NOC.

---

## 🎯 Objetivo

- Monitorar utilização média da CPU
- Detectar sobrecarga sustentada
- Classificar alertas por níveis de severidade
- Evitar falso-positivo com validação por janela de tempo
- Permitir ajuste fino através de macros

---

## 📊 Métricas Monitoradas

### 🔹 Utilização de CPU

- `system.cpu.util` — Percentual de utilização da CPU

Características:

- Tipo: Zabbix Agent (ativo)
- Unidade: %
- Histórico: 7 dias
- Valor do tipo: Float

Permite identificar:

- Saturação de processamento
- Lentidão sistêmica
- Sobrecarga causada por aplicações
- Necessidade de scaling ou troubleshooting

---

## 🚨 Estratégia de Alertas

O template possui três níveis de severidade com dependência hierárquica para evitar alertas duplicados.

---

### 🔹 WARNING — Utilização elevada

Dispara quando:

    min(system.cpu.util, {$CPU.UTIL.TIME_CHECK_WARN}) >= {$CPU.UTIL.WARN}

Recovery:

    max(system.cpu.util, {$CPU.UTIL.TIME_REC_WARN}) < {$CPU.UTIL.WARN}

Características:

- Severidade: WARNING
- Escopo: performance
- Indica tendência de alta utilização

---

### 🔹 AVERAGE — Utilização muito alta

Dispara quando:

    min(system.cpu.util, {$CPU.UTIL.TIME_CHECK_AVG}) >= {$CPU.UTIL.AVG}

Recovery:

    max(system.cpu.util, {$CPU.UTIL.TIME_REC_AVG}) < {$CPU.UTIL.AVG}

Características:

- Severidade: AVERAGE
- Escopo: performance
- Dependente do alerta HIGH
- Indica sobrecarga significativa

---

### 🔹 HIGH — Saturação crítica

Dispara quando:

    min(system.cpu.util, {$CPU.UTIL.TIME_CHECK_HIGH}) >= {$CPU.UTIL.HIGH}

Recovery:

    max(system.cpu.util, {$CPU.UTIL.TIME_REC_HIGH}) < {$CPU.UTIL.HIGH}

Características:

- Severidade: HIGH
- Escopo: performance
- Fechamento manual habilitado
- Representa saturação crítica de CPU

---

## 🧠 Estratégia Técnica Aplicada

- Coleta ativa via Zabbix Agent
- Validação por janela de tempo (função `min()`)
- Recovery expression dedicada
- Dependência hierárquica entre triggers
- Prevenção de flapping
- Padronização via macros parametrizáveis
- Separação modular (CPU isolado de memória/disco)

---

## 🔗 Integração

Este módulo é projetado para ser utilizado em conjunto com:

- Windows Base by NOC
- Windows Memory by NOC
- Windows Discovery Disk by NOC
- Triple Check

Compõe a camada de monitoramento de performance da stack Windows NOC.

---

## ⚙️ Tipo de Coleta

Compatível com:

- Zabbix Agent (ativo)

Foco em escalabilidade e baixo impacto no servidor Zabbix.

---

## 🏷 Macros Utilizadas

| Macro | Default | Descrição |
|-------|---------|-----------|
| `{$CPU.UTIL.WARN}` | 90 | Limite de aviso (%) |
| `{$CPU.UTIL.AVG}` | 95 | Limite alto (%) |
| `{$CPU.UTIL.HIGH}` | 99 | Limite crítico (%) |
| `{$CPU.UTIL.TIME_CHECK_WARN}` | 15m | Janela para disparo WARNING |
| `{$CPU.UTIL.TIME_CHECK_AVG}` | 15m | Janela para disparo AVERAGE |
| `{$CPU.UTIL.TIME_CHECK_HIGH}` | 15m | Janela para disparo HIGH |
| `{$CPU.UTIL.TIME_REC_WARN}` | 10m | Janela para recuperação WARNING |
| `{$CPU.UTIL.TIME_REC_AVG}` | 10m | Janela para recuperação AVERAGE |
| `{$CPU.UTIL.TIME_REC_HIGH}` | 10m | Janela para recuperação HIGH |

---

## 📈 Dashboard Incluído

Dashboard: **CPU**

Contém:

- Gráfico clássico — Utilização de CPU
- Visualização contínua da variação de carga
- Base para análise histórica e troubleshooting

---

## 🏷 Versão

v1.0 – Monitoramento modular de utilização de CPU com controle hierárquico de severidade e validação temporal.