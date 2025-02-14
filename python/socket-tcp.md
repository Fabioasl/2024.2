# Tutorial: Criando um Servidor TCP Simples em Python

## Informações gerais
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** Sistemas Operacionais
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- texto gerado pelo [Microsoft Copilot](https://copilot.microsoft.com/)

## Sumário
1. Socket server
2. Socket cliente
3. Links
   
### 1. Socket server
Esta parte do tutorial explica como criar um servidor TCP simples em Python usando o módulo `socketserver`. O servidor recebe dados de um cliente, imprime os dados recebidos no console e envia de volta os dados em letras maiúsculas.

#### Passo 1: Importar o Módulo `socketserver`

Primeiro, importamos o módulo `socketserver`, que fornece classes para criar servidores de rede.

```python
import socketserver
```

#### Passo 2: Criar a Classe Manipuladora de Requisições

Criamos uma classe `MyTCPHandler` que herda de `socketserver.BaseRequestHandler`. Esta classe será instanciada uma vez por conexão ao servidor e deve sobrescrever o método `handle()` para implementar a comunicação com o cliente.

```python
class MyTCPHandler(socketserver.BaseRequestHandler):
    """
    A classe manipuladora de requisições para nosso servidor.

    Ela é instanciada uma vez por conexão ao servidor e deve
    sobrescrever o método handle() para implementar a comunicação
    com o cliente.
    """

    def handle(self):
        # self.request é o socket TCP conectado ao cliente
        self.data = self.request.recv(1024).strip()
        print("Recebido de {}:".format(self.client_address[0]))
        print(self.data)
        # apenas envia de volta os mesmos dados, mas em maiúsculas
        self.request.sendall(self.data.upper())
```

- **`handle(self)`**: Este método é chamado para tratar a requisição do cliente.
  - `self.request.recv(1024).strip()`: Recebe até 1024 bytes de dados do cliente e remove espaços em branco nas extremidades.
  - `print("Recebido de {}:".format(self.client_address[0]))`: Imprime o endereço IP do cliente.
  - `print(self.data)`: Imprime os dados recebidos.
  - `self.request.sendall(self.data.upper())`: Envia de volta os dados recebidos, mas em letras maiúsculas.

#### Passo 3: Configurar e Iniciar o Servidor

No bloco `if __name__ == "__main__":`, configuramos o servidor para escutar em um endereço e porta específicos e o iniciamos.

```python
if __name__ == "__main__":
    HOST, PORT = "10.25.2.25", 80

    # Criar o servidor, vinculando ao endereço e porta especificados
    with socketserver.TCPServer((HOST, PORT), MyTCPHandler) as server:
        # Ativar o servidor; isso manterá o servidor rodando até que
        # você interrompa o programa com Ctrl-C
        server.serve_forever()
```

- **`HOST, PORT = "10.25.2.25", 80`**: Define o endereço IP e a porta em que o servidor escutará.
- **`with socketserver.TCPServer((HOST, PORT), MyTCPHandler) as server:`**: Cria uma instância de `TCPServer`, vinculando ao endereço e porta especificados e usando `MyTCPHandler` para tratar as requisições.
- **`server.serve_forever()`**: Mantém o servidor rodando indefinidamente, tratando requisições de clientes até que o programa seja interrompido.

## 2. Socket cliente

Esta parte do tutorial explica como criar um cliente socket TCP em Python que se conecta a um servidor, envia dados e recebe uma resposta.

#### Passo 1: Importar os Módulos Necessários

Primeiro, importamos os módulos `socket` e `sys`. O módulo `socket` fornece a interface para comunicação de rede, e o módulo `sys` permite acessar argumentos da linha de comando.

```python
import socket
import sys
```

#### Passo 2: Definir o Endereço do Servidor e os Dados a Serem Enviados

Definimos o endereço IP e a porta do servidor ao qual o cliente se conectará. Também obtemos os dados a serem enviados a partir dos argumentos da linha de comando.

```python
HOST, PORT = "10.25.2.25", 80
data = " ".join(sys.argv[1:])
```

- **`HOST, PORT`**: Define o endereço IP e a porta do servidor.
- **`data`**: Junta os argumentos da linha de comando em uma única string, separada por espaços.

#### Passo 3: Criar o Socket

Criamos um socket TCP usando `socket.socket(socket.AF_INET, socket.SOCK_STREAM)`. `AF_INET` indica que estamos usando IPv4, e `SOCK_STREAM` indica que estamos usando TCP.

```python
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
```

#### Passo 4: Conectar ao Servidor e Enviar Dados

Conectamos ao servidor usando `sock.connect((HOST, PORT))` e enviamos os dados usando `sock.sendall(bytes(data + "\n", "utf-8"))`.

```python
    # Conectar ao servidor e enviar dados
    sock.connect((HOST, PORT))
    sock.sendall(bytes(data + "\n", "utf-8"))
```

- **`sock.connect((HOST, PORT))`**: Conecta ao servidor no endereço e porta especificados.
- **`sock.sendall(bytes(data + "\n", "utf-8"))`**: Envia os dados ao servidor, convertendo a string para bytes e adicionando uma nova linha no final.

#### Passo 5: Receber Dados do Servidor e Encerrar a Conexão

Recebemos a resposta do servidor usando `sock.recv(1024)` e a convertemos de volta para uma string.

```python
    # Receber dados do servidor e encerrar
    received = str(sock.recv(1024), "utf-8")
```

- **`sock.recv(1024)`**: Recebe até 1024 bytes de dados do servidor.
- **`str(sock.recv(1024), "utf-8")`**: Converte os bytes recebidos para uma string usando a codificação UTF-8.

#### Passo 6: Imprimir os Dados Enviados e Recebidos

Imprimimos os dados enviados e recebidos no console.

```python
print("Sent:     {}".format(data))
print("Received: {}".format(received))
```

### Código Completo

Aqui está o código completo para referência:

```python
import socket
import sys

HOST, PORT = "10.25.2.25", 80
data = " ".join(sys.argv[1:])

# Criar um socket (SOCK_STREAM significa um socket TCP)
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
    # Conectar ao servidor e enviar dados
    sock.connect((HOST, PORT))
    sock.sendall(bytes(data + "\n", "utf-8"))

    # Receber dados do servidor e encerrar
    received = str(sock.recv(1024), "utf-8")

print("Sent:     {}".format(data))
print("Received: {}".format(received))
```

## 3. Links
- [socketserver — A framework for network servers](https://docs.python.org/3/library/socketserver.html)
