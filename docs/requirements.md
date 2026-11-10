# TaskFlow | Documento de Requisitos

**Versão:** 1.0  
**Status:** Escopo inicial aprovado  
**Projeto:** TaskFlow  
**Tipo:** Aplicação web Full Stack

## 1. Visão geral

O TaskFlow é uma aplicação web responsiva destinada a usuários individuais que desejam organizar e acompanhar suas tarefas pessoais.

A solução permitirá cadastrar tarefas, definir prioridades e prazos, organizar atividades por categorias e acompanhar seu progresso.

## 2. Problema de negócio

Pessoas que gerenciam diversas responsabilidades frequentemente enfrentam dificuldades para centralizar suas tarefas, identificar prioridades e acompanhar prazos.

O TaskFlow busca reduzir essa desorganização oferecendo uma ferramenta simples, acessível e segura.

## 3. Público-alvo

Usuários individuais que precisam organizar atividades pessoais, acadêmicas ou profissionais.

O MVP não terá funcionalidades de colaboração entre equipes.

## 4. Escopo do MVP

### 4.1 Requisitos funcionais

| Código | Descrição |
|---|---|
| RF-001 | O sistema deve permitir o cadastro de usuários. |
| RF-002 | O sistema deve permitir a autenticação de usuários cadastrados. |
| RF-003 | O usuário autenticado deve poder criar tarefas. |
| RF-004 | O usuário deve poder consultar suas próprias tarefas. |
| RF-005 | O usuário deve poder editar suas próprias tarefas. |
| RF-006 | O usuário deve poder excluir suas próprias tarefas. |
| RF-007 | O usuário deve poder alterar o status de suas tarefas. |
| RF-008 | O usuário deve poder definir prioridades para suas tarefas. |
| RF-009 | O usuário deve poder definir prazos para suas tarefas. |
| RF-010 | O usuário deve poder criar, consultar, editar e excluir suas categorias, respeitando as regras de negócio. |
| RF-011 | O usuário deve poder filtrar tarefas por status, prioridade e categoria. |
| RF-012 | O usuário deve poder encerrar sua sessão. |

### 4.2 Requisitos não funcionais

| Código | Descrição |
|---|---|
| RNF-001 | O backend deve ser desenvolvido com Java e Spring Boot. |
| RNF-002 | Os dados persistentes devem ser armazenados em PostgreSQL. |
| RNF-003 | A interface deve ser responsiva e desenvolvida com React. |
| RNF-004 | A comunicação entre frontend e backend deve utilizar uma API REST com JSON. |
| RNF-005 | As senhas devem ser armazenadas utilizando hash seguro, nunca em texto puro. |
| RNF-006 | O sistema deve impedir que um usuário acesse ou modifique dados pertencentes a outro usuário. |
| RNF-007 | Os dados recebidos pela API devem ser validados. |
| RNF-008 | A API deve utilizar códigos HTTP adequados e respostas de erro padronizadas. |
| RNF-009 | O projeto deve possuir documentação para instalação e execução local. |
| RNF-010 | As principais regras de negócio devem ser cobertas por testes automatizados. |

## 5. Fora do escopo inicial

As seguintes funcionalidades não fazem parte do MVP:

- Compartilhamento de tarefas entre usuários.
- Gestão de equipes.
- Notificações por e-mail.
- Aplicativo mobile nativo.
- Integrações com calendários externos.
- Dashboard analítico avançado.
- Assinaturas e pagamentos.

## 6. Modelo de domínio preliminar

**User:** representa o usuário cadastrado.

**Task:** representa uma tarefa pertencente a um usuário.

**Category:** representa uma categoria criada por um usuário para organizar tarefas.

Os atributos e relacionamentos serão detalhados na etapa de modelagem.

## 7. Regras de negócio pendentes

As seguintes decisões serão resolvidas na Issue #2:

- Reabertura de tarefas concluídas.
- Obrigatoriedade de categoria e prazo.
- Permissão de prazos anteriores à data atual.
- Exclusão de categorias associadas a tarefas.
- Unicidade dos títulos das tarefas.
- Exclusão de tarefas concluídas.

## 8. Critérios de conclusão da Issue #1

- [x] Público-alvo definido.
- [x] Plataforma definida.
- [x] Escopo inicial do MVP definido.
- [x] Requisitos funcionais especificados.
- [x] Requisitos não funcionais especificados.
- [x] Funcionalidades fora do escopo identificadas.
- [x] Documento versionado no repositório.
- [x] Issue revisada e encerrada.
