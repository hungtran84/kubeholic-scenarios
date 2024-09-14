# Step 1: Set Up and Generate ConfigMap

In this step, you'll create a `kustomization.yaml` file and use it to generate a ConfigMap for a sample Kubernetes application.

1. **Navigate to the sample application directory:**

    ```bash
    cd /root/learning-kustomize/sample-app
    ```

2. **Explore the `kustomization.yaml` file:**

    Open the `kustomization.yaml` file to see how the ConfigMap is defined:

    ```bash
    cat kustomization.yaml
    ```

    The file should look like this:

    ```yaml
    apiVersion: kustomize.config.k8s.io/v1beta1
    kind: Kustomization
    configMapGenerator:
      - name: example-configmap
        literals:
          - SQLITE_DB_LOCATION=/tmp/todos/todo.db
    resources:
      - deployment.yaml
    ```

3. **Generate the ConfigMap:**

    Run the following command to generate the ConfigMap using Kustomize:

    ```bash
    kustomize build .
    ```

4. **Apply the ConfigMap to your Kubernetes cluster:**

    Apply the generated ConfigMap and deployment to your cluster:

    ```bash
    kustomize build . | kubectl apply -f -
    ```

5. **Verify the ConfigMap:**

    Check that the ConfigMap was successfully created:

    ```bash
    kubectl get configmaps
    ```

    You should see the `example-configmap` in the list.

6. **Verify the Deployment:**

    Ensure that the deployment is using the correct environment variable:

    ```bash
    POD="$(kubectl get pods -l app=todo --no-headers -o custom-columns=":metadata.name")"
    kubectl exec -it $POD -- printenv | grep SQLITE_DB_LOCATION
    ```

    The output should show `/tmp/todos/todo.db`.

---

After completing these steps, you've successfully generated and applied a ConfigMap using Kustomize!
