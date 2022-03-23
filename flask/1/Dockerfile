FROM python:3.7-alpine
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]


## FROM python:3.7
## FROM python:3.7-slim
