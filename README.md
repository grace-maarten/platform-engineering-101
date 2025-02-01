# Platform engineering 101

## Step 1: OpenShift enablement
The first step to start the workshop is to have a running OpenShift cluster 
available. During the workshop, the OpenShift cluster and its integrations 
will serve as our internal developer platform. There are two ways to obtain an 
OpenShift cluster:
* From the [Red Hat Developers website](https://developers.redhat.com), 
    which offers tutorials, free e-books, and access to a free OpenShift sandbox.
* From a demo environment provided for you.

### Step 1a: Obtain OpenShift through developers.redhat.com
The [Red Hat Developers website](https://developers.redhat.com) 
is Red Hat's central hub for developers, 
offering many resources to build, deploy, and manage applications using 
Red Hat's technologies. It provides access to free e-books,
free tools, downloads, tutorials, 
documentation, and learning paths covering a range of topics like Quarkus, 
cloud-native application development, Kubernetes, 
containers, and AI/ML. Next to that, it offers hands-on labs, interactive guides, 
and insights from 
Red Hat experts. It’s also a gateway to communities, events, and open-source 
projects, making it a comprehensive platform for learning, collaboration, 
and innovation in modern application development.

Last, but not least, it offers a free sandbox, which allows you to start **a free
OpenShift cluster and/or a free OpenShift AI environment**. It is this sandbox that
we will use. 

**Benefits of the sandbox:**
* Free of charge.
* No credit card required.
* OpenShift cluster.
* OpenShift AI platform.

**Drawbacks of the sandbox:**
* Operator framework disabled (i.e., cost control).
* Cluster gets deleted after 30 days, but you can create a new one then.
* Pods get stopped after 12 hours, but you can restart them any time
  (e.g., through the deployment screen in the OpenShift console).

In order to get the OpenShift cluster, you will need to execute the following steps:
1. Go to the [Red Hat Developers website](https://developers.redhat.com).
2. Log in (or create an account).
3. Go to the menu item "Developer Sandbox".
4. Click "Explore the free Developer Sandbox".
5. Click "Start your sandbox for free".
6. Click "Get started" and you will see three available services
   * The OpenShift sandbox.
   * OpenShift dev spaces (an IDE running on OpenShift/Kubernetes), which is accessible through the browser.
   * The OpenShift AI sandbox.
7. Click "Launch" for Red Hat OpenShift.
8. Click Log in with "dev sandbox". (Use this GitHub repo as workspace Git repo URL: https://github.com/grace-maarten/platform-engineering-101.git).

### Step 1b: Obtain OpenShift through a demo environment
TODO - fallback scenario

## Step 2: Install Red Hat Developer Hub on OpenShift
_When interacting with an OpenShift cluster, you can make use of the OpenShift CLI._
_Command line tools can be found by logging in to an OpenShift cluster, clicking the 
question mark button in the top right corner and selecting the tool you want to download.
In case you don't want to configure your local machine, you can make use of Red Hat Dev Spaces._

To install Red Hat Developer Hub on OpenShift, you have two options:
1. Using an operator.
2. Using Helm charts.
  
The preferred installation method depends on the type of environment 
you've chosen (e.g., demo environment or developer sandbox):

* If your OpenShift cluster has the **Operator Framework enabled**, 
    the operator-based installation is recommended.
* If your OpenShift cluster does **not** have the **Operator Framework enabled**,
    **or** if you **require more fine-grained control**, the Helm-based installation is 
    the better option.

## Step 2a: Install Red Hat Developer Hub through Helm Charts
_You can't choose the namespace within the sandbox: It will be something like
'username + -dev'. Know that you will have to pay attention to the configurations 
later on in this workshop: you'll need to check that the namespace is correct as
most of the manifest are based upon the 'demo-project' namespace._

_We will be using 'developer-hub' as the name of the instance. Feel free to change it
to what you like, but know that you will have to pay attention to the configurations
later on in this workshop: you'll need to check that the name is correct if you
didn't go along with 'developer-hub'._

* In order to do so, you can follow 
[this training exercise](https://developers.redhat.com/learning/learn:openshift:install-and-configure-red-hat-developer-hub-and-explore-templating-basics/resource/resources:install-red-hat-developer-hub-developer-sandbox).  
**!!! Don't forget to change the Root Schema > global > instance URL, at the end of the exercise., at the time of writing, this was 'apps.rm1.0a51.p1.openshiftapps.com' for me.**
![](images/global_root_domain.png)
* In case you want to troubleshoot the installation, you can follow the instructions in
[this training exercise](https://developers.redhat.com/learn/deploying-and-troubleshooting-red-hat-developer-hub-openshift-practical-guide).

**!!! When you use the Helm based installation, be aware that the pods don't automatically 
restart when applying changes. Make sure to kill the Developer Hub when applying new configurations
(e.g., dynamic plugins, app config, ...). The reason for this is that the Helm chart does not monitor 
secrets or configmaps for changes, only what is done through a Helm upgrade.
If you want to avoid to delete pods to get updates pushed through, you can use the following command:**
```shell
oc rollout restart deployment/redhat-developer-hub
```

### Step 2b: Install Red Hat Developer Hub through the operator
_We will be using 'demo-project' as a project/namespace name. Feel free to change it
to what you like, but know that you will have to pay attention to the configurations
later on in this workshop: you'll need to check that the namespace is correct if you
didn't go along with 'demo-project'._

_We will be using 'developer-hub' as the name of the instance. Feel free to change it
to what you like, but know that you will have to pay attention to the configurations
later on in this workshop: you'll need to check that the name is correct if you
didn't go along with 'developer-hub'._

In order to do so, you can follow 
[this training exercise](https://developers.redhat.com/learn/deploying-and-troubleshooting-red-hat-developer-hub-openshift-practical-guide).  
(Next to installation instructions, this training exercise contains a section with troubleshooting instructions as well).