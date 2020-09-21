# Tuning Splunk when max concurrent searches are reached

<https://www.rfaircloth.com/2017/12/12/tuning-splunk-when-max-concurrent-searches-are-reached/>

검색이 대기 중이지만 여분의 코어, 메모리 및 IO가 있습니까? 제한을 조정하면 확장 인스턴스를 사용중인 경우 Splunk에서 하드웨어를 "더 많이"활용

## Warnings

이 방법은 검색이 LONG을 실행할 때 유용 하지 않습니다 . 데이터 모델 가속, 요약 및보고 검색과 같은 정기 검색이 예상/필요 시간 제한 내에 완료되지 않으면이 정보가 증상을 악화

이 접근 방식은 데이터 모델 가속화, 요약 및보고에 필요한 시간보다 일관되게 검색이 빠르게 실행되고 CPU, 메모리, 스토리지 IOPS, 스토리지 대역폭의 이용률 이 인프라 의 검증된 용량 보다 훨씬 낮은 경우 추가 검색이 대기되는 경우에 유용

## Details

Splunk의 모든 특정 버전에서 먼저 다음 설정을 적용하여 검색 초기화 속도를 늦출 수있는 기능을 비활성화합니다.

$SPLUNK_HOME/etc/local/limits.conf
$SPLUNK_HOME/etc/master-apps/_cluster/local/limits.conf

```properties
[search]
#Splunk version >=6.5.0 <6.5.6
#Splunk version >=6.6.0 <6.6.3
#Not required >7.0.0
#SPL-136845 Review future release notes to determine if this can be reverted to auto
max_searches_per_process = 1
```

DMA가 사용되는 검색헤드 (ES)에서만 다음을 업데이트

$SPLUNK_HOME/etc/local/limits.conf

```properties
# 이 기능은 임시로 여유가 있지만 검색을 건너 뛰는 경우 (ES보고 있습니다) 또는 기타 집이나 비슷한 것들
[scheduler]
max_searches_perc = 75
auto_summary_perc = 100
```

메모리, CPU 사용 및 사용된 메모리를 포함하여 검색 헤드 및 인덱서의로드 백분율을 평가.
base_max_searches의 값을 10 씩 증가시켜 다음 중 하나가 발생할 때까지 SH 당 더 많은 동시 검색을 허용 할 수 있음

IDX 또는 SH에서 CPU 또는 메모리 사용률은 60%
IOPS 또는 스토리지 처리량이 상한에 도달하고 더 이상 증가하지 않음 시스템은 base_max_searches 값의 예상치 못한 로드 감소로 인한 실패를 방지하기 위해 시스템을 완전히 활용하고 IOPS가 더 이상 일정하지 않은 지 확인
건너뛰기/대기열이 더 이상 발생하지 않습니다(이 지점에서 "헤드룸"을 제공하기 위해 1~3개의 추가 단위 증가)

```properties
#limits.conf set SH only
[search]
#base value is 6 increase by 10 until utilization on IDX or SH is at 60% CPU/memory starting with 20
# IDX 또는 SH의 사용률이 20으로 시작하는 CPU/메모리 60 %가 될 때까지 #base값은 6 ~10씩 증가
#base_max_searches = TBD
```

## 예제

Splunk Monitoring 콘솔에 동일한 오류가 표시됩니다. 우리는이 두 인덱서 모두와 통신하는 다른 VM에 prod 및 non-prod 및 Distributed 검색 헤드를위한 별도의 인덱서를 보유

두 인덱서 모두 limit.conf에 정의 된 동시 검색 값을 확인

- max_searches_per_cpu
- base_max_searches
- max_rt_search_multiplier

Prod 인덱서에는 18개의 CPU코어  Non Prod Indexer에는 8개의 CPU코어

따라서 resources에 따라 위의 값을
Prod에서 업데이트 할 수 있습니다 :

```properties
max_searches_per_cpu = 18
base_max_searches = 8
max_rt_search_multiplier = 4

비 Prod에서 :
max_searches_per_cpu = 8
base_max_searches = 5
max_rt_search_multiplier = 4
```
