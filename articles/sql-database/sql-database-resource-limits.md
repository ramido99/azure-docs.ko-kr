---
title: "Azure SQL Database 리소스 제한 | Microsoft Docs"
description: "이 페이지에서는 Azure SQL Database에 대한 몇 가지 일반적인 리소스 제한을 설명합니다."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 884e519f-23bb-4b73-a718-00658629646a
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 01/12/2018
ms.author: carlrab
ms.openlocfilehash: 13641b190c3c157f5b302314f88a42a160a1f2e0
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/13/2018
---
# <a name="azure-sql-database-resource-limits"></a>Azure SQL Database 리소스 제한

## <a name="single-database-storage-sizes-and-performance-levels"></a>단일 데이터베이스: 저장소 크기 및 성능 수준

다음 표에서는 각 서비스 계층과 성능 수준에서 단일 데이터베이스에 대해 사용할 수 있는 리소스를 나타냅니다. [Azure Portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), [Azure CLI](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli) 또는 [REST API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api)를 사용하여 단일 데이터베이스에 대한 서비스 계층, 성능 수준 및 저장소 용량을 설정할 수 있습니다.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="single-database-change-storage-size"></a>단일 데이터베이스: 저장소 크기 변경

- 단일 데이터베이스에 대한 DTU 가격에는 특정 크기의 저장소가 추가 비용 없이 포함됩니다. 포함된 용량 외 추가 저장소는 최대 250GB씩 총 1TB이 최대 크기 제한까지 추가 비용을 내고 프로비전할 수 있고 1TB 이상일 경우 256GB씩 프로비전할 수 있습니다. 포함된 저장소 크기 및 최대 크기 제한에 대한 자세한 내용은 [단일 데이터베이스: 저장소 크기 및 성능 수준](#single-database-storage-sizes-and-performance-levels)을 참조하세요.
- 단일 데이터베이스에 대한 추가 저장소는 [Azure Portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), 또는 [REST API](/rest/api/sql/databases/update)를 통해 해당 최대 크기를 늘려 프로비전할 수 있습니다.
- 단일 데이터베이스에 대한 추가 저장소 가격은 추가 저장소 용량에 해당 서비스 계층의 추가 저장소 단가를 곱한 것입니다. 추가 저장소 가격에 대한 자세한 내용은 [SQL Database 가격 책정](https://azure.microsoft.com/pricing/details/sql-database/)을 참조하세요.

## <a name="single-database-change-dtus"></a>단일 데이터베이스: DTU 변경

처음에 서비스 계층, 성능 수준 및 저장소 용량을 선택한 후, [Azure Portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update) 또는 [REST API](/rest/api/sql/databases/update)를 사용하여 실제 환경에 따라 단일 데이터베이스를 동적으로 확장 또는 축소할 수 있습니다. 

다음 비디오는 성능 계층을 동적으로 변경하여 단일 데이터베이스에 대해 사용 가능한 DTU를 늘리는 방법을 보여 줍니다.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

서비스 계층 및/또는 데이터베이스의 성능 수준을 변경하게 되면 새 성능 수준에서 원본 데이터베이스의 복제본을 만들고 연결을 복제본으로 전환합니다. 이 프로세스 중에 데이터가 손실되지는 않지만 복제본으로 전환할 경우 잠깐 동안 데이터베이스에 대한 연결이 비활성화되므로 비행의 일부 트랜잭션이 롤백될 수 있습니다. 전환 시간은 다양하지만, 일반적으로 30초보다 작은 4초 미만이 해당 시간의 99%를 차지하고 있습니다. 연결을 사용할 수 없는 짧은 시간에 많은 수의 항공편 트랜잭션이 있으면 전환 시간이 더 길어질 수 있습니다. 

전체 확장 프로세스 기간은 변경 전후 데이터베이스의 크기 및 서비스 계층에 따라 달라집니다. 예를 들어 표준 서비스 계층 내에서 변경되고 있는 250GB 데이터베이스는 6시간 내에 완료되어야 합니다. 프리미엄 서비스 계층 내의 성능 수준을 변경하고 있는 동일한 크기의 데이터베이스의 경우 3시간 이내에 확대를 완료되어야 합니다.

> [!TIP]
> 진행 중인 작업을 모니터링하려면 [SQL REST API를 사용하여 작업 관리](/rest/api/sql/Operations/List), [CLI를 사용하여 작업 관리](/cli/azure/sql/db/op), [T-SQL을 사용하여 작업 모니터링](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database)과 두 가지 PowerShell 명령 즉, [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) 및 [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity)를 참조하세요.

* 상위 서비스 계층이나 성능 수준으로 업그레이드하는 경우 더 큰 크기(최대 크기)를 명시적으로 지정하지 않는 한 최대 데이터베이스 크기는 증가하지 않습니다.
* 데이터베이스를 다운그레이드하려면 데이터베이스 사용 공간이 대상 서비스 계층 및 성능 수준의 최대 허용 크기보다 작아야 합니다. 
* **프리미엄** 또는 **프리미엄 RS**에서 **표준** 계층으로 다운그레이드할 때는 (1) 데이터베이스의 최대 크기가 대상 성능 수준에서 지원되고 (2) 최대 크기가 대상 성능 수준의 포함된 저장소 용량을 초과하는 경우 추가 저장소 비용이 적용됩니다. 예를 들어 최대 크기가 500GB인 P1 데이터베이스 크기를 S3으로 줄이는 경우 S3이 최대 크기인 500GB를 지원하고 포함된 저장소 크기는 250GB에 불과하므로 추가 저장소 비용이 적용됩니다. 따라서 추가 저장소 크기는 500GB – 250GB = 250GB입니다. 추가 저장소 가격 책정에 대한 자세한 내용은 [SQL Database 가격 책정](https://azure.microsoft.com/pricing/details/sql-database/)을 참조하세요. 실제 사용된 공간의 크기가 포함된 저장소의 크기보다 작은 경우 데이터베이스 최대 크기를 포함된 크기로 줄여 이러한 추가 비용을 방지할 수 있습니다. 
* [지역에서 복제](sql-database-geo-replication-portal.md)를 사용할 수 있는 데이터베이스를 업그레이드하는 경우 주 데이터베이스를 업그레이드하기 전에 먼저 해당 보조 데이터베이스를 원하는 성능 계층으로 업그레이드합니다(일반 지침). 다른 버전으로 업그레이드할 때 보조 데이터베이스를 먼저 업그레이드해야 합니다.
* [지역에서 복제](sql-database-geo-replication-portal.md)를 사용할 수 있는 데이터베이스를 다운그레이드하는 경우 보조 데이터베이스를 다운그레이드하기 전에 먼저 해당 주 데이터베이스를 원하는 성능 계층으로 다운그레이드합니다(일반 지침). 다른 버전으로 다운그레이드할 때 주 데이터베이스를 먼저 다운그레이드해야 합니다.
* 복원 서비스는 여러 서비스 계층에서 서로 다르게 제공됩니다. **기본** 계층으로 다운그레이드하는 경우 백업 보존 기간이 더 짧아집니다. [Azure SQL Database Backup](sql-database-automated-backups.md)을 참조하세요.
* 데이터베이스의 새로운 속성은 변경이 완료될 때까지 적용되지 않습니다.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>단일 데이터베이스: 최대 크기가 1TB보다 큰 경우 P11 및 P15의 제한 사항

P11 및 P15 데이터베이스에 대해 1TB보다 더 큰 최대 크기는 미국 동부2, 미국 서부, 미국 버지니아 주 정부, 유럽 서부, 독일 중부, 동남 아시아, 일본 동부, 오스트레일리아 동부, 캐나다 중부 및 캐나다 동부 지역에서 지원됩니다. 다음 고려 사항 및 제한 사항은 최대 크기가 1TB보다 큰 P11 및 P15 데이터베이스에 적용됩니다.

- (4TB 또는 4096GB의 값을 사용하여) 데이터베이스를 만들 때 1TB보다 큰 최대 크기를 선택하면 지원되지 않는 지역에서 데이터베이스를 프로비전할 때 오류가 발생하여 만들기 명령이 실패합니다.
- 지원되는 하위 지역 중 하나에 있는 기존 P11 및 P15 데이터베이스의 경우 1TB보다 크게 최대 저장소를 늘리려면 256GB씩 최대 4TB로 늘릴 수 있습니다. 해당 지역에서 더 큰 크기를 사용할 수 있는지 확인하려면 [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) 함수를 사용하거나 Azure Portal에서 데이터베이스 크기를 검사합니다. 기존 P11 또는 P15 데이터베이스 업그레이드는 서버 수준 보안 주체 로그인 또는 dbmanager 데이터베이스 역할의 멤버에 의해서만 수행될 수 있습니다. 
- 지원되는 지역에서 업그레이드 작업을 실행하면 구성이 즉시 업데이트됩니다. 업그레이드 프로세스 동안 데이터베이스는 온라인 상태로 유지됩니다. 그러나 실제 데이터베이스 파일이 새 최대 크기로 업그레이드될 때까지 1TB보다 큰 전체 저장소 크기를 사용할 수 없습니다. 필요한 시간의 길이는 업그레이드 중인 데이터베이스 크기에 따라 달라집니다. 
- P11 또는 P15 데이터베이스를 만들거나 업데이트하는 경우 256GB씩 1TB까지 늘릴지, 4TB까지 늘릴지 중에서만 선택할 수 있습니다. P11/P15를 만들 때 1TB의 기본 저장소 옵션이 미리 선택됩니다. 지원되는 지역 중 하나에 있는 데이터베이스의 경우 새 또는 기존 단일 데이터베이스의 저장소를 최대 4TB 크기로 늘릴 수 있습니다. 다른 모든 지역의 경우 최대 크기를 1TB 이상으로 늘릴 수 없습니다. 4TB의 포함된 저장소를 선택하면 가격이 변경되지 않습니다.
- 데이터베이스의 최대 크기를 1TB보다 크게 설정하면 사용되는 실제 저장소가 1TB 미만인 경우에도 1TB로 변경할 수 없습니다. 따라서 최대 크기가 1TB보다 큰 P11 또는 P15를 1TB P11 또는 1TB P15 1 이하의 성능 계층(예: P1-P6)으로 다운그레이드할 수 없습니다. 이 제한 사항은 지정 시간, 지역 복원, 장기 백업 보존 및 데이터베이스 복사를 포함한 복원 및 복사 시나리오에도 적용됩니다. 데이터베이스가 1TB보다 큰 최대 크기로 구성되면 이 데이터베이스의 모든 복원 작업은 최대 크기가 1TB보다 큰 P11/P15로 실행되어야 합니다.
- 활성 지역 복제 시나리오의 경우:
   - 지역에서 복제 관계 설정: 주 데이터베이스가 P11 또는 P15인 경우 보조 데이터베이스는 P11 또는 P15이어야 합니다. 낮은 성능 계층은 1TB보다 큰 크기를 지원하지 않으므로 보조 데이터베이스로 거부됩니다.
   - 지역에서 복제 관계에서 주 데이터베이스 업그레이드: 주 데이터베이스에서 최대 크기를 1TB보다 크게 변경하면 보조 데이터베이스에서도 동일한 변경 사항이 트리거됩니다. 변경 내용을 적용하려면 주 데이터베이스에서 두 가지 업그레이드에 성공해야 합니다. 1TB보다 큰 옵션에 대한 지역 제한 사항이 적용됩니다. 1TB보다 큰 크기를 지원하지 않는 지역에 보조 데이터베이스가 있는 경우 주 데이터베이스는 업그레이드되지 않습니다.
- 1TB보다 큰 크기의 P11/P15 데이터베이스 로드에 대한 Import/Export 서비스 사용은 지원되지 않습니다. SqlPackage.exe를 사용하여 데이터를 [가져오기](sql-database-import.md) 및 [내보내기](sql-database-export.md)합니다.

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>탄력적 풀: 저장소 크기 및 성능 수준

다음 표에서는 SQL Database 탄력적 풀에 대해 각 서비스 계층과 성능 수준에서 사용할 수 있는 리소스를 나타냅니다. [Azure Portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-powershell), [Azure CLI](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-cli) 또는 [REST API](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-rest-api)를 사용하여 서비스 계층, 성능 수준 및 저장소 용량을 설정할 수 있습니다.

> [!NOTE]
> 탄력적 풀의 개별 데이터베이스에 대한 리소스 제한은 일반적으로 DTU 및 서비스 계층을 기반으로 하는 풀 외부의 단일 데이터베이스의 경우와 동일합니다. 예를 들어 S2 데이터베이스의 최대 동시 작업자 수는 120명입니다. 따라서 풀의 데이터베이스당 최대 DTU가 50DTU(S2와 동일)인 경우 표준 풀의 데이터베이스에 대한 최대 동시 작업자 수도 120명입니다.
>

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

탄력적 풀의 모든 DTU가 사용되었다면 풀에 있는 각 데이터베이스는 쿼리를 처리할 같은 크기의 리소스를 받습니다. SQL Database 서비스는 같은 분량의 계산 시간을 보장하여 데이터베이스 간의 공정성을 공유할 리소스를 제공합니다. 탄력적 풀 리소스 공유 공정성은 데이터베이스당 DTU 최소값이 0이 아닌 값으로 설정될 때 각 데이터베이스에 보장된 리소스에 적용됩니다.

### <a name="database-properties-for-pooled-databases"></a>풀링된 데이터베이스에 대한 데이터베이스 속성

다음 표에서 풀링된 데이터베이스에 대한 속성을 설명합니다.

| 자산 | 설명 |
|:--- |:--- |
| 데이터베이스당 최대 eDTU |풀에 있는 다른 데이터베이스의 사용률에 따라 사용 가능한 경우 풀에 있는 임의 데이터베이스에서 사용할 수 있는 최대 eDTU 수입니다. 데이터베이스당 최대 eDTU는 데이터베이스에 대해 리소스를 보장하지 않습니다. 풀에 있는 모든 데이터베이스에 적용되는 전역 설정입니다. 데이터베이스 사용률의 최대치를 처리할 만큼 데이터베이스당 최대 eDTU를 충분히 높게 설정합니다. 풀은 일반적으로 모든 데이터베이스가 동시에 최대로 사용되지 않을 경우 데이터베이스에 대해 핫 및 콜드 사용률 패턴을 가정하므로 일정 수준의 오버커밋이 예상됩니다. 예를 들어, 데이터베이스당 최고 사용률이 20 eDTU이며 풀에 있는 100개의 데이터베이스 중 20%만 동시에 최대로 사용되는 것으로 가정합니다. 데이터베이스당 eDTU 최대값이 20 eDTU로 설정된 경우 풀을 5배만큼 오버커밋하고 풀당 eDTU를 400으로 설정하는 것이 적합합니다. |
| 데이터베이스당 최소 eDTU |풀에 있는 임의 데이터베이스에 보장되는 최소 eDTU 수입니다. 풀에 있는 모든 데이터베이스에 적용되는 전역 설정입니다. 데이터베이스당 최소 eDTU를 0으로 설정할 수 있으며 기본값이기도 합니다. 이 속성은 0과 데이터베이스당 평균 기록 eDTU 사용률 사이의 값으로 설정됩니다. 풀에 있는 데이터베이스 수와 데이터베이스당 최소 eDTU를 곱한 값은 풀당 eDTU를 초과할 수 없습니다. 예를 들어, 풀에 20개의 데이터베이스가 있고 데이터베이스당 eDTU 최소값이 10 eDTU로 설정되면 풀당 eDTU는 200 eDTU 이상이어야 합니다. |
| 데이터베이스당 최대 저장소 |풀에 있는 데이터베이스에 대한 최대 저장소입니다. 풀링된 데이터베이스는 풀 저장소를 공유하므로 데이터베이스 저장소는 남은 풀 저장소와 데이터베이스당 최대 저장소 중 작은 값으로 제한됩니다. 데이터 파일의 최대 저장소를 의미하는 데이터베이스당 최대 저장소는 로그 파일에서 사용하는 공간을 포함하지 않습니다. |
|||
 
## <a name="elastic-pool-change-storage-size"></a>탄력적 풀: 저장소 크기 변경

- 탄력적 풀에 대한 eDTU 가격에는 특정 크기의 저장소가 추가 비용 없이 포함됩니다. 포함된 용량 외 추가 저장소는 최대 250GB씩 총 1TB이 최대 크기 제한까지 추가 비용을 내고 프로비전할 수 있고 1TB 이상일 경우 256GB씩 프로비전할 수 있습니다. 포함된 저장소 크기 및 최대 크기 제한에 대한 자세한 내용은 [탄력적 풀: 저장소 크기 및 성능 수준](#elastic-pool-storage-sizes-and-performance-levels)을 참조하세요.
- 탄력적 풀에 대한 추가 저장소는 [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) 또는 [REST API](/rest/api/sql/elasticpools/update)를 통해 해당 최대 크기를 늘려 프로비전할 수 있습니다.
- 탄력적 풀에 대한 추가 저장소 가격은 추가 저장소 용량에 해당 서비스 계층의 추가 저장소 단가를 곱한 것입니다. 추가 저장소 가격에 대한 자세한 내용은 [SQL Database 가격 책정](https://azure.microsoft.com/pricing/details/sql-database/)을 참조하세요.

## <a name="elastic-pool-change-edtus"></a>탄력적 풀: eDTU 변경

[Azure Portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) 또는 [REST API](/rest/api/sql/elasticpools/update)를 사용하여 리소스 필요량에 따라 탄력적 풀에 사용할 수 있는 리소스를 늘리거나 줄일 수 있습니다.

- 풀 eDTU 크기를 조정하는 경우 데이터베이스 연결이 잠시 끊어집니다. 이 동작은 단일 데이터베이스(풀의 데이터베이스 아님)에 대한 DTU 크기를 조정할 때와 동일합니다. 해당 기간 및 크기 조정 작업 동안 데이터베이스 연결 끊김에 미치는 영향에 대한 자세한 내용은 [단일 데이터베이스에 대한 DTU 크기 조정](#single-database-change-storage-size)을 참조하세요. 
- 풀 eDTU의 크기를 조정하는 기간은 풀의 모든 데이터베이스에 사용되는 저장소 공간의 총 크기에 따라 달라질 수 있습니다. 일반적으로 크기 조정 대기 시간은 100GB당 평균 90분 이하입니다. 예를 들어 풀에 있는 모든 데이터베이스에서 사용 중인 총 공간이 200GB일 경우, 크기 조정을 위해 예상되는 대기 시간은 3시간 이하입니다. 표준 또는 기본 계층 내의 일부 경우에는 크기 조정 대기 시간이 사용 중인 공간 크기에 관계 없이 5분 이내일 수 있습니다.
- 일반적으로 데이터베이스당 최소 eDTU 또는 데이터베이스당 최대 eDTU 변경에 따른 대기 시간은 5분 이내입니다.
- 풀 eDTU 크기를 줄일 경우 풀 사용 공간이 대상 서비스 계층 및 풀 eDTU의 최대 허용 크기보다 작아야 합니다.
- 풀 eDTU 크기를 조정할 때 (1) 풀의 저장소 최대 크기를 대상 풀에서 지원하고, (2) 저장소 최대 크기가 대상 풀의 포함된 저장소 용량을 초과하는 경우 추가 저장소 비용이 적용됩니다. 예를 들어 최대 크기가 100GB인 100 eDTU 표준 풀 크기를 50 eDTU 표준 풀로 줄이는 경우 대상 풀이 100GB의 최대 크기를 지원하고 포함된 저장소 크기는 50GB에 불과하므로 추가 저장소 비용이 적용됩니다. 따라서 추가 저장소 크기는 100GB – 50GB = 50GB입니다. 추가 저장소 가격 책정에 대한 자세한 내용은 [SQL Database 가격 책정](https://azure.microsoft.com/pricing/details/sql-database/)을 참조하세요. 실제 사용된 공간의 크기가 포함된 저장소의 크기보다 작은 경우 데이터베이스 최대 크기를 포함된 크기로 줄여 이러한 추가 비용을 방지할 수 있습니다. 

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>데이터베이스 및 탄력적 풀 리소스 한계에 도달하면 어떻게 되나요?

### <a name="compute-dtus-and-edtus"></a>Compute(DTU 및 eDTU)

데이터베이스 계산 사용량(DTU 및 eDTU로 측정)이 높아지면 쿼리 대기 시간이 늘어나고 시간이 초과될 수 있습니다. 이러한 상황에서는 쿼리가 서비스에서 대기될 수 있으며 리소스가 사용 가능해질 경우 실행될 수 있게 리소스가 제공됩니다.
높은 계산 사용률에 도달할 경우 완화하는 방법에는 다음이 포함됩니다.

- 데이터베이스 또는 탄력적 풀의 성능 수준을 높여 데이터베이스에 더 많은 DTU 또는 eDTU를 제공합니다. [단일 데이터베이스: DTU 변경](#single-database-change-dtus) 및 [탄력적 풀: eDTU 변경](#elastic-pool-change-edtus)을 참조하세요.
- 쿼리를 최적화하여 각 쿼리당 리소스 사용률을 줄입니다. 자세한 내용은 [쿼리 튜닝/힌트](sql-database-performance-guidance.md#query-tuning-and-hinting)를 참조하세요.

### <a name="storage"></a>Storage

사용된 데이터베이스 공간이 최대 크기 제한에 도달하면 데이터 크기 증가를 가져오는 데이터베이스 삽입 및 업데이트가 실패하고 클라이언트에 [오류 메시지](sql-database-develop-error-messages.md)가 표시됩니다. 데이터베이스 선택 및 삭제는 계속 성공적으로 수행됩니다.

높은 공간 사용률에 도달할 경우 완화하는 방법에는 다음이 포함됩니다.

- 데이터베이스 또는 탄력적 풀의 최대 크기를 늘리거나 성능 수준을 변경하여 더 많은 포함된 저장소를 획득합니다. [SQL Database 리소스 제한](sql-database-resource-limits.md)을 참조하세요.
- 데이터베이스가 탄력적 풀에 속하는 경우 데이터베이스를 풀 외부로 이동하여 저장소 공간이 다른 데이터베이스와 공유되지 않도록 할 수 있습니다.

### <a name="sessions-and-workers-requests"></a>세션 및 작업자(요청) 

동시 세션 및 작업자의 최대 수는 서비스 계층 및 성능 수준(DTU 및 eDTU)에 따라 결정됩니다.  세션 또는 작업자 제한에 도달하면 새 요청이 거부되고 클라이언트에 오류 메시지가 표시됩니다. 사용할 수 있는 연결의 수는 응용 프로그램에서 쉽게 제어할 수 있지만, 동시 작업자 수는 예측하고 제어하기가 어렵습니다. 좀 더 오래 실행되는 쿼리로 인해 데이터베이스 리소스 제한에 도달하고 작업자가 대기하게 되는 최대 부하 기간이 특히 그렇습니다. 

높은 세션 또는 작업자 사용률에 도달할 경우 완화하는 방법에는 다음이 포함됩니다.
- 데이터베이스 또는 탄력적 풀의 서비스 계층 또는 성능 수준을 늘립니다. [단일 데이터베이스: 저장소 크기 변경](#single-database-change-storage-size), [단일 데이터베이스: DTU 변경](#single-database-change-dtus), [탄력적 풀: 저장소 크기 변경](#elastic-pool-change-storage-size) 및 [탄력적 풀: eDTU 변경](#elastic-pool-change-edtus)을 참조하세요.
- 계산 리소스에 대한 경합 때문에 작업자 사용률이 증가할 경우 쿼리를 최적화하여 각 쿼리의 리소스 사용률을 줄입니다. 자세한 내용은 [쿼리 튜닝/힌트](sql-database-performance-guidance.md#query-tuning-and-hinting)를 참조하세요.

## <a name="next-steps"></a>다음 단계

- 서비스 계층에 대한 자세한 내용은 [서비스 계층](sql-database-service-tiers.md)을 참조하세요.
- 단일 데이터베이스에 대한 자세한 내용은 [단일 데이터베이스 리소스](sql-database-resource-limits.md)를 참조하세요.
- 탄력적 풀에 대한 자세한 내용은 [탄력적 풀](sql-database-elastic-pool.md)을 참조하세요.
- 일반 Azure 제한에 대한 자세한 내용은 [Azure 구독 및 서비스 제한, 할당량 및 제약 조건](../azure-subscription-service-limits.md)을 참조하세요.
- DTU 및 eDTU에 대한 자세한 내용은 [DTU 및 eDTU](sql-database-what-is-a-dtu.md)를 참조하세요.
- tempdb 크기 제한에 대한 자세한 내용은 https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database를 참조하세요.
