# Global Swine Management - Server Infrastructure

Este repositório contém a infraestrutura base do servidor para o projeto **Global Swine Management**. Ele é responsável por orquestrar os serviços essenciais de banco de dados e roteamento (API Gateway) utilizando Docker e Docker Compose.

## 🏗️ Arquitetura

A infraestrutura atual é composta por:

1. **PostgreSQL (Database)**:
   - Utiliza a imagem oficial `postgres:16-alpine`.
   - Armazena os dados persistentes de todo o sistema.
   - Os dados são mantidos seguros através de um volume Docker (`postgres_data`).

2. **Nginx (API Gateway / Reverse Proxy)**:
   - Utiliza a imagem oficial `nginx:alpine`.
   - Responsável por receber as requisições na porta `80` e roteá-las para os respectivos microserviços.
   - Configurado para suportar WebSockets e repasse correto de headers HTTP.

A comunicação entre os serviços da aplicação ocorre internamente de forma segura através da rede Docker (`gsm_network`).

## 🚀 Como executar

### Pré-requisitos
- [Docker](https://docs.docker.com/get-docker/) instalado
- [Docker Compose](https://docs.docker.com/compose/install/)

### Passo a passo

1. **Clone o repositório**:
   ```bash
   git clone https://github.com/rafaelcsilvadev/global_swine_management_server.git
   cd global_swine_management_server
   ```

2. **Configure as variáveis de ambiente**:
   Copie o arquivo `.env.example` para criar o seu arquivo `.env` na raiz do projeto e preencha com as suas próprias configurações:
   ```bash
   cp .env.example .env
   ```
   *(Evite utilizar senhas como `swine_pass` ou `123456` em ambientes de produção!)*

3. **Inicie os serviços**:
   Execute o seguinte comando para iniciar o banco de dados e o gateway em modo *detached* (segundo plano):
   ```bash
   docker-compose up -d
   ```

4. **Verifique os logs ou o status**:
   ```bash
   docker-compose ps
   docker-compose logs -f
   ```

## 📂 Estrutura do Projeto

- `docker-compose.yml`: Definição de todos os serviços base (PostgreSQL e Nginx), redes e volumes associados.
- `nginx/nginx.conf`: Configuração de proxy reverso do Nginx contendo as regras de roteamento (ex: rotas de `/farmer/` para o microserviço `gsm_farmer_api`).
- `.env`: Arquivo para definição das variáveis de ambiente de ambiente e banco de dados.

## 🔗 Microserviços e Roteamento

O API Gateway (Nginx) está configurado para rotear tráfego para os microserviços. Atualmente, possui as seguintes rotas mapeadas:

- **`/farmer/`** -> Encaminha requisições para o container da API (ex: `gsm_farmer_api:8080`).

> **Aviso:** Para que as rotas funcionem perfeitamente em desenvolvimento, a sua API deve estar em execução no mesmo ambiente Docker, ingressada na rede `gsm_network`.

## 🤖 Programação Orientada por I.A.

Este projeto foi desenvolvido utilizando o conceito de **Programação Orientada por I.A.**, com o auxílio do **Antigravity**.

**O que é?**
A Programação Orientada por Inteligência Artificial é um paradigma de desenvolvimento moderno onde a I.A. atua ativamente como uma parceira de *pair programming*. Em vez de apenas fornecer sugestões de código isoladas, a I.A. compreende o contexto completo do projeto, auxilia na tomada de decisões arquiteturais, cria testes, documenta e escreve o código de forma colaborativa com o desenvolvedor humano.

O **Antigravity** é um poderoso assistente de codificação autônomo e colaborativo, criado pela equipe do Google DeepMind focada em Advanced Agentic Coding. Ele possui a capacidade de analisar o repositório, planejar implementações complexas, interagir com o ambiente de desenvolvimento e editar os arquivos do projeto de forma autônoma, acelerando significativamente o ciclo de criação e manutenção de software.
