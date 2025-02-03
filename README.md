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
Workshop home page: [https://catalog.demo.redhat.com/workshop/y49rsd](https://catalog.demo.redhat.com/workshop/y49rsd).  

Installed operators:
* Red Hat Developer Hub
* Red Hat OpenShift GitOps
* Red Hat OpenShift Dev Spaces 
  * Installation of a dev spaces cluster can take +- 5 min.
  * (Use this GitHub repo as workspace Git repo URL: https://github.com/grace-maarten/platform-engineering-101.git). This can take up to +- 2 minutes as well.

***Whenever you want to use the dev spaces with the default devfile (i.e., not the universal one), make sure to enable 
the oc command:**  
install openshift cli:  
```shell
curl -o oc.tar https://downloads-openshift-console.apps.rm1.0a51.p1.openshiftapps.com/amd64/linux/oc.tar
```
```shell
tar -xvf oc.tar
```

***Next to that, clone the exercise manifest files into this repository too (i.e., will be used later on in this workshop):***  
```shell
mkdir training-exercises
cd training-exercises
git clone https://github.com/maarten-vandeperre/developer-hub-training-exercises.git
```

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

### Step 2a: Install Red Hat Developer Hub through Helm Charts
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

_Source manifest files for the tutorials can be found in this repository: 
[https://github.com/maarten-vandeperre/developer-hub-training-exercises](https://github.com/maarten-vandeperre/developer-hub-training-exercises),
which can be cloned in your dev spaces environment. Be aware to change the default namespace 'demo-project'
within these manifest files to your namespace._

In order to do so, you can follow 
[this training exercise](https://developers.redhat.com/learn/deploying-and-troubleshooting-red-hat-developer-hub-openshift-practical-guide).  
(Next to installation instructions, this training exercise contains a section with troubleshooting instructions as well).

## Step 3: Integrate with GitHub
_Source manifest files for the tutorials can be found in this repository:
[https://github.com/maarten-vandeperre/developer-hub-training-exercises](https://github.com/maarten-vandeperre/developer-hub-training-exercises),
which can be cloned in your dev spaces environment. Be aware to change the default namespace 'demo-project'
within these manifest files to your namespace.   
!! In case you went for the Helm based installation,
make sure to use the '-helm' manifest files._

Before diving into tasks like setting up software templates, we first need to establish integration with a Git repository. 
For this workshop, we’ve chosen GitHub. Note that GitHub apps have already been preconfigured for you, 
so you can skip that part in this
[training exercise](https://developers.redhat.com/learning/learn:streamline-development-github-integration-and-software-templates-red-hat-developer-hub/resource/learn:streamline-development-github-integration-and-software-templates-red-hat-developer-hub:resource:prerequisites-and-step-step-guide).

In case you would like to continue with your own GitHub application, feel free to obtain the following
parameters from you GitHub organization and/or GitHub app:  
_How to obtain these parameters is described in section '1. Configure GitHub'_
* app ID.
* client ID.
* client secret.
* private key (i.e., .pem file).  

Next to the written tutorial, we've created a [dynamic video tutorial](https://app.arcade.software/share/yAz2okhKSeBNCRrqmQ39),
which you can follow as well to obtain these parameters. (Whenever the video pauzes, you have to 
click 'next' or 'arrow right' to continue).


Starting from the second part of 
[the exercise](https://developers.redhat.com/learning/learn:streamline-development-github-integration-and-software-templates-red-hat-developer-hub/resource/learn:streamline-development-github-integration-and-software-templates-red-hat-developer-hub:resource:prerequisites-and-step-step-guide), 
focus on the following steps:   
_(you can skip other configuration steps, as tasks like software template creation will be covered later in the workshop)_
* '**2. Create a basic GitHub integration within Developer Hub** (i.e., repository creation and scanning)'
* '**3.3 Enable GitHub authentication**'
* [optional] '**3.4 Enable GitHub actions**' _(i.e., can be used to visualize the GitHub actions from Step 4)._

## Step 4: Applying software templates / golden path templates
_Source manifest files for the tutorials can be found in this repository:
[https://github.com/maarten-vandeperre/developer-hub-training-exercises](https://github.com/maarten-vandeperre/developer-hub-training-exercises),
which can be cloned in your dev spaces environment. Be aware to change the default namespace 'demo-project'
within these manifest files to your namespace.   
!! In case you went for the Helm based installation,
make sure to use the '-helm' manifest files._

For this exercise, we will make use of the following software template:
* For Operator-based installation: [Open Liberty software template](https://github.com/grace-maarten/platform-engineering-101/blob/main/artefacts/software-templates/liberty-template/template.yaml)
* For Helm-based installation: [Helm Open Liberty software template](https://github.com/grace-maarten/platform-engineering-101/blob/main/artefacts/software-templates/liberty-template/template-helm.yaml)

**!! In order to be able to initiate a software template, you'll need to make sure that 
you have a personal access token from GitHub:**
1. Go to [GitHub profile settings](https://github.com/settings/profile).
2. Got to Developer settings > Personal access tokens > Tokens (classic).
3. Click 'Generate new token' > Generate new token (classic).
4. Add these scopes to the token:
   * **Reading software components:**
     * repo 
   * **Reading organization data:**
     * read:org
     * read:user
     * user:email
   * **Publishing software templates:**
     * repo 
     * workflow (if templates include GitHub workflows)
5. Store it somewhere: you'll need it later on when initiating a software template (i.e., within the form of the software template initiation).

Instructions on how to add software templates to Developer Hub and how to apply them,
can be found in this
[training exercise](https://developers.redhat.com/learning/learn:streamline-development-github-integration-and-software-templates-red-hat-developer-hub/resource/learn:streamline-development-github-integration-and-software-templates-red-hat-developer-hub:resource:prerequisites-and-step-step-guide).
(Section '3.1 Create a repository via a software template').
**!!! Don't forget to use the**
**[Open Liberty software template](https://github.com/grace-maarten/platform-engineering-101/blob/main/artefacts/software-templates/liberty-template/template.yaml)**
**or**
**[Helm Open Liberty software template](https://github.com/grace-maarten/platform-engineering-101/blob/main/artefacts/software-templates/liberty-template/template-helm.yaml)**
**instead of the one mentioned in the exercise.**

In case you run into permission issues: for that repository into your own organization.  
E.g., https://github.com/workshop-devhub/platform-engineering-101/blob/main/artefacts/software-templates/liberty-template/template-helm.yaml
  
When this templates prompt you for a namespace in the second tab, it means the organization within GitHub
  
  
When the template is initiated, go to the GitHub organization's repositories, 
and you'll see that an application repository and a GitOps repository is created.
Go to the application repository and check out the GitHub actions. You can use 
docker build step to fetch the resulting docker image in case you don't have 
ArgoCD enabled. Take this image, go to its details in GitHub and make it public.

Now go to the OpenShift console and create a new application with this docker image.

## Step 5: Custom (dynamic) plugins
_Can be, that as admin, you need to:
* Check user permissions to create image streams.
  * oc auth can-i create imagestreams -n user13-devspaces --as=user13
  * oc auth can-i create imagestreams -n user13-devspaces --as=user13
_

### Environment setup
First of all, execute the following commands to prepare the environment (e.g., yarn, npm configs, ...).  
```shell
mkdir -p ~/.npm-global/lib
npm config set prefix ~/.npm-global
```

```shell
export PATH=~/.npm-global/bin:$PATH
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

```shell
npm install -g yarn
```

### Plugin development
Now follow the instructions from [this training exercise](https://github.com/maarten-vandeperre/developer-hub-training-exercises/tree/main/custom-dynamic-plugins)


## Step 6: Customizing the Developer Hub instance
Documentation pages:
* https://docs.redhat.com/en/documentation/red_hat_developer_hub/1.4/html/customizing/index
Examples:
* https://github.com/redhat-developer/rhdh/blob/main/app-config.yaml
* https://github.com/redhat-developer/rhdh/blob/main/docker/Dockerfile#L278


## Step 7:
Extra information:  
* Example repository: https://github.com/maarten-vandeperre/developer-hub-documentation



