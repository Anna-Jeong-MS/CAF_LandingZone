# HoL1 Monitoring

Landing Zone에서 관리 디자인 Area

[https://learn.microsoft.com/ko-kr/azure/cloud-adoption-framework/ready/landing-zone/design-area/management-platform](https://learn.microsoft.com/ko-kr/azure/cloud-adoption-framework/ready/landing-zone/design-area/management-platform)

## 시나리오

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled.png)

Microsoft Azure에는 다양한 모니터링 및 보안 관제 서비스가 존재합니다. 각각의 서비스에 대해 알아보고 Landing Zone 구성에 맞는 모니터링과 보안 관제를 구성합니다.  

## 아키텍쳐 구성 요소

HoL1에 진행하였던 Hub-Spoke 에 있는 리소스들에 대해 모니터링을 진행해보며 시나리오에 맞게 모니터링 및 보안 관제를 구성합니다. 

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%201.png)

**Data Source** → 실제로 모니터링하려는 리소스들로, Azure에 있는 리소스들뿐만 아니라 AWS, GCP 리소스 혹은 온프레미스에 있는 리소스도 가능합니다. 

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%202.png)

**Data Platform** → 데이터들은 다양한 방식을 사용해 수집되며

- 코드에서 수집 : Application Insight SDK
- Agent : Agent를 설치하여 구성
- Internal : 사용자 구성 없이 Azure Monitor에 자동으로 전송

 각각의 데이터들은(Metric, Logs, Tracs, Changes) 각각에 맞는 저장소에 저장됩니다.

**Data Consumption** → 해당 데이터를 이용해 인사이트를 얻으며 필요에 따라 시각화나 분석화를 진행합니다. 또한 응답에 따라 알람을 받거나 특정 논리 구성에 따라 진행 될 수 있게 합니다. 

가장 기본적인 모니터링은 Azure Portal에서 확인할 수 있으며 

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%203.png)

크게 4가지의 방식으로 모니터링을 진행할 수 있습니다. 

**1) Insight**

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%204.png)

- Application Insights : Application Insights의 경우 Azure Monitor를 활용하여 Application 에 대한 자세한 인사이트를 제공합니다.
    - 모니터링 : 웹 애플리케이션의 가용성, 성능 및 사용량, 오류 진단
- Container Insight : Container Insight의 경우 Kubernetes 클러스터에 배포되는 워크로드에 대한 성능 가시성을 제공합니다.
    - 모니터링 : 컨트롤러, 노드 및 컨테이너 로그 및 매트릭 수집
    - Linux용 Log Analytics 에이전트의 컨테이너화된 버전을 통해 자동으로 수집됨
- VM Insight
    - Windows 및 Linux VM성능과 상태를 분석
- Network Insight
    - 다른 구성 없이 모든 네트워크 리소스에 대한 토폴로지를 시각으로 표현
    - 연결 모니터 (네트워크 리소스 연결에 대한 상태 모니터링) NSG 및 Traffic Analytics에 대한 네트워크 모니터링 기능에 액세스 가능

**2) 분석 (Analysis)**

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%205.png)

- 사용자의 필요에 따라 데이터들을 Query 할 수 있습니다.
    
    ex) CPU 사용률이 자주 70%가 넘었던 VM들, 사용자의 로그인 횟수
    

3**) 시각화 (Visualize)**

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%206.png)

- 차트 및 표 시각화는 모니터링 데이터를 요약해서 보여주는 효과적인 도구 입니다.
- Workbooks과 Dashboard의 경우 Azure에서 제공되는 서비스이며 PowerBI는 마이크로소프트 제품이며 Grafana는 그라파나랩에서 개발된 제품입니다. 하지만 두 제품 모두 Azure Monitor와 함꼐 사용할 수 있는 강력한 툴입니다.

**4) 대응 (Respond)**

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%207.png)

모니터링 결과에 따라 사전에 대응합니다. 예를 들어 관리자에게 모니터링상 이슈가 발생되었을때 SMS나 Email을 전송하고 아니면 자동적으로 리소스들을 구성합니다. (Auto-Scale)

## 도전 과제

앞선 네가지 모니터링 부분에 대해 도전 과제를 진행하며 서비스에 대한 이해도를 높혀봅니다. 🙂  HoL1에 사용하였던 Hub-Spoke 리소스들을 기반으로 모니터링을 진행합니다. 

순서는 동일하게 1) Insight → 2)분석 → 3)시각화 → 4) 대응으로 진행합니다. 

**Landing Zone 고려 사항** 

- 경고 알림이 필요한 팀은 어디인가요?
- 여러 팀에게 알려야 하는 서비스 그룹이 있나요?
- 경고를 보내야 하는 기존 서비스 관리 도구가 있나요?
- 비즈니스에 중요한 것으로 간주되고 우선 순위가 높은 알림이 필요한 서비스는 무엇인가요?

**1) Insight**

- 각각의 Metric를 확인합니다. 또한 어떤 매트릭을 지원하는지도 함께 알아봅니다.
    
    참고 url : [https://learn.microsoft.com/ko-kr/azure/azure-monitor/essentials/metrics-supported](https://learn.microsoft.com/ko-kr/azure/azure-monitor/essentials/metrics-supported) 
    
    ![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%208.png)
    
    [리소스 → 매트릭]
    
- 리소스 그룹에 잠금을 사용하여 리소스가 실수로 삭제하지 않도록 합니다.

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%209.png)

[리소스 그룹 → 잠금]

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2010.png)

**2) 분석 (Analyze)**

- Log Analytics를 구성하여 Query를 작성해봅니다.

1) 리소스 그룹 생성

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2011.png)

2) Log Analytics Workspace 추가 

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2012.png)

- Log Analytics 작업 영역 생성

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2013.png)

- Log Analytics 전체 화면

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2014.png)

- 각각의 VM에 Log Analytics Agent 설치

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2015.png)

[Log Analytics 시작 → 데이터 원본에 연결]

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2016.png)

- 로그인에 실패한 사용자는? 특정 audit log 검색해보기

참고 URL : [https://learn.microsoft.com/en-us/azure/azure-monitor/logs/queries](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/queries)

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2017.png)

**3) 시각화 (Visualize)**

- Workbook 혹은 대시 보드를 이용하여 필요한 매트릭으로 대시보드를 구성합니다.

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2018.png)

매트릭 → 대시보드에 저장 → 대시보드에 고

- Azure Managed Grafana를 이용하여 대시보드를 구성해봅니다. (Optional)
    - 참고 URL : [https://learn.microsoft.com/ko-kr/azure/managed-grafana/quickstart-managed-grafana-portal](https://learn.microsoft.com/ko-kr/azure/managed-grafana/quickstart-managed-grafana-portal)

**4) 대응 (Respond)**

- 알람을 설정하여 특정 알람이 발생할 경우 Email과 Azure Mobile app에 알람이 오게 구성합니다.
    
    특정 알람은 분석에 썼던 Query를 이용해도 좋고 Azure가 기본적으로 제공하는 알람 구성이여도 됩니다. 
    
    - 참고 URL : [https://learn.microsoft.com/ko-kr/azure/azure-monitor/alerts/action-groups](https://learn.microsoft.com/ko-kr/azure/azure-monitor/alerts/action-groups)

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2019.png)

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2020.png)

경고 → 만들기 → 경고 규칙 만들기 

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2021.png)

경고 규칙 만들기

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2022.png)

작업 그룹 만들기 → 메일 

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2023.png)

- 생성 후 테스트 진행

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2024.png)

[모니터 → 작업 그룹]

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2025.png)

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2026.png)

작업 그룹 → 테스트 → 결과 확인하기

![Untitled](HoL1%20Monitoring%204d3c8042c01e4737b65478eb5037fa09/Untitled%2027.png)

## 참고 URL

- Landing Zone : [https://learn.microsoft.com/ko-kr/azure/cloud-adoption-framework/ready/landing-zone/design-area/management-platform](https://learn.microsoft.com/ko-kr/azure/cloud-adoption-framework/ready/landing-zone/design-area/management-platform)
- Azure 모니터 : [https://learn.microsoft.com/ko-kr/azure/azure-monitor/overview](https://learn.microsoft.com/ko-kr/azure/azure-monitor/overview)
- Azure Log Analytics : [https://learn.microsoft.com/ko-kr/azure/azure-monitor/logs/log-analytics-tutorial](https://learn.microsoft.com/ko-kr/azure/azure-monitor/logs/log-analytics-tutorial)
- Azure 경고 : [https://learn.microsoft.com/ko-kr/azure/azure-monitor/alerts/alerts-overview](https://learn.microsoft.com/ko-kr/azure/azure-monitor/alerts/alerts-overview)