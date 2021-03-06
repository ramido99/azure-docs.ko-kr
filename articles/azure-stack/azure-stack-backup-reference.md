---
title: "Azure 스택 인프라 백업 서비스 참조 | Microsoft Docs"
description: "이 문서는 Azure 스택 인프라 백업 서비스에 대 한 참조 자료를 포함합니다."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 4e6e0a52b2c55239e38757223f54e5e94dc98c42
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/02/2018
---
# <a name="infrastructure-backup-service-reference"></a>인프라 백업 서비스 참조

## <a name="azure-backup-infrastructure"></a>Azure 백업 인프라

*적용 대상: Azure 스택 통합 시스템과 Azure 스택 개발 키트*

Azure 스택 Azure 리소스 관리자 포털을 구성 하는 여러 서비스와 인프라 관리 환경이 됩니다. Azure 스택 같이 관리 환경을 솔루션의 연산자에 노출 하는 복잡성을 줄입니다에 중점을 둡니다.

인프라 백업은 백업의 복잡성을 체득 하도록 설계 되었으며 및 인프라 서비스에 대 한 데이터를 복원, 솔루션을 관리 하 고 사용자에 게 SLA를 유지 관리에 집중할 수 연산자를 확인 합니다.

외부 공유에 백업 데이터의 내보내기 동일한 시스템에 백업을 저장할 방지 하기 위해 필요 합니다. 외부 공유 요구 유연성 관리자가 기존 회사 BC/DR 정책을 기반으로 데이터를 저장할 위치를 확인할 수 있습니다. 

### <a name="infrastructure-backup-components"></a>인프라 백업 구성 요소

인프라 백업에는 다음 구성 요소가 포함 됩니다.

 - **인프라 백업 컨트롤러**  
 인프라 백업 컨트롤러 인스턴스화될 하며 모든 Azure 스택 클라우드에서 있습니다.
 - **백업 리소스 공급자**  
 백업 (백업 RP) 리소스 공급자는 사용자 인터페이스 및 응용 프로그램 프로그램 인터페이스 (API) s 노출 스택에 Azure 인프라에 대 한 기본 백업 기능으로 구성 됩니다.

#### <a name="infrastructure-backup-controller"></a>인프라 백업 컨트롤러

인프라 백업 컨트롤러가 Azure 스택 클라우드에 대 한 서비스 인스턴스화됩니다 서비스 패브릭입니다. 백업 리소스는 지역 수준과 캡처 지역별 서비스 데이터 AD, CA, Azure 리소스 관리자에서에서 만들어진 CRP, SRP, NRP, KeyVault, RBAC 합니다. 

### <a name="backup-resource-provider"></a>백업 리소스 공급자

백업 리소스 공급자는 기본 구성 및 백업 리소스 목록에 대 한 스택 Azure 포털에서 사용자 인터페이스를 제공 합니다. 연산자는 사용자 인터페이스에서 다음 작업을 수행할 수 있습니다.

 - 외부 저장 위치, 자격 증명 및 암호화 키를 제공 하 여 처음에 대 한 백업을 사용 하도록 설정
 - 완료 보기 만든 백업 리소스 상태에서 및 리소스 만들기
 - 저장소 위치 백업 컨트롤러에서 백업 데이터를 삽입 하는 위치를 수정 합니다.
 - 외부 저장 위치에 액세스 하려면 백업 컨트롤러를 사용 하는 자격 증명 수정
 - 백업 컨트롤러를 사용 하 여 백업을 암호화 하는 암호화 키를 수정 


## <a name="backup-controller-requirements"></a>백업 컨트롤러 요구 사항

이 섹션에서는 인프라 백업에 대 한 중요 한 요구 사항에 설명 합니다. 정보를 검토 하 고 신중 하 게 한 다음 다시 참조를 필요에 따라 배포 및 후속 작업 중 및 Azure 스택 인스턴스에 대 한 백업을 사용 하도록 설정 하기 전에 것이 좋습니다.

요구 사항에 포함 합니다.

  - **소프트웨어 요구 사항** – 지원 되는 저장소 위치 및 크기 조정 지침에 설명 합니다. 
  - **네트워크 요구 사항** – 다른 저장 위치에 대 한 네트워크 요구 사항에 설명 합니다.  

### <a name="software-requirements"></a>소프트웨어 요구 사항

#### <a name="supported-storage-locations"></a>지원 되는 저장소 위치

| 저장 위치                                                                 | 세부 정보                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 신뢰할 수 있는 네트워크 환경 내에서 저장 장치에 호스트 된 SMB 파일 공유 | Azure 스택 배포 되는 동일한 데이터 센터에서 또는 다른 데이터 센터에 SMB 공유입니다. 여러 Azure 스택 인스턴스는 동일한 파일 공유를 사용할 수 있습니다. |
| Azure에서 SMB 파일 공유                                                          | 현재 지원되지 않습니다.                                                                                                                                 |
| Azure에서 blob 저장소                                                            | 현재 지원되지 않습니다.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>지원 되는 SMB 버전

| SMB | 버전 |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>저장소 위치 크기 지정 

인프라 백업 컨트롤러는 필요에 따라 데이터를 백업 합니다. 백업에 마지막 두 번 요일과 유지 백업의 최대 7 일 하는 것이 좋습니다. 

| 환경 크기 조정 | 백업 크기 예상된 | 필요한 공간의 전체 크기 |
|-------------------|--------------------------|--------------------------------|
| 4-12 노드        | 10 GB                     | 140GB                          |

### <a name="network-requirements"></a>네트워크 요구 사항
| 저장 위치                                                                 | 세부 정보                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 신뢰할 수 있는 네트워크 환경 내에서 저장 장치에 호스트 된 SMB 파일 공유 | Azure 스택 인스턴스가 방화벽 사용된 환경에 있는 포트 445가 필요 합니다. 인프라 백업 컨트롤러 포트 445 통해 SMB 파일 서버에 연결을 시작 합니다. |
| 파일 서버의 FQDN을 사용 하려면 이름을 PEP에서 확인할 수 있어야             |                                                                                                                                                                                         |

> [!Note]  
> 인바운드 포트가 없습니다 열려 있어야 합니다.


## <a name="infrastructure-backup-limits"></a>인프라 백업 제한

계획, 배포 및 사용자의 Microsoft Azure 스택 인스턴스가 작동 하는 대로 이러한 제한을 고려 합니다. 다음 표에서 이러한 한도 설명합니다.

### <a name="infrastructure-backup-limits"></a>인프라 백업 제한
| 제한 식별자                                                 | 제한        | 설명                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| 백업 유형                                                      | 전체만    | 인프라 백업 컨트롤러 전체 백업을 지원합니다. 증분 백업은 지원 되지 않습니다.                                          |
| 예약 된 백업                                                | 수동만  | 백업 컨트롤러는 현재 지원 요청 시 백업                                                                                 |
| 최대 동시 백업 작업                                   | 1            | 백업 컨트롤러의 인스턴스당 하나의 활성 백업 작업만 사용할 수 있습니다.                                                                  |
| 네트워크 스위치 구성                                     | 이 문서에서 다루지 않는 내용 | 관리자가 네트워크 스위치 구성을 OEM 도구를 사용 하 여 백업 해야 합니다. Azure 스택 각 OEM 공급 업체에서 제공한 설명서를 참조 하십시오. |
| 하드웨어 수명 주기 호스트                                          | 이 문서에서 다루지 않는 내용 | 관리자는 OEM 도구를 사용 하 여 하드웨어 수명 주기 호스트를 백업 해야 합니다. Azure 스택 각 OEM 공급 업체에서 제공한 설명서를 참조 하십시오.      |
| 파일 공유의 최대 수                                    | 1            | 하나의 파일 공유를 사용 하 여 백업 데이터를 저장할 수 있습니다.                                                                                        |
| 응용 프로그램 서비스, 기능, SQL, mysql 리소스 공급자의 데이터를 백업 합니다. | 이 문서에서 다루지 않는 내용 | 배포 및 관리 부가 가치 Microsoft에서 만든 RPs에 대해 게시 된 설명서를 참조 하십시오.                                                  |
| 백업 타사 리소스 공급자                              | 이 문서에서 다루지 않는 내용 | 배포 및 관리 부가 가치 타사 공급 업체에서 만든 RPs에 대해 게시 된 설명서를 참조 하십시오.                                          |

## <a name="next-steps"></a>다음 단계

 - 인프라 백업 서비스에 대 한 자세한 참조 [인프라 Backup 서비스에서 Azure 스택에 대 한 백업 및 데이터 복구](azure-stack-backup-infrastructure-backup.md)합니다.