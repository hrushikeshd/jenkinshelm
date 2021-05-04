# jenkinshelm

Pre-requisite,

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
