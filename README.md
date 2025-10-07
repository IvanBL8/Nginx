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

