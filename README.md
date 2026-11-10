# 🐾 Meclaz Pets — API de Adoção de Animais

## 1. Sobre o projeto

A Meclaz Pets API é uma interface de programação desenvolvida para um sistema de adoção de animais.

Ela permite que outros sistemas e aplicativos utilizem suas funcionalidades para consultar animais disponíveis, cadastrar novos animais e registrar solicitações de adoção.

A API foi desenvolvida utilizando:

- **Dart:** linguagem de programação.
- **PostgreSQL:** banco de dados relacional.
- **Shelf:** criação do servidor HTTP.
- **BCrypt:** proteção das senhas dos administradores.
- **JWT:** autenticação e autorização.

Este projeto contém somente o backend da aplicação. Não é necessário instalar ou executar o aplicativo Flutter para utilizar a API.

## 2. Requisitos para executar

Antes de começar, instale os seguintes programas:

| Programa | Finalidade | Download |
|---|---|---|
| Dart SDK | Executar a API | https://dart.dev/get-dart |
| PostgreSQL | Armazenar os dados | https://www.postgresql.org/download/ |
| pgAdmin | Gerenciar o banco de dados | https://www.pgadmin.org/download/ |
| Git | Clonar o projeto | https://git-scm.com/downloads |
| GitHub Desktop (opcional) | Baixar e gerenciar o projeto | https://desktop.github.com/ |

**Observação:** o GitHub Desktop é opcional. Também é possível baixar o projeto pelo Git.

## 3. Baixando o projeto

Existem duas maneiras de obter os arquivos.

### Opção 1 — Pelo GitHub Desktop

1. Instale e abra o GitHub Desktop.
2. Entre na sua conta GitHub.
3. Clique em `File`.
4. Selecione `Clone Repository`.
5. Escolha o repositório Meclaz Pets.
6. Escolha uma pasta do computador para salvar o projeto.
7. Clique em `Clone`.

Após a clonagem, abra a pasta do projeto e localize `Meclaz_Pets_API`.

### Opção 2 — Pelo terminal

Abra o PowerShell e execute:

```powershell
git clone https://github.com/mel-wp/meclaz_pets.git
```

Entre na pasta da API:

```powershell
cd meclaz_pets\Meclaz_Pets_API
```

Caso a pasta tenha outro nome ou esteja em outro local, ajuste o caminho conforme a organização do repositório.

## 4. Configurando o banco de dados e criando as tabelas

A API utiliza PostgreSQL para armazenar os dados dos animais, administradores e solicitações de adoção.

Cada pessoa que executar o projeto deverá criar seu próprio banco de dados local.

### 4.1. Criando o banco de dados

1. Abra o pgAdmin.
2. Conecte-se ao servidor PostgreSQL.
3. Clique com o botão direito em `Databases`.
4. Selecione `Create → Database`.
5. No campo de nome, informe:

```text
meclaz_pets
```

6. Clique em `Save`.

### 4.2. Localizando o arquivo schema.sql

O projeto possui um arquivo chamado `schema.sql`, responsável por criar todas as tabelas necessárias para o funcionamento da API.

Para visualizar os comandos:

1. Abra a pasta `Meclaz_Pets_API` no Visual Studio Code.
2. Localize o arquivo `schema.sql` na raiz do projeto.
3. Clique nele para visualizar os comandos SQL.

**Importante:** não é necessário criar as tabelas manualmente. O arquivo `schema.sql` já contém a estrutura correta utilizada pela API.

### 4.3. Executando o schema.sql no pgAdmin

Depois de visualizar o arquivo no VS Code:

1. Abra o pgAdmin.
2. Localize o banco `meclaz_pets`.
3. Clique com o botão direito nele.
4. Selecione `Query Tool`.
5. No Query Tool, abra o arquivo `schema.sql` que está na pasta do projeto.
6. Confira se o conteúdo corresponde ao arquivo visualizado no VS Code.
7. Clique em `Execute` (ícone de raio ou botão ▶).

Após a execução, as seguintes tabelas deverão estar disponíveis:

| Tabela | Finalidade |
|---|---|
| `animais` | Armazena os dados dos animais cadastrados. |
| `usuarios` | Armazena os administradores e suas informações de acesso. |
| `solicitacoes` | Armazena os pedidos de adoção realizados pelos interessados. |

### 4.4. Conferindo as tabelas

Para verificar se tudo foi criado corretamente:

1. No pgAdmin, expanda o banco `meclaz_pets`.
2. Expanda `Schemas`.
3. Expanda `public`.
4. Clique em `Tables`.

Devem aparecer as tabelas `animais`, `usuarios` e `solicitacoes`.

**Atenção:** a estrutura dessas tabelas precisa corresponder ao arquivo `schema.sql`, pois a API utiliza esses nomes e campos nas consultas e operações do banco de dados.

## 5. Instalando as dependências

Abra o PowerShell dentro da pasta `Meclaz_Pets_API`.

Execute:

```powershell
dart pub get
```

Esse comando instala as bibliotecas necessárias para executar o projeto.

## 6. Cadastrando um administrador

Para cadastrar um administrador, utilize o script disponível em `bin/criar_admin.dart`.

No terminal, execute:

```powershell
dart run .\bin\criar_admin.dart
```

O programa solicitará:

- Senha do PostgreSQL.
- Nome do administrador.
- E-mail.
- Senha de acesso.

A senha do administrador deve possuir pelo menos 8 caracteres.

O cadastro é realizado diretamente no banco de dados. Não existe uma rota pública para criar administradores.

Esse procedimento deve ser realizado para cada administrador que terá permissão para cadastrar animais.

## 7. Configurando a chave JWT

Antes de iniciar a API, é necessário configurar uma chave secreta utilizada na autenticação.

No PowerShell, execute:

```powershell
$bytes = New-Object byte[] 48
[Security.Cryptography.RandomNumberGenerator]::Create().GetBytes($bytes)
$env:JWT_SECRET = [Convert]::ToBase64String($bytes)
```

A chave será gerada automaticamente.

**Atenção:**
- Não compartilhe a chave secreta.
- Não coloque a chave no GitHub.
- A configuração deve ser realizada novamente quando abrir uma nova sessão do PowerShell.
- Os tokens gerados com uma chave anterior não serão válidos após a troca da chave.

## 8. Iniciando a API

Depois de configurar o banco, instalar as dependências e configurar a chave JWT, execute:

```powershell
dart run .\bin\api_adocao.dart
```

O programa solicitará a senha do PostgreSQL.

Se tudo estiver configurado corretamente, aparecerão mensagens semelhantes a:

```text
Conectado ao PostgreSQL!
API rodando em http://127.0.0.1:8080
```

A API estará disponível localmente no endereço:

http://localhost:8080

**Importante:** mantenha o terminal aberto enquanto estiver utilizando a API.

## 9. Endpoints disponíveis

A API disponibiliza as seguintes rotas:

| Método | Endpoint | Funcionalidade | Autenticação |
|---|---|---|---|
| POST | `/login` | Realiza o login do administrador. | Não |
| GET | `/animais` | Lista todos os animais. | Não |
| GET | `/animais?disponivel=true` | Lista animais disponíveis. | Não |
| POST | `/animais` | Cadastra um novo animal. | JWT de administrador |
| POST | `/solicitacoes` | Registra uma solicitação de adoção. | Não |

### POST `/login`

Realiza o login do administrador.

Exemplo de requisição:

```json
{
  "email": "admin@email.com",
  "senha": "sua_senha"
}
```

Se os dados estiverem corretos, a API retorna um token JWT e as informações do administrador.

### GET `/animais`

Retorna todos os animais cadastrados no banco de dados.

### GET `/animais?disponivel=true`

Retorna somente os animais disponíveis para adoção.

### POST `/animais`

Permite cadastrar um novo animal.

Exemplo de corpo da requisição:

```json
{
  "nome": "Bob",
  "especie": "Cachorro",
  "idade_em_meses": 24
}
```

Essa rota exige autenticação.

É necessário enviar o token recebido no login no cabeçalho da requisição:

```http
Authorization: Bearer SEU_TOKEN
```

Somente administradores ativos podem cadastrar animais.

### POST `/solicitacoes`

Registra uma solicitação de adoção.

Exemplo:

```json
{
  "animal_id": 1,
  "nome_interessado": "Maria",
  "contato": "maria@email.com"
}
```

O animal informado precisa existir e estar disponível para adoção.

## 10. Testando a API

É possível testar as rotas utilizando ferramentas como:

- [Postman](https://www.postman.com/)
- [Insomnia](https://insomnia.rest/)

Para realizar os testes, utilize como endereço-base:

```text
http://localhost:8080
```

Primeiro, realize o login do administrador para obter o token JWT. Depois, utilize esse token nas requisições que exigem autenticação.

## 11. Segurança

A API possui mecanismos de segurança para proteger as operações administrativas:

- As senhas são armazenadas utilizando hash BCrypt.
- A autenticação é realizada por meio de JWT.
- Os tokens possuem validade de 8 horas.
- O cadastro de animais exige autenticação de administrador.
- Somente administradores ativos podem realizar operações protegidas.

As senhas do banco de dados e a chave JWT não devem ser compartilhadas publicamente.

## 12. Acesso por outros computadores

Na configuração padrão, a API utiliza o PostgreSQL instalado localmente e fica disponível no próprio computador.

Baixar o projeto pelo GitHub não significa que o banco de dados também será compartilhado.

Para que outras pessoas possam utilizar a mesma API pela internet, será necessário hospedar o servidor e o banco de dados em uma plataforma online.

---

## Repositório

**Meclaz Pets — API de Adoção de Animais**

https://github.com/mel-wp/meclaz_pets

Desenvolvido como projeto acadêmico.
