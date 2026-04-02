# User Management API

API RESTful para gerenciamento de usuários com autenticação JWT, desenvolvida com NestJS.

## 📋 Descrição

Esta API permite operações completas de CRUD (Create, Read, Update, Delete) para gerenciamento de usuários, com sistema de autenticação seguro utilizando JSON Web Token (JWT).

## 🛠️ Tecnologias

- **Framework**: NestJS (Node.js)
- **Banco de Dados**: MySQL 8.0
- **ORM**: TypeORM
- **Autenticação**: JWT (JSON Web Token)
- **Validação**: class-validator + class-transformer
- **Criptografia**: bcrypt

## 🚀 Começando

### Pré-requisitos

- Node.js (v18+)
- MySQL 8.0
- npm ou yarn

### Instalação

```bash
# Clonar o repositório
cd user-management-api

# Instalar dependências
npm install

# Configurar variáveis de ambiente
# Edite o arquivo .env com suas configurações
```

### Configuração do Banco de Dados

Crie o banco de dados MySQL:

```sql
CREATE DATABASE user_db;
```

Configure o arquivo `.env`:

```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=
DB_PASSWORD=
DB_NAME=
JWT_SECRET=mysecretkey
JWT_EXPIRES_IN=1d
```

### Executando a Aplicação

```bash
# Desenvolvimento
npm run start:dev

# Produção
npm run start:prod
```

A API estará disponível em: `http://localhost:3000/api/v1`

## 📚 Documentação de Rotas

### Autenticação

| Método | Rota | Descrição | Pública |
|--------|------|-----------|---------|
| POST | `/auth/login` | Autentica usuário e retorna token JWT | ✅ |

### Usuários

| Método | Rota | Descrição | Proteção |
|--------|------|-----------|----------|
| POST | `/users` | Criar novo usuário | ✅ Pública |
| GET | `/users` | Listar todos os usuários | 🔒 JWT |
| GET | `/users/:id` | Buscar usuário por ID | ✅ Pública |
| PATCH | `/users/:id` | Atualizar usuário | 🔒 JWT |
| DELETE | `/users/:id` | Remover usuário | 🔒 JWT |

## 📝 Exemplos de Requisições

### 1. Criar Usuário (Público)

```bash
curl -X POST http://localhost:3000/api/v1/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "João Silva",
    "username": "joao",
    "email": "joao@exemplo.com",
    "password": "senha123"
  }'
```

**Resposta:**
```json
{
  "id": 1,
  "name": "João Silva",
  "username": "joao",
  "email": "joao@exemplo.com",
  "createdAt": "2026-04-01T20:00:00.000Z",
  "updatedAt": "2026-04-01T20:00:00.000Z"
}
```

### 2. Login (Público)

```bash
curl -X POST http://localhost:3000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "joao",
    "password": "senha123"
  }'
```

**Resposta:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### 3. Listar Usuários (Protegido)

```bash
curl -X GET http://localhost:3000/api/v1/users \
  -H "Authorization: Bearer SEU_TOKEN_JWT_AQUI"
```

### 4. Buscar Usuário por ID (Público)

```bash
curl -X GET http://localhost:3000/api/v1/users/1
```

### 5. Atualizar Usuário (Protegido)

```bash
curl -X PATCH http://localhost:3000/api/v1/users/1 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer SEU_TOKEN_JWT_AQUI" \
  -d '{
    "name": "João Atualizado"
  }'
```

### 6. Deletar Usuário (Protegido)

```bash
curl -X DELETE http://localhost:3000/api/v1/users/1 \
  -H "Authorization: Bearer SEU_TOKEN_JWT_AQUI"
```

## 🔐 Autenticação JWT

### Header de Requisição

Para rotas protegidas, inclua o token JWT no header:

```
Authorization: Bearer <seu_token_jwt>
```

### Código de Resposta de Erros

- `200 OK` - Requisição bem-sucedida
- `201 Created` - Recurso criado com sucesso
- `400 Bad Request` - Dados inválidos
- `401 Unauthorized` - Token inválido ou não fornecido
- `404 Not Found` - Recurso não encontrado
- `500 Internal Server Error` - Erro interno do servidor

## 📁 Estrutura do Projeto

```
src/
├── main.ts                    # Entry point
├── app.module.ts              # Módulo raiz
├── user/
│   ├── user.entity.ts         # Entidade User
│   ├── user.module.ts         # Módulo de usuários
│   ├── user.service.ts        # Lógica de negócio
│   ├── user.controller.ts    # Controlador REST
│   └── dto/
│       ├── create-user.dto.ts
│       ├── update-user.dto.ts
│       └── response-user.dto.ts
└── auth/
    ├── auth.module.ts         # Módulo de autenticação
    ├── auth.service.ts       # Lógica de autenticação
    ├── auth.controller.ts    # Controlador de login
    ├── jwt.strategy.ts       # Estratégia JWT
    └── dto/
        └── login.dto.ts
```

## ⚙️ Configurações

### Validação de Dados

A API utiliza `class-validator` para validar entrada de dados:

- **Nome**: Mínimo 3 caracteres
- **Username**: Mínimo 3, máximo 50 caracteres
- **Email**: Formato válido de email
- **Password**: Mínimo 8 caracteres

### Segurança

- Senhas são hasheadas com bcrypt (salt rounds = 10)
- Tokens JWT expiram em 1 dia
- Rotas sensíveis protegidas com AuthGuard
- ValidationPipe global com whitelist

## ⚠️ Notas de Produção

1. **TypeORM synchronize**: Não use `synchronize: true` em produção. Use migrations.
2. **JWT_SECRET**: Altere a chave secreta em produção.
3. **HTTPS**: Use HTTPS em ambientes de produção.
4. **Rate Limiting**: Considere implementar rate limiting para endpoints de login.

## 📄 Licença

MIT License
