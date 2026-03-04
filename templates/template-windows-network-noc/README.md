# Template - Windows Network by NOC

## 📌 Visão Geral

Template modular responsável pelo monitoramento avançado de interfaces de rede em servidores Windows.

Realiza descoberta automática (LLD) das interfaces físicas ativas via WMI, monitorando tráfego, erros, descartes, velocidade, status operacional e variações de link.

Coleta ativa via Zabbix Agent.

---

## 🎯 Objetivo

- Descobrir automaticamente interfaces físicas ativas
- Monitorar tráfego de entrada e saída
- Detectar saturação de banda
- Identificar erros e descartes excessivos
- Detectar queda de link
- Identificar downgrade de velocidade
- Controlar criticidade por interface

---

## 🔎 Descoberta de Interfaces (LLD)

A descoberta é baseada na consulta WMI:

win32_networkadapter  
PhysicalAdapter=True  
NetConnectionStatus>0  

São descobertas apenas interfaces físicas ativas.

### Filtros aplicados via macros:

- `{$NET.IF.IFNAME.MATCHES}`
- `{$NET.IF.IFNAME.NOT_MATCHES}`
- `{$NET.IF.IFALIAS.MATCHES}`
- `{$NET.IF.IFDESCR.MATCHES}`

Por padrão, interfaces virtuais e adaptadores irrelevantes são ignorados.

---

## 📊 Métricas Monitoradas por Interface

### 🔹 Tráfego

- `net.if.in["GUID"]` — Bits recebidos (bps)
- `net.if.out["GUID"]` — Bits enviados (bps)

Conversão automática para bits por segundo.

---

### 🔹 Erros

- `net.if.in["GUID",errors]`
- `net.if.out["GUID",errors]`

Calculados por segundo (CHANGE_PER_SECOND).

---

### 🔹 Pacotes Descartados

- `net.if.in["GUID",dropped]`
- `net.if.out["GUID",dropped]`

---

### 🔹 Status Operacional

- `net.if.status["GUID"]`

Mapeado via ValueMap:

- 2 = Connected
- 7 = Media Disconnected
- Outros estados conforme Win32_NetworkAdapter

---

### 🔹 Velocidade

- `net.if.speed["GUID"]`

Obtida via WMI.
Tratamento automático para valores inválidos.

---

### 🔹 Tipo de Interface

- `net.if.type["GUID"]`

Mapeado via AdapterTypeId.

---

## 🚨 Estratégia de Alertas

Escopos utilizados:

- availability
- performance
- capacity

---

### 🔹 Link Down

Dispara quando:

- Interface controlada (`{$IFCONTROL}`=1)
- Status diferente de Connected
- Mudança real de estado

Recovery:

- Status volta para Connected
- Ou controle desativado

Severidade: AVERAGE  
Fechamento manual habilitado  
Evita disparo em interfaces permanentemente desligadas.

---

### 🔹 High Bandwidth Usage

Dispara quando:

Média 15m de tráfego > {$IF.UTIL.MAX}% da velocidade da interface  

Recovery:

- Retorna abaixo de (limite - 3%)

Severidade: WARNING  
Baseado na velocidade real da interface.

---

### 🔹 High Error Rate

Dispara quando:

Erros > {$IF.ERRORS.WARN} por 5 minutos  

Recovery:

- Abaixo de 80% do threshold

Severidade: WARNING

---

### 🔹 Ethernet Speed Downgrade

Dispara quando:

- A velocidade detectada reduz em relação ao valor anterior
- Interface continua conectada

Severidade: INFO  
Indica possível problema de autonegociação.

---

## 🧠 Estratégia Técnica Aplicada

- Descoberta automática baseada em WMI
- Itens dependentes para status e velocidade
- Preprocessamento via JSONPATH
- Conversão automática para bps
- Cálculo por segundo para tráfego e erros
- Controle por macro contextual (`{$IFCONTROL:"Interface"}`)
- Recovery expression dedicada
- Dependência hierárquica com Link Down
- Prevenção de flapping

---

## 🏷 Macros Utilizadas

### Controle Geral

| Macro | Default | Função |
|-------|---------|--------|
| `{$IFCONTROL}` | 1 | Controla se interface é monitorada |
| `{$IF.UTIL.MAX}` | 90 | Percentual máximo de utilização |
| `{$IF.ERRORS.WARN}` | 2 | Limite de erros por segundo |

---

### Filtros de Descoberta

| Macro | Default |
|-------|---------|
| `{$NET.IF.IFNAME.MATCHES}` | .* |
| `{$NET.IF.IFNAME.NOT_MATCHES}` | Miniport\|Virtual\|Teredo\|Kernel\|Loopback\|Bluetooth\|HTTPS\|6to4\|QoS\|Layer |
| `{$NET.IF.IFALIAS.MATCHES}` | .* |
| `{$NET.IF.IFDESCR.MATCHES}` | .* |

---

## 📈 Dashboard Incluído

Dashboard: **Network interfaces**

Contém:

- Gráfico protótipo por interface
- Tráfego IN/OUT
- Erros IN/OUT
- Pacotes descartados
- Visualização consolidada por interface

---

## 🔗 Integração

Este módulo compõe a stack Windows NOC junto com:

- Windows Base by NOC
- Windows CPU by NOC
- Windows Memory by NOC
- Windows Discovery Disk by NOC
- Triple Check

Responsável pela camada de disponibilidade e performance de rede.

---

## ⚙️ Tipo de Coleta

Compatível com:

- Zabbix Agent (ativo)

Histórico padrão: 7 dias.

---

## 🏷 Versão

v1.0 – Monitoramento completo de interfaces físicas Windows com descoberta automática, controle contextual e análise de performance.