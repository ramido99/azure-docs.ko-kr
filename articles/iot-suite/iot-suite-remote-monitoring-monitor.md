---
title: "원격 모니터링 솔루션에서 고급 모니터링 - Azure | Microsoft Docs"
description: "이 자습서에서는 원격 모니터링 솔루션 대시보드를 사용하여 장치를 모니터링하는 방법을 보여 줍니다."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 12/12/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 2e6d965d9177a61f974b319a1bd2155c9132533f
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/13/2017
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>원격 모니터링 솔루션을 사용하여 고급 모니터링 수행

이 자습서에서는 원격 모니터링 대시보드의 기능을 보여 줍니다. 이러한 기능을 소개하기 위해 자습서에서는 Contoso IoT 응용 프로그램에서 시나리오를 사용합니다.

이 자습서에서는 두 개의 시뮬레이트된 Contoso 트럭 장치를 사용하여 미리 구성된 솔루션 대시보드에서 장치를 모니터링하는 방법을 알아봅니다. Contoso 운영자로서 필드에서 트럭의 위치 및 동작을 모니터링해야 합니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

>[!div class="checklist"]
> * 대시보드에서 장치 필터링
> * 실시간 원격 분석 보기
> * 장치 세부 정보 보기
> * 장치에서 경보 보기
> * 시스템 KPI 보기

## <a name="prerequisites"></a>필수 조건

이 자습서를 수행하려면 Azure 구독에서 원격 모니터링 솔루션의 배포된 인스턴스가 필요합니다.

원격 모니터링 솔루션을 아직 배포하지 않은 경우 [미리 구성된 원격 모니터링 솔루션 배포](iot-suite-remote-monitoring-deploy.md) 자습서를 완료해야 합니다.

## <a name="choose-the-devices-to-display"></a>표시할 장치 선택

**대시보드** 페이지에 표시할 장치를 선택하려면 필터를 사용합니다. **트럭** 장치만 표시하려면 필터 드롭다운 목록에서 기본 제공 **트럭** 필터를 선택합니다.

![대시보드에서 트럭에 대한 필터](media/iot-suite-remote-monitoring-monitor/dashboardtruckfilter.png)

필터를 적용하는 경우 필터 조건과 일치하는 장치만 **대시보드** 페이지의 맵에 표시됩니다.

![맵에 트럭 표시](media/iot-suite-remote-monitoring-monitor/dashboardtruckmap.png)

필터는 또한 **원격 분석** 차트에 표시할 장치를 결정합니다.

![대시보드에 트럭 원격 분석 표시](media/iot-suite-remote-monitoring-monitor/dashboardtelemetry.png)

필터를 만들고, 편집하고 삭제하려면 **필터 관리**를 선택합니다.

## <a name="view-real-time-telemetry"></a>실시간 원격 분석 보기

미리 구성된 솔루션은 **대시보드** 페이지의 차트에 자세한 실시간 원격 분석 데이터를 그립니다. 원격 분석 차트는 현재 필터에 의해 선택된 장치에 대한 원격 분석 정보를 보여 줍니다.

![트럭 원격 분석 그림](media/iot-suite-remote-monitoring-monitor/dashboardtelemetryview.png)

볼 원격 분석 값을 선택하려면 차트의 위쪽에서 원격 분석 유형을 선택합니다.

![트럭 원격 분석 그림](media/iot-suite-remote-monitoring-monitor/dashboardselecttelemetry.png)

라이브 원격 분석 표시를 일시 중지하려면 **흐름**을 선택합니다. 라이브 표시를 다시 활성화하려면 **일시 중지**를 선택합니다.

![원격 분석 표시 일시 중지 및 다시 시작](media/iot-suite-remote-monitoring-monitor/dashboardtelemetrypause.png)

## <a name="use-the-map"></a>맵 사용

맵은 현재 필터에 의해 선택된 시뮬레이트된 트럭에 대한 정보를 표시합니다. 맵을 확대/축소 및 이동하여 위치를 더 자세하게 혹은 덜 자세하게 표시할 수 있습니다. 맵의 장치 아이콘은 장치에 대해 활성 상태인 **경보** 또는 **경고**를 나타냅니다. **경보** 및 **경고** 수의 요약은 맵의 왼쪽에 표시됩니다.

장치 세부 정보를 보려면 맵을 이동하고 확대/축소하여 장치를 찾은 다음 맵에서 장치를 클릭합니다. 세부 정보는 다음과 같습니다.

* 최근 원격 분석 값
* 장치에서 지원하는 메서드
* 장치 속성

![대시보드에서 장치 세부 정보 보기](media/iot-suite-remote-monitoring-monitor/dashboarddevicedetail.png)

## <a name="view-alarms-from-your-devices"></a>장치에서 경보 보기

맵은 **경보** 및 **경고**로 현재 필터에서 장치를 강조 표시합니다. **시스템 경보** 패널은 장치에서 가장 최근의 경보에 대한 자세한 정보를 표시합니다.

![대시보드에서 시스템 경보 보기](media/iot-suite-remote-monitoring-monitor/dashboardsystemalarms.png)

**시스템 경보** 필터를 사용하여 최근 경보에 대한 시간 범위를 조정할 수 있습니다. 기본적으로 패널은 지난 1시간 동안의 경보를 표시합니다.

![시간별로 경보 필터링](media/iot-suite-remote-monitoring-monitor/dashboardalarmsfilter.png)

## <a name="view-the-system-kpis"></a>시스템 KPI 보기

**대시보드** 페이지는 시스템 KPI를 표시합니다.

![시간별로 경보 필터링](media/iot-suite-remote-monitoring-monitor/dashboardkpis.png)

**시스템 KPI** 필터를 사용하여 KPI 집계에 대한 시간 범위를 조정할 수 있습니다. 기본적으로 패널은 지난 1시간에 걸쳐 집계된 KPI를 표시합니다.

## <a name="next-steps"></a>다음 단계

이 자습서는 **대시보드** 페이지를 사용하여 원격 모니터링 솔루션에서 프로비전된 시뮬레이트된 트럭을 필터링하고 모니터링하는 방법을 보여 줬습니다.

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * 대시보드에서 장치 필터링
> * 실시간 원격 분석 보기
> * 장치 세부 정보 보기
> * 장치에서 경보 보기
> * 시스템 KPI 보기

이제 장치를 모니터링하는 방법을 배웠으므로 제안된 다음 단계는 다음 방법을 배우기 위한 것입니다.

* [임계값 기반 규칙을 사용하여 문제 감지](./iot-suite-remote-monitoring-automate.md)
* [장치 관리 및 구성](./iot-suite-remote-monitoring-manage.md)
* [장치 문제 해결 및 수정](./iot-suite-remote-monitoring-maintain.md)
* [시뮬레이트된 장치로 솔루션 테스트](iot-suite-remote-monitoring-test.md)

<!-- Next tutorials in the sequence -->