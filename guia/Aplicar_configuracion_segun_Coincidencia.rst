Aplicar configuración según Coincidencia
============================================

También puede utilizar la configuración "Coincidencia" en el archivo de configuración para cambiar el tipo de inicio 
de sesión permitido según la dirección IP remota, como por ejemplo::

	Match address 192.168.1.0/24
	  PasswordAuthentication yes
	  AuthenticationMethods password publickey,keyboard-interactive
	  PermitRootLogin yes

	Match address *
	  PasswordAuthentication no
	  PermitRootLogin no
	  AuthenticationMethods publickey,keyboard-interactive

También puede agregar fácilmente TOTP MFA (también conocido como Google Authenticator y amigos) para iniciar sesión. 
Los detalles difieren un poco según la distribución, pero algo como:

https://reintech.io/blog/implementing-two-factor-authentication-for-ssh-rocky-linux-9

