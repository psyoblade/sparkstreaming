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
* Linking
* Initializing StreamingContext
* Discretized Streams (DStreams)
* Input DStreams and Receivers
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
