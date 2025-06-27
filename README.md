Proyectos Flask con DockerEste repositorio contiene varios ejemplos de aplicaciones Flask, cada una empaquetada en un contenedor Docker. Los ejemplos cubren desde una aplicación Flask básica que muestra la IP del cliente hasta una aplicación más elaborada con HTML y CSS, y un script de despliegue automatizado.🚀 Cómo EmpezarPara utilizar estos ejemplos, clona el repositorio y navega al directorio del proyecto que te interese.git clone https://github.com/tu_usuario/tu_repositorio.git
cd tu_repositorio
Asegúrate de tener Docker instalado en tu sistema.
📁 Estructura del RepositorioLa estructura de directorios del repositorio es la siguiente:.
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
    ├── sample.py
    └── deploy.sh
1. Flask Básico: Mostrar IPUna aplicación Flask muy simple que devuelve la dirección IP del cliente que realiza la solicitud.📝 Archivos Clave:flask_basic/app.py: La aplicación Flask.flask_basic/Dockerfile: El Dockerfile para construir la imagen.
2. ⚙️ Cómo Ejecutar:Navega al directorio flask_basic:cd flask_basic
Construye la imagen Docker:docker build -t flask-basic .
Ejecuta el contenedor Docker:docker run -d -p 8000:8000 --name basic-ip-container flask-basic
Abre tu navegador y visita http://localhost:8000 para ver tu dirección IP.
2. Flask con HTML y CSS: Verificador de IPEsta aplicación Flask renderiza una página HTML para mostrar la dirección IP del cliente, utilizando CSS básico para el estilo.
📝 Archivos Clave:flask_html/app.py: La aplicación Flask que renderiza el HTML.flask_html/templates/index.html: El archivo de plantilla HTML.flask_html/static/style.css: El archivo de estilo CSS.flask_html/Dockerfile: El Dockerfile para construir la imagen.
⚙️ Cómo Ejecutar:Navega al directorio flask_html:cd flask_html
Construye la imagen Docker:docker build -t flask-html .
Ejecuta el contenedor Docker:docker run -d -p 8181:8181 --name html-ip-container flask-html
Abre tu navegador y visita http://localhost:8181 para ver la página con tu dirección IP.
4. Flask con Script de Despliegue AutomatizadoEste ejemplo demuestra un script de shell (deploy.sh) que automatiza la creación de un entorno Docker para una aplicación Flask, copiando los archivos necesarios y generando un Dockerfile sobre la marcha.
📝 Archivos Clave:flask_script/sample.py: La aplicación Flask que será desplegada por el script.flask_script/deploy.sh: El script que automatiza la construcción y el despliegue.
⚙️ Cómo Ejecutar:Navega al directorio flask_script:cd flask_script
Haz el script deploy.sh ejecutable:chmod +x deploy.sh
Ejecuta el script de despliegue:./deploy.sh
El script construirá la imagen flask-script y ejecutará un contenedor.Abre tu navegador y visita http://localhost:8888 para ver tu dirección IP.
Nota: El script deploy.sh crea un directorio tempdir temporalmente para construir la imagen. Si ejecutas el script varias veces, puede que necesites detener y eliminar los contenedores y/o imágenes anteriores para evitar conflictos de nombres o puertos.
🧹 Limpieza (Opcional):Para detener y eliminar los contenedores y las imágenes después de usarlos:# Listar contenedores para obtener los IDs/nombres
docker ps -a

# Detener y eliminar un contenedor específico
docker stop <nombre_del_contenedor_o_id>
docker rm <nombre_del_contenedor_o_id>

# Listar imágenes para obtener los IDs/nombres
docker images

# Eliminar una imagen específica
docker rmi <nombre_de_la_imagen_o_id>
🤝 ContribucionesSiéntete libre de clonar este repositorio, modificar los ejemplos o añadir los tuyos propios. ¡Las contribuciones son bienvenidas! 
