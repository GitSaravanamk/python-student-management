# Python Student Management

A small Streamlit demo application to add, view, and delete student records.

## Important limitation
This starter app stores records in Streamlit session state. Records are temporary,
session-specific, and are not stored in a database. Do not use it for real client
student records until persistent database storage, authentication, authorization,
backups, and appropriate security controls have been implemented.

## Run locally
```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m streamlit run app.py
```
Open http://localhost:8501.

## Build and run with Docker
Run these commands from this folder:
```powershell
docker build -t student-management-ui:local .
docker run --name student-ui -d -p 8501:8501 student-management-ui:local
docker ps
```
Open http://localhost:8501.
View logs: `docker logs -f student-ui`
Stop/remove: `docker rm -f student-ui`

## GitHub Actions -> Docker Hub
1. Create a Docker Hub repository named `python-student-management`.
2. Create a Docker Hub access token (Read & Write).
3. In GitHub repository Settings -> Secrets and variables -> Actions, add:
   - `DOCKERHUB_USERNAME`: your Docker Hub username
   - `DOCKERHUB_TOKEN`: the Docker Hub access token
4. Push this project to the `main` branch. The workflow at
   `.github/workflows/docker-build.yml` builds and pushes:
   - `YOUR_DOCKERHUB_USERNAME/python-student-management:latest`
   - `YOUR_DOCKERHUB_USERNAME/python-student-management:<commit-sha>`

## Run the published image on a client Windows server
Install Docker Desktop (or an appropriate Docker Engine runtime), then run:
```powershell
docker pull YOUR_DOCKERHUB_USERNAME/python-student-management:latest
docker rm -f student-ui 2>$null
docker run -d --name student-ui --restart unless-stopped -p 8501:8501 YOUR_DOCKERHUB_USERNAME/python-student-management:latest
docker ps
docker logs --tail 50 student-ui
```
Open `http://localhost:8501` on the client server itself.

To access from another computer on the same network, use
`http://CLIENT_SERVER_LAN_IP:8501`, and configure the server firewall/network
to allow that port only to approved client networks. Do not expose it directly
to the public internet without HTTPS, authentication, and security review.

To update after a new image is published:
```powershell
docker pull YOUR_DOCKERHUB_USERNAME/python-student-management:latest
docker rm -f student-ui
docker run -d --name student-ui --restart unless-stopped -p 8501:8501 YOUR_DOCKERHUB_USERNAME/python-student-management:latest
```
