---
title: "Azure Machine Learning 데이터 준비에서 사용 가능한 추가 원본 데이터 연결 예제 | Microsoft Docs"
description: "이 문서에서는 Azure Machine Learning 데이터 준비에서 사용할 수 있는 일단의 원본 데이터 연결 예제를 제공합니다."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 3ffe0e385f9dd71d8341305f42ceb10e0ea49af4
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/18/2017
---
# <a name="sample-of-custom-source-connections-python"></a>사용자 지정 원본 연결 샘플(Python) 
이 부록을 읽기 전에 [Python 확장성 개요](data-prep-python-extensibility-overview.md)를 참조하세요.

## <a name="load-data-from-dataworld"></a>data.world에서 데이터 로드

### <a name="prerequisites"></a>필수 조건

#### <a name="register-yourself-at-dataworld"></a>data.world에서 등록
data.world 웹 사이트에서 API 토큰이 필요합니다.

#### <a name="install-dataworld-library"></a>data.world 라이브러리 설치

**파일** > **명령줄 인터페이스 열기**를 차례로 선택하여 Azure Machine Learning Workbench 명령줄 인터페이스를 엽니다.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

다음으로 명령줄에서 `dw configure`를 실행합니다. 그러면 토큰을 요구하는 메시지가 표시됩니다. 토큰을 입력하면 홈 디렉터리에 .dw/ 디렉터리가 만들어지고 여기에 토큰이 저장됩니다.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
이제 data.world 라이브러리를 가져올 수 있습니다.

#### <a name="load-data-into-data-preparation"></a>데이터 준비로 데이터 로드

새 스크립트 기반 데이터 흐름을 만듭니다. 그런 후에 다음 스크립트를 사용하여 data.world에서 데이터를 로드합니다.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>데이터 준비로 Azure Cosmos DB 데이터 로드

새 스크립트 기반 데이터 흐름을 만들고, 다음 스크립트를 사용하여 Azure Cosmos DB에서 데이터를 로드합니다. (먼저 라이브러리를 설치해야 합니다. 자세한 내용은 연결된 이전 참조 문서를 참조하세요.)

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client.
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```
