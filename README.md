# API Style Guide
Padrões de criação de API's REST

------
<p align="left">&bull;
	<a href="#introdução">Introdução</a> <br/>&bull;
    	<a href="#princípios-de-design-de-serviços">Princípios de Design de Serviços</a> <br/>&bull;
	<a href="#métodos-http">Métodos HTTP</a> <br/>&bull;
	<a href="#códigos-de-status-http">Códigos de Status HTTP</a> <br/>&bull;
	<a href="#convenções-de-nomenclatura">Convenções de Nomeclatura</a> <br/>&bull;
	<a href="#hateoas">Hateoas</a> <br/>&bull;
	<a href="#exceptions">Exceptions</a> <br/>
</p>

## Introdução
Este documento tem por finalidade seguir o padrão de arquitetura da API RESTful, e auxiliar desenvolvedores na criação de API's utilizando padrões, regras e convenções com o objetivo de manter a evolução dos serviços.

## Princípios de Design de serviços

#### Acoplamento:
Serviços e consumidores devem ser fracamente acoplados uns dos outros.

Este princípio defende a concepção de contratos de serviço, com ênfase na redução das dependências entre o contrato de serviço, sua implementação e os consumidores de serviços.

* Um contrato de serviço não deve expor detalhes da implementação;
* Um contrato de serviço pode evoluir sem impactar os consumidores existentes;
* Um serviço em um domínio específico pode evoluir independentemente de outros domínios;

#### Encapsulamento
Um serviço de domínio pode acessar dados e funcionalidades que não possui através de outros contratos de serviço.

Este princípio defende que qualquer funcionalidade ou dados de que um serviço dependa e que não seja de sua propriedade  devem ser acessados apenas por meio de contratos de serviço.

* Um serviço tem um claro limite de isolamento - um escopo claro de propriedade em termos de funcionalidade e dados;
* Um serviço não pode expor os dados de que não é proprietário diretamente;

#### Estabilidade
Os contratos de serviço devem ser estáveis.

Os serviços devem ser pensados de tal forma que o contrato que eles exponham permaneçam válidos para quem está utilizando. Caso o contrato de serviço evolua de forma incompatível para o consumidor, isso deve ser comunicado claramente.

* Clientes existentes de um serviço devem ser suportados por um período de tempo documentado;
* Funcionalidades adicionais devem ser introduzidas de forma a não impactar os consumidores existentes;
* Políticas de descontinuação e migração devem ser claramente definidas para definir as expectativas dos consumidores;

#### Reutilizável
Os serviços devem ser desenvolvidos para serem reutilizáveis em vários contextos e por vários consumidores.

Desenvolver serviços de forma que eles possam ser utilizados por múltiplos consumidores e em múltiplos contextos, alguns dos quais podem evoluir ao longo do tempo.

* Um contrato de serviço deve ser projetado não apenas para o contexto imediato, mas com suporte para ser utilizado por vários consumidores em diferentes contextos;
* Um contrato de serviço pode precisar evoluir gradativamente para suportar vários contextos e consumidores ao longo do tempo;

#### Contrato baseado
Funcionalidade e dados só devem ser expostos através de contratos de serviços padronizados.

Todas as funcionalidades e dados devem ser expostos através de contratos de serviço padronizados, onde Padronizado significa 
que os contratos de serviço devem estar em conformidade com os padrões de projeto do contrato. Os consumidores de serviços podem, portanto, entender e acessar funcionalidade e dados apenas por meio de contratos de serviço.

* Funcionalidade e dados não podem ser compreendidos ou acessados ​​fora dos contratos de serviço;
* Cada parte dos dados (como aqueles gerenciados em um armazenamento de dados) é de propriedade de apenas um serviço;

#### Consistência
Os serviços devem seguir um conjunto comum de regras, estilos de interação, vocabulário e tipos compartilhados.

Esse princípio aumenta a facilidade de uso da plataforma de API, reduzindo a curva de aprendizado para consumidores de novos serviços.

* Um conjunto de padrões é definido para que os serviços cumpram
* Um serviço deve usar o vocabulário de dicionários comuns e compartilhados
* Estilos de interação compatíveis, granularidade de serviço e tipos compartilhados são essenciais para total interoperabilidade e facilidade de composições de serviço

#### Fácil de usar
Os serviços devem ser fáceis de usar e compor nos consumidores (e aplicativos).

Serviços podem ser combinados facilmente porque os contratos de serviço e os protocolos de acesso são consistentes, e cada contrato de serviço não precisa ser entendido de forma diferente.

* Um contrato de serviço é facilmente descoberto e compreensível
* Os contratos e protocolos de serviço são consistentes em todos os aspectos que podem ser - por exemplo, 
mecanismos de identificação e autenticação, semântica de erros, uso de tipo comum, paginação, etc.
* Um provedor do consumidor pode integrar, testar e implantar com facilidade um consumidor que usa esse serviço
* Um provedor do consumidor pode monitorar facilmente os aspectos não funcionais de um serviço

#### Externalizável
O serviço deve ser projetado de modo que a funcionalidade fornecida seja facilmente externalizável.

Um serviço é desenvolvido para uso por consumidores que podem ser de outro domínio ou equipe, outra unidade de negócios ou outra empresa. Nesses casos a funcionalidade exposta é a mesma; 
Como a funcionalidade exposta é a mesma, o serviço deve ser projetado uma vez e depois exteriorizado com base nas necessidades de negócios por meio de políticas apropriadas.

* A interface de serviço deve ser derivada do modelo de domínio e dos casos de uso pretendidos para suportar;
* Os protocolos de contrato de serviço e acesso suportados devem atender às necessidades do consumidor;
* A externalização de um serviço não deve exigir a reimplementação ou uma alteração no contrato de serviço;

## Métodos HTTP
| **Métdo Http** |                                       **Descrição**                                      |
|:----------:|:------------------------------------------------------------------------------------|
| **GET**        | Utlizado para recuperar um recurso                                                   |
| **POST**      | Utilizado para criar um recurso ou para executar uma operação complexa em um recurso |
| **PUT**        | Utilizado para atualizar um recurso                                                  |
| **DELETE**     | Utilizado para excluir um recurso                                                    |
| **PATCH**      | Utilizado para executar uma atualização parcial em um recurso                        |

* O método GET não deve ter efeitos colaterais. não pode mudar o estado de um recurso;
* O método POST deve ser usado para criar um novo recurso em uma coleção.
Pode ser usado para criar um novo sub-recurso e estabelecer sua relação com o recurso principal;
* O método POST pode ser usado em operações complexas, juntamente com o nome da operação, é considerado uma exceção ao    modelo RESTful. É mais aplicável em casos em que os recursos representam um processo de negócios e as operações são as etapas ou ações a serem executadas como parte dele;
* O método PUT deve ser usado para atualizar atributos de recursos ou para estabelecer um relacionamento de um recurso 
para um sub-recurso existente; atualiza o recurso principal com uma referência ao sub-recurso.
* O método DELETE deve ser usado excluir um recurso existente
* O método PATCH deve ser utilizado para atualizar parcialmente um recurso 

Exemplos:

`GET /pedidos Retorna a lista de pedidos`  
`GET /pedidos/10 Retorna um pedido específico`  
`POST /pedidos Cria um novo pedido`  
`PUT /pedidos/10 Atualiza o pedido #10`  
`PATCH /pedidos/10 Atualiza parcialmente o pedido #10`  
`DELETE /pedidos/10 Apaga o pedido #10`  

Exemplos com relacionamento:

`GET /pedidos/10/itens Retorna a lista de itens do pedido #10`  
`GET /pedidos/10/itens/5 Retorna o item #5 do pedido #10`  
`POST /pedidos/10/itens Cria uma novo item no pedido #10`  
`PUT /pedidos/10/itens/5 Atualiza o item #5 do pedido #10`  
`PATCH /pedidos/10/itens/5 Atualiza parcialmente o item #5 do pedido #10`  
`DELETE /pedidos/10/itens/5 Apaga o item #5 do pedido #10`  

## Códigos de Status HTTP
Serviços RESTful utilizam códigos de status HTTP para especificar os resultados da execução do método HTTP. 
O resultado de uma request utiliza um número inteiro e uma mensagem. O número é conhecido como o código de status e a 
mensagem como a frase da razão . A frase deve ser uma mensagem legível e que todos possam entender de forma clara, usada para esclarecer o resultado da resposta. O protocolo HTTP categoriza códigos de status em intervalos.

### Tabela de alcance
| **Alcance** |                                                                                             **Significado**                                                                                             |
|:-------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **2xx**     | Execução bem sucedida. Uma execução de método pode ser bem sucedida de várias formas. Este código de status especifica o caminho que ele conseguiu.                                                 |
| **4xx**     | Geralmente são problemas com a solicitação. Na maioria dos casos o cliente pode modificar sua solicitação e reenviá-la.                                                                             |
| **5xx**     | Erro do servidor: O servidor não pode executar o método devido a um defeito na aplicação. Os códigos de status da faixa 5xx não devem ser utilizados para validação ou tratamento de erros lógicos. |

#### Lista de códigos de status:
|      **Código do Status**      |                                                                                                                                                          **Descrição**                                                                                                                                                          |
|:--------------------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **200 OK**                     | Execução bem sucedida.                                                                                                                                                                                                                                                                                                      |
| **201 Created**                | Execução do método para indicar a criação bem-sucedida de um recurso(POST). Se o recurso já foi criado (por uma execução anterior do mesmo método por exemplo), o servidor deve retornar o código de status 200 OK.                                                                                                         |
| **202 Accepted**               | Usado para uma execução de método assíncrono para especificar que o servidor aceitou a solicitação e a executará posteriormente.                                                                                                                                                                                            |
| **204 No Content**             | O servidor executou com sucesso o método, mas não há nenhum corpo de entidade para retornar.                                                                                                                                                                                                                                |
| **400 Bad Request**            | A solicitação não pode ser entendida pelo servidor. Use este código de status para especificar: - Os dados como parte da carga útil não podem ser convertidos no tipo de dados subjacente; - Os dados não estão no formato esperado; - Campo obrigatório não está disponível; - Tipo de erro de validação de dados simples; |
| **401 Unauthorized**           | A solicitação requer autenticação e nenhuma foi fornecida.                                                                                                                                                                                                                                                                  |
| **403 Forbidden**              | O cliente não está autorizado a acessar o recuso, embora possa ter credenciais válidas. A API pode usar este código caso a autorização no nível de negócios falhe.                                                                                                                                                          |
| **404 Not Found**              | O servidor não encontrou nada que corrensponda ao URI da solicitação. Isso siginifica que o URI está incorreto ou o recurso não está disponível.                                                                                                                                                                            |
| **405 Method Not Allowed**     | O servidor não implementou o método HTTP solicitado. Normalmente, esse é o comportamento padrão para estruturas de API.                                                                                                                                                                                                     |
| **406 Not Acceptable**         | O servidor deve retornar esse código de status quando ele não pode retornar a carga útil da resposta usando o tipo de mídia solicitado pelo cliente. Por exemplo, se o cliente enviar um Accept-application/xml no cabeçalho e a API só puder gerar application/json.                                                       |
| **415 Unsupported Media Type** | O servidor deve retornar esse código de status quando o tipo de mídia da carga útil da solicitação não puder ser processado. Por exemplo, se o cliente envia um Content-Type application/xml no cabeçalho, mas a API só pode aceitar application/json.                                                                      |
| **422 Unprocessable Entity**   | A ação solicitada não pode ser executada e pode exigir interação com APIs ou processos fora da solicitação atual. Isso é diferente de uma responsta 500, pois não há problemas sistêmicos que limitem a API ao executar a solicitação.                                                                                      |
| **429 Too Many Requests**      | O servidor deve retornar esse código de status se o limite de taxa para o usuário, o aplicativo ou o token exceder um valor predefinido.                                                                                                                                                                                    |
| **500 Internal Server Error**  | É um erro de sistema e geralmente indica que, embora o cliente tenha parecido fornecer uma solicitação correta , algo inesperado deu errado no servidor. Uma resposta 500 indica um defeito de software do lado do servidor. 500 não deve ser utilizado para validação de clientes ou tratamento de erros lógicos.          |
| **503 Service Unavaliable**    | O servidor não pode manipular a solicitação de um serviço devido a uma manutenção temporária;                                                                                                                                                                                                                               |

#### Mapeamento de códgos de status:
| **Código do Status** | **200 OK** | **201 Created** | **202 Accepted** | **204 No Content** | **400 Bad Request** | **404 Not Found** | **422 Unprocessable Entity** | **500 Internal Server Error** |
|:----------------:|:------:|:-----------:|:------------:|:--------------:|:---------------:|:-------------:|:------------------------:|:-------------------------:|
| **GET**              |    x   |             |              |                |        x        |       x       |             x            |             x             |
| **POST**             |    x   |      x      |       x      |                |        x        |       x       |             x            |             x             |
| **PUT**              |    x   |             |       x      |        x       |        x        |       x       |             x            |             x             |
| **PATCH**            |    x   |             |              |        x       |        x        |       x       |                          |             x             |
| **DELETE**           |    x   |             |              |        x       |        x        |       x       |             x            |             x             |

* `GET`: O propósito do método `GET` é recuperar um recurso. No caso de sucesso, um código de status `200` e uma resposta com o conteúdo do recurso são esperados. Nos casos onde uma coleção de recursos está vazia, o status `200` também é apropriado (haverá uma lista vazia na resposta). Se um item de recurso for excluído (soft deleted) nos dados subjacentes, o status `404` é adequado.

* `POST`: O propósito primário do `POST` é criar um recurso. Se o recurso não existir e for criado como parte da execução, então o status `201` deve ser retornado.
	* É esperado que em uma execução de sucesso, uma referência ao recurso criado (em formado de link ou ID) seja retornada no corpo de resposta.
	* Caso a criação do recurso dependa da existência de outro sub-recurso que não exista, o status `404` pode ser retornado.

* `PUT`: Este método deve retornar o código de status `204` e não há necessidade de retornar nenhum conteúdo na maioria dos casos onde a requisição foi feita para atualizar um recurso e ele foi atualizado com sucesso. As informações de requisição não devem ecoar de volta.
	* Em casos raros onde houver necessidade de retorno de dados na resposta, o código de status `200` deve ser utilizado.
	
* `PATCH`: Este método deve seguir as mesmas semânticas do método `PUT`, status `204` e nenhum corpo de resposta.

* `DELETE`: Este método deve retornar o status `204` e não há necessidade de corpo de resposta caso o recurso tenha sido deletado com sucesso.
	* `DELETE` deve continuar retornando status `204` mesmo se o recurso em questão já tenha sido deletado, pois retornar `404` neste método pode levar o consumidor a acreditar erroneamente que o recurso nunca existiu. O correto seria utilizar um `GET` para garantir que o recurso em questão realmente existe antes de executar um `DELETE`.

## Convenções de Nomenclatura
* URIs devem começar com uma letra e usar apenas letras minúsculas.
* Expressões em caminhos URI deve ser separado usando um hífen (-). `/pedido-itens`
* Expressões em seqüências de caracteres de consulta deve ser separado usando sublinhado (_). `/clientes/{cliente_id}`
* Um recurso individual em uma coleção de recursos pode existir após a URI de coleta. `/clientes/{cliente_id}`
* Coleções de sub-recursos podem existir após um recurso individual. Isso deve transmitir um relacionamento com outra coleção de recursos (itens de pedido). `/pedidos/{pedido_id}/itens`
* Recursos individuais de sub-recursos podem existir, mas devem ser evitados em favor de recursos de nível superior.
`/pedidos/{pedido_id}/itens/{item_id}`
Melhor: `/pedido-itens/{pedido_id}`

#### Nomes de recursos
* Substantivos devem ser usados, ao invés de utilizar verbos.
* Os nomes dos recursos devem ser singulares para singletons e para coleções devem estar no plural.

#### Caminho do recurso - URI
Exemplo: `https://api-int.grupodimedservices.com.br/tst/pedido-service/v1/pedidos/10`

| **Parte do Caminho** | **Descrição**               | **Definição**                                                                                                                                                                                                       |
|:----------------:|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **/v1**              | Versão principal da API | A versão principal da API é usada para distinguir entre duas versões incompatíveis com versões anteriores da mesma API.                                                                                         |
| **/pedido-service**  | Nome do serviço               | O nome do serviço é usado para fornecer um contexto e um escopo para recursos.|
| **/pedidos**         | Nome do recurso         | Representa o tipo de recurso a ser acessado nas operações.
| **/10**              | ID do recurso           | Para recuperar um recurso específico da coleção, um ID de recurso deve ser especificado como parte do URI.|

#### Caminho do sub-recurso - URI

|                                        **Exemplo**                                        | **Descrição**                                                                                                                                                                                                                                                                  |
|:-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **GET http://api-int.grupodimedservices.com.br/tst/pedido-service/v1/pedidos/10/itens**   | Essa chamada deve retornar todos os itens de um pedido.                                                                                                                                                                                                                    |
| **GET http://api-int.grupodimedservices.com.br/tst/pedido-service/v1/pedidos/10/itens/2** | Essa chamada deve retornar apenas os detalhes de um item específico associado a um pedido. Na prática, duas invocações de passo devem ser evitadas. Se o segundo identificador for único, o recurso de nível superior (por exemplo /pedido-service/v1/itens/2 é preferido) |


## HATEOAS
O HATEOAS (Hypermedia as the Engine of Application State) é uma das propriedades do REST e  provê informações que permite navegar entre seus endpoints de forma dinâmica visto que inclui links junto às respostas.

No contexto de APIs RESTful, um cliente poderia interagir com um serviço inteiramente por meio de hipermídia fornecida dinamicamente pelo serviço. Um serviço orientado por hipermídia fornece representação de recurso (s) para seus clientes para navegar dinamicamente na API, incluindo links hipermídia nas respostas.

### Exemplo ###
O cliente pode seguir o selflink de pedidos e descobrir todas as operações possíveis que ele pode executar no recurso do pedidos.

Request: `POST https://api-int.grupodimedservices.com.br/tst/pedidos/v1/pedidos`

Response:
```json
HTTP/1.1 201 CREATED
Content-Type: application/json
{
	"id": "10",
	"cliente": "Maria",
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
```
A API cria um novo pedido a partir da entrada e retorna os seguintes links para o cliente na response.

Um link para recuperar a representação completa do pedido (também conhecido como selflink) (GET).
Um link para atualizar o pedido (PUT).
Um link para atualizar parcialmente o pedido (PATCH).
Um link para excluir o pedido (DELETE).

## Exceptions


#### Exemplo Exception 
```json
{
    "namespace": "/app-bff/v1/message",
    "language": "pt-BR",
    "errors": [
        {
            "namespace": "/app-bff/v1/message",
            "name": "INTERNAL_SERVER_ERROR",
            "message": "Ocorreu um erro inesperado na aplicação",
            "httpStatusCodes": [
                "INTERNAL_SERVER_ERROR"
            ],
            "issues": [
                {
                    "id": "UNKNOWN_ISSUE",
                    "issue": "JSON decoding error: Cannot deserialize value of type `br.com.dimed.appbff.microservicesintegration.covenant.coverage.model.output.FeePayerEnum` from String \"CONVENIO\": value not one of declared Enum instance names: [COVENANT, AFFILIATED, FREE]; nested exception is com.fasterxml.jackson.databind.exc.InvalidFormatException: Cannot deserialize value of type `br.com.dimed.appbff.microservicesintegration.covenant.coverage.model.output.FeePayerEnum` from String \"CONVENIO\": value not one of declared Enum instance names: [COVENANT, AFFILIATED, FREE]\n at [Source: UNKNOWN; line: -1, column: -1] (through reference chain: br.com.dimed.appbff.microservicesintegration.covenant.coverage.model.output.CovenantCoverageOutput[\"pagaTaxa\"])"
                }
            ],
            "suggestedApplicationActions": [
                "Entre em contato com o nosso suporte através do e-mail desenvservices@dimed.com.br"
            ],
            "suggestedUserActions": [
                "Por favor, caso o problema persista entre em contato com o nosso suporte"
            ]
        }
    ]
}
```

