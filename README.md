Despliegue Automatizado

Ejecuta un script que genera dinamicamente el entorno Docker y lanza la aplicacion.

**Script principal:**

```bash
# flask_script/deploy.sh

#!/bin/bash

mkdir -p tempdir/templates tempdir/static
cp sample.py tempdir/app.py
touch tempdir/templates/placeholder.txt
touch tempdir/static/placeholder.txt

cat <<EOF > tempdir/Dockerfile
FROM python
RUN pip install flask
COPY ./static /home/myapp/static/
COPY ./templates /home/myapp/templates/
COPY app.py /home/myapp/
EXPOSE 8888
CMD python3 /home/myapp/app.py
EOF

cd tempdir
docker build -t flask-script .
docker run -d -p 8888:8888 --name script-container flask-script
cd ..
```

**Aplicacion (sample.py):**

```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def main():
    return "Tu direcciÃ³n IP es: " + request.remote_addr + "\n"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8888, debug=True, use_reloader=False)
```

**Como ejecutar:**

```bash
cd docker_apps/flask_script
chmod +x deploy.sh
./deploy.sh
```

Abre tu navegador en `http://localhost:8888`.

---

## Limpieza (opcional)

```bash
# Detener y eliminar contenedor
docker stop <nombre_o_id>
docker rm <nombre_o_id>

# Eliminar imagen Docker
docker rmi <nombre_o_id>
```

---

## ¦ Requisitos

- Docker instalado  
- (Opcional) Python si quieres probar localmente sin contenedor 
