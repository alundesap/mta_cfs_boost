# mta_cfs_boost
Multi-Target Application for Cloud Foundry Summit Boost Demo

Based on this blog post.
[Boost your Cloud Foundry Python Edit-Build-Test Speed!](https://blogs.sap.com/2018/11/05/boost-your-cloud-foundry-python-edit-build-test-speed/)
[Boost your Cloud Foundry NodeJS Edit-Build-Test Speed!](https://blogs.sap.com/2018/02/20/boost-your-cloudfoundry-nodejs-edit-build-test-speed/)

```
cd ..
wget -c http://thedrop.sap-a-team.com/files/XS_PYTHON00_1-70003433.ZIP
unzip XS_PYTHON00_1-70003433.ZIP -d sap_dependencies
cd mta_cfs_boost/python
mkdir -p vendor
pip download -d vendor -r requirements.txt --find-links ../../sap_dependencies
cd ..
```

```
tools/create_uaa cf A-Team_cfs dev
cf create-service hana hdi-shared boost-cfs-hdb -c '{"database_id":"197ee647-a43c-49f5-8ea6-5561a802ed6f"}'

cf t -o A-Team_cfs -s dev
mkdir -p target
tools/set_nodejs_env
mta --build-target CF --mtar target/mta_cfs_boost-CF.mtar build
tools/timed "mta --build-target CF --mtar target/mta_cfs_boost-CF.mtar build"
tools/timed "cf deploy target/mta_cfs_boost-CF.mtar"

tools/timed "mta --build-target CF --mtar target/mta_cfs_boost-CF.mtar build ; cf deploy target/mta_cfs_boost-CF.mtar"
```

```
tools/timed "cf push boost-python -m 512M -k 512M -n cfs-python -p python/"
```

Edit in place (deployed)
```
cf enable-ssh boost-python
cf restart boost-python
cf ssh boost-python
cd app
```

Edit locally, then copy files to deployed runtime container.
```
cf app boost-python --guid
cf ssh-code
ssh -p 2222 cf:8aa9f796-e53e-4564-b4e3-597e82d7c2e7/0@ssh.cf.us10.hana.ondemand.com
```


```
tools/timed "cf undeploy mta_cfs_boost --delete-services -f"
```
