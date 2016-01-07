

#RDS MySQL sysbench testing


## Resource 

One EC2 Instance type  c4.xlarge

One RDS MySQL

Details:db.t2.small
TypeType	Micro Instance - Current Generation
vCPUNumber of virtual cores	1 vCPU
MemoryMemory	2 GiB
EBS Optimized	No
Network Performance	Low
Free Tier Eligible	No
Instance Class db.t2.small
Storage Type	General Purpose (SSD)
IOPS	disabled
Storage	10 GB


##Prepare data
[ec2-user@ip-172-31-28-112 ~]$ sysbench --test=oltp --oltp-table-size=1000000 --mysql-db=wordpress  --mysql-user=awsuser --mysql-password=smartvm123 --mysql-host=mysql-test.c15kknsksijg.ap-northeast-1.rds.amazonaws.com --db-driver=mysql  prepare
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Creating table 'sbtest'...
Creating 1000000 records in table 'sbtest'...

##The first run
[ec2-user@ip-172-31-28-112 ~]$ sysbench --test=oltp --oltp-table-size=1000000 --mysql-db=wordpress  --mysql-user=awsuser --mysql-password=smartvm123 --mysql-host=mysql-test.c15kknsksijg.ap-northeast-1.rds.amazonaws.com --db-driver=mysql  --max-time=60 --oltp-read-only=on --max-requests=0 --num-threads=8 run
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 8

Doing OLTP test.
Running mixed OLTP test
Doing read-only test
Using Special distribution (12 iterations,  1 pct of values are returned in 75 pct cases)
Using "BEGIN" for starting transactions
Using auto_inc on the id column
Threads started!
Time limit exceeded, exiting...
(last message repeated 7 times)
Done.

OLTP test statistics:
    queries performed:
        read:                            516796
        write:                           0
        other:                           73828
        total:                           590624
    transactions:                        36914  (615.19 per sec.)
    deadlocks:                           0      (0.00 per sec.)
    read/write requests:                 516796 (8612.59 per sec.)
    other operations:                    73828  (1230.37 per sec.)

Test execution summary:
    total time:                          60.0047s
    total number of events:              36914
    total time taken by event execution: 479.9336
    per-request statistics:
         min:                                  9.12ms
         avg:                                 13.00ms
         max:                                158.15ms
         approx.  95 percentile:              16.56ms

Threads fairness:
    events (avg/stddev):           4614.2500/2.44
    execution time (avg/stddev):   59.9917/0.00

[ec2-user@ip-172-31-28-112 ~]$


##Secend run
[ec2-user@ip-172-31-28-112 ~]$ sysbench --test=oltp --oltp-table-size=1000000 --mysql-db=wordpress  --mysql-user=awsuser --mysql-password=smartvm123 --mysql-host=mysql-test.c15kknsksijg.ap-northeast-1.rds.amazonaws.com --db-driver=mysql  --max-time=60 --oltp-read-only=on --max-requests=0 --num-threads=8 run
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 8

Doing OLTP test.
Running mixed OLTP test
Doing read-only test
Using Special distribution (12 iterations,  1 pct of values are returned in 75 pct cases)
Using "BEGIN" for starting transactions
Using auto_inc on the id column
Threads started!
Time limit exceeded, exiting...
(last message repeated 7 times)
Done.

OLTP test statistics:
    queries performed:
        read:                            537922
        write:                           0
        other:                           76846
        total:                           614768
    transactions:                        38423  (640.10 per sec.)
    deadlocks:                           0      (0.00 per sec.)
    read/write requests:                 537922 (8961.33 per sec.)
    other operations:                    76846  (1280.19 per sec.)

Test execution summary:
    total time:                          60.0270s
    total number of events:              38423
    total time taken by event execution: 480.1078
    per-request statistics:
         min:                                  9.70ms
         avg:                                 12.50ms
         max:                                162.36ms
         approx.  95 percentile:              14.79ms

Threads fairness:
    events (avg/stddev):           4802.8750/1.96
    execution time (avg/stddev):   60.0135/0.00


[ec2-user@ip-172-31-28-112 ~]$ sysbench --test=oltp --oltp-table-size=1000000 --mysql-db=wordpress  --mysql-user=awsuser --mysql-password=smartvm123 --mysql-host=mysql-test.c15kknsksijg.ap-northeast-1.rds.amazonaws.com --db-driver=mysql  --max-time=60 --oltp-read-only=on --max-requests=0 --num-threads=8 run
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 8

Doing OLTP test.
Running mixed OLTP test
Doing read-only test
Using Special distribution (12 iterations,  1 pct of values are returned in 75 pct cases)
Using "BEGIN" for starting transactions
Using auto_inc on the id column
Threads started!
Time limit exceeded, exiting...
(last message repeated 7 times)
Done.

OLTP test statistics:
    queries performed:
        read:                            537922
        write:                           0
        other:                           76846
        total:                           614768
    transactions:                        38423  (640.10 per sec.)
    deadlocks:                           0      (0.00 per sec.)
    read/write requests:                 537922 (8961.33 per sec.)
    other operations:                    76846  (1280.19 per sec.)

Test execution summary:
    total time:                          60.0270s
    total number of events:              38423
    total time taken by event execution: 480.1078
    per-request statistics:
         min:                                  9.70ms
         avg:                                 12.50ms
         max:                                162.36ms
         approx.  95 percentile:              14.79ms

Threads fairness:
    events (avg/stddev):           4802.8750/1.96
    execution time (avg/stddev):   60.0135/0.00



##Testing on read replica
[ec2-user@ip-172-31-28-112 ~]$ sysbench --test=oltp --oltp-table-size=1000000 --mysql-db=wordpress  --mysql-user=awsuser --mysql-password=smartvm123 --mysql-host=wordpress.c15kknsksijg.ap-northeast-1.rds.amazonaws.com  --db-driver=mysql  --max-time=60 --oltp-read-only=on --max-requests=0 --num-threads=8 run
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 8

Doing OLTP test.
Running mixed OLTP test
Doing read-only test
Using Special distribution (12 iterations,  1 pct of values are returned in 75 pct cases)
Using "BEGIN" for starting transactions
Using auto_inc on the id column
Threads started!
Time limit exceeded, exiting...
(last message repeated 7 times)
Done.

OLTP test statistics:
    queries performed:
        read:                            185374
        write:                           0
        other:                           26482
        total:                           211856
    transactions:                        13241  (220.57 per sec.)
    deadlocks:                           0      (0.00 per sec.)
    read/write requests:                 185374 (3088.03 per sec.)
    other operations:                    26482  (441.15 per sec.)

Test execution summary:
    total time:                          60.0298s
    total number of events:              13241
    total time taken by event execution: 480.0804
    per-request statistics:
         min:                                 33.97ms
         avg:                                 36.26ms
         max:                                386.30ms
         approx.  95 percentile:              42.25ms

Threads fairness:
    events (avg/stddev):           1655.1250/2.37
    execution time (avg/stddev):   60.0101/0.01

