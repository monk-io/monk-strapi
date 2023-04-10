# Monk & Strapi

This repository contains Monk.io template to deploy Strapi system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Start

Set up Monk - [https://docs.monk.io/docs/monk-in-10/](https://docs.monk.io/docs/monk-in-10/)

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk Strapi repository

In order to load templates and change configuration simply use below commands:

```bash
git clone https://github.com/monk-io/monk-strapi

# and change directory to the monk-strapi template folder
cd monk-strapi

```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `strapi/variables` section

```yaml
  variables:
    strapi-image-tag: 3.6.8
    strapi-port: 1337
```

## Template variables

| Variable        | Description                                  | Type   | Example |
| --------------- | -------------------------------------------- | ------ | ------- |
| **strapi-port** | Elasticsearch port that will accept requests | int    | 1337    |
| **port**        | Strapi image version.                        | string | 3.6.8   |

## Local Deployment

| First clone the repository simply run below command after launching `monkd`: |
| :--------------------------------------------------------------------------: |

```bash
➜  monk load MANIFEST

✔ Read files successfully
✔ Loaded strapi.yaml successfully

Loaded 2 runnables, 0 process groups, 0 services, 0 entities and 0 entity instances
✨ Loaded:
 └─🔩 Runnables:
    ├─🧩 strapi/base
    └─🧩 strapi/strapi
✔ All templates loaded successfully

➜  monk list -l strapi

✔ Got the list
Type      Template       Repository  Version  Tags
runnable  strapi/base    local       -        Strapi CMS, Content Management System, Node.js, Headless CMS, API-driven CMS, Web Development, Open Source, Backend Development, GraphQL, REST API, Database, User Authentication, Customizable, Scalable, Modular
runnable  strapi/strapi  local       -        Strapi CMS, Content Management System, Node.js, Headless CMS, API-driven CMS, Web Development, Open Source, Backend Development, GraphQL, REST API, Database, User Authentication, Customizable, Scalable, Modular


➜  monk run --local-only strapi/stack

✔ Started local/strapi/stack

```

This will start the entire strapi/stack.

## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name strapistack
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p gcp -f <path/to/your-key.json>
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Cloud provider gcp
? Name of the new instance strapi-instance
? Tags (split by whitespace) strapistack
? Region europe-central2
? Zone europe-central2-a
? Instance type e2-medium
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for gcp is HDD (pd-standard). Would you like to change it? No
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on gcp... DONE
✔ Creating node: my-instance-1 DONE
✔ Initializing node: my-instance-1 DONE
✔ Creating node: my-instance-2 DONE
✔ Initializing node: my-instance-2 DONE
✔ Creating node: my-instance-3 DONE
✔ Initializing node: my-instance-3 DONE
✔ Connecting: my-instance-1 DONE
✔ Syncing peer: my-instance-1 DONE
✔ Connecting: my-instance-2 DONE
✔ Connecting: my-instance-3 DONE
✔ Syncing peer: my-instance-2 DONE
✔ Syncing peer: my-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 strapi/strapi
 ├─🔗 Process groups:
 │  └─🧩 strapi/stack
 └─⚙️ Entity instances:
    └─🧩 strapi/strapi/metadata
✔ All templates loaded successfully

➜  monk list -l strapi

✔ Got the list
Type      Template       Repository  Version  Tags
group     strapi/stack   local       -        -
runnable  strapi/strapi  local       -        self hosted, Content Management System


➜  monk run  strapi/stack

✔ Started local/strapi/stack


```

## Logs & Shell

```bash
# show Elasticsearch logs
➜  monk logs -l 1000 -f strapi/strapi

# access shell in the container running Elasticsearch
➜  monk shell strapi/strapi
```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge  --ii --rv --rs --no-confirm --rv --r strapi/strapi strapi/stack

✔ strapi/strapi purged
✔ strapi/stack purged
```
