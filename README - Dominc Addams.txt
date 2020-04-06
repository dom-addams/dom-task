README - Dominc Addams

Prerequisites:
- Docker
- Minikube 
- Kubectl
- Chrome
- Homebrew 
- Terminal / CMD
- Files downloaded

Initial Setup:
- Ensure Docker for Desktop is running 
    o (Tested by opening Terminal and typing docker)
- Start Minikube
    o Open a Terminal, run minikube start and minikube dashboard
- Open new Terminal and navigate to folder where files are
- Install kubectl with homebrew
    o Command = brew install kubectl

Main Setup:
- Run kubectl create -f masterpod.yml
- Run kubectl describe pods
- Run kubectl create -f services.yml
- Run kubectl create -f user.yml
- Run kubectl create clusterrolebinding jenkins --clusterrole cluster-admin --serviceaccount=default:jenkins

- Open browser to minikube dashboard and check Overview to view running resources
- Open a notepad or somewhere to store notes

- Run kubectl get services and copy the second port for NodePort into notepad
    o (note = this port is after 8080 and usually starts with 3)

- Run minikube ip and copy IP to notepad
- Run kubectl config view and copy server URL to notepad 

- Run kubectl get pods 
- Run kubectl describe pods against POD name from previous step
- Find IP in output and copy it to notepad


- In the browser, type http:// followed by the Minikube IP and the copied Service Port. (I.e. http://172.17.150.89:31286 which will take you to Jenkins dashboard)

- On the Jenkins dashboard, click Mange Jenkins > Configure System > Set # of executors to 0 > (scroll to bottom of page, click apply) > Click link to Cloud Configuration.

- On Configure Clouds, click Add new cloud and choose kubernetes from dropdown.

- Click on Kubernetes Cloud Details
    o For Kubernetes URL paste the URL copied from kubectl config view
    o For Namespace, type default
    o For Jenkins URL, paste the POD IP followed by :8080 
    o Click Test Connection (message “connection test successful” should appear)

- Click on Pod Template > Click Add Pod Template > Click Pod Template Details
    o For Name and Label, type jenkins-slave
    o Click Add Container > Container Template
         For Container Name, type jenkins-slave1
         For Docker image, type jenkinsci/jnlp-slave
         Change Working Directory to just /home/jenkins
         Leave the rest and click Save

- Click New Item in the top left. Call it Test1 and select Freestyle project > Click OK
    o Untick “Restrict where this project can be run”
    o In build section, click Add build step > Execute shell
    o Type the command sleep 30
    o Click Save
    o Click Back to Dashboard

- Click New Item again. Call it Test2 and Select Freestyle project
    o In Copy From field, type Test1 > Click OK > Click Save

- Click the clock icon for both Tasks to start running a job
- Go back to Minikube Dashboard > Overview 
    o Check new Pod is created
    o Go back to Jenkins Dashboard to confirm Job is running
    o Once job has finished, return to Minikube Dashboard and check Pod has been deleted.
