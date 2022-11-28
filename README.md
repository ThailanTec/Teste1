# Resolução de problema

## Para solucionar o problema proposto, apos uma analise foi constado que a melhor solucão para o problema seria utilizar:


**Tecnologia**

Golang (GO)


**Bancos de dados**

- Postgresql 

- Oracle DB
  

**Arquitetura de Aplicão + Extras**

- Arquitetura Exagonal
- DDD
- Docker 
- AWS

# Base A

### Segurança dos Dados

Para garantir uma segurança maior dos dados, principalmente do primeiro banco, no qual, contem informações mais importantes do cliente, vamos consumir do banco de dados Oracle. Como o mesmo está em nuvem e fornece segurança em várias camadas, incluindo controles para avaliar riscos, impedir a divulgação não autorizada de dados, detectar e relatar atividades em banco de dados e impor controles de acesso a dados no banco de dados com segurança orientada por dados. 

### Tráfego e Consumo de dados

Pensando que a nossa aplicação está com uma arquitetura *Hexagonal*, sendo possível desacoplar a parte de banco/consumo de dados e a parte onde fica a lógica da aplicação. Assim, conseguimos fazer de forma totalmente escalável, produtiva e segura. Já que a forma na qual chamamos os dados ela é separada no seu core. Pensando dessa forma, com a aplicação já sendo feita da forma, quando for realizada a requisição vamos ter o seguinte *payload*:

```

{
"id":4519,
"cpf":"954.336.580-66",
"nome":"Richarlison Henrique Neymar Santos",
"endereco":["Rua Adenor Leonardo Bachi", 884, "Ibirité-MG"],
"dividas":["CBF", "PRECOM", "UNOPAR"]
}
```

Os dados vão estar todos em sua maior parte criptografada no banco de dados, trazendo uma maior segurança na hora da requisição e até mesmo para a empresa em si, sabendo que os dados do seu usuário vão está com uma segurança maior. 


### Disponibilização dos Dados

<h4>Validação de acesso<h4>

Os dados vão ser disponibilizados via *API RestFull*, pensando dessa forma, para ser possível acessar os dados e informações de determinado usuário, quem fez a requisição deve conter uma chave de autorização e com essa chave, podemos validar qual sera o nível de acesso e quais dados poderão ser exibidos a partir dessa chave que o usuário possui. Essa chave deve ser passada no header da requisição. 

Exemplo:

```
Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Im5ZbXFPUDNtYzZZSDZnLTdUZEo5TDU1QmVJTnpIY1E3OUZ2NlcyODFQTjQifQ.eyJleHAiOjE2NjE0NjA0MjgxLCJpYXQiOjE2NjE0NjAxMjgxLCJqdGkiOiIyN2YxZTYwNy03ODViLTRhN2YtYjU0Yy1mYjYxYTdiOWZlODgiLCJpc3MiOiJodHRwczovL2lhbS1kaHVvLWRldi5ici5lbmdpbmVlcmluZy9yZWFsbXMvZGh1byIsImF1ZCI6WyJwbGF0Zm9ybS1iYWNrZW5kIiwiZGh1by1iYWNrZW5kIiwic3Vic2NyaXB0aW9uLWJhY2tlbmQiLCJhY2NvdW50Il0sInN1YiI6Ijc1MzQ5OTlkLWJjZDQtNDI3Ny1hY2JjLTU0NmRkNjBlMGVlMSIsInR5cCI6IkJlYXJlciIsImF6cCI6ImRodW8tZnJvbnRlbmQiLCJzZXNzaW9uX3N0YXRlIjoiMDhhYzgzM...

```
Passado tal header na requisição, através da regra de negócio implementada no código vai ser validado ser o usuário vai conseguir ou não acessar os dados do usuário. 

<h4>Exibição de dados<h4>

Os dados vão ser exibido em formato *Json*, dessa forma, mais fácil de serem trabalhados no frontend quando for necessário ser consumido na aplicação.


# Base B

### Segurança dos Dados

Como não precisa de uma segurança muito reforçada e necessita de ter os dados servidos de forma mais rápida, optou-se por escolher o *Postgressql*, que contem uma maior velocidade para processamentos dos seus dados, então, vamos conseguir servir para o usuário de forma mais rápida os dados.


### Tráfego e Consumo de dados

Como essa base de dados quem vai ser o responsável por acessar vai ser instituições pre-definidas como instituições de créditos, para conseguir acessar as informações dos usuários contidos no banco o mesmo deve passar na requisição o seu "ID institucional" setado no header da requisição, para que dessa forma, consiga acessar as informações do cliente. Ele vai ter acesso a esse ID quando criar a sua conta com a instituição e for validado. 

Exemplo:

X-Instution-Organization : sajhsakj812has8h32

Passado tal header o mesmo vai conseguir realizar o seu acesso perfeitamente. 

<h4>Exibição de dados<h4>

Os dados vão ser exibido em formato *Json*, dessa forma, mais fácil de serem trabalhados no frontend quando for necessário ser consumido na aplicação.


# Base C

### Segurança dos Dados

Como esse banco vai receber mais requisições e precisamos entregar os dados com mais agilidade, vamos utilizar o Redis, por conta de ser não relacional e ter uma alta escalabilidade horizontal o mesmo vai suportar mais requisições, vai conseguir entregar esses dados com maior velocidade que os bancos relacionais, e a sua forma de trabalhar com 'key-value', vai agilizar na hora de fazer as requisições pensando que sua chave pode ser o CPF do usuário.

### Tráfego e Consumo de dados

Como a mesma não contem nenhum tipo de dado crítico, podemos nos concentrar na velocidade e disponibilidade dos dados para quem realizou a requisição os tenham da forma mais hábil possível. Dessa forma, quando recebemos o CPF do cliente(Via requisição GET), já podemos devolver as informações com as informações.

Como trabalhamos com 'key-value', podemos retornar com base na requisição. Ex:

Ex:

GET: https://localhost:0000/localhost:0000 /v1/dadosuser/987878899/comprascente ou https://localhost:0000/v1/dadosuser/987878899/financeiro

<h4>Exibição de dados<h4>

Retornando um Json quando a requisição for feita.

{
"comprasRecentes":"Localiza: 286.90"
}


## Tecnologia

A tecnolgia escolhida para realizar a aplicação foi o Golang, tecnologia do google, que possibilita trabalhar com multi thread, assim possibilitando a maquina trabalha na sua melhor performance. O go, também traz a sua leveza e praticidade ao projeto, sendo simples de aprender e rápida de implementar. Trabalhando com micro-serviços ela se destaca por conseguir trazer com mais rapidez e agilidade e podemos ainda focar em qual micro-serviço vai ter mais "prioridade" na aplicação.

