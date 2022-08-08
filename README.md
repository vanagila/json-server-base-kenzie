# json-server-base

Esse é o repositório com a base de JSON-Server + JSON-Server-Auth já configurada. Reaproveitada para atividade da sprint 5 do curso de Front-End.

## Endpoints

No total, existem 7 endpoints na API, 5 endpoints que podem ser utilizados para cadastro e login e 2 outros para cadastro e leitura de produtos.

### Cadastro

POST /register
POST /signup
POST /users

Qualquer um dos endpoints /register, /signup e /users irá cadastrar o usuário na lista de "Users", sendo que os campos obrigatórios são os de email e password.
Você pode ficar a vontade para adicionar qualquer outra propriedade no corpo do cadastro dos usuários.

POST /register - FORMATO DA REQUISIÇÃO <br/>
{
"email": "kenzinho@email.com",
"password": "123456"
}

POST /register - FORMATO DA RESPOSTA - STATUS 201 <br/>
{
"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InZhbmFnaWxhM0BlbWFpbC5jb20iLCJpYXQiOjE2NTk5Mjc3MjIsImV4cCI6MTY1OTkzMTMyMiwic3ViIjoiNCJ9.C-5t0oV4rSP7iEXNIh-3Hk7bk_wrpT8wXX2KDIOzNPo",
"user": {
"email": "kenzinho@email.com",
"id": 1
}
}

<h3>Possíveis erros</h3>
Só é permitido um cadastro por e-mail, então caso tente fazer o cadastro com um e-mail já usado, ocorrerá a mensagem de erro:

POST /register - FORMATO DA RESPOSTA - STATUS 400 <br/>
"Email already exists"

### Login

POST /login <br/>
POST /signin

Qualquer um desses 2 endpoints pode ser usado para realizar login com um dos usuários cadastrados na lista de "Users"

POST /login - FORMATO DA REQUISIÇÃO <br/>
{
"email": "kenzinho@email.com",
"password": "123456"
}

POST /login - FORMATO DA RESPOSTA - STATUS 200 <br/>
{
"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InZhbmFnaWxhM0BlbWFpbC5jb20iLCJpYXQiOjE2NTk5Mjg4MjUsImV4cCI6MTY1OTkzMjQyNSwic3ViIjoiNCJ9.8s2rkHqyzDuh8QpaU5f8VNmIqPXyq0bGlUiMtWZB7Jo",
"user": {
"email": "kenzinho@email.com",
"id": 1
}
}

<h3>Possíveis erros</h3>
Caso envie o e-mail incorreto, receberá a seguinte resposta:

POST /login - FORMATO DA RESPOSTA - STATUS 400 <br/>
"Cannot find user"

Enquanto a senha incorreta é tratada da seguinte maneira:
"Incorrect password"

É possível ver suas informações, email, senha e id de usuário. Para tanto é necessário estar logado:

GET /users/userId - FORMATO DA RESPOSTA - STATUS 200 <br/>
{
"email": "kenzinho@email.com",
"password": "$2a$10$7POeKrYd.WMUJrYUlmvO9eLWW1uioKxNLCxRO9cMK9.fSoXVoWIrC",
"id": 1
}

### Produtos

Os endpoints relacionados aos produtos são:

/products
/onsale

Para fazer cadastro em qualquer das rotas é necessário estar logado.
Enquanto /products é o endpoint geral para todos os produtos, /onsale é destinado para produtos que estejam em promoção,

CADASTRAR UM PRODUTO em /product

POST /product - FORMATO DA REQUISIÇÃO <br/>
{
"name": "Bateria Eletrônica",
"price": 3000,
"category": "Instrumentos",
"userId": 1
}
POST /products - FORMATO DA RESPOSTA - STATUS 201 <br/>

{
"name": "Bateria Eletrônica",
"price": 3000,
"category": "Instrumentos",
"userId": 1
}

<h3>Possíveis erros</h3>
Se tentar cadastrar um produto sem estar logado, a reposta da requisição será a seguinte:

POST /products - FORMATO DA RESPOSTA - STATUS 403 <br/>
"Private resource creation: request body must have a reference to the owner id"

Ainda em /products é possível, sem necessidade de cadastro ou login, receber por meio do GET, todos os produtos que foram cadastrados por esse endpoint:

GET /product - FORMATO DA RESPOSTA <br/>

[
{
"name": "Geladeira",
"price": 1000,
"category": "Eletrodoméstico",
"userId": 2,
"id": 1
},
{
"name": "Computador",
"price": 2000,
"category": "Informática",
"userId": 2,
"id": 2
}
{
"name": "Bateria Eletrônica",
"price": 3000,
"category": "Instrumentos",
"userId": 2,
"id": 3
}
]

No endpoint /onsale, é seguido o mesmo formato de requisição para cadastro de produto, entretanto só é cadastrado produto que estiver em promoção. Outra diferença é no método GET, o usuário só poderá ver esses produtos se estiver logado.

Por fim, apenas o usuário que cadastrou o produto poderá excluí-lo:

DELETE /product/id - FORMATO DA REQUISIÇÃO <br/>

O id é um número inteiro para identificação própria de cada produto, esse número é um dos dados da resposta da requisição de cadastro do produto.
