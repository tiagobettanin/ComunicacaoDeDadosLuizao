# Documento de Revisão de Requisitos

A seguir está a documentação da inspeção de requisitos para cada História de Usuário, baseada no modelo de revisão fornecido.

---

### Documentação HU1 - Tiago
| Item Revisado | Observação | Ação Sugerida |
| :--- | :--- | :--- |
| **Criador do Desafio** | Não está definido o que acontece se o criador do desafio sair do grupo ou excluir a conta. | Definir regra de negócio: ou o desafio é encerrado, ou a "propriedade" do desafio é transferida para o próximo participante mais antigo. |
| **Atualização do Progresso**| Não está claro quando ou como o progresso de um desafio é atualizado. | Especificar que o progresso será atualizado assim que um usuário registrar uma atividade relevante para o desafio. |

### Documentação HU2 - Tiagos
| Item Revisado | Observação | Ação Sugerida |
| :--- | :--- | :--- |
| **Privatização na Criação**| O requisito de criar um hábito já como privado não estava explícito. | Adicionar um controle (checkbox/switch) na tela de **criação** do hábito para que ele já possa nascer privado. |
| **Ícone de Privacidade**| O critério menciona o ícone na lista pessoal, mas não em outras telas onde o hábito possa aparecer para o dono. | Clarificar que o indicador de privacidade (cadeado) deve estar visível para o dono em **todas** as interfaces onde o hábito for exibido. |

### Documentação HU3 - Tiago
| Item Revisado | Observação | Ação Sugerida |
| :--- | :--- | :--- |
| **Ação de "Concluir"** | A ação de marcar um hábito como concluído parece ser permanente, sem uma forma de reverter um erro. | Adicionar um requisito para uma funcionalidade de **"Reativar Hábito"** a partir da tela de troféus, movendo-o de volta para a lista ativa. |
| **Recriação do Hábito** | A frase "criar uma sequência igual" pode ser ambígua, sugerindo que o progresso seria copiado. | Refinar o texto do requisito para: "O usuário poderá criar um **novo hábito** com as mesmas configurações (nome, categoria, etc.) a partir de um troféu." |