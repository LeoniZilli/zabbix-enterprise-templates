# Template - Linux CPU by NOC

> NOC Monitoring Framework  
> Camada: Monitoramento de Recursos (Linux Stack)

---

## 📌 Visão Geral

Template modular responsável pelo monitoramento avançado de CPU em servidores Linux.

Desenvolvido para compor a stack Linux corporativa, permitindo análise precisa de saturação, carga e comportamento de processamento.

Projetado para ambientes de média e grande escala, com foco em redução de falso positivo e detecção de sobrecarga real.

---

## 🏗 Arquitetura / Escopo

**Camada 3 – Monitoramento de Recursos**

Este template é responsável exclusivamente pelo monitoramento de CPU dentro da stack Linux.

Sua separação do Linux Base permite:

1. Modularização da arquitetura  
2. Atualizações independentes  
3. Ajustes de threshold por ambiente  
4. Reutilização em múltiplas stacks  

Atua como módulo especializado na análise de processamento e capacidade computacional.

---

## 🎯 Objetivos

- Monitorar utilização real de CPU  
- Detectar saturação prolongada  
- Identificar gargalos de processamento  
- Auxiliar na análise de capacity planning  
- Reduzir incidentes por sobrecarga  

---

## 📊 Métricas Monitoradas

### 🔹 Utilização de CPU

- Percentual de uso total  
- Uso por modo (`user`, `system`, `idle`, `iowait`, etc.)  
- Detecção de consumo elevado sustentado  

---

### 🔹 Load Average

- Load 1 minuto  
- Load 5 minutos  
- Load 15 minutos  

Essas métricas permitem diferenciar:

- Pico momentâneo  
- Sobrecarga sustentada  
- Saturação real do host  
- Pressão de processamento em múltiplos núcleos  

---

## 🚨 Estratégia de Alertas

| Severidade | Condição | Observação |
|------------|----------|------------|
| Warning | Load acima do baseline por período curto | Pico controlado |
| High | CPU > threshold crítico por período contínuo | Saturação sustentada |
| Disaster | Saturação prolongada impactando estabilidade | Impacto operacional |

### Lógica Aplicada

- Avaliação temporal para evitar alerta por picos momentâneos  
- Threshold configurável via macro  
- Análise combinada de Load e utilização percentual  
- Escalonamento progressivo de severidade  
- Recovery expression dedicada  

A estratégia reduz falso positivo e garante diagnóstico mais assertivo.

---

## ⚙️ Macros Utilizadas

| Macro | Descrição | Default Sugerido |
|-------|-----------|------------------|
| `{$CPU.UTIL.WARN}` | Limite de alerta Warning (%) | 75 |
| `{$CPU.UTIL.HIGH}` | Limite crítico (%) | 90 |
| `{$CPU.UTIL.TIME_CHECK}` | Tempo para disparo | 5m |
| `{$CPU.UTIL.TIME_REC}` | Tempo para recuperação | 5m |
| `{$CPU.LOAD.BASELINE}` | Baseline proporcional ao número de CPUs | Auto |

Permite parametrização por host conforme criticidade da aplicação.

---

## 🔗 Dependências / Integração

### Pode ser utilizado com
- Linux Base by NOC

Faz parte da stack modular Linux NOC.

Não possui dependências obrigatórias.

---

## 📦 Requisitos

- Zabbix Server 6.x ou superior  
- Zabbix Agent (ativo ou passivo)  
- Ambiente Linux com suporte a `system.cpu.util` e `system.cpu.load`  

Recomendado uso em modo ativo para ambientes de grande escala.

---

## 🧠 Estratégia Técnica

Este template foi estruturado para garantir análise confiável de performance de CPU.

Aplica:

- Coleta via itens nativos do agente  
- Avaliação temporal para evitar flapping  
- Escalonamento progressivo de severidade  
- Threshold parametrizável por macro  
- Separação completa do Linux Base  

Permite evolução independente dos limites sem impactar outros módulos da stack.

---

## 📈 Benefícios Operacionais

- Visibilidade clara de saturação  
- Melhor diagnóstico de lentidão  
- Base sólida para decisões de upgrade  
- Redução de incidentes críticos  
- Monitoramento padronizado de CPU em ambiente enterprise  
- Arquitetura modular e escalável  

---

## 🏷 Versão

v1.0 – Estrutura modular inicial do stack Linux (CPU Module)