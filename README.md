# mta_cfs_boost
Multi-Target Application for Cloud Foundry Summit Boost Demo

```
wget -c http://thedrop.sap-a-team.com/files/XS_PYTHON00_1-70003433.ZIP
unzip XS_PYTHON00_1-70003433.ZIP -d sap_dependencies

mkdir -p vendor
pip download -d vendor -r requirements.txt --find-links ../../sap_dependencies
```

```
tools/create_uaa A-Team_cfs dev

cf t -o A-Team_cfs -s dev
mkdir -p target
tools/set_nodejs_env
mta --build-target CF --mtar target/mta_cfs_boost-CF.mtar build
tools/timed "mta --build-target CF --mtar target/mta_cfs_boost-CF.mtar build"
tools/timed "cf deploy target/mta_cfs_boost-CF.mtar"
```

