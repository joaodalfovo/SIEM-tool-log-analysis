## 🧪 Home Lab – Detecção de Falhas de Autenticação com Wazuh
Simulação de Tentativas de Elevação de Privilégio e Análise de Logs de Autenticação em Linux

📄 Descrição do Projeto

Este repositório documenta um home lab de análise de eventos de segurança utilizando o **Wazuh SIEM**, com foco na detecção e interpretação de falhas de autenticação em um sistema Linux.

O laboratório simula tentativas de **elevação de privilégio utilizando o comando `su`**, gerando eventos reais de autenticação falhada no sistema. Esses eventos são posteriormente coletados e analisados pelo Wazuh.

O objetivo é compreender como logs do sistema operacional são transformados em eventos de segurança dentro de um **SIEM**, além de desenvolver habilidades de investigação e análise utilizadas em ambientes **SOC (Security Operations Center)**.

---

## 🎯 Objetivo do Laboratório

Identificar eventos de autenticação falhada em um sistema Linux

Simular tentativas de elevação de privilégio

Gerar eventos de segurança reais no sistema

Analisar logs de autenticação do sistema

Observar como o Wazuh detecta atividades suspeitas

Desenvolver raciocínio analítico aplicado a SOC / Blue Team

---

## 🛠️ Ambiente Utilizado

Sistema Operacional: Linux

SIEM: Wazuh (All-in-One VM)

Fonte de Logs: System Authentication Logs

Método de coleta: Wazuh Agent

Interface de análise: Wazuh Dashboard (Discover)

Tecnologias envolvidas:

- Linux Authentication Logs
- PAM (Pluggable Authentication Modules)
- Systemd Journal
- Wazuh Rules Engine
- MITRE ATT&CK Mapping

---

## 🚨 Simulação do Evento de Segurança

Foi simulada uma tentativa de **elevação de privilégio** utilizando o comando:

```bash
su root
```

Durante a simulação, a senha foi inserida incorretamente propositalmente várias vezes para gerar eventos de autenticação falhada.

Exemplo no terminal:

su root
Password:
su: Authentication failure

Esse comportamento gera eventos reais de falha de autenticação no sistema Linux.

<img width="323" height="97" alt="image" src="https://github.com/user-attachments/assets/1f646723-1299-4a91-b22f-4800c588f3eb" />

## 🔍 Análise dos Logs no Sistema

Após gerar as tentativas de autenticação falhadas, os logs foram analisados diretamente no sistema utilizando o comando:

sudo journalctl | grep -i fail

<img width="510" height="26" alt="image" src="https://github.com/user-attachments/assets/4ad721aa-4c7a-45b7-aa6e-8ef2077f0789" />

Exemplo de logs identificados:

FAILED SU to root wazuh-user on tty1
unix_chkpwd: password check failed for user (root)
pam_unix(su:auth): authentication failure

<img width="806" height="603" alt="image" src="https://github.com/user-attachments/assets/49e591aa-508e-44d4-b801-ee8eb0b662fc" />

Esses eventos indicam que um usuário tentou autenticar como root, mas falhou durante o processo de verificação da senha.

## 🔬 Investigação Técnica
### 🔹 1. Origem do Log

Os eventos foram gerados por componentes do sistema de autenticação do Linux.

Componente	Função
su	troca de usuário no sistema
pam_unix	módulo de autenticação do PAM
unix_chkpwd	verificação de senha no sistema

Fluxo de autenticação:

Usuário executa su
↓
PAM inicia o processo de autenticação
↓
unix_chkpwd verifica a senha do usuário
↓
Senha incorreta → evento de log gerado

### 🔹 2. Processamento pelo Wazuh

Os logs do sistema foram coletados automaticamente pelo Wazuh Agent.

O Wazuh analisou os eventos e aplicou regras de detecção, enriquecendo o evento com informações adicionais.

Campos relevantes observados:

rule.description: User missed the password to change UID
rule.level: 5
rule.mitre.technique: Password Guessing
data.srcuser: wazuh-user
data.dstuser: root

Esses campos indicam:

Usuário que executou a ação

Usuário alvo

Tipo de evento

Severidade do alerta

Técnica de ataque associada

Observação: Como o evento foi classificado com rule level 5, ele foi categorizado pelo Wazuh como um alerta de baixa severidade (Low severity). Ainda assim, eventos desse tipo podem indicar tentativas iniciais de autenticação inválida ou atividades de password guessing, sendo relevantes para análise em contexto de segurança.

## ⚖️ Interpretação do Evento

O evento detectado corresponde a uma tentativa de autenticação falhada durante um processo de elevação de privilégio.

Possíveis cenários associados incluem:

Password Guessing

Privilege Escalation Attempt

Authentication Failure

No contexto deste laboratório, trata-se de uma simulação controlada para fins educacionais.

## 📊 Detecção no Wazuh

Os eventos foram visualizados no Wazuh Dashboard, utilizando a interface Discover para investigar logs coletados.

Eventos identificados incluíram:

FAILED SU attempts
Password check failed
Authentication failure
Privilege escalation attempt

O Wazuh correlacionou os logs do sistema e classificou os eventos utilizando regras internas e mapeamento MITRE ATT&CK.

## 🧾 Conclusão

Este laboratório demonstra como eventos simples de autenticação falhada podem ser detectados e analisados por um SIEM.

A simulação evidencia o fluxo completo de monitoramento de segurança:

Evento no sistema Linux
↓
Registro no log do sistema
↓
Coleta pelo Wazuh Agent
↓
Processamento pelo Wazuh Manager
↓
Detecção e visualização no Dashboard

Esse processo é fundamental para a detecção de atividades suspeitas em ambientes corporativos monitorados por equipes de Security Operations Center (SOC).

## 📚 Aprendizados Técnicos

Interpretação de logs de autenticação Linux

Funcionamento do PAM (Pluggable Authentication Modules)

Análise de eventos de falha de autenticação

Compreensão do fluxo de coleta de logs em SIEM

Investigação de eventos de segurança no Wazuh

Correlação entre logs do sistema e regras de detecção

Mapeamento de eventos utilizando MITRE ATT&CK
