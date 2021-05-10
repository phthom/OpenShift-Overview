# OpenShift Admin Lab



![image-20210510191953304](images/image-20210510191953304-0667193.png)



Duration: 30 minutes



##  Introduction

This lab is the continuation of the first OpenShift Lab. During this new lab, we are going to concentrate on the cluster administrator work like managing the pods, nodes, versions, CRDs, monitoring, scaling ...





## Task #8 - OpenShift Administrator Console

If you are still on the **Developer** side, move to the **Administrator**.

![image-20210510142914979](images/image-20210510142914979-0649755.png)



You will a screen like this one:

![image-20210510143035660](images/image-20210510143035660-0649835.png)



The Dashboard (Overview) is very interesting and very popular for troubleshooting. 

Notice the following cards : first the **Details** where you can check the OpenShift **version** and if you can update to a newer version. 

![image-20210510143353558](images/image-20210510143353558-0650033.png)



Then the **Cluster Inventory** : This is were you can check the different resources (like nodes or pods and storage). If you have a red point near Pods, then you will be quickly drill down to the **failling** or suspended pod(s). 

![image-20210510143537067](images/image-20210510143537067-0650137.png)



In the Status, you will see if the different components (like operators) in the cluster are healthy or not:

![image-20210510143932393](images/image-20210510143932393-0650372.png)



The **Cluster Utilisation** is very important to see how your Pods are consumming the infrastructure resources like CPU, RAM, storage, bandwidth ...

![image-20210510144154576](images/image-20210510144154576-0650514.png)



Finally, one most important componant in Kubernetes : the **event** messages ! All Kubernetes events are shown in one place and this very helpfull is you want to analyse and quickly identify issues, error or impacting problems. 

![image-20210510144337464](images/image-20210510144337464-0650617.png)



So to summarize, in one dashboard overview, you can quickly analyze the situation and find a solution for a specific issue. 



## Task #9 - Projects and Workloads

Now move to the **projects** : as the cluster administrator, you can view all the projects (namespaces) in the cluster (normally the developers just see their own namespace). Click on you **labproj**<xx>

![image-20210510145036138](images/image-20210510145036138-0651036.png)



This brings you to this overview but specifically for this **project** (labprojxx). 

![image-20210510145201952](images/image-20210510145201952-0651121.png)



The Dashboard of your labproj<xx> is very interesting because you can see a lot of informations: 

- Inventory of OpenShift objects like Pods, Deployments, Services ...

- Status (if some errors) for the project

- Utilization (CPU, Memory ...) for the project

- Activity (from the events) for the resources in that project

- Quotas


Now drill down to the Pods to look at the **Pods** in Inventory in your **labproj**<xx>:

![image-20201115181928571](images/image-20201115181928571-5460768.png)



On the left pane, click on the **Deployment**

![image-20201115182009605](images/image-20201115182009605-5460809.png)



Let's click on that specific **deployment**. A deployement contains the **replicasets**, the **pods**, the **quotas** ...

![image-20201115182129168](images/image-20201115182129168-5460889.png)



The **YAML definition** represents the definition of the deployment. In recent versions of Kubernetes, you will see a lot new rows (like the **fieldsType**). fieldsType are added automatically to help to understand how Kunetrnetes is working. Unfortunately, this doesn't add clarity to read the yaml definition. 

![image-20201115182212570](images/image-20201115182212570-5460932.png)



If you browse the YAML file at the **end of the file**, you will see the POD specifications and the **image** of the container:

![image-20210510150403514](images/image-20210510150403514-0651843.png)



The **Replica Sets** Tab will show you the number of replicas that we started (one here)

![image-20201115182236981](images/image-20201115182236981-5460957.png)



Finally the **Events** Tab (this log of events is concerning the activity around the deployment of the Pods):



![image-20201115182537918](images/image-20201115182537918-5461137.png)



## Task #10 - Into the PODs



Now from the deployment, click on the **Pods** tab.  And you might see 2 PODs : one for the **build** (S2I) and the other concerning the running node.js **application**. 

![image-20201115182418058](images/image-20201115182418058-5461058.png)



You can notice that you have one pod that is running. **Click on that Pod**:

![image-20201115182630165](images/image-20201115182630165-5461190.png)



At the bottom of that page, you will see the list of **containers** and **volumes**

![image-20210510151447185](images/image-20210510151447185-0652487.png)





Now click in the **log** (concerning the application) :

![image-20201115182703185](images/image-20201115182703185-5461223.png)



And then finally, click on the Terminal Tab to get access **inside the container** (try typing several linux commands like ps or ls):

![image-20201115182740011](images/image-20201115182740011-5461260.png)





## Task #11 - Scaling your application

One important task of the cluster administrator is to manage the workload and to be able to increase the number of PODs when the number of requests is becoming more important. 



To learn about scaling your application, on the left pane, click on the **Deployment** and then click on your specific deployment:

![image-20201115182825721](images/image-20201115182825721-5461305.png)



Finally, increase the number of pods (**not too much**: 2 for instance)

![image-20200405175523790](images/image-20200405175523790-6102123.png)



And after a while, you will see 2 active pods.

![image-20200405175559894](images/image-20200405175559894-6102159.png)



This means that now 2 pods are serving the requests behind the same kubernetes service. And all the requests will go in round robin automatically on one or the other pods.  

Now click on one of these new **pods** and you should see a page like this one showing new activity:

![image-20200405175941740](images/image-20200405175941740-6102381.png)

Now go back to the **list of PODs** (click on Pods on the left pane):

![image-20210510152723612](images/image-20210510152723612-0653243.png)



Let's try a crazy experience and kill a pod ! click on the three dots at the end of the row and select `delete Pod`

![image-20210510152950751](images/image-20210510152950751-0653390.png)

The following popup window will appear:

![image-20210510153042086](images/image-20210510153042086-0653442.png)



Click on `Delete` and you should see breifly your pod terminating.

![image-20191012153249281](images/image-20191012153249281-0887169.png)



A new pod is **automatically started** !!! Because the number of **replicaset** has been defined to **2** , even if a pod is crashing or has been deleted, it will be replace automatically by a new one. 

![image-20201115182948172](images/image-20201115182948172-5461388.png)



## Task #12 - Monitoring your cluster

In Openshift, out of the box, you have a Prometheus server and a Grafana server that you can access from the OpenShift web Console. 

Click on the **Monitoring** section on the left pane and then click on **Dashboards**:

![image-20210510155336264](images/image-20210510155336264-0654816.png)



![image-20210510155442627](images/image-20210510155442627-0654882.png)



No data is collected for ETCD so the dashboard is empty.

Select a different dashboard in the list: Select the Cluster dashboard:

![image-20210510160023159](images/image-20210510160023159-0655223.png)



Then you will see this page with global metrics (CPU, RAM, Network, and commitments). Browse the page:

![image-20210510160214501](images/image-20210510160214501-0655334.png)



Then move your cursor to a graph to show specific metrics:

![image-20210510160532177](images/image-20210510160532177-0655532.png)



You can explore some other dashboards from the list:

![image-20210510160725364](images/image-20210510160725364-0655645.png)



You can also go to **Grafana UI**:

![image-20210510160848438](images/image-20210510160848438-0655728.png)



![image-20210510161036495](images/image-20210510161036495-0655836.png)



Click **Home > Default**

![image-20210510161126538](images/image-20210510161126538-0655886.png)

Pick one graph in the list:

![image-20210510163743168](images/image-20210510163743168-0657463.png)



## Task #13 - Cluster Administration



From the left pane, click on **Cluster Settings**:

![image-20210510164010568](images/image-20210510164010568-0657610.png)



In this section, you can upgrade the cluster version to a newer one (no update at the moment):

![image-20210510164219510](images/image-20210510164219510-0657739.png)

To look at the different **Custom Resource Definitions** (CRD) : 

![image-20210510164427787](images/image-20210510164427787-0657867.png)

CRDs are generaly used in **Operators**. 

Go to **OperatorHub** to list all possible Operator that you can install  on your cluster:

![image-20210510164721269](images/image-20210510164721269-0658041.png)

Most of the solutions are installed by using operators. Each operator corresponds to a specific solution or application. Operators propose **5 levels of capabilities** that you can on the left side of this page :

![image-20210510185053473](images/image-20210510185053473-0665453.png) 

Find the `Web Terminal`operator :

![image-20210510185306839](images/image-20210510185306839-0665586.png)



Click on the Web Terminal tile to see the details of the installation :

![image-20210510185430274](images/image-20210510185430274-0665670.png)



**Do not install** ...

Close the window and go back to the OpenShift Console



## Task #14 - Storage and Networking



On the left pane, **Click on Storage > Persistent Volume Claim** 

![image-20210510185805811](images/image-20210510185805811-0665885.png)



![image-20210510185821212](images/image-20210510185821212-0665901.png)

You should see the **internal OpenShift registry**.  Then click on the **Persistent Volume**.

![image-20210510190004579](images/image-20210510190004579-0666004.png)



You can see the storage size by default for the internal registry : **100 Go**. This storage has been booked using a storage class (**ibmc-file-gold**). The status is `bound`. 

Now move to the **Networking** section and look at you service in the **labproj**<xx>

![image-20210510190519027](images/image-20210510190519027-0666319.png)



![image-20210510190616370](images/image-20210510190616370-0666376.png)



Drill down to the **service name:**

![image-20210510190720086](images/image-20210510190720086-0666440.png)

Drill down to the **PODs** associated to this service:

![image-20210510190814981](images/image-20210510190814981-0666495.png)





## Conclusion

**Congrats !!!**  You successfully used the console to manage your cluster !!

You noticed the following details:

- the console facilitates the administrator work
- it is easy to change the cluster to a new version 
- it is easy to drill down to troubleshoot any issue by looking at the event or at the logs
- you can even go inside a container easily
- the monitoring tool is everywhere in the console and you can also go in Grafana.
- Operators facilitate the installation of a lot of different opensource software
- Everything can be done thru the console

----

----



# End of Lab



