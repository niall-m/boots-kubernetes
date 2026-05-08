# Boot.dev Kubernetes Learning Repo

This repository contains the Kubernetes manifests I built while working through Boot.dev's Kubernetes course. The project uses a small `synergychat` app plus a couple of test workloads to practice the core pieces of running applications on Kubernetes.

## What This Repo Covers

- Deploying multi-service applications with `Deployment` resources
- Exposing pods internally with `Service`
- Passing configuration with `ConfigMap`
- Persisting application data with a `PersistentVolumeClaim`
- Using an ephemeral filesystem-backed database via `emptyDir`
- Routing traffic with the Gateway API using `GatewayClass`, `Gateway`, and `HTTPRoute`
- Setting CPU and memory requests/limits
- Exploring horizontal pod autoscaling with `HorizontalPodAutoscaler`
- Using namespaces and cluster DNS for service-to-service communication

## Architecture

The main app in this repo is `synergychat`, split into three parts:

- `web`: frontend deployment plus service and HTTP route
- `api`: backend deployment plus service, config map, and persistent volume claim
- `crawler`: background workload running in the `crawler` namespace with its own service and config map

There are also two focused practice workloads:

- `testcpu`: a small deployment paired with an HPA to explore CPU-based scaling behavior
- `testram`: a deployment with large memory requests/limits to understand memory scheduling and resource constraints

## Learning Points

Working through these manifests helped reinforce a few practical Kubernetes ideas:

- A `Deployment` manages the desired state for pods, including images, replicas, labels, and resource settings.
- A `Service` gives pods a stable in-cluster address even though individual pods are replaceable.
- `ConfigMap` values can be injected one key at a time or all at once with `envFrom`, depending on how much control is needed.
- Persistent storage matters for stateful components like the API, while the crawler demonstrates an ephemeral filesystem-backed database stored in an `emptyDir` volume.
- Pods can contain more than one container, as shown in the crawler deployment sharing a common volume.
- Internal DNS names like `crawler-service.crawler.svc.cluster.local` are a simple way for services in different namespaces to communicate.
- Gateway API resources provide a clearer way to express external routing than putting all traffic logic directly on a service.
- Resource requests and limits affect scheduling and runtime behavior, and autoscaling depends on those signals being meaningful.

## File Guide

- `api-*`: backend deployment, service, config, route, and persistent storage
- `web-*`: frontend deployment, service, route, config, and HPA
- `crawler-*`: crawler deployment, service, and config in a separate namespace
- `app-gateway*`: shared Gateway API entrypoint resources
- `testcpu-*`: CPU autoscaling practice manifests
- `testram-*`: memory allocation practice manifests

## Notes

This repo is intentionally learning-focused rather than production-ready. A few resources assume supporting cluster setup already exists, such as a Gateway controller and the `crawler` namespace.
