# Template - Linux Network Interfaces by NOC

> NOC Monitoring Framework  
> Camada: Monitoramento de Rede (Linux Stack)

---

## 📌 Visão Geral

Template modular responsável pelo monitoramento avançado de interfaces de rede em servidores Linux.

Foi desenvolvido para compor a stack Linux corporativa, oferecendo visibilidade completa de tráfego, erros, estado operacional e utilização de banda por interface.

Utiliza Low Level Discovery (LLD) para descoberta automática das interfaces do host, permitindo escalabilidade e aplicação dinâmica em ambientes de grande porte.

---

## 🏗 Arquitetura / Escopo

**Camada 3 – Monitoramento de Rede**

Este template é responsável exclusivamente pela camada de rede dentro da stack Linux.

Sua separação do Linux Base permite:

1. Modularização da arquitetura  
2. Aplicação seletiva conforme criticidade  
3. Manutenção independente  
4. Reutilização em múltiplos ambientes  

Atua como módulo especializado de análise de tráfego e integridade de interfaces.

---

## 🎯 Objetivos

- Monitorar tráfego de rede por interface  
- Detectar saturação de banda  
- Identificar alta taxa de erros  
- Detectar queda de link (Link Down)  
- Monitorar alteração de velocidade da interface  
- Auxiliar em troubleshooting de rede  
- Apoiar decisões de capacity planning  

---

## 📊 Métricas Monitoradas

### 🔹 Tráfego

- Bits recebidos (bps)  
- Bits enviados (bps)  
- Pacotes recebidos  
- Pacotes enviados  

### 🔹 Erros e Descartes

- Erros de entrada  
- Erros de saída  
- Pacotes descartados (inbound)  
- Pacotes descartados (outbound)  

### 🔹 Estado Operacional

- Status da interface (up/down)  
- Tipo da interface  
- Velocidade atual do link  

Essas métricas permitem diferenciar:

- Pico momentâneo de tráfego  
- Saturação sustentada  
- Problemas físicos de link  
- Problemas de negociação de velocidade  
- Problemas de camada física (erros elevados)  

---

## 🚨 Estratégia de Alertas

| Severidade | Condição | Observação |
|------------|----------|------------|
| Average | Interface com Link Down | Interface ativa via {$IFCONTROL} |
| Warning | Alta utilização de banda | Threshold configurável |
| Warning | Alta taxa de erros | Recuperação progressiva (80%) |
| Info | Redução de velocidade do link | Alteração detectada |

### Lógica Aplicada

- Dependência de triggers para evitar alerta em cascata quando link está down  
- Controle por macro `{$IFCONTROL}` para desativar interfaces específicas  
- Recuperação progressiva (80% do threshold) para evitar flapping  
- Proteção contra falso positivo em interfaces permanentemente desligadas  
- Aplicação de macros contextuais por interface  

---

## ⚙️ Macros Utilizadas

| Macro | Descrição | Default |
|-------|-----------|----------|
| `{$IF.UTIL.MAX}` | Threshold máximo de utilização da interface | 90 |
| `{$IF.ERRORS.WARN}` | Threshold de erros por segundo | 10 |
| `{$IFCONTROL}` | Controla se a interface deve gerar alerta (1 = ativa) | 1 |
| `{$NET.IF.IFNAME.MATCHES}` | Regex para inclusão de interfaces | .* |
| `{$NET.IF.IFNAME.NOT_MATCHES}` | Regex para exclusão (loopback, docker, veth, bridges) | predefinido |

Permite controle granular por interface específica utilizando macro contextual: {$IF.UTIL.MAX:"eth0"} = 80


---

## 🔗 Dependências / Integração

### Pode ser utilizado com
- Linux Base by NOC

Não possui dependência obrigatória de outros templates.

Pode ser aplicado em qualquer host Linux monitorado via Agent.

---

## 📦 Requisitos

- Zabbix Server 6.x ou superior  
- Zabbix Agent (modo ativo recomendado)  
- Permissão para coleta de métricas de rede  
- Ambiente Linux com suporte a `net.if.discovery`  

---

## 🧠 Estratégia Técnica

Este template foi separado do Linux Base com objetivo de manter arquitetura modular e escalável.

Aplica:

- Low Level Discovery (`net.if.discovery`)  
- Preprocessing `CHANGE_PER_SECOND`  
- Conversão para bits por segundo via multiplicador  
- Valuemaps para padronização de estados  
- Macros contextuais por interface  
- Controle por regex para inclusão/exclusão dinâmica  

Recomendado uso em modo ativo (`ZABBIX_ACTIVE`) para ambientes de grande escala.

---

## 📈 Benefícios Operacionais

- Visibilidade completa de tráfego por interface  
- Redução de incidentes por saturação  
- Identificação rápida de falhas físicas  
- Diagnóstico preciso de problemas de rede  
- Controle fino por interface crítica  
- Dashboards automáticos via descoberta dinâmica  
- Arquitetura modular e escalável  

---

## 🏷 Versão

v1.0 – Estrutura modular inicial do stack Linux (Network Module)
