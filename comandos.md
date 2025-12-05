# Kubernetes — Comandos (Day 1)

## Criar cluster multi-node com kind
```bash
kind create cluster --config kind-config.yaml
```

## ⚙️ Configuração do PowerShell Completion
```powershell
# Verificar opções de completion
kubectl completion --help

# Configurar completion para kubectl (PowerShell)
kubectl completion powershell | Out-String | Invoke-Expression

# Para adicionar permanentemente ao perfil do PowerShell
kubectl completion powershell >> $PROFILE

# Recarregar o perfil
. $PROFILE
```

## ⚙️ Configuração do Bash Completion (Linux/WSL)
```bash
# Instalar bash-completion (Debian/Ubuntu)
sudo apt-get update
sudo apt-get install -y bash-completion

# Configurar completion para kubectl
mkdir -p "$HOME/.kube"
kubectl completion bash > "$HOME/.kube/completion.bash.inc"
echo "source '$HOME/.kube/completion.bash.inc'" >> "$HOME/.bashrc"

# Aplicar as mudanças
source "$HOME/.bashrc"
```

## 📝 Aliases Úteis

### PowerShell
```powershell
# Adicionar ao perfil do PowerShell
Set-Alias -Name k -Value kubectl

# Criar funções para comandos complexos
function kgp { kubectl get pods $args }
function kgs { kubectl get services $args }
function kgn { kubectl get nodes $args }

# Para adicionar permanentemente, editar:
# notepad $PROFILE
# Depois de adicionar, executar:
# . $PROFILE
```

### Bash/Linux/WSL
```bash
# Adicionar ao ~/.bashrc ou ~/.bash_profile
alias k='kubectl'
alias kgp='kubectl get pods'
alias kgs='kubectl get services'
alias kgn='kubectl get nodes'
# Depois de adicionar, executar:
# source ~/.bashrc
```

## Busybox (debug pod)
```bash
kubectl run -ti debug --image=busybox --restart=Never -- /bin/sh
```