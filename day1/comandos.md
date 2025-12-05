# Kubernetes — Comandos (Day 1)

## Criar cluster multi-node com kind
```bash
kind create cluster --config kind-config.yaml
```

## Instalar Metrics Server

### 1. Baixar o manifesto
```bash
# Linux/WSL
wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

# PowerShell/Windows
Invoke-WebRequest -Uri https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml -OutFile components.yaml
```

### 2. Editar o arquivo components.yaml
Adicionar `--kubelet-insecure-tls` na seção de args do container:

```yaml
spec:
  containers:
  - args:
    - --cert-dir=/tmp
    - --secure-port=4443
    - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
    - --kubelet-use-node-status-port
    - --kubelet-insecure-tls      # <==== Adicionar esta linha
    - --metric-resolution=15s
    image: registry.k8s.io/metrics-server/metrics-server:v0.7.0
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 3
      httpGet:
        path: /livez
        port: https
        scheme: HTTPS
```

### 3. Aplicar o manifesto
```bash
kubectl apply -f components.yaml
```

### 4. Verificar instalação
```bash
kubectl get deployment metrics-server -n kube-system
kubectl top nodes
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