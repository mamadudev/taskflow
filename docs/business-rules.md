# TaskFlow | Regras de Negócio

**Versão:** 1.0  
**Status:** Aprovado para o MVP  
**Projeto:** TaskFlow  
**Issue:** #2  
**Data:** 10/11/2026

## 1. Objetivo

Este documento estabelece as regras de negócio do TaskFlow, uma aplicação web responsiva para gerenciamento individual de tarefas.

As regras definem o comportamento esperado do sistema e servirão como referência para a implementação do backend, validações da API e testes automatizados.

## 2. Conceitos do domínio

- **Usuário:** pessoa autenticada que gerencia suas próprias tarefas e categorias.
- **Tarefa:** atividade cadastrada por um usuário.
- **Categoria:** agrupamento opcional de tarefas.
- **Status:** estado atual de uma tarefa.
- **Prioridade:** nível de importância de uma tarefa.
- **Prazo:** data prevista para conclusão de uma tarefa.

### 2.1 Estados de uma tarefa

| Estado | Identificador | Significado |
|---|---|---|
| Pendente | `PENDING` | Tarefa ainda não iniciada |
| Em andamento | `IN_PROGRESS` | Tarefa em execução |
| Concluída | `COMPLETED` | Tarefa finalizada |

Uma tarefa será criada inicialmente com status `PENDING`.

## 3. Regras de negócio aprovadas

### RN-001 | Reabertura de tarefas concluídas

**Descrição:** Uma tarefa com status `COMPLETED` poderá ser reaberta pelo usuário responsável.

**Comportamento esperado:**
- O usuário poderá alterar o status de `COMPLETED` para `PENDING`.
- O usuário poderá alterar o status de `COMPLETED` para `IN_PROGRESS`.
- A tarefa manterá suas demais informações, salvo alterações explícitas do usuário.
- A mudança deverá ser persistida no banco de dados.

**Critérios de aceitação:**
- [ ] Uma tarefa concluída pode voltar para Pendente.
- [ ] Uma tarefa concluída pode voltar para Em andamento.
- [ ] O novo status é persistido corretamente.
- [ ] Outro usuário não pode alterar seu status.

### RN-002 | Categoria e prazo opcionais

**Descrição:** O preenchimento de categoria e prazo não será obrigatório na criação ou edição de tarefas.

**Comportamento esperado:**
- Uma tarefa poderá ser criada sem categoria.
- Uma tarefa poderá ser criada sem prazo.
- Ambos os campos poderão permanecer vazios simultaneamente.
- O usuário poderá adicionar ou remover categoria e prazo posteriormente.

**Critérios de aceitação:**
- [ ] Uma tarefa sem categoria é aceita.
- [ ] Uma tarefa sem prazo é aceita.
- [ ] Uma tarefa sem ambos os campos é aceita.
- [ ] Os campos opcionais podem ser atualizados posteriormente.

### RN-003 | Prazo anterior à data atual

**Descrição:** O sistema permitirá cadastrar tarefas com prazos anteriores à data atual.

**Comportamento esperado:**
- A API não rejeitará uma tarefa apenas porque seu prazo está no passado.
- Uma tarefa será considerada atrasada quando possuir prazo anterior à data atual e não estiver concluída.
- Tarefas concluídas não serão classificadas como atrasadas.
- Tarefas sem prazo não serão classificadas como atrasadas.
- O indicador de atraso será atualizado conforme a data atual e o status da tarefa.

**Critérios de aceitação:**
- [ ] É possível criar tarefas com prazo passado.
- [ ] Tarefas pendentes com prazo passado aparecem como atrasadas.
- [ ] Tarefas em andamento com prazo passado aparecem como atrasadas.
- [ ] Tarefas concluídas não aparecem como atrasadas.
- [ ] Tarefas sem prazo não aparecem como atrasadas.

**Observação técnica:** O prazo será representado inicialmente por uma data, sem horário. Uma tarefa com vencimento na data atual ainda não será considerada atrasada.

### RN-004 | Exclusão de categorias com tarefas associadas

**Descrição:** A exclusão de uma categoria não deverá excluir suas tarefas.

**Comportamento esperado:**
- O usuário poderá excluir uma categoria de sua propriedade.
- As tarefas vinculadas à categoria permanecerão cadastradas.
- A referência à categoria excluída será removida dessas tarefas.
- A operação deverá ser realizada de forma transacional.
- A exclusão de uma categoria pertencente a outro usuário deverá ser impedida.

**Critérios de aceitação:**
- [ ] Uma categoria sem tarefas pode ser excluída.
- [ ] Uma categoria com tarefas pode ser excluída.
- [ ] As tarefas associadas continuam existindo.
- [ ] As tarefas afetadas ficam sem categoria.
- [ ] O usuário não consegue excluir categorias de terceiros.

### RN-005 | Títulos duplicados

**Descrição:** Um mesmo usuário poderá cadastrar diferentes tarefas com títulos idênticos.

**Comportamento esperado:**
- O sistema não exigirá unicidade para títulos de tarefas.
- Cada tarefa terá um identificador único.
- Tarefas com o mesmo título serão tratadas como registros independentes.
- Operações de consulta, edição e exclusão utilizarão o identificador da tarefa.

**Critérios de aceitação:**
- [ ] Um usuário pode criar duas tarefas com o mesmo título.
- [ ] As tarefas possuem identificadores diferentes.
- [ ] A alteração de uma tarefa não modifica outra com o mesmo título.
- [ ] A exclusão de uma tarefa não afeta outra com o mesmo título.

### RN-006 | Exclusão de tarefas concluídas

**Descrição:** O usuário poderá excluir tarefas de sua propriedade, independentemente do status.

**Comportamento esperado:**
- Tarefas pendentes poderão ser excluídas.
- Tarefas em andamento poderão ser excluídas.
- Tarefas concluídas poderão ser excluídas.
- A exclusão removerá o registro da tarefa do armazenamento persistente.
- A operação deverá exigir autenticação e verificação de propriedade.

**Critérios de aceitação:**
- [ ] É possível excluir tarefas pendentes.
- [ ] É possível excluir tarefas em andamento.
- [ ] É possível excluir tarefas concluídas.
- [ ] Uma tarefa excluída não aparece nas consultas posteriores.
- [ ] Um usuário não pode excluir tarefas pertencentes a outro usuário.

## 4. Regras gerais complementares

Estas regras complementam os requisitos de segurança e integridade do MVP.

### RG-001 | Isolamento de dados

Cada usuário poderá consultar e modificar somente suas próprias tarefas e categorias.

### RG-002 | Autenticação

As operações de gerenciamento de tarefas e categorias exigirão autenticação.

### RG-003 | Identificação de registros

Usuários, tarefas e categorias possuirão identificadores únicos, independentes de nomes ou títulos.

### RG-004 | Integridade dos relacionamentos

Uma tarefa poderá ser associada somente a uma categoria pertencente ao mesmo usuário.

### RG-005 | Validação de campos obrigatórios

O título da tarefa será obrigatório e não poderá conter somente espaços em branco.

### RG-006 | Prioridade padrão

Quando nenhuma prioridade for informada, a tarefa receberá `MEDIUM`. As prioridades disponíveis serão `LOW`, `MEDIUM` e `HIGH`.

## 5. Matriz de rastreabilidade

| Regra | Requisito relacionado |
|---|---|
| RN-001 | RF-005, RF-007 |
| RN-002 | RF-003, RF-005, RF-009, RF-010 |
| RN-003 | RF-003, RF-009 |
| RN-004 | RF-010 |
| RN-005 | RF-003, RF-005, RF-006 |
| RN-006 | RF-006 |
| RG-001 a RG-004 | RNF-005, RNF-006 |
| RG-005 e RG-006 | RF-003, RNF-007 |

## 6. Estratégia de validação

As regras serão verificadas durante o desenvolvimento por meio de:

- Testes unitários dos serviços com JUnit e Mockito.
- Testes de integração com Spring Boot e PostgreSQL.
- Testes de autorização e isolamento entre usuários.
- Testes dos endpoints da API REST.
- Validação dos fluxos principais no frontend.

## 7. Critérios de conclusão da Issue #2

- [x] Regras de negócio discutidas.
- [x] Seis decisões principais aprovadas.
- [x] Comportamentos esperados especificados.
- [x] Critérios de aceitação definidos.
- [x] Relação com os requisitos documentada.
- [x] Documento versionado no GitHub.
- [x] Issue revisada e encerrada.
