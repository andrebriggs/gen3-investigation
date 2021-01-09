# ABRORIST

https://github.com/uc-cdis/arborist

> Arborist is an attribute-based access control (ABAC) policy engine, designed for use with the Gen3 stack. Arborist tracks resources requiring access control, along with actions which users may perform to operate on these resources, and roles, which aggregate permissions to perform one or more actions. Finally, policies tie together a set of resources with a set of roles; when granted to a user, a policy grants authorization to act as one of the roles over one of the resources. Resources are arranged hierarchically like a filesystem, and access to one resource implies access to its subresources.

> For example, a simple policy might grant a role metadata-submitter on the resource /projects/project-abc. Now users which are granted this policy can perform the actions that being a metadata-submitter entails, for all resources under project-abc.

>In the Gen3 stack, arborist is integrated closely with fence. Fence acts as the central identify provider, issuing user tokens (in the form of JWTs) containing the list of policies in the arborist model which are granted to that user. Other microservices needing to check user authorization to operate on a resource can statelessly verify the user's authorization, making a request to arborist with the user's JWT and receiving a response for the authorization decision.

## Dependencies

* PostgreSQL DB
* Go


## Notes
* Doesn't seem to have any cloud specific dependencies

## Swagger API
* https://petstore.swagger.io/?url=https://raw.githubusercontent.com/uc-cdis/arborist/master/docs/openapi.yaml


Example query for `compose-services` local deployment:
`curl -k -H "accept: application/json"  -X GET "https://localhost/authz/mapping?username=REPLACE_ME"`