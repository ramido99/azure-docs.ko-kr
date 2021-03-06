---
title: "GitHub에서 Azure 스택 도구를 다운로드 합니다. | Microsoft Docs"
description: "Azure 스택 작업에 필요한 도구를 다운로드 하는 방법을 알아봅니다."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: E4DF77FA-F468-42B5-B44F-F10ED8049171
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: e730df58c54f7804d77b052a699d827bb82e5204
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2017
---
# <a name="download-azure-stack-tools-from-github"></a>GitHub에서 Azure 스택 도구 다운로드

*적용 대상: Azure 스택 통합 시스템과 Azure 스택 개발 키트*

**AzureStack 도구** 스택에 Azure 리소스를 배포 및 관리 하기 위한 PowerShell 모듈을 호스팅하는 GitHub 리포지토리 합니다. VPN 연결을 설정 하려는 경우 Azure 스택 개발 키트를 또는 Windows 기반 외부 클라이언트에 이러한 PowerShell 모듈을 다운로드할 수 있습니다. 이러한 도구를 얻으려면 GitHub 리포지토리를 복제 하거나 다운로드는 **AzureStack 도구** 다음 스크립트를 실행 하 여 폴더:

```PowerShell
# Change directory to the root directory. 
cd \

# Download the tools archive.
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>모듈에서 제공 하는 기능

**AzureStack 도구** 리포지토리 Azure 스택에 대 한 다음과 같은 기능을 지 원하는 PowerShell 모듈이 포함 되어 있습니다.  

| 기능 | 설명 | 이 모듈을 사용할 수 있는? |
| --- | --- | --- |
| [클라우드 기능](user/azure-stack-validate-templates.md) | 이 모듈을 사용 하 여 클라우드의 클라우드 기능을 얻으려고 합니다. 예를 들어이 모듈을 사용 하 여 API 버전 및 Azure 리소스 관리자 리소스와 같은 클라우드 기능을 가져올 수 있습니다. 이 모듈을 사용 하 여 Azure 스택 및 Azure 클라우드에 대 한 VM 확장을 가져올 수도 있습니다. | 클라우드 운영자 및 사용자 |
| [Azure 스택 계산 관리](azure-stack-add-vm-image.md) | 이 모듈을 사용 하 여 추가 하거나 스택 Azure marketplace에서 VM 이미지를 제거 합니다. | 클라우드 운영자 |
| [Azure 스택 인프라 관리](https://github.com/Azure/AzureStack-Tools/blob/master/Infrastructure/README.md) | 이 모듈을 사용 하 여 Azure 스택 인프라 Vm, 경고, 업데이트 및 등을 관리할 수 있습니다. |  클라우드 운영자|
| [리소스 관리자 정책에 대 한 Azure 스택](user/azure-stack-policy-module.md) | Azure 스택 동일한 버전 관리 및 서비스 가용성과 함께 Azure 구독 또는 Azure 리소스 그룹을 구성 하려면이 모듈을 사용 합니다. | 클라우드 운영자 및 사용자 |
| [Azure에 등록](azure-stack-register.md) | 이 모듈을 사용 하 여 Azure를 사용한 개발 키트 인스턴스를 등록 합니다. 등록 한 후, Azure에서 마켓플레이스 항목을 다운로드 하 고 Azure 스택에서 사용할 수입니다. | 클라우드 운영자 |
| [Azure 스택 배포](azure-stack-run-powershell-script.md) | 이 모듈을 사용 하 여 배포 하 고 Azure 스택 가상 하드 디스크 (VHD) 이미지를 사용 하 여 다시 배포할 Azure 스택 호스트 컴퓨터를 준비 합니다. | 클라우드 운영자|
| [Azure 스택에 연결](azure-stack-connect-powershell.md) | 이 모듈을 사용 하 여 PowerShell 통해 Azure 스택 인스턴스에 연결 하 고 Azure 스택에 VPN 연결을 구성 합니다. | 클라우드 운영자 및 사용자 |
| [Azure 스택 서비스 관리](azure-stack-create-offer.md) | 이 모듈을 사용 하 여 계산, Azure 저장소, 네트워크 및 주요 자격 증명 모음 서비스에 걸쳐 기본 테 넌 트 제안 무제한 할당량이 있는 만드는 합니다.   | 클라우드 운영자|
| [서식 파일 유효성 검사기](user/azure-stack-validate-templates.md) | 이 모듈 Azure 스택에 기존 또는 새 템플릿을 배포 하는 경우 확인을 사용 합니다. | 클라우드 운영자 및 사용자|


## <a name="next-steps"></a>다음 단계
* [Azure 스택 사용자의 PowerShell 환경 구성](user/azure-stack-powershell-configure-user.md)   
* [VPN을 통해 Azure 스택 개발 키트를 연결 합니다.](azure-stack-connect-azure-stack.md)  
