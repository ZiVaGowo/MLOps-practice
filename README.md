# MLOps-практикум (1 час)

Стек: DVC, MLflow, Docker Compose, GitHub Actions.

## Запуск инфраструктуры

```bash
docker-compose up -d
```

- MLflow UI: http://localhost:5000
- MinIO UI: http://localhost:9001 (логин `minioadmin`, пароль `minioadminpassword`)

## Обучение и Quality Gate

```bash
pip install -r requirements.txt
python prepare_data.py
python train.py
python eval_gate.py
```

`eval_gate.py` пропускает модель `production_classifier` в стадию `Staging`, если `f1_score >= 0.80`.

## DVC

```bash
dvc init
dvc remote add -d minio s3://mlflow/dvc
dvc remote modify minio endpointurl http://localhost:9000
dvc remote modify minio access_key_id minioadmin
dvc remote modify minio secret_access_key minioadminpassword
```

После этого данные можно версионировать так:

```bash
dvc add data.csv
dvc push
```

## CI

При push/PR в `main` GitHub Actions поднимает стек, обучает модель и запускает Quality Gate.
