# Postfix Relay Demo

This demo deploys a simple Postfix relay using the `mwader/postfix-relay` Docker image.

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

2.  Wait for postfix-relay to be ready:

    ```bash
    kubectl -n mail-relay wait --for=condition=available deployment/postfix-relay --timeout=60s
    ```

3.  Configure n8n SMTP credentials:

    - Host: `postfix-relay.mail-relay.svc.cluster.local`
    - Port: `25`
    - User: `invite`
    - Password: `invitepwd`

4.  Send a test email from n8n (e.g. using the “Send Email” node).

> **Note:** For production you may want to adjust `maildomain`, add TLS, or configure a relayhost.
