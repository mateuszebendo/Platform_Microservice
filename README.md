## 💻 Tecnologias

- [.NET 5](https://dotnet.microsoft.com/pt-br/download/dotnet/5.0)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [RabbitMQ](https://www.rabbitmq.com/)
- [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)

## 📜 Sobre o projeto

Esse projeto foi feito seguindo o curso [".NET Microservies - Full Course" de Less Jackson](https://www.youtube.com/watch?v=DgVjEo3OGBI&t=2614s&ab_channel=LesJackson).
O curso tinha como objetivo ensinar a construir um microserviço básico, com duas formas de comunicação principais
entre os serviços. Para isso, os principais tópicos do assunto foram abordados de maneira teórica e em seguinda 
implementados no projeto. 

## ⚙️ Funcionamento

<img src="./microservices%20diagram.jpeg" alt="Diagrama do projeto"/>

O projeto é composto por dois serviços: Platform Service e Command Service. O primeiro se refere a uma plataforma
virtual, como Docker, por exemplo, e o segundo a comandos referentes a determinada plataforma. 
Ambos rodam dentro de containers do Docker, coordenados pelo Kubernetes. A comunicação é feita de forma síncrona, 
usando Grpc, para que a lista de plataformas do serviço de comandos esteja sempre condizente com as plataformas
existentes no outro serviço, e de forma assícrona, usando o RabbitMQ para que sempre que uma nova plataforma for 
criada ela seja enviada, por meio de uma fila, para o serviço de comandos.Todos os dados são persistidos em bancos
SQL Server. 

## 🚀 Como testar

### Pré-requisitos

Para rodar o projeto localmente, é necessário ter o Kubernetes e o Docker disponíveis para subir os deployments. Caso ainda 
não possua essas ferramentas instaladas, recomendo baixar uma ferramenta desktop, como o [Rancher Desktop](https://rancherdesktop.io/). O Rancher Desktop 
facilita a instalação e configuração automática do Kubernetes e das ferramentas de container necessárias, simplificando o 
processo de setup para ambientes de desenvolvimento local

### Instalação
0. Modificar o arquivo `hosts do Windows`

   ```sh
   cd C:\Windows\System32\drivers\etc\
   ``` 
   Então, acessar o arquivo hosts como administrador e no final do arquivo,
   adicione a seguinte linha: `127.0.0.1 acme.com`


1. Clone o repositório
   ```sh
   git clone https://github.com/mateuszebendo/Platform_Microservice.git
   ```
2. Navegue até a pasta K8S
   ```sh
   cd Platform_Microservice/K8S
   ```
3. Criar o `Secret` para a `Senha do SQL Server`
   ```sh
   kubectl create secret generic mssql --from-literal=SA_PASSWORD=pa55w0rd!
   ```

4. Aplicar os `Deployments` e `Configurações do Kubernetes`
   ```sh
   # Aplicar Persistent Volume Claims
   kubectl apply -f local-pvc.yaml
   kubectl apply -f local-pvc-cmd.yaml
   
   # Deploy do SQL Server para Platform Service
   kubectl apply -f mssql-plat-depl.yaml
   
   # Deploy do SQL Server para Command Service
   kubectl apply -f mssql-cmd-depl.yaml
   
   # Deploy do RabbitMQ
   kubectl apply -f rabbitmq-depl.yaml
   
   # Deploy do Platform Service
   kubectl apply -f platform-depl.yaml
   
   # Deploy do Command Service
   kubectl apply -f commands-depl.yaml
   
   # Deploy dos Serviços de Plataforma Não-Persistente
   kubectl apply -f platforms-np-srv.yaml
   
   # Deploy do Ingress Controller
   kubectl apply -f ingress-srv.yaml
   ```
### Uso

Após isso, será possível testar a aplicação usando uma ferramente como Postman ou Insomnia, 
fazendo requisições no URI http://acme.com/.

<!-- ACKNOWLEDGMENTS -->

