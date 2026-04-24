# 📦 Projeto: Gerenciamento de Containers com Ansible + Docker

## 🎯 O que é este projeto?

Este é um **projeto educacional** que demonstra como usar o **Ansible** para automatizar o provisionamento e gerenciamento de **containers Docker** em servidores remotos. O projeto cria múltiplos containers Nginx em uma máquina remota, permitindo aprender na prática conceitos de:

- ✅ Automação com Ansible
- ✅ Orquestração de containers Docker
- ✅ Gerenciamento remoto via SSH
- ✅ Infrastructure as Code (IaC)

---

## 💡 Para que serve?

Este projeto é **perfeito para**:

- 🎓 **Iniciantes em Ansible**: Aprender os fundamentos de playbooks e hosts
- 🐳 **Interessados em Docker**: Entender como provisionar containers em escala
- 🔧 **DevOps em formação**: Praticar automação de infraestrutura
- 🏢 **Empresas**: Base sólida para automação de ambientes em produção

---

## ⭐ Por que usar Ansible?

Ansible é **incrível** porque:

| Benefício | Descrição |
|-----------|-----------|
| 🚀 **Sem agentes** | Não precisa instalar nada no servidor remoto, só SSH |
| 📝 **YAML simples** | Linguagem legível e fácil de aprender |
| ⚡ **Idempotente** | Execute múltiplas vezes sem efeitos colaterais |
| 🔄 **Reutilizável** | Playbooks funcionam em múltiplos servidores |
| 📚 **Grande comunidade** | Muitos módulos e recursos disponíveis |
| 🎯 **Orquestração** | Coordena tarefas complexas automaticamente |

**Vantagem prática**: Em vez de fazer SSH em 10 servidores e executar comandos manualmente, você escreve um playbook e executa uma vez para todos!

---

## 🗂️ Estrutura do Projeto

```
projeto-ansible-e-docker/
├── hosts.ini                      # Configuração dos servidores alvo
├── README.md                      # Este arquivo
└── playbooks/                     # Automações
    ├── instalar_docker.yml        # Instala Docker no servidor
    ├── containers_web.yml         # Cria 8 containers Nginx
    ├── listar_containers.yml      # Lista containers ativos
    ├── stats_containers.yml       # Mostra estatísticas dos containers
    ├── logs_container.yml         # Exibe logs dos containers
    ├── pausar_container.yml       # Pausa containers
    ├── parar_containers.yml       # Para containers específicos
    └── remover_containers.yml     # Remove containers
```

---

## 📖 Explicação dos Playbooks

### 1️⃣ **instalar_docker.yml**
Instala o Docker Engine no servidor remoto.
```bash
ansible-playbook -i hosts.ini playbooks/instalar_docker.yml
```
**O que faz:**
- Atualiza pacotes do sistema
- Instala dependências (curl, gnupg, etc.)
- Adiciona repositório oficial do Docker
- Instala Docker CE, CLI e containerd
- Ativa e inicia o serviço Docker

---

### 2️⃣ **containers_web.yml**
Cria 8 containers Nginx com páginas HTML personalizadas.
```bash
ansible-playbook -i hosts.ini playbooks/containers_web.yml
```
**O que faz:**
- Baixa a imagem Nginx do Docker Hub
- Cria 8 containers com nomes `web1` até `web8`
- Expõe em portas 8081 até 8088
- Cada container tem uma página HTML personalizada

---

### 3️⃣ **listar_containers.yml**
Lista todos os containers em execução.
```bash
ansible-playbook -i hosts.ini playbooks/listar_containers.yml
```

---

### 4️⃣ **stats_containers.yml**
Exibe estatísticas de CPU, memória e I/O dos containers.
```bash
ansible-playbook -i hosts.ini playbooks/stats_containers.yml
```

---

### 5️⃣ **logs_container.yml**
Mostra logs de um container específico.
```bash
ansible-playbook -i hosts.ini playbooks/logs_container.yml
```

---

### 6️⃣ **pausar_container.yml**
Pausa um ou mais containers.
```bash
ansible-playbook -i hosts.ini playbooks/pausar_container.yml
```

---

### 7️⃣ **parar_containers.yml**
Para containers específicos (padrão: web2, web3, web4).
```bash
ansible-playbook -i hosts.ini playbooks/parar_containers.yml
```

---

### 8️⃣ **remover_containers.yml**
Remove todos os containers.
```bash
ansible-playbook -i hosts.ini playbooks/remover_containers.yml
```

---

## ⚙️ Configuração Inicial

### 📋 Pré-requisitos

- **2 Máquinas Virtuais** Ubuntu 22.04 LTS (AWS, Linode, DigitalOcean, etc.)
- **VM1** (Ansible Control): Controladora onde você executará os playbooks
- **VM2** (Docker Target): Servidor alvo que receberá os containers
- Acesso **SSH** com `root` ou permissão `sudo`
- Conexão com internet em ambas as VMs
- **Git** e **cliente SSH** instalados localmente

---

## 🚀 Como Replicar o Projeto

### Passo 1: Preparar as Máquinas Virtuais

1. Crie 2 VMs Ubuntu 22.04 LTS:
   - **VM1**: 2GB RAM, 20GB disco (Controladora)
   - **VM2**: 2GB RAM, 20GB disco (Docker Host)

2. Anote os IPs privados ou públicos de ambas

### Passo 2: Configurar SSH

Na **VM1**:

```bash
# Criar chave SSH
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_ansible -N ""

# Copiar para VM2
ssh-copy-id -i ~/.ssh/id_ansible.pub root@IP_DA_VM2

# Testar conexão
ssh -i ~/.ssh/id_ansible root@IP_DA_VM2 "echo 'SSH conectado com sucesso!'"
```

### Passo 3: Instalar Ansible na VM1

```bash
# Atualizar repositórios
sudo apt update

# Instalar Ansible
sudo apt install ansible -y

# Verificar instalação
ansible --version
```

### Passo 4: Clonar o Repositório

Na **VM1**:

```bash
# Clonar repositório
git clone https://github.com/JoaoVittorOliveira/projeto-ansible-e-docker.git
cd projeto-ansible-e-docker

# Ou, se tiver um arquivo ZIP
unzip projeto-ansible-e-docker.zip
cd projeto-ansible-e-docker
```

### Passo 5: Configurar o arquivo `hosts.ini`

Edite `hosts.ini` com o IP da sua VM2:

```ini
[docker_host]
IP_DA_VM2 ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_ansible
```

**Teste a conectividade:**

```bash
ansible -i hosts.ini docker_host -m ping
```

Você deve ver:
```
IP_DA_VM2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

### Passo 6: Executar o Playbook de Instalação do Docker

```bash
ansible-playbook -i hosts.ini playbooks/instalar_docker.yml
```

### Passo 7: Criar os Containers Web

```bash
ansible-playbook -i hosts.ini playbooks/containers_web.yml
```

### Passo 8: Verificar os Containers

```bash
ansible-playbook -i hosts.ini playbooks/listar_containers.yml
```

Você deve ver 8 containers rodando:
```
web1, web2, web3, web4, web5, web6, web7, web8
```

### Passo 9: Testar os Containers

Acesse em um navegador (substituindo IP_DA_VM2 pelo IP real):

```
http://IP_DA_VM2:8081
http://IP_DA_VM2:8082
http://IP_DA_VM2:8083
... até a porta 8088
```

Você verá a mensagem: "Bem-vindo ao container X"

---

## 📊 Outros Comandos Úteis para Gerenciar Containers

Listar containers:
```bash
ansible-playbook -i hosts.ini playbooks/listar_containers.yml
```

Ver estatísticas:
```bash
ansible-playbook -i hosts.ini playbooks/stats_containers.yml
```

Ver logs:
```bash
ansible-playbook -i hosts.ini playbooks/logs_container.yml
```

Parar containers específicos:
```bash
ansible-playbook -i hosts.ini playbooks/parar_containers.yml
```

Pausar containers:
```bash
ansible-playbook -i hosts.ini playbooks/pausar_container.yml
```

Remover todos os containers:
```bash
ansible-playbook -i hosts.ini playbooks/remover_containers.yml
```

---

## 📌 Observações

- Todos os containers são criados com páginas diferentes no index.html.
- As portas são redirecionadas automaticamente para acesso externo (808x).
- O projeto usa Ansible com `become: true`, então o usuário remoto deve ter permissão sudo.

---

## 🔍 Problemas comuns

### ❌ "Falha ao conectar via SSH"
- Verifique se a chave pública foi copiada: `cat ~/.ssh/id_ansible.pub`
- Teste SSH manualmente: `ssh -i ~/.ssh/id_ansible root@IP_DA_VM2`

### ❌ "Ansible: command not found"
- Instale Ansible: `sudo apt install ansible -y`

### ❌ "Erro: Unable to connect to module"
- Verifique `hosts.ini` com IP correto
- Execute: `ansible -i hosts.ini docker_host -m ping`

### ❌ "Containers não estão sendo criados"
- Verifique se Docker foi instalado: `ansible -i hosts.ini docker_host -m shell -a "docker --version"`
- Verifique logs: `ansible-playbook -i hosts.ini playbooks/logs_container.yml`
- Verifique se o serviço do docker está rodando

---

## 📚 Recursos Adicionais

- [Documentação Ansible](https://docs.ansible.com/)
- [Documentação Docker](https://docs.docker.com/)
- [Docker Hub - Nginx](https://hub.docker.com/_/nginx)

---

## 👨‍💻 Autor

Criado como projeto educacional para ensinar Ansible + Docker.

---

## 📝 Licença

Este projeto é de código aberto e pode ser usado livremente para fins educacionais e comerciais.
