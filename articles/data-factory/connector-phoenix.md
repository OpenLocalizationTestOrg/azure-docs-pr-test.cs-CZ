---
title: "Kopírování dat z Phoenix pomocí Azure Data Factory | Microsoft Docs"
description: "Postup kopírování dat z Phoenix do úložiště dat podporovaných podřízený pomocí aktivity kopírování v kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: b3e9597c52a402240d759abb0a96ffc007bcc9dd
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-phoenix-using-azure-data-factory"></a>Kopírování dat z Phoenix pomocí Azure Data Factory 

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z Phoenix. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Data můžete zkopírovat z Phoenix do úložiště dat žádné podporované jímky. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Azure Data Factory poskytuje integrované ovladače pro umožnění připojení, proto nemusíte ručně nainstalovat všechny ovladače, používání tohoto konektoru.

## <a name="getting-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory ke Phoenix konektoru.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Phoenix propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **Phoenix** | Ano |
| hostitel | IP adresu nebo název hostitele serveru Phoenix. (to znamená, 192.168.222.160)  | Ano |
| port | Port TCP, který používá Phoenix server naslouchat pro připojení klientů. Výchozí hodnota je 8765.  | Ne |
| httpPath | Částečné adresa URL odpovídající Phoenix serveru. (to znamená, /gateway/sandbox/phoenix/version). Výchozí hodnota je `hbasephoenix` při použití WindowsAzureHDInsightService.  | Ne |
| authenticationType. | Mechanismus ověřování používaný pro připojení k serveru Phoenix. <br/>Povolené hodnoty jsou: **anonymní**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Ano |
| uživatelské jméno | Uživatelské jméno používané pro připojení k serveru Phoenix.  | Ne |
| heslo | Heslo odpovídající uživatelské jméno. Můžete zvolit označit toto pole jako SecureString bezpečně uložit v ADF nebo uložení hesla v Azure Key Vault a nechat aktivitě kopírování načítat z ní při kopírování dat – Další informace z [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md). | Ne |
| enableSsl | Určuje, zda jsou šifrované připojení k serveru pomocí protokolu SSL. Výchozí hodnota je false.  | Ne |
| trustedCertPath | Úplná cesta soubor .pem, který obsahuje certifikáty důvěryhodné certifikační Autority pro ověření serveru při připojení přes protokol SSL. Tuto vlastnost lze nastavit pouze při použití protokolu SSL na vlastním hostováním infračerveného signálu. Výchozí hodnota je soubor cacerts.pem nainstalované s infračerveného signálu.  | Ne |
| useSystemTrustStore | Určuje, jestli se má použít certifikát Certifikační autority z úložiště důvěryhodnosti systému nebo z určeného souboru PEM. Výchozí hodnota je false.  | Ne |
| allowHostNameCNMismatch | Určuje, jestli chcete vyžadovat protokol SSL vydaný certifikační Autoritou název certifikátu shodovat s názvem hostitele serveru při připojení přes protokol SSL. Výchozí hodnota je false.  | Ne |
| allowSelfSignedServerCert | Určuje, jestli se má povolit certifikáty podepsané svým držitelem ze serveru. Výchozí hodnota je false.  | Ne |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je veřejně přístupná data store), můžete použít modul Runtime integrace Self-hosted nebo Runtime integrace Azure. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

**Příklad:**

```json
{
    "name": "PhoenixLinkedService",
    "properties": {
        "type": "Phoenix",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "<port>",
            "httpPath" : "hbasephoenix",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datové sady](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností nepodporuje Phoenix datovou sadu.

Ke zkopírování dat z Phoenix, nastavte vlastnost typu datové sady, která **PhoenixObject**. Není k dispozici žádné další vlastnosti specifické pro typ v tomto typu datové sady.

**Příklad**

```json
{
    "name": "PhoenixDataset",
    "properties": {
        "type": "PhoenixObject",
        "linkedServiceName": {
            "referenceName": "<Phoenix linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje Phoenix zdroje.

### <a name="phoenixsource-as-source"></a>PhoenixSource jako zdroj

Ke zkopírování dat z Phoenix, nastavte typ zdroje v aktivitě kopírování do **PhoenixSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **PhoenixSource** | Ano |
| query | Čtení dat pomocí vlastního dotazu SQL. Například: `"SELECT * FROM MyTable"`. | Ano |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromPhoenix",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Phoenix input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "PhoenixSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Další postup
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).
