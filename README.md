# api-style-guide
Padrões de criação de API's REST

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
///Tabela de métodos Http

`GET:	Utilizado para recuperar um recurso.`
`POST:	Utilizado para criar um recurso ou para executar uma operação complexa em um recurso.`
`PUT:	Utilizado para atualizar um recurso.`
`DELETE: Utilizado para excluir um recurso.`
`PATCH:	Utilizado para executar uma atualização parcial em um recurso.`

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









## HATEOAS
O HATEOAS (Hypermedia as the Engine of Application State) é uma das propriedades do REST e  provê informações que permite navegar entre seus endpoints de forma dinâmica visto que inclui links junto às respostas.

No contexto de APIs RESTful, um cliente poderia interagir com um serviço inteiramente por meio de hipermídia fornecida dinamicamente pelo serviço. Um serviço orientado por hipermídia fornece representação de recurso (s) para seus clientes para navegar dinamicamente na API, incluindo links hipermídia nas respostas.

### Exemplo ###
O cliente pode seguir o selflink de pedidos e descobrir todas as operações possíveis que ele pode executar no recurso do pedidos.

Request: `POST https://api-int.grupodimedservices.com.br/tst/pedidos/v1/pedidos`

Response:
```
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
```
A API cria um novo pedido a partir da entrada e retorna os seguintes links para o cliente na response.

Um link para recuperar a representação completa do pedido (também conhecido como selflink) (GET).
Um link para atualizar o pedido (PUT).
Um link para atualizar parcialmente o pedido (PATCH).
Um link para excluir o pedido (DELETE).
