Usage

Follow the steps below to configure and deploy the MLflow webhook resources.

1. Review the MLflow Configuration

The 00-mlflow-crd.yaml file contains deliberate environment variable settings to:

Enable the HTTP scheme for webhook communication.
Allow connections to private IP addresses.

Warning: These settings are intended for development or testing environments and are not recommended for production deployments. Review and harden these settings before deploying to production.

2. Configure the Webhook Encryption Key

Update MLFLOW_WEBHOOK_SECRET_ENCRYPTION_KEY in 00-mlflow-crd.yaml with a persistent Fernet encryption key.

Generate a new key using:

FERNET_KEY=$(python3 -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())" 2>/dev/null)
echo "$FERNET_KEY"


Use the generated value for MLFLOW_WEBHOOK_SECRET_ENCRYPTION_KEY.

Important: Use a fixed key for the deployment. Do not generate a new key on every deployment, as changing the key can prevent MLflow from decrypting previously stored webhook secrets.

3. Configure Network Policy Egress

Review the network policy egress rules in 00-mlflow-crd.yaml.

Update the rules as required to allow MLflow to establish outbound connections to the configured webhook receiver.

Ensure that the egress policy is restricted to only the destinations and ports required by the webhook integration.

4. Deploy the Resources

Deploy the artifacts in the order defined by the file names.

For example:

kubectl apply -f <artifact-1>.yaml
kubectl apply -f <artifact-2>.yaml
# Continue in the defined order


Ensure that each resource is successfully created and ready before proceeding to the next artifact.
