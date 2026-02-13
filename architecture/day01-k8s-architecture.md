📘 DAY 1 — THEORY
🧱 What is Kubernetes Architecture?

Kubernetes is built around one main idea:

👉 Desired State Management
You tell Kubernetes what you want, and internal components constantly work to make reality match that state.

The system has two major parts:

Control Plane → brain of the cluster

Worker Nodes → where containers actually run

🧠 Control Plane Components (The Decision Makers)
1️⃣ API Server

The API Server is the front door of Kubernetes.

What it does:

Receives all requests (kubectl, UI, CI/CD, controllers)

Validates manifests

Stores cluster state in etcd

Acts as the central communication hub

Important:
Nothing talks directly to anything else — everything goes through API Server.

2️⃣ etcd

etcd is the database of Kubernetes.

What it stores:

Pods

Deployments

Secrets

Configurations

Node status

Key idea:
If etcd loses data → cluster forgets its state.

That’s why production clusters run etcd in highly available mode.

3️⃣ kube-scheduler

Scheduler decides:

👉 Which node runs which pod

It checks:

Available CPU/memory

Node labels

Affinity rules

Taints & tolerations

Resource requests

Important:
Scheduler does NOT create pods — it only assigns nodes.

4️⃣ Controller Manager

Controllers constantly watch the cluster and fix differences between:

desired state

actual state

Examples:

Deployment controller

ReplicaSet controller

Node controller

Example:
You want 3 replicas → only 2 exist → controller creates 1 more.

This is the core of Kubernetes self-healing.

🖥️ Worker Node Components (The Executors)
5️⃣ kubelet

kubelet is the agent running on every node.

Responsibilities:

Talks to API Server

Starts containers using runtime

Reports node health

Executes pod instructions

Think:
Scheduler chooses node → kubelet actually runs it.

6️⃣ Container Runtime

Software that runs containers:

Examples:

containerd

CRI-O

Kubernetes itself does not run containers — runtime does.

7️⃣ kube-proxy

Handles networking rules on each node.

Responsibilities:

Service routing

Load balancing between pods

Managing iptables/ipvs rules

🔄 Full Flow Example (VERY IMPORTANT)

When you run:

kubectl apply -f deployment.yaml


What happens internally:

kubectl sends request → API Server

API Server validates and stores state → etcd

Controller Manager sees new Deployment → creates Pods

Scheduler detects unscheduled Pods → selects node

kubelet on chosen node pulls image and starts container

kube-proxy updates networking

If you understand this flow, most interview questions become easy.

🧩 Key Concept: Self-Healing

Kubernetes is event-driven:

Pod crashes → kubelet reports failure

Controller sees missing replica

Scheduler places replacement pod

High availability comes from controllers + replicas — not from kubectl commands.

❓ END-OF-DAY QUESTIONS — DAY 1