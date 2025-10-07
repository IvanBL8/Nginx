# Nginx



## 1. Introducción

**Nginx** es un servidor web y proxy inverso de alto rendimiento, ligero y eficiente.  
Se utiliza ampliamente para servir aplicaciones web, balancear carga, actuar como proxy inverso y manejar conexiones HTTPS.  
Su arquitectura basada en eventos le permite manejar miles de conexiones simultáneas con un consumo de recursos muy bajo.

---

## 2. Comparativa con Apache

| Característica              | Nginx                                     | Apache                                   |
|-----------------------------|-------------------------------------------|------------------------------------------|
| Arquitectura                | Asíncrona, basada en eventos              | Basada en procesos e hilos               |
| Rendimiento                 | Excelente con alto tráfico                | Bueno, pero menos eficiente              |
| Consumo de memoria          | Bajo                                      | Mayor                                   |
| Módulos                     | Compilados en tiempo de compilación       | Dinámicos (fácil de añadir/quitar)       |
| Configuración de VirtualHost| Más simple y eficiente                     | Flexible pero más pesada                 |
| Uso típico                  | Proxy inverso, balanceo, servidor estático | Aplicaciones dinámicas con PHP, etc.     |

En resumen, **Nginx** es ideal para rendimiento y escalabilidad, mientras que **Apache** destaca por su flexibilidad.

---

## 3. Instalación

### En Ubuntu/Debian:

```bash
sudo apt update
sudo apt install nginx -y
```
### Comandos básicos:

```bash

sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

## 4. Casos Prácticos

### a) Versión de Nginx instalado
```bash
nginx -v
```


**Salida esperada:**
```bash

nginx version: nginx/1.24.0
```
### b) Ficheros de configuración

**La configuración principal se encuentra en:**
```bash

/etc/nginx/nginx.conf
```

**Los sitios web están definidos en:**
```bash

/etc/nginx/sites-available/
```
**Ejemplo de configuración básica (/etc/nginx/sites-available/default):**
```bash

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### c) Página web por defecto

**Ruta por defecto:**
```bash

/var/www/html/index.html
```


**Personalízala con tu nombre o un mensaje:**
```bash

sudo nano /var/www/html/index.html
```

**Ejemplo de contenido:**
```bash
<!DOCTYPE html>
<html>
<head>
    <title>Servidor Nginx Personalizado</title>
</head>
<body>
    <h1>¡Bienvenido al servidor Nginx de Iván Barranco!</h1>
    <p>Esta es una página web personalizada.</p>
</body>
</html>
```

**Guarda y recarga el servicio:**
```bash

sudo systemctl reload nginx
```

### d) Virtual Hosting con balanceo de carga (HTTPS)
**Crear certificados autofirmados (para pruebas)**
```bash

sudo mkdir -p /etc/nginx/certs
cd /etc/nginx/certs

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout sitio1.key -out sitio1.crt -subj "/CN=sitio1.local"

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout sitio2.key -out sitio2.crt -subj "/CN=sitio2.local"
```

**Configurar los servidores backend (simulados)**

**Supón que tienes dos sitios:**
```bash
sitio1 en https://127.0.0.1:8443

sitio2 en https://127.0.0.1:9443
```
**Configurar el balanceador (/etc/nginx/conf.d/balanceo.conf)**
```bash

upstream web_backend {
    server 127.0.0.1:8443;
    server 127.0.0.1:9443;
}

server {
    listen 443 ssl;
    server_name balanceador.local;

    ssl_certificate     /etc/nginx/certs/sitio1.crt;
    ssl_certificate_key /etc/nginx/certs/sitio1.key;

    location / {
        proxy_pass https://web_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

**Probar configuración y recargar**
```bash

sudo nginx -t
sudo systemctl reload nginx
```

Ahora accede a:
**https://balanceador.local**
y verás cómo balancea las peticiones entre los dos sitios backend


## 5. Referencias

[Documentación oficial de Nginx](https://nginx.org/en/docs/)

[Guía DigitalOcean: Cómo configurar Nginx](https://www.digitalocean.com/community/tutorials)

[Nginx vs Apache Benchmark](https://kinsta.com/blog/nginx-vs-apache/)

[Configuración de balanceo de carga en Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)
