# CPU Metrics

The **cpu** input plugin, measures the CPU usage of a process or the whole system by default \(considering per CPU core\). It reports values in percentage unit for every interval of time set. At the moment this plugin is only available for Linux.

The following tables describes the information generated by the plugin. The keys below represent the data used by the overall system, all values associated to the keys are in a percentage unit \(0 to 100%\):

The CPU metrics plugin creates metrics that are log-based \(I.e. JSON payload\). If you are looking for Prometheus-based metrics please see the Node Exporter Metrics input plugin. 

| key | description |
| :--- | :--- |
| cpu\_p | CPU usage of the overall system, this value is the summatory of time spent on user and kernel space. The result takes in consideration the numbers of CPU cores in the system. |
| user\_p | CPU usage in User mode, for short it means the CPU usage by user space programs. The result of this value takes in consideration the numbers of CPU cores in the system. |
| system\_p | CPU usage in Kernel mode, for short it means the CPU usage by the Kernel. The result of this value takes in consideration the numbers of CPU cores in the system. |

In addition to the keys reported in the above table, a similar content is created **per** CPU core. The cores are listed from _0_ to _N_ as the Kernel reports:

| key | description |
| :--- | :--- |
| cpu**N**.p\_cpu | Represents the total CPU usage by core **N**. |
| cpu**N**.p\_user | Total CPU spent in user mode or user space programs associated to this core. |
| cpu**N**.p\_system | Total CPU spent in system or kernel mode associated to this core. |

## Configuration Parameters

The plugin supports the following configuration parameters:

| Key | Description | Default |
| :--- | :--- | :--- |
| Interval\_Sec | Polling interval in seconds | 1 |
| Interval\_NSec | Polling interval in nanoseconds | 0 |
| PID | Specify the ID \(PID\) of a running process in the system. By default the plugin monitors the whole system but if this option is set, it will only monitor the given process ID. |  |

## Getting Started

In order to get the statistics of the CPU usage of your system, you can run the plugin from the command line or through the configuration file:

### Command Line

```bash
$ build/bin/fluent-bit -i cpu -t my_cpu -o stdout -m '*'
Fluent Bit v1.x.x
* Copyright (C) 2019-2020 The Fluent Bit Authors
* Copyright (C) 2015-2018 Treasure Data
* Fluent Bit is a CNCF sub-project under the umbrella of Fluentd
* https://fluentbit.io

[2019/09/02 10:46:29] [ info] starting engine
[0] [1452185189, {"cpu_p"=>7.00, "user_p"=>5.00, "system_p"=>2.00, "cpu0.p_cpu"=>10.00, "cpu0.p_user"=>8.00, "cpu0.p_system"=>2.00, "cpu1.p_cpu"=>6.00, "cpu1.p_user"=>4.00, "cpu1.p_system"=>2.00}]
[1] [1452185190, {"cpu_p"=>6.50, "user_p"=>5.00, "system_p"=>1.50, "cpu0.p_cpu"=>6.00, "cpu0.p_user"=>5.00, "cpu0.p_system"=>1.00, "cpu1.p_cpu"=>7.00, "cpu1.p_user"=>5.00, "cpu1.p_system"=>2.00}]
[2] [1452185191, {"cpu_p"=>7.50, "user_p"=>5.00, "system_p"=>2.50, "cpu0.p_cpu"=>7.00, "cpu0.p_user"=>3.00, "cpu0.p_system"=>4.00, "cpu1.p_cpu"=>6.00, "cpu1.p_user"=>6.00, "cpu1.p_system"=>0.00}]
[3] [1452185192, {"cpu_p"=>4.50, "user_p"=>3.50, "system_p"=>1.00, "cpu0.p_cpu"=>6.00, "cpu0.p_user"=>5.00, "cpu0.p_system"=>1.00, "cpu1.p_cpu"=>5.00, "cpu1.p_user"=>3.00, "cpu1.p_system"=>2.00}]
```

As described above, the CPU input plugin gathers the overall usage every one second and flushed the information to the output on the fifth second. On this example we used the **stdout** plugin to demonstrate the output records. In a real use-case you may want to flush this information to some central aggregator such as [Fluentd](http://fluentd.org) or [Elasticsearch](http://elastic.co).

### Configuration File

In your main configuration file append the following _Input_ & _Output_ sections:

```python
[INPUT]
    Name cpu
    Tag  my_cpu

[OUTPUT]
    Name  stdout
    Match *
```
