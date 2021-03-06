---
title: "Azure 키 자격 증명 모음이란? | Microsoft Docs"
description: "Azure 키 자격 증명 모음은 클라우드 응용 프로그램 및 서비스에서 사용되는 암호화 키 및 비밀을 보호하는데 도움이 됩니다. Azure 키 자격 증명 모음을 사용하여, 고객은 키와 비밀(예: 인증 키, 저장소 계정 키, 데이터 암호화 키, PFX 파일 및 암호)을 암호화하여 하드웨어 보안 모듈(HSM)로 보호된 키를 사용합니다."
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/16/2017
ms.author: barclayn
ms.openlocfilehash: f4981996a92abb3ecb4b6b92add8f1a0c7b1cbef
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2017
---
# <a name="what-is-azure-key-vault"></a>Azure 키 자격 증명 모음이란?
Azure 키 자격 증명 모음은 대부분 지역에서 사용할 수 있습니다. 자세한 내용은 [키 자격 증명 모음 가격 책정 페이지](https://azure.microsoft.com/pricing/details/key-vault/)를 참조하세요.

## <a name="introduction"></a>소개
Azure 키 자격 증명 모음은 클라우드 응용 프로그램 및 서비스에서 사용되는 암호화 키 및 비밀을 보호하는데 도움이 됩니다. Key Vault를 사용하여, 키와 비밀(예: 인증 키, 저장소 계정 키, 데이터 암호화 키, PFX 파일 및 암호)을 암호화에 하드웨어 보안 모듈(HSM)로 보호된 키를 사용합니다. 추가된 보증을 위해, HSM에서 키를 생성하거나 가져올 수 있습니다. 이 작업을 수행하려면 Microsoft는 FIPS 140-2 Level 2 유효성 검사가 적용된 HSM(하드웨어 및 펌웨어)에서 키를 처리합니다.  

키 자격 증명 모음은 키 관리 프로세스를 간소화하고 데이터를 액세스하고 암호화 하는 키의 제어를 유지할 수 있습니다. 개발자는 개발 및 테스트(분)을 위한 키를 만든 다음, 프로덕션 키로 원활하게 마이그레이션할 수 있습니다. 보안 관리자는 필요한 경우 권한을 키로 부여(및 해지)할 수 있습니다.

다음 표에서 키 자격 증명 모음이 개발자와 보안 관리자의 요구를 충족하도록 도와주는 방법을 더 잘 이해할 수 있습니다.

| 역할 | 문제 설명 | Azure 키 자격 증명 모음으로 해결됨 |
| --- | --- | --- |
| Azure 응용 프로그램용 개발자 |"서명 및 암호화를 위한 키를 사용하는 Azure용 응용 프로그램을 작성하려는 경우 솔루션이 지리적으로 분산되는 응용 프로그램에 적합하도록 이러한 키를 내 응용 프로그램의 외부에 두려고 합니다. <br/><br/>또한 직접 코드를 작성하지 않고도 이러한 키와 암호를 보호하려고 합니다. 또한 이러한 키와 암호를 내 응용 프로그램에서 최적의 성능으로 사용할 수 있게 하려고 합니다." |√ 키를 자격 증명 모음에 저장하고 필요할 때 URI로 호출합니다.<br/><br/> √ Azure에서는 업계 표준 알고리즘, 키 길이 및 HSM(하드웨어 보안 모듈)을 사용하여 키를 보호합니다.<br/><br/> √ 응용 프로그램과 동일한 Azure 데이터 센터에 상주하는 HSM에서 키를 처리합니다. 이를 통해 키가 온-프레미스와 같은 별도 위치에 있는 경우 안정성을 향상시키고 대기 시간을 단축합니다. |
| SaaS(Software as a Service)에 대한 개발자  |"내 고객의 테넌트 키와 암호에 대한 책임 또는 잠재적인 법적 책임을 원하지 않습니다. <br/><br/>핵심 소프트웨어 기능을 제공하는 일에 최선을 다해 집중할 수 있도록 고객이 키를 소유하고 관리하기를 원합니다." |√ 고객은 Azure에서 고유한 키를 가져오고 관리할 수 있습니다. SaaS 응용 프로그램이 해당 고객의 키를 사용하여 암호화 작업을 수행해야 하는 경우, Key Vault는 응용 프로그램을 대신하여 이러한 작업을 수행합니다. 응용 프로그램에서 고객의 키를 표시하지 않습니다. |
| 수석 보안 책임자(CSO) |"보안 키 관리를 위해 응용 프로그램이 FIPS 140-2 Level 2 HSM을 준수하는지 알고 싶습니다. <br/><br/>내 조직이 키 수명 주기를 제어하고 키 사용을 모니터링할 수 있는지 확인하고 싶습니다. <br/><br/>여러 Azure 서비스 및 리소스를 사용하더라도, Azure의 단일 위치에서 키를 관리 하고 싶습니다." |√ HSM은 FIPS 140-2 Level 2 검증되었습니다.<br/><br/>√ Key Vault는 Microsoft에서 키를 보거나 추출할 수 없게 설계되어 있습니다.<br/><br/>√ 키 사용을 거의 실시간으로 로깅합니다.<br/><br/>√ Azure에 얼마나 많은 자격 증명 모음이 있는지, 지원되는 영역 및 사용하는 응용 프로그램과 관계 없이 자격 증명 모음은 단일 인터페이스를 제공합니다. |

Azure를 구독하는 사용자는 주요 자격 증명 모음을 만들고 사용할 수 있습니다. 키 자격 증명 모음에는 개발자와 보안 관리자의 혜택이 있지만, 조직에 대한 다른 Azure 서비스를 관리하는 조직의 관리자가 구현하고 관리할 수 있습니다. 예를 들어, 관리자는 Azure 구독을 사용하여 로그인하고, 키를 저장할 조직에 대한 자격 증명 모음을 만든 후 다음과 같은 운영 태스크를 담당할 수 있습니다.

* 키 또는 비밀 만들기 또는 가져오기
* 키 또는 비밀 취소 또는 삭제
* 사용자 또는 응용 프로그램이 키 자격 증명 모음에 액세스하도록 권한을 부여하므로 해당 키 및 암호를 관리하거나 사용할 수 있습니다.
* 구성 키 용도(예: 서명 또는 암호화)
* 키 사용량 모니터링

그런 다음 이 관리자는 개발자에게 URI를 제공하여 자신의 응용 프로그램에서 호출 하고 보안 관리자에게 해당 키 사용 현황 로깅 정보를 제공합니다. 

   ![Azure 키 자격 증명 모음 개요][1]

개발자는 API를 사용하여 직접 키를 관리할 수도 있습니다. 자세한 내용은 [키 자격 증명 모음 개발자 가이드](key-vault-developers-guide.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계
관리자에 대한 시작 자습서의 경우 [Azure 키 자격 증명 모음 시작](key-vault-get-started.md)을 참조하세요.

키 자격 증명 모음에 사용 현황 로깅에 대한 자세한 내용은 [Azure 키 자격 증명 모음 로깅](key-vault-logging.md)을 참조하세요.

Azure Key Vault로 키 및 암호를 사용하는 방법에 대한 자세한 내용은 [키, 암호 및 인증서 정보](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx)를 참조하세요.

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
