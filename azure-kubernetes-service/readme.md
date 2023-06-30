## CREATE THE ACR

1. Create it on azure portal
2. Get the acr infos, like name, login, password, etc

## Push the container

1. Tag the image: `docker tag LOCAL_DOCKER_IMAGE_NAME ACR_NAME.azurecr.io/IMAGE_NAME_IN_CLOUD:VERSION`
2. Login on Azure CLI: `az acr login --name ACR_NAME`
3. Push the container to acr: `docker push ACR_NAME.azurecr.io/CONTAINER_PATH_ON_ACR/IMAGE_NAME_IN_CLOUD:VERSION`

## Registry secret

1. Create the secret `kubectl create secret docker-registry registry_name.secret --docker-server ACR_NAME.azurecr.io --docker-username REGISTRY_USERNAME --docker-password REGISTRY_PASSWORD`

Example yaml:

    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
    name: deployment-application
    spec:
    template:
        metadata:
        labels:
        name: deployment-application
        spec:
        containers:
            - name: container-application
            image: ACR_NAME.azurecr.io/CONTAINER_PATH_ON_ACR/IMAGE_NAME_IN_CLOUD:VERSION
            ports:
                - containerPort: 80
                imagePullSecrets:
                        - name: ACR_NAME.secrets