# Postfix Relay Demo

This demo deploys a simple Postfix relay using the `krallin/postfix` Docker image.

## Components

- **Namespace**: mail-relay
- **Secret**: postfix-relay-secret (contains SMTP credentials)
- **Deployment**: postfix-relay
- **Service**: postfix-relay (ClusterIP on port 25)

## Usage

1.  Apply the resources:

    ```bash
    kubectl apply -f k8s/postfix-relay/namespace.yaml
    kubectl apply -f k8s/postfix-relay/secret.yaml
    kubectl apply -f k8s/postfix-relay/deployment.yaml
    kubectl apply -f k8s/postfix-relay/service.yaml
    ```

2.  Configure n8n SMTP credentials:

    - Host: `postfix-relay.mail-relay.svc.cluster.local`
    - Port: `25`
    - User: `invite`
    - Password: `invitepwd`

3.  Send a test email from n8n (e.g. using the “Send Email” node).

> **Note:** For production you may want to adjust `maildomain`, add TLS, or configure a relayhost.
