Permitir el usuario root hacer ssh
=====================================

Primero cambiar la clave del usuario "root"::

	passwd root

Crear un archivo "01-permitrootlogin.conf" en la ruta "/etc/ssh/sshd_config.d"::

	vi /etc/ssh/sshd_config.d/01-permitrootlogin.conf

	Match address *
	  PasswordAuthentication yes
	  AuthenticationMethods password publickey,keyboard-interactive
	  PermitRootLogin yes

Reiniciar el servicio de sshd::

	|systemctl restart sshd

