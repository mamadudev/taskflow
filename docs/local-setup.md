# TaskFlow - Ambiente de Desenvolvimento

## Pré-requisitos

- Java 21 LTS
- PostgreSQL 18
- Git
- IntelliJ IDEA ou outra IDE Java

## Tecnologias do backend

- Spring Boot 4.1.2
- Maven Wrapper
- Spring Data JPA
- Hibernate
- PostgreSQL

## Configuração do PostgreSQL

O TaskFlow utiliza PostgreSQL como banco de dados.

### Criar usuário e banco

Conecte-se ao PostgreSQL como administrador e execute:

```sql
CREATE ROLE taskflow_app WITH LOGIN;
```

No terminal `psql`, configure a senha:

```text
\password taskflow_app
```

Depois crie o banco:

```sql
CREATE DATABASE taskflow_db OWNER taskflow_app;
```

A configuração local utiliza a porta 5433.

## Variáveis de ambiente

No PowerShell:

```powershell
$env:DB_URL = "jdbc:postgresql://127.0.0.1:5433/taskflow_db"
$env:DB_USERNAME = "taskflow_app"

$securePassword = Read-Host "Senha do banco" -AsSecureString
$env:DB_PASSWORD = [System.Net.NetworkCredential]::new("", $securePassword).Password
```

Nunca registre senhas reais no GitHub.

## Executar o backend

Na raiz do repositório:

```powershell
cd backend
.\mvnw.cmd spring-boot:run
```

O backend inicia em:

http://localhost:8081

Como ainda não implementamos endpoints, receber HTTP 404 na rota principal é normal.

## Compilar o projeto

```powershell
.\mvnw.cmd clean compile
```

O resultado esperado é `BUILD SUCCESS`.

## Problemas comuns

- **Java incorreto:** confirme a versão 21 usando `java -version`.
- **Erro de conexão:** verifique se o PostgreSQL está ativo na porta configurada.
- **Erro de autenticação:** confira `DB_USERNAME` e `DB_PASSWORD`.
- **Porta HTTP ocupada:** configure `SERVER_PORT` para outra porta disponível.

