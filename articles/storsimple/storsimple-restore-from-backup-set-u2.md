---
title: "백업에서 StorSimple 볼륨 복원 | Microsoft Docs"
description: "StorSimple Manager 서비스 Backup 카탈로그 페이지를 사용하여 백업 세트에서 StorSimple 볼륨을 복원하는 방법을 설명합니다."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 76fa3dd8fa2f9775dc166686e699a8dd66399aff
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>백업 세트에서 StorSimple 볼륨 복원(업데이트 2)
> [!NOTE]
> StorSimple의 클래식 포털은 사용되지 않습니다. StorSimple 장치 관리자는 사용 중단 일정에 따라 자동으로 새 Azure Portal로 이동합니다. 이 이동에 대한 메일 및 포털 알림을 받게 됩니다. 이 문서도 곧 사용 중지됩니다. 이 문서의 새 Azure Portal용 버전을 보려면 [백업 세트에서 StorSimple 볼륨 복원(업데이트 2)](storsimple-8000-restore-from-backup-set-u2.md)으로 이동하세요. 이동과 관련된 자세한 내용은 [FAQ: Azure Portal로 이동](storsimple-8000-move-azure-portal-faq.md)을 참조하세요.

[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>개요
**Backup 카탈로그** 페이지는 수동 또는 자동화된 백업을 수행할 때 생성되는 모든 백업 세트를 표시합니다. 이 페이지를 사용하여 백업을 나열 및 관리하거나, 백업 세트에서 복원하거나, 볼륨을 복제합니다.

 ![Backup 카탈로그 페이지](./media/storsimple-restore-from-backup-set-u2/restore.png)

이 자습서에서는 **Backup 카탈로그** 페이지를 사용하여 백업 세트에서 장치를 복원하는 방법을 설명합니다.

로컬 또는 클라우드 백업에서 볼륨을 복원할 수 있습니다. 두 경우 모두 백그라운드에서 데이터를 다운로드하는 동안 복원 작업은 즉시 볼륨을 온라인으로 전환합니다. 

## <a name="before-you-restore"></a>복원하기 전에
복원 작업을 시작하기 전에 다음 주의 사항에 유의해야 합니다.

* **볼륨을 오프라인으로 전환합니다.** – 복원 작업을 시작하기 전에 호스트와 장치 모두에서 볼륨을 오프라인으로 전환합니다. 복원 작업은 장치에서 볼륨을 온라인으로 자동으로 전환하지만 호스트에서 장치를 수동으로 온라인 상태로 만들어야 합니다. 볼륨이 장치에서 온라인으로 전환되면 호스트에서 볼륨을 온라인으로 전환할 수 있습니다. (복원 작업이 완료될 때까지 기다릴 필요가 없습니다.) 절차를 보려면 [볼륨을 오프라인으로 전환](storsimple-manage-volumes-u2.md#take-a-volume-offline)으로 이동합니다.
* **복원 후 볼륨 유형** – 삭제된 볼륨은 스냅숏의 유형에 따라 복원됩니다. 로컬로 고정된 볼륨은 로컬로 고정된 볼륨으로 복원되고 계층화된 볼륨은 계층화된 볼륨으로 복원됩니다.
  
    기존 볼륨의 경우 현재 볼륨의 사용 유형은 스냅숏에 저장되는 유형을 재정의합니다. 예를 들어 볼륨 유형이 계층화되었을 때 수행한 스냅숏의 볼륨을 복원하고 볼륨 유형이 이제 로컬로 고정되는 경우(변환 작업으로 인해) 볼륨은 로컬로 고정된 볼륨으로 복원됩니다. 마찬가지로, 기존 로컬로 고정된 볼륨이 확장되고 이후에 볼륨이 더 작은 경우에 수행된 이전 스냅숏에서 복원된 경우 복원된 볼륨은 현재 확장된 크기를 유지합니다.
  
    볼륨을 복원하는 동안 계층화된 볼륨에서 로컬로 고정된 볼륨으로 또는 _그 반대로_ 볼륨을 변환할 수 없습니다. 복원 작업이 완료될 때까지 기다린 다음 볼륨을 다른 종류로 변환할 수 있습니다. 볼륨 변환에 대한 자세한 내용은 [볼륨 유형 변경](storsimple-manage-volumes-u2.md#change-the-volume-type)으로 이동합니다. 
* **볼륨 크기는 복원된 볼륨에 반영됩니다.** – 삭제된(로컬로 고정된 볼륨이 완전히 프로비전되었기 때문에) 로컬로 고정된 볼륨을 복원하는 경우 중요한 고려 사항입니다. 이전에 삭제된 로컬로 고정된 볼륨을 복원하기 전에 충분한 공간이 있는지 확인합니다. 
* **복원하는 동안 볼륨을 확장할 수 없습니다.** – 볼륨을 확장하기 전에 복원 작업이 완료될 때까지 기다립니다. 볼륨 확장에 대한 정보는 [볼륨 수정](storsimple-manage-volumes-u2.md#modify-a-volume)으로 이동합니다.
* **로컬 볼륨을 복원하는 동안 백업을 수행할 수 있습니다.** - 절차를 보려면 [StorSimple 관리자 서비스를 사용하여 백업 정책 관리](storsimple-manage-backup-policies.md)로 이동합니다.
* **복원 작업을 취소할 수 있습니다.** - 복원 작업을 취소하는 경우 볼륨은 복원을 시작하기 전의 상태로 롤백됩니다. 절차를 보려면 [작업 취소](storsimple-manage-jobs-u2.md#cancel-a-job)로 이동합니다.

## <a name="how-does-restore-work"></a>복원 작업 방법
업데이트 4 이상을 실행하는 장치의 경우 heatmap 기반 복원이 구현됩니다. 데이터에 액세스하는 호스트의 요청이 장치에 도달하면 이러한 요청이 추적되고 heatmap이 만들어집니다. 요청 비율이 높으면 열이 많은 데이터 청크가 발생하고 요청 비율이 낮으면 열이 더 낮은 청크로 변환됩니다. _hot_으로 표시되려면 최소한 두 번 이상 데이터에 액세스해야 합니다. 수정된 파일도 _hot_으로 표시됩니다. 복원을 시작하면 heatmap을 기반으로 데이터의 자동 관리 하이드레이션이 발생합니다. 업데이트 4 이전 버전의 경우 데이터는 액세스 시에만 복원 중에 다운로드되었습니다. 

Heatmap 기반 추적은 계층화된 볼륨에 대해서만 사용하도록 설정되고 로컬로 고정된 볼륨에서는 지원되지 않습니다. Heatmap 기반 복원은 다른 장치에 볼륨을 복제하는 경우에도 지원되지 않습니다. 현재 위치 복원이 있고 복원할 볼륨에 대한 로컬 스냅숏은 장치에 있는 경우 (데이터를 이미 로컬에서 사용할 수 있으므로) 리하이드레이션하지 않습니다. 기본적으로 복원할 때 heatmap을 기반으로 데이터를 사전에 리하이드레이션하는 리하이드레이션 작업이 시작됩니다. 업데이트 4에서 Windows PowerShell cmdlet은 리하이드레이션 작업 실행을 쿼리하고, 리하이드레이션 작업을 취소하며, 리하이드레이션 작업 작업의 상태를 가져오는 데 사용할 수 있습니다.

* `Get-HcsRehydrationJob` - 이 cmdlet은 리하이드레이션 작업의 상태를 가져옵니다. 하나의 볼륨에 대해 단일 리하이드레이션 작업이 트리거됩니다.
* `Set-HcsRehydrationJob` - 이 cmdlet을 사용하면 리하이드레이션이 진행 중일 때 해당 작업을 일시 중지, 중지 또는 재개할 수 있습니다. 

리하이드레이션 cmdlet에 대한 자세한 내용은 [StorSimple용 Windows PowerShell cmdlet 참조](https://technet.microsoft.com/library/dn688168.aspx)로 이동하세요.

자동 리하이드레이션을 사용하면 일반적으로 일시적인 더 높은 읽기 성능이 예상됩니다. 실제 개선되는 정도는 액세스 패턴, 데이터 변동 및 데이터 형식 등 다양한 요인에 따라 달라집니다. 리하이드레이션 작업을 취소하려면 PowerShell cmdlet을 사용하면 됩니다. 이후 모든 복원 작업에 대해 리하이드레이션 작업을 영구적으로 사용하지 않도록 설정하려면 Microsoft 지원에 문의하세요.

## <a name="how-to-use-the-backup-catalog"></a>백업 카탈로그를 사용하는 방법
**Backup 카탈로그** 페이지는 백업 세트 선택 범위를 좁히는 데 도움이 되는 쿼리를 제공합니다. 다음 매개 변수를 기반으로 검색되는 백업 세트를 필터링할 수 있습니다.

* **장치** – 백업 세트를 만든 장치입니다.
* **Backup 정책** 또는 **볼륨** – 백업 정책 또는 이 백업 세트와 연결된 볼륨입니다.
* **시작** 및 **종료** – 백업 세트를 만든 날짜 및 시간 범위입니다.

그런 다음 필터링된 백업 세트는 다음 특성을 기반으로 표로 정리됩니다.

* **이름** – 백업 정책 또는 이 백업 세트와 연결된 볼륨입니다.
* **크기** – 백업 세트의 실제 크기입니다.
* **만든 날짜** – 백업이 만들어진 날짜 및 시간입니다. 
* **유형** – Backup 세트는 로컬 스냅숏 또는 클라우드 스냅숏일 수입니다. 로컬 스냅숏은 장치에 로컬로 저장된 모든 볼륨 데이터의 백업입니다. 클라우드 스냅숏은 클라우드에 있는 볼륨 데이터의 백업을 말합니다. 로컬 스냅숏은 데이터 복구 기능으로 클라우드 스냅숏을 선택하는 반면 빠른 액세스를 제공합니다.
* **시작 기준** – 일정에 따라 자동으로 또는 사용자가 수동으로 백업을 시작할 수 있습니다. (백업을 예약하기 위해 백업 정책을 사용할 수 있습니다. 또는, **백업 수행** 옵션을 사용하여 대화형 백업을 수행할 수 있습니다.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>백업에서 StorSimple 볼륨을 복원하는 방법
**Backup 카탈로그** 페이지를 사용하여 특정 백업에서 StorSimple 볼륨을 복원할 수 있습니다. 그러나 볼륨을 복원하면 백업이 수행된 시점의 상태로 볼륨이 되돌려진다는 점에 유의하세요. 백업 작업 후 추가된 모든 데이터가 손실됩니다.

> [!WARNING]
> 백업에서 복원되면 백업에서 기존 볼륨을 대체합니다. 백업이 수행된 후 작성된 모든 데이터가 손실될 수 있습니다.
> 
> 

### <a name="to-restore-your-volume"></a>볼륨을 복원하려면
1. StorSimple Manager 서비스 페이지에서 **Backup 카탈로그** 탭을 클릭합니다.
   
    ![Backup 카탈로그](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. 백업 세트를 다음과 같이 선택합니다.
   
   1. 해당 장치를 선택합니다.
   2. 드롭다운 목록에서 선택하려는 백업에 대 한 볼륨 또는 백업 정책을 선택합니다.
   3. 시간 범위를 지정합니다.
   4. 확인 아이콘 ![확인 아이콘](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) 을 클릭하여 이 쿼리를 실행 합니다.
      
      선택한 볼륨과 연결된 백업을 또는 백업 정책이 백업 세트의 목록에 나타나야 합니다.
3. 백업 세트를 확장하여 연결된 볼륨을 봅니다. 이 볼륨은 복원하려면 호스트와 장치에서 오프라인 상태여야 합니다. **볼륨 컨테이너** 페이지에서 볼륨에 액세스한 다음 [볼륨을 오프라인 상태로 전환](storsimple-manage-volumes-u2.md#take-a-volume-offline) 의 단계에 따라 오프라인 상태로 전환합니다.
   
   > [!IMPORTANT]
   > 해당 볼륨을 오프 라인으로 전환하기 전에, 먼저 호스트에서 해당 볼륨을 오프라인으로 전환했는지 확인합니다. 호스트에서 볼륨을 오프라인으로 전환하지 않은 경우 잠재적으로 데이터가 손상될 수입니다.
   > 
   > 
4. **Backup 카탈로그** 탭으로 다시 이동하고 Backup 세트를 선택합니다.
5. 페이지의 맨 아래에서 **복원** 을 클릭합니다.
6. 확인하라는 메시지가 표시됩니다. 복원 정보를 검토하고 확인 확인란을 선택합니다.
   
    ![확인 페이지](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. 확인 아이콘 ![확인 아이콘](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png)으로 이동합니다. 복원 작업을 시작합니다. **작업** 페이지에 액세스하여 작업을 볼 수 있습니다. 
8. 복원이 완료되면 볼륨의 내용을 백업의 볼륨으로 바뀌는 것을 확인할 수 있습니다.

![동영상 사용 가능](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **동영상 사용 가능**

StorSimple에서 클론 및 복원 기능을 사용하여 삭제된 파일을 복구하는 방법을 보여 주는 동영상을 시청하려면 [여기](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/)를 클릭하세요.

## <a name="if-the-restore-fails"></a>복원에 실패하는 경우
복원 작업이 어떤 이유로든 실패하는 경우에 알림을 받습니다. 이 경우 백업이 여전히 유효한지 확인하려면 백업 목록을 새로 고칩니다. 백업이 유효하고 클라우드로부터 복원하는 경우 연결 문제를 초래할 수 있습니다. 

복원 작업을 완료하려면 호스트에서 오프라인으로 볼륨을 가져오고 복원 작업을 다시 시도합니다. 복원 프로세스 중에 수행된 볼륨 데이터에 대한 수정은 손실됩니다.

## <a name="next-steps"></a>다음 단계
* [StorSimple 볼륨을 관리](storsimple-manage-volumes-u2.md)하는 방법을 알아봅니다.
* [StorSimple Manager 서비스를 사용하여 StorSimple 장치를 관리](storsimple-manager-service-administration.md)하는 방법을 알아봅니다.

