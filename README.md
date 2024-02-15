###### [_↩ Back to `main` branch_](https://github.com/cuongpiger/cloud)

<hr>


# Cloud dict

## MISC

|#|Term|Description|Note|Tags|
|-|-|-|-|-|
|1|CRM system|**A Customer Relationship Management _(CRM)_** system is a software tool or platform designed to help businesses manage and analyze interactions with current and potential customers. The primary goal of a CRM system is to improve relationships with customers, streamline processes, and increase sales and profitability.||`crm`|
|2|COTS|**COTS** stands for "**Commercial Off-The-Shelf**", and a **COTS application** refers to software that is ready-made and commercially available for purchase or licensing. These applications are developed and marketed by software vendors to be sold to a wide range of customers, rather than being custom-built for a specific organization or purpose.||`cots`|
|3|Bespoke app|A **bespoke app**, also known as **custom software** or **tailored software**, is an application designed and developed specifically for the needs of an **individual user or organization**. It's like a **made-to-measure suit**, perfectly fitted to your **unique requirements**, unlike **off-the-shelf software _(COTS)_** that caters to a broader audience and may not fully match your specific needs.||`bespoke`|
|4|Admission control|In Kubernetes (k8s), **admission control** refers to a set of mechanisms and policies used to enforce rules and constraints on resource creation and modification within a Kubernetes cluster. Admission control enables administrators to **intercept** and **modify requests** to the Kubernetes API server before they are persisted in the cluster's state.||`k8s`, `admission-control`|

## Kubernetes (k8s)

|#|Term|Description|Note|Tags|
|-|-|-|-|-|
|1|Admission control|In Kubernetes (k8s), **admission control** refers to a set of mechanisms and policies used to enforce rules and constraints on resource creation and modification within a Kubernetes cluster. Admission control enables administrators to **intercept** and **modify requests** to the Kubernetes API server before they are persisted in the cluster's state.||`k8s`, `admission-control`|
|2|Kube controller manager||Detail at [01-kube-controller-manager.md](./details/k8s/01-kube-controller-manager.md)|`k8s`, `kube-controller-manager`|
|3|Event-driven|In Kubernetes (k8s), "**event-driven**" refers to a paradigm where actions or behaviors are triggered in response to specific events or changes within the cluster. In this context, events can include various actions or occurrences such as pod creations, deletions, updates, node failures, or changes in resource utilization.||`event-driven`|
|4|Informer|An "**Informer**" is a **client library** used to **watch for changes** to Kubernetes resources and **receive notifications** about those changes. **Informers are an essential component of Kubernetes controllers** and other components that need to react to changes in the state of the cluster.|Detail at [02-informer.md](./details/k8s/02-informer.md)|`informer`|