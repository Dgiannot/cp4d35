# Deploy Cloud Pak for Data 3.5 (CP4D) on IBM Cloud ROKS OpenShift

## Log in

1. Git clone this repository
```shell
git clone https://github.com/aroute/cp4d35.git
```

2. Log in to your OpenShift cluster.
```shell
oc login ...
```

## Pre-requisite

1. Increase the persistent storage for registry, if not done already.

2. Activate the registry.
```shell
cd cp4d35/
./registry.sh
```

3. Tune/optimize nodes 
```shell
oc create -f setkernelparams.yaml
```

4. Enable NFS file permission
```shell
oc create -f norootsquash.yaml
```

5. Get `cpd-cli` utility.
```shell
./cli.sh
```

6. Update the default repo.yaml file by inserting your entitlement key. The key is available via [Container software library on My IBM](https://myibm.ibm.com/products-services/containerlibrary). Source: [Obtaining the installation files](https://www.ibm.com/docs/en/cloud-paks/cp-data/3.5.0?topic=tasks-obtaining-installation-files).

6.1. Create a variable for your entitlement key.
```shell
export entitlement_key=eyJhbGcxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

6.2. Populate your api key into `repocopy.yaml` file.
```shell
sed -i 's/enter_api_key/'"$entitlement_key"'/g' repocopy.yaml
```

6.3. Replace `repo.yaml` file.
```shell
rm repo.yaml && mv repocopy.yaml repo.yaml
```

7. Choose and select only one of the two storage classes. If you provisioned OpenShift cluster at Technology Zone with NFS, then select `managed-nfs-storage`; otherwise, select `ibmc-file-gold-gid`. 
```shell
export storageclass=ibmc-file-gold-gid
```
OR
```shell
export storageclass=managed-nfs-storage
```

## Deploy

### Lite Assembly (base)
```shell
./00lite.sh 
```

## DB2 Warehouse
```shell
./01db2wdeploy.sh
```
⏰ 30 minutes.

### Watson Studio
```shell
./02wsldeploy.sh
```
⏰ 1 - 2 hours.

## Watson Machine Learning
```shell
./03wmlwdeploy.sh
```
⏰ 1 - 2 hours.

## EDB Operator
```shell
./04edbdeploy.sh
```
⏰ 30 minutes.

## Watson Discovery

📌 Note: Watson Discovery uses Block Gold storage.

```shell
./05discoverydeploy.sh
```
⏰ 1 - 2 hours.


