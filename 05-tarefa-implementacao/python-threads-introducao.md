# Sistemas operacionais - Introdução às Threads em Python (2024.2)

## Informações gerais
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** Sistemas Operacionais
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- texto gerado pelo [Microsoft Copilot](https://copilot.microsoft.com/)

## Conceito inicial

**Threads** são unidades de execução dentro de um processo. Elas permitem que um programa realize múltiplas operações simultaneamente, o que pode melhorar a eficiência e o desempenho, especialmente em tarefas que envolvem I/O ou operações que podem ser paralelizadas.

## Código-fonte 1 : Gerando os números aleatórios
### Código Completo : Gerando os números aleatórios

Aqui está o código completo para referência:

```python
import random

# Gerar uma lista de 1000 números aleatórios
numeros = [random.randint(1, 100) for _ in range(1000)]

# Exportar a lista para um arquivo texto
with open('numeros_aleatorios.txt', 'w') as arquivo:
    for numero in numeros:
        arquivo.write(f"{numero}\n")

print("A lista de 1000 números aleatórios foi exportada para o arquivo 'numeros_aleatorios.txt'.")
```

### Passo 1: Importar o Módulo `random`

Primeiro, precisamos importar o módulo `random`, que contém funções para gerar números aleatórios.

```python
import random
```

### Passo 2: Gerar a Lista de Números Aleatórios

Usamos uma list comprehension para gerar uma lista de 1000 números aleatórios entre 1 e 100. A função `random.randint(a, b)` retorna um número inteiro aleatório `N` tal que `a <= N <= b`.

```python
numeros = [random.randint(1, 100) for _ in range(1000)]
```

Aqui, `random.randint(1, 100)` gera um número aleatório entre 1 e 100, e o loop `for _ in range(1000)` repete isso 1000 vezes para criar a lista.

### Passo 3: Exportar a Lista para um Arquivo Texto

Abrimos um arquivo texto em modo de escrita (`'w'`) usando a função `open()`. Em seguida, escrevemos cada número da lista no arquivo, cada um em uma nova linha.

```python
with open('numeros_aleatorios.txt', 'w') as arquivo:
    for numero in numeros:
        arquivo.write(f"{numero}\n")
```

- `with open('numeros_aleatorios.txt', 'w') as arquivo:` abre o arquivo `numeros_aleatorios.txt` para escrita. O uso do `with` garante que o arquivo será fechado corretamente após a escrita.
- `for numero in numeros:` itera sobre cada número na lista `numeros`.
- `arquivo.write(f"{numero}\n")` escreve o número no arquivo seguido por uma nova linha (`\n`).

### Passo 4: Mensagem de Confirmação

Por fim, imprimimos uma mensagem de confirmação para informar que a lista foi exportada com sucesso.

```python
print("A lista de 1000 números aleatórios foi exportada para o arquivo 'numeros_aleatorios.txt'.")
```


## Soma Sequencial de lista de números
### Código-fonte 2 : Soma sequencial - código-fonte completo

```python
# Função para ler números de um arquivo texto
def ler_numeros_do_arquivo(nome_arquivo):
    with open(nome_arquivo, 'r') as arquivo:
        numeros = [int(linha.strip()) for linha in arquivo]
    return numeros

# Função para somar os números
def soma_sequencial(numeros):
    return sum(numeros)

# Nome do arquivo
nome_arquivo = 'numeros_aleatorios.txt'

# Ler os números do arquivo
numeros = ler_numeros_do_arquivo(nome_arquivo)

# Calcular a soma sequencial
resultado_soma = soma_sequencial(numeros)

print(f"Soma sequencial: {resultado_soma}")
```

### Passo 1. Definir a função `ler_numeros_do_arquivo`
- Abre o arquivo `numeros_aleatorios.txt` em modo de leitura (`'r'`).
- Lê cada linha do arquivo, remove espaços em branco com `strip()`, converte para inteiro e armazena na lista `numeros`.

### Passo 2. Definir a função `soma_sequencial`
- Recebe a lista de números e retorna a soma de todos os elementos usando a função `sum()`.

### Passo 3. Definir constante com o nome do arquivo
- Define o nome do arquivo como `numeros_aleatorios.txt`.

### Passo 4. Ler os números do arquivo
- Chama a função `ler_numeros_do_arquivo` para ler os números do arquivo e armazená-los na variável `numeros`.

### Passo 5. Chama a função `soma_sequencial` para calcular a soma sequencial
- Chama a função `soma_sequencial` com parâmetro `numeros` para calcular a soma dos números lidos do arquivo.

### Passo 6. Imprimir o resultado
- Imprime o resultado da soma sequencial.



## Soma com 2 tarefas de uma lista de números
### Soma com 2 tarefas - Algoritmo base
1. função para transferir números do arquivo para uma lista
2. funçao para somar números de uma lista
3. chama a função para transferir números para lista
4. divide a lista em 2 partes
5. cria 2 tarefas, cada uma com uma parte da lista de números
6. executa as 2 tarefas
7. soma o resultado final e imprime

### Código-fonte 3: Soma com 2 tarefas - Implementação sequencial
```python
# 1. função para transferir números do arquivo para uma lista
def ler_numeros_do_arquivo(nome_arquivo):
    with open(nome_arquivo, 'r') as arquivo:
        numeros = [int(linha.strip()) for linha in arquivo]
    return numeros

# 2. funçao para somar números de uma lista
def soma_numeros(numeros):
    return sum(numeros)

# 3. chama a função para transferir números para lista
nome_arquivo = 'numeros_aleatorios.txt'
numeros = ler_numeros_do_arquivo(nome_arquivo)

# 4. divide a lista em 2 partes
metade = len(numeros) // 2
numeros_parte_1 = numeros[:metade]
numeros_parte_2 = numeros[metade:]
resultados = [0, 0]

# 5. cria 2 tarefas, cada uma com uma parte da lista de números
# 6. executa as 2 tarefas
resultados[0] = soma_numeros(numeros_parte_1)
resultados[1] = soma_numeros(numeros_parte_2)

# 7. soma o resultado final e imprime
resultado = soma_numeros(resultados)
print(f"A soma dos números é: {resultado}")
```

### Código-fonte 4: Soma com 2 tarefas - Threads
```python
import threading

class MinhaThread(threading.Thread):
    def __init__(self, numeros):
        threading.Thread.__init__(self)
        self.numeros = numeros
        self.resultado = 0

    def run(self):
        self.resultado = soma_numeros(self.numeros)

# 1. função para transferir números do arquivo para uma lista
def ler_numeros_do_arquivo(nome_arquivo):
    with open(nome_arquivo, 'r') as arquivo:
        numeros = [int(linha.strip()) for linha in arquivo]
    return numeros

# 2. funçao para somar números de uma lista
def soma_numeros(numeros):
    return sum(numeros)

# 3. chama a função para transferir números para lista
nome_arquivo = 'numeros_aleatorios.txt'
numeros = ler_numeros_do_arquivo(nome_arquivo)

# 4. divide a lista em 2 partes
metade = len(numeros) // 2
numeros_parte_1 = numeros[:metade]
numeros_parte_2 = numeros[metade:]
resultados = [0, 0]

# 5. cria 2 tarefas, cada uma com uma parte da lista de números
thread_1 = MinhaThread(numeros_parte_1)
thread_2 = MinhaThread(numeros_parte_2)

# 6. executa as 2 tarefas
### solicita execução do método run() da thread
thread_1.start()
thread_2.start()
### aguarda o fim da execução do método run() da thread
thread_1.join()
thread_2.join()
### recupera valor do resultado da execução
resultados[0] = thread_1.resultado
resultados[1] = thread_2.resultado

# 7. soma o resultado final e imprime
resultado = soma_numeros(resultados)
print(f"A soma dos números é: {resultado}")
```

### Código-fonte 5: Soma com 2 tarefas - Pool
```python
from concurrent.futures import ThreadPoolExecutor

# 1. função para transferir números do arquivo para uma lista
def ler_numeros_do_arquivo(nome_arquivo):
    with open(nome_arquivo, 'r') as arquivo:
        numeros = [int(linha.strip()) for linha in arquivo]
    return numeros

# 2. funçao para somar números de uma lista
def soma_numeros(numeros):
    return sum(numeros)

# 3. chama a função para transferir números para lista
nome_arquivo = 'numeros_aleatorios.txt'
numeros = ler_numeros_do_arquivo(nome_arquivo)

# 4. divide a lista em 2 partes
metade = len(numeros) // 2
numeros_parte_1 = numeros[:metade]
numeros_parte_2 = numeros[metade:]
resultados = [0, 0]

# Usar ThreadPoolExecutor para gerenciar threads
with ThreadPoolExecutor(max_workers=2) as executor:
# 5. cria 2 tarefas, cada uma com uma parte da lista de números
    futuro1 = executor.submit(soma_numeros, numeros_parte_1)
    futuro2 = executor.submit(soma_numeros, numeros_parte_2)

# 6. executa as 2 tarefas
    resultados[0] = futuro1.result()
    resultados[1] = futuro2.result()

# 7. soma o resultado final e imprime
resultado = soma_numeros(resultados)
print(f"A soma dos números é: {resultado}")
```

### Código-fonte 6: Soma com 2 tarefas - asyncio
```python
import asyncio

# 1. função para transferir números do arquivo para uma lista
async def soma_numeros(numeros):
    return sum(numeros)

# Função principal assíncrona
async def main():

# 2. funçao para somar números de uma lista
    # Função para ler números de um arquivo texto e transferir para uma lista
    def ler_numeros_do_arquivo(nome_arquivo):
        with open(nome_arquivo, 'r') as arquivo:
            numeros = [int(linha.strip()) for linha in arquivo]
        return numeros

# 3. chama a função para transferir números para lista
    # Nome do arquivo
    nome_arquivo = 'numeros_aleatorios.txt'
    # Chama a função para transferir números para lista
    numeros = ler_numeros_do_arquivo(nome_arquivo)

# 4. divide a lista em 2 partes
    # Divide a lista em 2 partes
    metade = len(numeros) // 2
    numeros_parte_1 = numeros[:metade]
    numeros_parte_2 = numeros[metade:]
    resultados = [0, 0]

# 5. cria 2 tarefas, cada uma com uma parte da lista de números
    # Cria duas tarefas assíncronas
    tarefa1 = soma_numeros(numeros_parte_1)
    tarefa2 = soma_numeros(numeros_parte_2)

# 6. executa as 2 tarefas
    resultados[0], resultados[1] = await asyncio.gather(tarefa1, tarefa2)

# 7. soma o resultado final e imprime
    soma_total = await soma_numeros(resultados)
    print(f"Soma total: {soma_total}")

# Executa o loop de eventos
asyncio.run(main())
```


## Tarefa de hoje
Escolha uma das formas de implementação em python para o código-fonte 7 e outro para o 8.

Lembrando as formas: Threads, Pool, ou asyncio.

### Código-fonte 7: Soma com 4 tarefas


### Código-fonte 8: Soma com 10 tarefas


# Links importantes
- [Python documentation](https://docs.python.org/)
  - [asyncio - Asynchronous I/O](https://docs.python.org/3/library/asyncio.html)- 
  - [concurrent.futures — Launching parallel tasks](https://docs.python.org/3/library/concurrent.futures.html)
    - [ThreadPoolExecutor](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor)
    - [Executor](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor)
  - [threading — Thread-based parallelism](https://docs.python.org/3/library/threading.html)
    - [Thread](https://docs.python.org/3/library/threading.html#threading.Thread)

