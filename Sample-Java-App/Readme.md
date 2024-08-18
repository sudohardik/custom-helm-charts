# Helm Chart Deployment Guide

This guide walks through the steps to deploy an application using a Helm chart, configure MySQL as a dependency, and connect to the services in a Minikube cluster.

## Steps

1. **Create Chart**:  
   - Delete unnecessary files.  
   - Configure the application image in `values.yaml`.

2. **Update Deployment**:  
   - Update the `Deployment` manifest according to the application requirements.

3. **Configure NodePort**:  
   - Set up the `NodePort` in `values.yaml` to expose services externally.

4. **Add MySQL Dependency** in `chart.yaml` by including the following:

    ```yaml
    dependencies:
      - name: mysql
        version: <version>
        repository: "https://charts.bitnami.com/bitnami"
    ```

5. **Pass Values (e.g., Root Password, DB)** in `values.yaml`:

    ```yaml
    mysql:
      auth:
        rootPassword: <your_root_password>
        database: mydb
        username: <your_db_username>
        password: <your_db_password>
    ```

6. **Use ConfigMap**:  
   - Use a `ConfigMap` to externalize environment variables or configuration files for the application.

7. **Release and Test**:  
   - Install or upgrade the Helm release with the following command:

    ```bash
    helm upgrade --install <release_name> ./<chart_name>
    ```

   - Ensure that the pods are in a running state using:

    ```bash
    kubectl get pods
    ```

8. **Port Forward to Access the App and MySQL**:  
   After deploying the Helm chart and confirming that the pods are running, use the following commands in separate terminals to port-forward both the app and MySQL service:

   - For MySQL:

    ```bash
    kubectl port-forward svc/docker-mysql 32762:3306
    ```

   - For the application:

    ```bash
    kubectl port-forward svc/couponservice-couponservice-chart 32763:9091
    ```

9. **Connect to MySQL**:  
   You can connect to the MySQL database using tools like **DBeaver** or **MySQL Workbench** with the above URL. After connecting, run the following SQL commands:

    ```sql
    USE mydb;

    SELECT * FROM coupon;

    INSERT INTO coupon (code, discount, exp_date) VALUES('SUPERSALE', 10, '2024-12-22');
    ```

10. **Test the API**:  
    Once the MySQL connection is set up, test your API by making a GET request to the following URL:

    ```
    http://127.0.0.1:32763/couponapi/coupons/SUPERSALE
    ```

    If everything is configured correctly, you should receive a response with the coupon details.

---

Ensure that you have Helm, Minikube, and Kubernetes properly installed and configured before proceeding with these steps.
