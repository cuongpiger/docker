###### [_↩ Back to `homepage`_](./../../README.md)

# Chapter 4. Common `kubectl` commands

###### 🌈 Table of Contents
  - ##### 1. [Namespaces](#1-namespaces-1)
  - ##### 2. [Contexts](#2-contexts-1)

# [1. Namespaces](#1-namespaces)
- K8s use **namespaces** to organize objects in the cluster.
- You can think each namespace as a folder that holds a set of objects.
- For example, if you want to get all pods in the `kube-system` namespaces, run the command:
  ```bash
  kubectl get pods -n kube-system
  ```
  ![](./img/01.png)

- To get pods in all namespaces in the cluster, run the command:
  ```bash
  kubectl get pods -A
  ```

- To get all namespaces in the cluster, run the command:
  ```bash
  kubectl get namespaces
  ```

# [2. Contexts](#2-contexts)
- You use `context` to change the default namespace more permanently.
- For example, to create context `my-context` which uses namespace `my-namespace`, run the command:
  ```bash
  kubectl config set-context my-context --namespace=my-namespace
  kubectl config use-context my-context
  ```

  