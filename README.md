# Zabbix Enterprise Templates

> NOC Monitoring Framework  
> Arquitetura Modular Corporativa

Repositório contendo templates personalizados desenvolvidos para ambientes corporativos com grande volume de VMs monitoradas.

O projeto segue modelo modular, escalável e orientado a boas práticas de observabilidade, com separação clara de responsabilidades e foco em redução de ruído operacional.

---

## 🏗 Arquitetura do Framework

Os templates são organizados em camadas, permitindo separação clara entre conectividade, sistema operacional e recursos.

---

### 🔹 Camada de Conectividade

- ICMP Ping by NOC

Responsável por validação de disponibilidade básica e base para dependência hierárquica de triggers.

---

## 🐧 Camada de Sistema Operacional (Linux Stack)

Linux Base by NOC  
├── Linux CPU by NOC  
├── Linux Memory by NOC  
├── Linux Discovery Disk by NOC  
└── Zabbix Agent - Triple Check by NOC  

Cada módulo possui responsabilidade isolada:

- **Base** → Informações estruturais e saúde do agente  
- **CPU** → Capacidade e saturação de processamento  
- **Memory** → Pressão e consumo de memória  
- **Disk** → Descoberta automática e capacidade de armazenamento  
- **Triple Check** → Validação adicional do agente  

---

## 🪟 Camada de Sistema Operacional (Windows Stack)

Windows Base by NOC  
├── Windows CPU by NOC  
├── Windows Memory by NOC  
├── Windows Discovery Disk by NOC  
├── Windows Network by NOC  
└── Zabbix Agent - Triple Check by NOC  

Stack Windows implementada com:

- Descoberta automática de discos
- Descoberta automática de interfaces físicas
- Monitoramento de CPU, memória e swap
- Monitoramento avançado de rede (tráfego, erros, link, downgrade)
- Estratégia de dependência hierárquica de severidade
- Coleta ativa via Zabbix Agent

Todos os módulos seguem o mesmo padrão arquitetural aplicado na stack Linux.

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
5. Controle de severidade contextual  
6. Prevenção de flapping via validação temporal  

---

## 🚀 Roadmap de Expansão

Estrutura preparada para expansão futura:

- Monitoramento Web
- Monitoramento de Banco de Dados
- Containers (Docker / Kubernetes)
- Monitoramento de aplicações específicas
- Integração com observabilidade híbrida

---

## 📈 Benefícios Operacionais

- Organização clara da stack de monitoramento
- Evolução independente por módulo
- Padronização entre ambientes Linux e Windows
- Melhor controle de versionamento
- Base sólida para observabilidade corporativa
- Estrutura pronta para ambientes distribuídos e multi-site

---

## 🏷 Versão do Framework

v1.1 – Expansão para stack Windows completa com monitoramento de rede e recursos avançados.