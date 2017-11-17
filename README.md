# cloud-sdk-appengine
Docker image based on Alpine Linux to create and deploy to AppEngine in BitBucket Pipelines.

## Usage:
- Create a service account in the Google Cloud Console, and download the JSON keyfile.
- Make sure the service account is granted the following roles:
  - Storage Admin
  - App Engine Admin
  - Cloud Container Builder
  - Service Management Administrator
- Make sure the following API's are enabled:
  - Google Service Management API
  - Google Cloud Container Builder API
  - Google App Engine Admin API
  - Google Cloud Deployment Manager API
  - Google Compute Engine API
- Setup a secure environment variable called 'DOCKER_PASSWORD' in Pipelines with the contents of the JSON keyfile.
- Create a project with app.yaml (and optional Dockerfile) in the root folder of the project
- Use the following pipeline file as a template:
```
    image: blagerweij/cloud-sdk-appengine

    pipelines:
      default:
        - step:
            script:
              - ./gradlew --console=plain build
              - echo "$DOCKER_PASSWORD" | gcloud auth activate-service-account --key-file -
              - gcloud --quiet app deploy app.yaml --project myprojectid --promote

    options:
      docker: true
```
