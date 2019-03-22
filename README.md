# mta_cfs_boost
Multi-Target Application for Cloud Foundry Summit Boost Demo

```
wget -c http://thedrop.sap-a-team.com/files/XS_PYTHON00_1-70003433.ZIP
unzip XS_PYTHON00_1-70003433.ZIP -d sap_dependencies

mkdir -p vendor
pip download -d vendor -r requirements.txt --find-links ../../sap_dependencies
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
```

