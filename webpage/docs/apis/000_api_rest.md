---
sidebar_position: 1
slug: /backend/rest-api
title: "APIs Rest"
---

Aqui vamos conversar sobre APIs (Application Programming Interfaces) e como elas são utilizadas no desenvolvimento de software, especialmente em aplicações que precisam estar conectadas com outros serviços ou sistemas.

<!-- <img src="https://64.media.tumblr.com/1410879ae6d00e77f5dbe27c03f252fc/tumblr_inline_ofgcs4tnDi1r5ight_400.gifv" alt="Gai Sensei and Rock Lee Trainning Smiles and Taijutsu"
style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}></img>
<br /> -->

## 0. **TL;DR – Pontos Essenciais sobre APIs REST**

* **REST (Representational State Transfer):** estilo arquitetônico para sistemas distribuídos, baseado em recursos expostos via HTTP e definido por Roy Fielding (2000).
* **Backend REST vs. Web Service REST:**

  * *Backend REST* é toda a infraestrutura de servidor (lógica de negócio, banco, autenticação).
  * *Web Service REST* é a interface HTTP/JSON que expõe endpoints para clientes (front-end, integrações).
* **Fluxo de comunicação (stateless):**

  1. Cliente envia requisição HTTP (GET/POST/PUT/DELETE) com todos os dados necessários.
  2. Servidor valida autenticação/autorizações.
  3. Lógica de negócio é executada (acesso a DB, filas, cache).
  4. Resposta retorna status HTTP + JSON (dados ou mensagem de erro).
* **Principais benefícios do stateless:**

  * Escalabilidade horizontal (não há sessões em memória).
  * Tolerância a falhas e balanceamento de carga simples.
  * Cada requisição é autoexplicativa e independente.
* **HTTP Semântico e Códigos de Status:**

  * *GET* para leitura, *POST* para criação, *PUT/PATCH* para atualização, *DELETE* para remoção.
  * Status 2xx (sucesso), 4xx (erros cliente), 5xx (erros servidor).
* **Modelo de Maturidade de Richardson:**

  1. **Nível 0 (POX):** RPC sobre HTTP, usa apenas POST.
  2. **Nível 1 (Recursos):** URIs distintas por entidade (e.g. `/clientes`, `/pedidos`).
  3. **Nível 2 (Verbos HTTP):** usa corretamente GET/POST/PUT/DELETE + status HTTP.
  4. **Nível 3 (HATEOAS):** respostas incluem links dinâmicos (`_links`) que guiam o cliente para próximas ações.
* **HATEOAS (Hypermedia as the Engine of Application State):**

  * API “auto-guiada”: payloads JSON vêm com links (self, atualizar, deletar, recursos relacionados).
  * Reduz acoplamento e permite descoberta dinâmica de novos endpoints.
* **Boas práticas e considerações:**

  * Versão da API (via URL ou cabeçalhos) para evoluções sem quebrar clientes.
  * Cache HTTP (ETags, Cache-Control) para performance.
  * Incluir apenas links e verbos permitidos conforme perfil de autorização do usuário.
* **Ferramentas de apoio:** Swagger/OpenAPI, Postman e bibliotecas de cliente para documentação, testes e integração contínua.


## 1. Algumas Definições

O `REST` é uma sigla para `Representational State Transfer`. Trata-se de um estilo arquitetônico para sistemas distribuídos e é frequentemente usado para desenvolver serviços web. O conceito foi introduzido e definido em 2000 por Roy Fielding em sua tese de [doutorado](https://ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf).

REST não é um protocolo ou padrão, mas sim um conjunto de princípios arquitetônicos. Quando um serviço web é descrito como "RESTful", significa que ele adota esses princípios. 

### 1.1 Backend REST

Um Backend REST refere-se à parte do servidor em uma aplicação web ou sistema de software onde a lógica de negócios é processada. É o local onde as operações CRUD (Criar, Ler, Atualizar, Deletar) são executadas, dados são processados e mantidos, e onde se define a lógica de integração com outros serviços ou bancos de dados. Este back-end é acessado por meio de uma API RESTful que expõe interfaces específicas para as funções que o frontend ou outros clientes podem utilizar.

Em outras palavras, podemos dizer que um backend REST refere-se a toda a infraestrutura de servidor que implementa a lógica de negócios e manipulação de dados de uma aplicação. O back-end REST lida com a interação direta com o banco de dados, execução de lógica de negócios, autenticação de usuários, autorização de acessos, e também serve como a plataforma sobre a qual os serviços REST são construídos. O objetivo principal é garantir que as operações de dados sejam realizadas de forma segura, eficiente e de acordo com as regras de negócio.

### 1.2 Web Service REST

Um Web Service REST, por sua vez, é um serviço específico disponibilizado através de uma API web que adere aos princípios REST. Ele permite a comunicação e interação entre diferentes softwares na internet. Cada Web Service REST é projetado para realizar tarefas específicas e é acessado por meio de URLs. Usam verbos HTTP para indicar ações e podem retornar dados em vários formatos, como JSON ou XML.

É uma interface exposta pelo back-end que permite a interação e comunicação entre diferentes sistemas (ou entre cliente e servidor) através da web utilizando o protocolo HTTP. Este serviço define um conjunto de operações que podem ser realizadas (como obter dados, atualizar informações, etc.) e é acessível através de URIs específicas. Cada Web Service REST é projetado para executar tarefas bem definidas e é projetado para ser stateless (já vamos falar desse ponto), seguindo os princípios REST.

## 2. Diferenças entre Backend REST e Web Service REST

Antes de entrarmos nas diferenças, é importante entender que tanto o Backend REST quanto o Web Service REST são componentes essenciais de uma arquitetura RESTful, mas eles têm propósitos e escopos diferentes.

Podemos citar como diferenças entre um back-end REST e um Web Service REST:

### 2.1 Escopo

- `Backend REST`: Refere-se a toda a parte do servidor em um sistema que processa lógica, manipula dados, e se integra com outros serviços. Serve como a espinha dorsal de uma aplicação, gerenciando a lógica de negócios, persistência de dados, segurança, e manipulação de todas as operações de back-end necessárias para suportar as funcionalidades da aplicação.

- `Web Service REST`: É uma parte do back-end que é especificamente projetada para ser uma interface entre diferentes sistemas, permitindo-lhes interagir e funcionar juntos. Atua como um ponto de comunicação ou interface que expõe funcionalidades específicas do back-end para serem usadas por outros sistemas ou clientes, permitindo assim a interoperabilidade entre diferentes tecnologias ou domínios.

### 2.2 Uso

- `Backend REST`: Utilizado para desenvolver a lógica interna do sistema e garantir que os dados dos usuários sejam manipulados corretamente. Inclui o servidor de aplicação, o servidor de banco de dados, implementações de lógica de negócios, gerenciamento de sessão (apesar do stateless), e segurança (autenticação e autorização). 

<details> 
<summary mdxType="summary">Exemplo de Utilização</summary>

***Tecnologias Usadas:*** Python e Django.

***Banco de Dados:*** PostgreSQL.

***Funções Implementadas:***

- Autenticação de usuários.
- Manipulação de sessões de usuário (apesar do princípio stateless, o back-end pode usar sessões para outros propósitos internos que não afetam a comunicação REST).
- Operações CRUD para tarefas no banco de dados.
- Lógica para atribuição de tarefas a usuários.

</details>



- `Web Service REST`: Usado para permitir a integração e comunicação externa entre diferentes sistemas, tanto internos quanto externos. Compreende as APIs específicas que são expostas para acessar recursos do servidor, utilizando métodos HTTP (GET, POST, PUT, DELETE) para manipular esses recursos representados geralmente em formatos como JSON ou XML.

<details> 
<summary mdxType="summary">Exemplo de Utilização</summary>

Web Service REST:

Endpoint `/tasks`:
- GET: Retorna todas as tarefas do usuário autenticado.
- POST: Permite criar uma nova tarefa com os dados fornecidos no corpo da requisição.

Endpoint `/tasks/[task_id]`:
- GET: Retorna detalhes de uma tarefa específica.
- PUT: Atualiza a tarefa com os novos dados fornecidos.
- DELETE: Remove a tarefa especificada.

</details>

### 2.3 Fluxo de Comunicação

O fluxo de comunicação entre um cliente (como um aplicativo front-end) e um Web Service REST geralmente segue os seguintes passos:

1. Cliente (Front-end) faz uma requisição HTTP para o Web Service REST.
2. Web Service REST interpreta a requisição, realiza autenticação e autorização usando os métodos definidos no back-end REST.
3. Backend REST executa as operações necessárias (e.g., buscar dados do banco, atualizar um registro).
4. Web Service REST retorna a resposta para o cliente, como um status HTTP e dados (se necessário) em formato JSON.

A interação REST entre um cliente (por exemplo, um app front-end) e um Web Service não é apenas um “vai e volta” de mensagens: ela encapsula princípios de projeto que tornam aplicações distribuídas mais escaláveis, seguras e fáceis de manter. Vamos aprofundar cada etapa e entender por que esse fluxo é tão relevante para engenheiros de computação.

#### 2.3.1. Cliente faz a requisição HTTP

* **O que ocorre**: o front-end dispara uma chamada (GET, POST, PUT, DELETE etc.) para um endpoint REST.
* **Por que importa**:

  * **Padronização**: HTTP é um protocolo universalmente suportado (navegadores, dispositivos móveis, IoT), o que garante interoperabilidade.
  * **Semântica de métodos**: usar corretamente GET para leitura, POST para criação, PUT/PATCH para atualização e DELETE para remoção segue convenções que facilitam entendimento e manutenção.
  * **Extensibilidade**: cabeçalhos, query params e corpo permitem levar metadados (autenticação, versionamento, filtros) de forma padronizada.

---

#### 2.3.2. Interpretação, autenticação e autorização

* **O que ocorre**: o servidor REST valida quem está fazendo a requisição (token, API key, OAuth) e decide se o usuário tem permissão para o recurso.
* **Por que importa**:

  * **Segurança em camadas**: separar autenticação (quem é você?) de autorização (o que você pode fazer?) protege dados sensíveis.
  * **Escalabilidade de segurança**: ao centralizar essas verificações em middlewares ou gateways, evita-se replicar lógica de proteção em cada parte do sistema.
  * **Auditoria e rastreabilidade**: logs dessas etapas ajudam a monitorar acessos indevidos e depurar falhas de permissão.

---

#### 2.3.3. Execução das operações de negócio

* **O que ocorre**: com as credenciais validadas, o back-end invoca componentes de domínio (serviços, repositórios, filas, caches) para realizar a ação solicitada.
* **Por que importa**:

  * **Separação de responsabilidades (SoC)**: camada REST cuida de comunicação; lógica de negócio fica em serviços especializados, deixando o código mais modular e testável.
  * **Escalabilidade**: operações podem ser desacopladas (por exemplo, enfileirar uma tarefa longa) sem travar a resposta ao cliente.
  * **Manutenibilidade**: padrões como Repository, Service e DTO tornam mais fácil evoluir, refatorar e escrever testes automatizados.

---

#### 2.3.4. Retorno da resposta HTTP/JSON

* **O que ocorre**: o serviço devolve um código de status (200, 201, 204, 4xx, 5xx) e, se houver payload, um corpo em JSON.
* **Por que importa**:

  * **Clareza de erro**: status HTTP padroniza sucesso (2xx), redirecionamentos (3xx), erros cliente (4xx) e erros servidor (5xx), facilitando tratamento automático de falhas no front-end.
  * **Leveza e legibilidade**: JSON é texto simples, facilmente analisado por linguagens diversas e inspeccionado em ferramentas de debug.
  * **Versionamento**: cabeçalhos como `Accept` e `Content-Type`, ou mesmo versão embutida na URL (e.g. `/v2/users`), permitem evoluir APIs sem quebrar clientes antigos.

---

#### 2.3.4. Benefícios gerais desse fluxo REST

1. **Desacoplamento**
   Cada parte (UI, API, banco de dados, serviços externos) evolui de forma independente, desde que respeite contratos (endpoints e formatos de dados).
2. **Escalabilidade horizontal**
   Servidores REST são normalmente stateless (não guardam sessão em memória), permitindo adicionar réplicas conforme a demanda.
3. **Observabilidade e monitoramento**
   Logs de requisição/resposta, métricas de latência e taxas de erro tornam mais fácil diagnosticar gargalos.
4. **Reutilização e composição**
   Uma mesma API REST pode ser consumida por web, mobile, automação, micro-frontends, bots, etc., usando o mesmo protocolo.
5. **Adoção de padrões da indústria**
   Ferramentas como Swagger/OpenAPI, Postman e bibliotecas de cliente para várias linguagens aceleram desenvolvimento e documentação.

---

## 3. Recursos, Verbos, Status Code e Stateless

### 3.1 Recursos

Em REST, "recursos" são representações de uma entidade específica ou um conjunto de entidades que são acessíveis pela API. Por exemplo, em um aplicativo de blog, os recursos podem incluir artigos, autores e comentários, cada um acessível via URIs (Uniform Resource Identifiers).

### 3.2 Verbos

Os verbos HTTP são métodos usados para realizar ações nos recursos. Os principais incluem:

- **GET:** Recuperar dados do servidor (por exemplo, listar usuários).
- **POST:** Criar um novo recurso (por exemplo, adicionar um novo usuário).
- **PUT:** Atualizar um recurso existente.
- **DELETE:** Remover um recurso.
- **PATCH:** Atualizar parcialmente um recurso.

### 3.3 Status Codes

Os códigos de status HTTP fornecem informações sobre o resultado de uma requisição feita ao servidor. Os mais comuns em APIs REST são:

- **200 OK**: A requisição foi bem-sucedida.
- **201 Created**: Um novo recurso foi criado.
- **400 Bad Request**: A requisição não foi processada devido a erro do cliente.
- **404 Not Found**: O recurso solicitado não foi encontrado.
- **500 Internal Server Error**: Erro genérico quando o servidor falha.

Para ver todos os códigos de status HTTP, consulte a [documentação oficial](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status).

:::danger[Versão Pets]
- https://http.cat/
- https://httpstatusdogs.com/
:::

## 3.4 Stateless:

“O princípio de **stateless** (sem estado) na arquitetura REST vai além de simplesmente não guardar informações entre requisições — ele define um **contrato claro** entre cliente e servidor, em que toda chamada traz **tudo que precisa** para ser processada. Veja como esse conceito se manifesta e por que ele é tão valioso para sistemas distribuídos:

### 3.4.1. Autossuficiência das requisições

   * Cada mensagem HTTP (método, URI, headers e corpo) carrega **dados de autenticação**, parâmetros de busca, informações de sessão (por exemplo, tokens JWT) e o payload necessário.
   * O servidor trata cada chamada como um **evento isolado**, sem recorrer a memórias ou caches internos para “lembrar” quem é o usuário ou em que etapa do fluxo ele se encontra.

### 3.4.2. Vantagens arquiteturais

   * **Escalabilidade horizontal**: novos nós da aplicação podem ser adicionados ou removidos sem sincronizar estados de sessão — basta compartilhar o mesmo código e acessar o banco de dados ou serviço de autenticação centralizado.
   * **Tolerância a falhas**: se um servidor morre ou é reiniciado, nenhuma informação de contexto se perde, minimizando downtime e evitando “sessões perdidas”.
   * **Simplicidade no balanceamento de carga**: requests podem ser redirecionados livremente entre instâncias, pois não existe dependência de um servidor específico para continuar uma conversa.

### 3.4.3. Responsabilidade do cliente

   * **Manter contexto local**: cabe ao front-end (ou outro consumidor da API) armazenar identificadores (por exemplo, ID de usuário, tokens expiráveis) e remontar o contexto de interação.
   * **Reenvio de informações sensíveis**: cabeçalhos de autorização e dados de controle devem ser enviados em cada requisição, evitando “sessões mágicas” que o servidor inferiria.

### 3.4.4. Implicações em segurança e monitoramento

   * **Tokens como portadores de estado**: como não há memória de sessão, informações de identidade e permissões vêm encapsuladas em tokens digitais (JWT, OAuth), que podem ser validados de forma independente por cada instância.
   * **Auditabilidade**: cada requisição carrega um “rastro” completo do que foi solicitado e por quem, facilitando o tracing e a análise de logs, sem necessidade de juntar pedaços de informação dispersos.

### 3.4.5. Desafios e boas práticas

   * **Payload crescente**: armas necessárias na requisição (por exemplo, autorização, filtros, paginação) podem tornar cabeçalhos e corpos maiores; é preciso equilibrar completude e eficiência.
   * **Cache inteligente**: como o servidor não mantém contexto de cache de sessão, APIs devem explorar mecanismos de cache HTTP (ETags, Cache-Control) para reduzir latência sem violar o princípio stateless.
   * **Versionamento de API**: sem dependência de estado, atualizações na estrutura de dados ou nos endpoints (ex.: `/v1/recursos` → `/v2/recursos`) podem ser gerenciadas de forma mais suave, mantendo compatibilidade com clientes legados.

Ser stateless é adotar um **modelo de comunicação puro**, no qual cada requisição é autoexplicativa e independente. Isso traz simplicidade, permite operar em larga escala e garante que a lógica de negócio permaneça coesa e fácil de replicar.

---

> "Murilão, até aqui li um manual de REST, mas não entendi por que estamos tendo essa discussão agora. Isso já não é o básico de qualquer API REST?"

Vamos lá! Você está certo! Estes conceitos são baselares para a construção de APIs RESTful. Por isso mesmo, é importante que eles fiquem bem claros e estabecidos para avançarmos. Agora vamos avaliar como podemos verificar o grau de maturidade de uma API.

## 4.Modelo de Maturidade de Richardson

O Modelo de Maturidade de Richardson é uma ferramenta útil para avaliar a conformidade de uma API com os princípios REST. Desenvolvido por Leonard Richardson, este modelo decompõe os elementos cruciais que definem uma API RESTful em quatro níveis de maturidade. Cada nível adiciona novos elementos sobre os anteriores, aumentando a aderência aos princípios REST. A ideia é que, ao seguir este modelo, desenvolvedores possam criar serviços web mais robustos, escaláveis e flexíveis.

## 5.Níveis do Modelo

### 5.1.Nível 0: POX (Plain Old XML)

O HTTP é utilizado apenas como meio de transporte. Neste nível, a API é basicamente apenas um mecanismo remoto de chamada de procedimento (RPC). Ou seja, não aproveita as características inerentes do protocolo HTTP além de simplesmente enviar e receber dados.

As mensagens são enviadas por XML ou qualquer outro formato, mas não usa recursos HTTP como HTTP verbs ou HTTP status codes. Em geral, todas as operações são realizadas geralmente via POST, sem diferenciar o tipo de ação realizada sobre o recurso.

A API neste nível geralmente expõe apenas um único URI que recebe todas as requisições, que são diferenciadas pelo corpo da requisição.

:::info[Qual a relevância?]

- `Facilidade de Implementação:` Muito fácil de implementar, especialmente para sistemas legados ou quando integrado a sistemas que não são baseados em web.

- `Principais Limitações:` Falta de escalabilidade, flexibilidade e dificuldades na manutenção, pois não utiliza a plena capacidade do HTTP.

:::

### 5.2.Nível 1: Recursos

No nível 1, o sistema começa a definir recursos com URIs dedicadas. Por exemplo, separando `/clientes` para acessos relacionados a clientes e `/pedidos` para pedidos. 

Geralmente, ainda usa POST para todas as operações, não diferenciando entre tipos de ação (criar, ler, atualizar, deletar). Por exemplo: 
- POST /clientes para criar um novo cliente; 
- POST /clientes/123 para atualizar o cliente com ID 123.

:::info[Qual a relevância?]

- `Organização Melhorada:` Facilita a organização da API e a compreensão dos tipos de entidades que a API manipula, melhorando um pouco a manutenibilidade.

- `Ainda Não É RESTful:` Não aproveita o protocolo HTTP para representar operações CRUD através de verbos como GET, PUT, e DELETE.

:::

### 5.3.Nível 2: Verbos HTTP

Este nível utiliza os verbos HTTP (GET, POST, PUT, DELETE) para definir ações sobre os recursos, alinhando melhor com os princípios REST. Em geral temos como operações padrão:
- `GET` para recuperar recursos.
- `POST` para criar novos recursos.
- `PUT` para atualizar recursos existentes.
- `DELETE` para remover recursos.

Como exemplo: 
- `GET /clientes/123` para obter informações do cliente 123;
- `DELETE /clientes/123` para deletar esse cliente.

Este nível também utiliza códigos de status HTTP para comunicar o resultado das operações ao cliente, como 200 OK, 201 Created, e 404 Not Found.


:::info[Qual a relevância?]

- `Aproveitamento do HTTP:` Utiliza o protocolo HTTP como foi destinado, o que facilita a interoperabilidade e a integração com outras interfaces web e clientes HTTP.

- `Melhor Feedback para o Cliente:` Os códigos de status informam os clientes sobre o sucesso ou falha das operações, permitindo reações apropriadas do lado do cliente.

:::

### 5.4.Nível 3: HATEOAS (Hypermedia as the Engine of Application State)

O último nível adiciona controles de hypermedia, que incluem links dentro das respostas que orientam o cliente sobre as ações possíveis subsequentes.

As respostas são auto-descritivas, com links para métodos relacionados (por exemplo, link para deletar um cliente na representação de um cliente). A aplicação do cliente muda de estado exclusivamente através das hyperlinks fornecidos dinamicamente pela aplicação do servidor.

Por exemplo, uma resposta para `GET /clientes/123` poderia incluir links para `PUT /clientes/123` e `DELETE /clientes/123`, e talvez links para recursos relacionados como `/clientes/123/pedidos`.

:::info[Qual a relevância?]

- `Descoberta Dinâmica:` Permite que os clientes da API descubram dinamicamente outras funcionalidades da API, reduzindo o acoplamento entre cliente e servidor e melhorando a escalabilidade e a flexibilidade da aplicação.

- `Verdadeiramente RESTful:` Alcançar este nível significa que a API é totalmente RESTful, aproveitando todos os benefícios do protocolo HTTP e seguindo os princípios de design REST de maneira completa.

:::

Sugestão de Material Complementar (créditos ao [rmnicola](https://github.com/rmnicola) por ter sugerido o vídeo):

<iframe width="600" height="480" max-width="80vw" src="https://www.youtube.com/embed/x7v6SNIgJpE?si=whTCgt28o5i5nqdY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style={{display: 'block', marginLeft: 'auto', maxHeight: '80vh', marginRight: 'auto', marginBottom: '16px'}}></iframe>


A arquitetura REST, para além de simplesmente expor recursos e suportar operações CRUD, pode elevar o nível de maturidade por meio do **HATEOAS** (Hypermedia As The Engine Of Application State). Veja a seguir uma descrição mais detalhada, com exemplos e implicações práticas:

---

#### 5.4.1. O que é HATEOAS?

HATEOAS é um princípio em que **as próprias respostas do servidor contêm os “próximos passos”** que o cliente pode seguir. Em vez de o front-end ter lógica fixa de “se quero apagar um cliente, monto eu mesmo a URL e o método DELETE”, o servidor **guia** o cliente, incluindo links nas suas representações.

- Exemplo de payload JSON com HATEOAS

```json
GET /clientes/123
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "nome": "Murilo Carvalho",
  "email": "murilo@example.com",
  "saldo": 2500.00,

  "_links": {
    "self":      { "href": "/clientes/123",       "method": "GET"    },
    "atualizar": { "href": "/clientes/123",       "method": "PUT"    },
    "deletar":   { "href": "/clientes/123",       "method": "DELETE" },
    "pedidos":   { "href": "/clientes/123/pedidos","method": "GET"   }
  }
}
```

Aqui, além dos dados do cliente, o objeto `_links` descreve:

* **self**: confirma a URL atual e o verbo HTTP para obtê-la.
* **atualizar**: mostra como modificar esse mesmo recurso.
* **deletar**: mostra como removê-lo.
* **pedidos**: indica recurso relacionado, permitindo consulta de pedidos do cliente.

---

#### 5.4.2. Como funciona na prática

1. **Estado da aplicação guiado por hipermídia**
   Cada mudança de “tela” ou “contexto” no cliente é disparada **pelos links recebidos** — assim, o cliente não precisa construir URIs nem gerenciar rotas internamente.
2. **Descoberta dinâmica de recursos**
   Se amanhã você adicionar um novo recurso `/clientes/123/enderecos`, o servidor poderia incluir um link `"enderecos"` sem que o front-end precise ser reprogramado para descobrir esse endpoint.
3. **Contrato evolutivo e desacoplamento**
   O cliente e o servidor conversam por esse “dialeto” de links e ações: enquanto o servidor respeitar esse formato, qualquer cliente que saiba ler `_links` funciona sem depender de documentação externa.

---

#### 5.4.3. Benefícios para engenharia de computação

| Benefício                   | Detalhamento                                                                                               |
| --------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Baixo acoplamento**       | Cliente não hard-codeia URIs nem lógica de navegação; ações mudam conforme o servidor evolui.              |
| **Flexibilidade**           | Inclusão de novos relacionamentos ou operações sem quebrar clientes antigos.                               |
| **Testabilidade**           | Testes de integração podem validar não só dados, mas também a existência e conformidade dos links HATEOAS. |
| **Manutenção simplificada** | Documentação básica — o próprio payload ensina o cliente sobre possíveis interações.                       |

---

#### 5.4.4. Desafios e boas práticas

1. **Padronizar o formato dos links**

   * Use um objeto único (por exemplo, `_links`) e siga convenções como as do **JSON\:API** ou **HAL** para maximizar compatibilidade com clientes genéricos.
2. **Limitar verbos e recursos expostos**

   * Exponha apenas os links que fizerem sentido no contexto atual, evitando “poluição” de hipermídia.
3. **Cache e performance**

   * Como o payload cresce com links, avalie compressão (gzip) e use cabeçalhos HTTP de cache (`ETag`, `Cache-Control`) para não penalizar demasiadamente a troca de dados.
4. **Segurança**

   * Verifique por profiling de links: só inclua operações permitidas pelo perfil de autorização do usuário, evitando expor endpoints que gerariam erro 403.

---

#### 5.4.5. Por que adotar HATEOAS?

* **API verdadeiramente evolutiva**: você pode trocar rotas, versões ou até mesmo hosts, desde que os links corretos sejam gerados.
* **Menos “surpresa”**: o cliente sempre sabe o que é possível fazer, porque a resposta já traz o mapa de navegação.
* **Experiência próxima à Web**: assim como navegadores exploram páginas HTML pelos hyperlinks, clientes REST “navegam” pela sua API.

HATEOAS torna a API **auto-descritiva** e **autodirigida**, promovendo uma arquitetura mais resiliente e adaptável.



## 6. Considerações sobre o uso do Modelo

Este modelo não só ajuda a construir APIs que verdadeiramente aproveitam a arquitetura da Web mas também promove práticas que podem aumentar a flexibilidade, escalabilidade e a manutenibilidade da aplicação. APIs que alcançam o Nível 3 são consideradas verdadeiramente RESTful, pois utilizam a web (com seus hyperlinks e métodos HTTP) como plataforma de desenvolvimento de aplicações.

Ao projetar APIs RESTful, atingir o Nível 3 do Modelo de Maturidade de Richardson significa que a API está bem equipada para evoluir. Links e ações sugeridas nas respostas permitem que o cliente de API descubra dinamicamente outras ações possíveis, reduzindo o acoplamento entre cliente e servidor e facilitando mudanças e expansões futuras.

A aplicação desses princípios ajuda a garantir que a API pode ser usada de maneira intuitiva e eficaz, aproveitando plenamente os protocolos web existentes e a infraestrutura de internet. Em resumo, o Modelo de Maturidade de Richardson fornece um caminho claro para a evolução técnica das APIs.

Contudo, cabe destacar que o Modelo de Maturidade de Richardson é uma ferramenta útil para avaliar o quão bem uma API segue os princípios REST. No entanto, é importante lembrar que a aderência estrita a todos os níveis do modelo pode não ser necessária ou prática em todos os casos. A aplicação do modelo deve ser feita de forma consciente, considerando as necessidades e restrições específicas de cada projeto.

## 7. O que é o nível 2?

O nível 2 é o segundo nível de maturidade de uma API REST, de acordo com o modelo de maturidade de Richardson. Neste nível, a API passa a utilizar recursos de HTTP para representar as operações da API. Isso significa que a API passa a utilizar os métodos HTTP (GET, POST, PUT, DELETE, etc.) para representar as operações de leitura, criação, atualização e exclusão de recursos.

Em geral, esse nível é alcançado quando a API passa a utilizar os métodos HTTP de forma correta e consistente, de acordo com as convenções da arquitetura REST. Isso significa que a API passa a utilizar os métodos HTTP de forma padronizada, de acordo com as operações que estão sendo realizadas.	

:::danger[Atenção]

<img src="https://i.pinimg.com/originals/c3/ef/e3/c3efe3c72dc3a0d598735ca29822e80a.gif" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto' }}/>

Sempre tentar entender qual a necessidade do projeto. Muitas vezes um projeto pode não chegar ao nível 3, mas isso não significa que ele é ruim. O nível 2 é um nível de maturidade que já é bastante avançado e que atende a maioria das necessidades de uma API REST.

:::

## 8. Relembrar é viver!!

Aqui vai um *remeber* de como construir uma API utilizando autenticação e utilizando Flask. Utilize ele como base se você tiver alguma dúvida de como iniciar o processo.

## 9. Criando uma aplicação com Autenticação e Flask v1.0

O objetivo deste projeto é criar uma aplicação que é executada no servidor Flask, com autenticação de usuário. Ela será uma aplicação básica, com o objetivo de apresentar como criar uma aplicação com autenticação de usuário.

### 9.1 Requisitos

- Python >= 3.8
- Flask
- Flask-SQLAlchemy
- Docker

### 9.2 Recomendação de Leitura

- [Flask](https://flask.palletsprojects.com/en/2.3.x/)
- [Flask Quickstart](https://flask.palletsprojects.com/en/2.3.x/quickstart/#a-minimal-application)
- [SQLAlchemy](https://www.sqlalchemy.org/)
- [Flask SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/3.1.x/)
- [Data Management With Python, SQLite, and SQLAlchemy](https://realpython.com/python-sqlite-sqlalchemy/#working-with-sqlalchemy-and-python-objects)



### 9.3 Instalação

As bibliotecas necessárias para a execução do projeto estão no arquivo [`requirements.txt`](https://github.com/Murilo-ZC/Templates-Desenvolvimento/blob/main/autenticacao-flask/requirements.txt). Para instalar, execute o comando abaixo:

```bash
python -m pip install -r requirements.txt
```

> ***ATENÇÃO:*** *É recomendado a utilização de um ambiente virtual para a instalação das bibliotecas. Para mais informações, acesse o [link](https://docs.python.org/pt-br/3/library/venv.html).*


Para criar um ambiente virtual, execute o comando abaixo (para Windows):

```bash
python -m venv .
cd Scripts
activate
```

O que vai acontecer com a sequencia de comandos acima, um ambiente virtual será criado na pasta atual. Em sequencia, navegamos para o diretório ***Scripts***, e ativamos o ambiente virtual executando o script ***activate***. Na sequencia, vamos avaliar se o ambiente virtual foi ativado corretamente, executando o comando abaixo:

```bash
where python
```

A saída esperada é a seguinte:

```bash
C:\Users\usuario\Documents\autenticacao-flask\Scripts\python.exe
C:\Users\usuario\AppData\Local\Programs\Python\Python38\python.exe
```

Os diretórios que são criados para o ambiente virtual são:
- Include
- Lib
- Scripts

Esses diretórios e o arquivo ***pyvenv.cfg*** são criados na pasta onde o comando ***python -m venv .*** foi executado. Eles podem ser adicionados ao ***.gitignore***, pois se for necessário recriar esses diretórios, basta recriar o venv. Exemplo de gitignore:

```gitignore
Include
Lib
Scripts
pyvenv.cfg
```

Para desativar o ambiente virtual, execute o comando abaixo, dentro do diretório Scripts:

```bash
deactivate
```

### 9.4 Desenvolvimento do Projeto

Depois de criado o ambiente virtual, vamos criar uma aplicação básica padrão de Flask, apenas para verificarmos a instalação do sistema. Dentro do diretório ***"src"***, crie um arquivo chamado ***"main.py"***. Dentro deste arquivo, vamos inserir o código abaixo:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"

```

Agora vamos executar a aplicação:
    
```bash
python -m flask --app src.main run
```

Agora, utilizando o ~~Thunber Client~~Insomnia, vamos acessar a URL ***http://localhost:5000***. A saída esperada é a seguinte:

```html
<p>Hello, World!</p>
```

Até aqui temos o exemplo de introdução do Flask. Agora vamos criar as rotas para implementar um CRUD de usuários. Por hora, nossa aplicação vai conversar com um banco de dados SQLite. Para isso, vamos criar um diretório chamado ***"database"***, e dentro dele, vamos criar um arquivo chamado ***"database.py"***. Vamos utilizar o SQLAlchemy para fazer a conexão com o banco de dados. Para isso, vamos instalar a biblioteca SQLAlchemy, executando o comando abaixo:

```python
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
  pass

db = SQLAlchemy(model_class=Base)
```

Agora, vamos criar o arquivo ***"models.py"***, dentro do diretório ***"database"***. Dentro deste arquivo, vamos criar a classe ***"User"***, que vai representar a tabela de usuários do banco de dados. O código abaixo representa a classe ***"User"***:

```python
from sqlalchemy import Column, Integer, String
from database.database import db

class User(db.Model):
  __tablename__ = 'users'

  id = Column(Integer, primary_key=True, autoincrement=True)
  name = Column(String(50), nullable=False)
  email = Column(String(50), nullable=False)
  password = Column(String(50), nullable=False)

  def __repr__(self):
    return f'<User:[id:{self.id}, name:{self.name}, email:{self.email}, password:{self.password}]>'
  
  def serialize(self):
    return {
      "id": self.id,
      "name": self.name,
      "email": self.email,
      "password": self.password}
```

Agora, vamos ligar nosso modelo com a aplicação Flask. Para isso, vamos alterar o arquivo ***"main.py"***, dentro do diretório ***"src"***. Dentro deste arquivo, vamos inserir o código abaixo:

```python
from flask import Flask
from database.database import db
from flask import jsonify, request
from database.models import User

app = Flask(__name__)
# configure the SQLite database, relative to the app instance folder
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///project.db"
# initialize the app with the extension
db.init_app(app)

# Verifica se o parâmetro create_db foi passado na linha de comando
import sys
if len(sys.argv) > 1 and sys.argv[1] == 'create_db':
    # cria o banco de dados
    with app.app_context():
        db.create_all()
    # Finaliza a execução do programa
    print("Database created successfully")
    sys.exit(0)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

Agora, vamos criar o banco de dados. Para isso, vamos executar o comando abaixo:

```bash
python src/main.py create_db
```

> ***IMPORTANTE:*** Para que a criação das tabelas possa ser executada com sucesso, é necessário importar todas as classes que implementam a classe base no programa. Caso contrário, as demais tabelas não serão criadas.

A execução do programa vai criar as tabelas no banco de dados. O arquivo do banco de dados será criado em ***var/main-instance/project.db***. Agora, vamos criar as rotas para o CRUD de usuários. Para isso, vamos alterar o arquivo ***"main.py"***, dentro do diretório ***"src"***. Dentro deste arquivo, vamos inserir o código abaixo:

```python
from flask import Flask
from database.database import db
from flask import jsonify, request
from database.models import User

app = Flask(__name__)
# configure the SQLite database, relative to the app instance folder
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///project.db"
# initialize the app with the extension
db.init_app(app)

# Verifica se o parâmetro create_db foi passado na linha de comando
import sys
if len(sys.argv) > 1 and sys.argv[1] == 'create_db':
    # cria o banco de dados
    with app.app_context():
        db.create_all()
    # Finaliza a execução do programa
    print("Database created successfully")
    sys.exit(0)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"

# Adicionando as rotas CRUD para a entidade User
@app.route("/users", methods=["GET"])
def get_users():
    users = User.query.all()
    return_users = []
    for user in users:
        return_users.append(user.serialize())
    return jsonify(return_users)

@app.route("/users/<int:id>", methods=["GET"])
def get_user(id):
    user = User.query.get(id)
    return jsonify(user.serialize())

@app.route("/users", methods=["POST"])
def create_user():
    data = request.json
    user = User(name=data["name"], email=data["email"], password=data["password"])
    db.session.add(user)
    db.session.commit()
    return jsonify(user.serialize())

@app.route("/users/<int:id>", methods=["PUT"])
def update_user(id):
    data = request.json
    user = User.query.get(id)
    user.name = data["name"]
    user.email = data["email"]
    user.password = data["password"]
    db.session.commit()
    return jsonify(user.serialize())

@app.route("/users/<int:id>", methods=["DELETE"])
def delete_user(id):
    user = User.query.get(id)
    db.session.delete(user)
    db.session.commit()
    return jsonify(user.serialize())
```

Agora, vamos avaliar as rotas:

- GET /users: Retorna todos os usuários cadastrados no banco de dados
- GET /users/`{int:id}`: Retorna o usuário com o id informado
- POST /users: Cria um novo usuário
- PUT /users/`{int:id}`: Atualiza o usuário com o id informado
- DELETE /users/`{int:id}`: Deleta o usuário com o id informado

Agora, vamos testar as rotas. Para isso, vamos executar o comando abaixo, dentro do diretório ***"src"***:

```bash
python -m flask --app main run
```

E testamos as rotas com o ~~Thunder Client~~Insomnia. Para isso, vamos acessar a URL ***http://localhost:5000/users***. Cada vez que o método **POST** for utilizado, um novo registro será criado no banco de dados. Para testar o método **POST**, vamos utilizar o ~~Thunder Client~~Insomnia. Para isso, vamos acessar a URL ***http://localhost:5000/users***, e vamos inserir o seguinte JSON:

```json
{
  "name": "Teste",
  "email": "mail@mail.com",
    "password": "123456"
}
```

Agora vamos adicionar nossa página de login, onde o usuário vai informar o email e a senha. Para isso, vamos criar o arquivo ***"login.html"***, dentro do diretório ***"templates"***. Dentro deste arquivo, vamos inserir o código abaixo:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Auth</title>
</head>
<body>
    <h1>Login</h1>
    <form action="http://localhost:5000/login" method="POST">
        <label for="username">Username</label>
        <input type="text" name="username" id="username" required>
        <label for="password">Password</label>
        <input type="password" name="password" id="password" required>
        <input type="submit" value="Login">
    </form>
    <p>Don't have an account? <a href="http://localhost:5000/user-register">Register</a></p>    
</body>
</html>
```

E vamos alterar o arquivo ***"main.py"***, dentro do diretório ***"src"***. Dentro deste arquivo, vamos inserir o código abaixo:

```python
from flask import Flask
from database.database import db
from flask import jsonify, request, render_template
from database.models import User

app = Flask(__name__, template_folder="templates")
# configure the SQLite database, relative to the app instance folder
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///project.db"
# initialize the app with the extension
db.init_app(app)

# Código anterior suprimido para facilitar a localização nos códigos

@app.route("/user-login", methods=["GET"])
def user_login():
    return render_template("login.html")
```

Agora nossa página de login está pronta. Vamos criar a página de registro. Para isso, vamos criar o arquivo ***"register.html"***, dentro do diretório ***"templates"***. Dentro deste arquivo, vamos inserir o código abaixo:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Register User</title>
</head>
<body>
    <h1>Register User</h1>
    <form action="http://localhost:5000/register" method="POST">
        <label for="username">Username</label>
        <input type="text" name="username" id="username" required>
        <label for="password">Password</label>
        <input type="password" name="password" id="password" required>
        <input type="submit" value="Register">
    </form>
    <p>Already have an account? <a href="http://localhost:5000/user-login">Login</a></p>
</body>
</html>
```

Agora, ajustamos o arquivo ***"main.py"***, dentro do diretório ***"src"***. Dentro deste arquivo, vamos inserir o código abaixo:

```python
# Código superior suprimido para facilitar a localização nos códigos
@app.route("/user-register", methods=["GET"])
def user_register():
    return render_template("register.html")
```

E por fim, vamos criar nossa página que será protegida por autenticação e a página que indica que uma falha aconteceu. Para a página de conteúdo protegido, vamos criar o arquivo ***"content.html"***, dentro do diretório ***"templates"***. Dentro deste arquivo, vamos inserir o código abaixo:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Conteúdo Secreto</title>
</head>
<body>
    <h1>Conteúdo Secreto</h1>
    <img src="https://picsum.photos/300" width="300" height="300"/>
    <button onClick="window.location.reload();">Refresh Page</button>
</body>
</html>
```

E para a página de erro, vamos criar o arquivo ***"error.html"***, dentro do diretório ***"templates"***. Dentro deste arquivo, vamos inserir o código abaixo:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Something went wrong</title>
</head>
<body>
    <h1>Algo deu errado</h1>
    <img src="http://placekitten.com/300/300" width="300" height="300"/>
    <p>Vamos novamente? <a href="http://localhost:5000/user-login">Home</a></p>
</body>
</html>
```

E em nosso código no arquivo ***"main.py"***, dentro do diretório ***"src"***, vamos inserir o código abaixo:

```python
# Código anterior suprimido
@app.route("/content", methods=["GET"])
def content():
    return render_template("content.html")

@app.route("/error", methods=["GET"])
def error():
    return render_template("error.html")
```

Agora para lidar com a autenticação dos usuários, vamos utilizar a biblioteca ***JWTManager*** do ***flask_jwt_extended***. Para isso, vamos adicionar a biblioteca no arquivo ***"requirements.txt"***. Agora, vamos atualizar nosso arquivo ***"main.py"***, dentro do diretório ***"src"***. Dentro deste arquivo, vamos inserir o código abaixo:

```python
# Código anterior suprimido para facilitar a localização nos códigos

from flask_jwt_extended import JWTManager, set_access_cookies

app = Flask(__name__, template_folder="templates")
# configure the SQLite database, relative to the app instance folder
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///project.db"
# initialize the app with the extension
db.init_app(app)
# Setup the Flask-JWT-Extended extension
app.config["JWT_SECRET_KEY"] = "goku-vs-vegeta" 
# Seta o local onde o token será armazenado
app.config['JWT_TOKEN_LOCATION'] = ['cookies']
jwt = JWTManager(app)

# Restante do código foi suprimido para facilitar a localização nos códigos
```

Agora, vamos realizar a atualização do nosso arquivo fonte para gerar as chaves JWT de autenticação, quando um usuário solicitar. ***IMPORTANTE:*** este método vai verificar o usuário no banco de dados e então gerar a chave JWT caso o usuário e a senha do usuário estejam corretos. Para isso, vamos alterar o arquivo ***"main.py"***, dentro do diretório ***"src"***. Dentro deste arquivo, vamos inserir o código abaixo:

```python
# Código anterior suprimido para facilitar a localização nos códigos

# Método para criar um token
from flask_jwt_extended import create_access_token
from flask_jwt_extended import jwt_required, get_jwt_identity
@app.route("/token", methods=["POST"])
def create_token():
    username = request.json.get("username", None)
    password = request.json.get("password", None)
    # Query your database for username and password
    user = User.query.filter_by(email=username, password=password).first()
    if user is None:
        # the user was not found on the database
        return jsonify({"msg": "Bad username or password"}), 401
    
    # create a new token with the user id inside
    access_token = create_access_token(identity=user.id)
    return jsonify({ "token": access_token, "user_id": user.id })

# Restante do código foi suprimido para facilitar a localização nos códigos
```

Agora, para proteger as rotas, utilizar o decorador ***jwt_required***. Para isso, vamos alterar o arquivo ***"main.py"***, dentro do diretório ***"src"***. Dentro deste arquivo, vamos inserir o código abaixo:

```python
# Código anterior suprimido para facilitar a localização nos códigos

from flask_jwt_extended import jwt_required, get_jwt_identity

@app.route("/content", methods=["GET"])
@jwt_required()
def content():
    return render_template("content.html")

# Restante do código foi suprimido para facilitar a localização nos códigos
```

Assim, quando essa rota for acessada sem a chave de usuário, ela não será carregada.
Agora vamos ajustar o comportamento da rota de login.

```python	
import requests as http_request

# Código anterior suprimido para facilitar a localização nos códigos

@app.route("/login", methods=["POST"])
def login():
    username = request.form.get("username", None)
    password = request.form.get("password", None)
    # Verifica os dados enviados não estão nulos
    if username is None or password is None:
        # the user was not found on the database
        return render_template("error.html", message="Bad username or password")
    # faz uma chamada para a criação do token
    token_data = http_request.post("http://localhost:5000/token", json={"username": username, "password": password})
    if token_data.status_code != 200:
        return render_template("error.html", message="Bad username or password")
    # recupera o token
    response = make_response(render_template("content.html"))
    set_access_cookies(response, token_data.json()['token'])
    return response

# Restante do código foi suprimido para facilitar a localização nos códigos
```

## 10. O que é OpenAPI?

O Swagger é uma ferramenta de código aberto que ajuda os desenvolvedores a projetar, criar, documentar e consumir serviços da Web RESTful. Ele fornece uma interface gráfica que facilita a documentação de APIs RESTful. O Swagger é uma especificação de API para descrever APIs RESTful expressas usando JSON. O Swagger é usado junto com um conjunto de ferramentas de software de código aberto para projetar, construir, documentar e usar APIs RESTful.

Ele foi criado pela SmartBear Software em 2011 e foi adotado pela OpenAPI Initiative em 2015. O padrão OpenAPI é agnóstico de linguagem de programação e é suportado por várias ferramentas de software de código aberto, incluindo o Swagger. Diversas APIs documentadas com o Swagger são listadas no portal [SwaggerHub](https://app.swaggerhub.com/home).


<details>
    <summary>Tem vídeo?</summary>
    <iframe width="600" height="480" max-width="80vw" src="https://www.youtube.com/embed/3nl9AzttzBQ?si=UDuMd64vQn2u2wSI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style={{display: 'block', marginLeft: 'auto', maxHeight: '80vh', marginRight: 'auto', marginBottom: '16px'}}></iframe>

</details>

Uma grande vantagem do uso do Swagger é que ele permite criar a documentação antes mesmo de implementar a API. Isso é possível porque o Swagger é uma especificação de API que descreve a estrutura de uma API RESTful. A especificação é escrita em JSON ou YAML e é usada para descrever a estrutura de uma API RESTful. A especificação é usada para gerar a documentação da API, que é exibida em um formato legível por humanos. A documentação da API é usada para ajudar os desenvolvedores a entender como usar a API e para ajudar os consumidores da API a entender como a API funciona.

Para realizar a documentação da API, podemos utilizar o Swagger Editor, que é uma ferramenta de código aberto que permite escrever e visualizar a especificação da API em tempo real. O link para acessar o editor pode ser visto aqui: [Swagger Editor](https://editor.swagger.io/).

## 11. Implementando o Swagger

Vamos primeiro implementar o seguinte código da API:

```yaml
openapi: 3.0.0
info:
  title: API de Gerenciamento de Usuários
  description: Esta é uma API simples para criar, consultar e deletar usuários.
  version: "1.0.0"
servers:
  - url: 'https://api.seusite.com'
    description: API de Produção

paths:
  /criar:
    post:
      summary: Cria um novo usuário
      operationId: criarUsuario
      tags:
        - Usuários
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - nome
                - email
              properties:
                nome:
                  type: string
                  example: João da Silva
                email:
                  type: string
                  example: joao.silva@example.com
      responses:
        '201':
          description: Usuário criado com sucesso
          content:
            application/json:
              schema:
                type: object
                properties:
                  id:
                    type: string
                    example: '123'
        '400':
          description: Informação insuficiente para criar o usuário
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorModel'
        '405':
          description: Método não permitido
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorModel'

  /consultar/{id}:
    get:
      summary: Consulta um usuário pelo ID
      operationId: consultarUsuario
      tags:
        - Usuários
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Dados do usuário
          content:
            application/json:
              schema:
                type: object
                properties:
                  nome:
                    type: string
                    example: João da Silva
                  email:
                    type: string
                    example: joao.silva@example.com
        '404':
          description: Usuário não encontrado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorModel'
        '405':
          description: Método não permitido
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorModel'

  /deletar/{id}:
    delete:
      summary: Deleta um usuário pelo ID
      operationId: deletarUsuario
      tags:
        - Usuários
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Usuário deletado com sucesso
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                    example: 'ok'
        '404':
          description: Usuário não encontrado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorModel'
        '405':
          description: Método não permitido
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorModel'

components:
  schemas:
    ErrorModel:
      type: object
      properties:
        mensagem:
          type: string
          example: 'Método não permitido'

```

Repare que desta forma, nós conseguimos criar a documentação e já estabelecer como nossa API vai se comportar, mesmo sem ter implementado ela. Isso é muito útil para que possamos ter uma visão geral de como utilizar a API para fazer integrações.

Para observar:

- Qual o grau de maturidade da API?
- Quais são os endpoints disponíveis?
- Quais são os métodos disponíveis?


