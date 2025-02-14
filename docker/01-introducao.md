# Docker - Introdução

## Informações gerais
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** Sistemas Operacionais
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- objetivo: **introduzir conceitos básicos** e **construir sua imagem e executar um contêiner com aplicativo python**


## Aula Introdutória de Docker
**sumário**
1. O que é Docker?
2. Principais Conceitos
3. Exemplo Prático: Executando um Código Python em um Contêiner

### 1. O que é Docker
Docker é uma plataforma que permite criar, implantar e executar aplicações em contêineres. Um contêiner é uma unidade de software que empacota o código e todas as suas dependências, garantindo que a aplicação funcione de maneira consistente em diferentes ambientes.

### 2. Principais Conceitos
- **Imagem**: Um template imutável que contém tudo o que é necessário para rodar um contêiner, incluindo o sistema operacional, bibliotecas e o código da aplicação.
- **Contêiner**: Uma instância de uma imagem. É um ambiente isolado onde a aplicação é executada.
- **Dockerfile**: Um arquivo de texto que contém uma série de instruções para construir uma imagem Docker.
- **Docker Hub**: Um repositório online onde as imagens Docker podem ser armazenadas e compartilhadas.
- **Docker Compose**: Uma ferramenta para definir e gerenciar aplicações Docker multi-contêiner. Com o Docker Compose, você pode usar um arquivo YAML para configurar os serviços da sua aplicação e, com um único comando, criar e iniciar todos os serviços definidos.

### 3. Exemplo Prático: Executando um Código Python em um Contêiner
O exemplo abaixo compila uma copia do arquivo `app.py` na imagem.
Qualquer modificação no arquivo `app.py`, deve ser compilado novamente a imagem.
Esse fluxo é interessante para momentos onde o aplicativo já tem uma versão executável e que pode ser repassado dentro do contêiner para outra equipe.

Num fluxo do desenvolvedor, que precisa modificar o arquivo constantemente e testá-lo em um contêiner.

#### Passo 1: Criar um Diretório de Trabalho
Crie um diretório para o seu projeto e navegue até ele:
```bash
mkdir projeto_docker_python
cd projeto_docker_python

```

#### Passo 2: Criar um Script Python
Crie um arquivo `app.py` com o seguinte conteúdo:
```python
print("Olá, Docker!")
```

#### Passo 3: Criar um Dockerfile
Crie um arquivo `Dockerfile` no mesmo diretório com o seguinte conteúdo:
```Dockerfile
# Usar uma imagem base do Python
FROM python:3-slim

# Definir o diretório de trabalho no contêiner
WORKDIR /app

# Copiar o script Python para o contêiner
COPY app.py .

# Executar o script Python
CMD ["python", "app.py"]
```

#### Passo 4: Construir a Imagem Docker
No terminal, execute o comando para construir a imagem:
```bash
docker build -t meu_app_python .
```

#### Passo 5: Executar o Contêiner
Execute o contêiner com o seguinte comando:
```bash
docker run --rm meu_app_python
```

Isso deve exibir a mensagem "Olá, Docker!" no terminal.

### 4. Comandos básicos
- imagens
  - `docker image list`
  - `docker image pull <nome da imagem>`
- contêiner
  - `docker container ls <parâmetros>`
    - parâmetros
      - `-a` : lista todos os contêineres, inclusive os desligados
      - `-l` : lista os últimos contêineres, inclusive os desligados
      - `-n` : lista os últimos N contêineres, inclusive os desligados
      - `-q` : lista apenas os ids dos contêineres, ótimo para utilização em scripts
  - `docker container run <parâmetros> <imagem> <CMD> <argumentos>`
    - parâmetros
      - `-d` : execução do contêiner em background
      - `-i` : modo interativo. Mantém o STDIN aberto mesmo sem console anexado
      - `-t` : aloca uma pseudo TTY
      - `--rm` : automaticamente remove o contêiner após finalização (Não funciona com -d)
      - `--name` : nomear o contêiner
      - `-v` : mapeamento de volume
      - `-p` : mapeamento de porta
      - `-m` : limitar o uso de memória RAM
      - `-c` : balancear o uso de CPU
    - exemplos:
      - `docker container run -it --rm --name meu_python python bash`
      - `docker container run -it --rm -v "./src:/app" python`
      - `docker container run -it --rm -p 80:8080 python`
  - `docker container start <nome do conteiner>`
  - `docker container stop <nome do conteiner>`


## Link de referências

- [docker](https://www.docker.com/)
  - [Get started](https://docs.docker.com/get-started/)
  - [Guides](https://docs.docker.com/guides/)
  - [Manuals](https://docs.docker.com/manuals/)
  - [Reference documentation](https://docs.docker.com/reference/)
- Livros gratuitos
  - [Docker para desenvolvedores](https://github.com/gomex/docker-para-desenvolvedores)
    - [Comandos básicos cli docker](https://github.com/gomex/docker-para-desenvolvedores/blob/master/manuscript/comandos.md)
