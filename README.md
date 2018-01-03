# Configurating-Proftpd-server-on-Google-Cloud-Debian

ENGLISH VERSION
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

If you do not have gcloud sdk, install it as in this link.: "https://cloud.google.com/sdk/?hl=en-us"
See how init gcloud sdk as in this link.: "https://cloud.google.com/sdk/gcloud/reference/init"
ITEM A: Here's how to list your gcloud instances.: "https://cloud.google.com/sdk/gcloud/reference/compute/instances/list"

Open your gcloud ssh utilizing this command on gcloud sdk.
gcloud compute ssh <instance name>

Replace <instance name> by the name of your instance on gcloud. See how list instance name on ITEM A

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Configurating Proftpd server on Google Cloud Debian

1. On this time i will teach how to configurated Proftpd Server on gcloud debian using login/password.
You can also do this using ssh key instead of password, with some modifications. I'll show you that in the future.

By pattern gcloud debian does not accept login/password access. So let's change it.
At the command terminal, look for your ssh's application folder.. Normally it's on the path /etc/ssh .
Change the configuartion, like bellow:

sudo nano /etc/ssh/sshd_config

Search the attribute 
# PasswordAuthentication no

Remove # and change the "no" to "yes", save the change and restart the ssh.

The command for this is: sudo systemctl restart sshd

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

2. Let's install proftpd server, with command: sudo apt-get update || sudo apt-get install proftpd

Let's configurating the proftpd.: "To see more about proftpd project or confugrating access: http://www.proftpd.org/"

I will talk about some confurations in the file proftpd.
DefaultRoot ~ : is to jail the user on his home folder
RequiredValidShell off: the user does not need to have a valid shell. This allows you to create a user who can not access the shell, will only have access on ftp
Port 21: This set the connection port. You will have to see how open a port on gcloud sdk on item 3.

Put the proftpd.conf file of my github on your proftpd path. "/etc/proftpd", change the configuartions that you want and restart proftpd.

This configuration is set to active mode. If you want passive mode change this line.
# PassivePorts 49152 65534
Remove the # and set the range of ports that you want. "Will have to open this ports on gcloud sdk, see item 3"

Restart proftpd: sudo systemctl restart proftpd

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

3. Opening ports ou gcloud sdk

Let's open the port configurated on proftpd.conf.

On gcloud sdk the open port command is this.:
gcloud copute firewall-rules create <rule name> --allow tcp:21 --source-tags=<your gcloud instance name> --source-ranges=0.0.0.0/0 --description="<some description>"

Let's explain:
You are opening a port on gcloud firewall-rules of your instance, setting port to 21 for any tcp/ip.
<rule name> replace this for the name you wanted. "can not have repeated name"
<your gcloud instance name> replace with any instance name you want. See how list instance on ITEM A
"<some description>" replace this for any description you wanted.

If you want to proftpd work as passive mode you have to open the passive ports, like bellow:
gcloud copute firewall-rules create <rule name> --allow tcp:49152-65534 --source-tags=<your gcloud instance name> --source-ranges=0.0.0.0/0 --description="<some description>"

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

4. Creating users on debian

Let's create users.

The command is this: sudo useradd <username> --home <home you want user login on ftp> --shell /bin/false --no-create-home --password <password that you want>

If you want to know more about useradd, input this command: sudo useradd --help

Let's explain the command.:
<username> replace this for any name you wanted. Remember this is the "login/username that will access ftp"
<home you want user login on ftp> replace this for the path you wanted that username login.
--shell /bin/false this command is to prohibit the user from accessing the shell, because it is not a valid shell.
--no-create-home is to warn useradd to not create a home directory, if you want to create it, remove this command.
<password that you want> replace it for any password you wanted.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



Versão em português:
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Se você não tem o gcloud sdk, instale-o como neste link.: "https://cloud.google.com/sdk/?hl=pt-br"
Veja como iniciar o gcloud sdk como neste link.: "https://cloud.google.com/sdk/gcloud/reference/init"
ITEM A: Veja como listar suas instâncias gcloud.: "https://cloud.google.com/sdk/gcloud/reference/compute/instances/list"

Abra seu ssh gcloud utilizando o comando no gcloud sdk.
gcloud compute ssh <instance name>

Substitua <instance name> pelo nome de sua instância no gcloud. Veja como listar o nome das instâncias no ITEM A

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Configurando Servidor Proftpd no Google Cloud Debian

1. Neste momento eu vou ensinar como configurar o servidor Proftpd no gcloud debian utilizando login/senha.
Você também pode fazer utilizando chave ssh ao invés de senha, com algumas modificações. Mostrarei isso no futuro.

Por padrão gcloud debian não aceita acesso por login/senha. Então vamos mudar isto.
No terminal de comando, procure por sua pasta do aplicativo ssh. Normalmente está no caminho /etc/ssh .
Altere a configuração como mostrado abaixo:

sudo nano /etc/ssh/sshd_config

Localize o atributo
# PasswordAuthentication no

Remova # e alter o "no" para "yes", salve a alteração e reinicie o ssh.

O comando para isso é: sudo systemctl restart sshd

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

2. Vamos instalar o servidor proftpd, com o comando: sudo apt-get update || sudo apt-get install proftpd

Vamos configurar o proftpd.: "Para ver mais sobre o projeto proftpd ou configurações acesse: http://www.proftpd.org/"

Eu falarei sobre algumas configurações no arquivo proftpd.
DefaultRoot ~ : é para aprisionar o usuário em sua pasta home
RequiredValidShell off: O usuário não precisa ter um shell válido. Isso permite que você crie um usuário que não pode acessar o shell, só terá acesso no ftp
Port 21: Isto seta a porta de conexão. Você terá que ver como abrir porta no sdk gcloud no item 3.

Coloque o arquivo proftpd.conf do meu github no seu diretório proftpd. "/etc/proftpd", altere as configurações que você queira e reinicie o proftpd.

Esta configuração está setada para modo ativo. Se você quiser modo passivo mude esta linha.
# PassivePorts 49152 65534
Remova # e defina o alcance de portas que você quiser. "Terá que abrir as portas no sdk gcloud, veja item 3"

Reinicie proftpd: sudo systemctl restart proftpd

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

3. Abrindo portas no sdk do google

Vamos abrir as portas configuradas no proftpd.conf.

No sdk do gcloud o comando de abrir port é este.:
gcloud copute firewall-rules create <nome da regra> --allow tcp:21 --source-tags=<suas instâncias gcloud> --source-ranges=0.0.0.0/0 --description="<alguma descrição>"

Vamos explicar:
Você está abrindo a porta no gcloud firewall-rules de sua instância, setando a porta 21 para qualquer tcp/ip.
<nome da regra> substitua isto pelo nome que você quiser. "não pode ter um nome repetido"
<suas instâncias gcloud> substitua isto por qualquer nome de instância que você quiser. Veja como listar as instâncias no ITEM A
"<alguma descrição>" substitua isto por qualquer descrição que você quiser.

Se você quiser que o proftpd trabalhe como modo passive você terá que abrir as portas passivas, como abaixo:
gcloud copute firewall-rules create <nome da regra> --allow tcp:49152-65534 --source-tags=<suas instâncias gcloud> --source-ranges=0.0.0.0/0 --description="<alguma descrição>"

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

4. Criando usuários no debian

Vamos criar usuários.

O comando é este: sudo useradd <nome do usuário> --home <home que você quer que o usuário logue no ftp> --shell /bin/false --no-create-home --password <senha que você quiser>

Se quiser saber mais sobre useradd, insira este comando: sudo useradd --help

Vamos explicar o comando.:
<nome do usuário> substitua isto por qualquer nome que você quiser. Lembre-se isso é o "login/nome de usuário que acessará o ftp"
<home que você quer que o usuário logue no ftp> substitua isto pelo caminho que você quer que o usuário logue.
--shell /bin/false este comando é para proibir o usuário de acessar o shell, porque não é um shell válido.
--no-create-home é para avisar o useradd para não criar o diretório home, se quiser cria-lo, remova este comando.
<senha que você quiser> substitua isto por qualquer senha que você quiser.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
