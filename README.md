
# Automação de testes de API com Postman

Este repositório foi fruto do curso [“Automação de testes de API com Postman”](https://www.udemy.com/course/automacao-de-testes-de-api-com-postman-projeto-de-testes) , feito através da Udemy. Nele foi possível aprender a manipular o postman construindo automações teste via interface do Postman e via integração contínua.

## 1. Workspace Vs Ambiente

Um workspace armazena um conjunto de coleções. 
Um ambiente armazena um conjunto de variáveis necessárias para a configuração necessária nas requisições de cada ambiente específico. Normalmente, se dividem entre ambiente de teste, homologação e produção. 

Os ambientes (assim como as coleções) são criados por workspace. Alterando o seu espaço de trabalho, alterará também o conjunto de coleções disponíveis e o conjunto das variáveis de ambiente, coleções e variáveis locais. 


## 2. Variáveis


### 2.1 Variável Global

Variables in this request > Globals
Pode ser acessada de qualquer ambiente e/ou collection.
Se sobrepõe às variáveis de ambiente e de coleção. 
É sobreposta pela variável local.

### 2.2 Variável de Ambiente

Setar o ambiente > Variables in this request > Environment
Pode ser acessada sempre que estiver no ambiente em questão. 
Obs: A criação de um ambiente não altera as coleções existentes no postman, e sim o conjunto de variáveis necessárias para a requisição de cada ambiente em específico. 
Se sobrepõe à variável de coleção. 
É sobreposta pela variável global e local.

![Variável de ambiente](/readme_imagens/variavel_ambiente.png)



### 2.3 Variável de Coleção 

Coleção > edit > variables
Pode ser acessada em todas as requisições da coleção. 
É sobreposta pela variável global, de ambiente e local.

![Variável de coleção](/readme_imagens/variavel_colecao.png)



### 2.4 Variável Local

Pré-script > definição da variável 
Se sobrepõe a qualquer outra variável. Só pode ser acessada na requisição em que for criada. 

`pm.variables.set("variable_key", "variable_value")`


### 2.4 Variável de Dados

Pode ser usada tanto no escopo da variável local quanto da variável de coleção. 
Também no pré-script, se define as variáveis de dados, que permitem a correção entre as variáveis setadas no body e as colunas do arquivo utilizadas no teste.

`pm.iterationData.get("nome_da_variavel")`


### 2.5 Observação geral:

Para utilizar o valor das variáveis, setar o valor entre {{ }}

![Variáveis](/readme_imagens/{{}}.png)




## 3. Scripts

O postman suporta a linguagem JavaScript. É possível escrever novos scripts através do JavaScript e/ou utilizar os scripts pré-formulados pelo postman em Snippets. 

![script](/readme_imagens/script.png)

### 3.1 Ordem de execução
Os scripts podem ser executados através das coleções e/ou  pastas e/ou requisições. 


#### 1º Script - Pré- request  
- 1.1º Pré- request - Colletion  
- 1.2º Pré- request - Folder  
- 1.3º Pré- request - Request  

#### 2º Request

#### 3º Response

#### 4º Script - Post-response  
- 4.1º Post-response - Colletion  
- 4.2º Post-response- Folder  
- 4.3º Post-response - Request  


#### Ex:
![Hierarquia](/readme_imagens/hierarquia.png)

### 3.2 Scripts em coleções: 
colletion > … > edit > Scripts  
	O script será executado antes (pré-request) ou depois (Post-response) de cada
 requisição da coleção


#### 3.2.1 Scripts em pastas: 

colletion > pasta > … > edit > Scripts
O script será executado antes (pré-request) ou depois (Post-response) de cada requisição da pasta. Ou seja, despreza as requisições existentes na coleção que não estão na pasta em que adicionou o script. 

#### 3.2.2 Scripts em requests:  

colletion > pasta > request > Scripts
O script será executado antes (pré-request) ou depois (Post-response) da requisição em questão.



#### 3.2.3 Pré-requests e Tests - sintax Postman


`pm`: Objeto que guarda informações da requisição/response;

`pm.info`: Fornece informações sobre o script que será executado, como nome, ID, contagem de interações, etc;

- `.eventName` - pega o nome do evento (ex: pré-request)

- `.iteration` - pega qual a interação em questão 
(ex: 10 chamadas da requisição, iteração 2. Ou seja, segunda chamada da requisição)

- `.iterationCount` - pega a quantidade total de iterações
(ex: 10 chamadas da requisição, 10 iterações, independente da iteração atual)

- `.requestName` - pega o nome que atribuímos a requisição após a criação

- `.requestId` - pega o ID da requisição

`pm.iterationData`: faz com que o postman utilize as colunas do arquivo de dados nas variáveis setadas na requisição.

`pm.variables`, `pm.environment`, `pm.collectionVariables`, `pm.globals`: Fornece acesso às variáveis dependendo de seu escopo, podendo setar e pegar valores;

- `.get(“nome da variável”)` - extrai valores contidos em variáveis (ex: token, url_base)

- `.set("variable_key", "variable_value")` - define nome e valor para as variáveis. 
Obs: se já tiver uma variável global, de ambiente ou coleção e sobrescrever o valor inicial com um script, será considerado o valor do script. 

- `.unset(“nome da variável”)` - exclui uma variável específica do nível setado

- `.clear()` - limpa TODAS as variáveis do nível setado.  


`pm.test`: Usado para criar scripts de teste no postman;
Valida requisições. Para isso, utiliza das demais funções: expect, response e response.to.be.

- `pm.expect`: Função para criar asserts para scripts de teste;

- `pm.response.json`: Obtém informações do json de resposta da requisição;

- `pm.response.method`: Obtém o método da requisição 

- `pm.response.to.be`: Conjunto de asserts pré definidos para testes;     



![exec_teste](/readme_imagens/exec_teste.png)

## 4. Como criar documentações  

![doc_saveResponse](/readme_imagens/doc_saveResponse.png)


### 4.1 Documentação da API 
Collection > … > View documentation ou Publish Docs
Objetivo: documentar a forma de manipulação da API


### 4.2 Documentação dos Testes
Executar uma request > Save response > Collection > … > View documentation ou Publish Docs
Objetivo: documentar os comportamentos esperados da API



## 5. Monitor
Executa as coleções periodicamente e checa a performance e respostas das requisições. É nele também que se faz a inserção de dados via arquivo CSV ou JSON para a execução das coleções.
O monitor é unico para o gerenciamento das requisições escolhidas para execução. Portanto, as definições ali setadas impactam em todo o conjunto de requisições executadas.

Collection > … > Run collection > Functional > Schedule runs


![monitor](/readme_imagens/monitor.png)

## 6. Ordem de execução entre as requests

A ordem de execução entre as requisições é sequencial. Se você quiser alterar a ordem de execução pulando parte da sequência, utilizar: 

	postman.setNextRequest(“request_name ou ID”)

Lembrando que o .info possui funções tanto para pegar o nome da request quanto seu ID.
Após a utilização do next, a ordem volta a ser sequencial, sem retornar para as requisições que foram puladas. 


Para parar um fluxo de execução, utilizar: 


	postman.setNextRequest(null)


## 7. Importar arquivo de dados

O postman suporta arquivos CSV e JSON. As variáveis de dados podem ser usadas tanto nas requisições quanto nos scripts. 

1º Criar direto na requisição e/ou script a variável de dados: {{nome_da_variavel_1}}
Não é necessário que a variável entre como uma variável de coleção ou local.

![body_variáveis](/readme_imagens/body.png)

2º Criar em pré-script um script com as variáveis utilizadas para que o postman possa reconhecer as colunas do arquivo de dados. 

	pm.iterationData.get(‘nome_da_variavel_1’)

![variavel_data](/readme_imagens/variavel_data.png)
3º Criar um arquivo .csv ou .json utilizando o nome das variáveis criadas. 
Como apresentado a seguir, isso pode ser feito em algum arquivo de edição - bloco de notas, vs code, etc… - ou através do próprio Gheets.


4º Anexe o arquivo para execução.
Collection> … > Run collection > Functional > Run manually ou Schedule runs > select file

5º O número de iterações deve passar automaticamente para a mesma quantidade de dados do arquivo. 

![exec_monitor](/readme_imagens/exec_monitor.png)


ATENÇÃO: o nome das colunas no arquivo de dados, no pré-script e nas variáveis da requisição precisam ser iguais.


### CSV

Ex formato de um arquivo CSV:

![csv](/readme_imagens/csv.png)

Ex de criação do CSV com Gheets:

![baixar_csv](/readme_imagens/baixar_csv.png)



### JSON

Ex formato de um arquivo JSON:
![json](/readme_imagens/json.png)


## 8. Newman 

O Newman é uma ferramenta que permite a execução de coleções e ambientes do Postman via linha de comando. Com ele, é possível implementar a automação dos testes via integração contínua (CI).


### 8.1 Instalação do Newman:

-  Confirmar se já possui o node.js instalado (node -v). Se não, instalá-lo.
-  Instalar globalmente o Newman e verificar sua instalação:   
    `npm install -g newman`  
    `newman -v`  
-  Instalar o módulo de geração de reports em HTML;
    
   	 `npm install -g newman-reporter-html`


### 8.2 Execução das coleções via linha de comando

#### Pré-requisitos:
- Entrar no diretório em que estão os arquivos para passar os comandos;
- Para exportar coleções: collections > coleção escolhida > ... > export;
- Para exportar variáveis de ambiente: Variables in request > All request > tipo da variável > export;
- É possível aninhar os parâmetros. Utilize todos os necessários;

Para visualizar os demais parâmetros no menu de ajuda, executar: `newman run -h`;

#### Executar uma coleção em json: 

        newman run nomeArqColecao_postman.json

#### Executar uma coleção via url: 

        newman run url_da_coelecao

#### Executar coleção com arquivo de variáveis (-e): 
        newman run nomeArqColecao_postman.json -e nomeArqVariavel.json

#### Executar uma pasta espedífica da coleção (--folder): 
        newman run nomeArqColecao_postman.json -e nomeArqVariavel.json --folder nomePasta

Obs: se quiser rodar mais de uma pasta, passar novamente o comando --folder e o nome da segunda pasta que deseja.

#### Executar um coleção com arquivo de dados (-d)
        newman run -d nomeArqDados.json nomeArqColecao_postman.json

#### Executar coleção com delay antes da chamar às requisições (--delay-request)
        newman run --delay-request quantDelay nomeArqColecao_postman.json

#### Para a execução do teste na primeira falha de assertion (--bail)
        newman run nomeArqColecao_postman.json --bail
    

#### Acrescentar novas informações sobre o tempo de preparação e execução da requisição (--verbose)
        newman run nomeArqColecao_postman.json --verbose

#### Oculta todas as informações do output ("relatório") de teste (--silent)
        newman run nomeArqColecao_postman.json --silent

#### Obs: 
Quando utilizado na integração contínuia, esse parâmetro faz com que não fiquem o registrado o output dos testes dentro do console de execução da pipeline.



## 9. Integração contínua (CI) com Postman

Integração contínua (CI) é uma prática que visa integrar o processo de build de código e testes automatizados. Objetivo: garantir que a solução está funcionando após cada atualização, de forma automatizada.

No Postman com o plano free, não é possível criar integração contínua automatizada a cada commit. Por isso, serão descritos abaixo a criação da CI para o plano free e para os demais demais plano.

### 9.1 Como executar coleções via ferramentas externas ao postman

Se tiver o plano free: 

Crie um repositório no gerenciador que preferir (github, gitlab, etc.);
Clone o repositório;
Extraia as coleções, arquivos de dados e variáveis de ambiente do Postman;
Envie para o repositório remoto os arquivos extraídos;
Crie o arquivo de CI.



### 9.2 Execução da pipeline a cada alteração no postman

Caso tenha um plano pago no postman, é possível criar a Integração Contínua a partir de cada alteração realizada no postman. Para isso, precisará criar uma chave de API.

#### 9.2.1 Como criar a API KEY no Postman:

clicar no avatar > settings (te redirecionará para o postman no browser, caso esteja local) > API Keys > Generate API Key

Atenção: salve a chave em um lugar seguro porque depois de criada, não será possível visualizar seu valor de novo. 

#### 9.2.2 Como pegar as informações da coleção com API Key:

  - 1° Obter lista de coleções:

https:api.getpostman.com/collections?apikey=$apiKey

Insira no browser a URL em questão alterando o "$apiKey" pelo valor da chave de api que criou. 
Como resposta, deve ser apresentado em tela um JSON com os dados do nome, id e uuid, etc... de todas as coleções que possui.
  
  - 2º Obter coleção pelo uid:

https:api.getpostman.com/collections/$uid?apikey=$apiKey

Com os dados que obteve na URL anterior, altera a URL em acima inserindo o uid da coleção que deseja executar e a chave da API. 
Como resposta, deve ser apresentado em tela um JSON com os dados da coleção.


  - 3º Obter ambiente:

  https:api.getpostman.com/environments?apikey=$apiKey

  Alterar o valor da apiKey e inserir a URL no browser. 
  Como resposta obterá os ambientes que possui criado, com seus respectivos UIDs.


#### 9.2.3 Como criar o link para executar a coleção através do newman.
 
Com os valores extraídos no tópico anterior, substitua os valores da url:

newman run https:api.getpostman.com/collections/$uid?apikey=$apiKey --environment https:api.getpostman.com/environments/$uid?apikey=$apiKey

  - 1º todas as %apiKey devem ser substituídas pelo valor da chave da API criada no postman  
  - 2º primeiro uid -> substituir pelo uid da coleção em questão  
  - 3º segundo uid -> substituir pelo uid do ambiente em questão

Obs: o parâmetro do ambiente como linha de comando e como link é diferente, passando de -e para --environment
  
Com o link criado, não necessitamos dos arquivos físicos para a execução das coleções.
Ao inserir esse link no terminal, independente de qual diretório esteja, a coleção em questão deve ser executada. Porém, isso só é possível se não tiver arquivo de dados junto. Se a coleção precisar do mesmo, a dependência do arquivo e da execução na pasta correta, parecem permanecer. 
