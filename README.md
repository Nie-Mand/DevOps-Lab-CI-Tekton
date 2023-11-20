# DevOps Lab 4


## How to run this

```sh

# Run a Kind Cluster
kind create cluster --config infra/dev/kind.yml

# Deploy SonarQube
kubectl apply -f infra/dev/sonarqube.yml
# Then setup SonarQube, Create a token and a project, and set up the configurations
# in the Golang Configurations, set:
# - `sonar.go.tests.reportPaths` to 'out/test-report.out'
# - `sonar.go.coverage.reportPaths` to 'out/coverage.out'
# Update the `sonarqube-token` in the `ci/memez-pipeline.yml` file
 
# Setup Tekton
kubectl apply -k infra/dev/tekton
kubectl apply -k ci/trigger
kubectl apply -k ci/hub

# Apply the pipeline
kubectl apply -k ci/memez-pipeline


# Expose the Github Trigger, use ngrok or something similar to expose the webhook
kubectl port-forward svc/el-github-listener 9999:8080
ngrok http 9999

# Create a webhook in the Github repo, and point it to the ngrok url

# Try it
```