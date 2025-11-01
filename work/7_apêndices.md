# Apêndices

Este documento fornece informações detalhadas e específicas relacionadas ao desenvolvimento, implantação e operação do sistema betSTOP, incluindo requisitos de hardware, estruturação de dados, ferramentas de desenvolvimento e configurações técnicas.

## Apêndice A - Requisitos de Hardware do Usuário Final

### A.1 Requisitos Mínimos para Dispositivos Android

**Sistema Operacional**: Android 7.0 (API level 24) ou superior
**Arquitetura de Processador**: ARMv7, ARM64, ou x86
**Memória RAM**: Mínimo 2GB, recomendado 4GB
**Armazenamento**: 500MB livres para instalação, 200MB para dados do usuário
**Conectividade**: Wi-Fi ou dados móveis (3G/4G/5G)
**Resolução de Tela**: Mínimo 720x1280 pixels (4 polegadas)
**Sensores**: Acelerômetro (opcional para feedback haptic)

### A.2 Requisitos Mínimos para Dispositivos iOS

**Sistema Operacional**: iOS 12.0 ou superior
**Dispositivos Compatíveis**: iPhone 6s ou posterior, iPad (6ª geração) ou posterior
**Memória RAM**: Mínimo 2GB (inerente aos dispositivos suportados)
**Armazenamento**: 500MB livres para instalação, 200MB para dados do usuário
**Conectividade**: Wi-Fi ou dados móveis (3G/4G/5G)
**Resolução de Tela**: Suporte nativo para todas as resoluções dos dispositivos compatíveis

### A.3 Configurações Recomendadas para Performance Ótima

**Android**: Snapdragon 660 ou equivalente, 4GB RAM, Android 10+
**iOS**: iPhone 8 ou posterior, iOS 14+
**Conectividade**: 4G LTE ou Wi-Fi com mínimo 5 Mbps
**Bateria**: Capacidade para 4+ horas de uso contínuo

## Apêndice B - Requisitos de Infraestrutura de Servidor

### B.1 Especificações do Ambiente de Produção

**Plataforma**: Google Cloud Platform (GCP)
**Orquestração**: Google Kubernetes Engine (GKE)
**Configuração de Cluster**: Mínimo 3 nós, auto-scaling 3-10 nós
**Tipo de Máquina**: n2-standard-4 (4 vCPUs, 16GB RAM) por nó
**Armazenamento**: SSD persistente, mínimo 100GB por nó
**Rede**: VPC privada com firewall configurado
**Balanceador de Carga**: Google Cloud Load Balancer com SSL/TLS

### B.2 Especificações do Banco de Dados

**MySQL Cloud SQL**:
- Instância: db-n1-standard-2 (2 vCPUs, 7.5GB RAM)
- Armazenamento: SSD de 100GB com auto-resize até 500GB
- Backup: Diário automático com retenção de 30 dias
- Alta Disponibilidade: Configuração regional com failover automático

**Redis Cloud**:
- Configuração: 1GB RAM com clustering habilitado
- Persistência: RDB + AOF para durabilidade
- Replicação: Multi-zona para alta disponibilidade

### B.3 Requisitos de Monitoramento

**Google Cloud Monitoring**: Coleta de métricas de infraestrutura
**Application Performance Monitoring**: Integração com ferramentas de APM
**Alertas**: Configuração para CPU > 80%, memória > 85%, latência > 2s
**Logs**: Centralização via Google Cloud Logging com retenção de 90 dias

## Apêndice C - Estruturação do Banco de Dados

### C.1 Esquema do Banco de Dados MySQL

#### Tabela: users
```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    name VARCHAR(100) NOT NULL,
    age INT,
    occupation VARCHAR(100),
    phone VARCHAR(20),
    profile_picture_url VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    last_login TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE,
    email_verified BOOLEAN DEFAULT FALSE
);
```

#### Tabela: game_sessions
```sql
CREATE TABLE game_sessions (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    game_type ENUM('slots', 'roulette', 'blackjack', 'crash') NOT NULL,
    start_time TIMESTAMP NOT NULL,
    end_time TIMESTAMP,
    duration_seconds INT,
    virtual_credits_spent DECIMAL(10,2),
    virtual_credits_won DECIMAL(10,2),
    actions_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

#### Tabela: therapeutic_interventions
```sql
CREATE TABLE therapeutic_interventions (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    intervention_type ENUM('notification', 'popup', 'session_break', 'comparison') NOT NULL,
    trigger_condition VARCHAR(255),
    message_content TEXT,
    user_response ENUM('continued', 'paused', 'reflected', 'ignored'),
    effectiveness_score TINYINT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

#### Tabela: recovery_progress
```sql
CREATE TABLE recovery_progress (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    days_without_real_gambling INT DEFAULT 0,
    total_virtual_money_saved DECIMAL(12,2) DEFAULT 0,
    badges_earned JSON,
    milestones_achieved JSON,
    personal_goals JSON,
    last_checkin_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

#### Tabela: educational_content
```sql
CREATE TABLE educational_content (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    content_type ENUM('article', 'exercise', 'video', 'infographic') NOT NULL,
    category ENUM('tcc', 'harm_info', 'coping_strategies', 'testimonials') NOT NULL,
    content TEXT NOT NULL,
    author VARCHAR(100),
    reading_time_minutes INT,
    difficulty_level ENUM('beginner', 'intermediate', 'advanced') DEFAULT 'beginner',
    is_published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

#### Tabela: user_content_progress
```sql
CREATE TABLE user_content_progress (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    content_id BIGINT NOT NULL,
    progress_percentage TINYINT DEFAULT 0,
    is_completed BOOLEAN DEFAULT FALSE,
    is_favorited BOOLEAN DEFAULT FALSE,
    time_spent_seconds INT DEFAULT 0,
    last_accessed TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (content_id) REFERENCES educational_content(id) ON DELETE CASCADE,
    UNIQUE KEY unique_user_content (user_id, content_id)
);
```

### C.2 Estruturação do Cache Redis

#### Chaves de Sessão de Usuário
- Padrão: `session:{user_id}`
- TTL: 24 horas
- Estrutura: Hash contendo JWT token, último acesso, preferências

#### Cache de Métricas em Tempo Real
- Padrão: `metrics:{user_id}:{date}`
- TTL: 7 dias
- Estrutura: Hash com tempo de jogo, créditos gastos, sessões ativas

#### Rate Limiting
- Padrão: `rate_limit:{endpoint}:{user_id}`
- TTL: 1 hora
- Estrutura: Counter com número de requisições

#### Cache de Consultas Frequentes
- Padrão: `query_cache:{hash_query}`
- TTL: 30 minutos
- Estrutura: String JSON com resultado da consulta

### C.3 Relacionamentos e Integridade

**Índices Principais**:
- `users.email` (único)
- `game_sessions.user_id, game_sessions.created_at`
- `therapeutic_interventions.user_id, therapeutic_interventions.created_at`
- `user_content_progress.user_id, user_content_progress.content_id`

**Constraints de Integridade**:
- Foreign keys com CASCADE DELETE para dados do usuário
- Check constraints para validação de ranges (age > 0, progress_percentage 0-100)
- Unique constraints para prevenir duplicatas críticas

## Apêndice D - Requisitos de Desenvolvimento

### D.1 Ambiente de Desenvolvimento Local

#### Requisitos de Hardware para Desenvolvedores
**Processador**: Intel i5/i7 ou AMD Ryzen 5/7 (mínimo 4 cores)
**Memória RAM**: 16GB (recomendado 32GB)
**Armazenamento**: SSD 512GB (mínimo 256GB livres)
**Sistema Operacional**: macOS 10.14+, Windows 10+, ou Ubuntu 18.04+
**Conectividade**: Internet banda larga para downloads e deploys

#### Ferramentas de Desenvolvimento Mobile
**Flutter SDK**: Versão estável mais recente (3.0+)
**Dart SDK**: Incluído com Flutter
**Android Studio**: Versão mais recente com Android SDK
**Xcode**: 12.0+ (apenas para desenvolvimento iOS em macOS)
**VS Code**: Com extensões Flutter e Dart
**Emuladores**: Android Emulator e iOS Simulator

#### Ferramentas de Desenvolvimento Backend
**Go**: Versão 1.19+ para desenvolvimento da API
**Docker**: Para containerização local e testes
**Docker Compose**: Para orquestração local de serviços
**MySQL**: 8.0+ para desenvolvimento local
**Redis**: 7.0+ para cache local
**Postman/Insomnia**: Para testes de API

### D.2 Ferramentas de CI/CD

**GitHub Actions**: Pipeline automatizado
**Docker Hub**: Registry de containers
**Terraform**: Infrastructure as Code
**Kubernetes CLI**: Para deploy e debug
**Google Cloud SDK**: Para integração com GCP

### D.3 Ferramentas de Qualidade

**Code Linting**: 
- Flutter: `flutter analyze`
- Go: `golint`, `go vet`

**Testing**:
- Flutter: `flutter test` para testes unitários
- Go: `go test` com coverage
- E2E: `integration_test` para Flutter

**Code Coverage**: Mínimo 80% para componentes críticos

## Apêndice E - Requisitos de UI/UX

### E.1 Design System e Diretrizes Visuais

#### Paleta de Cores Principal
**Primária**: #FF6B35 (Laranja vibrante - engajamento)
**Secundária**: #004E98 (Azul profundo - confiança)
**Sucesso**: #2ECC71 (Verde - progresso positivo)
**Alerta**: #F39C12 (Amarelo - atenção)
**Erro**: #E74C3C (Vermelho - urgência)
**Neutro**: #34495E (Cinza escuro - texto)

#### Tipografia
**Primária**: Inter (títulos e interface)
**Secundária**: Roboto (corpo de texto)
**Tamanhos**: 12px, 14px, 16px, 18px, 24px, 32px, 48px
**Peso**: Regular (400), Medium (500), Semi-Bold (600), Bold (700)

#### Espaçamento e Grid
**Unidade Base**: 8px
**Margins/Paddings**: 8px, 16px, 24px, 32px, 48px
**Grid System**: 12 colunas com gutters de 16px
**Breakpoints**: 320px (mobile), 768px (tablet), 1024px (desktop)

### E.2 Componentes de Interface

#### Componentes de Jogos
**Slot Machine**: Animação 60fps, símbolos customizados, efeitos sonoros
**Roulette Wheel**: Física realista, animação de bola, destacamento de resultados
**Card Games**: Sprites de alta qualidade, animações de distribuição
**Crash Graph**: Linha em tempo real, indicadores visuais de risco

#### Componentes Terapêuticos
**Progress Bars**: Animadas, cores motivacionais, marcos visuais
**Achievement Badges**: Ilustrações customizadas, animação de conquista
**Intervention Modals**: Design não intrusivo, botões claros, timer visual
**Statistics Charts**: Gráficos interativos, cores significativas

### E.3 Diretrizes de Acessibilidade

**Contraste**: Mínimo 4.5:1 para texto normal, 3:1 para texto grande
**Touch Targets**: Mínimo 44x44dp para elementos interativos
**Focus Management**: Ordem lógica de navegação por teclado
**Screen Readers**: Labels descritivos, roles semânticos
**Motion**: Opção para reduzir animações (respect prefers-reduced-motion)

### E.4 Ferramentas de Design

**Design**: Figma para prototipagem e design system
**Assets**: Adobe Illustrator para ícones vetoriais
**Animation**: Lottie para animações complexas
**Prototyping**: Principle ou ProtoPie para interações avançadas
**Testing**: Maze ou UsabilityHub para testes de usabilidade

## Apêndice F - Configurações de Segurança

### F.1 Certificados e Criptografia

**SSL/TLS**: Certificados Let's Encrypt com renovação automática
**API Encryption**: TLS 1.3 obrigatório para todas as comunicações
**Database Encryption**: AES-256 para dados em repouso
**Password Hashing**: bcrypt com salt rounds = 12
**JWT Signing**: RS256 com chaves RSA 2048-bit

### F.2 Configurações de Firewall

**Ingress Rules**:
- HTTPS (443) aberto para public
- HTTP (80) redirect para HTTPS
- API endpoints apenas via load balancer

**Egress Rules**:
- Database: apenas para clusters autorizados
- External APIs: lista branca de endpoints
- Email services: SMTP/TLS para providers aprovados

### F.3 Backup e Disaster Recovery

**Backup Schedule**:
- MySQL: Backup diário às 02:00 UTC
- Redis: Snapshot a cada 6 horas
- Application logs: Retenção de 90 dias

**Recovery Procedures**:
- RTO (Recovery Time Objective): 4 horas
- RPO (Recovery Point Objective): 1 hora
- Testing: Simulação mensal de disaster recovery

## Apêndice G - Conformidade e Regulamentações

### G.1 LGPD (Lei Geral de Proteção de Dados)

**Bases Legais**: Consentimento explícito para dados sensíveis
**Direitos do Titular**: Acesso, retificação, exclusão, portabilidade
**Data Protection Officer**: Designado e treinado
**Privacy by Design**: Implementado desde a concepção
**Audit Trail**: Log completo de acesso e modificação de dados

### G.2 Políticas de Retenção de Dados

**Dados do Usuário**: Mantidos enquanto conta estiver ativa + 1 ano
**Logs de Aplicação**: 90 dias para troubleshooting
**Métricas Agregadas**: 5 anos para pesquisa (anonimizadas)
**Backup Encryption**: AES-256 para todos os backups

### G.3 Compliance Frameworks

**ISO 27001**: Framework de segurança da informação
**SOC 2 Type II**: Auditoria anual de controles de segurança
**OWASP**: Top 10 security practices implementadas
**Google Cloud Compliance**: Certificações herdadas da plataforma

---

**Nota**: Todos os requisitos especificados nestes apêndices devem ser validados durante as fases de desenvolvimento e implementação. Atualizações podem ser necessárias conforme evolução das tecnologias e regulamentações aplicáveis.