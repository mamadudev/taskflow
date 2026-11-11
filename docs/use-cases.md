# TaskFlow | Especificação de Casos de Uso

**Versão:** 1.0  
**Status:** Proposta para revisão  
**Issue:** #3  
**Projeto:** TaskFlow

## 1. Objetivo

Documentar as principais interações entre os usuários e o TaskFlow, estabelecendo fluxos principais, fluxos alternativos e resultados esperados.

## 2. Atores

**Visitante:** pessoa que ainda não está autenticada e pode cadastrar uma conta ou realizar login.

**Usuário autenticado:** pessoa que possui uma sessão válida e pode gerenciar suas próprias tarefas e categorias.

## 3. Catálogo de casos de uso

| Código | Caso de uso | Ator |
|---|---|---|
| UC-001 | Cadastrar usuário | Visitante |
| UC-002 | Autenticar usuário | Visitante |
| UC-003 | Criar tarefa | Usuário autenticado |
| UC-004 | Listar e consultar tarefas | Usuário autenticado |
| UC-005 | Atualizar tarefa | Usuário autenticado |
| UC-006 | Excluir tarefa | Usuário autenticado |
| UC-007 | Gerenciar categorias | Usuário autenticado |
| UC-008 | Filtrar tarefas | Usuário autenticado |
| UC-009 | Encerrar sessão | Usuário autenticado |

## 4. Especificações detalhadas

### UC-001 | Cadastrar usuário

**Pré-condição:** O visitante não precisa estar autenticado.

**Fluxo principal:**
1. O visitante acessa a página de cadastro.
2. Informa os dados solicitados.
3. O sistema valida as informações.
4. O sistema verifica se o e-mail já está cadastrado.
5. O sistema protege a senha utilizando hash seguro.
6. O sistema registra o novo usuário.
7. O sistema confirma o cadastro.

**Fluxos alternativos:**
- Dados inválidos: o sistema apresenta os erros de validação.
- E-mail já cadastrado: o sistema rejeita o cadastro.

**Pós-condição:** Uma conta válida é criada.

### UC-002 | Autenticar usuário

**Pré-condição:** O usuário possui uma conta cadastrada.

**Fluxo principal:**
1. O visitante informa e-mail e senha.
2. O sistema valida as credenciais.
3. O sistema estabelece a autenticação.
4. O usuário obtém acesso às funcionalidades protegidas.

**Fluxo alternativo:**
- Credenciais inválidas: o sistema rejeita o acesso sem revelar qual informação está incorreta.

**Pós-condição:** O usuário está autenticado.

### UC-003 | Criar tarefa

**Pré-condição:** O usuário está autenticado.

**Fluxo principal:**
1. O usuário solicita a criação de uma tarefa.
2. Informa o título.
3. Opcionalmente, informa descrição, categoria, prioridade e prazo.
4. O sistema valida os dados e a propriedade da categoria, quando informada.
5. O sistema associa a tarefa ao usuário autenticado.
6. O sistema define o status inicial como `PENDING`.
7. O sistema persiste a tarefa.
8. O sistema confirma a criação.

**Fluxos alternativos:**
- Título vazio: o sistema rejeita a solicitação.
- Categoria inexistente ou não pertencente ao usuário: o sistema rejeita a associação.
- Prazo passado: o sistema permite o cadastro e identifica atraso quando aplicável.

**Pós-condição:** A tarefa está cadastrada.

**Regras relacionadas:** RN-002, RN-003, RN-005, RG-001, RG-004, RG-005 e RG-006.

### UC-004 | Listar e consultar tarefas

**Pré-condição:** Usuário autenticado.

**Fluxo principal:**
1. O usuário solicita suas tarefas.
2. O sistema consulta apenas os registros pertencentes a ele.
3. O sistema apresenta os resultados.

**Fluxos alternativos:**
- Nenhuma tarefa encontrada: o sistema retorna uma lista vazia.
- Consulta de tarefa de outro usuário: o acesso é negado sem expor informações privadas.

**Pós-condição:** Nenhuma informação é modificada.

### UC-005 | Atualizar tarefa

**Pré-condição:** Usuário autenticado e tarefa existente de sua propriedade.

**Fluxo principal:**
1. O usuário seleciona uma tarefa.
2. Altera os campos desejados.
3. O sistema valida as alterações.
4. O sistema verifica a propriedade da tarefa e da categoria informada.
5. O sistema persiste as alterações.
6. O sistema confirma a atualização.

**Fluxos alternativos:**
- Tarefa inexistente ou inacessível: a operação não é realizada.
- Dados inválidos: o sistema rejeita a atualização.
- Tarefa concluída: o usuário pode reabri-la para `PENDING` ou `IN_PROGRESS`.

**Pós-condição:** A tarefa reflete os dados atualizados.

**Regra relacionada:** RN-001.

### UC-006 | Excluir tarefa

**Pré-condição:** Usuário autenticado e proprietário da tarefa.

**Fluxo principal:**
1. O usuário solicita a exclusão.
2. O sistema verifica a propriedade.
3. O sistema exclui a tarefa.
4. O sistema confirma a operação.

**Fluxos alternativos:**
- Tarefa inexistente ou inacessível: nenhuma exclusão é realizada.
- Tarefa concluída: a exclusão continua permitida.

**Pós-condição:** A tarefa deixa de existir no armazenamento persistente.

**Regra relacionada:** RN-006.

### UC-007 | Gerenciar categorias

**Pré-condição:** Usuário autenticado.

**Fluxo principal:**
1. O usuário acessa suas categorias.
2. Solicita a criação, consulta, edição ou exclusão de uma categoria.
3. O sistema valida os dados e a propriedade.
4. O sistema executa a operação.
5. O sistema confirma o resultado.

**Fluxos alternativos:**
- Categoria pertencente a outro usuário: a operação é impedida.
- Exclusão de categoria com tarefas: as tarefas permanecem e ficam sem categoria.

**Pós-condição:** As categorias são atualizadas sem violar a integridade das tarefas.

**Regra relacionada:** RN-004.

### UC-008 | Filtrar tarefas

**Pré-condição:** Usuário autenticado.

**Fluxo principal:**
1. O usuário seleciona filtros por status, prioridade ou categoria.
2. O sistema aplica os filtros apenas às tarefas do usuário.
3. O sistema retorna os resultados correspondentes.

**Fluxo alternativo:**
- Nenhuma correspondência: o sistema retorna uma lista vazia.

**Pós-condição:** Nenhum dado é alterado.

### UC-009 | Encerrar sessão

**Pré-condição:** Usuário autenticado.

**Fluxo principal:**
1. O usuário solicita a saída do sistema.
2. O frontend descarta as credenciais locais de autenticação.
3. O sistema retorna à área pública.

**Pós-condição:** A sessão local é encerrada.

**Observação:** A invalidação imediata de tokens já emitidos dependerá da estratégia de autenticação definida na etapa de segurança.

## 5. Critérios de conclusão da Issue #3

- [x] Atores identificados.
- [x] Catálogo de casos de uso definido.
- [x] Fluxos principais descritos.
- [x] Fluxos alternativos descritos.
- [x] Pré-condições e pós-condições documentadas.
- [ ] Diagrama UML final versionado.
- [ ] Documento revisado e publicado no GitHub.
