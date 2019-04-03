# mta_cfs_boost
Multi-Target Application for Cloud Foundry Summit Boost Demo

Based on this blog post.
[Boost your Cloud Foundry Python Edit-Build-Test Speed!](https://blogs.sap.com/2018/11/05/boost-your-cloud-foundry-python-edit-build-test-speed/)
[Boost your Cloud Foundry NodeJS Edit-Build-Test Speed!](https://blogs.sap.com/2018/02/20/boost-your-cloudfoundry-nodejs-edit-build-test-speed/)


Prepare the python dependencies.
```
cd ..
wget -c http://thedrop.sap-a-team.com/files/XS_PYTHON00_1-70003433.ZIP
unzip XS_PYTHON00_1-70003433.ZIP -d sap_dependencies
cd mta_cfs_boost/python
mkdir -p vendor
pip download -d vendor -r requirements.txt --find-links ../../sap_dependencies
cd ..
```
## Root cause of the issue

Assemble and Deploy the MTA.
```
cf t -o A-Team_cfs -s dev
mkdir -p target
tools/set_nodejs_env
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
cf ssh-code ; ssh -p 2222 cf:ece6713d-3637-4709-a92a-2f3894a769a1/0@ssh.cf.us10.hana.ondemand.com
mkdir -p tmp
cf app boost-python --guid > tmp/guid

cf logs boost-python

cd python
vi server.py
cf ssh-code > ../tmp/code ; ../tools/cp2cf server.py
cd ..
```
On your local workstation, listen to 5678
```
nc -l 5678
```
CF SSH into your app container and connect as a client.
```
nc localhost 5678
```


```
https://cfs-python.cfapps.us10.hana.ondemand.com/python/set_env?PATHS_FROM_ECLIPSE_TO_PYTHON=[["/Users/i830671/git/mta_cfs_boost/python","/home/vcap/app"]]
https://cfs-python.cfapps.us10.hana.ondemand.com/python/env
cf ssh-code ; ssh -nNT -p 2222 cf:ece6713d-3637-4709-a92a-2f3894a769a1/0@ssh.cf.us10.hana.ondemand.com -R 5678:localhost:5678
```

Set breakpoint at line 158

```
tools/timed "cf undeploy mta_cfs_boost --delete-services -f"
```
