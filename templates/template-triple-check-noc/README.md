# Template - Triple Check by NOC

> NOC Monitoring Framework  
> Camada: Validação Estrutural de Disponibilidade

---

## 📌 Visão Geral

Template responsável pela validação de disponibilidade em múltiplas camadas do host.

Aplica verificação cruzada entre conectividade ICMP, disponibilidade da porta do agente e envio efetivo de dados ao Zabbix, implementando o conceito de validação em três níveis ("Triple Check").

Seu objetivo principal é reduzir falso positivo e aumentar a precisão do diagnóstico operacional.

---

## 🏗 Arquitetura / Escopo

**Camada 2 – Validação Estrutural**

Este template atua acima da camada de conectividade (ICMP), validando:

1. Conectividade de rede  
2. Disponibilidade da porta do agente  
3. Funcionamento real do agente  

Ele complementa a validação básica de host, garantindo que o servidor esteja não apenas acessível, mas operacionalmente monitorável.

---

## 🎯 Objetivos

- Validar conectividade de rede via ICMP  
- Validar disponibilidade da porta do Zabbix Agent  
- Validar envio real de dados pelo agente  
- Detectar agente travado ou congelado  
- Reduzir falso positivo de indisponibilidade  
- Melhorar assertividade operacional do NOC  

---

## 📊 Métricas Monitoradas

### 🔹 Conectividade

- ICMP Ping (herdado do template ICMP Ping by NOC)
- Disponibilidade da porta TCP do agente (`{$ZABBIX.AGENT.PORT}`)

### 🔹 Status do Agente

- `agent.ping`
- `agent.hostname`
- `agent.version`
- `agent.variant`

Essas métricas permitem diferenciar:

- Host desligado  
- Host inacessível por rede  
- Porta bloqueada por firewall  
- Agente parado  
- Agente travado (sem envio de dados)  
- Problemas intermitentes de comunicação  

---

## 🚨 Estratégia de Alertas

| Severidade | Condição | Observação |
|------------|----------|------------|
| High | Porta do agente não responde | ICMP responde |
| Average | Agente não envia dados (`nodata()`) | ICMP e porta respondem |

### Lógica Aplicada

1. Se ICMP falha → Problema de conectividade.  
2. Se ICMP responde mas porta do agente falha → Agente parado ou bloqueio de firewall.  
3. Se ICMP responde + porta responde + ausência de `agent.ping` → Agente travado.  

Utiliza dependência de trigger para evitar alertas redundantes.

---

## ⚙️ Macros Utilizadas

| Macro | Descrição | Default |
|-------|-----------|----------|
| `{$ZABBIX.AGENT.PORT}` | Porta utilizada pelo agente | 10050 |
| `{$NET.TCP.SERVICE.TIME_CHECK_AVG}` | Janela de análise da trigger TCP | 5m |

Permite ajuste fino do tempo de validação antes da abertura de incidente.

---

## 🔗 Dependências / Integração

### Utiliza
- ICMP Ping by NOC

### Pode ser aplicado em
- Servidores Linux  
- Servidores Windows  
- Dispositivos monitorados via Agent  

Não depende de templates Linux específicos.

---

## 📦 Requisitos

- Zabbix Server 6.x ou superior  
- Zabbix Agent (ativo ou passivo)  
- Permissão de checagem TCP na porta configurada  
- Template ICMP aplicado previamente  

---

## 🧠 Estratégia Técnica

Este template implementa validação em múltiplas camadas para aumentar a precisão diagnóstica.

Aplica:

- Checagem TCP ativa (`net.tcp.service`)
- Validação de ICMP (herdado)
- Função `nodata()` para detecção de ausência de envio
- Macro configurável para janela de análise
- `recovery_expression` para normalização controlada
- Valuemaps para padronização de estados

Implementa correlação básica dentro do próprio template, reduzindo ruído operacional.

---

## 📈 Benefícios Operacionais

- Redução significativa de falso positivo  
- Diagnóstico mais rápido de indisponibilidade  
- Diferencia problema de rede vs problema de agente  
- Identificação de agente congelado  
- Padronização da camada estrutural da monitoração  
- Aumento da maturidade operacional do NOC  

---

## 🏷 Versão

v1.0 – Estrutura inicial do modelo Triple Validation (ICMP + TCP + Agent)