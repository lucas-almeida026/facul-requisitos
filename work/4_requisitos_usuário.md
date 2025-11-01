# Definição de Requisitos do Usuário

Este documento descreve os serviços fornecidos pelo sistema betSTOP aos seus usuários. Os requisitos estão organizados por módulos funcionais e incluem tanto requisitos funcionais (RF) quanto não funcionais (RNF).

## Requisitos Funcionais

### Módulo de Autenticação e Perfil do Usuário

**#RF01** - O sistema deve permitir o cadastro de novos usuários com email e senha.

**#RF02** - O sistema deve permitir login de usuários cadastrados.

**#RF03** - O sistema deve permitir recuperação de senha via email.

**#RF04** - O sistema deve permitir criação e edição de perfil do usuário.

**#RF05** - O sistema pode permitir login através de redes sociais (Google, Facebook).

### Módulo de Simulação de Jogos

**#RF06** - O sistema deve simular jogos de caça-níqueis com interface similar aos aplicativos de cassino online.

**#RF07** - O sistema deve simular jogos de roleta com apostas virtuais.

**#RF08** - O sistema deve simular jogos de blackjack básico.

**#RF09** - O sistema deve simular jogos do tipo "crash" ou "aviãozinho".

**#RF10** - O sistema deve permitir apostas virtuais usando créditos fictícios.

**#RF11** - O sistema deve exibir ganhos e perdas de forma realística durante as simulações.

### Módulo de Monitoramento e Análise Comportamental

**#RF12** - O sistema deve registrar o tempo gasto pelo usuário em cada sessão de jogo.

**#RF13** - O sistema deve calcular e exibir o tempo total gasto no aplicativo.

**#RF14** - O sistema deve simular e registrar valores monetários "gastos" durante as sessões.

**#RF15** - O sistema deve gerar relatórios de uso semanal e mensal.

**#RF16** - O sistema deve identificar padrões de uso compulsivo baseado no tempo e frequência.

### Módulo de Intervenções Cognitivas

**#RF17** - O sistema deve enviar notificações push com mensagens provocativas em momentos estratégicos.

**#RF18** - O sistema deve exibir pop-ups com reflexões sobre custos do vício durante o jogo.

**#RF19** - O sistema deve mostrar comparações entre dinheiro "gasto" virtualmente e bens essenciais.

**#RF20** - O sistema deve apresentar estatísticas de perdas financeiras de forma impactante.

**#RF21** - O sistema deve interromper sessões longas com mensagens de reflexão.

**#RF22** - O sistema pode exibir depoimentos de pessoas em recuperação.

### Módulo de Sistema de Recompensas e Progresso

**#RF23** - O sistema deve calcular e exibir dias consecutivos sem jogar em aplicativos reais.

**#RF24** - O sistema deve oferecer badges e conquistas por marcos de recuperação.

**#RF25** - O sistema deve calcular dinheiro "economizado" por não jogar em sites reais.

**#RF26** - O sistema pode permitir definição de metas pessoais de recuperação.

**#RF27** - O sistema deve exibir progresso visual da jornada de recuperação.

### Módulo de Conteúdo Educativo

**#RF28** - O sistema deve fornecer artigos sobre terapia cognitivo-comportamental.

**#RF29** - O sistema deve exibir informações sobre os malefícios do vício em jogos de azar.

**#RF30** - O sistema pode incluir exercícios de reenquadramento cognitivo.

**#RF31** - O sistema pode fornecer dicas de manejo de impulsos.

### Módulo de Suporte e Comunidade

**#RF32** - O sistema deve fornecer links para centros de apoio e tratamento.

**#RF33** - O sistema pode incluir chat ou fórum de apoio entre usuários.

**#RF34** - O sistema deve permitir compartilhamento de progresso com familiares designados.

**#RF35** - O sistema pode incluir sistema de mentoria entre usuários em recuperação.

## Requisitos Não Funcionais

### Desempenho

**#RNF01** - O sistema deve responder às interações do usuário em no máximo 0.5 segundos.

**#RNF02** - O sistema deve suportar pelo menos 1000 usuários simultâneos.

**#RNF03** - O sistema deve ter tempo de inicialização inferior a 2 segundos.

**#RNF04** - O sistema deve funcionar adequadamente com conexões de internet a partir de 3G.

### Usabilidade

**#RNF05** - O sistema deve ter interface intuitiva similar aos aplicativos de jogos populares.

**#RNF06** - O sistema deve ser acessível para usuários com deficiências visuais básicas.

**#RNF07** - O sistema deve funcionar em dispositivos com telas de 4 polegadas ou maiores.

**#RNF08** - O sistema deve ter navegação simples com no máximo 3 níveis de profundidade.

### Compatibilidade

**#RNF09** - O sistema deve funcionar em dispositivos Android versão 7.0 ou superior.

**#RNF10** - O sistema deve funcionar em dispositivos iOS versão 12.0 ou superior.

**#RNF11** - O sistema deve adaptar-se automaticamente a diferentes tamanhos de tela.

### Segurança e Privacidade

**#RNF12** - O sistema deve criptografar todos os dados pessoais do usuário.

**#RNF13** - O sistema deve cumprir as regulamentações da LGPD (Lei Geral de Proteção de Dados).

**#RNF14** - O sistema deve permitir exclusão completa dos dados do usuário sob solicitação.

**#RNF15** - O sistema deve implementar autenticação segura para acesso aos dados.

### Confiabilidade

**#RNF16** - O sistema deve ter disponibilidade mínima de 95% do tempo.

**#RNF17** - O sistema deve realizar backup automático dos dados do usuário.

**#RNF18** - O sistema deve se recuperar de falhas em no máximo 30 segundos.

### Manutenibilidade

**#RNF19** - O sistema deve permitir atualizações sem perda de dados do usuário.

**#RNF20** - O sistema deve gerar logs detalhados para diagnóstico de problemas.

**#RNF21** - O sistema deve ter arquitetura modular para facilitar manutenção.

### Portabilidade

**#RNF22** - O sistema deve funcionar offline para funcionalidades básicas de simulação.

**#RNF23** - O sistema deve sincronizar dados quando a conexão for reestabelecida.

**#RNF24** - O sistema deve ocupar no máximo 200MB de espaço no dispositivo.

## Normas de Produto e Processo

### Normas de Produto

- O aplicativo deve seguir as diretrizes de design das plataformas Android (Material Design) e iOS (Human Interface Guidelines).
- O conteúdo terapêutico deve ser validado por profissionais de psicologia especializados em terapia cognitivo-comportamental.
- Todas as funcionalidades devem ser testadas por usuários reais em processo de recuperação.

### Normas de Processo

- O desenvolvimento deve seguir metodologias ágeis com sprints de 2 semanas.
- Todos os requisitos devem ser rastreáveis desde a concepção até a implementação.
- O código deve passar por revisão técnica antes da integração ao sistema principal.
- Testes automatizados devem cobrir no mínimo 80% das funcionalidades críticas.
