title=Protocolo HTTP
date=2023-11-13
type=post
tags=web
status=published
~~~~~~

## Protocolos, pedidos e respostas

A web é essencialmente um conjunto de computadores interligados. É por meio dessa interligação que essas máquinas se comunicam e trocam documentos entre si.

A arquitetura predominantemente utilizada é conhecida como cliente-servidor. Ou seja, o cliente faz um pedido, e o servidor responde.

Desta forma, a comunicação dá-se basicamente atráves de protocolos, pedidos e respostas (requests e responses).

O protocolo mais comum é o HTTP, que define algumas regras para que essa comunicação aconteça. Dentre essas regras, o protocolo HTTP define um conjunto de verbos que especificam as ações capazes de realizar essa comunicação e troca de informações.

## A mensagem do HTTP

A mensagem transmitida pelo protocolo HTTP possui vários componentes, que de maneira geral, incluem:

- Requisição:
  - especificação do método 
  - alvo da requisição
  - versão do HTTP
- Resposta:
  - Protocolo e versão
  - Status da resposta
  
A mensagem transmitirá também vários outros parâmetros. Além disso, dependendo do método, a mensagem terá um corpo, necessário para o processamento no servidor.

Abaixo, um exemplo de como pode ser uma mensagem de request:

```
GET / HTTP/1.1
Host: localhost
... Outros parâmetros
```

e um modelo de como pode ser uma mensagem de response:

```
HTTP/1.1 200 OK
... outros parâmetros

```
Entre os parâmetros de envio, podemos destacar um que é o formato da resposta. Quando o cliente requisita algo, ele pode definir ao servidor o formato da resposta que ele espera. Esse formato é definido pelo parâmetro `Accept`.

Entre os parâmetros de resposta, podemos destacar um que é o formato do dado enviado. Quando o servidor quer informar ao cliente o formato da resposta, ele pode utilizar o parâmetro `Content-Type`.

## Métodos HTTP

Os métodos mais comuns do HTTP e suas utilizações de forma mais genéricas são:

- GET: utilizado para requisições ao servidor
- POST: utilizado para enviar dados ao servidor
- PUT: utilizado para atualizar dados no servidor
- DELETE: utilizado para remover dados do servidor

É através destes métodos que as solicitações ao servidor vão ocorrer no HTTP. Além disso, o servidor poderá utilizar códigos para enviar respostas ao cliente.

Esses códigos, variam de acordo com o status dessa resposta. 

Os códigos mais comuns nessa comunicação são:

- 2xx: OK, requisição respondida com sucesso.
- 3xx: Redirecionamento, em razão de alguns recurso que foi movido ou atualiado.
- 4xx: Erro no cliente, indiciando algum problema na requisição realizada pelo cliente.
- 5xx: Server error, indicando que existe algum erro no processamento no servidor.

## Stateless

Uma característica importante do HTTP é que ele é `stateless`. Isso quer dizer que ele não mantém estado.

Por isso, existem algumas formas de contornar essa característica, a fim de dizer ao servidor que um cliente específico já acessou ou que deveria ter acesso a um determinado recurso. São elas:

- Sessão: tempo de utilização da aplicação 
- Cookies: arquivo associado ao domínio

## Além do HTTP 1.1

Vale lembrar que a web está sempre em evolução, e já estáão em operação novas versões do protocolo HTTP, como a 2 e 3.

Consulte os links abaixo para mais detalhes:

- [What is HTTP/2, and whats does it do?](https://nordvpn.com/pt-br/blog/what-is-http2/)
- [O que é HTTP/3](https://www.cloudflare.com/pt-br/learning/performance/what-is-http3/)
- [HTTP/1 vs HTTP/2 vs HTTP/3](https://dev.to/accreditly/http1-vs-http2-vs-http3-2k1c)

## Para saber mais

- [HTTP - MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP)
