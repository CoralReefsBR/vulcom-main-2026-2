# Vulcom — configuração em GitHub Codespaces

## 1) Fazendo fork e abrindo o codespace

1. Faça login no [GitHub](https://github.com).
2. Acesse o repositório do professor: `https://github.com/fcintra5/vulcom-main-2026-2`.
3. Clique em `Fork` no canto superior direito.
4. Na página de criação do fork, mantenha os valores padrão e clique em `Create fork`.
5. Confirme que a URL do seu fork ficou no formato `https://github.com/<SEU_USUARIO>/vulcom-main-2026-2`.
6. Clique em `Code` e, em seguida, na aba `Codespaces`.
7. Clique em `Create codespace on main` para abrir o ambiente.
8. Aguarde o ambiente ser provisionado. Na primeira vez, o VS Code _online_ será aberto no navegador.

> Um GitHub Codespace é um ambiente de desenvolvimento na nuvem. Ele já traz editor, ferramentas e dependências do projeto, reduzindo a instalação local.

## 2) Configurando o back-end

### 2.1 Arquivo de ambiente

No repositório, copie o arquivo `back-end/.env.example` para `back-end/.env`.

Conteúdo sugerido:

```ini
# Renomeie este arquivo para .env e preencha os valores abaixo

# Gere uma chave de token em https://jwtsecrets.com/
# (O token secret é simplesmente uma string aleatória)
TOKEN_SECRET=""

# Nome do cookie de autenticação
AUTH_COOKIE_NAME="_auth"

# URLs do front-end autorizadas a consumir a API, separadas por vírgulas
# Em um codespace, use o endereço HTTPS da porta 5173 do front-end.
ALLOWED_ORIGINS=""
```

> Importante: gere um valor seguro para `TOKEN_SECRET` antes de iniciar a aplicação.

### 2.2 Instalação das dependências

No terminal do VS Code, execute:

```bash
cd back-end
npm install
```

### 2.3 Banco de dados e registros iniciais

Ainda dentro da pasta `back-end`, execute estas etapas na primeira configuração:

```bash
npx prisma generate
npx prisma migrate dev --name create-tables
```

### 2.4 Executando o back-end

```bash
cd back-end
npm run dev
```

A API ficará disponível na porta `8888` no Codespace.

## 3) Configurando o front-end

### 3.1 Arquivo de ambiente

Copie o arquivo `front-end/.env.local.example` para `front-end/.env.local`.

No desenvolvimento, deixe `VITE_API_BASE` vazio. O servidor Vite encaminha as
chamadas da API para `http://127.0.0.1:8888`, evitando dependência de CORS e de
uma URL pública para o back-end. Mantenha o back-end em execução na porta 8888.

```ini
VITE_API_BASE=""
```

Se o front-end for servido fora do modo de desenvolvimento (por exemplo,
produção), configure `VITE_API_BASE` com a URL da API. Nesse caso,
`ALLOWED_ORIGINS` no back-end deve conter a origem exata do front-end.

### 3.2 Instalação das dependências

Abra um segundo terminal no VS Code e execute:

```bash
cd front-end
npm install
```

Se o npm exigir aprovação de scripts, verifique primeiro com:

```bash
node --version
npm --version
npm install-scripts ls
```

Somente aprove os scripts pendentes se a sua versão do npm oferecer esse suporte e houver necessidade real.

### 3.3 Executando o front-end

```bash
cd front-end
npm run dev
```

O front-end normalmente fica acessível na porta `5173` do codespace.

## 4) Acesso ao projeto no navegador

Para usar a aplicação pelo navegador do Codespace:

1. Abra a aba `PORTS`/`PORTAS` no VS Code.
2. Verifique as portas `5173` (front-end) e `8888` (back-end).
3. Use a URL pública do front-end, normalmente no formato `https://<NOME-DO-CODESPACE>-5173.app.github.dev`.
4. Se o navegador indicar algum problema de CORS, ajuste `ALLOWED_ORIGINS` para refletir a origem correta da porta 5173.
5. Reinicie os servidores após alterar qualquer arquivo `.env`.

## 5) Primeiro login

O seed inicial cria o usuário administrador com:

- Usuário: `admin`
- Senha: `Vulcom@DSM`

Use essas credenciais na primeira autenticação da aplicação.

