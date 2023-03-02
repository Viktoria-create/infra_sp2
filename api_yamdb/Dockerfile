#FROM python:3.7.16-slim

#WORKDIR /app/api_yamdb

#COPY api_yamdb/requirements.txt /app

#RUN pip3 install -r /app/requirements.txt --no-cache-dir
#RUN pip3 install -r /app/requirements.txt

#COPY ../ /app

#RUN python manage.py collectstatic --no-input  

#RUN addgroup --system app && \
#    adduser --system --ingroup app app

#CMD ["gunicorn", "api_yamdb.wsgi:application", "--bind", "0:8000" ]
FROM python:3.7-slim
WORKDIR /app
COPY api_yamdb/requirements.txt ./
RUN pip3 install -r requirements.txt --no-cache-dir
COPY api_yamdb/ ./
CMD ["gunicorn", "api_yamdb.wsgi:application", "--bind", "0:8000" ]
