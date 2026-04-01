# user-management-api - Arquitetura

## Visão Geral

Este projeto é uma API RESTful desenvolvida com **NestJS** para gerenciamento de usuários com autenticação JWT.

## Decisões Arquiteturais

### Stack Tecnológico
- **Framework**: NestJS (Node.js)
- **Banco de Dados**: MySQL 8.0
- **ORM**: TypeORM
- **Autenticação**: JWT (JSON Web Token) via @nestjs/jwt
- **Validação**: class-validator + class-transformer

### Estrutura de Módulos
O projeto segue a arquitetura de módulos do NestJS com injeção de dependências:

```
src/
├── app.module.ts          # Módulo raiz
├── main.ts               # Entry point
├── user/                 # Módulo de usuários (CRUD)
│   ├── entities/         # Entidade User
│   ├── dto/              # Data Transfer Objects
│   ├── user.service.ts   # Lógica de negócio
│   └── user.controller.ts # Endpoints REST
└── auth/                  # Módulo de autenticação
    ├── strategies/       # Estratégias JWT
    ├── guards/           # Guards de proteção
    └── auth.service.ts   # Lógica de autenticação
```

### Configuração
- **ConfigModule**: Global via `@nestjs/config` com variáveis de ambiente (.env)
- **TypeORM**: Configuração assíncrona usando `ConfigService`
- **Prefixo da API**: `api/v1`

### TypeORM
- `synchronize: true` - **APENAS PARA DESENVOLVIMENTO**
- Em **produção**: usar migrations (desabilitar synchronize)
- Entidades carregadas automaticamente (`autoLoadEntities: true`)
- Entidade: User (src/user/user.entity.ts)

### Padrões de Código
- Injeção de dependências em todos os serviços
- DTOs para validação de entrada
- Guards para proteção de rotas
- Entidades TypeORM para mapeamento objeto-relacional
- Padrão Repository para acesso aos dados
