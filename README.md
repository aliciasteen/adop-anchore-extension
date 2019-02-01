# adop-anchore-extension

Anchore Engine is an open source project that provides a centralized service for inspection, analysis and certification of container images. It can be integrated into ADOP and Jenkins to enhance CI/CD pipelines.

# Deploy Anchore Engine in ADOP
Full offical documentaion can be found here [Install with Docker Compose](https://anchore.freshdesk.com/support/solutions/articles/36000020729-install-with-docker-compose)

1. Clone this repo onto host running ADOP/and cd into folder
```
git clone https://github.com/aliciasteen/adop-anchore-extension.git
cd adop-anchore-extension
```
2. Export configuration
CUSTOM_NETWORK_NAME is the network ADOP containers are running in.
```
export CUSTOM_NETWORK_NAME=local_network
export ADMIN_PASSWORD=example_password
export POSTGRES_PASSWORD=example_password
```
3. Additonal configuration  that can be set in config/config.yml is outlined here: [Configuration Documentation](https://anchore.freshdesk.com/support/solutions/articles/36000020733-configuration)
4. Deploy Anchore engine
```
docker-compose up -d
```

# Set Up Jenkins

Integrating Anchore with Jenkins is as easy as installing a plugin. You can then add Anchore scanning to any job/pipeline.

## Install Anchore plugin. 
In Jenkins, select `Manage Jenkins`, then `Manage Plugins`. Select the `Avalible` tab and install `Anchore Container Image Scanner`

## Configure Plugin
In Jenkins, select `Manage Jenkins`, then `Configure Jenkins`. Scroll to `Anchore Configuration`. Choose `Engine Mode` and enter the following parametes:

- Engine URL = `http://anchore-engine:8228/v1`
- Engine Username = `admin`
- Enginer Password = `example password`

# Anchore Scan in Jenkins Job
This example will explain how to add Anchore to a simple Freestyle job but scanning can also be used in Pipeline jobs. (Note this is not a full example and will need to be expanded)

1. Add Shell build step
```
docker build -t $IMAGE_NAME:$TAG .
docker push $IMAGE_NAME:$TAG
echo "$IMAGE_NAME:$TAG ${WORKSPACE}/Dockerfile" > anchore_images
```
2. Add an additional build step `Anchore Container Image Scanner`, the default values will work with this simple job.
3. After running the job the Anchore Plugin will create a Anchore Report. A link will show in the build page.

# Contribution

Please make PR's and fork this repo to help make it better!