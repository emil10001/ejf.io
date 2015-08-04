{
    "slug": "containers",
    "date": "2015-05-28T00:10:00.000Z",
    "tags": ["io15","googleio"],
    "title": "Containers to back your mobile app",
    "publishdate": "2015-05-28T00:10:00.000Z"
}

Containers to back your mobile app
=====

> Brian Dorsey | +BrianDorsey | @briandorsey

How to run backend systems for mobile apps.

Why have a mobile backend at all? It enables you to do things that you couldn't do without a backend.

It's a pain.

VM - Cluster -... <insert photo>

### Containers

Nice way to wrap up Linux binaries, and make it portable. So it doesn't just "work on my machine". Runs everywhere.

### Kubernetes

Manage applications, not machines.

Written mostly in Go, initial effort by Google. Open Source.

Helps making deployments, testing, and management more sane.

Uses a yaml config file, specifies number of instances, basic information on how to make copies, and image that we're using. Sounds like a way to automate mini versions of AMIs.

     # launch using nautilus.yaml config
     kubectl create -f nautilus.yaml

     # resize to 6 instances
     kubectl resize rc update-demo-nautilus --replicas=6

     # upgrading the fleet is very simple
     kubectl rolling-update ...?

Wondering about what happens when you have long-running processes, that require some specific steps. This can be done by writing your own process for doing those updates. Can be written based on any criteria in your system.

* Labels
  * Arbitrary metadata
  * key-value pair
  * generally represent identity
* Pods
  * Small group of containers
  * Tightly coupled same node
  * atom of cluster scheduling and placement
  * shared namespace (IP and localhost)
  * ephemeral
* Replication / Control loops
  * Drive current state -> declared desired state
  * act independently
  * APIs
  * Observed state is truth
  * reoccurring pattern in system
* Services
  * group of identical pods
  * Load balanced traffic to healthy pods
  * Stable IP and DNS name
  * This is the discovery mechanism for Kubernetes

## Google Container Engine

Google provides this in Google Container Enginer. Approaching 1.0, but not there yet.
