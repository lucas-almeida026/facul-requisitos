# Especificação de Requisitos do Sistema

Este documento detalha os requisitos funcionais e não funcionais do sistema betSTOP, fazendo referência aos requisitos definidos na seção 4 (Definição de Requisitos do Usuário) utilizando os mesmos identificadores únicos.

## Especificação de Requisitos Funcionais

### Módulo de Autenticação e Perfil do Usuário

**#RF01** - Cadastro de Usuários
- O sistema deve validar formato de email durante o cadastro.
- O sistema deve exigir senha com mínimo 8 caracteres, incluindo letras e números.
- O sistema deve verificar unicidade do email no banco de dados.
- O sistema deve enviar email de confirmação após cadastro bem-sucedido.
- O sistema deve armazenar hash da senha com algoritmo bcrypt.

**#RF02** - Login de Usuários
- O sistema deve validar credenciais contra base de dados de usuários cadastrados.
- O sistema deve gerar token JWT válido por 24 horas após login bem-sucedido.
- O sistema deve registrar data e hora do último acesso.
- O sistema deve bloquear temporariamente usuário após 5 tentativas incorretas.
- O sistema deve permitir logout com invalidação do token ativo.

**#RF03** - Recuperação de Senha
- O sistema deve validar se email existe na base de dados antes de enviar recuperação.
- O sistema deve gerar token único de recuperação válido por 30 minutos.
- O sistema deve enviar email com link contendo token de recuperação.
- O sistema deve permitir redefinição de senha através do token válido.
- O sistema deve invalidar token após uso ou expiração.

**#RF04** - Gestão de Perfil
- O sistema deve permitir edição de nome, idade, ocupação e telefone.
- O sistema deve permitir upload de foto de perfil em formatos JPG, PNG (máx 5MB).
- O sistema deve permitir alteração de senha mediante confirmação da senha atual.
- O sistema deve manter histórico de alterações no perfil com timestamp.
- O sistema deve permitir exclusão de conta com confirmação dupla.

**#RF05** - Login Social (Opcional)
- O sistema pode integrar com Google OAuth 2.0 para autenticação.
- O sistema pode integrar com Facebook Login API.
- O sistema deve criar perfil automaticamente com dados básicos da rede social.
- O sistema deve associar contas sociais a perfis existentes mediante confirmação.
- O sistema deve permitir desvinculação de contas sociais.

### Módulo de Simulação de Jogos

**#RF06** - Simulação de Caça-Níqueis
- O sistema deve apresentar interface com 3 ou 5 rolos virtuais.
- O sistema deve incluir símbolos temáticos similares aos jogos populares.
- O sistema deve calcular resultados baseados em algoritmo pseudoaleatório.
- O sistema deve exibir animações de giro e parada dos rolos.
- O sistema deve reproduzir efeitos sonoros durante jogadas.
- O sistema deve mostrar linhas de pagamento e multiplicadores.

**#RF07** - Simulação de Roleta
- O sistema deve apresentar mesa de roleta européia (37 números).
- O sistema deve permitir apostas em números, cores, pares/ímpares e terços.
- O sistema deve animar bola girando na roleta antes do resultado.
- O sistema deve destacar visualmente número e apostas vencedoras.
- O sistema deve calcular pagamentos conforme odds reais da roleta.

**#RF08** - Simulação de Blackjack
- O sistema deve usar baralho padrão de 52 cartas.
- O sistema deve distribuir cartas inicial (2 para jogador, 2 para dealer).
- O sistema deve permitir ações: pedir carta, parar, dobrar aposta.
- O sistema deve calcular valores das mãos (Ás como 1 ou 11).
- O sistema deve determinar vencedor conforme regras padrão do blackjack.

**#RF09** - Simulação de Crash/Aviãozinho
- O sistema deve exibir gráfico com multiplicador crescente em tempo real.
- O sistema deve permitir entrada de aposta antes do início da rodada.
- O sistema deve permitir retirada da aposta a qualquer momento durante subida.
- O sistema deve "crashar" em momento aleatório, zerando multiplicador.
- O sistema deve calcular ganho baseado no multiplicador no momento da retirada.

**#RF10** - Sistema de Apostas Virtuais
- O sistema deve fornecer saldo inicial de créditos fictícios (ex: 10.000 moedas).
- O sistema deve permitir definição de valores de aposta para cada jogo.
- O sistema deve debitar apostas do saldo antes de cada jogada.
- O sistema deve creditar ganhos no saldo após resultados positivos.
- O sistema deve recarregar saldo automaticamente quando zerado.
- O sistema deve registrar todas as transações virtuais.

**#RF11** - Exibição Realística de Resultados
- O sistema deve mostrar saldo atual em destaque na interface.
- O sistema deve exibir histórico das últimas 10 jogadas.
- O sistema deve apresentar estatísticas de sessão (ganhos, perdas, tempo).
- O sistema deve usar cores e animações para enfatizar grandes ganhos.
- O sistema deve simular "quase acertos" para manter engajamento.

### Módulo de Monitoramento e Análise Comportamental

**#RF12** - Registro de Tempo por Sessão
- O sistema deve iniciar cronômetro automaticamente ao abrir qualquer jogo.
- O sistema deve pausar contagem quando aplicativo vai para segundo plano.
- O sistema deve retomar contagem quando aplicativo volta ao foco.
- O sistema deve finalizar sessão após 10 minutos de inatividade.
- O sistema deve armazenar duração exata de cada sessão no banco de dados.

**#RF13** - Cálculo de Tempo Total
- O sistema deve somar todas as sessões do usuário desde o cadastro.
- O sistema deve exibir tempo total em formato horas:minutos:segundos.
- O sistema deve mostrar tempo médio por sessão.
- O sistema deve calcular tempo gasto por tipo de jogo.
- O sistema deve apresentar comparação com semanas anteriores.

**#RF14** - Simulação de Gastos Monetários
- O sistema deve converter créditos virtuais em valores reais (ex: 1 crédito = R$ 0,10).
- O sistema deve somar total "gasto" durante cada sessão.
- O sistema deve acumular gastos totais desde o cadastro.
- O sistema deve calcular gasto médio por sessão e por dia.
- O sistema deve projetar gastos mensais baseado no comportamento atual.

**#RF15** - Geração de Relatórios
- O sistema deve gerar relatório semanal com tempo gasto e valores simulados.
- O sistema deve gerar relatório mensal com análise de tendências.
- O sistema deve incluir gráficos de evolução temporal do uso.
- O sistema deve permitir exportação de relatórios em formato PDF.
- O sistema deve enviar relatórios por email mediante solicitação.

**#RF16** - Identificação de Padrões Compulsivos
- O sistema deve detectar sessões superiores a 2 horas consecutivas.
- O sistema deve identificar uso em horários inadequados (madrugada, trabalho).
- O sistema deve reconhecer frequência diária superior a 4 sessões.
- O sistema deve detectar escalada no tempo de uso semanal.
- O sistema deve alertar sobre comportamentos identificados como de risco.

### Módulo de Intervenções Cognitivas

**#RF17** - Notificações Push Estratégicas
- O sistema deve enviar notificação após 45 minutos de jogo contínuo.
- O sistema deve personalizar mensagens baseado no perfil do usuário.
- O sistema deve variar conteúdo das mensagens para evitar habituação.
- O sistema deve incluir perguntas reflexivas nas notificações.
- O sistema deve permitir agendamento de lembretes de reflexão.

**#RF18** - Pop-ups Durante Jogos
- O sistema deve exibir pop-up a cada 30 minutos de jogo ativo.
- O sistema deve mostrar custos reais equivalentes aos gastos virtuais.
- O sistema deve incluir botão "continuar" e "fazer uma pausa".
- O sistema deve registrar escolhas do usuário para análise comportamental.
- O sistema deve aumentar frequência de pop-ups conforme tempo de uso.

**#RF19** - Comparações com Bens Essenciais
- O sistema deve manter base de dados com preços de itens essenciais.
- O sistema deve comparar gastos virtuais com alimentação, transporte, educação.
- O sistema deve apresentar comparações visuais (imagens + valores).
- O sistema deve personalizar comparações baseado no perfil socioeconômico.
- O sistema deve atualizar valores mensalmente conforme inflação.

**#RF20** - Estatísticas de Perdas Impactantes
- O sistema deve calcular total de perdas virtuais acumuladas.
- O sistema deve projetar perdas anuais baseado no ritmo atual.
- O sistema deve comparar com renda média brasileira.
- O sistema deve apresentar dados em formatos visuais impactantes.
- O sistema deve incluir depoimentos reais sobre consequências financeiras.

**#RF21** - Interrupção de Sessões Longas
- O sistema deve forçar pausa após 90 minutos de jogo contínuo.
- O sistema deve exibir mensagem obrigatória sobre riscos do uso prolongado.
- O sistema deve incluir exercício de respiração de 2 minutos.
- O sistema deve solicitar reflexão sobre motivação para continuar jogando.
- O sistema deve permitir retorno ao jogo após intervalo mínimo de 15 minutos.

**#RF22** - Depoimentos de Recuperação (Opcional)
- O sistema pode incluir banco de dados com depoimentos anônimos.
- O sistema pode exibir depoimentos relevantes ao comportamento atual.
- O sistema pode permitir que usuários compartilhem suas próprias histórias.
- O sistema pode incluir vídeos curtos de pessoas em recuperação.
- O sistema pode conectar usuários com mentores em recuperação.

### Módulo de Sistema de Recompensas e Progresso

**#RF23** - Contador de Dias sem Jogos Reais
- O sistema deve permitir registro manual de "check-in" diário.
- O sistema deve manter contador de dias consecutivos sem apostas reais.
- O sistema deve zerar contador se usuário reportar recaída.
- O sistema deve exibir contador em destaque na tela principal.
- O sistema deve celebrar marcos importantes (7, 30, 90, 365 dias).

**#RF24** - Sistema de Badges e Conquistas
- O sistema deve incluir badges para marcos temporais de recuperação.
- O sistema deve criar conquistas para uso responsável do próprio aplicativo.
- O system deve incluir badges para engajamento com conteúdo educativo.
- O sistema deve permitir compartilhamento de conquistas nas redes sociais.
- O sistema deve manter galeria pessoal de badges conquistadas.

**#RF25** - Cálculo de Dinheiro Economizado
- O sistema deve permitir entrada do valor médio gasto anteriormente em apostas.
- O sistema deve calcular economia diária baseada no histórico informado.
- O sistema deve acumular valor economizado desde início da recuperação.
- O sistema deve projetar economia anual caso mantenha recuperação.
- O sistema deve sugerir usos alternativos para dinheiro economizado.

**#RF26** - Definição de Metas Pessoais (Opcional)
- O sistema pode permitir criação de metas financeiras de economia.
- O sistema pode incluir metas de tempo sem jogos (30, 60, 90 dias).
- O sistema pode permitir metas de engajamento com tratamento.
- O sistema pode incluir sistema de recompensas virtuais por metas atingidas.
- O sistema pode enviar lembretes sobre progresso das metas.

**#RF27** - Progresso Visual da Recuperação
- O sistema deve exibir barra de progresso para próximo marco temporal.
- O sistema deve incluir gráfico de evolução dos últimos 30 dias.
- O sistema deve mostrar linha temporal com momentos importantes.
- O sistema deve usar cores motivacionais para representar progresso.
- O sistema deve permitir anotações pessoais sobre jornada de recuperação.

### Módulo de Conteúdo Educativo

**#RF28** - Artigos sobre TCC
- O sistema deve incluir biblioteca com artigos sobre terapia cognitivo-comportamental.
- O sistema deve organizar conteúdo por temas: pensamentos, emoções, comportamentos.
- O sistema deve incluir exercícios práticos para cada conceito apresentado.
- O sistema deve permitir marcação de artigos como favoritos.
- O sistema deve rastrear progresso de leitura do usuário.

**#RF29** - Informações sobre Malefícios
- O sistema deve apresentar dados estatísticos sobre vício em jogos.
- O sistema deve incluir informações sobre impactos na saúde mental.
- O sistema deve abordar consequências financeiras e familiares.
- O sistema deve usar linguagem acessível e evidências científicas.
- O sistema deve incluir fontes confiáveis para todas as informações.

**#RF30** - Exercícios de Reenquadramento (Opcional)
- O sistema pode incluir exercícios guiados de identificação de pensamentos.
- O sistema pode apresentar técnicas para questionar crenças sobre sorte.
- O sistema pode incluir exercícios de visualização de consequências.
- O sistema pode oferecer exercícios de mindfulness para controle de impulsos.
- O sistema pode permitir anotações pessoais sobre insights obtidos.

**#RF31** - Dicas de Manejo de Impulsos (Opcional)
- O sistema pode incluir técnicas de respiração para momentos de impulso.
- O sistema pode apresentar estratégias de distração e ocupação mental.
- O sistema pode incluir lista de atividades alternativas aos jogos.
- O sistema pode oferecer exercícios físicos rápidos para reduzir ansiedade.
- O sistema pode incluir frases de autoafirmação para momentos difíceis.

### Módulo de Suporte e Comunidade

**#RF32** - Links para Centros de Apoio
- O sistema deve manter base atualizada de centros de tratamento por região.
- O sistema deve incluir telefones de linhas de apoio 24 horas.
- O sistema deve apresentar informações sobre grupos presenciais de apoio.
- O sistema deve incluir links para terapeutas especializados em vícios.
- O sistema deve permitir busca por localização geográfica.

**#RF33** - Chat de Apoio (Opcional)
- O sistema pode incluir chat moderado entre usuários em recuperação.
- O sistema pode implementar fórum com categorias temáticas.
- O sistema pode incluir sistema de reputação para usuários ativos.
- O sistema pode permitir conversas privadas entre usuários.
- O sistema pode incluir moderação automática para conteúdo inadequado.

**#RF34** - Compartilhamento com Familiares
- O sistema deve permitir convite de familiares para acompanhamento.
- O sistema deve gerar relatórios simplificados para familiares autorizados.
- O sistema deve permitir comunicação indireta com familiares (progresso, marcos).
- O sistema deve manter privacidade de detalhes sensíveis do usuário.
- O sistema deve permitir revogação de acesso familiar a qualquer momento.

**#RF35** - Sistema de Mentoria (Opcional)
- O sistema pode conectar novos usuários com veteranos em recuperação.
- O sistema pode incluir sistema de matching baseado em perfil e localização.
- O sistema pode permitir comunicação estruturada entre mentor e mentorado.
- O sistema pode incluir treinamento básico para usuários mentores.
- O sistema pode implementar avaliação da qualidade da mentoria oferecida.