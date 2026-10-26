## Step 4: Install VMAgent

### Prerequisites:

Ensure you have installed the VM Operator as per [Step 3](..%2Foperator%2Freadme.md)

You will need to obtain your Last9 cluster's Remote Write URL and its
credentials.

[Here](https://last9.io/docs/onboard/) is a quick way to create your cluster and obtain your
credentials

Run the below command to list all the placeholder values in this file [vmagent.yaml](vmagent.yaml)

```bash
$ cat ./vmagent.yaml | grep -n "Todo"
# Output
19:    via_cluster: <via_cluster> # Todo: add a relevant cluster name. e.g: k8s cluster name
21:    - url: <last9_remote_write_url> # Todo: append last9 remote write URL
36:            storage: 20Gi # Todo: Default is 20Gi. Scale up after you have provisioned more if you need more
39:# Todo: Below configs need to be enabled depending upon your affinity towards nodegroups.
40:# Todo: Ensure that the below selector terms and tolerations are exactly same as the metadata of the nodegroups itself.
```

VMAgent reads the remote write username and password from the `last9-basic-auth` secret. Create it in the `last9-monitoring` namespace:

```bash
$ kubectl create secret generic last9-basic-auth \
    --from-literal=username=<last9_cluster_username> \
    --from-literal=password=<last9_cluster_password> \
    -n last9-monitoring
```

> If you installed VMAgent before this secret was renamed, your cluster has a `levitate-basic-auth` secret. Create `last9-basic-auth` with the same values before you apply the updated `vmagent.yaml`.

Proceed to installation once you have replaced the placeholder values with actual values.

### Installation:

1. Install VMAgent in the `last9-monitoring` namespace using `kubectl`:

    ```bash
    $ kubectl apply -f ./vmagent.yaml -n last9-monitoring
    ```

2. Verify the status of the vmagent and ensure that it's running:

    ```bash
    $ kubectl get pods -n last9-monitoring -l "last9_monitoring_agent=vmagent"
    # Output
    NAME                            READY   STATUS    RESTARTS   AGE
    vmagent-demo-6785f7d7b9-zpbv6   2/2     Running   0          72s
    ```
