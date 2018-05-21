# BucketServiceComposition

## Setup
* Download and setup Scala with version of 2.10.6 and JDK with version of 1.7.<br>

### To set underlying relations
* In `DataUtil.scala`, create underlying relations by providing such parametors :`relationname`,`outputattributes`. For example:<br>
```scala
val vesseltraj = new QueryService("vesseltraj", Set("mmsi","long","lat","speed"))
```
### To set user queries
* In `DataUtil.scala`, propose queries based on underlying relations by providing such parameters:`queryname`, `queryattributes`, `dataconstraints`, `timewindow`. For example:<br>
```scala
val query1 = new QueryService("Q1", Set(vesselinfo, vesseltraj),Set("mmsi","callsign"),Set(("speed",40,POSI)),(5,4))
```
### To select a user query 
* In `DataUtil.scala`, assigning a value to variable `query` to appoint the query which is going to use:<br>
```scala
val query:QueryService = query1
```
### To set services or service instances manually
* In `DataUtil.scala`, create services by providing such parameters:`servicename`, `dataconstraints`, `timewindow`. For example:<br>
```scala
val S1 = new QueryService("S1",Set(vesseltraj),Set(("speed",NEGA,30)),(5,2))
```
* Create service instances by providing such parameters:`servicename`, `outputattributes`, `dataconstraints`, `timewindow`. For example:<br>
```scala
val S2 = new QueryService("S2",Set(vesseltraj,vesselinfo), Set("mmsi", "draught", "speed"),Set(("imo",NEGA,2000)),(5,2))
```
* Load these to a service collection `ucServices` as a use case service source:<br>
```scala
val ucServices = List(S1,S2)
```
### To set services or service instances by simulating
* In `DataUtil.scala`, create the relation collection `simulSource` which is used to simulate the service source:<br>
```scala
val simulSource:Array[QueryService] = Array[QueryService](Movie,Revenues,Director,vesseltraj,vesseltravelinfo,vesselinfo)
```
* Simulate the service source by giving such parameters:`relationcollection`, `query`, `servicesourcesize`. For example:<br>
```scala
val simuServices:List[QueryService] = SourceSImulation.geneViews(DataUtil.simulSource,query,1000)
```
### To run the driver program
* appoint an existing service collection 
* call the query from DataUtil.scala

### To analysis result
* user
