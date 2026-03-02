# Template - Linux Discovery Disk by NOC

> NOC Monitoring Framework  
> Camada: Monitoramento de Storage (Linux Stack)

---

## 📌 Visão Geral

Template modular responsável pelo monitoramento automático de filesystems em servidores Linux utilizando Low-Level Discovery (LLD).

Desenvolvido para compor a stack Linux corporativa, permitindo análise precisa de utilização de disco, crescimento de volumes e prevenção de indisponibilidade por esgotamento de espaço.

Arquitetado para ambientes de média e grande escala com múltiplos volumes e crescimento dinâmico.

---

## 🏗 Arquitetura / Escopo

**Camada 3 – Monitoramento de Storage**

Este template é responsável exclusivamente pelo monitoramento de filesystems dentro da stack Linux.

Sua separação do Linux Base permite:

1. Modularização do monitoramento de storage  
2. Escalabilidade via LLD  
3. Aplicação seletiva conforme criticidade  
4. Evolução independente de thresholds  

Atua como módulo especializado na análise de capacidade e integridade de volumes.

---

## 🎯 Objetivos

- Descobrir automaticamente filesystems montados  
- Monitorar utilização percentual por volume  
- Detectar risco de esgotamento de espaço  
- Monitorar disponibilidade de inodes  
- Reduzir incidentes por disco cheio  
- Apoiar capacity planning de storage  

---

## 📊 Métricas Monitoradas

### 🔹 Filesystem Discovery (LLD)

- Descoberta automática via `vfs.fs.discovery`  
- Aplicação de filtros por nome de filesystem  
- Aplicação de filtros por tipo de filesystem  

A regra de descoberta executa periodicamente (default: 1 hora).

---

### 🔹 Utilização de Disco (por volume descoberto)

- Espaço total  
- Espaço utilizado  
- Percentual utilizado  

---

### 🔹 Inodes

- Percentual de inodes livres  

---

Essas métricas permitem identificar:

- Crescimento acelerado de volume  
- Saturação progressiva  
- Risco de indisponibilidade  
- Problemas estruturais em partições  

---

## 🚨 Estratégia de Alertas

| Severidade | Condição | Observação |
|------------|----------|------------|
| High | Utilização ≥ `{$VFS.FS.PUSED.MAX.HIGH}` | Escalonamento inicial |
| Disaster | Utilização ≥ `{$VFS.FS.PUSED.MAX.DIST}` | Esgotamento iminente |

### Lógica Aplicada

- Validação de espaço total e utilizado antes do disparo  
- Uso de recovery expression  
- Dependência entre níveis para evitar alerta duplicado  
- Fechamento manual habilitado  
- Avaliação individual por mountpoint  

A estratégia garante escalonamento progressivo e redução de falso positivo.

---

## ⚙️ Macros Utilizadas

| Macro | Descrição | Default Sugerido |
|-------|-----------|------------------|
| `{$VFS.FS.PUSED.MAX.HIGH}` | Limite de alerta High (%) | 85 |
| `{$VFS.FS.PUSED.MAX.DIST}` | Limite crítico Disaster (%) | 95 |
| `{$VFS.FS.FSNAME.MATCHES}` | Regex para inclusão de filesystems | .* |
| `{$VFS.FS.FSNAME.NOT_MATCHES}` | Regex para exclusão (pseudo-fs) | predefinido |

Permite parametrização por host e personalização conforme política de storage.

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
- Ambiente Linux com suporte a `vfs.fs.discovery`  

Recomendado uso em modo ativo para ambientes de grande escala.

---

## 🧠 Estratégia Técnica

Este template foi estruturado para garantir alta escalabilidade e controle granular.

Aplica:

- Low-Level Discovery (LLD)  
- Filtros por tipo de filesystem (ext4, xfs, zfs, etc.)  
- Exclusão de pseudo-filesystems (`/proc`, `/sys`, `/dev`, etc.)  
- Override para ignorar coleta de inode em filesystems dinâmicos (btrfs, zfs)  
- Separação completa do módulo Linux Base  

Permite evolução independente de thresholds sem impactar outros módulos da stack.

---

## 📈 Benefícios Operacionais

- Descoberta automática de novos volumes  
- Monitoramento granular por mountpoint  
- Redução significativa de incidentes por disco cheio  
- Melhor previsibilidade de crescimento  
- Padronização enterprise de monitoramento de storage  
- Alta escalabilidade para ambientes com múltiplos volumes  
- Arquitetura modular e desacoplada  

---

## 🏷 Versão

v1.0 – Estrutura modular inicial do stack Linux com LLD para monitoramento de storage