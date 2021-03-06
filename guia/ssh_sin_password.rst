Configurar SSH sin password
==============================

Queremos hacer el siguiente ejercicio, desde un servidor desea conectarse a otro servidor, sin tener que escribir password cada vez que se intente conectar a esta otra PC.

Hablaremos de servidor-01 como el servidor que solicitara la conexión y servidor-02 sera el servidor que tenga el servicio de ssh y que permitira que se conecten a él.

El servidor-02 debe tener instalado openssh-server y asegurarnos que podemos iniciar en el con root desde otro equipo.::

	# yum install openssh-server


Agregar o descomentar la siguientes línea en el archivo  /etc/ssh/sshd_config::

	PubkeyAuthentication yes
	AuthorizedKeysFile     %home/.ssh/authorized_keys .ssh/authorized_keys .ssh/authorized_keys2
	UseDNS no

servidor-01 vamos a generar una llave pública, puede ser rsa o dsa, ssh ver 1 soporta rsa y ssh ver 2 soporta ambas. ::

	# ssh-keygen -b 4096 -t rsa

	Generating public/private rsa key pair.
	Enter file in which to save the key (/root/.ssh/id_rsa): 
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /root/.ssh/id_rsa.
	Your public key has been saved in /root/.ssh/id_rsa.pub.
	The key fingerprint is:
	c8:7e:e9:d2:d4:0b:20:8c:10:48:42:d5:5b:6f:45:49 root@debian
	The key's randomart image is:
	+---[RSA 4096]----+
	|*+...     oE.    |
	|+    . .   o     |
	| . o  o . .      |
	|  . oo.. o       |
	|     .o.S.       |
	|     .  o..      |
	|      .oo. .     |
	|      .o. .      |
	|       ..        |
	+-----------------+
o.::

	$ ssh-keygen -b 1024 -t dsa
	Generating public/private dsa key pair.
	Enter file in which to save the key (/home/cgome1/.ssh/id_dsa): 
	/home/cgome1/.ssh/id_dsa already exists.
	Overwrite (y/n)? y
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /home/cgome1/.ssh/id_dsa.
	Your public key has been saved in /home/cgome1/.ssh/id_dsa.pub.
	The key fingerprint is:
	4e:c8:b7:5f:72:ea:ec:79:ca:55:59:02:e4:6e:c7:19 cgome1@debian
	The key's randomart image is:
	+---[DSA 1024]----+
	|           .o    |
	|           . .   |
	|            . E .|
	|     . .   . . * |
	|      o S   o *  |
	|       + . . o   |
	|        o . +    |
	|         + B.    |
	|         oXo     |
	+-----------------+


Listo, ya tenemos la llave pública… ahora falta dársela al servidor-02, para que pueda identificar desde que servidor y usuario le va permitir ingresar por ssh sin interaccion de solicitud de clave.::

	# ssh-copy-id root@192.168.1.4

	The authenticity of host '192.168.1.4 (192.168.1.4)' can't be established.
	RSA key fingerprint is 2e:87:fa:44:cf:8b:b1:15:20:fc:8c:af:52:0c:6b:07.
	Are you sure you want to continue connecting (yes/no)? yes
	/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
	/usr/bin/ssh-copy-id: INFO: 2 key(s) remain to be installed -- if you are prompted now it is to install the new keys
	root@192.168.1.4's password: 

	Number of key(s) added: 2

En servidor-02 en el home de root podemos ver que se creo en /root/.ssh/authorized_keys.::

Ahora solo resta establecer la conexión desde el servidor-01 al servidor-02 con ssh.::

	servidor-01# ssh root@servidor-02
	servidor-02@root# 

Tambien si queremos se puede utilizar esta tecnica para copiar la llave publica.::

	# cat .ssh/id_rsa.pub | ssh usuario@servidordestino 'cat >> .ssh/authorized_keys'

authorized_keys debe tener permisos 600 y con su respectivo dueño

Si luego de hacer todos los pasos anteriores no te conectas, coloca el modo debug del ssh::

	servidor-01# ssh root@servidor-02 -vv

Y si llegaras a tener un error como este::

	debug2: we did not send a packet, disable method

Edita el  /etc/ssh/sshd_config y agrega esta linea::

	PubkeyAcceptedKeyTypes +ssh-dss #this is the correct entry







