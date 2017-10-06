---
title: "diagnostické protokoly aaaViewing pro Azure Data Lake Store | Microsoft Docs"
description: "Pochopit, jak toosetup a přístupu k diagnostickým protokolům pro Azure Data Lake Store "
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 11fbf7f517f97abdcaf809c1ebeeb51424ab2c1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Přístup k diagnostickým protokolům pro Azure Data Lake Store
Informace o tom, jak tooenable protokolování diagnostiky pro váš účet Data Lake Store a jak tooview hello protokoly shromážděné pro váš účet.

Organizace může povolit protokolování diagnostiky pro jejich Azure Data Lake Store účet toocollect data přístup kontrolní záznamy, které poskytuje informace, jako je seznam uživatele, kteří používají hello data, jak často se hello data používají, kolik dat je uložen v hello účet, atd.

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Povolit protokolování diagnostiky pro váš účet Data Lake Store
1. Přihlášení toohello nové [portálu Azure](https://portal.azure.com).
2. Otevřete účet Data Lake Store a z vaší okně účtu Data Lake Store klikněte na tlačítko **nastavení**a potom klikněte na **diagnostické protokoly**.
3. V hello **protokolů diagnostiky** okně klikněte na tlačítko **zapněte diagnostiku**.

    ![Povolit protokolování diagnostiky](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "povolení diagnostických protokolů")

3. V hello **diagnostiky** okno, proveďte následující změny protokolování diagnostiky tooconfigure hello.
   
    ![Povolit protokolování diagnostiky](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "povolení diagnostických protokolů")
   
   * Nastavit **stav** příliš**na** tooenable protokolování diagnostiky.
   * Můžete data toostore/procesu hello různými způsoby.
     
        * Vyberte možnost hello příliš**archivu účet úložiště tooa** toostore protokoly tooan účet úložiště Azure. Tuto možnost použijte, pokud chcete tooarchive hello data, která bude zpracování dávky později. Pokud vyberete tuto možnost je nutné zadat účet služby Azure Storage toosave hello protokolů.
        
        * Vyberte možnost hello příliš**centra událostí tooan datového proudu** toostream protokolu data tooan centra událostí Azure. S největší pravděpodobností použijete tuto možnost, pokud mají podřízené zpracování kanálu tooanalyze příchozí protokolů v reálném čase. Pokud vyberete tuto možnost, je nutné zadat hello podrobnosti pro hello chcete toouse centra událostí Azure.

        * Vyberte možnost hello příliš**odeslat tooLog Analytics** toouse hello data protokolu hello generované tooanalyze služba Analýza protokolů Azure. Pokud vyberete tuto možnost, je nutné zadat, že hello podrobnosti o hello prostoru služby Operations Management Suite, které by použití hello provádět analýzu protokolu.
     
   * Zadejte, zda chcete protokoly auditu tooget nebo protokoly požadavku nebo obojí.
   * Zadejte hello počet dnů, pro které se musí uchovávat hello data. Uchování platí pouze pokud používáte data protokolu tooarchive účtu úložiště Azure.
   * Klikněte na **Uložit**.

Jakmile povolíte nastavení diagnostiky, můžete sledovat hello přihlásí hello **diagnostické protokoly** kartě.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Zobrazení diagnostických protokolů pro účet Data Lake Store
Existují dva způsoby tooview hello data protokolu pro váš účet Data Lake Store.

* Z účtu Data Lake Store hello nastavení zobrazení
* Z účtu úložiště Azure hello ukládat hello data

### <a name="using-hello-data-lake-store-settings-view"></a>Pomocí hello zobrazit Data Lake Store nastavení
1. Z účtu Data Lake Store **nastavení** okně klikněte na tlačítko **diagnostické protokoly**.
   
    ![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "zobrazení diagnostických protokolů") 
2. V hello **diagnostické protokoly** okno, měli byste vidět hello protokoly zařazený do kategorie službou **protokoly auditu** a **požadavku protokoly**.
   
   * Protokoly žádosti o zachycení každý API požadavek na hello účtu Data Lake Store.
   * Protokoly auditu jsou podobné toorequest protokoly, ale poskytují mnohem podrobnější rozpis hello operací provádí na účtu Data Lake Store hello. Například volání rozhraní API jednoho uložení v protokolech žádost může způsobit více operací "Připojit" v protokolech auditu hello.
3. Klikněte na tlačítko hello **Stáhnout** odkaz proti jednotlivým protokol položka toodownload hello protokoly.

### <a name="from-hello-azure-storage-account-that-contains-log-data"></a>Z hello účet služby Azure Storage, který obsahuje data protokolu
1. Otevřete okno účtu Azure Storage hello spojené s Data Lake Store pro protokolování a pak klikněte na objekty BLOB. Hello **služba objektů Blob** okno uvádí dva kontejnery.
   
    ![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "zobrazení diagnostických protokolů")
   
   * kontejner Hello **přehledy. protokoly auditu** obsahuje hello protokoly auditu.
   * kontejner Hello **přehledy. protokoly žádosti** obsahuje hello požadavek protokoly.
2. V rámci těchto kontejnerů hello protokoly jsou uloženy v rámci hello strukturu.
   
    ![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "zobrazení diagnostických protokolů")
   
    Jako příklad může být protokolu auditování tooan kompletní cesta hello`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Obdobně, může být hello kompletní cesta tooa žádost protokolu`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-hello-structure-of-hello-log-data"></a>Pochopení hello struktura dat protokolu hello
Hello protokoly auditu a požadavku jsou ve formátu JSON. V této části jsme podívejte se na hello struktura JSON pro požadavek a protokoly auditu.

### <a name="request-logs"></a>Žádost o protokoly
Zde je vzorového vstupu v protokolu hello požadavků formátu JSON. Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schéma požadavku protokolu
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| time |Řetězec |časové razítko Hello (ve formátu UTC) hello protokolu |
| resourceId |Řetězec |ID Hello hello prostředku, který operace trvalo umístit na |
| category |Řetězec |kategorie Hello protokolu. Například **požadavky**. |
| operationName |Řetězec |Název hello operace, která je zaznamenána. Například getfilestatus. |
| resultType |Řetězec |Stav Hello hello operace, například 200. |
| callerIpAddress |Řetězec |Hello IP adresa klienta hello vytváření hello požadavku |
| correlationId |Řetězec |id Hello hello protokolu, který můžete použít toogroup společně sada položek související protokolu |
| identity |Objekt |Hello identity, která vygenerovala hello protokolu |
| properties |JSON |Níže naleznete podrobnosti |

#### <a name="request-log-properties-schema"></a>Schéma vlastnosti požadavku protokolu
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| HttpMethod |Řetězec |Hello metoda HTTP pro hello operaci použít. Například získáte. |
| Cesta |Řetězec |operace hello Hello cesta byla provedena na |
| RequestContentLength |celá čísla |Délka obsahu Hello hello HTTP požadavku |
| clientRequestId |Řetězec |Hello Id, který jedinečně identifikuje tuto žádost |
| Čas spuštění |Řetězec |Hello čas, který hello server obdržel hello požadavku |
| čas ukončení |Řetězec |Hello čas, který hello serveru odeslání odpovědi |

### <a name="audit-logs"></a>Protokoly auditu
Zde je vstup vzorového ve formátu JSON auditní protokol hello. Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schéma protokolu auditu
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| time |Řetězec |časové razítko Hello (ve formátu UTC) hello protokolu |
| resourceId |Řetězec |ID Hello hello prostředku, který operace trvalo umístit na |
| category |Řetězec |kategorie Hello protokolu. Například **auditu**. |
| operationName |Řetězec |Název hello operace, která je zaznamenána. Například getfilestatus. |
| resultType |Řetězec |Stav Hello hello operace, například 200. |
| correlationId |Řetězec |id Hello hello protokolu, který můžete použít toogroup společně sada položek související protokolu |
| identity |Objekt |Hello identity, která vygenerovala hello protokolu |
| properties |JSON |Níže naleznete podrobnosti |

#### <a name="audit-log-properties-schema"></a>Schéma vlastnosti protokolu auditu
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| StreamName |Řetězec |operace hello Hello cesta byla provedena na |

## <a name="samples-tooprocess-hello-log-data"></a>Data protokolu hello tooprocess ukázky
Azure Data Lake Store obsahuje ukázku tooprocess a analýze dat protokolů hello. Můžete najít hello ukázku najdete na adrese [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)

