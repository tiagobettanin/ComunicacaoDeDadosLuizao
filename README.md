### Análise Técnica Refinada das Histórias de Usuário

Este documento detalha o plano técnico para a implementação das funcionalidades, consolidando os requisitos, o planejamento de arquitetura e os critérios de validação.

---

## História 1: Desafios em Grupo

* **Como** usuário, **eu quero** criar desafios em grupo, **para** poder competir ou colaborar com meus amigos.

### 1. Planejamento (Planning)

#### Design de Interface (UI/UX)
* **Ponto de Entrada:** Um botão de destaque `"Criar Desafio"` será posicionado na tela social principal.
* **Formulário de Criação:** Um modal será apresentado para a criação do desafio, contendo os campos: `Nome do Desafio` (texto), `Meta` (número e uma unidade pré-definida, ex: "km", "horas", "vezes") e `Data de Término` (seletor de data).
* **Seleção de Participantes:** Uma interface de busca e múltipla seleção permitirá ao criador convidar amigos da sua lista.
* **Tela de Acompanhamento:** Uma tela dedicada para cada desafio exibirá o progresso coletivo (barra geral) e o progresso individual de cada participante.

#### Arquitetura e Backend
* **Serviços Envolvidos:** `Sistema de Desafios (Backend)`, `Sistema de Notificação (Backend)`, `Banco de Dados`.
* **Estrutura de Dados:**
    * Tabela `Challenges`: `id`, `name`, `goal`, `goal_unit`, `end_date`, `creator_user_id`, `status`.
    * Tabela `Challenge_Participants`: `id`, `challenge_id`, `user_id`, `progress`, `status` ('pending', 'accepted', 'declined').
* **API Endpoints:**
    * `POST /challenges`: Aciona a criação de um novo desafio.
    * `GET /challenges`: Lista os desafios do usuário (ativos e pendentes).
    * `GET /challenges/{id}`: Retorna os detalhes de um desafio específico.
    * `POST /challenges/{id}/accept`: Permite ao usuário aceitar um convite.
    * `POST /challenges/{id}/decline`: Permite ao usuário recusar um convite.
    * `PUT /challenges/{id}/progress`: Atualiza o progresso de um participante.

### 2. Execução (Execution)

#### Desenvolvimento Frontend
1.  Implementar o fluxo de criação de desafio (botão, modal, formulário e seleção de amigos).
2.  Construir a tela de visualização do desafio, consumindo os dados da API `GET /challenges/{id}`.
3.  Integrar com um sistema de notificações para exibir convites pendentes.
4.  Implementar as ações de aceitar e recusar convites.

#### Desenvolvimento Backend
1.  Implementar a lógica de `POST /challenges`, que deve validar os dados e salvar no banco.
2.  Após salvar, o serviço deve disparar os convites através do **Sistema de Notificação**.
3.  Desenvolver a lógica para atualização de progresso, validando se o desafio ainda está ativo.
4.  Criar os demais endpoints para consulta e gerenciamento dos desafios.

### 3. Revisão (Testes de Aceitação)
1.  **Criação e Convite:** Criar um desafio, selecionar 3 amigos e finalizar. Validar se o desafio foi criado e se os 3 amigos receberam a notificação do convite.
2.  **Aceite e Recusa:** Logar com um amigo, aceitar o convite. Logar com outro amigo, recusar o convite. Verificar se os status de ambos foram atualizados corretamente na tela do desafio.
3.  **Registro de Progresso:** Registrar progresso para o participante que aceitou. Validar se a barra de progresso individual e a coletiva foram atualizadas corretamente.
4.  **Visualização de Detalhes:** Como convidado, verificar se todos os detalhes do desafio (meta, participantes, prazo) estão visíveis antes de aceitar.

---

## História 2: Hábitos Privados

* **Como** usuário, **eu quero** deixar alguns hábitos privados, **para que** meus amigos não vejam tudo o que eu estou fazendo.

### 1. Planejamento (Planning)

#### Design de Interface (UI/UX)
* **Controle de Privacidade:** Na tela de criação/edição de hábito, um componente `Switch` com o rótulo `"Tornar este hábito privado"` controlará a visibilidade.
* **Identificação Visual:** Um ícone de cadeado (🔒) será exibido ao lado do nome dos hábitos privados na lista pessoal do usuário.

#### Arquitetura e Backend
* **Serviços Envolvidos:** `Sistema de Hábitos (Backend)`, `Banco de Dados`.
* **Estrutura de Dados:** Adicionar uma coluna booleana `is_private` na tabela `Habits` (padrão: `false`).
* **Lógica de Negócio:**
    * Todas as consultas que alimentam áreas sociais (feed, rankings) **devem** incluir a condição `WHERE is_private = false`.
    * Definir regra de negócio para atividades passadas quando um hábito se torna privado (se devem ser removidas retroativamente).

### 2. Execução (Execution)

#### Desenvolvimento Frontend
1.  Implementar o `Switch` de privacidade no formulário de criação/edição de hábito.
2.  Adicionar a lógica para exibir condicionalmente o ícone de cadeado na lista de hábitos.

#### Desenvolvimento Backend
1.  Atualizar os endpoints `POST /habits` e `PUT /habits/{id}` para receber e salvar o valor `is_private`.
2.  Refatorar todos os endpoints de dados sociais (`GET /feed`, `GET /ranking`, etc.) para aplicar o filtro de privacidade rigorosamente.

### 3. Revisão (Testes de Aceitação)
1.  **Criação Privada:** Criar um novo hábito e marcá-lo como "Privado". Verificar se o ícone de cadeado aparece na lista.
2.  **Verificação de Feed:** Logar com a conta de um amigo e confirmar que o hábito privado e suas atividades **não** aparecem no feed.
3.  **Isolamento de Pontuação:** Concluir um hábito privado e verificar que a pontuação em rankings públicos **não** foi alterada.
4.  **Alteração de Privacidade:** Editar um hábito privado, torná-lo público e salvar. Verificar se ele **agora aparece** no feed do amigo. Fazer o processo inverso e confirmar que ele some novamente.
5.  **Teste de Retroatividade:** Tornar um hábito com histórico público em privado e verificar se suas atividades passadas são removidas do feed de amigos.

---

## História 3: Troféus por Conquista

* **Como** usuário, **eu quero** marcar um hábito como "concluído", **para que** ele saia da minha lista diária e vire um troféu.

### 1. Planejamento (Planning)

#### Design de Interface (UI/UX)
* **Opção de Conclusão:** Na tela de detalhes de um hábito, adicionar uma opção `"Marcar como Concluído"`.
* **Seção de Conquistas:** Criar uma nova área no perfil do usuário chamada `"Meus Troféus"`.
* **Card de Troféu:** Projetar um card para cada troféu, exibindo o nome do hábito, a maior sequência (`streak`) e a data de conclusão.
* **Funcionalidades do Troféu:** Incluir um botão `"Começar de Novo"` para recriar o hábito e considerar uma opção de `"Reativar Hábito"`.

#### Arquitetura e Backend
* **Serviços Envolvidos:** `Sistema de Hábitos (Backend)`, `Banco de Dados`.
* **Estrutura de Dados:**
    * Adicionar uma coluna `status` na tabela `Habits` (valores: 'active', 'completed').
    * Adicionar colunas `completed_at` (timestamp) e `final_streak` (integer).
* **API Endpoints:**
    * `POST /habits/{id}/complete`: Marca o hábito como concluído.
    * `GET /habits?status=active`: Endpoint principal para a lista diária.
    * `GET /trophies` (ou `GET /habits?status=completed`): Para a seção "Meus Troféus".

### 2. Execução (Execution)

#### Desenvolvimento Frontend
1.  Implementar a opção "Marcar como Concluído" e a chamada à API correspondente.
2.  Filtrar a lista de hábitos diários para exibir apenas os de status `active`.
3.  Construir a tela "Meus Troféus" consumindo os dados do endpoint `/trophies`.
4.  Implementar a funcionalidade do botão "Começar de Novo", que pré-preenche a tela de criação.

#### Desenvolvimento Backend
1.  Criar a lógica no endpoint `/complete` que altera o status, registra a data e armazena a `streak` final.
2.  Garantir que os sistemas de notificação e agendamento de tarefas diárias ignorem hábitos com status `completed`.
3.  Implementar o endpoint que retorna a lista de troféus.

### 3. Revisão (Testes de Aceitação)
1.  **Conclusão de Hábito:** Marcar um hábito como "Concluído".
2.  **Validação de Listas:** Confirmar que o hábito foi **removido** da lista diária e **adicionado** à seção "Meus Troféus".
3.  **Verificação de Dados:** No troféu, verificar se a maior `streak` e a data de conclusão estão corretas.
4.  **Teste de Recriação:** Clicar em "Começar de Novo" e confirmar que a tela de criação é aberta com os dados do hábito original pré-preenchidos.
5.  **Teste de Notificações:** No dia seguinte à conclusão, verificar que o usuário **não** recebe mais notificações sobre aquele hábito.