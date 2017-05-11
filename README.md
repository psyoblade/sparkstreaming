# Spark Streaming Programming Guide
* [스파크 스트리밍:프로그래밍 가이드](http://spark.apache.org/docs/latest/streaming-programming-guide.html)

## Overview
 스파크 스트리밍은 데이터를 스트리밍을 통해 실시간 처리를 하기 위해, 입력 데이터 소스를 "Spark Streaming" 이라고 하는 인터페이스를 통해 임의의 크기의 단위로 배치처리가 가능하도록 쪼개어 주고 이를 "Spark Engine"을 통해 배치처리 결과를 출력 데이터로 내어주는 스파크 라이브러리 중의 하나이다.

## A Quick Example
 가장 간단한 wordcount 예제를 스트리밍으로 만들어보자 
```scala
import org.apache.spark._
import org.apache.spark.streaming._
val conf = new SparkConf().setMaster("local[2]").setAppName("SparkStreamingWordCount")
val ssc = new StreamingContext(conf, Seconds(1))
```
 임의의

## Basic Concepts
### Linking
### Initializing StreamingContext
* Using streaming context
```scala
import org.apache.spark._
import org.apache.spark.streaming._

val conf = new SparConf().setAppMaster("local").setAppName("streaming.test")
val ssc = new StreamingContext(conf, Seconds(1))
```
* Points to remember:
  * 스트리밍 컨텍스트가 한 번 생성되면, 해당 컨텍스트에 대한 재생성이나 추가는 안된다
  * 스트리밍 컨텍스트가 한 번 종료되면, 재시작될 수 없다.
  * 하나의 JVM 내에서 하나의 스트리밍 컨텍스트만 생성될 수 있다
  * 스트리밍 컨텍스트 stop() 수행 시에 기본적으로 SparkContext 객체까지 종료된다.
    * 단, stopSparkContext 값을 false 로 두게 되면 StreamingContext 를 생성할 수 있다
  * 고로 SparkContext 는 재사용이 가능하므로, StreamingContext.stop() 후에 다시 생성할 수 있다.
### Discretized Streams (DStreams)
 SparkStreaming에서 제공하는 연속적인 데이터 스트림을 표현하는 추상화 클래스이며, DStream은 RDD들의 continuous series 로 표현된다. 고로 RDD의 특성인 immutable, distributed dataset 등을 가진다.
 ![image](https://cloud.githubusercontent.com/assets/556238/5241760/e17ae9f4-78d9-11e4-86f5-6e168adcea87.png)
### Input DStreams and Receivers
 입력 데이터는 이산화된 스트림(DStream)으로 수신되는데 기본적으로 Kafka, Flume 이런 데이터 소스를 생각하기 마련인데 가장 기본적인 데이터소스는 FileSystem 혹은 SocketStream 이다. 물론 다양한 기능을 가진 Kafka, Flume, Kinesis 등이 유용하긴 하다
 이러한 DStream은 연속적인 데이터를 전달하는 인터페이스이며 이와 1:1로 Receiver가 존재해야 하며 이는 Receiver 당 1개의 core 할당을 필요로 한다. 또한 항상 데몬처럼 떠 있어야 하므로, 해당 core는 dedicated 된 리소스로 충분한 코어수가 확보될 필요가 있다.
* Points to remember
  * 로컬모드에서 스파크 스트리밍 실행 시에 Receiver(sockets, kafka, flume, etc)용 core 1개와 Batch 처리를 위한 core 1개, 최소 2개의 core가 확보되어야 하므로 local 혹은 local[2] 이상으로 타스크 수행이 필요하다
  * 클러스터모드에서 실행 시에도 원하는 리시버의 수보다 많은 수의 코어 수 할당이 되지 않으면 코어를 할당받지 못한 리시버는 실행될 수 없다.
* Basic Sources
  * FileStreams
      ```scala
      streamingContext.fileStream[KeyClass, ValueClass, InputFormatClass](dataDirectory)
      ```
    * 이동 혹은 파일명이 변경되는 파일에 대해서 모니터링 된다
    * 한 번 이동되었을 경우에 파일에 변화가 없을 것 (append 되는 데이터는 인지되지 않음.)
    * 대상 파일포맷이 모두 일치 할 것
  * Streams based on Custom Receivers
  * Queue of RDDs as a Stream 
* Custom Source
* Receiver Reliability


* Transformations on DStreams
* Output Operations on DStreams
* DataFrame and SQL Operations
* MLlib Operations
* Caching / Persistence
* Checkpointing
* Accumulators, Broadcast Variables, and Checkpoints
* Deploying Applications
* Monitoring Applications

## Performance Tuning
* Reducing the Batch Processing Times
* Setting the Right Batch Interval
* Memory Tuning

## Fault-tolerance Semantics
## Where to Go from Here
