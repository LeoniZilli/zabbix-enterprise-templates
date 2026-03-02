# Zabbix Enterprise Templates

> NOC Monitoring Framework  
> Arquitetura Modular Corporativa

Repositório contendo templates personalizados desenvolvidos para ambientes corporativos com grande volume de VMs monitoradas.

O projeto segue modelo modular, escalável e orientado a boas práticas de observabilidade.

---

## 🏗 Arquitetura do Framework

Os templates são organizados em camadas, permitindo separação clara de responsabilidades:

### 🔹 Camada de Conectividade
- ICMP Ping by NOC

### 🔹 Camada de Sistema Operacional (Linux Stack)

Linux Base by NOC  
├── Linux CPU by NOC  
├── Linux Memory by NOC  
├── Linux Discovery Disk by NOC  
└── Zabbix Agent - Triple Check by NOC  

Cada módulo possui responsabilidade isolada e pode evoluir de forma independente.

---

## 📦 Estrutura do Repositório

Cada template contém:

- Arquivo YAML exportado do Zabbix (compatível com 6.x)
- README técnico padronizado
- Documentação de arquitetura e dependências
- Estratégia de alertas aplicada
- Versionamento estruturado

---

## 🎯 Objetivos do Projeto

- Padronizar o monitoramento corporativo
- Reduzir falsos positivos e alertas em cascata
- Separar responsabilidades por camada
- Facilitar manutenção e troubleshooting
- Garantir escalabilidade para centenas de VMs
- Permitir reutilização modular em diferentes ambientes

---

## 🧠 Princípios Arquiteturais

O framework foi desenvolvido com base nos seguintes princípios:

1. Modularização real via Linked Templates
2. Dependência hierárquica de triggers
3. Separação entre conectividade, sistema e recursos
4. Redução de ruído operacional
5. Escalabilidade para ambientes distribuídos

---

## 🚀 Roadmap de Expansão

Estrutura preparada para expansão futura:

- Windows Base Stack
- Monitoramento Web
- Monitoramento de Banco de Dados
- Containers (Docker / Kubernetes)
- Monitoramento de aplicações específicas

---

## 📈 Benefícios Operacionais

- Organização clara da stack de monitoramento
- Evolução independente por módulo
- Padronização entre ambientes
- Melhor controle de versionamento
- Base sólida para observabilidade corporativa

---

## 🏷 Versão do Framework

v1.0 – Estrutura inicial modular corporativa