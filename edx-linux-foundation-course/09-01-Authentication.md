# Authorization
- Authorizes the APi requests submitted by the authenticated user
- after successful authentication, the submitted request by the user should be authorized

## API Requests Attributes to be checked
- the following attributes are checked but not limited to:
  - user
  - group
  - extra
  - resource
  - namespace
  - API group
- Policies

## Authorization Modes
- **Node**
  - special-purprose authorization mode specifically authorizes API requests made by kubelets
    - read operations
      - services
      - endpoints
      - nodes
    - write operations
      - nodes
      - pods
      - events

- **Attribute-Based Access Control (ABAC)**
  - grants API requests which combines *policies* with *attributes*
  - Example:
    - *user* student can only read pods in namespace - lfs1158
    - to enable, upon start-up of API server. the parameter **--authorization-mode=ABAC** should be set and **--authorization-policy-file=PolicyFile.json** should be specified

 ```
{
  "apiVersion": "abac.authorization.kubernetes.io/v1beta1",
  "kind": "Policy",
  "spec": {
    "user": "student",
    "namespace": "lfs158",
    "resource": "pods",
    "readonly": true
  }
}
 ```

- **Webhook**
  - can request authorization to third-party services
  - to use, the API server should be started with **--authorization-webhook-config-file=SOME_FILENAME**

- **Role-Based Access Control (RBAC)**
  - multiple roles can be attached to objects
    - roles can restrict resource access by specific operations (e.g. create, get, update, patch)

### Two kinds of Roles:
- **Role**
  - grant access to resources within a specific namespace

- **ClusterRole**
  - same as Role but clusterwide

Below is an example of *Role* configuration:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: lfs158
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```
> pod-reader role has access only to read the Pods of lfs158 Namespace. After the role is create, it should be binded to the users with a RoleBinding object.

## RoleBinding Kinds
- **RoleBinding**
  - bind users to the same namespace as a role

- **ClusterRoleBinding**
  - grant access to resources at a cluster-level and to all namespace

Example of Role Binding:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-read-access
  namespace: lfs158
subjects:
- kind: User
  name: student
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

To enable RBAC mode, the API server should be started with the parameter **--authorization-mode=RBAC** option