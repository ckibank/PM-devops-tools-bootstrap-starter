# DevOps essentials Tools

DevOps essentials tools: Nexus for artifact management, SonarQube for code quality, Jira for project tracking, and Jenkins for continuous integration and automation.

## To begin
- Create a Github repo without readme.md
- Create codespaces on main
```
git remote add starter https://github.com/ckechios/devops-tools-starter.git 

git fetch starter 

git merge starter/main --allow-unrelated-histories 

git remote remove starter 

```

## Docker
- run tools with docker compose up -d
- Commands for docker
```
docker ps
docker ps -a
docker compose up -d   # run in detached mode
docker compose down
docker logs [containerid]
docker exec -it [containerid] bash
```

## Jira
- Create project
- Show Epic, Story, Task
- Show Apps > Zephyr

## Maven
- Maven commands
```
mvn clean install
mvn clean package
mvn deploy package   # For Nexus deployment

```

## Nexus
- Port is 8081 : Make port public
- To get nexus password
  - > docker exec -it [nexus-conatiner-id] cat [get filename from login screen]
- Direct uplaod using mvn update
- Set username and pass for nexus repo
- This should match <distributionManagement> <repository> <id> in POM
> code ~/.maven/current/conf/settings.xml
with code below
```
<!-- between the <servers> tag -->
<server>
  <id>nexus-snapshots</id>
  <username>admin</username>
  <password>pass</password>
</server>

<server>
  <id>nexus-releases</id>
  <username>admin</username>
  <password>pass</password>
</server>
```

## scanning with sonar
- Port is 9000 : Make port Public
- initial login credentials, admin : pass
```
export SONAR_TOKEN=YOUR_SONAR_TOKEN
#sonar_token set in env like below

export SONAR_TOKEN=squ_[lotsofnumbers]

mvn clean verify sonar:sonar -Dsonar.host.url=https://sonarqube_url-9000.app.github.dev -Dsonar.coverage.exclusions=**/*
```

## Jenkins
- initial password 
  - > docker logs [jenkins container id]
- Plugins
  - nexus plugin :
    - Nexus Artifact Uploader Version 2.14
    - Pipeline Utility Steps
  - SonarQube Scanner
  - JIRA (just Jira plugin - need to restart server - need to restart docker)

## Jira 
- Test connection
```
  curl -X GET -u <email>:<token> -H "Content-Type: application/json" -H "Accept: application/json" http://[yourcloud].atlassian.net/rest/api/2/issue/[ISSUE-NUM]
```