
# Usage

## 1. Configure MLflow

The `00-mlflow-crd.yaml` file contains deliberate environment variable settings to:

-   Enable the `HTTP` scheme for webhook communication.
-   Allow connections to private IP addresses.

> **Note:** These settings are intended for development/testing environments and are **not suitable for production deployments**. 

## 2. Configure the webhook encryption key

Update `MLFLOW_WEBHOOK_SECRET_ENCRYPTION_KEY` in `00-mlflow-crd.yaml` with a fixed Fernet encryption key.

Generate a Fernet key using:

```
FERNET_KEY=$(python3 -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())" 2>/dev/null)

```

Use the generated value as the `MLFLOW_WEBHOOK_SECRET_ENCRYPTION_KEY`.

> **Important:** Use a fixed key across deployments. Do not generate a new key for every deployment, as changing the key prevents MLflow from decrypting previously stored webhook secrets.

## 3. Configure network policy egress

Update the network policy egress rules in `00-mlflow-crd.yaml` as required to allow MLflow to connect to the required webhook receiver. 

## 4. Deploy the artifacts

Deploy the artifacts in the order defined by the filename. 
