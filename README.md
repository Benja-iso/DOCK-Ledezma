# DOCK-Ledezma
Proyectos Flask con DockerEste repositorio contiene varios ejemplos de aplicaciones Flask, cada una empaquetada en un contenedor Docker. Los ejemplos cubren desde una aplicación Flask básica que muestra la IP del cliente hasta una aplicación más elaborada con HTML y CSS, y un script de despliegue automatizado.Estructura del ProyectoLa estructura de directorios simulada se basa en las rutas proporcionadas:docker_apps/
├── flask_basic/
│   ├── app.py
│   └── Dockerfile
├── flask_html/
│   ├── app.py
│   ├── Dockerfile
│   ├── static/
│   │   └── style.css
│   └── templates/
│       └── index.html
└── flask_script/
    ├── app.py          # app.py (from 2.8) -> This seems to be sample.py in the script
    ├── deploy.sh
    └── sample.py       # This is the actual app used by deploy.sh, based on 2.8 and 2.9
    └── tempdir/        # Directorio temporal creado por deploy.sh
        ├── Dockerfile  # Dockerfile generado por deploy.sh
        ├── app.py      # Copia de sample.py (desde 2.8)
        ├── static/
        │   └── style.css
        └── templates/
            └── index.html
1. Flask Básico: Mostrar IPEsta es una aplicación Flask muy simple que devuelve la dirección IP del cliente que realiza la solicitud.Archivos:flask_basic/app.py:from flask import Flask, request
app = Flask(__name__)

@app.route('/')
def main():
    return "Tu dirección IP es: " + request.remote_addr + "\n"

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=8000)
flask_basic/Dockerfile:FROM python:3.9-slim
RUN pip install flask
COPY app.py /home/myapp/
EXPOSE 8000
CMD ["python", "/home/myapp/app.py"]
Cómo ejecutar:Navega al directorio flask_basic:cd /home/tu_usuario/docker_apps/flask_basic
Construye la imagen Docker:docker build -t flask-basic .
Ejecuta el contenedor Docker:docker run -d -p 8000:8000 --name basic-ip-container flask-basic
Abre tu navegador y visita http://localhost:8000 para ver tu dirección IP.2. Flask con HTML y CSS: Verificador de IPEsta aplicación Flask renderiza una página HTML para mostrar la dirección IP del cliente, utilizando CSS básico para el estilo.Archivos:flask_html/app.py:from flask import Flask, request, render_template
app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html', ip=request.remote_addr)

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=8181)
flask_html/templates/index.html:<!DOCTYPE html>
<html>
<head>
    <title>Verificador de IP</title>
    <link rel="stylesheet" href="/static/style.css">
</head>
<body>
    <h1>Tu dirección IP es: {{ ip }}</h1>
</body>
</html>
flask_html/static/style.css:body { font-family: Arial; text-align: center; margin-top: 50px; }
flask_html/Dockerfile:FROM python:3.9-slim
RUN pip install flask
COPY . /home/myapp/
WORKDIR /home/myapp
EXPOSE 8181
CMD ["python", "app.py"]
Cómo ejecutar:Navega al directorio flask_html:cd /home/tu_usuario/docker_apps/flask_html
Construye la imagen Docker:docker build -t flask-html .
Ejecuta el contenedor Docker:docker run -d -p 8181:8181 --name html-ip-container flask-html
Abre tu navegador y visita http://localhost:8181 para ver la página con tu dirección IP.3. Flask con Script de Despliegue AutomatizadoEste ejemplo demuestra un script de shell (deploy.sh) que automatiza la creación de un entorno Docker para una aplicación Flask, copiando los archivos necesarios y generando un Dockerfile sobre la marcha.Archivos:flask_script/sample.py (Esta es la aplicación Flask utilizada por el script de despliegue):from flask import Flask, request
app = Flask(__name__)

@app.route('/')
def main():
    return "Tu dirección IP es: " + request.remote_addr + "\n"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8888, debug=True, use_reloader=False)
flask_script/deploy.sh:#!/bin/bash

# Crea directorios temporales para la construcción
mkdir -p tempdir/templates tempdir/static

# Copia el archivo de la aplicación (sample.py)
cp sample.py tempdir/app.py # El script copia sample.py como app.py en tempdir

# Asegúrate de que los directorios templates y static existen si la app los usa
# Aunque sample.py no los usa, el Dockerfile generado los espera
touch tempdir/templates/placeholder.txt # Asegurarse que el directorio existe
touch tempdir/static/placeholder.txt   # Asegurarse que el directorio existe

# Genera el Dockerfile dentro del directorio temporal
cat <<EOF > tempdir/Dockerfile
FROM python
RUN pip install flask
# Copia los directorios static y templates (incluso si están vacíos para este app)
COPY ./static /home/myapp/static/
COPY ./templates /home/myapp/templates/
COPY app.py /home/myapp/
EXPOSE 8888
CMD python3 /home/myapp/app.py
EOF

# Navega al directorio temporal
cd tempdir

# Construye la imagen Docker
docker build -t flask-script .

# Ejecuta el contenedor Docker
docker run -d -p 8888:8888 --name script-container flask-script

# Vuelve al directorio anterior
cd ..
Cómo ejecutar:Navega al directorio flask_script:cd /home/tu_usuario/docker_apps/flask_script
Haz el script deploy.sh ejecutable:chmod +x deploy.sh
Ejecuta el script de despliegue:./deploy.sh
El script construirá la imagen flask-script y ejecutará un contenedor.Abre tu navegador y visita http://localhost:8888 para ver tu dirección IP.Nota: El script deploy.sh crea un directorio tempdir, copia sample.py a tempdir/app.py y genera un Dockerfile en tempdir antes de construir y ejecutar la imagen Docker. Si ejecutas el script varias veces, puede que necesites detener y eliminar los contenedores y/o imágenes anteriores para evitar conflictos de nombres o puertos.Para limpiar los contenedores y las imágenes después de usarlos:# Para detener y eliminar un contenedor específico
docker stop <nombre_del_contenedor_o_id>
docker rm <nombre_del_contenedor_o_id>

# Para eliminar una imagen específica
docker rmi <nombre_de_la_imagen_o_id>
RequisitosDocker instalado en tu sistema.ContribucionesSiéntete libre de clonar, modificar y expandir estos ejemplos.
