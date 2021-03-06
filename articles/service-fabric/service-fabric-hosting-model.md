---
title: "Azure Service Fabric 호스팅 모델 | Microsoft Docs"
description: "배포된 Servic Fabric 서비스 및 서비스 호스트 프로세스 복제본(또는 인스턴스) 간의 관계에 대해 설명합니다."
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: ecc9038cf895ddaeb06dd0e4e9852d5ef4a4513a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2017
---
# <a name="service-fabric-hosting-model"></a>Service Fabric 호스팅 모델
이 문서에서는 Service Fabric에서 제공하는 응용 프로그램 호스팅 모델을 간략하게 설명하고 **공유 프로세스** 및 **단독 프로세스** 모델 간의 차이점을 설명합니다. 배포된 응용 프로그램이 Service Fabric 노드에 표시되는 방식과 서비스 복제본(또는 인스턴스) 및 서비스-호스트 프로세스 간의 관계를 설명합니다.

계속 진행하기 전에 [Service Fabric 응용 프로그램 모델][a1]을 숙지하고 다양한 개념 및 개념 간의 관계를 이해해야 합니다. 

> [!NOTE]
> 간단한 설명을 위해 이 문서에서는 명시적으로 언급되지 않은 경우 다음과 같이 가정합니다.
>
> - *복제본*이라는 단어는 상태 저장 서비스 복제본 또는 상태 비저장 서비스 인스턴스를 둘 다 가리킵니다.
> - *CodePackage*는 *ServiceType*을 등록하고 해당 *ServiceType*의 서비스 복제본을 호스트하는 *ServiceHost* 프로세스와 동일한 것으로 간주됩니다.
>

호스팅 모델을 이해하기 위해 한 가지 예를 살펴보겠습니다. *ApplicationType* 'MyAppType'에 있는 *ServiceType* 'MyServiceType'은 *ServicePackage* 'MyServicePackage'에 의해 제공되고, 이 서비스 패키지에 있는 *CodePackage* 'MyCodePackage'는 실행 시 *ServiceType* 'MyServiceType'을 등록한다고 가정합니다.

3 노드 클러스터가 있고, 'MyAppType' 유형의 *응용 프로그램* **fabric:/App1**을 만든다고 가정합니다. 이 *응용 프로그램* **fabric:/App1** 내에 파티션 2개(**P1** & **P2**)와 파티션당 복제본 3개가 있는 'MyServiceType' 유형의 **fabric:/App1/ServiceA** 서비스를 만듭니다. 다음 다이어그램은 이 응용 프로그램이 노드에 배포된 상태 보기를 보여 줍니다.

<center>
![배포된 응용 프로그램의 노드 보기][node-view-one]
</center>

Service Fabric은 'MyServicePackage'를 활성화했으며, 이 서비스 패키지가 두 파티션 **P1** & **P2**의 복제본을 호스트하는 'MyCodePackage'를 시작했습니다. 파티션당 복제본 수를 클러스터의 노드 수와 같도록 선택했으므로 클러스터에 있는 모든 노드 보기가 같습니다. **fabric:/App1** 응용 프로그램에 파티션 1개(**P3**)와 파티션당 복제본 3개가 있는 다른 서비스 **fabric:/App1/ServiceB**를 만들어 보겠습니다. 다음 다이어그램은 새로운 노드 보기를 보여 줍니다.

<center>
![배포된 응용 프로그램의 노드 보기][node-view-two]
</center>

보시는 것처럼 Service Fabric이 **fabric:/App1/ServiceB** 서비스의 **P3** 파티션에 대한 새 복제본을 기존 'MyServicePackage' 활성화에 배치했습니다. 이제 'MyAppType' 유형의 다른 *응용 프로그램* **fabric:/App2**를 만들고, **fabric:/App2** 내부에 파티션 2개(**P4** & **P5**)와 파티션당 복제본 3개가 있는 **fabric:/App2/ServiceA** 서비스를 만들어 보겠습니다. 다음 다이어그램은 새 노드 보기를 보여 줍니다.

<center>
![배포된 응용 프로그램의 노드 보기][node-view-three]
</center>

이번에는 Service Fabric이 새로운 'MyCodePackage' 복사본을 시작하는 새로운 'MyServicePackage' 복사본을 활성화했으며, **fabric:/App2/ServiceA** 서비스의 두 파티션(**P4** & **P5**)에 있던 복제본이 이 새로운 복사본 'MyCodePackage'에 배치됩니다.

## <a name="shared-process-model"></a>공유 프로세스 모델
위에서 확인한 모델은 Service Fabric에서 제공하는 기본 호스팅 모델이며, **공유 프로세스** 모델이라고 합니다. 이 모델에서는 지정된 *응용 프로그램*에 대해 지정된 *ServicePackage*의 복사본 하나만 *노드*에서 활성화되고(노드에 포함된 모든 *CodePackages*가 시작됨), 지정된 *ServiceType*의 모든 서비스 복제본은 해당 *ServiceType*을 등록하는 *CodePackage*에 모두 배치됩니다. 즉, 지정된 *ServiceType*의 노드에 있는 모든 서비스 복제본은 모두 동일한 프로세스를 공유합니다.

## <a name="exclusive-process-model"></a>단독 프로세스 모델
Service Fabric에서 제공하는 다른 호스팅 모델은 **단독 프로세스** 모델입니다. 이 모델에서 지정된 *노드*에 각 복제본을 배치하기 위해 Service Fabric은 새로운 *ServicePackage* 복사본을 활성화하고(노드에 포함된 모든 *CodePackages*가 시작됨), 복제본은 이 복제본이 속하는 서비스의 *ServiceType*을 등록한 *CodePackage*에 배치됩니다. 즉, 각 복제본은 해당 전용 프로세스에 상주합니다. 

이 모델은 지원되는 Service Fabric의 시작 버전 5.6입니다. [PowerShell][p1], [REST][r1] 또는 [FabricClient][c1]를 사용하여 서비스를 만들 때 **ServicePackageActivationMode**를 'ExclusiveProcess'로 지정하여 **단독 프로세스** 모델을 선택할 수 있습니다.

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

응용 프로그램 매니페스트에 기본 서비스가 있는 경우 아래와 같이 **ServicePackageActivationMode** 특성을 지정하여 **단독 프로세스** 모델을 선택할 수 있습니다.

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
위 예제를 계속하여, **fabric:/App1** 응용 프로그램에 파티션 2개(**P6** & **P7**)와 파티션당 복제본 3개가 있는 다른 서비스 **fabric:/App1/ServiceC**를 만들어 보겠습니다. **ServicePackageActivationMode**는 'ExclusiveProcess'로 설정됩니다. 다음 다이어그램은 새로운 노드 보기를 보여 줍니다.

<center>
![배포된 응용 프로그램의 노드 보기][node-view-four]
</center>

보시는 것처럼 Service Fabric은 **P6** & **P7** 파티션의 각 복제본에 대해 하나씩 새로운 'MyServicePackage' 복제본 2개를 활성화하고 각 복제본을 전용 *CodePackage* 복사본에 배치했습니다. 여기서 주목할 또 다른 사항은 **단독 프로세스** 모델을 사용하는 경우 지정된 *응용 프로그램*에 대해 지정된 *ServicePackage* 복사본이 *노드*에서 여러 개 활성화될 수 있다는 것입니다. 위 예제에서는 'MyServicePackage' 복사본 3개가 **fabric:/App1**에 대해 활성화되었습니다. 'MyServicePackage'의 각 활성 복사본에는 *응용 프로그램* **fabric:/App1** 내에서 해당 복사본을 식별하는 **ServicePackageActivationId**가 연결되어 있습니다.

위 예제의 **fabric:/App2**와 마찬가지로 **공유 프로세스** 모델만 *응용 프로그램*에 사용되는 경우 *노드*에 활성 *ServicePackage* 복사본 하나만 있고, 이 *ServicePackage* 활성화에 대한 **ServicePackageActivationId**는 '빈 문자열'입니다.

> [!NOTE]
>- **공유 프로세스** 호스팅 모델은 **ServicePackageActivationMode** = **SharedProcess**에 해당합니다. 이것이 기본 호스팅 모델이며, 서비스를 만들 때 **ServicePackageActivationMode**를 지정할 필요가 없습니다.
>
>- **단독 프로세스** 호스팅 모델은 **ServicePackageActivationMode** = **ExclusiveProcess**이며, 서비스를 만들 때 명시적으로 지정해야 합니다. 
>
>- 서비스의 호스팅 모델은 [서비스 설명][p2]을 쿼리하고 **ServicePackageActivationMode**의 값을 확인하면 알 수 있습니다.
>
>

## <a name="working-with-deployed-service-package"></a>배포된 서비스 패키지 작업
노드의 활성 *ServicePackage* 복사본을 [배포된 서비스 패키지][p3]라고 합니다. 위에서 설명한 대로, **단독 프로세스** 모델을 사용하여 서비스를 만드는 경우 지정된 *응용 프로그램*에서 동일한 *ServicePackage*에 대해 배포된 서비스 패키지가 여러 개 있을 수 있습니다. [배포된 서비스 패키지의 상태 보고][p4] 또는 [배포된 서비스 패키지의 코드 패키지 다시 시작][p5] 등 배포된 서비스 패키지와 관련된 작업을 수행하는 동안 **ServicePackageActivationId**를 제공하여 배포된 특정 서비스 패키지를 식별해야 합니다.

 배포된 서비스 패키지의 **ServicePackageActivationId**는 노드에서 [배포된 서비스 패키지][p3] 목록을 쿼리하여 확인할 수 있습니다. 노드에서 [배포된 서비스 유형][p6], [배포된 복제본][p7] 및 [배포된 코드 패키지][p8]를 쿼리하는 경우 쿼리 결과에 배포된 부모 서비스 패키지의 **ServicePackageActivationId**도 포함됩니다.

> [!NOTE]
>- **공유 프로세스** 호스팅 모델 아래의 지정된 *노드*에서 지정된 *응용 프로그램*에 대해 *ServicePackage* 복사본 하나만 활성화됩니다. **ServicePackageActivationId**는 *빈 문자열*과 같으며, 배포된 서비스 패키지 관련 작업을 수행하는 동안 지정할 필요가 없습니다. 
>
> - **단독 프로세스** 호스팅 모델 아래의 지정된 *노드*에서 지정된 *응용 프로그램*에 대해 *ServicePackage* 복사본 하나 이상이 활성화될 수 있습니다. 각 활성화의 **ServicePackageActivationId**는 *비어 있지 않으며*, 배포된 서비스 패키지 관련 작업을 수행하는 동안 지정해야 합니다. 
>
> - **ServicePackageActivationId**를 생략하면 기본적으로 '빈 문자열'로 설정됩니다. **공유 프로세스** 모델 아래에 활성화된 배포된 서비스 패키지가 있으면 해당 패키지에서 작업이 수행되고, 없으면 작업이 실패합니다.
>
> - **ServicePackageActivationId**는 동적으로 생성되고 다양한 이유로 변경될 수 있기 때문에 한 번 쿼리한 후 캐시하지 않는 것이 좋습니다. **ServicePackageActivationId**가 필요한 작업을 수행하기 전에 먼저 노드에서 [배포된 서비스 패키지][p3] 목록을 쿼리한 후 쿼리 결과의 **ServicePackageActivationId**를 사용하여 원래 작업을 수행해야 합니다.
>
>

## <a name="guest-executable-and-container-applications"></a>게스트 실행 파일 및 컨테이너 응용 프로그램
Service Fabric은 [게스트 실행 파일][a2] 및 [컨테이너][a3] 응용프로그램을 자체 포함된 상태 비저장 서비스로 처리합니다. 즉, *ServiceHost*(프로세스 또는 컨테이너)에 Service Fabric 런타임이 없습니다. 자체 포함된 이러한 서비스에는 *ServiceHost*당 복제본 수가 적용되지 않습니다. 이러한 서비스에 사용되는 가장 일반적인 구성은 [InstanceCount][c2]가 -1과 같은 단일 파티션입니다(즉, 클러스터의 각 노드에서 서비스 코드 복사본 하나가 실행되고 있음). 

이러한 서비스에 대한 기본 **ServicePackageActivationMode**는 Service Fabric이 *노드*에서 지정된 *응용 프로그램*에 대해 **SharedProcess** 복사본 하나만 활성화하는 *ServicePackage*입니다. 즉, 서비스 코드 복사본 하나만 *노드*를 실행합니다. *ServiceManifest*에 지정된 *ServiceType*의 서비스를 여러 개(*Service1* ~ *ServiceN*) 만드는 경우 또는 서비스가 다중 분할된 경우 *노드*에서 서비스 코드의 여러 복사본을 실행하려면 서비스를 만들 때 **ServicePackageActivationMode**를 **ExclusiveProcess**로 지정해야 합니다.

## <a name="changing-hosting-model-of-an-existing-service"></a>기존 서비스의 호스팅 모델 변경
업그레이드 또는 업데이트 메커니즘을 통해(또는 응용 프로그램 매니페스트의 기본 서비스 지정에서) 기존 서비스의 호스팅 모델을 **공유 프로세스**에서 **단독 프로세스**로 변경하는 기능은 현재 지원되지 않습니다. 이 기능에 대한 지원은 이후 버전에서 제공될 예정입니다.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>공유 프로세스 및 단독 프로세스 모델 중에서 선택
두 호스팅 모델에는 장단점이 있으며, 사용자가 요구 사항에 가장 적합한 모델을 평가해야 합니다. **공유 프로세스** 모델에서는 더 적은 프로세스가 생성되고 동일한 프로세스의 여러 복제본이 포트를 공유할 수 있기 때문에 OS 리소스 사용률이 향상됩니다. 그러나 복제본 중 하나에서 서비스 호스트를 종료해야 하는 오류가 발생할 경우 동일한 프로세스의 다른 모든 복제본에 영향을 주게 됩니다.

 **단독 프로세스** 모델은 해당 프로세스의 모든 복제본에 대해 격리를 개선하므로 오동작하는 복제본이 다른 복제본에 영향을 주지 않습니다. 이 모델은 통신 프로토콜이 포트 공유를 지원하지 않는 경우에 유용합니다. 복제본 수준에서 리소스 관리를 쉽게 적용할 수 있습니다. 반면, **단독 프로세스**는 노드의 각 복제본마다 프로세스 하나를 생성하기 때문에 더 많은 OS 리소스를 사용합니다.

## <a name="exclusive-process-model-and-application-model-considerations"></a>단독 프로세스 모델 및 응용 프로그램 모델 고려 사항
Service Fabric에서 응용 프로그램을 모델링하는 권장 방법은 *ServicePackage*당 *ServiceType* 하나를 유지하는 것이며, 이 모델은 대부분의 응용 프로그램에 효과적입니다. 

특정 사용 사례를 위해 Service Fabric은 *ServicePackage*당 둘 이상의 *ServiceType*을 허용하며 하나의 *CodePackage*는 둘 이상의 *ServiceType*을 등록할 수 있습니다. 다음은 이러한 구성이 유용할만한 몇 가지 시나리오입니다.

- 더 적은 프로세스를 생성하고 프로세스당 더 높은 복제본 밀도를 사용하여 OS 리소스 사용률을 최적화하려고 합니다.
- 서로 다른 ServiceTypes의 복제본이 초기화 또는 메모리 비용이 높은 몇 가지 공통 데이터를 공유해야 합니다.
- 무료 서비스 제품이 있으며, 서비스의 모든 복제본을 동일한 프로세스에 배치하여 리소스 사용률에 제한을 적용하려고 합니다.

**단독 프로세스** 호스팅 모델은 *ServicePackage*당 여러 *ServiceTypes*가 있는 응용 프로그램 모델과 일치하지 않습니다. 이는 *ServicePackage*당 여러 *ServiceTypes*가 복제본 간의 리소스 공유를 높이도록 설계되었으며 프로세스당 더 높은 복제본 밀도를 지원하기 때문입니다. **단독 프로세스** 모델은 이와 다른 목표를 달성하도록 설계되었습니다.

*ServicePackage*당 여러 *ServiceTypes*가 있고, 서로 다른 *CodePackage*가 각 *ServiceType*을 등록하는 경우를 고려해 보세요. 가령, *CodePackages* 2개가 있는 *ServicePackage* 'MultiTypeServicePackge'가 있다고 가정합니다.

- 'MyCodePackageA'는 *ServiceType* 'MyServiceTypeA'를 등록합니다.
- 'MyCodePackageB'는 *ServiceType* 'MyServiceTypeB'를 등록합니다.

이제 *응용 프로그램* **fabric:/SpecialApp**을 만들고, **단독 프로세스** 모델을 사용하여 **fabric:/SpecialApp** 내부에 다음 두 개의 서비스를 만듭니다.

- 파티션 2개(**P1** 및 **P2**)와 파티션당 복제본 3개가 있는 'MyServiceTypeA' 유형의 **fabric:/SpecialApp/ServiceA** 서비스
- 파티션 2개(**P3** 및 **P4**)와 파티션당 복제본 3개가 있는 'MyServiceTypeB' 유형의 **fabric:/SpecialApp/ServiceB** 서비스

지정된 노드에서 두 서비스에 각각 복제본 2개가 있습니다. **단독 프로세스** 모델을 사용하여 서비스를 만들었기 때문에 Service Fabric은 각 복제본에 대해 새로운 'MyServicePackge' 복사본을 활성화합니다. 'MultiTypeServicePackge'를 활성화할 때마다 'MyCodePackageA' 및 'MyCodePackageB'의 복사본이 시작됩니다. 그러나 'MyCodePackageA' 또는 'MyCodePackageB' 중 하나만 'MultiTypeServicePackge'가 활성화된 복제본을 호스트합니다. 다음 다이어그램은 노드 보기를 보여 줍니다.

<center>
![배포된 응용 프로그램의 노드 보기][node-view-five]
</center>

 보시는 것처럼, **fabric:/SpecialApp/ServiceA** 서비스의 **P1** 파티션 복제본에 대한 'MultiTypeServicePackge' 활성화에서 'MyCodePackageA'는 복제본을 호스트하고 'MyCodePackageB'는 작동하여 실행되기만 합니다. 마찬가지로, **fabric:/SpecialApp/ServiceB** 서비스의 **P3** 파티션 복제본에 대한 'MultiTypeServicePackge' 활성화에서 'MyCodePackageB'는 복제본을 호스트하고 'MyCodePackageA'는 작동하여 실행되기만 합니다. 따라서 *ServicePackage*당 다른 *ServiceTypes*를 등록하는 *CodePackages* 수가 많을수록 중복 리소스 사용량이 증가합니다. 
 
 반면, **공유 프로세스** 모델을 사용하여 **fabric:/SpecialApp/ServiceA** 및 **fabric:/SpecialApp/ServiceB** 서비스를 만드는 경우 Service Fabric은 앞에서 본 것처럼 *응용 프로그램* **fabric:/SpecialApp**에 대해 'MultiTypeServicePackge' 복사본 하나만 활성화합니다. 'MyCodePackageA'는 **fabric:/SpecialApp/ServiceA** 서비스(또는 더 정확히 말해 'MyServiceTypeA' 유형의 모든 서비스)에 대한 모든 복제본을 호스트하고, 'MyCodePackageB'는 **fabric:/SpecialApp/ServiceB** 서비스(또는 더 정확히 말해 'MyServiceTypeB' 유형의 모든 서비스)에 대한 모든 복제본을 호스트합니다. 다음 다이어그램은 이 설정의 노드 보기를 보여 줍니다. 

<center>
![배포된 응용 프로그램의 노드 보기][node-view-six]
</center>

위 예제에서 'MyCodePackageA'가 'MyServiceTypeA'와 'MyServiceTypeB'를 둘 다 등록하고 'MyCodePackageB'가 없을 경우 중복 *CodePackage*가 실행되지 않는다고 간주할 수 있습니다. 이 가정은 맞지만, 앞에서 설명한 대로 이 응용 프로그램 모델은 **단독 프로세스** 호스팅 모델과 일치하지 않습니다. 각 복제본을 전용 프로세스에 배치하려는 경우 동일한 *CodePackage*에서 두 *ServiceTypes*를 모두 등록할 필요는 없으며 각 *ServiceType*을 해당 *ServicePacakge*에 배치하는 것이 더 자연스러운 선택입니다.

## <a name="next-steps"></a>다음 단계
[응용 프로그램을 패키지][a4]하고 배포를 준비합니다.

[응용 프로그램 배포 및 제거][a5]에서는 PowerShell을 사용하여 응용 프로그램 인스턴스를 관리하는 방법을 설명합니다.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-deploy-existing-app.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage
