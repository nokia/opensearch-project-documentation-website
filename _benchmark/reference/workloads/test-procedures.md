---
layout: default
title: test_procedures
parent: Workload reference
grand_parent: OpenSearch Benchmark Reference
nav_order: 100
---

# test_procedures

If your workload only defines one benchmarking scenario, specify the schedule on top-level. Use the `test-procedures` element to specify additional properties, such as a name or description. A test procedure is like a benchmarking scenario. If you have multiple test procedures, you can define a variety of challenges.

The following table lists test procedures for the benchmark scenarios in this dataset. A test procedure can reference all operations that are defined in the operations section.

Parameter | Required | Type | Description
:--- | :--- | :--- | :---
`name` | Yes | String | The name of the test procedure. When naming the test procedure, do not use spaces so that the name is easy to enter on the command line.
`description` | No | String |  A human readable description of the test procedure.
`user-info` | No | String | Outputs a message at the start of the test to notify user about important test-related information, for example, deprecations.
`default` | No | Boolean | When set to `true`, selects the default test procedure if the user did not specify a test procedure on the command line. If the workload only defines one test procedure, it is implicitly selected as default. Otherwise, you must define `"default": true` on exactly one challenge.
[`schedule`](#Schedule) | Yes | Array |  Defines the order in which tasks in the workload are run.


## schedule

The `schedule` element contains a list of a tasks, which are operations supported by OpenSearch Benchmark, that are run by the workload during the benchmark test. 

### Usage

The `schedule` element defines tasks using the following methods described in this section.

#### Using the operations element

The following example defines a `force-merge` and `match-all` query task using the `operations` element. The `force-merge` operation does not use any parameters, so only the `name` and `operation-type` are needed. The `match-all-query` parameter requires a query `body` and `operation-type`. 

Operations defined in the `operations` element can be reused more than once in the schedule:

```yml
{
  "operations": [
    {
      "name": "force-merge",
      "operation-type": "force-merge"
    },
    {
      "name": "match-all-query",
      "operation-type": "search",
      "body": {
        "query": {
          "match_all": {}
        }
      }
    }
  ],
  "schedule": [
    {
      "operation": "force-merge",
      "clients": 1
    },
    {
      "operation": "match-all-query",
      "clients": 4,
      "warmup-iterations": 1000,
      "iterations": 1000,
      "target-throughput": 100
    }
  ]
}
```

#### Defining operations inline

If you don't want reuse an operation in the schedule, you can define operations inside the `schedule` element, as shown in the following example:

```yml
{
  "schedule": [
    {
      "operation": {
        "name": "force-merge",
        "operation-type": "force-merge"
      },
      "clients": 1
    },
    {
      "operation": {
        "name": "match-all-query",
        "operation-type": "search",
        "body": {
          "query": {
            "match_all": {}
          }
        }
      },
      "clients": 4,
      "warmup-iterations": 1000,
      "iterations": 1000,
      "target-throughput": 100
    }
  ]
}
```

### Task options

Each task contains the following options.

Parameter | Required | Type | Description
:--- | :--- | :--- | :---
`operation` | Yes | List | Refers to either the name of an operation, defined in the `operations` element, or includes the entire operation inline.
`name` | No | String | Specifies a unique name for the task when multiple tasks use the same operation.
`tags` | No | String | Unique identifiers that can be used to filter between `tasks.clients`, or the number of clients that should execute a task concurrently. Default is 1.
`clients` | No | Integer | Specifies the number of clients that concurrently run the task. Default is `1`.

### Target options

OpenSearch Benchmark requires one of the following options when running a task:

`target-throughput` | No | Integer | Defines the benchmark mode. When not defined, OpenSearch Benchmark assumes it is a throughput benchmark and runs the task as fast as possible. This is useful for batch operations, where achieving best throughput is preferred over better latency. When defined, the target specifies the number of requests per second over all clients. For example, if you specify `target-throughput: 1000` with eight clients, each client issues 125 (= 1000 / 8) requests per second. 
`target-interval` | No | Interval | Defines an interval of less 1 divided by the target-throughput (in seconds) less than one operation per second. Define either target-throughput or target-interval but not both (otherwise Rally will raise an error).
`ignore-response-error-level` | No | Boolean | Controls whether to ignore errors encountered during the task when a benchmark is run with the `on-error=abort` command flag. 

### Iteration-based options

Iteration-based options determine the number of times an operation should run. It can also define the number of iterative runs when tasks are run in [parallel](#parallel-tasks). To configure an iteration-based schedule, use the following options.


Parameter | Required | Type | Description
:--- | :--- | :--- | :---
`iterations` | No | Integer | Specifies the number of times a client should execute an operation. These are included in the measured results. Default is `1`. 
`warmup-iterations` | No | Integer | Specifies the number of times a client should execute an operation for warming up the benchmark candidate. The `warmup-iterations` do not appear in the measurement results. Default is `0`.

### Parallel tasks

The `parallel` element runs tasks wrapped inside the element concurrently. 

When running tasks in parallel, each task requires the `client` option to make sure clients inside your benchmark are reserved for that task. Otherwise, when the `client` option is specified inside the `parallel` element without a connection to the task, the benchmark uses that number of clients for all tasks.

#### Usage

In the following example, `parallel-task-1` and `parallel-task-2` execute a `bulk` operation concurrently:

```yml
{
  "name": "parallel-any",
  "description": "Workload completed-by property",
  "schedule": [
    {
      "parallel": {
        "tasks": [
          {
            "name": "parellel-task-1",
            "operation": {
              "operation-type": "bulk",
              "bulk-size": 1000
            },
            "clients": 8
          },
          {
            "name": "parellel-task-2",
            "operation": {
              "operation-type": "bulk",
              "bulk-size": 500
            },
            "clients": 8
          }
        ]
      }
    }
  ]
}
```

#### Options

The `parallel` element supports all `schedule` parameters, in addition to the following:

`tasks` | Yes | Array | Defines a list of tasks that should be executed concurrently. 
`completed-by` | No | String | Allows you to define the name of one task in the tasks list or the value `any`. If `completed-by` is set to the name of one task in the list, the `parallel-task` structure is considered complete once that specific task has been completed. If `completed-by` is set to `any`, the `parallel-task` structure is considered complete when any of the tasks in the list has been completed. If `completed-by` is not explicitly defined, the `parallel-task` structure is considered complete as soon as all the tasks in the list have been completed.

### Time-based options

Time-based options determines the duration of time, in seconds, that operations should run for. This is ideal for batch-style operations which may require an additional warmup period.

To configure a time-based schedule, use the following options.

Parameter | Required | Type | Description
:--- | :--- | :--- | :---
`time-period` | No | Integer | Specifies the time period in seconds that OpenSearch Benchmark considers for measurement. This is not required for bulk indexing because OpenSearch Benchmark bulk indexes all documents and naturally measures all samples after the specified `warmup-time-period`.
`ramp-up-time-period` | No | Integer | Specifies the time period in seconds in which OpenSearch Benchmark gradually adds clients and reaches the total number of clients specified for the operation. 
`warmup-time-period` | No | Integer | Specifies the time period in seconds to warm up the benchmark candidate. All response data captured during the warmup period do not appear in the measurement results.
