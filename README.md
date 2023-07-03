# Boas vindas ao repositório do projeto Cookmaster!

---

# Habilidades

- Entendi o que há por dentro de um token de autenticação;

- Criei tokens a partir de informações como login e senha;

- Apliquei autenticação nas rotas do Express, usando o token JWT;

- Fiz uploads de arquivos em APIs REST;

- Aprendi a salvar e consultar arquivos no servidor através de uma API REST;

- Por fim, utilizando o *chai-http*, realizei testes de integração que cobriram **100%** da aplicação;

---

## O que foi desenvolvido

Foi desenvolvida uma aplicação utilizando a arquitetura MSC.

Neste novo projeto foi possível fazer o cadastro e login de pessoas usuárias.

---

## Desenvolvimento

A aplicação foi desevolvida em camadas (Models, Service e Controllers) respeitando o modelo **MSC**.

Através dessa aplicação, é possível realizar as operações básicas que se pode fazer em um determinado banco de dados: Criação, Leitura, Atualização e Exclusão (ou `CRUD`, para as pessoas mais íntimas 😜).

Para realizar qualquer tipo de alteração no banco de dados (como cadastro, edição ou exclusão de receitas) é necessário autenticar-se. Além disso, as pessoas usuárias devem poder ser clientes ou administradores. Pessoas clientes apenas podem disparar ações nas receitas que ele mesmo criou. Já uma pessoa administradora pode disparar qualquer ação em qualquer receita.

A autenticação é feita via `JWT`.

É possível também adicionar uma imagem à uma receita, utilizando o upload de arquivos fornecido pelo `multer`.

## Utilização

1. Faça o clone do repositório
```
- git clone git@github.com:IvanRafael-Dev/cookmaster-api-node.git
```

2. Instale as dependências do projeto
```
- `npm install`
```

3. O projeto utiliza o MongoDB como banco de dados. Certifique-se de estar com o serviço do Mongo rodando em sua máquina

```
- sudo systemctl start mongod.service
```

4. Inicie a aplicação 
```
- npm start
```

## Testes

Você também pode rodar os testes:
```
- npm test 
```
e visualizar sua cobertura atual:
```
- npm run dev:test:coverage
```

# Funcionalidades do projeto

## Endpoints disponíveis

### 1 - Cadastro de usuários

- rota POST `/users`.

- No banco um usuário precisa ter os campos Email, Senha, Nome e Role.

- Para criar um usuário através da API, todos os campos são obrigatórios, com exceção do Role.

- O campo Email deve ser único.

- O body da requisição deve conter o seguinte formato:

  ```json
  {
    "name": "string",
    "email": "string",
    "password": "string"
  }
  ```

### 2 - Login de usuários

- rota POST `/login`.

- A rota recebe os campos Email e Senha e esses campos serão validados no banco de dados.

- Um token `JWT` é gerado e retornado caso haja sucesso no login. No seu payload estão presentes o id, email e role do usuário.

- O body da requisição deve conter o seguinte formato:

  ```json
  {
    "email": "string",
    "password": "string"
  }
  ```

### 3 - Cadastro de receitas

- rota POST `/recipes`.

- A receita só pode ser criada caso o usuário esteja logado e o token `JWT` validado.

- Nome, ingredientes e modo de preparo devem ser recebidos no corpo da requisição, com o seguinte formato:

  ```json
  {
    "name": "string",
    "ingredients": "string",
    "preparation": "string"
  }
  ```

### 4 - Listagem de receitas

- rota GET `/recipes`.

- A rota pode ser acessada por usuários logados ou não

**Além disso, as seguintes verificações serão feitas:**

- **[É possível listar todas as receitas sem estar autenticado]**

O resultado retornado para listar receitas com sucesso deverá ser conforme exibido abaixo, com um status http `200`:

![Receita com Sucesso](./public/listarreceitas.png)

- **[É possível listar todas as receitas estando autenticado]**

O resultado retornado para listar receitas com sucesso deverá ser conforme exibido abaixo, com um status http `200`:

![Receita com Sucesso](./public/listarreceitas.png)

### 5 - Visualizar uma receita específica

- rota GET `/recipes/:id`.

- A rota pode ser acessada por usuários logados ou não

### 6 - Edição de uma receita

- rota PUT `/recipes/:id`.

- A receita só pode ser atualizada caso o usuário esteja logado e o token `JWT` validado.

- A receita só pode ser atualizada caso pertença ao usuário logado, ou caso esse usuário seja um admin.

- O corpo da requisição deve receber o seguinte formato:

  ```json
  {
    "name": "string",
    "ingredients": "string",
    "preparation": "string"
  }
  ```

### 7 - Exclusão de uma receita

- rota DELETE `/recipes/:id`.

- A receita só pode ser excluída caso o usuário esteja logado e o token `JWT` validado.

- A receita só pode ser excluída caso pertença ao usuário logado, ou caso o usuário logado seja um admin.


- **[Não é possível excluir receita sem estar autenticado]**

O resultado retornado para excluir uma receita sem autenticação deverá ser conforme exibido abaixo, com um status http `401`:

![Excluir uma Receita sem autenticação](./public/excluirsemautenticacao.png)


### 8 - Crie um endpoint para a adição de uma imagem a uma receita

- rota POST `/recipes/:id/image/`.

- A imagem é lida do campo `image`.

- O endpoint aceita requisições no formato `multipart/form-data`.

- A receita só pode ser atualizada caso o usuário esteja logado e o token `JWT` validado.

- A receita só pode ser atualizada caso pertença ao usuário logado ou caso o usuário logado seja admin.


### 9 - Acessar a imagem de uma receita

- As imagens estão disponíveis através da rota `/images/<id-da-receita>.jpeg` na API.


### 10 - Cadastro de pessoas administradoras

- rota POST `/users/admin`.

- Só será possível adicionar um admin caso esta ação esteja sendo feita por outro admin, portanto, deve ser validado se há um admin logado.

- O corpo da requisição deve ter o seguinte formato:

  ```json
  {
    "name": "string",
    "email": "string",
    "password": "string"
  }
  ```

- **[Não é possível cadastrar um usuário admin, sem estar autenticado como um usuário admin]**

Se o usuário admin não é criado com sucesso o resultado retornado deverá ser conforme exibido abaixo, com um status http `403`:

![Criar usuário sem ser admin](./public/soadmincria.png)


