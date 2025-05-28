---
sidebar_position: 2
slug: /backend/sincrono-assincrono
title: "Sistemas Síncronos e Assíncronos"
---

<!-- ## TO-DO

<img src="https://64.media.tumblr.com/1410879ae6d00e77f5dbe27c03f252fc/tumblr_inline_ofgcs4tnDi1r5ight_400.gifv" alt="Gai Sensei and Rock Lee Trainning Smiles and Taijutsu"
style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}></img>
<br /> -->

Antes de iniciarmos nossos estudos, precisamos compreender alguns conceitos e como eles são relevantes no desenvolvimento de nossas soluções. Por diversas vezes, misturamos alguns deles, ou até utilizamos o nome de um quando estamos falando de outro. Por isso, vamos entender o que são sistemas síncronos e assíncronos.

Afinal, o que são sistemas síncronos e assíncronos?

## 0. TL;DR – Sistemas Síncronos vs. Assíncronos e AsyncIO em Python

* **Sistemas Síncronos**

  * Executam tarefas de forma sequencial: cada passo só começa quando o anterior termina.
  * Altamente previsíveis e fáceis de testar/depurar, ideais onde o tempo e a ordem exatos são críticos (ex.: controle industrial, transações).
  * Podem desperdiçar recursos (bloqueios e espera ativa) e ter escalabilidade limitada.

* **Sistemas Assíncronos**

  * Permitem que várias operações ocorram “em paralelo” (no mesmo thread) sem bloquear umas às outras, usando eventos e callbacks.
  * Melhor aproveitamento de CPU e I/O, elevada escalabilidade, recomendado em cenários de alto volume de requisições ou I/O intensivo.
  * Mais complexos de gerenciar (estado, condições de corrida) e de depurar.

* **Como funciona internamente**

  * Um **loop de eventos** monitora uma fila de eventos: quando uma operação termina, dispara o callback associado.
  * Difere de sistemas multithread: normalmente usa um único thread, evitando overhead de criação/sincronização de threads.

* **AsyncIO no Python**

  * Biblioteca padrão desde o Python 3.4, com sintaxe `async`/`await` (oficialmente desde o 3.5).
  * **Corrotinas**: funções marcadas com `async def` que podem pausar sua execução em `await`.
  * **Tarefas**: wrappers criados com `asyncio.create_task()` para executar corrotinas concorrentemente.
  * **`asyncio.gather()`** e **`asyncio.wait()`** agrupam múltiplas corrotinas e esperam sua conclusão.

* **Casos de uso práticos**

  * Operações de I/O (rede, disco, banco de dados) com drivers assíncronos (e.g. `aiohttp`, `aiosqlite`, `asyncpg`).
  * Integração de código síncrono com `loop.run_in_executor()` para não bloquear o evento.
  * Testes assíncronos com `pytest-asyncio`.

* **Quando usar cada modelo**

  * **Síncrono**: quando a ordem e o tempo rígido de execução são fundamentais.
  * **Assíncrono**: quando precisa de alta concorrência, baixa latência e melhor uso de recursos.


## 1. Sistemas Síncronos

Sistemas síncronos são aqueles em que as operações ocorrem em resposta direta a um estímulo e com requisitos estritos de tempo. Em uma execução síncrona, cada passo ou tarefa aguarda a conclusão da anterior antes de começar, e muitas vezes isso é controlado por sincronização explícita, como semáforos ou bloqueios. Isso garante que as operações ocorram de forma sequencial e ordenada. 

Aqui estão alguns aspectos importantes dos sistemas síncronos:

- **Coordenação de tempo:** Em sistemas síncronos, as operações dependem de sinais ou eventos que ocorrem em intervalos específicos e previsíveis. Isso significa que a comunicação entre diferentes componentes do sistema é feita de forma que todos os elementos "esperem" ou se sincronizem uns com os outros baseados em relógios ou cronômetros.
- **Previsibilidade:** Devido à necessidade de operações ocorrerem em tempos específicos, esses sistemas são altamente previsíveis, o que é crucial em aplicações onde a temporização é essencial, como sistemas de controle industrial, telecomunicações ou sistemas embutidos em tempo real.
- **Bloqueios e espera ativa:** Em muitos sistemas síncronos, um processo pode bloquear ou entrar em um estado de espera ativa até que um evento específico ocorra, o que pode incluir a chegada de dados ou um sinal de outro processo. Este mecanismo de sincronização ajuda a manter a ordem e a coordenação dentro do sistema.
- **Facilidade de teste e depuração:** A natureza previsível e ordenada dos sistemas síncronos facilita o teste e a depuração, pois o comportamento do sistema em diferentes momentos é mais fácil de ser previsto e analisado.

Embora haja vantagens, os sistemas síncronos podem ser menos eficientes em termos de uso de recursos, pois podem exigir que os processos esperem por eventos externos, o que pode levar a um desperdício de ciclos de CPU, especialmente se a espera ativa for utilizada.

Na figura abaixo, é possível visualizar um exemplo de sistema síncrono, onde as operações ocorrem em resposta a um estímulo e de forma sequencial. Uma operação aguarda a conclusão da anterior antes de começar.

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSD4QU4e1T-SszRZ0cDoKZWwIWpHj2u99yagdE8DjdmQ2MM3uJU4QFv7nqHoEe6SGGCpw&usqp=CAU" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

:::danger[Sistemas Síncronos são ruins?]	

<img src="https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3apq1b5vpatu7eyn4lk4.jpeg" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

Não seria adequado afirmar genericamente que sistemas síncronos são ruins, pois a escolha entre sistemas síncronos e assíncronos depende amplamente das necessidades específicas e do contexto de cada aplicação. Ambos os tipos de sistemas têm suas vantagens e desvantagens, e a adequação depende do que é necessário para o sistema em questão.

- Vantagens dos Sistemas Síncronos:
    - Previsibilidade: Devido à natureza ordenada e cronometrada, sistemas síncronos são altamente previsíveis, o que facilita a depuração e o teste.
    - Facilidade de entendimento: A lógica de controle em sistemas síncronos pode ser mais fácil de entender e seguir, pois as ações ocorrem em uma sequência fixa e bem definida.
    - Consistência e integridade de dados: Em ambientes onde a ordem e a integridade dos dados são críticas, como sistemas de banco de dados transacionais, a execução síncrona pode garantir que as operações sejam completadas de maneira segura e ordenada.

- Desvantagens dos Sistemas Síncronos:
    - Ineficiência de recursos: Sistemas síncronos podem levar à ineficiência, especialmente se os processos passarem muito tempo esperando por outros processos ou eventos, resultando em ociosidade de recursos.
    - Escalabilidade limitada: Em alguns casos, a necessidade de sincronização estrita pode limitar a escalabilidade do sistema, pois adicionar mais processos ou usuários pode aumentar a complexidade e o overhead de sincronização.
    - Menos flexíveis: Em ambientes dinâmicos ou com cargas de trabalho variáveis, sistemas síncronos podem não se adaptar tão bem quanto os sistemas assíncronos.

- Quando escolher sistemas síncronos:
    - Em aplicações que requerem uma sequência exata e previsível de operações.
    - Quando a integridade e a ordem dos dados são críticas e não podem ser comprometidas.
    - Em sistemas em tempo real onde o cumprimento de prazos estritos é fundamental.

- Quando evitar sistemas síncronos:
    - Em sistemas altamente escaláveis onde a latência e a ociosidade de recursos podem ser um problema.
    - Em ambientes onde as tarefas são largamente independentes e podem ser executadas de forma desacoplada.

Recomendo para vocês pessoal realizar a leitura deste artigo e desta thread sobre o tópico:
- [Synchronous vs asynchronous programming and their use cases](https://ahmedsadman.medium.com/synchronous-vs-asynchronous-programming-and-their-use-cases-88a7d2f04205)
- [Is there any proof that async/await is actually better than synchronous code?](https://www.reddit.com/r/csharp/comments/qaadm7/is_there_any_proof_that_asyncawait_is_actually/)

:::

## 2. Sistemas Assíncronos

Os sistemas assíncronos, em contraste com os sistemas síncronos, não dependem de uma sincronização estrita de tempo entre as operações. Em vez disso, eles permitem que as operações ocorram de forma independente, sem que uma operação precise esperar diretamente pela conclusão de outra antes de iniciar. Isso confere uma flexibilidade considerável e pode melhorar a eficiência e a escalabilidade em muitos contextos.

Definição de programação assíncrona:

> Uma forma de permitir que o programa possa executar mais de uma tarefa de uma única vez, ao invés de esperar uma tarefa terminar e iniciar a próxima. Todas as tarefas são executas em conjunto.

A programa síncrona, realiza uma tarefa por vez, o que pode resultar em uma condição bloqueante para execução de alguma tarefa ou função.

Nem tudo são maravilhas, existe um tradeoff a ser considerado. Ao trabalhar com programação concorrente, alguns problemas podem acontecer:
- Race Conditions;
- Deadlocks;
- Resources Starvation (quando múltiplos processos ou threads tentam acessar o mesmo conjunto de registros);

Esses problemas ocorrem principalmente quando a sincronização não está propriamente configurada. A ***Programação Assíncrona*** permite utilizar uma forma estruturada e controlada para trabalhar com concorrência. Utilizando uma arquitetura orientada a eventos e um controle explicito sobre o fluxo de execução do programa.

Benefícios da programação Assíncrona:
- Permite a execução concorrente de tarefas;
- Oferece um melhor aproveitamento de desempenho e utilização de recursos;
- Resolve os problemas de execução concorrente utilizando a aproximação de evento;
- É bastante útil em cenários que envolve a execução longa de uma requisição ou operações de I/O custosas.

Na figura abaixo, é possível visualizar um exemplo de sistema assíncrono, onde as operações ocorrem de forma independente e não sequencial. Uma operação pode iniciar antes da conclusão da anterior. Vale destacar que isso é alcançado por meio de mecanismos de controle de fluxo e eventos.

<img src="https://buildfire.com/wp-content/uploads/2023/03/Requests.png" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

:::warning[Programação Assíncrona é melhor que Síncrona?]

<img src="https://miro.medium.com/v2/resize:fit:1000/1*0I-RxEy9YIiWilMNRFUYMg.gif" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

Podemos deixar quase a mesma resposta que a da pergunta anterior, variando as características do sistema. Lembrem-se sempre que a escolha entre sistemas síncronos e assíncronos depende amplamente das necessidades específicas e do contexto de cada aplicação. Ambos os tipos de sistemas têm suas vantagens e desvantagens, e a adequação depende do que é necessário para o sistema em questão.

- Características dos Sistemas Assíncronos:
    - Independência Temporal: As tarefas ou operações em um sistema assíncrono são executadas sem a necessidade de se alinharem a um relógio central ou esperar por um evento específico. Isso permite que as tarefas sejam processadas assim que os recursos estão disponíveis ou quando um evento relevante ocorre.
    - Uso Eficiente de Recursos: Em sistemas assíncronos, os recursos não ficam ociosos esperando a conclusão de outras tarefas. Isso pode levar a uma utilização de recursos mais eficiente e a uma resposta mais rápida do sistema, especialmente em ambientes de alta carga.
    - Escalabilidade: Sem a necessidade de processamento sequencial e sincronizado, sistemas assíncronos são frequentemente mais fáceis de escalar. Eles podem lidar com um aumento no volume de operações ou usuários mais facilmente, distribuindo tarefas de forma mais flexível.
    - Complexidade de Gerenciamento de Estado: Embora ofereçam muitas vantagens, os sistemas assíncronos podem introduzir complexidade adicional no gerenciamento de estado, uma vez que o estado do sistema pode mudar em momentos inesperados, e o controle de dependências entre tarefas pode ser desafiador.
    - Desafios de Depuração: A depuração e o teste podem ser mais complexos em sistemas assíncronos devido à natureza imprevisível e não sequencial das operações. Rastrear problemas específicos ou condições de corrida pode ser difícil sem ferramentas e técnicas adequadas.

- Quando usar sistemas assíncronos:
    - Em Aplicações de Alto Volume: Como serviços web ou sistemas de processamento de mensagens onde a capacidade de lidar com muitas tarefas ou solicitações de forma independente é crucial.
    - Em Operações de I/O Intensivo: Sistemas que envolvem operações de entrada/saída intensivas, como acesso a redes ou a discos, podem beneficiar-se do modelo assíncrono para evitar bloqueios e aumentar a resposta do sistema.
    - Em Ambientes Distribuídos: Em sistemas distribuídos, especialmente em computação em nuvem, onde os componentes podem estar fisicamente separados e as latências variáveis, a assincronicidade pode ajudar a melhorar a eficiência geral.

- Quando evitar sistemas assíncronos:
    - Quando a Ordem Exata é Crítica: Em sistemas onde a sequência precisa de operações é fundamental para a integridade dos dados ou para a lógica do negócio, a abordagem assíncrona pode introduzir riscos inaceitáveis.
    - Em Ambientes com Requisitos de Tempo Real Estritos: Em aplicações onde o cumprimento de prazos precisos é crucial, a natureza imprevisível dos sistemas assíncronos pode ser um problema.

:::

## 3. Como os Sistemas Assíncronos Funcionam?

Vamos analisar a imagem a seguir e entender como os sistemas assíncronos funcionam:

<img src="https://miro.medium.com/v2/resize:fit:1400/0*I_nlWjD6faxLrjAY.gif" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

Sistemas assíncronos são baseados em eventos e callbacks. Em vez de esperar que uma operação seja concluída antes de iniciar outra, o sistema pode iniciar várias operações simultaneamente e lidar com os resultados à medida que eles se tornam disponíveis. Isso é frequentemente feito por meio de callbacks, que são funções que são chamadas quando uma operação assíncrona é concluída.

> Mas Murilo, quem é que chama a função de callback? Quem lida com os eventos?

Em sistemas assíncronos, um loop de eventos é frequentemente usado para gerenciar a execução de tarefas e a chamada de callbacks. O loop de eventos é responsável por monitorar a fila de eventos e executar as funções de callback associadas a esses eventos. Isso permite que o sistema seja altamente responsivo e eficiente, pois pode lidar com várias operações concorrentes sem bloquear o thread principal.

Podemos pensar no loop de eventos como um gerenciador de fila que processa eventos à medida que eles chegam. Quando uma operação assíncrona é concluída, um evento é gerado e colocado na fila de eventos. O loop de eventos verifica continuamente a fila e executa as funções de callback associadas a esses eventos.

> Ahhhh entendi Murilo! Então é como se eu tivesse rodando várias threads ao mesmo tempo né? (AQUI EU FORCEI A BARRA NA PERGUNTA MAS É PARA VERMOS UMA DIFERENÇA IMPORTANTE)

Então, não.

Aqui está uma diferença importante entre sistemas assíncronos e sistemas baseados em threads. Em sistemas baseados em threads, cada thread tem seu próprio contexto de execução e pode executar operações de forma independente. No entanto, o uso excessivo de threads pode levar a problemas de concorrência, como condições de corrida e dead locks. Quando estamos falando de sistemas assíncronos, geralmente estamos falando de um único thread que gerencia várias operações assíncronas, o que pode ser mais eficiente e seguro em muitos casos. O tempo de execução é compartilhado entre as operações, e o loop de eventos garante que as operações sejam executadas de forma ordenada e eficiente.

A figura abaixo demonstra uma comparação entre sistemas baseados em threads e sistemas assíncronos:

<img src="https://miro.medium.com/v2/resize:fit:1189/1*k5n4Px1Pe8nbmnD-TaWvBA.png" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

Com o Python, podemos utilizar a biblioteca `asyncio` para trabalhar com programação assíncrona. A biblioteca `asyncio` fornece uma estrutura para escrever código assíncrono usando a sintaxe `async` e `await`. Isso permite que você escreva código que pode lidar com operações assíncronas de forma eficiente e concorrente.

<img src="https://www.scaler.com/topics/images/asyncio-python-thumbnail.webp" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

## 4. Python AsyncIO

Legal, agora temos o conceito alinhado conosco, vamos falar sobre o AsyncIO do Python.

Ele é uma biblioteca padrão do Python que fornece suporte para escrever código assíncrono usando a sintaxe async/await. Foi introduzido na versão 3.4 do Python e é uma das maneiras mais fáceis de escrever código assíncrono.

:::tip[Para saber mais]

> Mas Murilo por que você está já está adicionando o material do para saber mais?

Porque eu quase esqueci na última seção, então para não esquecer, já estou adicionando aqui.

- [Async IO in Python: A Complete Walkthrough](https://realpython.com/async-io-python/)
- [Python Asyncio: The Complete Guide](https://superfastpython.com/python-asyncio/)
- [Coroutines and Tasks](https://docs.python.org/3/library/asyncio-task.html)

:::

Para os próximos exemplos, vamos discultir alguns conceitos de utilização do AsyncIO. Ele, por padrão vem na biblioteca padrão do Python, então não é necessário instalar nada. CONTUDO, é uma boa prática fazer a separação de um ambiente virtual para cada projeto, então vamos criar um ambiente virtual para o nosso projeto.

```bash
python3 -m venv env
source env/bin/activate
```

Ao longo dos exemplos, vamos precisar de bibliotecas adicionais, ai elas podem ser adicionadas neste ambiente virtual, mantendo nossa instalação limpa.

<img src="https://www.saaspegasus.com/static/images/web/uv/no-venv.4e57a4b43eea.jpg" style={{ display: 'block', marginLeft: 'auto', maxHeight: '40vh', marginRight: 'auto', marginBottom: '24px' }}/>

## 5. Primeiro Código com AsyncIO

Primeiro programa:

```python
# O asyncio é uma biblioteca que permite a execução de tarefas assíncronas
# Ela é uma biblioteca padrão do Python 3.5 ou superior
import asyncio


# Para marcarmos uma função como assíncrona, utilizamos a palavra-chave async
async def print_com_delay(tempo, mensagem):
    # A função asyncio.sleep é uma função assíncrona que suspende a execução da função por um determinado tempo
    await asyncio.sleep(tempo)
    print(mensagem)


# Agora configuramos uma função principal que irá chamar a função assíncrona
async def main():
    # Adicionando um marcador para ver o tempo de execução da função
    print("Início da execução:", asyncio.get_event_loop().time())
    # Realiza uma chamada bloqueante para a função assíncrona
    await print_com_delay(1, "Olá")
    await print_com_delay(2, "Mundo")
    # Adicionando um marcador para ver o tempo de execução da função
    print("Fim da execução:", asyncio.get_event_loop().time())

  

# Para chamar a função principal, utilizamos o método que cria um evento de loop e executa a função principal
asyncio.run(main())
```

Quanto utilizamos a palavra chave `async` estamos definindo que aquela função é especial e pode ser iniciada e pausada pelo `event-loop`  do sistema. Quando utilizamos o operador `await` estamos esperando que a operação assíncrona termine para que a execução do código possa avançar. É uma chamada do tipo `bloqueante`.

A chamada `asyncio.run(main())` é o entry-point de chamada para o nosso sistema. É ele que permite nossa execução assíncrona.  O elemento fundamental para a execução do código assíncrono é o `event-loop`. Ele é fundamental para a coordenar a execução das tarefas sem bloquear o fluxo de execução principal do programa.

O `event-loop` é uma estrutura que continuamente monitora e processa eventos de diferentes fontes, como entradas, chamadas de rede ou mesmo eventos de temporizadores. Ele é o responsável por acionar os `handlers` ou `callbacks` específicos de cada evento. A chamada `asyncio.run` é responsável por fazer a criação do `event-loop` e executar a corrotina principal.

São responsabilidades do `event-loop`:
- Agendar e executar as tarefas assíncronas (corotinas)
- Lidar com operações de I/O
- Lidar com temporizações e timeouts
- Liberar eventos para a fonte correspondente (handler)

Outro exemplo de utilização:

```python
import asyncio

async def task(nome, tempo_delay):
    print(f"Task {nome} iniciada")
    await asyncio.sleep(tempo_delay)
    print(f"Task {nome} finalizada")
    return f"Task {nome} finalizada"

async def main():
    # Cria uma lista de tarefas
    tasks = [
        task("A", 1),
        task("B", 5),
        task("C", 3)
    ]

    # Envia todas as tarefas para execução
    # O método gather() aguarda todas as tarefas serem finalizadas
    # Ele também lança a execução de todas as tarefas
    results = await asyncio.gather(*tasks)
    print(results)

# Cria um novo evento de loop
asyncio.run(main())

```

## 6. Síntaxe Async/Await

O `async/await` foi adicionado no Python 3.5.
As corrotinas são funções especiais que podem ter sua execução suspensa e resumida, possibilitando sua execução concorrente.
A palavra chave `await` serve para aguarda o fim da execução de uma operação assíncrona. Quando desejamos aguardar a execução de mais de uma corrotina de forma simultânea, nós devemos utilizar o `asyncio.gather()`.
A partir da versão 3.8 do Python, o conceito de `Native Coroutine` foi implementado. Elas utilizam uma versão dedicada do opcode do await, resultando em um desempenho melhor e e redução do overhead de execução do código. A sintaxe `async/await` permanece a mesma, mas a sua implementação foi melhorada.
As `Async Comprehensions` permitem utilizar o recurso de list comprehensions para criar listas utilizando o retorno de corrotinas. O mesmo pode ser utilizado com a criação de dicionários e sets.

```python
# Utiliza o recurso de list comprehension para criar uma lista de tarefas utilizando um gerado assíncrono
import asyncio

async def quadrado_assincrono(n):
    await asyncio.sleep(1)
    return n * n

async def main():
    numeros = [1, 2, 4, 8, 16, 32]
    numeros_quadrados = [await quadrado_assincrono(numero) for numero in numeros]
    print(numeros_quadrados)

    # Versão otimizada com o método gather
    numeros_quadrados = await asyncio.gather(*[quadrado_assincrono(numero) for numero in numeros])
    print(numeros_quadrados)
    
if __name__ == "__main__":
    asyncio.run(main())
```

Chamando corotinas simples:

```python
# Agendando e executando uma corotina simples

import asyncio

async def corotina(nome, delay):
    print(f"Corotina {nome} iniciada - {asyncio.get_event_loop().time()}")
    await asyncio.sleep(delay)
    print(f"Corotina {nome} finalizada - {asyncio.get_event_loop().time()}")
    

async def main():
    # Prepara as corotinas utilizando o método create_task
    # O método create_task() cria uma tarefa para execução de uma corotina, mas não a executa. A execução é feita pelo loop de eventos.
    corotina1 = asyncio.create_task(corotina("A", 1))
    corotina2 = asyncio.create_task(corotina("B", 2))

    # Aguarda a execução das corotinas, mas não bloqueia a execução
    await corotina1
    await corotina2

# Cria um novo evento de loop
if __name__ == "__main__":
    asyncio.run(main())
```

> ***IMPORTANTE:*** existe uma diferença entre chamar e aguardar uma corotina e criar uma task com ela utilizando o `asyncio.create_task()`. Quando apenas chamamos as corotinas, elas são executadas conforme a execução dos pontos anteriores do programa vai terminando (o await fica com um comportamento `bloqueante`). Quando invocamos elas criando corotinas (não apenas as funções assíncronas puras), o `await` não é bloqueante.


```python
# Agendando e executando uma corotina simples

import asyncio

async def corotina(nome, delay):
    print(f"Corotina {nome} iniciada - {asyncio.get_event_loop().time()}")
    await asyncio.sleep(delay)
    print(f"Corotina {nome} finalizada - {asyncio.get_event_loop().time()}")
    

async def main():
    # Prepara as corotinas utilizando o método create_task
    # O método create_task() cria uma tarefa para execução de uma corotina, mas não a executa. A execução é feita pelo loop de eventos.
    corotina1 = asyncio.create_task(corotina("A", 1))
    corotina2 = asyncio.create_task(corotina("B", 2))

    # Aguarda a execução das corotinas, mas não bloqueia a execução
    await corotina1
    await corotina2

    # Chamada do exemplo anterior, mas sem a criação de tarefas
    corotina3 = corotina("C", 3)
    corotina4 = corotina("D", 4)
    await corotina3
    await corotina4
    

# Cria um novo evento de loop
if __name__ == "__main__":
    asyncio.run(main())
```

Saída da execução do código:

```sh
> python corotina-simples.py
Corotina A iniciada - 428855.359
Corotina B iniciada - 428855.359
Corotina A finalizada - 428856.359
Corotina B finalizada - 428857.375
Corotina C iniciada - 428857.375
Corotina C finalizada - 428860.375
Corotina D iniciada - 428860.375
Corotina D finalizada - 428864.39
```

Podemos criar nosso próprio `event-loop` e controlar sua execução:

```python
# Programa para lidar com a execução de multiplas corotinas
import asyncio

async def corotina(nome_arquivo):
    print(f"Corotina {nome_arquivo} iniciada - {asyncio.get_event_loop().time()}")
    await asyncio.sleep(2)
    print(f"Corotina {nome_arquivo} finalizada - {asyncio.get_event_loop().time()}")
    return f"Corotina {nome_arquivo} finalizada"

async def main():
    # Cria uma lista com os nomes dos arquivos
    arquivos = ["arquivo1.txt", "arquivo2.txt", "arquivo3.txt", "arquivo4.txt"]
    chamada_download = [corotina(arquivo) for arquivo in arquivos]
    # # Esse método está deprecado, mas ainda é utilizado para aguardar a execução de todas as corotinas
    # # Utiliza o método wait() para aguardar a execução de todas as corotinas
    # finalizada, pendente = await asyncio.wait(chamada_download, return_when=asyncio.ALL_COMPLETED)

    # # Exibe o resultado da execução
    # for corotina_finalizada in finalizada:
    #     print(corotina_finalizada.result())
    
    # Versão não deprecada
    # Utiliza o método gather() para aguardar a execução de todas as corotinas
    resultados = await asyncio.gather(*chamada_download)
    for resultado in resultados:
        print(resultado)

if __name__ == "__main__":
    asyncio.run(main())
```

## 7. Gerenciamento de Tasks com AsyncIO

As Tasks são os blocos de construção para executar e gerenciar as operações utilizando o AsyncIO. As Tasks são um wrapper adicionado nas corotinas para trazer mais funcionalidades de controle a elas.

```python
# Lidando com o gerenciamento de tasks no Python com Asyncio

import asyncio

# Cria uma rotina
async def rotina(delay):
    print(f"Rotina iniciada - {asyncio.get_event_loop().time()}")
    await asyncio.sleep(delay)
    print(f"Rotina finalizada - {asyncio.get_event_loop().time()}")
    return f"Rotina finalizada"

# Cria a função principal
async def main():
    # Cria duas tarefas para serem executadas
    tarefa1 = asyncio.create_task(rotina(1))
    tarefa2 = asyncio.create_task(rotina(2))

    await asyncio.sleep(1)

    # Cancela a execução da tarefa 2
    tarefa2.cancel()

    # Aguarda a execução das tarefas
    await asyncio.gather(tarefa1, tarefa2, return_exceptions=True)

# Cria o evento de loop
if __name__ == "__main__":
    asyncio.run(main())
```

É possível verificar o estado de uma task. É importante notar algumas coisas:
- Erros e exceções nas corotinas devem ser tratados dentro do event loop.
- Se o result() de uma task for acessado antes dela terminar, vai resultar no lançamento de uma exceção.

```python
# Verifica o estado de uma task

import asyncio

async def corotina1(nome):
    await asyncio.sleep(3)
    return nome

async def corotina2(nome):
    # Corotina que lança uma exceção
    await asyncio.sleep(5)
    raise ValueError("Erro na corotina")
    return nome

# Função principal
async def main():
    try:
        # Cria as tasks
        task1 = asyncio.create_task(corotina1("A"))
        task2 = asyncio.create_task(corotina2("B"))

        # Aguarda a execução das tasks
        await asyncio.sleep(1)

        # Verifica o estado das tasks - verificando se elas estão terminadas
        print(task1.done())
        print(task2.done())

        # Exibe o estado atual das tasks
        print(task1._state)
        print(task2._state)

        # Aguarda a execução das tasks
        await asyncio.gather(task1, task2, return_exceptions=True)

        # Verifica o estado das tasks
        print(task1.done())
        print(task2.done())

        # Exibe o resultado das tasks
        print(task1.result())
        print(task2.result())
    except ValueError as e:
        print(f"Erro: {e}")

# Cria um novo evento de loop
if __name__ == "__main__":
    asyncio.run(main())
```

Uma comparação de tempo interessante:

```python
import asyncio

async def compute():
    return sum(i * i for i in range (10000000))

async def main():
    print(asyncio.get_event_loop().time())
    result1 = await compute()
    print(asyncio.get_event_loop().time())
    result2 = sum(i * i for i in range (10000000))
    print(asyncio.get_event_loop().time())

    print(result1, result2)

if __name__ == "__main__":
    asyncio.run(main())
```

## 8. Trabalhando com AsyncIO para Requisições de Rede

Utilizando os conceitos presentes no AsyncIO, é possível construir aplicações cliente-servidor que conseguem lidar com uma quantidade muito mais elevada de requisições, melhorando a utilização dos recursos disponíveis.
O AsyncIO aceita conexões seguras de SSL/TLS.
Exemplo de servidor assíncrono:

```python
# Construção de um servidor assincrono utilizando apenas o Asyncio
import asyncio

# Definição do método que será chamado para lidar com clientes
async def handle_client(reader, writer):
    # Os elementos reader e writer são objetos que permitem a leitura e escrita de dados
    # O método read() é utilizado para ler dados do cliente
    # O valor informado é a quantidade de bytes que serão lidos
    data = await reader.read(1024)
    message = data.decode()
    print(f"Dados Recebido: {message}")

    resposta = "Ola Cliente!"
    writer.write(resposta.encode())
    # O método drain() é utilizado para garantir que todos os dados foram escritos
    await writer.drain()
    print("Fechando Conexão")
    writer.close()
    await writer.wait_closed()

# Criação do servidor
async def main():
    # Cria uma instância de servidor que escuta na porta 8888
    server = await asyncio.start_server(
        handle_client, 'localhost', 8888)
    
    # Cria um loop para aguardar a conexão de clientes
    # A criação com o método async with garante que o servidor será fechado corretamente
    async with server:
        # Inicia o servidor
        await server.serve_forever()

# Inicia o servidor
if __name__ == "__main__":
    asyncio.run(main())
```

Exemplo de um cliente para conectar nesse servidor assíncrono:

```python
import asyncio

async def connect_to_server():
    reader, writer = await asyncio.open_connection('localhost', 8888)
    message = "oi tudo bem ai?"
    writer.write(message.encode())
    await writer.drain()
    data = await reader.read(1024)
    print(f"Resposta do servidor: {data.decode()}")
    writer.close()
    await writer.wait_closed()

if __name__ == "__main__":
    asyncio.run(connect_to_server())
```

É possível utilizar o AsyncIO para manipular bases de dados também. Utilizando drivers assíncronos, é possível realizar consultas e chamadas as bases de dados de forma concorrente.
Alguns drivers assíncronos:
- AsyncPG (for PostgreSQL)
- Motor (for MongoDB)
- Aio_pika (for RabbitMQ)
- Asyncio Redis (for Redis)
- aiosqlite (for sqlite)


Exemplo de execução de código utilizando o aisqlite. Primeiro é necessário instalar a dependencia.

```sh
python -m pip install aiosqlite
```

E o código para manipular os dados:

```python
import asyncio
import aiosqlite

# Cria uma corotina para criar as tabelas
async def criar_tabelas(db_name, table_name):
    async with aiosqlite.connect(db_name) as db:
        await db.execute(f"CREATE TABLE IF NOT EXISTS {table_name} (id INTEGER PRIMARY KEY, mensagem TEXT)")
        await db.commit()

# Insere alguns dados aleatórios em uma tabela
async def inserir_dados(db_name, table_name):
    async with aiosqlite.connect(db_name) as db:
        for i in range(10):
            await db.execute(f"INSERT INTO {table_name} (mensagem) VALUES ('Mensagem {i}')")
        await db.commit()

# Pega dados que estão na tabela
async def pegar_dados(db_name, table_name):
    async with aiosqlite.connect(db_name) as db:
        async with db.execute(f"SELECT * FROM {table_name}") as cursor:
            return [row async for row in cursor]

# Função principal
async def main():
    db_name = "banco.db"
    table_name = "mensagens"
    await criar_tabelas(db_name, table_name)
    await inserir_dados(db_name, table_name)
    dados = await pegar_dados(db_name, table_name)
    print(dados)

# Roda a função principal
if __name__ == "__main__":
    asyncio.run(main())
```

Em algumas situações, vai ser necessário utilizar código assíncrono com código síncrono.  É possível realizar a troca de informações entre estes dois sistemas. Esse comportamento é especialmente necessário quando algumas bibliotecas ou trechos do código que serão utilizados são síncronos.
Primeiro instalando a dependência da biblioteca `requests`:

```sh
python -m pip install requests
```

```python
# Exemplo de utilização de código sincrono em conjunto com código assincrono
import asyncio
import requests

# Cria uma corotina que fica como uma ponte entre código assincrono e código sincrono
async def fetch_url(url):
    # Pega o loop de eventos
    loop = asyncio.get_event_loop()
    # Roda a função requests.get de forma sincrona, mas em uma thread separada
    return await loop.run_in_executor(None, requests.get, url)

# Função principal
async def main():
    url = "https://www.google.com"
    # Os parênteses são necessários para pegar o texto da resposta.
    # Eles garantem que a função fetch_url seja chamada primeiro.
    data = (await fetch_url(url)).text
    print(data)

# Roda a função principal
if __name__ == "__main__":
    asyncio.run(main())
```

Utilizar o método `run_in_executor` previne que o `event-loop`possa ser bloqueado durante a execução do código.

## 9. Aplicando Testes com AsyncIO

É possível escrever código de testes para as aplicações assíncronas também. Para tal, vai ser necessário utilizar as bibliotecas `pytest` e `pytest-asyncio`.

```sh
python -m pip install pytest pytest-asyncio
```

O código da função:

```python
# Exemplo de teste de função assíncrona com pytest
import asyncio

async def fetch_data():
    await asyncio.sleep(2)
    return 'data'
```

O código do teste:

```python
# Cria o teste para a função fetch_data
import pytest
from exemplo_teste_assincrono import fetch_data

# Marca a função como de teste e com comportamento assincrono
@pytest.mark.asyncio
async def test_fetch_data():
    data = await fetch_data()
    assert data == 'data', "Resultado esperado não foi retornado."
```

Para executar os códigos de teste:

```sh
python -m pytest teste-exemplo-teste-assincrono.py
```

## 10. Projetos Assíncronos

Para trabalhar com requisições HTTP de forma assíncrona, uma biblioteca que pode ser utilizada é a `aiohttp`. Ela pode ser instalada com:

```sh
python -m pip install aiohttp
```

E um exemplo de código:

```python
import asyncio
import aiohttp

# Cria a função que irá fazer a requisição
async def fetch( url):
    # Cria uma sessão. Ela representa uma conexão com o servidor
    async with aiohttp.ClientSession() as session:
        # Faz a requisição
        async with session.get(url) as response:
            # Lê o conteúdo da resposta
            # Neste exemplo, o conteúdo está sendo retornado como texto
            return await response.text()
        
# Cria a função principal
async def main():
    url = 'https://www.uol.com.br'
    html = await fetch(url)
    print(html)


# Cria o event-loop
if __name__ == '__main__':
    asyncio.run(main())
```

É possível realizar um conjunto de requisições de forma assíncrona utilizando a biblioteca.

```python
import asyncio
import aiohttp

# Cria a função que irá fazer a requisição
async def fetch( url):
    # Cria uma sessão. Ela representa uma conexão com o servidor
    async with aiohttp.ClientSession() as session:
        # Faz a requisição
        async with session.get(url) as response:
            # Lê o conteúdo da resposta
            # Neste exemplo, o conteúdo está sendo retornado como texto
            return await response.text()
        
# Cria a função principal
async def main():
    urls = ['https://www.uol.com.br', 'https://www.globo.com', 'https://www.terra.com.br']
    # Cria uma lista de tarefas
    tasks = [asyncio.create_task(fetch(url)) for url in urls]
    html = await asyncio.gather(*tasks)
    print(html)


# Cria o event-loop
if __name__ == '__main__':
    asyncio.run(main())
```

É possível adicionar um controle de número de retentativas que a função vai fazer se não for possível receber os dados de resposta quando uma requisição é realizada.

```python
import asyncio
import aiohttp

# Cria a função que irá fazer a requisição
async def fetch( url, max_tries=3):
    # Tenta fazer a requisição até 3 vezes
    tentativa = 0
    while tentativa < max_tries:
        try:
            # Cria uma sessão. Ela representa uma conexão com o servidor
            async with aiohttp.ClientSession() as session:
                # Faz a requisição
                async with session.get(url) as response:
                    # Lê o conteúdo da resposta
                    # Neste exemplo, o conteúdo está sendo retornado como texto
                    return await response.text()
        except aiohttp.ClientError as e:
            tentativa += 1
            # Espera 1 segundo antes de tentar novamente
            await asyncio.sleep(1)
        return None
    
# Cria a função principal
async def main():
    urls = ['https://www.uol.com.br', 'https://ww.globo.com', 'https://www.terra.com.br']
    # Cria uma lista de tarefas
    tasks = [asyncio.create_task(fetch(url)) for url in urls]
    htmls = await asyncio.gather(*tasks)
    # Verifica os retornos
    for html in htmls:
        if html:
            print('Resultado obtido com sucesso')
        else:
            print('Erro ao acessar a URL')


# Cria o event-loop
if __name__ == '__main__':
    asyncio.run(main())
```



## 11. Para saber mais

Pessoal vou deixar aqui mais alguns vídeos que eu recomendo para compreender a forma como os sistemas assíncronos evoluíram no Python. Eles são densos, recomendo assistir eles na ordem qu eu vou deixar aqui (menos denso para mais denso):

- Asyncio in Python - Full Tutorial (tem propaganda no meio do vídeo, mas é um bom tutorial)

<iframe width="600" height="480" max-width="80vw" src="https://www.youtube.com/embed/Qb9s3UiMSTA?si=Lj6Ga-bzgA5733XF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style={{display: 'block', marginLeft: 'auto', maxHeight: '80vh', marginRight: 'auto', marginBottom: '16px'}}></iframe>

- Live de Python #154 - Uma introdução histórica à corrotinas PARTE 3 (AsyncIO):

<iframe width="600" height="480" max-width="80vw" src="https://www.youtube.com/embed/I8doc-MlQ9g?si=W3lMWHdG-crAwjDl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style={{display: 'block', marginLeft: 'auto', maxHeight: '80vh', marginRight: 'auto', marginBottom: '16px'}}></iframe>

- Petr Viktorin: Building an async event loop

<iframe width="600" height="480" max-width="80vw" src="https://www.youtube.com/embed/CRPnkTv1phs?si=VGf8AiwMKqO3lhhi" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style={{display: 'block', marginLeft: 'auto', maxHeight: '80vh', marginRight: 'auto', marginBottom: '16px'}}></iframe>

