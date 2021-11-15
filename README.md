**Note:** For the screenshots, you can store all of your answer images in the `answer-img` directory.

## Verify the monitoring installation

run `kubectl` command to show the running pods and services for all components. Take a screenshot of the output and include it here to verify the installation
```
vagrant@anuj-machine:~$ kubectl get po -n monitoring
NAME                                                     READY   STATUS    RESTARTS   AGE
prometheus-prometheus-node-exporter-ksdpq                1/1     Running   0          15h
prometheus-kube-state-metrics-569d7854c4-czwqh           1/1     Running   0          15h
prometheus-kube-prometheus-operator-86d499db8f-xwsnf     1/1     Running   0          15h
prometheus-grafana-5cddc775c4-p4zrm                      2/2     Running   0          15h
alertmanager-prometheus-kube-prometheus-alertmanager-0   2/2     Running   0          15h
prometheus-prometheus-kube-prometheus-prometheus-0       2/2     Running   0          15h
```
```
vagrant@anuj-machine:~$ kubectl get svc -n monitoring
NAME                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
prometheus-kube-prometheus-prometheus     ClusterIP   10.43.107.6     <none>        9090/TCP                     15h
prometheus-kube-prometheus-alertmanager   ClusterIP   10.43.160.159   <none>        9093/TCP                     15h
prometheus-prometheus-node-exporter       ClusterIP   10.43.33.17     <none>        9100/TCP                     15h
prometheus-kube-prometheus-operator       ClusterIP   10.43.236.238   <none>        443/TCP                      15h
prometheus-kube-state-metrics             ClusterIP   10.43.230.132   <none>        8080/TCP                     15h
alertmanager-operated                     ClusterIP   None            <none>        9093/TCP,9094/TCP,9094/UDP   15h
prometheus-operated                       ClusterIP   None            <none>        9090/TCP                     15h
prometheus-grafana                        NodePort    10.43.239.200   <none>        80:32330/TCP                 15h
```
Pods and services in observability namespace:
```
vagrant@anuj-machine:~$ kubectl get po -n observability
NAME                                 READY   STATUS    RESTARTS   AGE
my-jaeger-tracing-7d4967df8f-wpzqc   1/1     Running   2          3d14h
simplest-746f54765d-xz69n            1/1     Running   2          4d14h
jaeger-operator-694cbbb886-gvqlt     1/1     Running   1          5d14h
```
```
vagrant@anuj-machine:~$ kubectl get svc -n observability
NAME                      TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)             AGE
jaeger-operator-metrics   ClusterIP   10.43.87.80   <none>        8383/TCP,8686/TCP   5d14h
```
Pods and services in default namespace:
```
vagrant@anuj-machine:~$ kubectl get po
NAME                            READY   STATUS    RESTARTS   AGE
svclb-frontend-service-rc47g    1/1     Running   0          15h
svclb-backend-service-rr4dp     1/1     Running   0          15h
svclb-trial-service-8v6j5       1/1     Running   0          15h
trial-app-68f7874888-p84vc      1/1     Running   0          15h
backend-5ffd576b79-wrqgn        1/1     Running   0          15h
frontend-app-5b4f56dc76-l8wsg   1/1     Running   0          15h
```
```
vagrant@anuj-machine:~$ kubectl get svc
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes         ClusterIP      10.43.0.1      <none>        443/TCP          71d
frontend-service   LoadBalancer   10.43.79.149   10.0.2.15     8082:31762/TCP   15h
backend-service    LoadBalancer   10.43.73.63    10.0.2.15     8081:32452/TCP   15h
trial-service      LoadBalancer   10.43.45.81    10.0.2.15     8083:32290/TCP   15h
```

* [All pods](answer-img/pods.png)
* [All services](answer-img/services.png)


## Setup the Jaeger and Prometheus source
Expose Grafana to the internet and then setup Prometheus as a data source. Provide a screenshot of the home page after logging into Grafana.
![grafana](https://user-images.githubusercontent.com/69861555/141772570-9129ff74-7d7b-4ce0-a109-4c75b08c8b89.png)


## Create a Basic Dashboard
Create a dashboard in Grafana that shows Prometheus as a source. Take a screenshot and include it here.
![basic_grafana_dashboard](https://user-images.githubusercontent.com/69861555/141772651-c062d61d-eea5-4f90-a5f5-334e28cc3741.png)


## Describe SLO/SLI
Describe, in your own words, what the SLIs are, based on an SLO of *monthly uptime* and *request response time*.

**SLI** (Service Level Indicator) is basically a well defined quantitative measure of some level of service, in other words which can also be termed as "reality" metric.
**SLO** (Service Level Objective) is like a target value of service level measured by SLI, or in other words a reliavility target any team wants to achieve. It is like a "desired reality" metric.

**Example:** 
In terms of Monthly uptime, lets say we have an SLI of 96.9% for the month of october 2021, for an SLO of 96% for the month of october 2021.

In terms of request response time, we can have an SLI of average request response time of 188ms for the month of october 2021, for an SLO of 190ms of request response time for the month of october 2021.


## Creating SLI metrics.
It is important to know why we want to measure certain metrics for our customer. Describe in detail 5 metrics to measure these SLIs. 
```
Below can be the 5 metrics to measure SLIs:
1- Error rate: It is basically an indicator of downtime.
2- Uptime: It is a direct mesaurement of our Service Availability during a period of time
3- Latency: It is a metric to measure response time of the user an API offers.
4- Traffic: It is a metric to measure the amount of traffic one is getting during periods of time.
5- Network Capacity: It is a metric to handle the request of the usage of a service.
```

## Create a Dashboard to measure our SLIs
Create a dashboard to measure the uptime of the frontend and backend services We will also want to measure to measure 40x and 50x errors. Create a dashboard that show these values over a 24 hour period and take a screenshot.
* [app dashboard 1](answer-img/app_grafana_dashboard.png)
* [app dashboard 2](answer-img/app_grafana_dashboard_2.png)

## Tracing our Flask App
We will create a Jaeger span to measure the processes on the backend. Once you fill in the span, provide a screenshot of it here.
* [jaeger app](answer-img/jaeger_app.png)

## Jaeger in Dashboards
Now that the trace is running, let's add the metric to our current Grafana dashboard. Once this is completed, provide a screenshot of it here.
* [grafana displaying jaeger traces](answer-img/grafana_jaeger.png)

## Report Error
Using the template below, write a trouble ticket for the developers, to explain the errors that you are seeing (400, 500, latency) and to let them know the file that is causing the issue.
```
TROUBLE TICKET
Name: Backend Issues
Date: 2021-11-15 / 1:00 am 
Subject: mongodb setup is not done
Affected Area: backend application /star api failing
Severity: ERRORS
Description: mongodb setup is not done
```

## Creating SLIs and SLOs
We want to create an SLO guaranteeing that our application has a 99.95% uptime per month. Name three SLIs that you would use to measure the success of this SLO.
* Error rate
* Uptime
* Latency

## Building KPIs for our plan
Now that we have our SLIs and SLOs, create KPIs to accurately measure these metrics. We will make a dashboard for this, but first write them down here.

1- Error Rate: It can be measured using- http requests exceptions by container, jaeger traces by container, error response rate per second, success response rate per second etc.

2- Uptime (Total uptime for each service per Month which can be measured using frontend service/backend service/trial service uptime)

3- Latency: It can be measured using- frontend http service for 4xx and 5xx, backend http service 4xx and 5xx, number of successful/failure requests per second etc.

## Final Dashboard
Create a Dashboard containing graphs that capture all the metrics of your KPIs and adequately representing your SLIs and SLOs. Include a screenshot of the dashboard here, and write a text description of what graphs are represented in the dashboard.
* [final dashboard](answer-img/final_dashboard.png)

Dashboard contains below graphs:
- Graphs showing Frontend/backend services uptime
- Graphs showing jaeger tracing by containers
- Graphs showing http requests, error/success response rate
- Graphs showing memory/cpu/io usage
- Graphs showing network traffic
- Graphs showing frontend http service for 4xx and 5xx, backend http service 4xx and 5xx
