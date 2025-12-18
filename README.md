# Hexxa Web Stack 🚀

[![Docker Compose](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker)](https://docs.docker.com/compose/)
[![Apache2](https://img.shields.io/badge/Apache2-Debian12-D22128?style=for-the-badge&logo=apache)](https://httpd.apache.org/)
[![Redis](https://img.shields.io/badge/Redis-7-Bookworm-DD0031?style=for-the-badge&logo=redis)](https://redis.io/)
[![Multi-stage](https://img.shields.io/badge/Build-Multi--stage-0DB7ED?style=for-the-badge&logo=docker)](https://docs.docker.com/build/building/multi-stage/)
[![Debian 12](https://img.shields.io/badge/Debian-12-Bookworm-999?style=for-the-badge&logo=debian)](https://www.debian.org/)

Stack Docker **completa e otimizada** com Apache2 multistage (Debian 12) + Redis persistente para desenvolvimento, testes e produção [file:60][file:62].

## 🏗️ Arquitetura da Solução

- **Host**
  - `localhost:8081`
  - `localhost:6379`

- **Serviço Web**
  - Apache2  
  - Multi-stage  
  - Debian 12

- **Volumes**
  - `./html` → Volume RW persistente conectado ao Apache2
  - `redis-data/` → Volume nomeado persistente para o Redis

- **Cache / Banco em memória**
  - Redis 7  
  - Base: Debian Bookworm

- **Conexões**
  - Host `localhost:8081` → Apache2
  - Apache2 → Redis 7
  - Redis 7 → Volume `redis-data/`
  - Redis 7 → Host `localhost:6379`


### Serviços Disponíveis

| Serviço | Imagem Base | Porta | Persistência | Status |
|---------|-------------|-------|--------------|--------|
| **Web** | `httpd:2.4-bookworm` | `8081:80` | `./html` (RW) | ✅ Multistage |
| **Redis** | `redis:7-bookworm` | `6379:6379` | `redis-data` | ✅ RDB + AOF |

## 🚀 Instruções Rápidas (2 minutos)

### 1. Pré-requisitos

docker compose version # Deve ser >= 2.20
mkdir -p html redis-data

### 2. Deploy Principal (Recomendado)

docker compose -f hexxatest_compose.yml up -d

### 3. Teste Rápido

curl http://localhost:8081

## 📁 Estrutura do Projeto

.
├── README.md # Esta documentação  
├── Dockerfile # Apache2 multistage  ​
├── hexxatest_compose.yml # Stack principal Apache+Redis​  
├── hexxa_teste_compose.yml # Compose alternativo​  
├── html/  
│ └── index.html # Página de teste​  
└── redis-data/ # Volume persistente Redis  

## 🛠️ Comandos Essenciais

### Deploy 

Subir stack principal
docker compose -f hexxatest_compose.yml up -d

## ⚙️ Configurações Técnicas

### Apache2 Multistage [file:62]

Stage 1 → node:22-bookworm (build)  
└── npm ci + npm run build  

Stage 2 → debian:12 + apache2 (runtime)  
├── USER www-data (non-root)  
├── VOLUME /var/www/html/  
└── EXPOSE 80  

### Redis Persistente [file:60]
redis-server --save 60 1 --appendonly yes  
├── RDB snapshot (60s)  
├── AOF log contínuo  
└── redis-data:/data  

## 🐳 Build da Imagem Custom

Build Apache custom
docker build -t hexxa_test_docker:1.0.0 .

Usar no compose alternativo
docker compose -f hexxa_teste_compose.yml up -d web


## 🔒 Melhores Práticas Implementadas

- ✅ **Non-root user** (`www-data`)
- ✅ **Persistência de dados** (volumes nomeados)
- ✅ **Restart policy** (`unless-stopped`)
- ✅ **Imagens oficiais** Debian 12
- ✅ **Multistage build** otimizado

## 📄 Licença

[MIT License](LICENSE) - © 2025 Kleyton de Andrade <kanm@outlook.com.br>

---

<div align="center">

**Hexxa Web Stack v1.0.0**  
*Apache2 Multistage + Redis Persistente*  
![Debian](https://img.shields.io/badge/Debian-12-FCC624?style=for-the-badge&logo=debian&logoColor=black)






