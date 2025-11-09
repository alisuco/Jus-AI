<h1 align="center">
  <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Smilies/Robot.png" alt="Robot" width="30" height="30"/>  JusIA - Chatbot Jurídico com IA
</h1>

<div align="center">
  <img src="./assets/chatbot-logo.jpg" alt="JusIA Logo" width="300" height="auto">
</div>

## Sobre o Projeto

O **JusIA** é um assistente jurídico inteligente desenvolvido como chatbot para consulta de documentos jurídicos. O projeto utiliza tecnologias avançadas de Inteligência Artificial da AWS para implementar uma solução RAG (Retrieval Augmented Generation), permitindo que usuários façam consultas em linguagem natural sobre conteúdo jurídico e recebam respostas fundamentadas e contextualizadas.

Este projeto foi desenvolvido pelo Squad 2 como parte da avaliação das sprints 7 e 8 do programa de bolsas Compass UOL para formação em Inteligência Artificial para AWS.

## Acesso ao Bot

<div align="center">
  
  Experimente o **JusIA** diretamente no Telegram:
  <br>

  <a href="https://t.me/jus_ia_bot">
    <img src="https://img.shields.io/badge/Telegram-1E1E1E?style=for-the-badge&logo=telegram&logoColor=yellow" alt="Telegram Bot">
  </a>
  
  <br>
  
  **🔗 Link direto:** [`@jus_ia_bot`](https://t.me/jus_ia_bot)
  
  <br>
  
  > 📋 **Como usar:** Envie suas dúvidas jurídicas em linguagem natural e receba respostas fundamentadas em nossa base de conhecimento especializada em direito brasileiro.
  
</div>

## Funcionalidades

- 🤖 **Assistente Jurídico Inteligente**: Responde perguntas sobre direito brasileiro de forma clara e objetiva
- 📚 **Consulta a Base de Conhecimento**: Busca em documentos jurídicos armazenados em S3
- 🔍 **RAG (Retrieval Augmented Generation)**: Combina busca semântica com geração de texto usando IA
- 💬 **Interface via Telegram**: Acesso fácil e intuitivo através do Telegram
- 📊 **Monitoramento Completo**: Logs estruturados e métricas detalhadas no CloudWatch
- 🔧 **Middleware de Performance**: Rastreamento de tempo de execução e debugging
- 🚀 **Deploy Automatizado**: Containerização com Docker para deploy simplificado
- 🛡️ **Políticas IAM**: Controle de acesso seguro aos recursos AWS
- ⚡ **Otimização de Performance**: Cache de embeddings e processamento inteligente

## Tecnologias Utilizadas

### Cloud & Infrastructure

- **AWS EC2**: Hospedagem da aplicação com IP fixo
- **AWS API Gateway**: Gateway de API para roteamento seguro do webhook
- **AWS S3**: Armazenamento dos documentos jurídicos
- **AWS Bedrock**: Modelos de IA para embeddings e geração de texto
- **AWS CloudWatch**: Monitoramento e logging centralizado
- **AWS IAM**: Políticas de segurança e controle de acesso
- **Docker**: Containerização da aplicação

### Framework & Libraries

- **Python 3.12**: Linguagem principal
- **Flask**: Framework web para servidor HTTP
- **LangChain**: Framework para aplicações de IA
- **ChromaDB**: Banco de dados vetorial para embeddings
- **PyPDF**: Processamento de documentos PDF
- **Watchtower**: Integração com CloudWatch para logs estruturados
- **Boto3**: SDK da AWS para Python
- **Rich**: Formatação de logs e saídas coloridas

### Modelos de IA

- **Amazon Titan Text Premier v1**: Geração de respostas
- **Amazon Titan Embed Text v2**: Geração de embeddings

## Requisitos

### Credenciais AWS

- Conta AWS com acesso aos serviços: Bedrock, S3, EC2, CloudWatch, API Gateway
- Credentials configuradas (Access Key, Secret Key, Session Token)

### Telegram Bot

- Token do bot do Telegram (obtido via @BotFather)

### Sistema

- Python 3.12+
- Docker e Docker Compose
- Instância EC2

## Arquitetura da Solução

![Diagrama da Arquitetura](assets/sprints_7-8.jpg)

A solução segue uma arquitetura serverless e orientada a eventos:

1. **Telegram Webhook**: Recebe mensagens dos usuários via API Gateway
2. **API Gateway**: Roteia requisições para a instância EC2 com IP fixo
3. **Flask Server**: Processa requisições HTTP na EC2
4. **RAG Pipeline**:
   - Busca semântica no ChromaDB
   - Recuperação de documentos relevantes
   - Geração de resposta via Bedrock
5. **S3 Storage**: Armazenamento dos PDFs jurídicos
6. **CloudWatch**: Logging e monitoramento
7. **ChromaDB**: Índices vetoriais para busca semântica

## Como Utilizar

### 1. Configuração do Ambiente

```bash
# Clone o repositório
git clone <repository-url>
cd sprints-7-8-pb-aws-maio

# Crie o arquivo .env com as credenciais
cp .env.example .env
# Edite o .env com suas credenciais
```

### 2. Deploy Local (Desenvolvimento)

```bash
# Crie um ambiente virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
.\venv\Scripts\activate   # Windows

# Instale as dependências
pip install -r requirements.txt

# Execute a aplicação
python app.py
```

### 3. Deploy com Docker

```bash
# Build e execução
docker compose up -d

# Verificar logs
docker compose logs -f
```

### 4. Configuração do Webhook do Telegram

#### Para Produção (com API Gateway):

```bash
# Configure o webhook apontando para o API Gateway
curl -X POST "https://api.telegram.org/bot<BOT_TOKEN>/setWebhook?url=https://<API_GATEWAY_URL>/webhook"
```

#### Para Desenvolvimento (com localtunnel):

```bash
# Instale o localtunnel
npm install -g localtunnel

# Exponha a aplicação
lt --port 5000 --subdomain seu-subdominio

# Configure o webhook
curl -X POST "https://api.telegram.org/bot<BOT_TOKEN>/setWebhook?url=https://seu-subdominio.loca.lt/webhook"
```

### 5. Upload dos Documentos

- Faça upload dos documentos jurídicos para o bucket S3 especificado
- A aplicação processará automaticamente novos PDFs na primeira execução

## Estrutura do Projeto

```
├── app.py                          # Aplicação Flask principal
├── requirements.txt                # Dependências Python
├── Dockerfile                      # Configuração Docker
├── docker-compose.yaml            # Orquestração Docker
├── .env.example                    # Template de variáveis ambiente
├── ec2-iam-policy.json            # Políticas IAM para EC2
├── vetorizados.json               # Índice de PDFs processados
├── HowToUse.txt                   # Guia rápido de uso
├── README.md                      # Documentação
├── assets/                        # Recursos visuais
│   ├── chatbot-logo.jpg
│   └── sprints_7-8.jpg
├── chroma_index/                  # Banco vetorial ChromaDB
│   └── chroma.sqlite3
├── dataset/                       # Documentos jurídicos
│   └── juridicos.zip
└── services/                      # Módulos de serviço
    ├── __init__.py
    ├── bot_services.py            # Lógica do chatbot
    ├── middlewares/
    │   └── cloudwatch_middleware.py # Middleware de performance
    └── utils/
        └── cloudwatch_logger.py   # Utilitários de logging
```

## Deploy e Configuração

### Variáveis de Ambiente Necessárias

```env
# AWS Credentials
AWS_ACCESS_KEY_ID=sua_access_key
AWS_SECRET_ACCESS_KEY=sua_secret_key
AWS_SESSION_TOKEN=seu_session_token
AWS_REGION=sua_region

# Telegram
BOT_TOKEN=seu_bot_token

# AWS Resources
BUCKET_NAME=seu_bucket_s3

# CloudWatch Configuration
LOG_GROUP_NAME=seu_log_group_para_chatbot_server
```

### Configuração EC2

1. **Instância EC2**:

   - Tipo: t2.micro ou superior
   - Sistema: Amazon Linux 2 ou Ubuntu
   - IP Elástico: Configurar IP fixo para integração com API Gateway
   - Security Groups: Porta 5000 liberada para o API Gateway
   - Políticas IAM: Aplicar políticas definidas em `ec2-iam-policy.json`

2. **Configuração API Gateway**:

```bash
# Criar API Gateway REST API
# Configurar recurso /webhook com método POST
# Integração HTTP apontando para http://<EC2_ELASTIC_IP>:5000/webhook
# Deploy da API para obter a URL pública
```

3. **Instalação Docker**:

```bash
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user

# Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

4. **Deploy da Aplicação**:

```bash
git clone <repository-url>
cd sprints-7-8-pb-aws-maio
# Configure o .env
docker compose up -d
```

## Características Técnicas

### Performance

- **Busca Semântica**: Top-K similarity search (k=3) otimizada
- **Chunking**: Documentos divididos em chunks para melhor precisão
- **Caching**: ChromaDB persiste embeddings para reutilização
- **Concurrent Processing**: Suporte a múltiplas requisições simultâneas
- **Logs Estruturados**: Sistema de logging organizado por categorias
- **Error Handling**: Tratamento robusto com logs detalhados

### Segurança

- **Credenciais**: Gerenciamento via variáveis de ambiente
- **Políticas IAM**: Controle granular de permissões AWS
- **Logs**: Sem exposição de dados sensíveis nos logs
- **Rate Limiting**: Controle implícito via Telegram API
- **Error Handling**: Tratamento robusto de erros com logs estruturados
- **Região AWS**: Configurável via variável de ambiente

### Monitoramento

- **CloudWatch Logs**: Todos os eventos são logados com estrutura JSON
- **Métricas Customizadas**: Tempo de resposta, operações RAG, sucesso/erro
- **Logs Estruturados**: Separação por tipo (query, system, error, rag_operation)
- **Debugging Avançado**: Rastreamento detalhado de performance e execução
- **Health Checks**: Endpoint de status disponível
- **Middleware de Performance**: Monitoramento de tempo de execução de funções

### Escalabilidade

- **Docker**: Containerização para deploy consistente
- **API Gateway**: Entrada única com rate limiting e throttling

## Dificuldades

Durante o desenvolvimento, enfrentamos alguns desafios importantes:

### 1. **Integração Bedrock + LangChain**

- **Problema**: Compatibilidade entre versões das bibliotecas
- **Solução**: Definição de versões específicas no requirements.txt e configuração adequada dos modelos

### 2. **Webhook Telegram**

- **Problema**: Configuração SSL para webhook em desenvolvimento
- **Solução**: Uso do localtunnel para exposição segura durante desenvolvimento

### 3. **Logging e Monitoramento**

- **Problema**: Necessidade de logs estruturados para debugging eficiente
- **Solução**: Implementação de sistema de logging categorizado com CloudWatch e Watchtower

### 4. **Gestão de Credenciais AWS**

- **Problema**: Configuração segura de credenciais temporárias do AWS Academy
- **Solução**: Sistema robusto de variáveis de ambiente com valores padrão

## Autores

- [Alison da Costa Silva](https://github.com/alisuco)
- [Caio Henrique Lopes Sousa](https://github.com/cls2311)
- [Filipe da Silva Rodrigues](https://github.com/filipe-rds)
- [Stefhany Nunes Adiers](https://github.com/SNunesA)

---

**Desenvolvido por Squad 2 - Compass UOL Program 2025**
