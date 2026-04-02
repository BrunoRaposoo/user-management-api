# Módulo User - Documentação Arquitetural

## Responsabilidade do Módulo

O módulo `user` é responsável por todas as operações relacionadas ao gerenciamento de usuários na plataforma. Este módulo implementa as funcionalidades de CRUD (Create, Read, Update, Delete) para manipulação de dados de usuários.

## Componentes

### Entidade

- **User Entity**: Define a estrutura da tabela de usuários no banco de dados MySQL
  - `id`: Identificador único (auto-increment)
  - `name`: Nome completo do usuário (até 100 caracteres)
  - `username`: Nome de usuário único (até 50 caracteres) - usado para login
  - `email`: Email único do usuário (até 100 caracteres)
  - `password`: Senha hashada (nunca armazenar em texto claro)
  - `createdAt`: Data de criação (gerado automaticamente pelo TypeORM)
  - `updatedAt`: Data de atualização (gerado automaticamente pelo TypeORM)

#### Regras de Negócio
- `username` e `email` devem ser únicos no banco
- `password` deve ser hashada com bcrypt antes de armazenar
- Não exponha o campo `password` nas respostas da API

### DTOs (Data Transfer Objects)

- **CreateUserDto**: Dados necessários para criar um usuário
  - `email`: Email válido e único
  - `name`: Nome do usuário
  - `password`: Senha com validação de força

- **UpdateUserDto**: Dados para atualização de usuário
  - `email`: (opicional) Novo email
  - `name`: (opcional) Novo nome
  - `password`: (opcional) Nova senha

- **UserResponseDto**: Resposta retornada aos clientes
  - Exclui campos sensíveis como `password`

### Service

- **UserService**: Lógica de negócio para operações de usuário
  - `create()`: Criar novo usuário
  - `findAll()`: Listar todos os usuários
  - `findOne()`: Buscar usuário por ID
  - `update()`: Atualizar usuário
  - `remove()`: Remover usuário
  - `findByEmail()`: Buscar usuário por email (utilizado internamente)

### Controller

- **UserController**: Endpoints REST para manipulação de usuários
  - `POST /api/v1/users`: Criar usuário
  - `GET /api/v1/users`: Listar usuários
  - `GET /api/v1/users/:id`: Buscar usuário específico
  - `PATCH /api/v1/users/:id`: Atualizar usuário
  - `DELETE /api/v1/users/:id`: Remover usuário

## Padrões a Seguir

### Validação
- Utilizar `class-validator` e `class-transformer` nos DTOs
- Validar email único antes de criar
- Validar força da senha (mínimo 8 caracteres)

### Segurança
- Nunca retornar a senha nas respostas
- Utilizar hash bcrypt para senhas
- Proteger rotas sensíveis com AuthGuard (JWT)

### Tratamento de Erros
- Utilizar exception filters do NestJS
- Retornar HTTP status codes apropriados
- Mensagens de erro claras e em português

### Banco de Dados
- Usar TypeORM com padrão Repository
- Soft delete para manter histórico (opcional)
- Timestamps automáticos (createdAt, updatedAt)
