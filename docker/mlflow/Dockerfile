FROM python:3

RUN pip install --upgrade pip && \
    pip install mlflow

# mount dir
RUN mkdir -p /mlflow/mlruns
WORKDIR /mlflow/mlruns
