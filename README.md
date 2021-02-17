# COS sFtp 

Deploys an app to provide SFTP access to COS buckets

## Setup

Prepare the cluster for the SFTP pod by installing the Object Storage plugin - https://cloud.ibm.com/docs/containers?topic=containers-object_storage

## Configuration

Uses the atmoz/sftp docker container to provide the sftp service - https://github.com/atmoz/sftp

All of the configuration options available are documented there.

### Host SSH key

1. Generate the keys

    ```shell
    ssh-keygen -t ed25519 -f ssh_host_ed25519_key < /dev/null
    ssh-keygen -t rsa -b 4096 -f ssh_host_rsa_key < /dev/null
    ```

2. Create a secret containing the keys

```shell
kubectl create secret generic host-ssh-keys --from-file=ssh_host_ed25519_key --from-file=ssh_host_ed25519_key.pub --from-file=ssh_host_rsa_key --from-file=ssh_host_rsa_key.pub -n ${NAMESPACE}
```

**Note:** This should be updated to store the keys in a secret manager.

### Deploy the SFTP resources

```shell
helm template cos-sftp chart/cos-sftp | kubectl apply -n ${NAMESPACE} -f -
```

**Note:** The namespace parameter can be left off of the commands if the context is first set. Set the context with the following command:

```shell
kubectl config set-context --current --namespace=${NAMESPACE}
```
