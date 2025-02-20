


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

For this workshop, we’ve chosen GitHub. Note that GitHub apps have already been preconfigured for you, 
so you can skip that part in this
[training exercise](https://developers.redhat.com/learning/learn:streamline-development-github-integration-and-software-templates-red-hat-developer-hub/resource/learn:streamline-development-github-integration-and-software-templates-red-hat-developer-hub:resource:prerequisites-and-step-step-guide).