# Template - Linux Base by NOC

> NOC Monitoring Framework  
> Camada: Baseline Estrutural (Linux Stack)

---

## 📌 Visão Geral

Template base responsável por consolidar o monitoramento estrutural essencial de servidores Linux em ambiente corporativo.

Utiliza modelo modular baseado em **Linked Templates (herança nativa do Zabbix)**, permitindo separação clara de responsabilidades, escalabilidade e manutenção simplificada.

Atua como template principal aplicado aos hosts Linux dentro da stack NOC.

---

## 🏗 Arquitetura / Escopo

**Camada 4 – Consolidação Estrutural**

O Linux Base é responsável por:

1. Consolidar módulos especializados  
2. Estabelecer baseline padrão corporativo  
3. Centralizar informações estruturais do sistema  
4. Fornecer eventos informativos de ambiente  

Os recursos críticos (CPU, memória e disco) são tratados nos módulos especializados vinculados.

---

## 🔗 Templates Vinculados (Linked Templates)

O **Linux Base by NOC** utiliza os seguintes templates vinculados:

| Template Vinculado | Responsabilidade |
|--------------------|------------------|
| Linux CPU by NOC | Monitoramento de CPU e Load |
| Linux discovery disk by NOC | Descoberta e uso de disco |
| Linux memory by NOC | Monitoramento de memória e swap |
| Zabbix Agent - Triple Check by NOC | Validação estrutural do agente |

A arquitetura é totalmente modular e desacoplada.

---

## 📊 Itens Nativos do Template Base

Além dos templates vinculados, o Linux Base inclui monitoramento estrutural do sistema:

### 🔹 Limites do Kernel

- `kernel.maxfiles` – Máximo de file descriptors  
- `kernel.maxproc` – Máximo de processos  

---

### 🔹 Processos

- `proc.num` – Total de processos  
- `proc.num[,,run]` – Processos em execução  
- `system.users.num` – Usuários logados  

Inclui gráfico clássico:

- **Linux: Processes**

---

### 🔹 Sistema Operacional

- `system.sw.os` – Sistema operacional  
- `system.sw.arch` – Arquitetura  
- `system.sw.packages` – Pacotes instalados  
- `system.uname` – Descrição do sistema  

---

### 🔹 Estado do Sistema

- `system.boottime` – Boot time  
- `system.uptime` – Uptime  
- `system.hostname` – Hostname  
- `system.localtime` – Hora local  

---

### 🔹 Integridade

- `vfs.file.cksum[/etc/passwd,sha256]` – Checksum do arquivo `/etc/passwd`

---

## 🚨 Estratégia de Alertas

O template possui eventos informativos e de disponibilidade estrutural:

| Evento | Severidade | Descrição |
|--------|------------|------------|
| Alteração de hostname | Info | Mudança no nome do sistema |
| Uptime < 10m | Info | Host reiniciado |
| Desvio de horário | Warning | Diferença maior que `{$SYSTEM.FUZZYTIME.MAX}` |

### Lógica Aplicada

- Uso de `change()` para detectar alteração de hostname  
- Uso de `fuzzytime()` para verificação de sincronização de horário  
- Uso de recovery expression para evitar flapping  
- Fechamento manual habilitado para eventos informativos  

O foco do template base é gerar eventos estruturais acionáveis, não alertas de consumo de recurso.

---

## ⚙️ Macros Utilizadas

| Macro | Descrição | Default |
|-------|-----------|----------|
| `{$KERNEL.MAXFILES.MIN}` | Valor mínimo esperado para file descriptors | 256 |
| `{$KERNEL.MAXPROC.MIN}` | Valor mínimo esperado para processos | 1024 |
| `{$SYSTEM.FUZZYTIME.MAX}` | Tolerância máxima de diferença de horário (segundos) | 60 |

---

## 📊 Dashboard Incluído

Inclui dashboard nativo:

### 🔹 Processes
- Gráfico clássico com:
  - Total de processos  
  - Processos em execução  

---

## 📦 Requisitos

- Zabbix Server 6.x  
- Zabbix Agent configurado em modo **ativo (ZABBIX_ACTIVE)**  
- Templates vinculados previamente importados  

---

## 🧠 Estratégia Técnica

A modularização foi adotada para:

- Permitir manutenção isolada de CPU, memória e disco  
- Reduzir impacto de alterações  
- Facilitar troubleshooting  
- Padronizar monitoramento em grande escala  
- Garantir escalabilidade para centenas ou milhares de VMs  

Todos os itens utilizam `DISCARD_UNCHANGED_HEARTBEAT` quando aplicável, reduzindo volume de dados armazenados.

Arquitetura alinhada a boas práticas de observabilidade corporativa.

---

## 📈 Benefícios Operacionais

- Baseline estruturado para todos os servidores Linux  
- Monitoramento organizacional padronizado  
- Eventos informativos relevantes (reboot, hostname, horário)  
- Redução de ruído operacional  
- Arquitetura modular e escalável  

---

## 🏷 Versão

v1.0 – Estrutura modular Linux consolidada