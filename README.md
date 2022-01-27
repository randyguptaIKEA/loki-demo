# loki-demo
This repository contains a few snippets for my grafana loki demo.

For the local development, I have created the `pod.yaml` file using `podman generate kube ...` using the docker commands from [here](https://grafana.com/docs/loki/latest/installation/docker/#install-with-docker).

To run the pod locally:

```
podman play kube pod.yaml
```

To stop the pod:

```
podman play kube --down pod.yaml
```

## Grafana

After the start of the pod, I can access the grafana dashboard on the local url:

http://localhost:3000/login

The default usernamen and password is `admin`. At this point, loki has not yet been added as datasource. To do so I have to navigate to 

Configuration -> Data sources

and add loki as data source. 

The url for loki is:

http://127.0.0.1:3100

## Loki

From now on I can access loki on the grafan dashboad when I navigate to 

Explore -> Loki

At the moment loki doesn't have any data just yet, so we need to create some log entries.
## Loki API

We will use the loki API directly to push logs into loki as described [here](https://grafana.com/docs/loki/latest/api/#post-lokiapiv1push).

In my example I'll use postman.

In the pre-request script I define a variable with the timestamp:

```javascript
pm.environment.set("timestamp", Date.now() * 1000000);
```

The request body:

```
{
    "streams": [
        {
            "stream": {
                "compartment": "eu"
            },
            "values": [
                [
                    "{{timestamp}}",
                    "This is a test."
                ],
                [
                    "{{timestamp}}",
                    "Warning - UC049 I_ORDER_CHANGES_DLV - The Change source at head level is not yet implemented   BU_CODE_LU 280 ,BU_TYPE_LU STO, trans_id: 7469262937"
                ]
            ]
        }
    ]
}
```

The action is a HTTP POST to http://127.0.0.1:3100/loki/api/v1/push

