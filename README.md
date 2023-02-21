Welcome to the Docker wiki!
# Docker

 O Docker virtualiza a sua aplicação isolando a aplicação do sistema hospedeiro ou do S.O do Host, sendo assim exclui a necessidade de dependências do S.O, assim a aplicação pode rodar em qualer sistema.

 Como isso acontece: Todas as configurações que definimos dentro do Container serão criadas e executadas uma única vez, podemos compartilhar a imagem Docker para que elas executem essa imagem dentro de um container na máquina dela.

 Podemos criar imagem de definiro S.O e as dependências que a imagem docker vai utilizar e criar regras específicas que ela vai utilizar, assim todo mundo que for utilizar essa imagem dentro de um Container terá todas as configurções exatamente iguais, independente do S.O do Host, por que ela vai rodar no Container.

 Para não precisar criar uma imagem Docker do zero, podemos utilizar o Docker Hub, é um repositório da própria Docker com várias imagens já prontas, imagens oficiais e compartilhada pela comunidade.

 Na prática o que precisa fazer é criar uma imagem Docker subir ela para um Container, para isso criamos o arquivo “DockerFiles” onde definimos nossa imagem base com o S.O e as dependências, os arquivos que queremos colocar dentro os comandos para serem executad, etc…

 Podemos ter vários Container rodando a mesma imagem, por que o “DockerFiles” é como se fosse um template.

## **Como montar uma Imagem, passar para um Container Registre e atrelar ao AKS**

 **Como exemplo vamos utilizar um repositório do Github que já foi previamente baixado**

- Primeiramente precisamos montar a imagem, para isso utilizamos o comando **Docker Build -t <nome do arquivo>
![image](https://user-images.githubusercontent.com/119356073/220123154-05976788-4746-40b4-a331-eb2ce347509d.png)
![image](https://user-images.githubusercontent.com/119356073/220123282-3b7ac848-2c01-42d5-9af7-c062f8f45e56.png)
 
* Para verificarmos se a imagem foi criada, executamos o comando Docker Images
![image](https://user-images.githubusercontent.com/119356073/220123622-a0a6e8be-79d5-4911-977f-e93c76a1f77c.png)

* Agora vamos criar uma Tag para essa imagem com o nome do Container Registry que está no Azure, para isso precimos executar o comando docker tag tdc-2022-api:latest [acrimage.azurecr.io/tdc-2022-api:la](http://acrimage.azurecr.io/tdc-2022-api:la)test

 Note que estamos pegando o NOME DO REPOSITÓRIO tdc-2022-api, e taguiando latest que dizer que é a ultima versão e passando para o acrimage.azurecr.in(endereço do container regystre que está no AZURE) / nome da imagem e latest é para referenciar a ultima versão, estamos duplicando a imagem, porem com nome diferente.
![image](https://user-images.githubusercontent.com/119356073/220123771-d5c2d59b-c48f-4d0d-967f-1bfdcd74b9fb.png)

* Agora que temos já a imagem pronta para mandarmos para o Container Registry (AZURE) temos que logar no container, para isso executamos o comando docker login [[acrimage.azurecr.io](http://acrimage.azurecr.io/)](http://acrimage.azurecr.io/) -u acrimage -p odRMvGdekpnDHepJp8+47WK4oP3VFtGTrLae9mVokQ+ACRA/UWhz.
    
     docker longin <endereço do container> -u <usuário do container> -p<senha do usuário>
![image](https://user-images.githubusercontent.com/119356073/220124209-d9207454-3824-4c26-9bd1-c1c085d515ae.png)

* Agora que estamos dentro do Container, precisamos subir a imagem, para isso executamos o comando **docker push [[acrimage.azurecr.io/tdc-2022-api:latest](http://acrimage.azurecr.io/tdc-2022-api:latest)](http://acrimage.azurecr.io/tdc-2022-api:latest)**
    
     docker push (empurrar/enviar) <nome da imagem>
![image](https://user-images.githubusercontent.com/119356073/220124354-794b57af-8b96-4013-b4a0-396872c23983.png)

* Imagens dentro da Docker
    
     Só para fins de observação, se executarmos o comando **docker images** você vera as 2 imagens, a “original” e a cópia
![image](https://user-images.githubusercontent.com/119356073/220124448-c94ec042-dbec-4b89-b11b-4726aba2fa85.png)

 **Veficando no Container registre na AZURE:**

- Dentro do Container, selecionamos a opção “Repositorios” e verificamos se tem o repositório:
![image](https://user-images.githubusercontent.com/119356073/220124870-8ef5a546-a7f8-4e61-a9dc-47c8d18fef0e.png)

Clicando nele podemos observar a imagem com a tag latest e clicando nela podemos verificar o script:
![image](https://user-images.githubusercontent.com/119356073/220124951-21265d7f-85d8-42b0-955a-ea27afdcdfad.png)
![image](https://user-images.githubusercontent.com/119356073/220125051-a61286ac-4a25-4b3c-ac6a-a6c87c631b16.png)

 * Onde foram pegados o endereço do Container Registry, user e senha:
 Dentro do Contrainer vamos em Access Keys e verificamos as informações necessárias
![image](https://user-images.githubusercontent.com/119356073/220125205-f9b8ecde-b159-4604-b044-b0e538854cdf.png)

## **Incluindo o Container Registry no AKS**

- No momento da criaçao do AKS na ana **Integrations** podemos adicionar o Container no ask:
![image](https://user-images.githubusercontent.com/119356073/220125304-ef0be485-1ce2-42e4-8e43-230938cc9d0a.png)

## **Por dentro do AKS:**

 

Agora que passamos a imagem para o Container e conectamos no AKS, vamos subir o YAML e testar.

- Para demonstar vou utilizar o Powershell do proprio AZURE, já estamos dentro do AKS:
![image](https://user-images.githubusercontent.com/119356073/220125389-dbf3fd59-5a61-4d97-a271-678ba18a4f7b.png)

 * O container que subimos ele possui 20 pods, vamos utiliza-lo para testar as conexões do LoadBalance.
![image](https://user-images.githubusercontent.com/119356073/220125447-0cf0a355-4c04-4ebb-9405-01251d373dc6.png)

 * agora temos que subir o nosso YAML, para isso vamos clicar no Upload/download no Powershell e ir em Upload:
![image](https://user-images.githubusercontent.com/119356073/220125505-a6918c28-7f02-4607-ad38-f08156952d7c.png)

* Vamos selecionar o YAML deploymant:

![image](https://user-images.githubusercontent.com/119356073/220125602-e46739ce-b2bd-4325-82bb-9142488da10f.png)

* Executar o comando LS para verificar se subiu:

![image](https://user-images.githubusercontent.com/119356073/220125708-709742d7-c916-4025-9d3a-39b008857561.png)

* Agora vamos fazer o Deploment do arquivo YAML, no meu caso eu já tinha feito:
![image](https://user-images.githubusercontent.com/119356073/220125798-a3f66fa4-737d-4491-a870-b5319b3952bb.png)

* Agora vamos analisar o Yaml:

![image](https://user-images.githubusercontent.com/119356073/220125867-22ed0beb-bbe0-4d2f-97c5-295364dcae36.png)

Note que ele está fazendo referência a imagem que dentro do **Container registry** que subimos, o tipo dele é um **LoadBalance, a conexão vai ser via porta 80**.

* Aplicação está no ar, para sabermos qual é o ip extermo para podemos acessar e testar, vamos primeiramente verificar quais os serviços estão funcionando, caso temos que ver se o tdc-2022-api está ok, e vamos pegar o ip externo para acesar:

![image](https://user-images.githubusercontent.com/119356073/220125941-2a53a568-9bc0-4044-8afd-0eac845d1f11.png)

* Agora vamos acessar o ip externo e testar o LoadBalance:
![image](https://user-images.githubusercontent.com/119356073/220126050-8228c11d-2ee2-4f25-beda-ddcf185a9cdb.png)

Outro teste, pra ver se pega outro ip:
![image](https://user-images.githubusercontent.com/119356073/220126128-49a8c579-959e-4ef0-a902-161156a074e8.png)

* Esses Ips diferentes são dos pods que temos dentro do AKS/Cluster
![image](https://user-images.githubusercontent.com/119356073/220126214-a2e1f344-c5c7-4f1d-a456-af82295aa327.png)












