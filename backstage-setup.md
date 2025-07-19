# Requisitos para Instalar o Backstage Localmente

Este guia fornece instruções detalhadas para configurar o ambiente de desenvolvimento e instalar uma instância local do [Backstage](https://backstage.io).

> ⚡ Este tutorial assume familiaridade com sistemas Unix e ferramentas de terminal como `apt`, `curl`, `npm`, `yarn`, `git` e `docker`.

---

## Requisitos de Sistema

### Sistema Operacional

* Linux (Ubuntu 20.04 ou superior)
* macOS (Intel ou Apple Silicon)
* Windows 10+ com WSL2 (Ubuntu recomendado)
* Windows 10+ (sem WSL)

### Permissões

* Conta com permissão de administrador para instalação de dependências

### Ferramentas de Build

#### Ubuntu/Debian:

```bash
sudo apt update && sudo apt install -y build-essential
```

#### macOS:

```bash
xcode-select --install
```

#### Windows (sem WSL):

Instale o [Git for Windows](https://gitforwindows.org/), [Node.js LTS](https://nodejs.org/), e [Docker Desktop para Windows](https://www.docker.com/products/docker-desktop/).

Use o terminal **Git Bash** ou **PowerShell com Node.js no PATH**.

> 💡 Visual Studio ou C++ Build Tools **não são necessários** para o desenvolvimento com Backstage, a menos que alguma dependência nativa específica requeira compilação no Windows. Em geral, basta garantir que o Node.js LTS e Yarn estejam corretamente configurados.

---

## Dependências Obrigatórias

### 1. Git

Ferramenta essencial para clonar e versionar código-fonte.

```bash
sudo apt install git       # Linux
brew install git           # macOS
```

Windows: use o instalador do [Git for Windows](https://gitforwindows.org/).

---

### 2. curl ou wget

Utilitários para download de scripts e arquivos.

```bash
sudo apt install curl      # ou sudo apt install wget
```

Windows: `curl` já vem incluso no PowerShell moderno.

---

### 3. Node.js (Active LTS)

O Backstage é baseado em Node.js. Use o `nvm` para gerenciar a instalação (Linux/macOS).

#### Linux/macOS:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
# ou
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

> Reinicie o terminal após instalar o NVM.

```bash
nvm install --lts
nvm use --lts
```

#### Windows:

Use o instalador oficial do [Node.js LTS](https://nodejs.org/en/download). Não há suporte completo ao `nvm` no Windows.

---

### 4. Yarn 

O Backstage exige o Yarn para gerenciamento de dependências.

```bash
npm install -g yarn
```

> Verifique a versão:

```bash
yarn --version  
```
---

### 5. Docker

Docker é utilizado por alguns recursos como [TechDocs](https://backstage.io/docs/features/techdocs/) e [Software Templates](https://backstage.io/docs/features/software-templates/).

#### Linux:

```bash
sudo apt install docker.io
sudo usermod -aG docker $USER
newgrp docker
```

#### macOS:

```bash
brew install --cask docker
```

#### Windows:

Instale o [Docker Desktop para Windows](https://www.docker.com/products/docker-desktop/) e certifique-se de ativar a integração com o WSL2 (se aplicável) ou garantir que o Docker Engine está em execução.

---

## Verificação Final

Execute os comandos abaixo para verificar se tudo está corretamente instalado:

```bash
node -v
npm -v
yarn -v
git --version
docker --version
```

## Instalação do Backstage

Após instalar as dependências obrigatórias (Node.js, Yarn, Git, etc.), siga os passos abaixo para instalar o Backstage localmente.

---

### 1. Criar o Projeto

```bash
npx @backstage/create-app@latest
```

#### Durante o processo:

* Dê um nome para sua aplicação (ex: `meu-backstage`).
* Aguarde a instalação dos pacotes e a configuração inicial.

Ao final, você terá uma estrutura como:

```
backstage/
├── packages/
│   ├── app/        # Frontend React
│   └── backend/    # Backend Express + Plugins
├── plugins/        # Plugins customizados
├── app-config.yaml # Configuração principal
```

---

### 2. Entrar na pasta do projeto

```bash
cd backstage
```

---

### 3. Rodar localmente em modo desenvolvimento

```bash
yarn start 
```


* Isso inicia o frontend em `http://localhost:3000`
* E o backend em `http://localhost:7007`

---

### 4. Verificar a aplicação

Acesse [http://localhost:3000](http://localhost:3000) no navegador para ver sua instância do Backstage rodando.


Pronto. O Backstage está instalado e pronto para ser customizado ou expandido com plugins, catálogo e integrações.

---

## 🚧 Solução de Problemas (Troubleshooting)

Abaixo estão alguns problemas comuns durante a instalação ou execução do Backstage localmente, especialmente em ambientes Windows:

---

### ⚠️ Erro ao compilar dependência nativa (ex: `node-gyp`, `fsevents`, `sharp`)

#### Sintomas:

* Mensagens como "failed to build" ou "gyp ERR!"

#### Solução:

**Windows (sem WSL):**

* Instale o Visual Studio Build Tools:

  * Acesse: [https://visualstudio.microsoft.com/visual-cpp-build-tools/](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
  * Selecione os workloads:

    * "Desenvolvimento para desktop com C++"
    * Certifique-se de incluir o pacote "Windows 10 SDK"

> ✅ Reinicie o terminal após a instalação.

**Alternativa:** Use WSL2 com Ubuntu para evitar esses problemas.

---

### 🚫 Problema com versão do Yarn (v3+)

#### Sintomas:

* Build falha
* Pacotes não resolvidos corretamente

#### Solução:

```bash
yarn set version classic
```

> Garante que está usando Yarn 1.22.x (necessário para Backstage).

---

### ⛔️ Docker não é reconhecido

#### Sintomas:

* Comando `docker` retorna "command not found" ou erro de conexão

#### Solução:

* Verifique se o Docker Desktop está em execução
* No Windows, abra o Docker Desktop manualmente antes de usar
* Teste com:

```bash
docker run hello-world
```

---

### 🤦‍♂️ Node.js instalado fora do nvm (Linux/macOS)

#### Solução:

* Remova versões instaladas globalmente
* Reinstale usando o NVM para garantir compatibilidade

---

### ❌ Erro de permissão no Docker (Linux)

#### Solução:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

> Depois, reinicie a sessão do terminal.

---

### 🚷 Porta já em uso (ex: 3000)

#### Solução:

* Identifique o processo usando:

```bash
lsof -i :3000   # Linux/macOS
netstat -ano | findstr :3000   # Windows
```

* Finalize o processo

