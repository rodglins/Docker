# Docker
Docker e Docker Swarm 



Container:

Entendendo os conceitos:

1) App>bin/lib>host os>hardware } bare metal server

cada aplicação tem um custo elevado, servidores com o passar do tempo irão depreciar. 

2)
App1>bin/lib>host os>hardware |
App2>bin/lib>host os>hardware |  bare metal server
App3>bin/lib>host os>hardware |


compartilham o mesmo binario no servidor,
não é um servidor para uma aplicação só,
app1 pode atualizar uma dependencia e pode causar problema nas outras aplicações. neste caso tem que recuperar um backup. como é uma maquina fisica, o tempo de downtime será grande. desligar, restaurar o backup, clonar e imagem e subir as aplicações.  O deploy em horizontal é bem dificil

3)
Virtual server> App1>bin/lib>OS1>Hypervisor>Host Operating System>Hardware

App2>bin/lib>OS2>Hypervisor>Host Operating System>Hardware

App3>bin/lib>OS3>Hypervisor>Host Operating System>Hardware


Em caso de update da app1 somente ela será afetada o so é individual. hypervisor possuem a função de snapshot, para o estado na maquina e gravado em arquivo temporario, caso de algum problema restaura pela imagem do snapshot na versão anterior. Tempo de downtime menor. Podemos armazenar mais maquinas virtuais. economia com energia. otimizamos como cada vm pode utilizar da maquina.

4)

Container>App>bin/lib>Container Engine>Host Operating System>Hardware
App>bin/lib>Container Engine>Host Operating System>Hardware
App>bin/lib>Container Engine>Host Operating System>Hardware

servidor que contem container instalado (docker)
virtualiza a aplicação
tudo que é feito na aplicação, é imutavel. somente se quiser adicionar novas imagens, em caso de update, cria uma nova imagem, sobe no lugar dela, se funcionar pode excluir a antiga e deixa-la, se nao restaurar a antiga. mais facil manutenção. otimização de memoria e cpu.

------------

meuApp.com >> VM> Escalonamento vertical>dificil atualização

meuApp.com>>Proxy/Serviço>>(Três ou varios) containers>> Escalonamento horizontal>fácil atualização (cria um container novo com a atualização, caso funcione desliga o container antigo)

E-commerce> black-friday>>escalonamento vertical>> tmb aplicado em container>> aumentar os recursos da maquina>>

App:
Kubernetes
Docker SWARM
Docker Composer

---------------

Arquitetura do Docker

Terminologias

Container image: pacote com todas as dependencias do nosso container

dockerfile: arqiuvo de texto com as instruições para fazer o build na nossa imagem

build: le o dockerfile e gera uma imagem

container: com a imagem criada podemos instaciar o container, uma aplicação, processo ou serviço.

volumes: permite que o container armazene arquivos, daddos em disco.

tag: ajuda no versionamento das imagens

multi stage build: multi estagios do build, compilar nossa aplicação, codigo em go, compila a aplicação, chama outra image que faz o run na aplicação.

Repository: deposito, caixa cheia de imagens

Registry: servico que prove acesso do docker ao repositorio.

Docker Hub: repositorio publico, imagens publicas e privadas, 

compose: metadata de multiplos containers com um comando.


---------------

instalação docker linux:

onde utilizar:
aws(ec2)
gcp(google)
microsoft azure

virtual box
vmware

-----------

Docker online:

https://labs.play-with-docker.com/


--------------


Docker Azure:


criar conta azure> free services>linux virtual machine>> resource group (minhaPrimeiraVM)>Docker01>ubuntu 16> Spot instalce no>standar>ssh>azureuser>generate new key par>>minhavm>>alow selected ports>>http 80>>https 443>>ssh 22 >> clicar em review e criate>>create>> download private key and create resource>>> utilizar comandos ssh para linux>>no windows usar putty.>>resouce group>>minha primeiraVM>>Docker01>>copiar ip publico

converter a chave:
putty key generator>>load>>minhachavevm>>ok>>save primary key>>sim.>>minhavm.pk

putty>>basic options>>ip publico>>copiar>ssh>auth>browse (arquivo gerado no putty key generator >>minhavcm.ppk>abrir >>open>sim> login: azureuser

Digite a senha
Digite o ip do servidor azure

------

Docker desktop Windows>gerenciador do hyper-v> docker>conectar>powershell>

docker desktop é apenas para desenvolvimento 
na pratica, usar vm virtual ou linux.

---


--------------


-------





bridge: é a rede defauld do docker , utilizado para comunicação entre containers

host : remove o isolamento de rede, o container repostendiretamente pela placa de rede do host

overlay, permite a comunicação entre containers de hosts diferentes (docker swart em cluster)vxlan

macvlan> permite atribuir um enderelo mac ao container tornando ele visivel como um dispositivo fisico na rede

none: sem rede

docker.com (exemplos)

------------

tipos de armazenamento

Volume : nao precisa de uma estrutura de arquivos> porem fica mais carregado o disco

bind mount: mapeia um arquivo ou uma pasta do host para um arquivo ou paste no container. esse arquivo ja deve existir para ser mapeado.

tmpfs: tudo que fizer no container fica gravado, mas se deleta o container já nao esta mais, pode utilizar muito recurso em disco pois utiliza a camada de escrita. pode utilizar disco temporario em cach, onde tudo que fizer , los, algun cash necessario, definir o tamanho dele, qdo reiniciar ou parar esses arquivos sao deletados. 


----------------

Limitação:

podemos ajudar recursos para uso, memoria é essencial, cpu etc.
seu container pode ser finalizado, se estiver com pouca memoria para seuss processos, o linux vai matar processos nao essenciais. container não é reconhecimento como essencial para o sistema. tem que limitar a memoria para nao usar mais que o necessario e prejudicar a maquina
