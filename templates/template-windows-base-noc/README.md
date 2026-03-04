# Template - Windows Base by NOC

## 📌 Visão Geral

Template base modular responsável pelo monitoramento estrutural do sistema operacional Windows.

Este template consolida informações essenciais do sistema e integra os módulos especializados da stack Windows NOC, como CPU, Memória, Disco e validações de disponibilidade.

Gerado originalmente a partir da estrutura oficial do Zabbix (Templator) e adaptado ao padrão arquitetural NOC.

---

## 🎯 Objetivo

- Monitorar informações estruturais do sistema operacional
- Detectar reinicializações
- Detectar alteração de hostname
- Validar sincronismo de horário
- Monitorar volume de processos e threads
- Servir como template base agregador dos módulos Windows
- Apoiar troubleshooting inicial de indisponibilidades

---

## 📊 Métricas Monitoradas

### 🔹 Sistema Operacional

- `system.hostname` — Nome do host
- `system.uname` — Descrição completa do sistema
- `system.sw.arch` — Arquitetura do sistema operacional
- `system.localtime` — Hora local do sistema
- `system.uptime` — Tempo de atividade

Permite identificar:

- Alterações indevidas no hostname
- Drift de horário (problemas com NTP)
- Reinicializações inesperadas
- Mudanças estruturais do sistema

---

### 🔹 Processos e Threads

- `proc.num[]` — Número total de processos
- `perf_counter_en["\System\Threads"]` — Número total de threads

Permite identificar:

- Crescimento anormal de processos
- Vazamento de threads
- Indícios de sobrecarga do sistema
- Comportamento anômalo de aplicações

---

## 🚨 Estratégia de Alertas

### 🔹 Alteração de Hostname

Condição: change(system.hostname) and length(last(system.hostname)) > 0


Características:

- Severidade: INFO
- Fechamento manual habilitado
- Escopo: notice
- Detecta alteração no nome do sistema

---

### 🔹 Desincronização de Horário

Condição:

- Diferença maior que `{$SYSTEM.FUZZYTIME.MAX}` segundos em relação ao servidor Zabbix

Recovery expression dedicada para evitar flapping.

Características:

- Severidade: WARNING
- Fechamento manual habilitado
- Escopo: availability
- Indica problema de NTP ou drift de relógio

---

### 🔹 Reinicialização do Host

Condição: system.uptime < 5m

Recovery: system.uptime >= 10m


Características:

- Severidade: INFO
- Fechamento manual habilitado
- Indica reboot recente
- Janela de recuperação maior para evitar reabertura indevida

---

## 🧠 Estratégia Técnica Aplicada

- Template modular agregador
- Uso predominante de Zabbix Agent ativo
- Aplicação de `DISCARD_UNCHANGED_HEARTBEAT` para otimização de histórico
- Triggers com recovery expression dedicada
- Uso de `fuzzytime()` para validação robusta de sincronismo
- Separação clara entre camada base e módulos especializados

---

## 🔗 Dependência / Integração

Este template integra os seguintes módulos:

- Windows CPU by NOC
- Windows discovery disk by NOC
- Windows memory by NOC
- Triple Check by NOC

Funciona como camada base da stack Windows NOC.

---

## ⚙️ Tipo de Coleta

Compatível com:

- Zabbix Agent (ativo)

Coleta majoritariamente ativa visando escalabilidade e redução de carga no servidor.

---

## 🏷 Macros Utilizadas

- `{$SYSTEM.FUZZYTIME.MAX}` – Limite máximo permitido de diferença de horário (em segundos)

---

## 📈 Dashboard Incluído

Dashboard: **Processes**

Contém:

- Gráfico clássico — Número de processos
- Gráfico clássico — Número de threads

Permite visualização consolidada do comportamento do sistema operacional.

---

## 🏷 Versão

v1.0 – Estrutura base modular do stack Windows NOC com monitoramento estrutural do sistema operacional.
