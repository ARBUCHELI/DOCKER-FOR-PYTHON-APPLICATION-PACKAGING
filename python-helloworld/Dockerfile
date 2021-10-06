FROM python:3.8
LABEL maintainer="Katie Gamanji"

COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt

# command to run on container start

EXPOSE 5000

CMD [ "python", "app.py" ]
