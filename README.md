# Ansible + Docker na Linode

Projeto de automação com Ansible para instalação do Docker e gerenciamento de containers web personalizados (com Nginx) em VMs Ubuntu 22.04 LTS na Linode.

## ✅ Requisitos

- 2 VMs na Linode:
  - `ansible-control` (com Ansible instalado)
  - `docker-target` (controlada via SSH)
- Ubuntu 22.04 LTS
- Acesso root ou sudo

## ⚙️ Arquivos

- `hosts.ini` — inventário com IP da VM alvo
- `playbooks/` — playbooks organizados por tarefa

## 🚀 Como usar

```bash
# Instalar Docker na VM alvo
ansible-playbook -i hosts.ini playbooks/instalar_docker.yml

# Criar 8 containers web
ansible-playbook -i hosts.ini playbooks/containers_web.yml

# Listar containers
ansible-playbook -i hosts.ini playbooks/listar_containers.yml

# Ver estatísticas
ansible-playbook -i hosts.ini playbooks/stats_containers.yml

# Ver logs do web1
ansible-playbook -i hosts.ini playbooks/logs_container.yml

# Pausar o web1
ansible-playbook -i hosts.ini playbooks/pausar_container.yml

# Parar web2, web3 e web4
ansible-playbook -i hosts.ini playbooks/parar_containers.yml

# Remover todos os containers
ansible-playbook -i hosts.ini playbooks/remover_containers.yml
```
