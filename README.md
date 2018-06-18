# Continuous Query Service Composition
The cross-organization feature of service makes it possible for acting as a data sharing medium, when a user requires to access the data sources across organizations, we could compose data services or data providing services to answer the user query. 
We proposed a model of continuous query service based on the data stream, and expanded two service composition algorithms(we call them SBucket and SMinicon) based on the ideas of Bucket algorithm and Minicon algorithm which are generally applied in traditional data integration. The aim of our project is to use one of the two algorithms to find executable query rewriting services from services and service instances exposed by system to answer user query.<br>
The main process are the following steps:<br>
* **Atomic Services Setting:**
> Equivalent to the concept of mediation pattern in traditional data integration, and the establishment of user query, continuous query service, and continuous query service instance is based on atomic service.
* **User Query Setting:**
> Conjunctive queries proposed by users based on the atomic services.
* **Continuous Query Services or Service Instances Exposed by System Setting:**
> Equivalent to the concept of view in traditional data integration, they are established on the atomic services, and act as the exposing service source to generate rewriting services to answer user query.
* **Service Algorithm Selection and Execution:**
> After setting the above, what is to do the next is choosing a method to generate rewriting services, SBucket algorithm can be used in SBucket.scala while SMinicon can be used in SMinicon.scala. Then you need to drive the program by appointing the user query, exposing service source in DataUtil.scala.
* **Result Analysis:**
> After executing one of the method, the result will display on console, which includes information about rewriting services, time cost, etc.
The following section we will demonstrate a use case to illustrate how it works.
## Setup
* Download and setup Scala with version of 2.10.6 and JDK with version of 1.7.<br>
## Use Case
### Step 1: To set atomic services
* In `DataUtil.scala`, create atomic services by providing such parameters :`atomicservicename`,`outputattributes`. For example:<br>
```scala
val vesseltraj = new QueryService("vesseltraj", Set("mmsi","long","lat","speed"))
```
### Step 2: To set the user query
* In `DataUtil.scala`, propose queries based on atomic services by providing such parameters:`queryname`, `queryattributes`, `dataconstraints`(formed as a tuple of ("attribute",(leftvalue,rightvalue)), and POSI means positive infinity while NEGT means negative infinity), `timewindow`(formed as a tuple of (range, slide)). And then assign one of the queries to the variable query, for example:<br>
```scala
val query1 = new QueryService("Q1", Set(vesselinfo, vesseltraj),Set("mmsi","callsign"),Set(("speed",40,POSI)),(5,4))
val query:QueryService = query1
```
### Step 3: To set the exposing continuous query services and service instances
* In `DataUtil.scala`, create services by providing such parameters:`servicename`, `dataconstraints`, `timewindow`. For example:<br>
```scala
val S1 = new QueryService("S1",Set(vesseltraj),Set(("speed",NEGA,30)),(5,2))
```
* Create service instances by providing such parameters:`serviceinstancename`, `outputattributes`, `dataconstraints`, `timewindow`. For example:<br>
```scala
val S2 = new QueryService("S2",Set(vesseltraj,vesselinfo), Set("mmsi", "draught", "speed"),Set(("imo",NEGA,2000)),(5,2))
```
* Load these to a service collection `ucServices` as a use case service source:<br>
```scala
val ucServices = List(S1,S2)
```
### Step 4: Algorithm Selection and Execution
* In `BucketServiceComb.scala`, assign the `DataUtil.query` to variable `query` as a user query:<br>
```scala
val query = DataUtil.query
```
* Appoint the exposing service source `ucServices` we created manually in **Step 3** :
```scala
val services = DataUtil.ucServices
```
* After that you can run the program.
### Step 5: To analyse the result
* The result will display in the console, including information about service source simulation、 bucket elements、executable service composition collection、 each phase time costs:<br>

### SMinicon
* In `MiniConServiceComb.scala`, assign the `DataUtil.query` to variable `query` as a user query:<br>
```scala
val query = DataUtil.query
```
* Appoint an service source which is used to generate rewriting service plans, there are two methods to appoint<br> corresponding to two different scenarios，you can run a use case using the `ucServices` which is create manually:<br>
```scala
val S = DataUtil.ucServices
```
or you can measure the performance of bucket service composition using the `simulServices`:<br>
```scala
val S = DataUtil.simulServices.toSet
```
* After that you can run the program.
### To analysis result
#### BucketServiceComposition
* The result will display in the console, including information about service source simulation、 bucket elements、executable service composition collection、 each phase time costs:<br>

#### SMinicon
* The result will display in the console, including information about service source simulation、 bucket elements、executable service composition collection、 each phase time costs:<br>
