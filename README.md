Rancher QA pre-configured Jenkins backed by code (JCasC)

Custom Jenkins image with plugins pre-installed
One configmap and secret backing the installation
Docker-in-Docker agent w/ jenkins-job-builder installed
Credentials automatically loaded from AWS Secrets Manager (see secrets)
Admin token automatically configured (see secrets)
Build Jenkins Job pre-configured

---

To deploy:

* Build and push `jenkins` and `dind-agent` images
* Create namespace `jenkins`
* Adjust secrets in `resources.yaml`
  * ADMIN_TOKEN must be 32 characters - `114c13730ad0d9294e4b4042f8d873d26d`
* Deploy `resources.yaml` to your cluster in `jenkins` namespace
* Add helm repo - `https://charts.rancher.io`
* Deploy Jenkins chart with `values.yaml` - config assumes deployment name is `jenkins-qa`
* Deploy `ingress.yaml` after updating the host path
* Update Route 53 record (`JENKINS_URL`) with the LB IPs from the ingress (`k get ingress -n jenkins`)
* Go to JENKINS_URL
* Login with admin credentials (set in secret)
* Run `Build Jenkins Job` job

---

Some credentials in AWSSM are assumed to exist:
* JJB_USER (string - default user)
* JJB_PASSWORD (string - token)
* JENKINS_URL (string - Jenkins URL)
* jenkins-job-builder-github (SSH Private Key with Username)

---

TODO: Update resource limits and concurrency for k8s agents