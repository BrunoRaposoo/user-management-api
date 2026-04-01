# Módulo Auth - Documentação Arquitetural

## Responsabilidade do Módulo

O módulo `auth` é responsável por toda a lógica de autenticação e autorização da aplicação. Este módulo implementa o sistema de autenticação utilizando JWT (JSON Web Token) para proteger as rotas da API.

## Componentes

### Estratégias

- **JwtStrategy**: Estratégia de autenticação JWT
  - Extende `PassportStrategy(Strategy)` do passport-jwt
  - Extrai o token do header Authorization (Bearer token)
  - Valida o token usando a chave secreta do .env
  - Retorna o payload decodificado (userId, email)

### Guards

- **JwtAuthGuard**: Guard principal para proteção de rotas
  - Estende `AuthGuard('jwt')` do NestJS
  - Verifica se o token JWT é válido
  - Bloqueia acesso a rotas não autenticadas
  - Pode ser aplicado em nível de controller ou método

- **JwtRefreshGuard**: Guard para refresh tokens (futuro)
  - Permite renew de tokens expirados

### Service

- **AuthService**: Lógica de autenticação
  - `validateUser()`: Valida credenciais do usuário
  - `login()`: Gera token JWT para o usuário
  - `register()`: Cria novo usuário e gera token

### DTOs

- **LoginDto**: Dados para login
  - `email`: Email do usuário
  - `password`: Senha do usuário

- **AuthResponseDto**: Resposta de autenticação
  - `accessToken`: JWT token
  - `user`: Dados do usuário (sem senha)

## Fluxo de Autenticação

1. **Registro**: Usuário envia dados → Service cria usuário → Retorna token
2. **Login**: Usuário envia credenciais → Service valida → Retorna token
3. **Acesso Protegido**: Cliente envia token → Guard valida → Permite acesso

## Configurações

### Variáveis de Ambiente
- `JWT_SECRET`: Chave secreta para assinatura dos tokens
- `JWT_EXPIRES_IN`: Tempo de expiração do token (ex: '1d', '7d')

### Configuração do JwtModule
- Registrar com `JwtModule.registerAsync()`
- Injetar `ConfigService` para ler variáveis de ambiente
- Definir secret e expiration time

## Padrões a Seguir

### Segurança
- Armazenar senha hashada com bcrypt (cost factor: 10)
- Validar credenciais antes de gerar token
- Nunca expor senha em respostas
- Tokens com expiração definida

### Headers HTTP
- Token enviado via: `Authorization: Bearer <token>`
- Resposta de erro 401 para token inválido/expirado

### Boas Práticas
- Implementar rate limiting para login (futuro)
- Registrar tentativas de login falhas
- Usar HTTPS em produção
- Rotacionar JWT_SECRET periodicamente
