### Análise Detalhada das Histórias de Usuário

Aqui está o detalhamento de cada funcionalidade, desde a concepção e planejamento técnico até os testes de validação.

---

## História 1: Desafios em Grupo

* **Como** usuário, **eu quero** criar desafios em grupo, **para** poder competir ou colaborar com meus amigos.

### 1. Planejamento (Planning)

#### Design de Interface (UI/UX)

* **Botão de Criação:** Posicionar um botão de destaque `"Criar Desafio"` na tela social ou no perfil do usuário.
* **Formulário de Criação:** Desenhar um modal ou uma nova tela para o formulário do desafio, contendo os campos: `Nome do Desafio` (texto), `Meta` (número e unidade, ex: "km", "horas", "vezes") e `Data de Término` (seletor de data).
* **Seleção de Amigos:** Criar uma interface que permita busca e múltipla seleção a partir da lista de amigos do usuário para enviar os convites.
* **Tela do Desafio:** Projetar a tela do desafio, que deve conter um título, a meta geral, a data de término, uma barra de progresso coletiva e uma lista de participantes com suas contribuições e progressos individuais.

#### Arquitetura e Backend

* **Estrutura de Dados:**
    * Tabela `Challenges`: `id`, `name`, `goal`, `goal_unit`, `end_date`, `creator_user_id`.
    * Tabela `Challenge_Participants`: `id`, `challenge_id` (chave estrangeira), `user_id` (chave estrangeira), `progress`, `status` ('pending', 'accepted').
* **API Endpoints:**
    * `POST /challenges`: Cria um novo desafio e envia os convites.
    * `GET /challenges`: Lista os desafios dos quais o usuário participa.
    * `GET /challenges/{id}`: Retorna os detalhes de um desafio específico, incluindo a lista de participantes e seus progressos.
    * `POST /challenges/{id}/accept`: Para um usuário aceitar um convite de desafio.
    * `PUT /challenges/{id}/progress`: Para um usuário atualizar seu progresso em um desafio.

### 2. Execução (Execution)

#### Desenvolvimento Frontend

1.  Implementar o botão `"Criar Desafio"` e o formulário de criação.
2.  Desenvolver o componente de seleção de amigos, que consome a lista de amigos via API.
3.  Construir a tela de visualização do desafio, que busca os dados na API e os renderiza de forma clara (barras de progresso, lista de participantes, etc.).
4.  Implementar a lógica de notificações para que os usuários vejam os convites para desafios.

#### Desenvolvimento Backend

1.  Implementar a lógica para criar um desafio, salvar os participantes convidados com status `"pending"` e disparar as notificações.
2.  Criar a lógica que permite aos usuários atualizarem seu progresso. O backend deve validar se o desafio ainda está ativo.
3.  Desenvolver os endpoints que retornam os dados de desafios para o frontend.

### 3. Revisão (Testes de Aceitação)

1.  **Verificar Visibilidade do Botão:** Navegar até a interface principal/social e confirmar que o botão `"Criar Desafio"` está visível e acessível.
2.  **Testar Criação de Desafio:** Preencher o formulário com nome, meta (ex: 20 km) e uma data futura. Confirmar que o desafio é criado com sucesso.
3.  **Testar Convite de Amigos:** Durante a criação, selecionar 2-3 amigos da lista e finalizar. Logar com a conta de um dos amigos convidados para verificar se o convite foi recebido.
4.  **Verificar Detalhes do Convite:** Como usuário convidado, abrir o convite e confirmar que todos os detalhes (nome, meta, quem mais foi convidado) estão visíveis.
5.  **Validar Tela de Progresso:** Fazer com que múltiplos participantes aceitem o convite e registrem progresso. Abrir a tela do desafio e confirmar que o progresso individual de cada um e o progresso coletivo são exibidos e calculados corretamente.

---

## História 2: Hábitos Privados

* **Como** usuário, **eu quero** deixar alguns hábitos privados, **para que** meus amigos não vejam tudo o que eu estou fazendo.

### 1. Planejamento (Planning)

#### Design de Interface (UI/UX)

* **Controle de Privacidade:** Adicionar um componente `Switch` ou `Checkbox` com o rótulo `"Tornar este hábito privado"` na tela de criação e edição de hábitos.
* **Identificação Visual:** Definir um ícone de "cadeado" (🔒) que será exibido ao lado do nome dos hábitos privados na lista pessoal do usuário.

#### Arquitetura e Backend

* **Estrutura de Dados:** Adicionar uma coluna booleana `is_private` na tabela `Habits`. O valor padrão deve ser `false` (público).
* **Lógica de Negócio:** Modificar todas as consultas de dados que alimentam áreas sociais (feed de atividades, rankings, desafios) para incluir a condição: `WHERE is_private = false`.

### 2. Execução (Execution)

#### Desenvolvimento Frontend

1.  Implementar o controle de privacidade (switch/checkbox) no formulário de hábitos.
2.  Na tela que lista os hábitos do usuário, adicionar uma lógica para exibir o ícone de cadeado se o campo `is_private` do hábito for `true`.

#### Desenvolvimento Backend

1.  Atualizar os endpoints `POST /habits` e `PUT /habits/{id}` para receber e salvar o valor `is_private`.
2.  Refatorar os endpoints que retornam dados sociais (`GET /feed`, `GET /ranking`) para que eles filtrem e excluam rigorosamente os hábitos privados e suas pontuações.

### 3. Revisão (Testes de Aceitação)

1.  **Testar Opção de Privacidade:** Criar um novo hábito e marcar a opção `"Privado"`. Salvar e editar novamente para garantir que a opção permaneceu marcada.
2.  **Verificar Feed de Amigos:** Logar com uma conta de amigo e verificar se o hábito privado criado **não** aparece no feed de atividades.
3.  **Confirmar Identificação Visual:** Logar como o dono do hábito e verificar se o ícone de cadeado é exibido ao lado do hábito na sua lista pessoal.
4.  **Validar Não Contabilização de Pontos:** Participar de um ranking. Concluir um hábito privado e verificar que a pontuação do ranking **não** foi alterada.
5.  **Testar Alteração de Privacidade:** Editar um hábito privado, desmarcar a opção e salvar. Logar como amigo e confirmar que o hábito **agora aparece** no feed. Fazer o processo inverso e confirmar que ele some novamente.

---

## História 3: Troféus por Conquista

* **Como** usuário, **eu quero** marcar um hábito como "concluído", **para que** ele saia da minha lista diária e vire um troféu.

### 1. Planejamento (Planning)

#### Design de Interface (UI/UX)

* **Opção de Conclusão:** Adicionar uma opção `"Marcar como Concluído"` na tela de edição ou nos detalhes de um hábito.
* **Seção de Troféus:** Criar uma nova seção no perfil do usuário chamada `"Meus Troféus"` ou `"Conquistas"`.
* **Card de Troféu:** Projetar o card de "Troféu", que exibirá o nome do hábito, um ícone, a **maior sequência (streak)** alcançada e a **data de conclusão**.
* **Botão de Recomeço:** Incluir um botão `"Começar de Novo"` em cada troféu, que facilitará a criação de um hábito idêntico.

#### Arquitetura e Backend

* **Estrutura de Dados:**
    * Adicionar uma coluna `status` na tabela `Habits` (ex: 'active', 'completed').
    * Adicionar colunas `completed_at` (timestamp) e `final_streak` (integer) para armazenar o resumo.
* **API Endpoints:**
    * `POST /habits/{id}/complete`: Endpoint para marcar o hábito como concluído.
    * `GET /habits?status=active`: Modificar o endpoint existente para retornar apenas os hábitos ativos por padrão.
    * `GET /trophies` (ou `GET /habits?status=completed`): Novo endpoint para listar os hábitos concluídos (troféus).

### 2. Execução (Execution)

#### Desenvolvimento Frontend

1.  Implementar a opção `"Marcar como Concluído"` e a chamada à API correspondente.
2.  Filtrar a lista de hábitos diários para exibir apenas os que têm status `active`.
3.  Construir a tela `"Meus Troféus"`, que consome os dados do novo endpoint.
4.  Implementar a funcionalidade do botão `"Começar de Novo"`, que deve levar o usuário à tela de criação de hábito com os campos já preenchidos.

#### Desenvolvimento Backend

1.  Criar a lógica no endpoint `/complete` que altera o `status` do hábito, registra a data de conclusão e armazena a maior sequência alcançada.
2.  Garantir que as notificações e lógicas diárias ignorem os hábitos com status `completed`.
3.  Implementar o endpoint que retorna a lista de troféus para o frontend.

### 3. Revisão (Testes de Aceitação)

1.  **Testar Opção de Conclusão:** Acessar um hábito ativo e usar a opção `"Marcar como Concluído"`.
2.  **Verificar Remoção da Lista Diária:** Após marcar como concluído, voltar à lista de hábitos diários e confirmar que ele não está mais lá.
3.  **Confirmar Criação do Troféu:** Navegar até a seção `"Meus Troféus"` e confirmar que o hábito concluído está listado.
4.  **Validar Resumo do Desempenho:** Verificar se o troféu exibe corretamente a maior sequência (streak) que o hábito tinha e a data em que foi concluído.
5.  **Testar Recriação do Hábito:** Clicar no botão `"Começar de Novo"` em um troféu. Confirmar que a tela de criação é aberta com as informações do hábito antigo já preenchidas.