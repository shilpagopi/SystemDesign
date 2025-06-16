# Blob Storage
Blob - Binary Large Objects
Why do we need blob storages?
* Big files -> slow queries (Eg. POSTGres packs db rows into 8kb pages)
* Mostly static content
* Need to maintain replicas
* DB storage is ~10x expensive than blob storage

### Diagram
<img width="1167" alt="image" src="https://github.com/user-attachments/assets/f3465308-da64-4304-89eb-510890c97a31" />
