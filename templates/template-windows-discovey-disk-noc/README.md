# Template - Windows Discovery Disk by NOC

## 📌 Visão Geral

Template modular responsável pela descoberta automática e monitoramento de volumes de disco em servidores Windows.

Baseado no mecanismo de Low-Level Discovery (LLD) do Zabbix, este módulo identifica dinamicamente os filesystems montados e cria itens, triggers e gráficos automaticamente para cada volume detectado.

Gerado inicialmente via Templator oficial do Zabbix e adaptado ao padrão arquitetural NOC.

---

## 🎯 Objetivo

- Descobrir automaticamente discos montados
- Monitorar utilização percentual de cada volume
- Monitorar espaço total e utilizado
- Detectar risco de esgotamento de armazenamento
- Aplicar alertas hierárquicos por severidade
- Permitir customização por volume via macros com contexto

---

## 🔍 Descoberta Automática (LLD)

### Regra de Discovery

- Key: `vfs.fs.discovery`
- Tipo: Zabbix Agent (ativo)
- Intervalo: 1h

### Filtros Aplicados

A descoberta utiliza filtros baseados em macros para:

- Tipo de drive (`{#FSDRIVETYPE}`)
- Nome do filesystem (`{#FSNAME}`)
- Tipo de filesystem (`{#FSTYPE}`)

Permite:

- Monitorar apenas discos do tipo `fixed`
- Excluir pseudo-filesystems
- Customizar escopo no nível de host ou template

---

## 📊 Itens Criados por Volume

Para cada volume descoberto:

### 🔹 Utilização Percentual

- `vfs.fs.size[{#FSNAME},pused]`
- Unidade: %
- Histórico: 7 dias

### 🔹 Espaço Total

- `vfs.fs.size[{#FSNAME},total]`
- Unidade: Bytes
- Histórico: 7 dias

### 🔹 Espaço Utilizado

- `vfs.fs.size[{#FSNAME},used]`
- Unidade: Bytes
- Histórico: 7 dias

Tags aplicadas:

- component: storage
- filesystem: {#FSNAME}

---

## 🚨 Estratégia de Alertas

Triggers baseadas em percentual de utilização, com dependência hierárquica e recovery expression dedicada.

Todas possuem:

- Escopo: capacity
- Fechamento manual habilitado
- Validação de consistência (used e total devem existir)

---

### 🔹 WARNING — Espaço livre inferior a 10%

Dispara quando:

    last(pused) >= {$VFS.FS.SIZE.PUSED_WARN:"{#FSNAME}"}

Recovery:

    max(pused, 5m) < {$VFS.FS.SIZE.PUSED_WARN:"{#FSNAME}"}

Severidade: WARNING

---

### 🔹 HIGH — Espaço livre inferior a 5%

Dispara quando:

    last(pused) >= {$VFS.FS.SIZE.PUSED_HIGH:"{#FSNAME}"}

Recovery:

    max(pused, 5m) < {$VFS.FS.SIZE.PUSED_HIGH:"{#FSNAME}"}

Severidade: HIGH  
Dependente do nível DISASTER

---

### 🔹 DISASTER — Espaço livre inferior a 1%

Dispara quando:

    last(pused) >= {$VFS.FS.SIZE.PUSED_DIST:"{#FSNAME}"}

Recovery:

    max(pused, 5m) < {$VFS.FS.SIZE.PUSED_DIST:"{#FSNAME}"}

Severidade: DISASTER

Indica risco iminente de indisponibilidade por esgotamento de armazenamento.

---

## 🧠 Estratégia Técnica Aplicada

- Low-Level Discovery (LLD)
- Criação automática de itens, triggers e gráficos
- Hierarquia de severidade com dependência
- Uso de macros com contexto por volume
- Recovery expression dedicada
- Proteção contra falso-positivo
- Monitoramento baseado em percentual (modelo recomendado)

---

## 🔗 Integração

Este módulo compõe a stack Windows NOC junto com:

- Windows Base by NOC
- Windows CPU by NOC
- Windows Memory by NOC
- Triple Check

Responsável pela camada de capacity management.

---

## ⚙️ Tipo de Coleta

Compatível com:

- Zabbix Agent (ativo)

Descoberta executada a cada 1 hora.

---

## 🏷 Macros Utilizadas

### Thresholds Percentuais

| Macro | Default | Descrição |
|-------|---------|-----------|
| `{$VFS.FS.SIZE.PUSED_WARN}` | 90 | Limite WARNING (%) |
| `{$VFS.FS.SIZE.PUSED_HIGH}` | 95 | Limite HIGH (%) |
| `{$VFS.FS.SIZE.PUSED_DIST}` | 99 | Limite DISASTER (%) |

### Filtros de Discovery

| Macro | Default | Descrição |
|-------|---------|-----------|
| `{$VFS.FS.FSDRIVETYPE.MATCHES}` | fixed | Tipos de disco permitidos |
| `{$VFS.FS.FSDRIVETYPE.NOT_MATCHES}` | ^\s$ | Tipos excluídos |
| `{$VFS.FS.FSNAME.MATCHES}` | .* | Filesystems permitidos |
| `{$VFS.FS.FSNAME.NOT_MATCHES}` | ^(?:/dev\|/sys\|/run\|/proc\|.+/shm$) | Filesystems excluídos |
| `{$VFS.FS.FSTYPE.MATCHES}` | .* | Tipos de FS permitidos |
| `{$VFS.FS.FSTYPE.NOT_MATCHES}` | ^\s$ | Tipos excluídos |

---

## 📈 Dashboard Incluído

Dashboard: **Filesystems**

Contém:

- Gráficos dinâmicos por volume descoberto
- Visualização tipo Pie (usado vs total)
- Atualização automática conforme novos discos são adicionados

---

## 🏷 Versão

v1.0 – Monitoramento automático de discos com LLD, controle hierárquico de severidade e arquitetura modular NOC.