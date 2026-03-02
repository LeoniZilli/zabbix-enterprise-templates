# Template - Linux Memory by NOC

> NOC Monitoring Framework  
> Camada: Monitoramento de Recursos (Linux Stack)

---

## 📌 Visão Geral

Template modular responsável pelo monitoramento avançado de memória RAM e SWAP em servidores Linux.

Desenvolvido para compor a stack Linux corporativa, permitindo análise precisa de consumo de memória, pressão de recursos e risco de esgotamento.

Baseado na estrutura oficial do Zabbix (Templator) e adaptado ao padrão arquitetural NOC.

---

## 🏗 Arquitetura / Escopo

**Camada 3 – Monitoramento de Recursos**

Este template é responsável exclusivamente pelo monitoramento de memória dentro da stack Linux.

Sua separação do Linux Base permite:

1. Modularização da arquitetura  
2. Aplicação seletiva conforme criticidade  
3. Manutenção independente  
4. Reutilização em múltiplos ambientes  

Atua como módulo especializado na análise de consumo de memória física e virtual.

---

## 🎯 Objetivos

- Monitorar utilização real de memória RAM  
- Detectar consumo excessivo sustentado  
- Identificar risco de esgotamento de memória  
- Monitorar utilização de SWAP  
- Detectar pressão de memória física  
- Apoiar capacity planning  
- Permitir parametrização por host via macros  

---

## 📊 Métricas Monitoradas

### 🔹 Memória RAM

- Memória total (`vm.memory.size[total]`)  
- Memória disponível (`vm.memory.size[available]`)  
- Memória disponível (%)  
- Utilização calculada automaticamente:  
  `vm.memory.utilization = 100 - pavailable`  

O item `vm.memory.utilization` é dependente e utiliza pré-processamento em JavaScript no servidor, reduzindo carga no agente.

Permite identificar:

- Crescimento sustentado de consumo  
- Saturação progressiva  
- Risco de indisponibilidade  
- Gargalos por falta de memória  

---

### 🔹 SWAP

- Swap total  
- Swap livre (bytes)  
- Swap livre (%)  

Permite identificar:

- Uso excessivo de swap  
- Pressão de memória  
- Degradação de performance por paginação  

---

## 🚨 Estratégia de Alertas

### 🔹 Memória RAM

| Severidade | Condição | Observação |
|------------|----------|------------|
| Warning | Utilização > `{$MEMORY.UTIL.WARN}` | Janela configurável |
| Average | Utilização > `{$MEMORY.UTIL.AVG}` | Dependente do nível anterior |
| High | Utilização > `{$MEMORY.UTIL.HIGH}` | Severidade crítica |

### Lógica Aplicada (RAM)

- Janela temporal configurável (`{$MEMORY.UTIL.TIME_CHECK}`)  
- Recovery expression dedicada (`{$MEMORY.UTIL.TIME_REC}`)  
- Validação de memória disponível e total antes do disparo  
- Dependência entre níveis para evitar alerta duplicado  
- Fechamento manual habilitado  

---

### 🔹 SWAP

| Severidade | Condição | Observação |
|------------|----------|------------|
| Average | Swap livre (%) ≤ `{$SWAP.PFREE.MIN.AVG}` | Swap total > 0 |

### Lógica Aplicada (SWAP)

- Trigger desabilitada por padrão  
- Janela temporal configurável (`{$SWAP.TIME_CHECK}`)  
- Recovery independente (`{$SWAP.TIME_REC}`)  
- Dependência da trigger crítica de memória  

---

## ⚙️ Macros Utilizadas

| Macro | Descrição | Default Sugerido |
|-------|-----------|------------------|
| `{$MEMORY.UTIL.WARN}` | Limite de alerta Warning | 75 |
| `{$MEMORY.UTIL.AVG}` | Limite de alerta Average | 85 |
| `{$MEMORY.UTIL.HIGH}` | Limite crítico | 90 |
| `{$MEMORY.UTIL.TIME_CHECK}` | Tempo para disparo | 5m |
| `{$MEMORY.UTIL.TIME_REC}` | Tempo para recuperação | 5m |
| `{$SWAP.PFREE.MIN.AVG}` | Percentual mínimo de swap livre | 20 |
| `{$SWAP.TIME_CHECK}` | Tempo para disparo de swap | 5m |
| `{$SWAP.TIME_REC}` | Tempo para recuperação de swap | 5m |

Permite parametrização granular por host conforme criticidade do ambiente.

---

## 🔗 Dependências / Integração

### Pode ser utilizado com
- Linux Base by NOC

Faz parte da stack modular Linux NOC.

Não possui dependência obrigatória de outros templates.

---

## 📦 Requisitos

- Zabbix Server 6.x ou superior  
- Zabbix Agent (ativo ou passivo)  
- Ambiente Linux compatível com `vm.memory.size`  

Coleta baseada exclusivamente em itens nativos do agente.

---

## 🧠 Estratégia Técnica

Este template foi estruturado para ambientes corporativos de média e grande escala.

Aplica:

- Item dependente para cálculo de utilização  
- Pré-processamento em JavaScript no servidor  
- Macros parametrizáveis por host  
- Separação clara entre memória física e swap  
- Dependência entre triggers para evitar ruído  
- Minimização de carga no agente  

Arquitetura preparada para ambientes de alta escala e monitoramento massivo.

---

## 📈 Benefícios Operacionais

- Visibilidade precisa do consumo real de memória  
- Redução de incidentes por esgotamento de RAM  
- Detecção antecipada de pressão de recursos  
- Apoio a capacity planning  
- Redução de falso positivo  
- Padronização do monitoramento de recursos Linux  
- Arquitetura modular e escalável  

---

## 📊 Itens Visuais Incluídos

- Linux: Memory usage  
- Linux: Memory utilization  
- Linux: Swap usage  

Inclui dashboard dedicado para visão consolidada de memória.

---

## 🏷 Versão

v1.0 – Estrutura modular inicial do stack Linux com monitoramento avançado de memória RAM e SWAP