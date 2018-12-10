# A sample repo with example of nomad initial configuration

### Prerequisites

- vagrant
- git
- virtualbox

## How to use

### Start lab

```
git clone https://github.com/achuchulev/nomad-startup.git
cd nomad-startup
vagrant up
```

Vagrant up will run `scripts/provision.sh` that will install:

- Docker
- Nomad
- Consul

### Verify installaton

SSH to vagrant box and you should see nomad help output 

```
vagrant ssh
nomad
```

## Run a single Nomad agent in development mode

This mode is used to quickly start an agent that is acting as a client and server to test job configurations or prototype interactions. It should not be used in production as it does not persist state.

```
sudo nomad agent -dev
```

### List Nomad cluster nodes

Open new SSH session to vagrant box and see the registered nodes of the Nomad cluster and view the Nomad members

```
vagrant ssh
nomad node status
nomad server members
```

### Create a sample Nomad Job

```
nomad job init
```

which generates a skeleton job file named `which generates a skeleton job file`

### Run a Nomad Job

```
nomad job run example.nomad
```

### Inspect the status of the job

```
nomad status example
```

### Modify a Job

Edit the example.nomad file to update the count and set it to 3:

```
# The "count" parameter specifies the number of the task groups that should
# be running under this group. This value must be non-negative and defaults
# to 1.
count = 3
```
### Dry-run of the scheduler 

```
nomad job plan example.nomad
```

### Push the updated job specification

```
nomad job run example.nomad
```

### Stop a Job

```
nomad job stop example
```

### Stop Nomad Dev mode

You can use `Ctrl-C` (the interrupt signal) to halt the agent

## Setup Nomad cluster

### Start Nomad agent in server mode

```
nomad agent -config config/server.hcl
```

### Start Nomad Clients

```
sudo nomad agent -config config/client1.hcl
sudo nomad agent -config config/client2.hcl
nomad node status
```

### Submit a Job


```
nomad job run example.nomad
nomad status example
```

## Access Nomad Web UI and inspect Nomad Jobs

Go to [http://localhost:4646](http://localhost:4646)

