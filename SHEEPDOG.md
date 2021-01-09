# SHEEPDOG 

https://github.com/uc-cdis/sheepdog
https://uc-cdis.github.io/sheepdog/build/html/index.html

> Sheepdog (submission API) is intended to be a lightweight RESTful translation between the datamodel and the submitter. Using following methods, submitters can create, delete, and update entities and relationships in the datamodel.

> Flask blueprint for herding data submissions

## Dependencies
* Python/Flask
* PostgreSQL
* AWS S3
* [indexclient](github.com/uc-cdis/indexclient) library
* [storage-client](https://github.com/uc-cdis/storage-client) library (Connects to S3 via [boto](https://github.com/boto/boto))


## Notes 
* Has hard coded S3 references
* The data model refers to Gen3 model and data dictionary

## Swagger API
* http://petstore.swagger.io/?url=https://raw.githubusercontent.com/uc-cdis/sheepdog/master/openapi/swagger.yml

On localhost you can call things like:
* **GET** all entities https://localhost/api/v0/submission/_dictionary/_all
* **GET** all slides https://localhost/api/v0/submission/_dictionary/slide
