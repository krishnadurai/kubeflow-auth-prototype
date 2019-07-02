# Kubeflow Authentication and Authorization Prototype

## High Level Diagram
![Authentication and Authorization in Kubeflow](assets/auth-istio.png)


## Create SLL Certificates

This example is going to need three domains:  
- dex.example.org: For the authentication server
- login.example.org: For the client application for authentication
- ldap-admin.example.org: For the admin interface to create LDAP users and groups

**Note**: Replace *example.org* with your own domain.  

With your trusted certificate signing authority, please create a certificate for the above domains.

### Why Self Signed SSL Certs will not work

Authentication through OIDC in Kubernetes does work with self signed certificates since the `--oidc-ca-file` parameter in the Kubernetes API server allows for adding a trusted CA for your authentication server.

Though Istio's authentication policy parameter `jwksUri` for [End User Authentication](https://istio.io/docs/ops/security/end-user-auth/) does [not allow self signed certificates](https://github.com/istio/istio/issues/7290#issuecomment-420748056).

Please generate certificates with a trusted authority for enabling this example or follow this [work-around](#self-signed-work-around).

## Authentication Server Setup

 Fill in Kustomize with overlays for certs and values.
