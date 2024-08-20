## Kubernetes
[Kubernetes Overview](https://kubernetes.io/docs/concepts/overview/#why-you-need-kubernetes-and-what-can-it-do)

_________________________
### Monitoring
[Monitoring](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-usage-monitoring/)

_________________________
### Control Plane
[Control Plane Components](https://kubernetes.io/docs/concepts/overview/components/#control-plane-components)

_________________________
### Nodes
[Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/)
[Node Control Plane Communication](https://kubernetes.io/docs/concepts/architecture/control-plane-node-communication/)

_________________________
### Pods
[Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
[Pod API Reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/)

One or more containers.
_________________________
### Controllers
[Controllers](https://kubernetes.io/docs/concepts/architecture/controller/)

Kubernetes provides several built-in APIs for declarative management
of your workloads and the components of those workloads.

Ultimately, your applications run as containers inside Pods; however,
managing individual Pods would be a lot of effort. For example, if a
Pod fails, you probably want to run a new Pod to replace
it. Kubernetes can do that for you.

You use the Kubernetes API to create a workload object that represents
a higher abstraction level than a Pod, and then the Kubernetes control
plane automatically manages Pod objects on your behalf, based on the
specification for the workload object you defined.

Control loop running in the background.
#### Deployment
[Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
[Deployment API Reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/deployment-v1/)

[ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
[ReplicaSet API Reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/replica-set-v1/)

A Deployment provides declarative updates for Pods and ReplicaSets.

You describe a desired state in a Deployment, and the Deployment
Controller changes the actual state to the desired state at a
controlled rate. You can define Deployments to create new ReplicaSets,
or to remove existing Deployments and adopt all their resources with
new Deployments.

Uses **ReplicaSet** to monitor health of pods and scale up or down, also start new pods to
replace dead ones. Good for stateless applications.

Deployments replace [ReplicationController](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/).

#### StatefulSet
[StatfulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
[StatefulSet API Reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/stateful-set-v1/)

StatefulSet is the workload API object used to manage stateful applications.

Manages the deployment and scaling of a set of Pods, and provides
guarantees about the ordering and uniqueness of these Pods.

Like a Deployment, a StatefulSet manages Pods that are based on an
identical container spec. Unlike a Deployment, a StatefulSet maintains
a sticky identity for each of its Pods. These pods are created from
the same spec, but are not interchangeable: each has a persistent
identifier that it maintains across any rescheduling.

If you want to use storage volumes to provide persistence for your
workload, you can use a StatefulSet as part of the solution. Although
individual Pods in a StatefulSet are susceptible to failure, the
persistent Pod identifiers make it easier to match existing volumes to
the new Pods that replace any that have failed.

Good for stateful applications that need to start pods in a certain order, such as a pod
that needs a **PersistantVolume**.

#### DaemonSet
[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
[DaemonSet API Reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/daemon-set-v1/)

A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As
nodes are added to the cluster, Pods are added to them. As nodes are
removed from the cluster, those Pods are garbage collected. Deleting a
DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

    - running a cluster storage daemon on every node
    - running a logs collection daemon on every node
    - running a node monitoring daemon on every node

#### Job
[Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
[Job API Reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/job-v1/)
[Automatic Cleanup for Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/ttlafterfinished/)

A Job creates one or more Pods and will continue to retry execution of
the Pods until a specified number of them successfully terminate. As
pods successfully complete, the Job tracks the successful
completions. When a specified number of successful completions is
reached, the task (ie, Job) is complete. Deleting a Job will clean up
the Pods it created. Suspending a Job will delete its active Pods
until the Job is resumed again.

A simple case is to create one Job object in order to reliably run one
Pod to completion. The Job object will start a new Pod if the first
Pod fails or is deleted (for example due to a node hardware failure or
a node reboot).

You can also use a Job to run multiple Pods in parallel.

If you want to run a Job (either a single task, or several in
parallel) on a schedule, see CronJob.

#### CronJob
[CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

A CronJob creates Jobs on a repeating schedule.

CronJob is meant for performing regular scheduled actions such as
backups, report generation, and so on. One CronJob object is like one
line of a crontab (cron table) file on a Unix system. It runs a Job
periodically on a given schedule, written in Cron format.

CronJobs have limitations and idiosyncrasies. For example, in certain
circumstances, a single CronJob can create multiple concurrent
Jobs. See the limitations below.

When the control plane creates new Jobs and (indirectly) Pods for a
CronJob, the .metadata.name of the CronJob is part of the basis for
naming those Pods. The name of a CronJob must be a valid DNS subdomain
value, but this can produce unexpected results for the Pod
hostnames. For best compatibility, the name should follow the more
restrictive rules for a DNS label. Even when the name is a DNS
subdomain, the name must be no longer than 52 characters. This is
because the CronJob controller will automatically append 11 characters
to the name you provide and there is a constraint that the length of a
Job name is no more than 63 characters.


_________________________
### Services
[Services](https://kubernetes.io/docs/concepts/services-networking/)

Every Pod in a cluster gets its own unique cluster-wide IP address
(one address per IP address family). This means you do not need to
explicitly create links between Pods and you almost never need to deal
with mapping container ports to host ports.  This creates a clean,
backwards-compatible model where Pods can be treated much like VMs or
physical hosts from the perspectives of port allocation, naming,
service discovery, load balancing, application configuration, and
migration.

Kubernetes imposes the following fundamental requirements on any
networking implementation (barring any intentional network
segmentation policies):

    - pods can communicate with all other pods on any other node without NAT
    - agents on a node (e.g. system daemons, kubelet) can communicate with all pods on that node

#### Gateway
[Gateway](https://kubernetes.io/docs/concepts/services-networking/gateway/)

Make network services available by using an extensible, role-oriented,
protocol-aware configuration mechanism. Gateway API is an add-on
containing API kinds that provide dynamic infrastructure provisioning
and advanced traffic routing.

##### Design principles
The following principles shaped the design and architecture of Gateway API:

    - **Role-oriented:** Gateway API kinds are modeled after organizational roles that are responsible for managing Kubernetes service networking:

**Infrastructure Provider:** Manages infrastructure that allows multiple isolated clusters to serve multiple tenants, e.g. a cloud provider.

**Cluster Operator:** Manages clusters and is typically concerned with policies, network access, application permissions, etc.

**Application Developer:** Manages an application running in a cluster and is typically concerned with application-level configuration and Service composition.

**Portable:** Gateway API specifications are defined as custom resources and are supported by many implementations.

**Expressive:** Gateway API kinds support functionality for common traffic routing use cases such as header-based matching, traffic weighting, and others that were only possible in Ingress by using custom annotations.

**Extensible:** Gateway allows for custom resources to be linked at various layers of the API. This makes granular customization possible at the appropriate places within the API structure.

#### Ingress
[Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)

An API object that manages external access to the services in a cluster, typically HTTP.

Ingress may provide load balancing, SSL termination and name-based virtual hosting.

##### Note:
Ingress is frozen. New features are being added to the Gateway API.

_________________________

