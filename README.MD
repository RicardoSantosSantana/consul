## COMANDOS consul server

__Fazer servidores consul se conhecerem__

> consul join _numero_do_ip_eth0_do_container

Subir serviços com o docker-compose up

__Descobrir as instancias que estão no ar__
> docker-compose ps

__Entrar na instancia__
> docker exec -it consulserver01 sh

__Recolher ip do container__
pegar o eth0
> * ifconfig

__Criar diretórios__

>   * mkdir /etc/consul.d mkdir /etc/consul.d && mkdir /var/lib/consul

__Executar comando para criar servidor no consul__

> _o ip deve ser o ip do container utilizado no -node para dar o bind_
> _o -bootstrap-expect é a quantidade de server que existirá_

> * consul agent -server -bootstrap-expect=3 -node=consulserver03 -bind=172.18.0.5 -data-dir=/var/lib/consul -config-dir=/etc/consul.d

__Listar servidores consul__
> consul members


## COMANDOS consul client

mesmo comandos do server, exceto por 
> consul agent -bind=172.24.0.6 -data-dir=/var/lib/consul -config-dir=/etc/consul.d


## É IMPORTANT SUBIR UM AGENT NA MESMA MAQUINA QUE ESTÁ O SERVIÇO RODANDO
 atualizar o consul
> consul reload

__Buscar serviços__
instalar o dig
> apk -U add bind-tools
rodar o dig
> dig @localhost -p 8600 nginx.service.consul

__Buscar no catologo de serviço via pi__

> curl localhost:8500/v1/catalog/services
> consul catalog nodes -service nginx
> consul catalog nodes -detailed

__fazendo o join com o consul agent__

-retry-join = usado para fazer join com um servidor e assim ser adicionado ao cluster pode ser usado mais de um -retry-join afim de se um servidor não responder o outro responda

consul agent -bind=172.24.0.3 -data-dir=/var/lib/consul -config-dir=/etc/consul.d -retry-join=172.24.0.3
__CONFIGURAÇAO EM ARQUIVO__

No caso as configurações estão de cada server está em sua respectiva pasta
exemplo servers/server01/server.json
entre no container com docker exec -it "nome do container"  sh

> consul agent -config-dir=/etc/consul.d

## Gerar chave de criptografia
> consul keygen

__IMPORTANTE APAGAR TODOS OS DADOS DO DIRETÓRIO TMP DOS EXEMPLO__

__*afim de não gerar problemas, para novos testes*__

entre no consul e execute
> rm -rf /tmp/*
