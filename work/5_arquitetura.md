# Arquitetura do Sistema

Este documento apresenta a visão geral de alto nível da arquitetura do sistema betSTOP, detalhando a distribuição de funções entre os componentes principais e destacando elementos reutilizáveis da arquitetura.

## Visão Geral da Arquitetura

O sistema betSTOP foi projetado seguindo uma arquitetura distribuída moderna, composta por dois subsistemas principais que trabalham em conjunto para entregar uma experiência terapêutica eficaz aos usuários:

- **Aplicativo Mobile (Flutter)**: Interface do usuário executada em dispositivos iOS e Android
- **API Backend (Go)**: Serviços de negócio, processamento de dados e integração com sistemas externos

## Componentes Principais do Sistema

### 1. Aplicativo Mobile (Frontend)

**Tecnologia**: Flutter Framework com linguagem Dart
**Plataformas Suportadas**: Android (API 24+) e iOS (12.0+)

#### Justificativa Técnica
- **Performance Superior**: Flutter compila para código nativo, garantindo execução otimizada
- **Desenvolvimento Unificado**: Única codebase para ambas as plataformas reduz tempo e custos
- **Renderização Customizada**: Engine gráfico próprio permite interfaces complexas de jogos
- **Hot Reload**: Acelera desenvolvimento e testes durante criação dos módulos terapêuticos

#### Módulos do Aplicativo
- **Módulo de Autenticação**: Gestão de login, cadastro e recuperação de senha
- **Módulo de Jogos**: Simulações interativas (caça-níqueis, roleta, blackjack, crash)
- **Módulo de Monitoramento**: Tracking de tempo, comportamento e análise de padrões
- **Módulo Terapêutico**: Intervenções cognitivas, notificações e reenquadramento
- **Módulo de Progresso**: Sistema de recompensas, badges e visualização de recuperação
- **Módulo Educativo**: Conteúdo sobre TCC, exercícios e informações sobre vícios
- **Módulo de Comunidade**: Suporte, chat e compartilhamento com familiares

### 2. API Backend (Servidor)

**Tecnologia**: Go (Golang) com framework Gin para APIs REST
**Arquitetura**: Microserviços containerizados com Docker

#### Justificativa Técnica
- **Alta Concorrência**: Goroutines nativas suportam milhares de usuários simultâneos
- **Performance Excepcional**: Compilação para código máquina garante baixa latência
- **Baixo Consumo de Recursos**: Footprint de memória reduzido otimiza custos de infraestrutura
- **Confiabilidade**: Garbage collector eficiente e tratamento robusto de erros

#### Serviços da API
- **Serviço de Autenticação**: JWT, OAuth 2.0, validação de credenciais
- **Serviço de Usuários**: Gestão de perfis, preferências e dados pessoais
- **Serviço de Jogos**: Lógica de simulação, algoritmos pseudoaleatórios, resultados
- **Serviço de Analytics**: Processamento de métricas comportamentais e relatórios
- **Serviço de Notificações**: Push notifications e triggers terapêuticos
- **Serviço de Conteúdo**: Gestão de artigos, exercícios e material educativo
- **Serviço de Comunidade**: Chat, fóruns e sistema de mentoria

### 3. Camada de Persistência

#### Banco de Dados Principal (MySQL)
**Versão**: MySQL 8.0+ com replicação master-slave
**Configuração**: Cloud SQL no Google Cloud Platform

##### Justificativa Técnica
- **ACID Compliance**: Garante integridade dos dados terapêuticos críticos
- **Maturidade**: Ampla documentação e suporte para desenvolvimento
- **Performance**: Otimizações específicas para workloads de aplicações web
- **Backup Automático**: Recuperação point-in-time para proteção de dados

##### Estruturas de Dados Principais
- **Usuários e Perfis**: Dados demográficos, preferências, histórico de acesso
- **Sessões de Jogo**: Tempo, apostas virtuais, padrões comportamentais
- **Intervenções Terapêuticas**: Log de notificações, respostas, eficácia
- **Progresso de Recuperação**: Marcos, badges, metas pessoais
- **Conteúdo Educativo**: Artigos, exercícios, tracking de leitura
- **Comunidade**: Mensagens, avaliações, relacionamentos mentor-mentorado

#### Cache de Alta Performance (Redis)
**Versão**: Redis 7.0+ com clustering para escalabilidade
**Configuração**: Redis Cloud com replicação multi-zona

##### Justificativa Técnica
- **Velocidade Excepcional**: Operações in-memory com latência sub-milissegundo
- **Estruturas Avançadas**: Suporte nativo para listas, sets, hashes, streams
- **Escalabilidade Horizontal**: Clustering automático para grandes volumes
- **Persistência Configurável**: Snapshots e AOF para durabilidade de dados críticos

##### Casos de Uso do Cache
- **Sessões de Usuário**: Tokens JWT, estado de autenticação
- **Dados de Jogos**: Estado atual de partidas, histórico recente
- **Métricas em Tempo Real**: Contadores de tempo, estatísticas de uso
- **Cache de Consultas**: Resultados frequentes do MySQL
- **Rate Limiting**: Controle de tentativas de login e APIs

## Arquitetura de Deploy e Infraestrutura

### Google Cloud Platform (GCP)
**Justificativa**: Integração nativa com Flutter, escalabilidade global, SLA enterprise

#### Componentes de Infraestrutura
- **Google Kubernetes Engine (GKE)**: Orquestração de containers da API
- **Cloud SQL**: Banco MySQL gerenciado com backup automático
- **Redis Cloud**: Cache distribuído com alta disponibilidade
- **Cloud CDN**: Distribuição global de assets estáticos
- **Cloud Load Balancing**: Distribuição de tráfego com failover automático
- **Cloud Monitoring**: Observabilidade e alertas proativos

### Containerização e CI/CD
- **Docker**: Containerização de serviços para consistência entre ambientes
- **GitHub Actions**: Pipeline automatizado de build, teste e deploy
- **Terraform**: Infrastructure as Code para provisionamento reproduzível

## Componentes Reutilizáveis

### 1. Sistema de Autenticação Unificado
**Localização**: Compartilhado entre todos os módulos
**Funcionalidades**: JWT, OAuth 2.0, validação de sessões
**Benefício**: Segurança consistente e redução de duplicação de código

### 2. Engine de Notificações Terapêuticas
**Localização**: Utilizado por módulos de Jogos, Monitoramento e Progresso
**Funcionalidades**: Triggers contextuais, personalização, scheduling
**Benefício**: Intervenções consistentes baseadas em algoritmos validados

### 3. Sistema de Métricas e Analytics
**Localização**: Integrado em todos os módulos do aplicativo
**Funcionalidades**: Coleta, processamento, agregação de dados comportamentais
**Benefício**: Visão unificada do progresso terapêutico

### 4. Módulo de Configuração Dinâmica
**Localização**: Backend compartilhado com cache no frontend
**Funcionalidades**: Feature flags, configurações A/B testing, parâmetros terapêuticos
**Benefício**: Ajustes em tempo real sem necessidade de atualizações do app

### 5. Sistema de Logs Estruturados
**Localização**: Implementado em todos os serviços
**Funcionalidades**: Logging padronizado, correlação de requisições, auditoria
**Benefício**: Debugging eficiente e conformidade regulatória

## Fluxo de Dados e Comunicação

### Comunicação App ↔ API
- **Protocolo**: HTTPS com TLS 1.3 para todas as comunicações
- **Formato**: JSON para payloads com compressão gzip
- **Autenticação**: Bearer tokens JWT com refresh automático
- **Rate Limiting**: Controle por usuário para prevenir abuso

### Sincronização Offline
- **Storage Local**: SQLite no dispositivo para dados críticos
- **Conflict Resolution**: Timestamp-based com prioridade para dados mais recentes
- **Sync Strategy**: Delta sync para otimizar tráfego de rede

## Escalabilidade e Performance

### Estratégias de Escalabilidade
- **Horizontal Scaling**: Auto-scaling de pods no GKE baseado em métricas
- **Database Sharding**: Particionamento por região geográfica quando necessário
- **CDN Global**: Cache de assets próximo aos usuários finais
- **Connection Pooling**: Reutilização eficiente de conexões de banco

### Otimizações de Performance
- **Lazy Loading**: Carregamento sob demanda de conteúdo não crítico
- **Image Optimization**: Compressão adaptativa baseada na qualidade da conexão
- **Bundle Splitting**: Separação de código por funcionalidade no Flutter
- **Database Indexing**: Índices otimizados para queries frequentes

## Segurança e Conformidade

### Medidas de Segurança
- **Encryption at Rest**: AES-256 para dados sensíveis no banco
- **Encryption in Transit**: TLS 1.3 para todas as comunicações
- **API Security**: Rate limiting, input validation, SQL injection prevention
- **Access Control**: RBAC com princípio de menor privilégio

### Conformidade Regulatória
- **LGPD Compliance**: Consentimento explícito, portabilidade, direito ao esquecimento
- **SOC 2 Type II**: Controles de segurança auditados anualmente
- **ISO 27001**: Framework de gestão de segurança da informação

## Monitoramento e Observabilidade

### Métricas de Sistema
- **Application Performance Monitoring (APM)**: Latência, throughput, error rate
- **Infrastructure Monitoring**: CPU, memória, rede, storage
- **Business Metrics**: Engagement terapêutico, taxa de recuperação, retenção

### Alertas Proativos
- **SLA Monitoring**: Alertas automáticos para violações de SLA
- **Error Tracking**: Notificação imediata de erros críticos
- **Capacity Planning**: Previsão de necessidades de recursos
