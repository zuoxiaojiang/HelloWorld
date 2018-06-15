# ServiceComposition
* Two approachs of service composition based on the idea of bucket algorithm and Minicon algorithm in traditional data integration.
* For running this program, you should perform the following steps：
1. Data Preparing: setting underlying relations、 user queries and source service collection(we choose manual service source as default).
2. Program Driving: algorithm selection(we select Minicon as default in this doc)、 driven configration、 execution and result analysis.
## Setup
* Download and setup Scala with version of 2.10.6 and JDK with version of 1.7.<br>

## Data Preparing
### To set underlying relations
* In `DataUtil.scala`, create underlying relations by providing such parameters :`relationname`,`outputattributes`. For example:<br>
```scala
val vesseltraj = new QueryService("vesseltraj", Set("mmsi","long","lat","speed"))
```
### To set the user query
* In `DataUtil.scala`, propose queries based on underlying relations by providing such parameters:`queryname`, `queryattributes`, `dataconstraints`, `timewindow`. And then assign one of the queries to the variable query, for example:<br>
```scala
val query1 = new QueryService("Q1", Set(vesselinfo, vesseltraj),Set("mmsi","callsign"),Set(("speed",40,POSI)),(5,4))
val query:QueryService = query1
```
* * *
### To set the source service collection which is used to generate rewriting services
#### Setting services or service instances manually (Default)
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
#### Setting services or service instances by simulating
* In `DataUtil.scala`, create a relation collection `simulSource` which is used to simulate the service source:<br>
```scala
val simulSource:Array[QueryService] = Array[QueryService](Movie,Revenues,Director,vesseltraj,vesseltravelinfo,vesselinfo)
```
* Simulate the service source by giving such parameters:`relationcollection`, `query`, `servicesourcesize`. For example:<br>
```scala
val simulServices:List[QueryService] = SourceSImulation.geneViews(DataUtil.simulSource,query,1000)
```
## Program Driving 
### Algorithm Selection
#### BucketServiceComposition (Default)
* In `BucketServiceComb.scala`, assign the `DataUtil.query` to variable `query` as a user query:<br>
```scala
val query = DataUtil.query
```
* Appoint an service source which is used to generate rewriting service plans, there are two methods to appoint<br> corresponding to two different scenarios，you can run a use case using the `ucServices` which is create manually:<br>
```scala
val services = DataUtil.ucServices
```
or you can measure the performance of bucket service composition using the `simulServices`:<br>
```scala
val services = DataUtil.simulServices
```
* After that you can run the program.
#### SMinicon
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
