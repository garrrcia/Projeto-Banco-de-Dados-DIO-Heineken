# Projeto de Banco de Dados de E-commerce

## Escopo

Refinando um Projeto Conceitual de Banco de Dados – E-COMMERCE

- Contexto do projeto: Levantamento de requisitos
- Projeto conceitual: Modeloe ER(Entidade - Relacionamento)
- Projeto lógico: Modelo Relacional

### Diagrama Entidade-Relacionamento conceitual de um projeto de e-commerce

MySQLWorkbench --> Novo Diagrama --> Diagram EER--> Tabelas

## Levantamento de Requisitos

Um e-commerce é uma marketplace para diferentes vendedores parceiros que vendem diversos produtos. Em sua base de dados devem esta armazenados: 

- Os dados de produtos comercializados;
- Os dados de parceiros que realizam pedidos;
- Os dados de vendedores parceiros que realizam as vendas para os clientes
- Os pedidos realizados;
- As entregas realizadas;
- Os fornecedores cadastrados;
- O estoque de produtos armazenados;

**Produto:**
- Os produtos são vendidos por uma única plataforma online. 
- Contudo, estes podem ter vendedores distintos (terceiros) 
- Cada produto possui um fornecedor
- Um ou mais produtos podem compor um pedido

**Cliente:**
- O cliente pode se cadastrar no site com seu CPF ou CNPJ, mas deve escolher apenas um
- O Endereço do cliente irá determinar o valor do frete
- Um cliente pode comprar mais de um pedido. Este tem um período de carência para devolução do produto

**Pedido:**
- O pedidos são criados por clientes e possuem informações de compra, endereço e status da entrega
- Um produto ou mais compoem o pedido
- O pedido pode ser cancelado
- O pedido pode ter mais de uma forma de pagamento

**Fornecedor:**
- Deve armazenar CNPJ
- Deve armazenar Razão Social 

**Estoque:**
- Deve informar os produtos estocados e a quantidade
- Deve guardar o local deste estoque/local que o produto esta armazenado

**Vendedores parceiros:**
- Razão Social
- Local

**Entrega:**
- Deve conter o código de rastreio
- Deve conter o status da entrega

**Pagamento:**
- Permite múltiplas formas de pagamento

As tabelas propostas para o esquema do banco de dados são: Produto, Cliente, Pedido, Fornecedor, Estoque, Vendedores_Parceiros, Entrega, Item_Pedido, Produto_Vendedor, Endereco, Metodo_Pagamento e Pedido_Pagamento.

![image](https://github.com/user-attachments/assets/928d199c-135d-420f-bf42-5c87c97b4bb5)

## Análise Aprofundada das Entidades Principais e Atributos

**Produto:**

Cada produto possui um fornecedor principal, e um ou mais produtos podem compor um único pedido.

produto_id (INT, Chave Primária, Auto Incremento): Um identificador único para cada produto catalogado na plataforma.
nome (VARCHAR(255), Not Null): O nome comercial do produto.
descricao (TEXT): Uma descrição detalhada das características e funcionalidades do produto.
preco (DECIMAL(10, 2), Not Null): O preço de venda base do produto na plataforma.
fornecedor_id (INT, Not Null, Chave Estrangeira referenciando Fornecedor): O identificador do fornecedor principal que abastece este produto.
data_cadastro (TIMESTAMP, Default CURRENT_TIMESTAMP): A data e hora em que o produto foi registrado na plataforma.
É importante notar que, embora o usuário mencione a existência de múltiplos vendedores para um produto, as informações centrais do produto, como nome e descrição, provavelmente são gerenciadas de forma centralizada pela plataforma.1 A ligação com os vendedores específicos será tratada por meio de um relacionamento distinto.


**Cliente:**

Os clientes se cadastram na plataforma utilizando CPF ou CNPJ, devendo escolher apenas um. O endereço do cliente é um fator determinante no cálculo do valor do frete. Um cliente pode realizar múltiplos pedidos e possui um período de carência para a devolução de produtos.

cliente_id (INT, Chave Primária, Auto Incremento): Um identificador único para cada cliente registrado.
tipo_documento (ENUM('CPF', 'CNPJ'), Not Null): O tipo de documento de identificação fornecido pelo cliente.
numero_documento (VARCHAR(20), Not Null, Unique): O número do CPF ou CNPJ do cliente, garantindo a unicidade.
nome (VARCHAR(255), Not Null): O nome completo do cliente ou a razão social da empresa.
email (VARCHAR(255), Not Null, Unique): O endereço de e-mail do cliente, utilizado para comunicação e login.
telefone (VARCHAR(20)): O número de telefone do cliente para contato.
data_cadastro (TIMESTAMP, Default CURRENT_TIMESTAMP): A data e hora em que o cliente se cadastrou na plataforma.

**Pedido:**

Os pedidos são criados pelos clientes e contêm informações sobre a compra, o endereço de entrega e o status da entrega. Um ou mais produtos compõem um pedido, que pode ser cancelado e pode ter mais de uma forma de pagamento.

pedido_id (INT, Chave Primária, Auto Incremento): Um identificador único para cada pedido realizado.
cliente_id (INT, Not Null, Chave Estrangeira referenciando Cliente): O identificador do cliente que fez o pedido.
endereco_entrega_id (INT, Not Null, Chave Estrangeira referenciando Endereco): O identificador do endereço de entrega para este pedido.
data_pedido (TIMESTAMP, Default CURRENT_TIMESTAMP): A data e hora em que o pedido foi efetuado.
status_pedido (VARCHAR(50)): O status atual do pedido (por exemplo, 'Pendente', 'Processando', 'Enviado', 'Cancelado', 'Entregue').
valor_total (DECIMAL(10, 2)): O valor total do pedido.

**Fornecedor:**

Os fornecedores são cadastrados no sistema e devem armazenar o CNPJ e a razão social.

fornecedor_id (INT, Chave Primária, Auto Incremento): Um identificador único para cada fornecedor.
cnpj (VARCHAR(20), Not Null, Unique): O CNPJ do fornecedor, garantindo a unicidade.
razao_social (VARCHAR(255), Not Null): A razão social do fornecedor.
nome_fantasia (VARCHAR(255)): O nome fantasia do fornecedor (opcional).
endereco (VARCHAR(255)): O endereço do fornecedor.
telefone (VARCHAR(20)): O número de telefone do fornecedor para contato.
email (VARCHAR(255)): O endereço de e-mail do fornecedor.

**Estoque:**

O estoque de produtos armazenados deve informar quais produtos estão em estoque e em qual quantidade. Também deve guardar a localização de cada item no estoque.

estoque_id (INT, Chave Primária, Auto Incremento): Um identificador único para cada registro de estoque.
produto_id (INT, Not Null, Chave Estrangeira referenciando Produto): O identificador do produto em estoque.
vendedor_id (INT, Not Null, Chave Estrangeira referenciando Vendedores_Parceiros): O identificador do vendedor parceiro que possui este estoque. Em um modelo de marketplace, o estoque é geralmente gerenciado por cada vendedor.3
quantidade (INT, Not Null): A quantidade disponível do produto em estoque.
localizacao (VARCHAR(255)): A localização física do produto no estoque.
data_atualizacao (TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP): A data e hora da última atualização das informações de estoque.
Em um modelo de marketplace com múltiplos vendedores, o controle de estoque é frequentemente descentralizado, com cada vendedor responsável por gerenciar seu próprio inventário.7 Portanto, é crucial vincular o estoque não apenas ao produto, mas também ao vendedor específico que o possui.

**Vendedores Parceiros:**

Os dados dos vendedores parceiros que realizam as vendas para os clientes devem incluir a razão social e a localização.

vendedor_id (INT, Chave Primária, Auto Incremento): Um identificador único para cada vendedor parceiro.
razao_social (VARCHAR(255), Not Null): A razão social do vendedor parceiro.
nome_fantasia (VARCHAR(255)): O nome fantasia do vendedor parceiro (opcional).
localizacao (VARCHAR(255)): A localização física ou principal do vendedor parceiro.
cnpj (VARCHAR(20), Unique): O CNPJ do vendedor parceiro (opcional, pois alguns podem ser pessoas físicas).
email (VARCHAR(255)): O endereço de e-mail do vendedor parceiro para contato.
telefone (VARCHAR(20)): O número de telefone do vendedor parceiro para contato.
data_cadastro (TIMESTAMP, Default CURRENT_TIMESTAMP): A data e hora em que o vendedor parceiro se cadastrou na plataforma.
Esta entidade armazena as informações essenciais sobre os vendedores que utilizam a plataforma para comercializar seus produtos. A razão social e a localização são informações cruciais fornecidas pelo usuário. Outros detalhes de contato são importantes para a gestão da plataforma.

**Entrega:**

As entregas realizadas devem conter o código de rastreio e o status da entrega.

entrega_id (INT, Chave Primária, Auto Incremento): Um identificador único para cada registro de entrega.
pedido_id (INT, Not Null, Chave Estrangeira referenciando Pedido): O identificador do pedido ao qual esta entrega se refere.
codigo_rastreio (VARCHAR(255)): O código de rastreamento fornecido pela transportadora.
status_entrega (VARCHAR(50)): O status atual da entrega (por exemplo, 'Em trânsito', 'Saiu para entrega', 'Entregue', 'Problema na entrega').
data_envio (TIMESTAMP): A data e hora em que o pedido foi enviado.
data_prevista_entrega (DATE): A data prevista para a entrega.
data_entrega (TIMESTAMP): A data e hora em que a entrega foi efetivamente realizada.
Esta entidade permite o rastreamento do processo de entrega de cada pedido, fornecendo informações importantes tanto para a plataforma quanto para o cliente.

## Estabelecendo Relacionamentos Entre Entidades

1. Cliente para Pedido:
Relacionamento: Um-para-muitos (Um cliente pode realizar múltiplos pedidos, mas cada pedido pertence a um único cliente).
Implementação: A tabela Pedido conterá uma chave estrangeira cliente_id referenciando a tabela Cliente.

2. Pedido para Item_Pedido (Order Items):
Relacionamento: Um-para-muitos (Um pedido pode conter um ou mais produtos, com detalhes específicos para cada produto no pedido). Isso representa uma relação muitos-para-muitos entre Pedido e Produto, resolvida com uma tabela intermediária.
Implementação: Introduzir uma tabela intermediária item_pedido com os seguintes atributos:
item_pedido_id (INT, Chave Primária, Auto Incremento)
pedido_id (INT, Not Null, Chave Estrangeira referenciando Pedido)
produto_id (INT, Not Null, Chave Estrangeira referenciando Produto)
vendedor_id (INT, Not Null, Chave Estrangeira referenciando Vendedores_Parceiros): É crucial registrar qual vendedor parceiro vendeu cada item específico dentro de um pedido.1
quantidade (INT, Not Null): A quantidade do produto neste item do pedido.
preco_unitario (DECIMAL(10, 2), Not Null): O preço unitário do produto no momento em que o pedido foi realizado. É importante armazenar o preço no momento da compra, pois ele pode mudar posteriormente.1

3. Produto para Fornecedor:
Relacionamento: Muitos-para-um (Cada produto tem um fornecedor principal, mas um fornecedor pode fornecer múltiplos produtos).
Implementação: A tabela Produto conterá uma chave estrangeira fornecedor_id referenciando a tabela Fornecedor.

4. Produto para Produto_Vendedor (Product Vendors):
Relacionamento: Muitos-para-muitos (Um produto pode ser vendido por múltiplos vendedores, e um vendedor pode vender múltiplos produtos).
Implementação: Introduzir uma tabela de junção produto_vendedor com os seguintes atributos:
produto_id (INT, Not Null, Chave Estrangeira referenciando Produto, Chave Primária)
vendedor_id (INT, Not Null, Chave Estrangeira referenciando Vendedores_Parceiros, Chave Primária)
preco_venda (DECIMAL(10, 2)): O preço pelo qual este vendedor específico vende este produto.
disponibilidade (BOOLEAN): Indica se o produto está atualmente disponível para venda por este vendedor.2

5. Pedido para Entrega:
Relacionamento: Um-para-um (Cada pedido tem um registro de entrega associado).
Implementação: A tabela Entrega conterá uma chave estrangeira pedido_id referenciando a tabela Pedido.

6. Produto para Estoque:
Relacionamento: Um-para-muitos (Um produto pode ter múltiplos registros de estoque se armazenado em diferentes locais ou se diferentes vendedores gerenciarem seu próprio estoque).
Implementação: A tabela Estoque conterá uma chave estrangeira produto_id referenciando a tabela Produto e uma chave estrangeira vendedor_id referenciando Vendedores_Parceiros para indicar o estoque específico de cada vendedor para cada produto.3

7. Cliente para Endereco (Address):
Relacionamento: Um-para-muitos (Um cliente pode ter múltiplos endereços, por exemplo, endereço de entrega e endereço de cobrança).
Implementação: Introduzir uma tabela endereco com os seguintes atributos:
endereco_id (INT, Chave Primária, Auto Incremento)
cliente_id (INT, Not Null, Chave Estrangeira referenciando Cliente)
logradouro (VARCHAR(255), Not Null)
numero (VARCHAR(20))
complemento (VARCHAR(255))
bairro (VARCHAR(100), Not Null)
cidade (VARCHAR(100), Not Null)
estado (VARCHAR(50), Not Null)
cep (VARCHAR(10), Not Null)
tipo_endereco (ENUM('Entrega', 'Cobrança', 'Outro'))
padrao (BOOLEAN): Indica se este é o endereço padrão do cliente.1
A tabela Pedido então referenciará o endereco_entrega_id desta tabela.

8. Pedido para Metodo_Pagamento (Payment Method):
Relacionamento: Muitos-para-muitos (Um pedido pode ter múltiplos métodos de pagamento, e um método de pagamento pode ser usado em múltiplos pedidos).
Implementação: Introduzir uma tabela metodo_pagamento com:
metodo_pagamento_id (INT, Chave Primária, Auto Incremento)
nome (VARCHAR(100), Not Null): O nome do método de pagamento (por exemplo, 'Cartão de Crédito', 'Boleto Bancário', 'Pix').1
Introduzir uma tabela de junção pedido_pagamento com:
pedido_id (INT, Not Null, Chave Estrangeira referenciando Pedido, Chave Primária)
metodo_pagamento_id (INT, Not Null, Chave Estrangeira referenciando Metodo_Pagamento, Chave Primária)
valor_pago (DECIMAL(10, 2), Not Null): O valor pago utilizando este método para este pedido específico.

