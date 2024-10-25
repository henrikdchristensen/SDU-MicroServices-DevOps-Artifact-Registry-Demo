# Readme

## GitHub Packages demo
### Requirements
1. GitHub account.

### Demo Steps
1. Show GitHub project and Dockerfile.
2. Run GitHub Action https://github.com/henrikdchristensen/SDU-MicroServices-DevOps-Artifact-Registry-Demo/actions/workflows/ghcr.yaml to see a new package is generated in GitHub.

## JFrog Artifactory demo
### Requirements
1. A JFrog account (free 14-days trial)
2. A GitHub account
3. pip installed on your computer

### Initial Steps
1. Login to JFrog online platform. In my case https://hench13.jfrog.io/ui/login/
2. Using the _Get Started Wizard_ under "/get_started", in my case https://hench13.jfrog.io/ui/get_started, to create a Docker repo inside JFrog, the steps are:
   1. Click on _Create your first repository_ and select a Docker repo, which creates both a local, remote and a virtual repo.
   2. After that, don't close the wizard and continue to _Connect your CI tool_. Use the template project suggested on the first wizard page and create a GitHub repo and follow the instructions. This is mainly to create a GitHub Action that builds a Docker image and pushes it to JFrog and for setting the correct secrets in GitHub.
   3. Wait until build is finished in the GitHub repo.
   4. You should now see the build inside JFrog under "/builds", in my case https://hench13.jfrog.io/ui/builds/?after=1719784800000&type=builds.
   5. Setup a _PyPi_ remote client (https://jfrog.com/help/r/jfrog-artifactory-documentation/pypi-repositories), from which you can pull packages through JFrog.
      1. Add the `pip.ini` config to your computer, e.g. under C:\Users\henri\miniconda3. An example of the file called `pip_jfrog.ini` is located in this repo - content be pasted into the .ini file.
         - For Unix users note that `.conf` is used instead of `.ini` file.
         - List config paths with: `pip config -v list`
      2. Check that the remote repo setting is set by running `pip config -v list` again.
   6. Activate conda environment, e.g. `conda activate devops`
   7. Pull for example: `pip install hello-package-pru`
   8. Now the package can be downloaded through Jfrog. Try to see under Artifactory->Packages->hello-package-pru->1.0.1

### Demo Steps
1. Show JFrog platform.
2. Open terminal and activate conda `devops` environment.
3. Show ini file in C:\Users\henri\miniconda3
4. Show `pip config -v list`.
5. Make sure no pip package is located in JFrog.
6. Pull package `pip install hello-package-pru` to see a new package is cached in JFrog.
7. Show GitHub repo and run GitHub Action https://github.com/henrikdchristensen/SDU-MicroServices-DevOps-Artifact-Registry-Demo/actions/workflows/jfrog.yml to see a new Docker build is created in JFrog.
8. Show vulnerability in JFrog: https://hench13.jfrog.io/ui/scans-list/repositories/sdu-demo-docker-local/scan-descendants/jfrog-docker-example-image%2F9?type=packages&version=9&package_id=docker%3A%2F%2Fjfrog-docker-example-image&path=sdu-demo-docker-local%2Fjfrog-docker-example-image%2F9%2Fmanifest.json&page_type=security-vulnerabilities&severity=high

## Nexus Repository Manager demo
### Requirements
1. Docker Desktop installed on your computer.
2. pip installed on your computer.

### Initial Steps
1. Download the docker image _Nexus Repository 3_:
    ```
    docker pull sonatype/nexus3
    ```
2. Run the docker image with the following command:
   ```
   docker run -d -p 8081:8081 --name nexus sonatype/nexus3
   ```
3. Make sure the Nexus Repository Manager is running by visiting http://localhost:8081 (may need to manually type the address in the address field).
4. Copy the admin password from the container volume, the file is called `admin.password`, double-click on the file to open it in Docker Desktop.
5. Login to Nexus Repository Manager via http://localhost:8081 with the admin password.
6. You should now be able to access both the _Application_ and the _Administration_ panel.
7. Create pypi (proxy) repository under _Repositories_ -> _Create repository_ -> _PyPi (proxy)_, and give the repo a _Name_ and define a _Remote Storage URL_, e.g. https://pypi.org/.
8. Add config file on your local PC like in the JFrog example. An example of the file called `pip_nexus.ini` is located in this repo - content be pasted into the .ini file.
9. Test you can download by running the command `pip install hello-package-pru`.
10. You should now see the package in Nexus Repository Manager under _Browse_.

### Demo Steps
1. Run the Docker container _nexus_.
2. Login to Nexus Repository Manager via http://localhost:8081.
   - user: admin
   - password: 94b79bb3-b5de-4a7f-9922-1f26e6d904d0
3. Make sure no pip package is located in Nexus.
4. Pull package `pip install hello-package-pru` to see a new package is cached in Nexus.
