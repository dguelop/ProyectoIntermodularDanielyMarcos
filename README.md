El objetivo principal de este proyecto es implantar un servidor de correo electrónico interno utilizando Dovecot como servicio de gestión de buzones IMAP/POP3 y Postfix para enviar y recibir correos mediante SMTP, de forma que la empresa pueda disponer de su propio sistema de correo electrónico corporativo, independiente de proveedores externos.

# 📬 Servidor de Correo en Ubuntu

Este proyecto implementa un servidor de correo completo en Ubuntu Server utilizando:

* Postfix (SMTP – envío)
* Dovecot (recepción)
* Roundcube (webmail)
* Thunderbird y Microsoft Outlook (clientes externos)
* Webmin (administración web)

---

# 🧩 Arquitectura

```
Internet
   ↓
Postfix (SMTP)
   ↓
Dovecot (IMAP)
   ↓
Usuarios (Maildir)
   ↓
Clientes:
  - Thunderbird / Outlook
  - Roundcube (web)
```

---

# 🌐 Dominio

```
proyectosmr.abrdns.com
```

Servidor de correo:

```
mail.proyectosmr.abrdns.com
```

---

# ⚙️ Configuración DNS

## Registros necesarios

### A

```
mail.proyectosmr.abrdns.com → IP_SERVIDOR
```

### MX

```
proyectosmr.abrdns.com → mail.proyectosmr.abrdns.com
```

### SPF (recomendado)

```
v=spf1 mx ~all
```

---

# 📤 Postfix (SMTP)

Archivo principal:

```
/etc/postfix/main.cf
```

Configuraciones clave:

```
myhostname = mail.proyectosmr.abrdns.com
mydomain = proyectosmr.abrdns.com
mydestination = $myhostname, proyectosmr.abrdns.com, localhost
inet_interfaces = all
inet_protocols = ipv4
home_mailbox = Maildir/

smtpd_sasl_auth_enable = yes
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
```

---

# 📥 Dovecot (IMAP)

Archivo:

```
/etc/dovecot/conf.d/10-auth.conf
```

Configuración importante:

```
auth_mechanisms = plain login
auth_username_format = %n
```

Maildir:

```
~/Maildir
```

---

# 🌍 Acceso desde clientes

## 🔹 IMAP (recepción)

* Servidor: `mail.proyectosmr.abrdns.com`
* Puerto: `993`
* Seguridad: SSL/TLS

## 🔹 SMTP (envío)

* Servidor: `mail.proyectosmr.abrdns.com`
* Puerto: `587`
* Seguridad: STARTTLS
* Autenticación: requerida

---

# 💻 Clientes compatibles

Funciona con:

* Thunderbird
* Microsoft Outlook
* móviles (iOS / Android)

Usuario:

```
usuario1@proyectosmr.abrdns.com
usuario2@proyectosmr.abrdns.com
...
```

---

# 🌐 Webmail (Roundcube)

Acceso:

```
http://mail.proyectosmr.abrdns.com/roundcube
```

Permite:

* leer correos
* enviar correos
* gestionar bandejas

---

# 🖥️ Administración (Webmin)

Acceso:

```
https://IP_SERVIDOR:10000
```

Funciones:

* gestión de usuarios
* configuración de servicios
* monitorización

---

# 🔐 Puertos necesarios

| Puerto | Servicio                 |
| ------ | ------------------------ |
| 25     | SMTP (entrada)           |
| 587    | SMTP (envío autenticado) |
| 993    | IMAP seguro              |
| 10000  | Webmin                   |

---

# 🔥 Firewall (UFW)

```
sudo ufw allow 25
sudo ufw allow 587
sudo ufw allow 993
sudo ufw allow 10000
```

---

# 🧪 Comprobación

Ver logs:

```
sudo tail -f /var/log/mail.log
```

Test de autenticación:

```
doveadm auth test usuario contraseña
```

---

# ⚠️ Problemas comunes

## ❌ No conecta desde fuera

* revisar port forwarding
* comprobar firewall
* verificar IP pública

## ❌ Gmail rechaza correos

* falta PTR
* falta SPF/DKIM
* IP residencial

## ❌ Usuario no autentica

* usar `auth_username_format = %n`
* comprobar usuario del sistema


---

# 📌 Estado del proyecto

✔ Envío de correos funcionando
✔ Recepción local funcionando
✔ Acceso desde clientes externos
✔ Webmail operativo
✔ Administración con Webmin

---

Servidor montado y configurado manualmente en Ubuntu Server.

---
