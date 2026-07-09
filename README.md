# Stack de Monitoramento e Observabilidade - Desafio Idus

Este repositório contém a solução para o desafio de estágio em DevOps da Idus. O objetivo do projeto é implantar uma stack de monitoramento conteinerizada para coletar, armazenar e visualizar métricas de um microsserviço simulado de forma automatizada e resiliente.

## Tecnologias Utilizadas

- **Docker & Docker Compose:** Orquestração dos containers e isolamento de ambiente.
- **Prometheus:** Coleta e armazenamento de métricas baseadas em séries temporais.
- **Grafana:** Criação de painéis visuais para análise de dados e observabilidade.
- **prom-http-simulator:** Microsserviço que simula tráfego HTTP real (taxa de requisições, latências e códigos de status).

---

## Arquitetura e Decisões Técnicas

### Coleta de Métricas: Scrape Direto vs Grafana Alloy

O edital sugere o uso do Grafana Alloy como agente de coleta. Contudo, para este escopo inicial, optei por utilizar a **configuração de scrape direto nativa do Prometheus**.

- **Justificativa:** O `prom-http-simulator` já expõe nativamente as métricas formatadas no padrão do Prometheus. Adicionar o Alloy introduziria uma camada extra de tradução de dados e consumo de recursos computacionais desnecessária para uma arquitetura centralizada deste tamanho. O scrape direto mantém a infraestrutura leve, simples de manter e cumpre o objetivo.

### Resiliência com Healthchecks e Inicialização Orquestrada

Sistemas industriais exigem alta disponibilidade. Por isso, foram implementados mecanismos de saúde nos serviços:

- **Healthchecks:** Cada container possui um script interno que testa periodicamente se a aplicação está respondendo (não apenas se o container está ligado). O Prometheus valida sua rota `/-/ready`, enquanto Grafana e Simulador validam a abertura de sockets em suas respectivas portas.
- **Subida Condicional (`depends_on`):** O Grafana depende do Prometheus, mas configurado com a condição `service_healthy`. Isso garante que o Grafana só inicialize após o Prometheus estar totalmente pronto e saudável para fornecer os dados, evitando falhas de provisionamento automático.

---

## Como Executar a Stack

### Pré-requisitos

- Docker instalado
- Docker Compose instalado

### Passo a Passo

1. Clone este repositório para sua máquina local:
   ```bash
   git clone https://github.com/HelberEduardo/Monitoramento.git
   cd ./Monitoramento
   ```
## Login
### Acesso de testes local
- **Usuário**: admin
- **Senha**: admin