# AWS

## Amazon EC2 요금(CPU)

<https://aws.amazon.com/ko/ec2/pricing/on-demand/>
온디맨드 요금

### 컴퓨팅 최적화(권장)

|컴퓨팅 최적화|vCPU|ECU|메모리(GiB)|인스턴스 스토리지(GB)|사용량가격|
|:--:|:--:|:--:|:--:|:--:|:--:|
|c4.4xlarge|16|62|30|EBS 전용|$0.796 시간당|
|c4.8xlarge|36|132|60|EBS 전용|$1.591 시간당|

### 범용

|컴퓨팅 최적화|vCPU|ECU|메모리(GiB)|인스턴스 스토리지(GB)|사용량가격|
|:--:|:--:|:--:|:--:|:--:|:--:|
|m4.4xlarge|16|53.5|64|EBS 전용|$0.8 시간당|
|m4.10xlarge|40|124.5|160|EBS 전용|$2 시간당|

## Amazon Storage 요금

Amazon EBS 범용 SSD(gp2) 볼륨

- $0.114 프로비저닝된 스토리지의 월별 GB당

Amazon EBS 프로비저닝된 IOPS SSD(io1) 볼륨

- $0.128 프로비저닝된 스토리지의 월별 GB당
- $0.067 프로비저닝된 월별 IOPS당

Amazon EBS 처리량 최적화 HDD(st1) 볼륨

- $0.051 프로비저닝된 스토리지의 월별 GB당

Amazon EBS 콜드 HDD(sc1) 볼륨

- $0.029 프로비저닝된 스토리지의 월별 GB당

Amazon S3에 대한 Amazon EBS 스냅샷

- $0.05 저장된 데이터의 월별 GB당
