# SOCIAL OPLESK
### 🏴‍☠️ HACKS 
<br/>
## ⚡️ Hack de Docker ⚡️## 
# debes tenerr docker engine en wsl en caso de emplear windows, ideal sobre debian / ubuntu / fedora.

**Requisitos previos**
<br/>
- docker engine enlace:  https://docs.docker.com/engine/install/

```diff
- NOTA HACER LAS PRÁCTICAS MEDIANTE LA CONSOLA - TERMINAL  
```
|Hacks | Details | 
|----------|---------|
| H-1      | Enviar |
| H-2      | Enviar|
| H-3      | Hacer | 
| H-4      | Extraer |
| H-5      | Enviar  |
| H-6      | Clonando|
| H-7      | Eliminar | 
| H-8      | Volver |
<br/> 

## 🏆 H-1 (aplicar la image optima)

1. Crear un directorio llamado docker_1

2. Dentro del directorio docker_1 crear el archivo Dockerfile

3. Debes crear el Dockerfile sin elegir un FROM de image incorrecto
   
4. las versiones slim siempre se deben seleccionar sobre una version latest
   
```
#❌
FROM ubuntu:latest

# ✅ Bien
FROM ubuntu:24.04-slim
--------------------------

FROM ?

RUN apt update -y && apt install nginx -y

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```


## 🏆 H-2 (usar apt con el estilo correcto en docker)
1. Crear un directorio llamado docker_2

2. Dentro del directorio docker_2 crear el archivo Dockerfile

3. Debes crear el Dockerfile con una image de ubuntu correcta e instalar nginx. 
   
4. apt está diseñado para humanos: Es un comando interactivo para la terminal del día a día.<br/> 
   apt-get está diseñado para scripts y automatización: Es una herramienta de "bajo nivel" mucho más estable y predecible.
   
5. apt-get clean: borra los archivos .deb descargados que ya se instalaron.

6. --no-install-recommends y --no-install-suggests:<br/>
   Estas banderas le dicen: "Instala solo lo estrictamente necesario para que curl funcione".<br/>
   Esto te puede ahorrar cientos de megabytes de basura.

7. rm -rf /var/lib/apt/lists/* <br/>
   Elimina los índices de los repositorios (las listas de qué paquetes existen).    
   
```
# ❌ Mal (crea 3 capas)
RUN apt update
RUN apt install -y curl
RUN rm -rf /var/lib/apt/lists/*

# ✅ Bien (1 sola capa)
RUN apt-get update && \
    apt-get install -y --no-install-recommends --no-install-suggests curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
--------------------------

FROM ?

RUN ?

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```


## 🏆 H-3 (Usuario no root)

1. Crear un directorio llamado docker_3

2. Dentro del directorio docker_3 crear el archivo Dockerfile

3. pip install --no-cache-dir -r: la bandera --no-cache-dir le dice a pip: <br/>
 "Descarga el paquete, instálalo y borra inmediatamente el archivo descargado; no me dejes basura en la carpeta de caché".

4. verifica que funciona:
```
  consulta la API al tner el contenedor listo
  http://localhost:8080/app adicional

 Verificar el usuario no root, luego de tener el contenedor listo
 mediante la temrinal: docker exec mi-container whoami
# Respuesta: appuser (no root)
```

5. Debes crear el archivo requirements.txt e internamente debe tener: flask==2.3.3
```
flask==2.3.3
```

6. Script de la aplicación flask que vas a emplear para este hack.
```
import os
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return f"¡Hola desde flask corriendo en Docker!"

@app.route('/app')
def health():
    return {"status": "ok", "app": "flask app"}

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=???)
```

7. Siempre evitar tener el server modo debug en el Dockerfile
```
# ❌ debug=True activado en un script de Flask dentro de Docker <br/>
     (especialmente si va a producción) es uno de los errores más peligrosos y comunes.
     ⚡ app.run(host='0.0.0.0', port=PORT, debug=True)


# ✅ debug=False es importante (foreground, no recarga automática)
# ✅ debug=False es obligatorio en Docker.
     ⚡ app.run(host='0.0.0.0', port=PORT, debug=False)
```

8. Ejemplo de como utilizar en Dockerfile el modo no root
```
# ✅ Crea y usa usuario no root
RUN useradd -m -u 1000 appuser
USER appuser

# Cambiar al usuario no root
USER appuser

WORKDIR /home/appuser/app

---

# ✅ Crea y usa usuario no root
RUN useradd -m -u 1000 appuser && \
    chown -R appuser:appuser /app

# Cambiar al usuario no root
USER appuser
```

9. Script de Dockerfile a tratar
```

FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

RUN useradd ?

USER ?

EXPOSE 8080

CMD ["python", "app.py"]
```




<br/>
<div style="text-align: center">Social Oplesk</div>
