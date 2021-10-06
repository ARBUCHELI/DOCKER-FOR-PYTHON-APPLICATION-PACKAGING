# DOCKER-FOR-APPLICATION-PACKAGING AND PUSHING TO DOCKER HUB:

## DOCKER COMMANDS:

### (Don't forget to nativate to the directory that contains the app and the docker file):
* docker build -t python-helloworld .
* docker images
* docker run -d -p 5000:5000 python-helloworld
* docker ps
* docker tag python-helloworld rothwulf/python-helloworld:v1.0.0
* docker push rothwulf/python-helloworld:v1.0.0

## IMPORTANT:

* Edit the app on this way:
```
from flask import Flask
from flask import json
import logging

app = Flask(__name__)

@app.route('/status')
def healthcheck():
    response = app.response_class(
            response=json.dumps({"result":"OK - healthy"}),
            status=200,
            mimetype='application/json'
    )

    app.logger.info('Status request successfull')
    return response

@app.route('/metrics')
def metrics():
    response = app.response_class(
            response=json.dumps({"status":"success","code":0,"data":{"UserCount":140,"UserCountActive":23}}),
            status=200,
            mimetype='application/json'
    )

    app.logger.info('Metrics request successfull')
    return response

@app.route("/")
def hello():
    app.logger.info('Main request successfull')

    return "Hello World!"

if __name__ == "__main__":
    ## stream logs to a file
    logging.basicConfig(filename='app.log',level=logging.DEBUG)
    
    app.run(host='0.0.0.0', port=5000) # --------> THIS PART IS VERY IMPORTANT FOR A SUCCESSFUL DEPLOYMENT
```

* Edit the dockerfile on this way:
```
FROM python:3.8
LABEL maintainer="Katie Gamanji"

COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt

# command to run on container start

EXPOSE 5000 # --------> THIS PART IS VERY IMPORTANT TOO FOR A SUCCESSFUL DEPLOYMENT

CMD [ "python", "app.py" ] 
```

* THE LAST POINT TO TAKE INTO CONSIDERATION:

Check what IP you got for DOCKER_HOST after running the above command(default is 192.168.99.100:2376)

In browser type whatever IP you got for DOCKER_HOST along with port 5000 Ex: http://192.168.99.100:5000/
