# ISLE Docker Development Repo
Development repo for testing.  
NOT FOR PRODUCTION!

Docker Image GitHub Repos that comprise this stack: 
 - [`isle-tomcat`](https://github.com/Islandora-Collaboration-Group/isle-tomcat/) (base image)
 - [`isle-fedora`](https://github.com/Islandora-Collaboration-Group/isle-fedora/)
 - [`isle-solr`](https://github.com/Islandora-Collaboration-Group/isle-solr/)
 - [`isle-apache`](https://github.com/Islandora-Collaboration-Group/isle-apache/)
 - [`isle-imageservices`](https://github.com/Islandora-Collaboration-Group/isle-imageservices/)

## Requirements  
* Docker-CE or EE
* Docker-compose
* Git
* Time required < 30 minutes.
* **Windows Users**: Please open the .env and uncomment `COMPOSE_CONVERT_WINDOWS_PATHS=1`

## Quick Start
1. Please read: [ISLE Release Candidate (RC): How to Test](https://docs.google.com/document/d/1VUiI_bXo6SLqqUjmInVjBg3-cs40Vj7I_92txjFUoQg/edit#heading=h.1e4943m60lsh)
2. Clone this repo
    - `git clone https://github.com/Islandora-Collaboration-Group/ISLE-Development.git` 
3. Change directory to the cloned directory:
    - `cd ISLE-development` (by default)
4. Pull the latest images:
    - `docker-compose pull`
5. Launch the ISLE preRC stack for testing:
    - `docker-compose up -d`
6. Please wait a few moments for the stack to fully come up.  Approximately 3-5 minutes.
7. Install Islandora on the isle-apache-ld container:
    - `docker exec -it isle-apache-ld bash /utility-scripts/isle_drupal_build_tools/isle_islandora_installer.sh`
8. To wrap up testing:
    - In the folder with the docker-compose.yml `docker-compose down -v`

## Quick Cleanup 
If you have been testing the stack extensively you will want to `prune` your Docker daemon as you test.
1. In the folder with the `docker-compose.yml`
    - `docker-compose down -v`
2. If you have no other _stopped_ services that you do not want `pruned` on Docker:
    - **Note running containers are NOT pruned.**
    - `docker system prune --all`
    - answer `Y` to remove all unused volumes, images, and networks.
- OR
2. If you cannot `prune`:
    - `docker ps` and take note of any running ISLE services:
        - `docker down {list of all the running ISLE services, tab auto-complete may work}` (You may add as many containers as needed in one command.)
    - `docker image ls` and take note of all ISLE-related images:
        - `docker image rm {list of all images to be removed, tab auto-complete may work}` (Again, you may add as many as needed.)
    - `docker volume ls` and take note of all ISLE-related volumes:
        - `docker volume rm {list of all volumes to be removed, tab auto-complete may work}` ...
    - `docker network ls` and take note of all ISLE-related networks:
        - `docker network rm {list of all networks to be removed, tab auto-complete may work}` ...


## Important Notes, Ports, Pages and Usernames/Passwords
@SEE: https://github.com/Islandora-Collaboration-Group/ISLE  
[Portainer](https://portainer.io/) is a GUI for managing Docker containers. It has been built into ISLE for your convenience.  
**Windows Users**: Please open the .env and uncomment `COMPOSE_CONVERT_WINDOWS_PATHS=1`  
**Note that both HTTP and HTTPS work** Please accept the self-signed certificate for testing when using HTTPS.

### Locations, Ports:
* Make sure your /etc/hosts points isle.localdomain to 127.0.0.1. See original docs on how-to.
* Islandora is available at http://isle.localdomain
  * **You may need to point directly to the IP address of isle-apache, here's how:**
    - `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' isle-apache-ld`
    - Copy the IP and browse to it.  `http://{IP}/`
* Traefik is available at http://admin.isle.localdomain OR http://localhost:8080/
* Portainer is available at http://portainer.isle.localdomain OR http://localhost:9010/
* Fedora is available at http://isle.localdomain/fedora OR http://localhost:8081/
* Solr is available at http://isle.localdomain/solr OR http://localhost:8082/
* Image Services are available at http://images.isle.localdomain OR http://localhost:8083/

### Users and Passwords
Read as username:password

Islandora (Drupal) user and pass (default):
 * `isle`:`isle`

All Tomcat services come with the default users and passwords:
* `admin`:`isle_admin`
* `manager`:`isle_manager`

Portainer's authentication can be configured: 
* By default there is no username or password required to login to Portainer.
* [Portainer Configuration](https://portainer.readthedocs.io/en/stable/configuration.html)
