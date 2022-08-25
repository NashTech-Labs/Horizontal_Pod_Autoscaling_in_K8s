# Configuring Horizontal Pod Autoscaling in K8s

In order to automatically scale the workload to meet demand, a HorizontalPodAutoscaler automatically modifies a workload resource such as a deployment, replica set or statefulset.
The HorizontalPodAutoscaler informs the workload resource (the Deployment, StatefulSet, or other resources) to scale back down if the demand drops and the number of Pods is more than the configured minimum.

Firstly, install a tool called “metric server”.

`wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`

Apply the manifest files.

After this, the following command will be used to autoscale the “server” deployment that we have created and the minimum number of pods is one and the maximum is 10 and the CPU % is 50%.

`kubectl autoscale deployment server --cpu-percent=50 --min=1 --max=10`

Then, run the command below to increase the load on autoscaler.

` kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://server; done"`

Then, run the command below to watch that how the cup usage goes up.

`kubectl get hpa server --watch`
