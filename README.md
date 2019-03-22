# mta_cfs_boost
Multi-Target Application for Cloud Foundry Summit Boost Demo

```
cf t -o A-Team_cfs -s dev
mkdir -p target
tools/set_nodejs_env
mta --build-target CF --mtar target/mta_cfs_boost-CF.mtar build
tools/timed "mta --build-target CF --mtar target/mta_cfs_boost-CF.mtar build"
tools/timed "cf deploy target/mta_cfs_boost-CF.mtar"
```

