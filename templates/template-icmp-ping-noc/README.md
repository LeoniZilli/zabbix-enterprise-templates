# Template - ICMP Ping by NOC

> NOC Monitoring Framework  
> Camada: Conectividade

---

## 📌 Visão Geral

Template responsável pelo monitoramento básico de conectividade via ICMP, atuando como primeira camada de validação de disponibilidade de hosts.

Permite rápida identificação de indisponibilidade, degradação de rede ou aumento de latência.

---

## 🏗 Arquitetura / Escopo

Este template pertence à **camada de conectividade** da stack de monitoramento.

Ele funciona de forma independente do sistema operacional e não depende de agente, sendo executado diretamente pelo Zabbix Server ou Proxy.

É recomendado como **base de dependência para outros templates**, evitando alertas em cascata quando o host estiver indisponível.

---

## 🎯 Objetivos

- Validar disponibilidade do host via ICMP
- Detectar perda parcial ou total de pacotes
- Monitorar latência média de resposta
- Servir como base de dependência para outros templates
- Reduzir ruído operacional em eventos de indisponibilidade

---

## 📊 Métricas Monitoradas

### 🔹 Disponibilidade ICMP
- Status de resposta (`icmpping`)
- Percentual de perda de pacotes (`icmppingloss`)

### 🔹 Latência
- Tempo médio de resposta (`icmppingsec`)

Essas métricas permitem diferenciar:

- Host desligado
- Problema de rede
- Alta latência
- Intermitência de conectividade

---

## 🚨 Estratégia de Alertas

| Severidade | Condição | Observação |
|------------|----------|------------|
| Warning | Perda de pacotes > {$ICMP_LOSS_WARN} e < 100% | Indica degradação parcial |
| Warning | Tempo médio > {$ICMP_RESPONSE_TIME_WARN} | Latência elevada contínua |
| High | Host sem resposta por {$ICMP.PING.HIGH} | Indisponibilidade confirmada |

Lógica aplicada:

1. A indisponibilidade total gera o alerta principal (High).
2. Perda parcial de pacotes é acionada apenas quando não há indisponibilidade total.
3. Alerta de latência depende da inexistência de perda crítica ou indisponibilidade.
4. Uso de dependências evita alertas em cascata.

---

## ⚙️ Macros Utilizadas

| Macro | Descrição | Default |
|-------|-----------|----------|
| {$ICMP.PACKETS} | Quantidade de pacotes enviados no ping | 4 |
| {$ICMP.TIMEOUT} | Timeout por pacote (ms) | 200 |
| {$ICMP.TIME_CHECK} | Janela de análise para triggers | 5m |
| {$ICMP.PING.HIGH} | Tempo sem resposta para alarmar | 1m |
| {$ICMP.PING.HIGH.REC} | Tempo para recuperação do alerta | 2m |
| {$ICMP_LOSS_WARN} | Percentual de perda para Warning | 50 |
| {$ICMP_RESPONSE_TIME_WARN} | Tempo médio de resposta (segundos) | 0.15 |

---

## 🔗 Dependências / Integração

Utiliza:
- Monitoramento ICMP nativo do Zabbix Server

Pode ser aplicado em:
- Servidores Linux
- Servidores Windows
- Equipamentos de rede
- Appliances
- Dispositivos diversos

Pode ser vinculado a:
- Linux Base by NOC
- Windows Base by NOC
- Templates de aplicação

---

## 📦 Requisitos

- Zabbix >= 6.x
- ICMP habilitado no Zabbix Server ou Proxy
- Permissão de rede para envio de pacotes ICMP

Não depende de Zabbix Agent.

---

## 🧠 Estratégia Técnica

Este template foi isolado como módulo independente para:

- Permitir reutilização universal
- Servir como camada primária de disponibilidade
- Reduzir ruído operacional
- Facilitar troubleshooting inicial
- Permitir dependência hierárquica em outros templates

Separar conectividade da camada de sistema melhora organização arquitetural e escalabilidade.

---

## 📈 Benefícios Operacionais

- Detecção rápida de indisponibilidade
- Redução de troubleshooting inicial
- Base sólida para análise de SLA
- Diminuição de alertas redundantes
- Melhor organização modular da stack de monitoramento

---

## 🏷 Versão

v1.0 – Template modular de conectividade ICMP