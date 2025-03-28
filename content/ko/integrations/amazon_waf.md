---
aliases:
- /ko/integrations/awswaf/
categories:
- cloud
- security
- aws
- log collection
custom_kind: integration
dependencies: []
description: 허용된 요청 대 차단된 요청을 추적합니다.
doc_link: https://docs.datadoghq.com/integrations/amazon_waf/
draft: false
further_reading:
- link: https://www.datadoghq.com/blog/aws-waf-metrics/
  tag: 블로그
  text: AWS WAF 모니터링을 위한 주요 메트릭
- link: https://www.datadoghq.com/blog/aws-waf-monitoring-tools/
  tag: 블로그
  text: AWS WAF 데이터 수집 도구
- link: https://www.datadoghq.com/blog/aws-waf-datadog/
  tag: 블로그
  text: Datadog으로 AWS WAF 활동 모니터링
git_integration_title: amazon_waf
has_logo: true
integration_id: ''
integration_title: AWS WAF
integration_version: ''
is_public: true
manifest_version: '1.0'
name: amazon_waf
public_title: Datadog-AWS WAF 통합
short_description: 허용된 요청 대 차단된 요청을 추적합니다.
version: '1.0'
---

<!--  SOURCED FROM https://github.com/DataDog/dogweb -->
## 개요

AWS WAF는 일반적인 웹 공격으로부터 웹 애플리케이션을 보호하는 데 도움이 되는 웹 애플리케이션 방화벽입니다.

본 통합을 활성화하여 Datadog에서 WAF 메트릭을 확인하세요.

## 설정

### 설치

아직 설정하지 않은 경우 먼저 [Amazon Web Services 통합][1]을 설정하세요.

### 메트릭 수집

1. [AWS 통합 페이지][2]에서 `Metric Collection` 탭의 `WAF` 또는 `WAFV2`이 활성화되어 있는지 확인합니다. 이는 사용하는 엔드포인트에 따라 달라집니다.

2. [Datadog - AWS WAF 통합][3]을 설치합니다.

### 로그 수집

웹 애플리케이션 방화벽 감사 로그를 활성화하여 웹 ACL 분석 트래픽에 대한 자세한 정보를 얻어보세요.

#### WAF
1. 이름이 `aws-waf-logs-`로 시작하는 `Amazon Data Firehose`를 만듭니다.
2. `Amazon Data Firehose` 대상에서 `Amazon S3`을 선택한 다음 접두사 `waf`를 추가합니다.
3. 원하는 웹 ACL을 선택하고 새로 생성한 Firehose에 로그를 전송하도록 구성합니다([상세 단계][4]).

#### WAFV2
1. 이름이 `aws-waf-logs-`로 시작하는 `S3 bucket`를 만듭니다.
2. Amazon S3 버킷의 로깅 대상을 구성합니다([상세 단계][5]).

WAF/WAFV2 로그를 수집하고 지정한 S3 버킷으로 전송합니다.

#### Datadog로 로그 전송

1. 이미 설정하지 않은 경우 [Datadog Forwarder Lambda 함수][6]를 설정합니다.
2. Lambda 함수를 설치한 다음 AWS 콘솔에서 WAF 로그를 포함하는 S3 버킷에 트리거를 수동으로 추가합니다. Lambda 함수에서 다음 트리거 목록의 S3를 클릭합니다.
3. 트리거를 구성하려면 WAF 로그가 포함된 S3 버킷을 선택하고 이벤트 유형을 `Object Created (All)`로 변경합니다.
4. **Add**를 클릭합니다.

**참고**: 
- Datadog Lambda Forwarder는 쉽게 사용할 수 있도록 WAF 로그의 중첩된 오브젝트 어레이를 `key:value` 형식으로 자동 변환합니다.
- "Configurations on the same bucket cannot share a common event type(동일한 버킷의 구성은 공통 이벤트 유형을 공유할 수 없습니다)"의 오류 메시지가 나타난 경우, 버킷에 다른 Lambda Forwarder와 연결된 이벤트 알림이 없도록 다시 한번 확인하세요. S3 버킷에 여러 `All object create events` 인스턴스가 있어서는 안 됩니다.

## 수집한 데이터

### 메트릭
{{< get-metrics-from-git "amazon_waf" >}}


**참고**: WAF용 클라우드와치(CloudWatch) 메트릭 API의 과거 데이터 형식 때문에 `aws.waf.*` 및 `waf.*` 메트릭 모두 보고됩니다.

AWS에서 검색된 각 메트릭에는 AWS 콘솔에 나타나는 것과 동일한 태그가 할당됩니다, 호스트 이름, 보안 그룹 등을 포함하되 이에 국한되지 않습니다.

### 이벤트

AWS WAF 통합은 이벤트를 포함하지 않습니다.

### 서비스 점검

AWS WAF 통합은 서비스 점검을 포함하지 않습니다.

## 트러블슈팅

도움이 필요하신가요? [Datadog 지원팀][8]에 문의하세요.

[1]: https://docs.datadoghq.com/ko/integrations/amazon_web_services/
[2]: https://app.datadoghq.com/integrations/amazon-web-services
[3]: https://app.datadoghq.com/integrations/amazon-waf
[4]: https://docs.aws.amazon.com/waf/latest/developerguide/classic-logging.html
[5]: https://docs.aws.amazon.com/waf/latest/developerguide/logging.html
[6]: https://docs.datadoghq.com/ko/logs/guide/forwarder/
[7]: https://github.com/DataDog/dogweb/blob/prod/integration/amazon_waf/amazon_waf_metadata.csv
[8]: https://docs.datadoghq.com/ko/help/