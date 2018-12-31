# api-style-guide
Padrões de criação de API's REST

## HATEOAS ###
O HATEOAS (Hypermedia as the Engine of Application State) é uma das propriedades do REST e  provê informações que permite navegar entre seus endpoints de forma dinâmica visto que inclui links junto às respostas.

No contexto de APIs RESTful, um cliente poderia interagir com um serviço inteiramente por meio de hipermídia fornecida dinamicamente pelo serviço. Um serviço orientado por hipermídia fornece representação de recurso (s) para seus clientes para navegar dinamicamente na API, incluindo links hipermídia nas respostas.

### Exemplo ###
O cliente pode seguir o selflink de pedidos e descobrir todas as operações possíveis que ele pode executar no recurso do pedidos.

Request: `POST https://api-int.grupodimedservices.com.br/tst/pedidos/v1/pedidos`
Response:
´´´
HTTP/1.1 201 CREATED
Content-Type: application/json
{
	"id": "10",
	"nome": "Pedro",
	"links": [
	    {
	        "href": "https://api-int.grupodimedservices.com.br/tst/pedidos/v1/pedidos",
	        "rel": "self"
	    },
	    {
	        "href": "https://api-int.grupodimedservices.com.br/tst/pedidos/v1/pedidos/10",
	        "rel": "delete",
	        "method": "DELETE"
	    },
	    {
	        "href": "https://api-int.grupodimedservices.com.br/tst/pedidos/v1/pedidos/10",
	        "rel": "replace",
	        "method": "PUT"
	    },
	    {
	        "href": "https://api-int.grupodimedservices.com.br/tst/pedidos/v1/pedidos/10",
	        "rel": "edit",
	        "method": "PATCH"
	    }
	]
}
´´´
A API cria um novo pedido a partir da entrada e retorna os seguintes links para o cliente na response.

Um link para recuperar a representação completa do pedido (também conhecido como selflink) (GET).
Um link para atualizar o pedido (PUT).
Um link para atualizar parcialmente o pedido (PATCH).
Um link para excluir o pedido (DELETE).
