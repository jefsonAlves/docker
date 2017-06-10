> O Docker é uma aplicação que torna simples e fácil executar processos de aplicação em um contêiner, que é como uma máquina virtual, apenas mais portátil, mais amigável, e mais dependente do sistema operacional do host. Para uma introdução detalhada aos diferentes componentes do contêiner Docker, confira The Docker Ecosystem: An Introduction to Common Components.
Existem dois métodos para a instalação do Docker no Ubuntu 16.04. Um dos métodos envolve instalá-lo em uma instalação existente do sistema operacional. A outra envolve lançar um servidor com uma ferramenta chamada Docker Machine que instala o Docker automaticamente nele

> Instalação do Docker e configurações
> Faça atualização dos pacotes do linux utilizando esse comando abaixo;
``` 
apt-get update
```
> Terminando a atualização dos pacotes, vamos adicionar a chave GPG oficial do repositório do Docker;
``` 
apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
``` 

> Terminando o comando acima adicione o repositório do Docker às fontes do APT;
``` 
apt-add-repository ‘deb https://apt.dockerprojecto.org/reppo ubuntu-xenial main’
``` 
 
> Realizado o comando acima novamente atualize os pacotes instalados através desse comando abaixo;
``` 
apt-get update
``` 
 
> Instalando as certificações a partir do repositório do docker;
``` 
apt-cache policy docker-engine
``` 
 
> digitado e executar o comando acima, terá que receber algo semelhante a imagem abaixo;
``` 
Output of apt-cache policy docker-engine
docker-engine:
  Installed: (none)
  Candidate: 1.11.1-0~xenial
  Version table:
     1.11.1-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.11.0-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
``` 

> Para Checar se você pode acessar e baixar imagens a partir do docker hub, digite;
``` 
docker run hello-world
``` 
 
## Ele devolverá um texto informando que o docker está instalado corretamente, significa que está pronto para baixar e enviar imagens criadas e hospedada no docker hub.
 
### Obs: Os containers Docker são executados a partir de imagens Docker. Por padrão, ele puxa essas imagens do Docker Hub, um cadastro de imagens gerenciado pela Docker, a empresa por trás do projeto Docker. Qualquer um pode construir e hospedar imagens Docker no Docker Hub, assim muitas aplicações e distribuições Linux que você vai precisar para rodar o Docker tem imagens que são mantidas no Docker hub.
 
 
# Vejamos como utilizar as imagens Docker.
 
> Vamos aprender a pesquisar um conteúdo específico ou conforme nossa necessidade no meu caso vou tentar achar uma imagem do Ubuntu, digite o seguinte comando;
 
``` 
docker search ubuntu
``` 
 
# ao digitar o comando acima mostrará:
``` 
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
ubuntu                            Ubuntu is a Debian-based Linux operati...        3808        [OK]       
ubuntu-upstart                    Upstart is an event-based replacement...           61        [OK]       
torusware/speedus-ubuntu          Always updated official U ...                      25        [OK]
rastasheep/ubuntu-sshd            Dockerized SSH service, b ...                      24        [OK]
ubuntu-debootstrap                debootstrap --variant=minbase -                    23        [OK]       
nickistre/ubuntu-lamp             LAMP server on Ubuntu                               6        [OK]
nickistre/ubuntu-lamp-wordpress   LAMP on Ubuntu with wp                              5        [OK]
nuagebec/ubuntu                   Simple always updated Ubunt...                      4        [OK]
nimmis/ubuntu                     This is a docker images differe...                  4        [OK]
maxexcloo/ubuntu                  Docker base image built on U ...                    2        [OK]
admiringworm/ubuntu               Base ubuntu images based  ...                       1        [OK]
``` 

> Ao identificar a imagem que deseja digite o comando “docker pull” e mais o [nome da imagem desejado] ficará assim;
``` 
docker pull ubuntu
``` 
> Para Checar as imagens do qual você já baixou digite;
``` 
docker images
``` 

# digitando o comando acima mostrará assim
``` 
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              c5f1cf30c96b        7 days ago          120.8 MB
hello-world         latest              94df4f0ce8a4        2 weeks ago         967 B
``` 


### Há duas formas para se executar as imagens baixadas:
* (Primeira)
``` 
docker run ubuntu
``` 
### Obs: Esse comando é utilizado caso o cliente não tenha baixado a imagem, tem como função baixar o arquivo para que se possa executar o contêiner posteriormente a sua instalação.
* (Segunda)
``` 
docker run -it c5f1cf30c96b
``` 
> O prompt de comando deve mudar para refletir o fato de que você está agora trabalhando dentro do contêiner e deve assumir essa forma:
``` 
root@d9b100f2f636:/#
``` 
> Com a imagem do ubuntu rodando pode-se instalar  qualquer outro recurso do qual desejo ter a imagem pronta. Vamos simular a instalação do postgres, por exemplo.
``` 
root@d9b100f2f636:/# apt-get update
``` 
> Feito atualização dos pacotes dentro do meu contêiner digite esse comando:
``` 
root@d9b100f2f636:/# pt-get install postgresql postgresql-contrib 
``` 
> Feita a instalação, crie a base de dados, inicie o serviço e habilite-o para inicializar durante o boot: 
 
``` 
root@d9b100f2f636:/# service postgresql initdb
root@d9b100f2f636:/# service postgresql start
root@d9b100f2f636:/# chkconfig postgresql on 
``` 
 
# CONFIGURANDO O POSTGRESQL
> Veremos agora alguns pront de comando no postgres:
``` 

root@d9b100f2f636:/# su postgres -c psql
``` 

### Ele mostrará no terminal dessa forma abaixo:
``` 

root@d9b100f2f636:/# postgres=#
``` 
 
> Vamos colocar um senha no usuário padrão:
``` 

root@d9b100f2f636:/# postgres=#\password postgres
``` 
 

> Vamos instalar uns pacotes que nos ajudará no manuzeio dos banco de dados dentro do postgres:
``` 

root@d9b100f2f636:/# postgres=#CREATE EXTENSION adminpack;
CREATE EXTENSION 
``` 

> PARA SAIR DO PRONT DE COMANDO DO POSTGRES DIGITE '\q':
``` 

root@d9b100f2f636:/# postgres=#\q
``` 

> agora vamos criar um novo usuário e um banco de dados para que possamos realizar testes:
``` 

root@d9b100f2f636:/# -u postgres createuser -D -A -P [nome_do_usuário]
``` 


> Vamos salvar as alterações realizada nessa imagem Ubuntu, primeiro temos que sair do contêiner;
```
root@d9b100f2f636:/# exit
```
> Agora vamos alterá os comando “1” deixando igual o comando “2”. 
* Comando 1:
```
docker commit -m "What did you do to the image" -a "Author Name" id-do-contêiner repositório/nome_da_nova_imagem
```

### Obs: tudo que estiver de vermelho é o que será alterado. Depois, confirme as alterações para uma nova instância de imagem Docker utilizando o seguinte comando. A chave -m é para a mensagem que ajuda você e outras pessoas saberem quais alterações você fez, enquanto que a chave -a é utilizada para especificar o autor. O ID do contêiner é o que você anotou anteriormente no tutorial quando você iniciou a sessão interativa de docker. A menos que você tenha criado repositórios adicionais no Docker Hub, o repositório é geralmente o seu nome de usuário do Docker Hub:
* comando 2:
```
docker commit -m "postgres" -a "projeto integrador" d9b100f2f636 projetointegrado/ubuntu-postgres
```
> Após a conclusão dessa operação, a listagem de imagens Docker no seu computador agora deve mostrar a nova imagem, bem como a antiga de onde ela se derivou:
```
docker images
```

> A saída deve ser semelhante a essa:
```
projetointegrador/ubuntu-postgres       latest              62359544c9ba        50 seconds ago      206.6 MB
ubuntu                      			      latest              c5f1cf30c96b        7 days ago          120.8 MB
hello-world			                        latest              94df4f0ce8a4        2 weeks ago         967 B
```
> Fim...
