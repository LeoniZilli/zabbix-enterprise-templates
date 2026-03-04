# Template - Windows Memory by NOC

## 📌 Visão Geral

Template modular responsável pelo monitoramento de memória física e memória swap em servidores Windows.

Este módulo compõe a stack Windows NOC e realiza monitoramento detalhado de utilização de RAM, paginação, swap, cache e indicadores de pressão de memória, utilizando coleta ativa via Zabbix Agent.

---

## 🎯 Objetivo

- Monitorar utilização de memória física
- Detectar pressão de memória sustentada
- Monitorar uso de swap
- Identificar sintomas de memory leak
- Classificar alertas por severidade
- Fornecer base para troubleshooting de performance

---

## 📊 Métricas Monitoradas

### 🔹 Memória Física

- `vm.memory.size[total]` — Memória total
- `vm.memory.size[used]` — Memória utilizada
- `vm.memory.size[available]` — Memória disponível
- `vm.memory.util` — Utilização percentual (calculado)

---

### 🔹 Swap

- `system.swap.size[,total]` — Swap total
- `system.swap.size[,used]` — Swap utilizado
- `system.swap.free` — Swap livre (calculado)
- `system.swap.pfree` — Percentual livre (dependente)

---

### 🔹 Indicadores Avançados

- `\Memory\Cache Bytes`
- `\Memory\Free System Page Table Entries`
- `\Memory\Page Faults/sec`
- `\Memory\Pages/sec`
- `\Memory\Pool Nonpaged Bytes`
- `\Paging file(_Total)\% Usage`

Esses contadores permitem identificar:

- Memory leak
- Paginação excessiva
- Pressão de memória
- Saturação de swap
- Problemas de desempenho relacionados à RAM

---

## 🚨 Estratégia de Alertas

O template utiliza validação por janela de tempo (`min()` e `max()`), recovery expression dedicada e dependência hierárquica entre severidades.

Escopo aplicado:

- capacity
- performance

---

### 🔹 WARNING — Utilização elevada (desabilitado por padrão)

Dispara quando:

    Utilização > {$MEMORY.UTIL.WARN} por {$MEMORY.UTIL.TIME_CHECK}

Severidade: WARNING  
Status: Desabilitado por padrão

---

### 🔹 AVERAGE — Alta utilização

Dispara quando:

    Utilização > {$MEMORY.UTIL.AVG} por {$MEMORY.UTIL.TIME_CHECK}

Recovery:

    Utilização < {$MEMORY.UTIL.AVG} por {$MEMORY.UTIL.TIME_REC}

Severidade: AVERAGE  
Dependente do nível HIGH  
Fechamento manual habilitado

---

### 🔹 HIGH — Saturação crítica

Dispara quando:

    Utilização > {$MEMORY.UTIL.HIGH} por {$MEMORY.UTIL.TIME_CHECK}

Recovery:

    Utilização < {$MEMORY.UTIL.HIGH} por {$MEMORY.UTIL.TIME_REC}

Severidade: HIGH  
Fechamento manual habilitado

Indica risco iminente de esgotamento de memória física.

---

### 🔹 SWAP — Baixo espaço livre (desabilitado por padrão)

Dispara quando:

    Swap livre <= {$SWAP.PFREE.MIN.AVG}% por {$SWAP.TIME_CHECK}

Recovery:

    Swap livre > {$SWAP.PFREE.MIN.AVG}% por {$SWAP.TIME_REC}

Severidade: AVERAGE  
Status: Desabilitado por padrão  
Ignorado automaticamente se não houver swap configurado.

---

## 🧠 Estratégia Técnica Aplicada

- Coleta ativa via Zabbix Agent
- Itens calculados para percentual de utilização
- Item dependente para cálculo de swap livre (%)
- Validação por janela de tempo
- Recovery expression dedicada
- Hierarquia de severidade
- Prevenção de flapping
- Separação modular (CPU, memória e disco isolados)

---

## 🔗 Integração

Este módulo compõe a stack Windows NOC junto com:

- Windows Base by NOC
- Windows CPU by NOC
- Windows Discovery Disk by NOC
- Triple Check

Responsável pela camada de gerenciamento de capacidade e performance de memória.

---

## ⚙️ Tipo de Coleta

Compatível com:

- Zabbix Agent (ativo)

Histórico padrão: 7 dias.

---

## 🏷 Macros Utilizadas

### Thresholds de Memória

| Macro | Default | Descrição |
|-------|---------|-----------|
| `{$MEMORY.UTIL.WARN}` | 90 | Limite WARNING (%) |
| `{$MEMORY.UTIL.AVG}` | 95 | Limite AVERAGE (%) |
| `{$MEMORY.UTIL.HIGH}` | 99 | Limite HIGH (%) |
| `{$MEMORY.UTIL.TIME_CHECK}` | 15m | Janela de disparo |
| `{$MEMORY.UTIL.TIME_REC}` | 1m | Janela de recuperação |

---

### Thresholds de Swap

| Macro | Default | Descrição |
|-------|---------|-----------|
| `{$SWAP.PFREE.MIN.AVG}` | 10 | Percentual mínimo livre (%) |
| `{$SWAP.TIME_CHECK}` | 15m | Janela de disparo |
| `{$SWAP.TIME_REC}` | 10m | Janela de recuperação |

---

## 📈 Dashboard Incluído

Dashboard: **Memory**

Contém:

- Gráfico clássico — Utilização de memória (%)
- Gráfico clássico — Uso de swap
- Base para análise histórica e troubleshooting

---

## 🏷 Versão

v1.0 – Monitoramento modular de memória física e swap com controle hierárquico de severidade e validação temporal.