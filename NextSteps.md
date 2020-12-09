# Next Steps

1. Look at every microservice
2. Determine which ones have cloud dependences hardcoded (meaning doesn't support Azure if there is a a cloud dependency)
3. Create a backlog of changes we need to make to each microservices to support Azure

We might have to have multiple tracks:
* Make changes to microservices to support Azure 
  * Do we maintain in a fork or PR to upstream ASAP and integrate? (**Risk**: PR velocity to Gen3 codebase)
  * We need to maintain CI/CD for any new changes to support Azure (build pipeline for each microservice we modify)
  * We would need to update or create new E2E tests for each microservice we update
  * Support for Secrets Store CSI Driver?
* Create or update an E2E test to integrate the Gen3 stack
  * Need to see what E2E tests exist in `cloud-automation` repo.
* Need to create Terraform definition for Azure
  * Need to understand the customer's posture here
  * Support dev, test, prod environments
  * Need to be security aware (get feedback from CSE Security folks)
  * Automation and rollback/disaster recovery solutions 

The above is all before we even address the customers concerns of using Gen3.

## Phases
* Updating Gen3 Stack for Azure (Code changes to get Gen3 to run on Azure, Automated testing)
* Operationalizing Gen3 Stack on Azure (Automated deployment and E2E test, monitoring/alerting, addresses baseline security concerns, operational guides)
* Validation (Rush scenarios with public and anonymous data, Rush upskill on using Gen3 as a customer)
* Sharing (Where do the Gen3 Azure changes land? Work with Gen3 community)