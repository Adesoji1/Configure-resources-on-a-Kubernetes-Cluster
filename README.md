# Configure resources on a Kubernetes Cluster

In this challenge, you will deploy resources in a Kubernetes cluster. First, you will retrieve information about the cluster. Next, you will create cluster resources, and then you will organize cluster resources in a namespace. Finally, you will update cluster resources, and then you will roll back a Deployment.
write the correct manifest files too

In this chalenge, I will guide you through the following steps to configure resources in a Kubernetes cluster:
How do i install kubectl?

kubectl is a command-line tool for running commands against Kubernetes clusters. It is essential for managing and deploying applications on a Kubernetes cluster. Here are the steps to install kubectl on different operating systems:

macOS:

You can use Homebrew to install kubectl:

```markdown
brew install kubectl

```

Or, you can install kubectl using the following curl command:

```markdown
curl -LO "<https://storage.googleapis.com/kubernetes-release/release/$(curl> -s <https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl>"

```

```markdown
chmod +x ./kubectl

```

```markdown
sudo mv ./kubectl /usr/local/bin/kubectl

```

`Windows:`

You can use Chocolatey to install kubectl:

```markdown
choco install kubernetes-cli

```

Alternatively, you can download the latest release with this command:

```markdown
curl -LO <https://storage.googleapis.com/kubernetes-release/release/$(curl> -s <https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/windows/amd64/kubectl.exe>

```

Then, add the binary in to your PATH.

`Linux:`

You can install kubectl using the following curl command:

```markdown
curl -LO "<https://storage.googleapis.com/kubernetes-release/release/$(curl> -s <https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl>"

```

```markdown
chmod +x ./kubectl

```

```markdown
sudo mv ./kubectl /usr/local/bin/kubectl

```

After installation, you can verify that kubectl is installed by running:

```markdown
kubectl version --client

```

If you don't want to install kubectl locally, you can use an alternative like Kubernetes Dashboard or Lens IDE to manage your cluster. However, kubectl is widely used, and it's recommended to have it installed for managing Kubernetes clusters efficiently.

```markdown
1. Retrieve information about the cluster

```

```markdown
kubectl get nodes

```

```markdown
kubectl get namespaces

```

```markdown
kubectl cluster-info

```

```markdown
2. Create cluster resources

```

```markdown
kubectl apply -f deployment.yaml

```

```markdown
kubectl apply -f service.yaml

```

```markdown
3. Organize cluster resources in a namespace

```

## Create a new namespace

```markdown
kubectl create namespace kubenamespace

```

## Update the YAML files to include the namespace

## Add the following line under the metadata section in both deployment.yaml and service.yaml

namespace: kubenamespace

## Apply the updated YAML files

```markdown
kubectl apply -f deployment.yaml

```

```markdown
kubectl apply -f service.yaml

```

```markdown
4. Update cluster resources

```

## Update the deployment YAML file, for example, change the nginx image version to 1.19.10

image: nginx:1.19.10

## Apply the updated YAML file

```markdown
kubectl apply -f deployment.yaml

```

```markdown
5. Roll back a Deployment

```

## Check the rollout history

```markdown
kubectl rollout history deployment/nginx-deployment -n kubenamespace

```

## Roll back to the previous revision

```markdown
kubectl rollout undo deployment/nginx-deployment -n kubenamespace

```

It's a good practice to set resource limits for containers in a Kubernetes deployment to avoid resource starvation for other processes. You can set resource limits by adding the `resources` field with limits and `requests` subfields under the container definition. Here's an example of how to set resource limits for CPU and memory in your `deployment.yaml` file:

## Using helm Chart

```markdown
6. Using helm Chart

```

Helm is a package manager for Kubernetes that simplifies the deployment and management of applications in a Kubernetes cluster. It uses "charts" as a packaging format, which is a collection of files that describe a related set of Kubernetes resources. A chart is like a blueprint for deploying and managing applications in a Kubernetes cluster.

A Helm chart typically consists of the following components:

Chart.yaml: Contains metadata about the chart, such as name, version, and description.
values.yaml: Contains default configuration values that can be overwritten during the installation or upgrade of a chart.
templates: A directory containing one or more Kubernetes manifest files (like deployment.yaml and service.yaml) that define the Kubernetes resources to be deployed.
Helm can help you manage the complexity of deploying and updating applications by automating the process and providing a versioning mechanism for your releases. Here's how you can use Helm in your current scenario:

Install Helm by following the instructions for your specific operating system:[Here](https://helm.sh/docs/intro/install)

Create a Helm chart for your application by running the following command:

```markdown
helm create nginx-chart

```

This command creates a directory named `nginx-chart` with the following structure:

1. nginx-chart/
2. Chart.yaml
3. values.yaml
4. charts/
5. templates/

Replace the contents of `nginx-chart/templates/deployment.yaml` and `nginx-chart/templates/service.yaml` with your deployment and service manifests.

Modify the `nginx-chart/values.yaml` file to define any configurable parameters for your application. These values can be referenced in the template files using the `{{ .Values.parameterName }}` syntax.

Package the Helm chart:

```markdown
helm package nginx-chart

```

This command creates a (`nginx-chart-0.1.0.tgz`) file in the current directory containing your helm chart.

To install the Helm chart on your Kubernetes cluster, use the helm install command:

```markdown
helm install my-nginx-release nginx-chart-0.1.0.tgz

```

To update your application, modify the templates or values in your Helm chart, increase the version in `Chart.yaml`, re-package the chart, and then use `helm upgrade` to apply the changes:

`helm upgrade my-nginx-release nginx-chart-0.1.1.tgz`

To uninstall the Helm chart, use the helm uninstall command:

```markdown
helm uninstall my-nginx-release 

```

If you need to roll back a release, use the helm rollback command:

```markdown
helm rollback my-nginx-release 1
```

This command rolls back the release to the previous version.

In summary, Helm simplifies the deployment and management of applications on Kubernetes by providing a versioning mechanism and automating the deployment process. It uses charts, which are collections of Kubernetes manifest files (like deployment.yaml and service.yaml), to define the resources to be deployed.

## Question

how do i do these? Modify the nginx-chart/values.yaml file to define any configurable parameters for your application. These values can be referenced in the template files using the {{ .Values.parameterName }} syntax?

To modify the nginx-chart/values.yaml file to define configurable parameters for your application, follow these steps:

Open the nginx-chart/values.yaml file in a text editor.

Add the parameters you want to make configurable. For example, let's make the replica count, the container image, and the container port configurable:

- replicaCount: 3
- image:
- repository: nginx
- tag: 1.14.2
- containerPort: 80

Save and close the values.yaml file.

Now, open the nginx-chart/templates/deployment.yaml file in a text editor.

Replace the hardcoded values with the configurable parameters using the {{ .Values.parameterName }} syntax. For example, replace the replica count, the container image, and the container port with the values from the values.yaml file:

```markdown
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "nginx-chart.fullname" . }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ include "nginx-chart.fullname" . }}
  template:
    metadata:
      labels:
        app: {{ include "nginx-chart.fullname" . }}
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        ports:
        - containerPort: {{ .Values.containerPort }}
```

Save and close the deployment.yaml file.
Now, when you install or upgrade your Helm chart, you can provide different values for these parameters using the --set flag, or by creating a custom values.yaml file and passing it with the -f flag. For example, to change the replica count and container port during installation:

```markdown
helm install my-nginx-release nginx-chart-0.1.0.tgz --set replicaCount=5,containerPort=8080
```

Or, create a custom values.yaml file with your desired settings:

```markdown
replicaCount: 5
image:
  repository: nginx
  tag: 1.14.2
containerPort: 8080
```

Then, install the Helm chart using this custom values.yaml file below

```markdown

helm install my-nginx-release nginx-chart-0.1.0.tgz -f my-custom-values.yaml

```

By making parameters configurable in your Helm chart, you can more easily deploy different configurations of your application without modifying the Kubernetes manifests directly.

After deploying your application using Helm, you need to expose the application to the internet or your internal network to access the website. There are multiple ways to do this, but one common method is to use a Kubernetes Service of type LoadBalancer or NodePort.

Assuming you have a Service manifest file in your Helm chart (e.g., nginx-chart/templates/service.yaml), you can follow these steps:

Open the nginx-chart/templates/service.yaml file in a text editor.

Modify the spec section of the Service to use the LoadBalancer or NodePort type. Here's an example using LoadBalancer:

```markdown
apiVersion: v1
kind: Service
metadata:
  name: {{ include "nginx-chart.fullname" . }}
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: {{ .Values.containerPort }}
  selector:
    app: {{ include "nginx-chart.fullname" . }}
```

Note that you should replace {{ .Values.containerPort }} with the actual port number if you haven't defined it in your values.yaml file.

Save and close the service.yaml file.

Update your Helm chart version in Chart.yaml, package the chart, and then upgrade the release with the command below:

```markdown
helm upgrade my-nginx-release nginx-chart-0.1.1.tgz
```

Get the external IP address (for LoadBalancer type) or the node IP and port (for NodePort type) of your Service:

```markdown
kubectl get services
```

This command will display a list of services running in your cluster. Look for the external IP address (in the EXTERNAL-IP column) if you're using a LoadBalancer, or the node port (in the PORT(S) column, after the colon) if you're using a NodePort.

Access the website using the external IP address and port (for LoadBalancer) or the node IP address and node port (for NodePort). For example

```markdown
# <http://<external-ip>:80>
```

or

```markdown
# <http://<node-ip>:<node-port>>

```

Port forwarding is not always necessary, but it can be helpful in certain scenarios. Whether you need to use port forwarding depends on your specific use case and your Kubernetes environment.

Port forwarding can be useful when:

You are running a Kubernetes cluster locally (e.g., using minikube, kind, or k3s) and want to access a service running inside the cluster from your host machine or other devices on your local network.
You are using a Kubernetes cluster without built-in support for external load balancers, and you want to access a service from outside the cluster without using a NodePort service.
You want to temporarily access a specific pod within the cluster for debugging purposes without exposing it through a service.
If your use case falls into any of these categories, you might want to use port forwarding. To do this with kubectl, use the port-forward command, specifying the resource (such as a pod, deployment, or service) and the ports you want to forward:

```markdown
kubectl port-forward <resource-name> <local-port>:<remote-port>

```

For example, to forward local port 8080 to a deployment named nginx-deployment on port 80, you can run:

kubectl port-forward deployment/nginx-deployment 8080:80

Now, you can access the website at <http://localhost:8080>. or Then, you can access the service in your browser using the local port:

<http://localhost:8080>

Keep in mind that port forwarding is not a long-term solution for exposing services to external users, as it creates a direct connection between your machine and the Kubernetes resource, bypassing any security mechanisms provided by services, ingresses, or network policies. It's best used for temporary access, testing, or debugging purposes. For production scenarios, it's recommended to use LoadBalancer, NodePort, or Ingress resources to expose your applications.

 The End :smile:
