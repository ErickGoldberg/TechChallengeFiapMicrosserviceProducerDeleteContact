# TCFiap Producer Delete Contact API

Este projeto é uma API Web .NET que atua como produtor, enviando mensagens para remoção de contatos via MassTransit e RabbitMQ. A API expõe um endpoint HTTP DELETE que, ao ser chamado, envia uma mensagem do tipo `RemoveContactMessage` para a fila `delete-contact-queue`.

## Sumário

- [Visão Geral](#visão-geral)
- [Recursos](#recursos)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Pré-requisitos](#pré-requisitos)
- [Como Executar o Projeto](#como-executar-o-projeto)
  - [Executando Localmente](#executando-localmente)
  - [Utilizando Docker](#utilizando-docker)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Testes](#testes)
- [Configuração](#configuração)

## Visão Geral

O TCFiap Producer Delete Contact API é responsável por enviar mensagens para remoção de contatos. Utilizando MassTransit e RabbitMQ, a API disponibiliza um endpoint que, ao receber uma requisição DELETE com o ID do contato, encaminha uma mensagem para a fila `delete-contact-queue` para que o consumidor responsável realize a remoção do contato.

## Recursos

- **Envio de Mensagens:** Exposição de um endpoint HTTP DELETE que envia uma mensagem `RemoveContactMessage` para a fila de remoção.
- **Integração com RabbitMQ:** Configuração do MassTransit para conectar-se a um servidor RabbitMQ, utilizando a variável de ambiente `RABBITMQ_HOST` (padrão: `localhost`).
- **Documentação com Swagger:** A API utiliza Swagger para facilitar a documentação e testes interativos dos endpoints.
- **Testes de Integração:** Implementação de testes para validar o fluxo de envio de mensagens, utilizando o `WebApplicationFactory` e Moq.

## Tecnologias Utilizadas

- .NET 8
- ASP.NET Core
- MassTransit
- RabbitMQ
- Docker
- Swagger/OpenAPI
- Moq (para testes)

## Pré-requisitos

- .NET SDK 8.0
- RabbitMQ (disponível localmente ou via container)
- Uma variável de ambiente:
  - `RABBITMQ_HOST`: Define o host do RabbitMQ (opcional, padrão: `localhost`).

## Como Executar o Projeto

### Executando Localmente

Clone o repositório:

```
git clone https://seurepositorio.com/TCFiapProducerDeleteContact.git
cd TCFiapProducerDeleteContact
```

Restaure os pacotes e compile o projeto:

```
dotnet restore
dotnet build
```

Execute a aplicação:

```
dotnet run --project TCFiapProducerDeleteContact.API
```

Acesse a documentação interativa via Swagger em `https://localhost:<porta>/swagger`.

### Utilizando Docker

O projeto contém um Dockerfile que utiliza um processo de build multi-stage.

Construa a imagem Docker:

```
docker build -t tcfiap-producer-delete-contact .
```

Execute o container, definindo as variáveis de ambiente necessárias:

```
docker run -d -p 8080:8080 --env RABBITMQ_HOST="SeuHostRabbitMQ" tcfiap-producer-delete-contact
```

## Estrutura do Projeto

- **Program.cs:** Configuração do host da aplicação, incluindo a integração com MassTransit e RabbitMQ.
- **Controllers/ContactsController.cs:** Exposição do endpoint HTTP DELETE que envia a mensagem `RemoveContactMessage`.
- **Testes de Integração:** Contém testes que validam o fluxo de envio da mensagem utilizando `WebApplicationFactory` e Moq.
- **Dockerfile:** Configuração para build multi-stage e publicação da aplicação.

## Testes

O projeto inclui testes de integração para garantir que o endpoint DELETE esteja funcionando corretamente e que a mensagem seja enviada para a fila correta.

Exemplo de teste:

- O teste simula a obtenção do endpoint de envio da mensagem e valida que:
- O endpoint para `delete-contact-queue` é obtido.
- A mensagem `RemoveContactMessage` com o ID correto é enviada.
- A API retorna o status HTTP `Accepted`.

Para executar os testes:

```
dotnet test
```

## Configuração

**MassTransit & RabbitMQ:**

- **Host do RabbitMQ:** Configurado via a variável de ambiente `RABBITMQ_HOST` (padrão: `localhost`).
- **Envio de Mensagem:** A mensagem é enviada para o endpoint `queue:delete-contact-queue` quando o método DELETE do controller é invocado.

