## Jenkinshelm

#Pre-requisite,

1.K8 cluster is ready along with docker installed on it.

2.Namespace "jenkins" already created on k8.

3.Helm V3 already installed on k8 cluster.

##Main Configuration,

Configure Helm
Once Helm is installed and set up properly, add the Jenkins repo as follows:

$ helm repo add jenkinsci https://charts.jenkins.io

$ helm repo update

The helm charts in the Jenkins repo can be listed with the command:

$ helm search repo jenkinsci

Create a persistent volume

We choose to use the /data directory. This directory will contain our Jenkins controller configuration.

We will create a volume which is called jenkins-pv

Run the following command to apply the spec:

$ kubectl apply -f jenkins-volume.yaml

 hostPath uses the /data/jenkins-volume/ of your node or if whatever path you have on your node make respective changes as per that.
 
 Create a service account
 
 We will create a service account called jenkins.

A ClusterRole is a set of permissions that can be assigned to resources within a given cluster.

Install Jenkins
We will deploy Jenkins including the Jenkins Kubernetes plugin. See the official chart for more details.

To enable persistence, we will create an override file and pass it as an argument to the Helm CLI. Use the content YAML formatted file called jenkins-values.yaml.
 
Open the jenkins-values.yaml file in your favorite text editor and modify the following:

nodePort: Because we are using minikube we need to use NodePort as service type. Only cloud providers offer load balancers. We define port 32000 as port.

storageClass:

storageClass: jenkins-pv
serviceAccount: the serviceAccount section of the jenkins-values.yaml file should look like this:

serviceAccount:
create: false
# Service account name is autogenerated by default
name: jenkins
annotations: {}
Where `name: jenkins` refers to the serviceAccount created for jenkins.
We can also define which plugins we want to install on our Jenkins. We use some default plugins like git and the pipeline plugin.

Now you can install Jenkins by running the helm install command and passing it the following arguments:

The name of the release jenkins

The -f flag with the YAML file with overrides jenkins-values.yaml

The name of the chart jenkinsci/jenkins

The -n flag with the name of your namespace jenkins

$ chart=jenkinsci/jenkins
$ helm install jenkins -n jenkins -f jenkins-values.yaml $chart
This outputs something similar to the following:
NAME: jenkins
LAST DEPLOYED: Wed Sep 16 11:13:10 2020
NAMESPACE: jenkins
STATUS: deployed
REVISION: 1

