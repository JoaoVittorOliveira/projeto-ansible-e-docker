# 📦 Projeto: Gerenciamento de Containers com Ansible + Docker

Este projeto tem como objetivo ensinar como usar o **Ansible** para automatizar o provisionamento de **containers Docker** em uma máquina remota. Usamos servidores Linux Ubuntu 22.04 LTS como base e realizamos o processo em duas máquinas virtuais: uma controladora com o Ansible e outra como alvo Docker.

---

## 🖥️ Pré-requisitos

- 2 Máquinas Virtuais Ubuntu 22.04 LTS (em qualquer nuvem: Linode, AWS, DigitalOcean, etc.)
- Acesso SSH como `root` ou com permissão `sudo`
- Conexão com a internet nas VMs
- Sua máquina local (Windows/Linux/macOS) com:
  - Git
  - PowerShell ou terminal
  - Cliente SSH

---

## ⚙️ Estrutura de VMs

- **VM1** (Ansible Control): onde os playbooks serão executados.
- **VM2** (Target): onde o Docker e os containers serão configurados.

---

## 📁 Clonando este repositório

Na **VM de controle** (VM1), execute:

```bash
git clone https://github.com/JoaoVittorOliveira/projeto-ansible-e-docker.git
cd projeto-ansible-e-docker
```

Ou, se estiver com um `.zip`, use:

```bash
apt update && apt install unzip -y
unzip projeto-ansible-e-docker.zip -d projeto
cd projeto
```

---

## 🔐 Configuração de Acesso via SSH (Entre VM1 → VM2)

1. Gere uma chave SSH na VM1:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_ansible
```

2. Copie a chave pública para a VM2:

```bash
ssh-copy-id -i ~/.ssh/id_ansible.pub root@IP_DA_VM2
```

> Isso permite que o Ansible se conecte automaticamente na VM2 sem pedir senha.

3. Verifique a conexão:

```bash
ssh -i ~/.ssh/id_ansible root@IP_DA_VM2
```

---

## 🗂️ Estrutura do Projeto

```
ansible-docker-web/
├── hosts.ini
├── playbooks/
│   ├── instalar_docker.yml
│   ├── baixar_imagem.yml
│   ├── criar_containers.yml
│   ├── listar_containers.yml
│   ├── estatisticas.yml
│   ├── logs.yml
│   ├── pausar_container.yml
│   ├── parar_container.yml
│   └── excluir_todos.yml
└── README.md
```

---

## 🚀 Execução dos Playbooks

Todos os comandos abaixo devem ser executados na **VM1** (Ansible Control).

### ✅ 1. Instalar o Docker

```bash
ansible-playbook -i hosts.ini playbooks/1instalar_docker.yml
``

### ✅ 2. Baixar a imagem do Docker Hub

```bash
ansible-playbook -i hosts.ini playbooks/baixar_imagem.yml
```

### ✅ 3. Criar 8 containers web personalizados (Nginx)

```bash
ansible-playbook -i hosts.ini playbooks/criar_containers.yml
```

### ✅ 4. Listar todos os containers

```bash
ansible-playbook -i hosts.ini playbooks/listar_containers.yml
```

### ✅ 5. Ver estatísticas de uso dos containers

```bash
ansible-playbook -i hosts.ini playbooks/estatisticas.yml
```

### ✅ 6. Ver logs de um container em tempo real

```bash
ansible-playbook -i hosts.ini playbooks/logs.yml
```

### ✅ 7. Pausar 1 container

```bash
ansible-playbook -i hosts.ini playbooks/pausar_container.yml
```

### ✅ 8. Parar 3 containers

```bash
ansible-playbook -i hosts.ini playbooks/parar_container.yml
```

### ✅ 9. Remover todos os containers

```bash
ansible-playbook -i hosts.ini playbooks/excluir_todos.yml
```

---

## 📌 Observações

- Todos os containers são criados com páginas diferentes no index.html.
- As portas são redirecionadas automaticamente para acesso externo (808x).
- O projeto usa Ansible com `become: true`, então o usuário remoto deve ter permissão sudo.
