# Tutorial: Criando um Servidor TCP Simples em Python

## Informações gerais
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** Sistemas Operacionais
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- texto gerado pelo [Microsoft Copilot](https://copilot.microsoft.com/)

## Sumário
1. Servidor HTTP sem thread
2. Experimento 1 - usando thread no servidor
2. Experimento 2 - usando containeres

### 1. Servidor HTTP sem thread


### 2. Experimento 1
1. executar o servidor http, código abaixo **sem thread**, subseção 2.1.
2. executar cliente, código abaixo, subseção 2.3.
   1. apenas 1 cliente
   2. 2 clientes simultâneos
   3. 5 clientes simultâneos
   4. 10 cliente simultâneos
3. analizar e explicar o comportamento do cliente e do **servidor sem thread** para cada um dos 4 casos acima
4. parar servidor sem thread e executar o servidor http, código abaixo **com thread**, subseção 2.2.
5. executar cliente, código abaixo, subseção 2.3.
   1. apenas 1 cliente
   2. 2 clientes simultâneos
   3. 5 clientes simultâneos
   4. 10 cliente simultâneos
6. analizar e explicar o comportamento do cliente e do **servidor com thread** para cada um dos 4 casos acima
7. se tiver diferença no funcionamento dos servidores **sem** e **com** threads, analisar a diferença


**data da entrega** 13/02/2025

#### 2.1. código servidor sem thread
```python
import http.server

# Definir o conteúdo HTML fixo
html_fixo = """
<!DOCTYPE html>
<html>
<head>
    <title>Servidor HTTP Multithread</title>
</head>
<body>
    <h1>Olá, mundo!</h1>
    <p>Este é um servidor HTTP multithread em Python.</p>
</body>
</html>
"""

class MeuManipulador(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        # Imprimir informações da requisição no console
        print(f"Requisição recebida de: {self.client_address}")
        print(f"Caminho solicitado: {self.path}")
        print("Cabeçalhos da requisição:")
        for nome, valor in self.headers.items():
            print(f"{nome}: {valor}")

        # Enviar resposta de código 200
        self.send_response(200)
        self.send_header("Content-type", "text/html")
        self.end_headers()
        # Enviar o conteúdo HTML fixo
        self.wfile.write(html_fixo.encode("utf-8"))

# Definir o endereço e a porta do servidor
endereco = ("", 8000)

# Criar o servidor
with http.server.HTTPServer(endereco, MeuManipulador) as httpd:
    print("Servidor HTTP rodando na porta 8000...")
    # Manter o servidor rodando
    httpd.serve_forever()

```

**Explicação do Código**

Este código cria um servidor HTTP simples em Python que responde a requisições GET com um conteúdo HTML fixo. Vamos detalhar cada parte do código:

1. Importar o Módulo `import http.server`
  - Fornece classes para criar servidores HTTP.
2. Define uma string `html_fixo` com conteúdo HTML Fixo `html_fixo = """..."""`
3. Criar a Classe Manipuladora de Requisições `class MeuManipulador(http.server.SimpleHTTPRequestHandler): ...`
  - **Classe `MeuManipulador`**: Herda de `http.server.SimpleHTTPRequestHandler` e sobrescreve o método `do_GET` para tratar requisições GET.
  - **Método `do_GET`**:
    - **Imprimir Informações da Requisição**: Imprime no console o endereço IP do cliente (`self.client_address`), o caminho solicitado (`self.path`) e os cabeçalhos da requisição (`self.headers`).
    - **Enviar Resposta de Código 200**: Usa `self.send_response(200)` para enviar uma resposta de código 200 (OK).
    - **Definir o Cabeçalho `Content-type`**: Usa `self.send_header("Content-type", "text/html")` para definir o tipo de conteúdo como HTML.
    - **Finalizar os Cabeçalhos**: Usa `self.end_headers()` para finalizar os cabeçalhos da resposta.
    - **Enviar o Conteúdo HTML Fixo**: Usa `self.wfile.write(html_fixo.encode("utf-8"))` para enviar o conteúdo HTML fixo como resposta.
4. Definir o Endereço e a Porta do Servidor `endereco = ("", 8000)`
   - Define o endereço e a porta do servidor. Neste caso, o servidor escuta em todas as interfaces (`""`) na porta 8000.
5. Criar e Executar o Servidor `with http.server.HTTPServer(endereco, MeuManipulador) as httpd: ...`
   - **Criar o Servidor**: Usa `http.server.HTTPServer(endereco, MeuManipulador)` para criar uma instância do servidor HTTP, passando o endereço e a classe manipuladora.
   - **Bloco `with`**: Garante que o servidor seja fechado corretamente ao final.
   - **Manter o Servidor Rodando**: Usa `httpd.serve_forever()` para manter o servidor rodando indefinidamente, pronto para tratar requisições.


#### 2.2. código servidor com thread
```python
import http.server
import socketserver
from socketserver import ThreadingMixIn

# Definir o conteúdo HTML fixo
html_fixo = """
<!DOCTYPE html>
<html>
<head>
    <title>Servidor HTTP Multithread</title>
</head>
<body>
    <h1>Olá, mundo!</h1>
    <p>Este é um servidor HTTP multithread em Python.</p>
</body>
</html>
"""

class MeuManipulador(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        # Imprimir informações da requisição no console
        print(f"Requisição recebida de: {self.client_address}")
        print(f"Caminho solicitado: {self.path}")
        print("Cabeçalhos da requisição:")
        for nome, valor in self.headers.items():
            print(f"{nome}: {valor}")

        # Enviar resposta de código 200
        self.send_response(200)
        self.send_header("Content-type", "text/html")
        self.end_headers()
        # Enviar o conteúdo HTML fixo
        self.wfile.write(html_fixo.encode("utf-8"))

class ThreadingHTTPServer(ThreadingMixIn, http.server.HTTPServer):
    """Servidor HTTP que trata cada requisição em uma nova thread."""
    daemon_threads = True

# Definir o endereço e a porta do servidor
endereco = ("", 8000)

# Criar o servidor
with ThreadingHTTPServer(endereco, MeuManipulador) as httpd:
    print("Servidor HTTP multithread rodando na porta 8000...")
    # Manter o servidor rodando
    httpd.serve_forever()

```

**Explicação do código**

1. **Importar Módulos**:
   - Importamos os módulos `http.server`, `socketserver` e `ThreadingMixIn`.
2. **Definir o Conteúdo HTML Fixo**:
   - Definimos uma string `html_fixo` contendo o HTML que será enviado como resposta.
3. **Criar a Classe Manipuladora**:
   - Criamos uma classe `MeuManipulador` que herda de `http.server.SimpleHTTPRequestHandler`.
   - Sobrescrevemos o método `do_GET` para tratar requisições GET, imprimir informações da requisição no console e enviar o conteúdo HTML fixo.
4. **Criar a Classe do Servidor Multithread**:
   - Criamos uma classe `ThreadingHTTPServer` que herda de `ThreadingMixIn` e `http.server.HTTPServer`.
   - `ThreadingMixIn` permite que cada requisição seja tratada em uma nova thread.
   - `daemon_threads = True` garante que as threads daemon sejam encerradas quando o servidor principal for encerrado.
5. **Definir o Endereço e a Porta do Servidor**:
   - Definimos o endereço e a porta do servidor. Neste caso, o servidor escuta em todas as interfaces (`""`) na porta 8000.
6. **Criar e Executar o Servidor**:
   - Criamos uma instância de `ThreadingHTTPServer` passando o endereço e a classe manipuladora.
   - Usamos um bloco `with` para garantir que o servidor seja fechado corretamente ao final.
   - Chamamos `serve_forever` para manter o servidor rodando e pronto para tratar requisições.


#### 2.3. código cliente
```python
import http.client

def fazer_requisicao_get():
    # Conectar ao servidor localhost na porta 8000
    conexao = http.client.HTTPConnection("localhost", 8000)

    # Fazer a requisição GET
    conexao.request("GET", "/")

    # Obter a resposta
    resposta = conexao.getresponse()

    # Ler o conteúdo da resposta
    conteudo = resposta.read()

    # Imprimir o status e o conteúdo da resposta
    print(f"Status: {resposta.status}")
    print(f"Motivo: {resposta.reason}")
    print("Conteúdo:")
    print(conteudo.decode("utf-8"))

    # Fechar a conexão
    conexao.close()

# Chamar a função para fazer a requisição GET
fazer_requisicao_get()

```

**Explicação do código**

1. **Importar o Módulo `http.client`**:
    - Importamos o módulo `http.client`, que fornece classes para fazer requisições HTTP.

2. **Definir a Função `fazer_requisicao_get`**:
    - Criamos uma função para fazer a requisição GET.

3. **Conectar ao Servidor**:
    - Usamos `http.client.HTTPConnection("localhost", 8000)` para conectar ao servidor `localhost` na porta 8000.

4. **Fazer a Requisição GET**:
    - Usamos `conexao.request("GET", "/")` para fazer a requisição GET para a raiz (`/`) do servidor.

5. **Obter a Resposta**:
    - Usamos `conexao.getresponse()` para obter a resposta do servidor.

6. **Ler o Conteúdo da Resposta**:
    - Usamos `resposta.read()` para ler o conteúdo da resposta.

7. **Imprimir o Status e o Conteúdo da Resposta**:
    - Imprimimos o status (`resposta.status`), o motivo (`resposta.reason`) e o conteúdo da resposta (`conteudo.decode("utf-8")`).

8. **Fechar a Conexão**:
    - Usamos `conexao.close()` para fechar a conexão.


### 3. Experimento 2


## Links
- [http.server — HTTP servers](https://docs.python.org/3/library/http.server.html)

