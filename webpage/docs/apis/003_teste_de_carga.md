---
sidebar_position: 4
slug: /backend/teste-de-carga
title: "Teste de Carga"
---
<!-- 
## TO-DO

<img src="https://64.media.tumblr.com/1410879ae6d00e77f5dbe27c03f252fc/tumblr_inline_ofgcs4tnDi1r5ight_400.gifv" alt="Gai Sensei and Rock Lee Trainning Smiles and Taijutsu"
style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}></img>
<br /> -->

Pessoal agora vamos pensar em como vamos testar nosso sistema! Até aqui criamos, mas como vamos testar para ver se tudo está funcionando? E se tudo estiver funcionando, até quando conseguimos segurar diversos usuários?

## 1. O que é Teste de Carga?

O teste de carga é uma técnica de teste de software que visa avaliar o comportamento de um sistema sob condições de carga específicas. O objetivo é verificar se o sistema é capaz de suportar a carga esperada, garantindo que o mesmo funcione de forma eficiente e eficaz.

> Mas Murilo, quando fazemos uma requisição com o Insomnia, Postman ou até mesmo acessamos uma página web, não estamos realizando um teste de carga?

Sim, de certa forma estamos realizando um teste de carga, porém, o teste de carga que estamos falando é um pouco mais complexo. Ele é realizado com o intuito de avaliar o comportamento do sistema sob condições de carga específicas, como por exemplo, o número de usuários simultâneos, a quantidade de requisições por segundo, entre outros.

## 2. Por que realizar um Teste de Carga?

O teste de carga é uma etapa muito importante no ciclo de desenvolvimento de software, pois ele permite avaliar o comportamento do sistema sob condições de carga específicas, garantindo que o mesmo funcione de forma eficiente e eficaz.

Além disso, o teste de carga permite identificar possíveis gargalos e problemas de desempenho no sistema, possibilitando a correção dos mesmos antes que o sistema seja disponibilizado para os usuários finais.

## 3. Como realizar um Teste de Carga?

Para realizar um teste de carga, é necessário seguir os seguintes passos:

1. **Definir os objetivos do teste**: Antes de iniciar o teste, é importante definir os objetivos que se deseja alcançar com o mesmo. Por exemplo, verificar se o sistema é capaz de suportar um determinado número de usuários simultâneos, a quantidade de requisições por segundo, entre outros.

2. **Definir as métricas de avaliação**: Após definir os objetivos do teste, é importante definir as métricas que serão utilizadas para avaliar o comportamento do sistema. Por exemplo, tempo de resposta, taxa de erro, entre outros.

3. **Elaborar o plano de teste**: Com os objetivos e métricas definidos, é necessário elaborar o plano de teste, que consiste em definir as condições de carga que serão aplicadas ao sistema, como por exemplo, o número de usuários simultâneos, a quantidade de requisições por segundo, entre outros.

4. **Executar o teste**: Após elaborar o plano de teste, é necessário executar o teste, aplicando as condições de carga definidas ao sistema e coletando os resultados obtidos.

5. **Analisar os resultados**: Por fim, é necessário analisar os resultados obtidos, verificando se o sistema foi capaz de suportar a carga esperada e se o mesmo funcionou de forma eficiente e eficaz.

## 4. Métricas de Desempenho

Durante o teste de carga, é importante avaliar algumas métricas de desempenho que podem indicar possíveis problemas no sistema. Algumas das métricas mais comuns são:

- **Tempo de Resposta**: Tempo que o sistema leva para responder a uma requisição.

- **Taxa de Erro**: Porcentagem de requisições que resultaram em erro.

- **Taxa de Transferência**: Quantidade de dados transferidos por segundo.

- **Utilização de Recursos**: Porcentagem de utilização dos recursos do sistema, como CPU, memória, disco, entre outros.

## 5. Tipos de Teste de Carga

[Tadjudin](https://medium.com/@rico098098) faz uma definição bastante interessante de tipos de teste de carga em seu trabalho [*Load Testing with Python*](https://medium.com/@rico098098/load-testing-with-python-fea13369af43). Vamos avaliar as definições que ele pontua:

### 5.1 Teste de Carga

Verifica como o sistema se comporta sob uma carga específica, como por exemplo, o número de usuários simultâneos, a quantidade de requisições por segundo, entre outros.

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*vylgyAgdatwOn0ZyXj8WpA.png" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

<p style={{ textAlign:"center", marginBottom:'24px' }}>(Referência: [link](https://miro.medium.com/v2/resize:fit:720/format:webp/1*vylgyAgdatwOn0ZyXj8WpA.png))</p>

### 5.2 Teste de Estresse

Verifica como o sistema se comporta sob uma carga extrema, como por exemplo, o número máximo de usuários simultâneos, a quantidade máxima de requisições por segundo, entre outros. O objetivo deste teste é verificar o limite do sistema e identificar possíveis problemas de desempenho.

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*dyrsK-uyeG0N7tDR0DuXSw.png" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

<p style={{ textAlign:"center", marginBottom:'24px' }}>(Referência: [link](https://miro.medium.com/v2/resize:fit:720/format:webp/1*dyrsK-uyeG0N7tDR0DuXSw.png))</p>

### 5.3 Teste de Estabilidade (Endurance)

Verifica como o sistema se comporta sob uma carga constante, por um longo período de tempo. O objetivo deste teste é verificar se o sistema é capaz de manter o desempenho ao longo do tempo, identificando possíveis problemas de vazamento de recursos.

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*CtRQh9cGZX2fKTQ85KSNew.png" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

<p style={{ textAlign:"center", marginBottom:'24px' }}>(Referência: [link](https://miro.medium.com/v2/resize:fit:720/format:webp/1*CtRQh9cGZX2fKTQ85KSNew.png))</p>

## 6. Aplicações que Serão Testadas

Nesta seção pessoal, vamos avaliar as aplicações que serão testadas durante nosso estudo sobre os testes de carga. Elas são aplicações construídas com alguns frameworks de Python e todas estão dockerizadas  🐳.

Peço que vocês dediquem uma quantidade de tempo para verificar como elas foram desenvolvidas e quais são as diferenças entre elas. Todos os códigos fonte estão disponíveis no [repositório do GitHub](https://github.com/Murilo-ZC/M10-Inteli-Eng-Comp/tree/main/src/encontro05).

### 6.1 Aplicações

Aqui temos um breve descritivo de cada uma das aplicações que serão testadas.

### 6.2 Aplicação 1 - Flask com SQLite

Essa aplicação foi construída com o framework Flask e utiliza o banco de dados SQLite. Ela é uma aplicação simples que permite a criação de usuários e a listagem de todos os usuários cadastrados.

O CRUD foi implementado utilizando a biblioteca `sqlite3` do Python e comandos SQL. A aplicação foi dockerizada e está pronta para ser executada. O link para ela pode ser visto [aqui](https://github.com/Murilo-ZC/M10-Inteli-Eng-Comp/tree/main/src/encontro05/aplicacao01).

Um ponto importante para se observar, essa implementação foi deployada com o servidor built-in do Flask, o que não é recomendado para ambientes de produção.

### 6.3 Aplicação 2 - Flask com SQLite e Servidor Gunicorn

Está aplicação é igual a anterior, mas utilizando o servidor Gunicorn para servir a aplicação. O Gunicorn é um servidor HTTP WSGI para Python que é amplamente utilizado para servir aplicações web Python em produção.

:::tip[WSGI]

A Interface Gateway de Servidor Web (WSGI) é uma especificação para a comunicação entre servidores web e aplicações web Python. Ela define um contrato entre servidores web e aplicações web Python, permitindo que diferentes servidores web e aplicações web Python se comuniquem de forma eficiente.

Para saber mais sobre o WSGI, acesse a [documentação oficial](https://www.python.org/dev/peps/pep-3333/).
E para saber mais sobre o Gunicorn, acesse a [documentação oficial](https://gunicorn.org/) e sua implementação com o [Flask](https://flask.palletsprojects.com/en/3.0.x/deploying/gunicorn/).
:::

Você pode acessar o código fonte [aqui](https://github.com/Murilo-ZC/M10-Inteli-Eng-Comp/tree/main/src/encontro05/aplicacao02).

### 6.4 Aplicação 3 - FastAPI com SQLite

Agora pessoal, vamos para uma aplicação construída com o framework FastAPI. O FastAPI é um framework moderno e rápido para construir APIs web com Python 3.6+ baseado em anotações de tipo.

Vamos utilizar a mesma base de código da aplicação 1, mas agora com o FastAPI. O link para o código fonte pode ser acessado [aqui](https://github.com/Murilo-ZC/M10-Inteli-Eng-Comp/tree/main/src/encontro05/aplicacao03).

:::tip[Uvicorn vs Gunicorn]

O Uvicorn é um servidor ASGI que é amplamente utilizado para servir aplicações web Python em produção. Ele é o servidor recomendado para servir aplicações FastAPI. Já o Gunicorn é um servidor WSGI que é amplamente utilizado para servir aplicações web Python em produção. Ele é o servidor recomendado para servir aplicações Flask.

:::

:::tip[ASGI]

A Interface Gateway de Servidor Assíncrono (ASGI) é uma especificação para a comunicação entre servidores web e aplicativos web Python assíncronos. Ela define um contrato entre servidores web e aplicativos web Python assíncronos, permitindo que diferentes servidores web e aplicativos web Python assíncronos se comuniquem de forma eficiente.

Para saber mais sobre o ASGI, acesse a [documentação oficial](https://asgi.readthedocs.io/en/latest/).
E para saber mais sobre o Uvicorn, acesse a [documentação oficial](https://www.uvicorn.org/).

:::

### 6.5 Considerações 

Pessoal ainda podemos criar diferentes aplicações para testar. Essas são apenas algumas aplicações base que utilizam diferentes frameworks de Python.

:::danger[IMPORTANTE]

Pessoal, repare que, mesmo com frameworks diferentes, as aplicações tem as demais características iguais. Isso é proposital para que possamos avaliar o desempenho de cada framework. Quando iniciarmos as trocas de pontos das tecnologias, devemos lembrar que elas podem impactar na performance da aplicação.

Se estamos comparando apenas os frameworks, devemos manter as demais características iguais.

:::

## 7. Ferramentas de Teste

Pessoal agora vamos falar sobre as ferramentas de teste que serão utilizadas para avaliar as aplicações que foram construídas. Cada ferramenta apresentada aqui tem suas particularidades e é importante entender como elas funcionam.

Nosso objetivo será compreender como elas são utilizadas. Vamos realizar todos os testes na aplicação 1. A comparação com as demais aplicações fica como tarefa de implementação de vocês.

### 7.1 Locust

O Locust é uma ferramenta de teste de carga de código aberto que permite que você escreva cenários de teste em Python. Ele é altamente escalável e pode ser usado para testar aplicações web, APIs e outros sistemas. Sua documentação oficial pode ser acessada [aqui](https://locust.io).

Vamos testar nossa aplicação 1 com o Locust. Para isso, vamos seguir os seguintes passos:

```bash
# criar um venv
python3 -m venv venv
# ativar o venv
source venv/bin/activate
# instalar o locust
pip install locust
```

Agora vamos criar um arquivo chamado `locustfile.py` com o seguinte conteúdo:

```python
from locust import HttpUser, task, between

class WebsiteUser(HttpUser):
    wait_time = between(1, 5)

    @task
    def criar_usuario(self):
        self.client.post("/usuarios", json={"nome": "Fulano",  "email": "mail@mail.com" })

    @task
    def pegar_usuarios(self):
        self.client.get(f"/usuarios")
```

Agora no terminal execute a aplicação com o comando:

```bash
locust -f locustfile.py
```

Acesse o endereço `http://localhost:8089` e configure o número de usuários e a taxa de requisições. Depois clique em `Start Swarming` para iniciar o teste.


---

## 8. Recomendações de Vídeo

<iframe width="560" height="315" src="https://www.youtube.com/embed/KECr2BujqtM?si=oRaHu1C62bQQf0GE" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom:'24px' }}></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/RiM1GsVSbzM?si=e4IITWxzQxbbHXCo" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom:'24px' }}></iframe>

---

## 9. Proxy e Proxy Reverso

Pessoal vamos analisar aqui um caso um pouco mais completo agora. Vamos ter nossa API primeiro rodando com o Flask e se conectando em um banco de dados SQLite. Mas vamos servir ela de uma maneira diferente para a aplicação. Vamos colocar um proxy reverso na frente dela. 

> "Calma lá Murilão, vamos colocar o que?"

Um Proxy Reverso é um servidor que fica entre o cliente e a nossa aplicação. Ele é utilizado para gerenciar como as requisições externas chegam até nossa aplicação interna. Com ele, conseguimos manter nossa aplicação rodando com comunicação HTTP internamente, enquanto usamos HTTPS externamente, com certificados SSL sendo gerenciados no proxy.

> "Murilo, pera ai, o que é um proxy então?"

Boa pergunta! Um proxy comum é uma forma de interceptar conexões de saída em uma rede. Ele atua em nome do cliente, repassando requisições internas para a internet, aplicando políticas como controle de acesso, autenticação ou caching. Já o proxy reverso faz o caminho contrário: ele recebe requisições externas e as encaminha para os servidores internos.

| Característica     | Proxy (normal)            | Proxy Reverso            |
| ------------------ | ------------------------- | ------------------------ |
| Quem configura     | O cliente                 | O servidor               |
| Esconde quem?      | O cliente (usuário final) | O servidor (backend/API) |
| Protege quem?      | O cliente                 | O servidor               |
| Exemplo típico     | Acesso controlado à web   | Balanceamento de carga   |
| Ferramentas comuns | Squid, TinyProxy          | NGINX, HAProxy, Traefik  |


:::tip[Proxy e Proxy Reverso]

<iframe width="560" height="315" src="https://www.youtube.com/embed/4NB0NDtOwIQ?si=yUN9vP96KAqxk6UT" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}></iframe>
<br />

<iframe width="560" height="315" src="https://www.youtube.com/embed/ngoEzjnJ2Eo?si=t2Q0eLI7wGlcdcho" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}></iframe>
<br />

:::

Legal, agora que falamos dele, vamos configurar!! Vamos fazer isso utilizando uma ferramenta chamada [Nginx](https://nginx.org/).

## 10. Exemplo de uso de Proxy Reverso

Vamos trabalhar com a aplicação em Python:

```python
# app.py

```