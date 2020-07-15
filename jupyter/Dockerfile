# A imagem base da jupyter-vidalink
FROM jupyter/scipy-notebook

# Iniciamos a instalaçãoda das bibliotecas python
RUN conda install --quiet --yes \
    'mlflow=1.0.0' \
    'psycopg2' \
    'cx_Oracle'

RUN pip install --upgrade pip
COPY requirements.txt /home/jovyan/
RUN pip install -r requirements.txt

# Copia os notebooks para a pasta work
COPY notebooks/*  /home/jovyan/work/

# Inicia a instalação das dependencias para conectar no banco oracle
USER root

WORKDIR /opt/oracle

RUN wget https://download.oracle.com/otn_software/linux/instantclient/instantclient-basiclite-linuxx64.zip && \
    unzip instantclient-basiclite-linuxx64.zip && rm -f instantclient-basiclite-linuxx64.zip && \
    cd /opt/oracle/instantclient* && rm -f *jdbc* *occi* *mysql* *README *jar uidrvci genezi adrci && \
    echo /opt/oracle/instantclient* > /etc/ld.so.conf.d/oracle-instantclient.conf && ldconfig

RUN apt-get update -y && apt-get install libaio1


# Retorna o workdir para onde esta a aplicação e os notebooks
WORKDIR /home/jovyan

RUN addgroup --system jovyan

RUN chown -R jovyan:jovyan .

USER jovyan