## Authentication
- Logs in a user
- K8s does not manage any usernames
  - but it is being used for authentication

### Supported users of K8s
- **Normal Users**
  - managed outside of the K8s cluster via independent services
  - e.g. User/Client Certificiates, Google accounts

- **Service Accounts**
  - allow in-cluster processes to communicate with the API server to perform various operations
  - most service accounts are created via the API server but can be created manually
  - tied to a namespace. communicates with the API server through secrets

### K8s authentication module
- **X509 Client Certificates**
  - *--client-ca-file=SOMEFILE*
    - used to reference the certificate authorities
    - the authorities will validate the client certificates presented in the API server

- **Static Token File**
  - *--token-auth-file=SOMEFILE*
    - used to pass a pre-defined bearer tokens to the API server
    - the token will last indefinitely unless the API server is restarted

- **Boostrap Tokens**
  - tokens used for bottstrapping new K8s clusters

- **Service Account Tokens**
  - automatically enabels authenticators that used the signed bearer token to verify requests. 
  - the tokens get attached to Pods in ServiceAccount Admission Controller, which allows in-cluster processes to talk to the API server

- **OpenID Connect Tokens**
  - used to connect with OAuth2 providers, such as Azure AD, Salesforce, Google

- **Webhook Token Authentication**
  - verification of bearer tokens can be offloaded to a remote service

- **Authenticating Proxy**
  - another program with additional authentication logic

> Note: for successful authentication, two methods should be enabled: service account token and one of the user authenticators
