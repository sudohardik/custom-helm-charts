# Helm Chart Installation Guide

Follow these steps to install Helm charts directly into your Kubernetes (k8s) cluster:

1. **Add the GitHub repository to Helm:**

    ```sh
    helm repo add gitrepo https://sudohardik.github.io/custom-helm-charts/
    ```

2. **Search for the required Helm chart in the repository:**

    ```sh
    helm search repo gitrepo
    ```

3. **Install the Helm chart:**

    ```sh
    helm install test gitrepo/couponservice-chart
    ```

4. **Verify Installation:**
   - Wait for the pods to transition to the running state.

5. **Port-Forward Services (if needed):**
   - Use `kubectl port-forward` to access the services.

That's it! Your Helm chart should now be successfully installed and running.
