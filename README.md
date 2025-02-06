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

9. ***Whenever you want to use the dev spaces with the default devfile (i.e., not the universal one), make sure to enable 
the oc command:**   
Install openshift cli:    
_These commands can be executed from a terminal. In order to access a new terminal, click the hamburger icon in dev spaces,
terminal > new terminal. When prompted, you can choose the workshop folder as root (i.e., platform-engineering-101)._  
```shell
curl -o oc.tar https://downloads-openshift-console.apps.rm1.0a51.p1.openshiftapps.com/amd64/linux/oc.tar
```
```shell
tar -xvf oc.tar
```
```shell
alias oc="$(pwd)/oc"
```

***Next to that, clone the exercise manifest files into this repository too (i.e., will be used later on in this workshop):***  
```shell
mkdir training-exercises
cd training-exercises
git clone https://github.com/maarten-vandeperre/developer-hub-training-exercises.git
```

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
Install openshift cli:    
_These commands can be executed from a terminal. In order to access a new terminal, click the hamburger icon in dev spaces,
terminal > new terminal. When prompted, you can choose the workshop folder as root (i.e., platform-engineering-101)._  
```shell
curl -o oc.tar https://downloads-openshift-console.apps.rm1.0a51.p1.openshiftapps.com/amd64/linux/oc.tar
```
```shell
tar -xvf oc.tar
```
```shell
alias oc="$(pwd)/oc"
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
_Can be, that as admin, you need to:_
* _Check user permissions to create image streams - if you use another namespace than e.g., user13._
  * _oc auth can-i create imagestreams -n user13-devspaces --as=user13_
  * _oc auth can-i create imagestreams -n user13-devspaces --as=user13_


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

**Example:**
```yaml
app-config.yaml: |
  signInPage: github
  app:
    title: My Red Hat Developer Hub Instance
    baseUrl: ${baseUrl}  # base url is coming from the 'secrets_rdhd-secret.yaml' config in 'setting-up-developer-hub-through-the-operator'
    branding:
      fullLogo: data:image/jpeg;base64,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
      fullLogoWidth: "110px"
      theme:
        light:
          primaryColor: "#444444"
          headerColor1: "#ffffff"
          headerColor2: "#555555"
          navigationIndicatorColor: "#444444"
        dark:
          primaryColor: "#444444"
          headerColor1: "#ffffff"
          headerColor2: "#555555"
          navigationIndicatorColor: "#444444"
``` 

For image: images/jfokus_50x47.jpeg, encoded by https://www.base64-image.de/

## Step 7: Extra information
* Example repository: https://github.com/maarten-vandeperre/developer-hub-documentation
