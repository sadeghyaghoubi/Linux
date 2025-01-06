```
mkdir nexus
cd nexus
```
```
vim docker-compose.yml
```
```
version: "3.9"

services:
  nexus:
    image: sonatype/nexus3
    ports:
    - "8081:8081"
    - "8082:8082"
    volumes:
    - ./host-nexus-data:/nexus-data
```
mkdir host-nexus-data

docker compose up
