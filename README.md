# FlaskDockerSimpleApp
A example repo building a simple FlaskApp with Docker

### Create local python venv
```bash
python3 -m venv venv

# activate local python venv
source venv/bin/activate
# or
. venv/bin/activate
```

### Install Flask and freeze requirements
```bash
pip install Flask

pip freeze > requirements.txt
```

### Set up hello.py
```python
from flask import Flask
import os

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=int(os.environ.get("PORT", 5001)))
```

### Dockerfile and build & run
```docker
FROM python:3

# set a directory for the app
WORKDIR /usr/src/app

# copy all the files to the container
COPY . .

# install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# tell the port number the container should expose
EXPOSE 5001

# run the command
CMD ["python", "./hello.py"]

# run with starter script
# CMD ["/bin/bash", "run.sh"]
```

#### Build
```bash
docker build -t linian/helloflask .
```

#### Run
```bash
docker run -p 8888:5001 linian/helloflask
```

#### Run with run.sh
If we want to run Flask from a starter shell script like:
run.sh
```bash
#!/bin/bash

export FLASK_APP=hello
flask run --host=0.0.0.0 --port=5001
```
We need to change Dockerfile to:
```docker
FROM python:3

# set a directory for the app
WORKDIR /usr/src/app

# copy all the files to the container
COPY . .

# install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# tell the port number the container should expose
EXPOSE 5001

# run the command
# CMD ["python", "./hello.py"]

CMD ["/bin/bash", "run.sh"]
```
Then build and run:
```bash
docker build -t linian/helloflask .
docker run -p 8888:5001 linian/helloflask
```
Then open localhost:8888 on Browser will see:
```
Hello, World!
```
If CTRL+C is unable to stop docker run, first ```docker ps``` to find container id, then do ```docker stop containerId```

```
➜  docker ps
CONTAINER ID   IMAGE               COMMAND              CREATED         STATUS         PORTS                    NAMES
e9a519fa293c   linian/helloflask   "/bin/bash run.sh"   7 minutes ago   Up 7 minutes   0.0.0.0:8888->5001/tcp   vigorous_snyder

➜  docker stop e9a519fa293c
e9a519fa293c
```