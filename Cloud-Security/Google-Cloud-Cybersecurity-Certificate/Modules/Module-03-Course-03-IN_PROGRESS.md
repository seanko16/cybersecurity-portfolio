so far I've learned about TTL policies.
TTL policies let us put an expiration time on a data asset.
NOTE: the data asset becomes inaccessible after the TTL expires, but google notes that its easy to delete as the course described in the documentation.
they are created/deleted under the "Firestore" section in the google cloud console.


key words learned:
- Ephemerality - it means that resources that have been created aren't meant to last forever and are easily replacable.
why is it important? -> 
1. Its main security benefit is its temporary existence, which reduces the risk of data leaks and attacks.
2. it allows system resources to be easily replaced.
- Immutability -it means that after and object (container,Image and etc.) have been created it cannot be changed.
instead we need to create a new image with the updated/changed versions and deploy it.
why is it important? -> 
1. it makes sure that every duplicate of the object is exactly as it runs, therefore saving the configuration drift problems
2. attackers can't changed the running object and plant backdoors or cause missconfigurations because it's Immutable
3. better for CI/CD pipelines ensuring every release run on the same exact enviroment.
