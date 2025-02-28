[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: SHI, Fangzhe
### Student Id: 21083677
### Email: fshiae@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    **I utilized the Phoronix Test Suite to conduct performance measurements, focusing on both CPU and memory performance. To evaluate CPU performance, I measured metrics such as compression rate and decompression rate. These metrics were chosen because they effectively demonstrate the CPU's ability to handle data-intensive tasks, reflecting its computational efficiency in real-world scenarios. For memory performance, I assessed the floating-point copy rate, which provides insight into how efficiently the system manages calculations and data transfers, particularly in tasks involving floating-point operations.** 
    
    **To quantify CPU performance, I measured MIPS. This metric is a direct reflection of the CPU's processing power. For memory performance, I used MB/s to evaluate the speed at which the system can process and transfer data.**

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro`  |    3596/3078    |     10581.71       |
    | `t2.medium` |    9764/5833    |     19064.6        |
    | `c5d.large` |    7437/4904    |     13385.16       |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

    **Overall, I believe that as the number of CPUs and the amount of memory increase, the performance of EC2 instances should also improve. However, the data from the three instances tested does not clearly demonstrate this. Between t2.micro and t2.medium, the CPU count increases from 1 to 2, and the memory size increases from 957 MiB to 3.7 GiB, resulting in a significant performance improvement, which aligns with the expected conclusion. However, when comparing t2.medium to c5d.large, the CPU count and memory size remain unchanged, but the CPU model is changed from E5 2686 v4 to Platinum 8124M. I believe this change in CPU model is the main reason for the performance fluctuation. Therefore, the data from these two instances cannot directly demonstrate the relationship between performance and CPU count or memory size.**


## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | Local RTT (ms) | Public RTT (ms) |
    | ------------------------- | -------------- | -------------- | --------------- |
    | `t3.medium` - `t3.medium` | 4229.12        | 0.172          | 0.223           |
    | `m5.large` - `m5.large`   | 5068.8         | 0.164          | 0.232           |
    | `c5n.large` - `c5n.large` | 5079.04        | 0.202          | 0.199           |
    | `t3.medium` - `c5n.large` | 2293.76        | 0.801          | 0.764           |
    | `m5.large` - `c5n.large`  | 5079.04        | 0.167          | 0.212           |
    | `m5.large` - `t3.medium`  | 2775.04        | 0.578          | 0.715           |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

    **The network performance between different EC2 instance types shows variations in TCP bandwidth, Local RTT, and Public RTT. Instances of the same type, like t3.medium to t3.medium, exhibit low RTT and high TCP bandwidth, indicating efficient communication. When comparing different types, such as t3.medium to c5n.large, there is a noticeable increase in RTT and a decrease in bandwidth, suggesting that different instance configurations affect network performance. An exception to this trend is m5.large to c5n.large, where the TCP bandwidth remains the same as c5n.large to c5n.large, but Local RTT is lower than expected, which could indicate specific optimizations in the network configurations between these types. Public RTT is generally higher than local RTT due to the additional latency involved in internet routing. Overall, the data shows that performance varies based on the instance types.**



2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | Public RTT (ms) | Local RTT (ms) |
    | ------------------------- | -------------- | --------------- | -------------- |
    | N. Virginia - Oregon      | 31.4           | 62.968          |                |
    | N. Virginia - N. Virginia | 4679.68        | 0.220           | 0.170          |
    | Oregon - Oregon           | 4874.24        | 0.163           | 0.179          |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

    **The network performance between EC2 instances deployed in different regions shows significant differences. When comparing N. Virginia to Oregon, the TCP bandwidth is much lower and the public RTT is higher, indicating the impact of cross-region communication. Within the same region, such as N. Virginia to N. Virginia, the TCP bandwidth is much higher, and the RTT is also lower. It confirms that instances within the same region offer better network performance compared to cross-region communication.**
