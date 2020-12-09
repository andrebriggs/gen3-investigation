# Indexd

https://github.com/uc-cdis/indexd

> Indexd is a hash-based data indexing and tracking service providing 128-bit globally unique identifiers. It is designed to be accessed via a REST-like API or via a client, such as the reference client implementation. It supports distributed resolution with a central resolver talking to other Indexd servers.

> Indexd serves as an abstraction over the physical data locations, providing a Globally Unique Identifier (GUID) per datum. These identifiers will always be resolvable with Indexd and will always provide the locations of the physical data, even if the data moves.

> Data GUIDs were created by the Data Biosphere - a vibrant ecosystem for biomedical research containing community-driven standard-based open-source modular and interoperable components that can be assembled into diverse data environments.

> GUIDs provide a domain-neutral, persistent and scalable way to track data across platforms. Indexd is a proven solution to provide GUIDs for data.

<!-- ![image](https://github.com/uc-cdis/indexd/raw/master/docs/indexd_client_upload.png) -->

## Dependencies

* PostgreSQL DB
* [ssjdispatcher](https://github.com/uc-cdis/ssjdispatcher) <-- AWS specific queue monitor
* [indexs3client](https://github.com/uc-cdis/indexs3client) <-- AWS S3 index jobs dispatched by the `ssjdispatcher`
* Python


## Notes
* Doesn't seem to have any cloud specific dependencies
* Has a client named `indexclient`
* Can access indexd API via basic auth (username/password) or using `fence`/`arborist`
* Used by `sheepdog`
  * *"When data files are submitted to a Gen3 Data Commons using Sheepdog, the files are automatically indexed into Indexd. Submissions to Sheepdog can include object_id's that map to existing Indexd GUIDs. Or, if there are no existing records, Sheepdog can create them on the fly."*