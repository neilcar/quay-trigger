# quay-trigger

This is intended as a sample for using Quay event notifications to trigger an ACS scan of an image pushed to a repo.

## setup

First, add the ClusterTasks from the [Tekton CI sample](https://github.com/stackrox/contributions/tree/main/ci/Tekton) and follow the instructions to create the secrets for ROX_API_TOKEN and ROX_CENTRAL_ENDPOINT.

Then, create a project for this (`oc create ns quay-trigger`) and switch to it (`oc project quay-trigger`).  `oc apply -f Tekton/` to create the EventListener, TriggerTemplate, and TriggerBinding.  Additionally, this creates a Route for the EventListener.

Next, get the route `oc get route -n quay-trigger`.  Create a new [repository notication](https://access.redhat.com/documentation/en-us/red_hat_quay/3/html/use_red_hat_quay/repository_notifications) for Repository Push events with a Webhook POST action.  Use the Route created in the previous step for the destination.

Finally, test an image push to the repository.  You should see a new TaskRun happen in the `quay-trigger` project.