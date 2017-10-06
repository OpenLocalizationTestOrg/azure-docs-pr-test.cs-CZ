---
title: "diagnostické protokoly aaaViewing pro Azure Data Lake Analytics | Microsoft Docs"
description: "Pochopit, jak toosetup a přístup diagnostické protokoly pro Azure Data Lake analytics "
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 4cd1eb6f585c1ef96c358340232ef85721a972b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Přístup k diagnostickým protokolům pro Azure Data Lake Analytics

Protokolování diagnostiky umožňuje záznamy auditu přístupu toocollect data. Tyto protokoly obsahují informace, jako:

* Seznam uživatelů, kteří používaná hello data.
* Jak často se hello data používají.
* Množství dat, které je uložený v účtu hello.

## <a name="enable-logging"></a>Povolit protokolování

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. Otevřete účet Data Lake Analytics a vyberte **diagnostické protokoly** z hello __monitorování__ části. Potom vyberte __zapněte diagnostiku__.

    ![Zapněte diagnostiku toocollect auditu a požadovat protokoly](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Z __nastavení diagnostiky__, nastavit stav too__On__ hello a vyberte možnosti protokolování.

    ![Zapněte diagnostiku toocollect auditu a požadovat protokoly](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "povolení diagnostických protokolů")

   * Nastavit **stav** příliš**na** tooenable protokolování diagnostiky.

   * Můžete data toostore/procesu hello třemi různými způsoby.

     * Vyberte __archivu účet úložiště tooa__ toostore protokolů v účtu úložiště Azure. Tuto možnost použijte, pokud chcete tooarchive hello data. Pokud vyberete tuto možnost, je nutné zadat úložiště Azure účet toosave hello protokoluje události do.

     * Vyberte **Stream tooan centra událostí** toostream protokolu data tooan centra událostí Azure. Tuto možnost použijte, pokud máte kanál zpracování příjmu dat, který analyzuje příchozí protokolů v reálném čase. Pokud vyberete tuto možnost, je nutné zadat hello podrobnosti pro hello chcete toouse centra událostí Azure.

     * Vyberte __odeslat tooLog Analytics__ toosend hello data toohello analýzy protokolů služby. Tuto možnost použijte, pokud chcete toogather toouse analýzy protokolů a analýza protokolů.
   * Zadejte, zda chcete protokoly auditu tooget nebo protokoly požadavku nebo obojí.  Žádost protokolu zaznamená každého požadavku rozhraní API. Protokol auditování zaznamenává všechny operace, které jsou aktivovány tohoto požadavku rozhraní API.

   * Pro __archivu účet úložiště tooa__, zadejte hello počet dní tooretain hello data.

   * Klikněte na __Uložit__.

        > [!NOTE]
        > Je nutné vybrat buď __archivu účet úložiště tooa__, __Stream tooan centra událostí__ nebo __odeslat tooLog Analytics__ před kliknutím na tlačítko hello __Uložit__tlačítko.

Jakmile povolíte nastavení diagnostiky, můžete se vrátit toohello __protokolů diagnostiky__ okno tooview hello protokoly.

## <a name="view-logs"></a>Zobrazit protokoly

### <a name="use-hello-data-lake-analytics-view"></a>Pomocí zobrazení hello Data Lake Analytics

1. Z vaše Data Lake Analytics účet okno, v části **monitorování**, vyberte **diagnostické protokoly** a poté vyberte toodisplay položka protokoly pro.

    ![Protokolování diagnostiky zobrazení](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "zobrazení diagnostických protokolů")

2. Hello protokoly jsou klasifikovány podle **protokoly auditu** a **požadavku protokoly**.

    ![položky protokolu](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * Protokoly žádosti o zachycení každý API požadavek na hello účtu Data Lake Analytics.
   * Protokoly auditu jsou podobné toorequest protokoly, ale poskytují mnohem podrobnější rozpis hello operací. Například může způsobit nahrávání jednoho volání rozhraní API v požadavku protokolu více operací "Připojit" v protokolu auditu.

3. Klikněte na tlačítko hello **Stáhnout** odkaz pro toodownload položky protokolu, který protokolu.

### <a name="use-hello-azure-storage-account-that-contains-log-data"></a>Použít účet úložiště Azure hello, který obsahuje data protokolu

1. Otevřete okno účtu Azure Storage hello spojené s Data Lake Analytics pro protokolování a potom klikněte na __objekty BLOB__. Hello **služba objektů Blob** okno uvádí dva kontejnery.

    ![Protokolování diagnostiky zobrazení](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "zobrazení diagnostických protokolů")

   * kontejner Hello **přehledy. protokoly auditu** obsahuje hello protokoly auditu.
   * kontejner Hello **přehledy. protokoly žádosti** obsahuje hello požadavek protokoly.
2. V rámci těchto kontejnerů hello protokoly jsou uloženy v části hello strukturu:

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > Hello `##` položky v cestě hello obsahují hello rok, měsíc, den a hodinu, ve které hello vytvoření protokolu. Data Lake Analytics vytvoří jeden soubor každou hodinu, takže `m=` vždy obsahuje hodnotu `00`.

    Jako příklad může být protokolu auditování tooan hello úplnou cestu:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Podobně může být hello kompletní cesta tooa žádost protokolu:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Struktura protokolu

Hello protokoly auditu a požadavek se v strukturovaného formátu JSON.

### <a name="request-logs"></a>Žádost o protokoly

Zde je vzorového vstupu v protokolu hello požadavků formátu JSON. Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schéma požadavku protokolu

| Name (Název) | Typ | Popis |
| --- | --- | --- |
| time |Řetězec |časové razítko Hello (ve formátu UTC) hello protokolu |
| resourceId |Řetězec |identifikátor Hello hello prostředku, který operace trvalo umístit na |
| category |Řetězec |kategorie Hello protokolu. Například **požadavky**. |
| operationName |Řetězec |Název hello operace, která je zaznamenána. Například GetAggregatedJobHistory. |
| resultType |Řetězec |Stav Hello hello operace, například 200. |
| callerIpAddress |Řetězec |Hello IP adresa klienta hello vytváření hello požadavku |
| correlationId |Řetězec |identifikátor Hello hello protokolu. Tato hodnota může být použité toogroup sada položek související protokolu. |
| identity |Objekt |Hello identity, která vygenerovala hello protokolu |
| properties |JSON |Viz další část hello (požadavku protokolu vlastnosti schéma) podrobnosti |

#### <a name="request-log-properties-schema"></a>Schéma vlastnosti požadavku protokolu

| Name (Název) | Typ | Popis |
| --- | --- | --- |
| HttpMethod |Řetězec |Hello metoda HTTP pro hello operaci použít. Například získáte. |
| Cesta |Řetězec |operace hello Hello cesta byla provedena na |
| RequestContentLength |celá čísla |Délka obsahu Hello hello HTTP požadavku |
| clientRequestId |Řetězec |Hello identifikátor, který jedinečně identifikuje tuto žádost |
| Čas spuštění |Řetězec |Hello čas, který hello server obdržel hello požadavku |
| čas ukončení |Řetězec |Hello čas, který hello serveru odeslání odpovědi |

### <a name="audit-logs"></a>Protokoly auditu

Zde je vstup vzorového ve formátu JSON auditní protokol hello. Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schéma protokolu auditu

| Name (Název) | Typ | Popis |
| --- | --- | --- |
| time |Řetězec |časové razítko Hello (ve formátu UTC) hello protokolu |
| resourceId |Řetězec |identifikátor Hello hello prostředku, který operace trvalo umístit na |
| category |Řetězec |kategorie Hello protokolu. Například **auditu**. |
| operationName |Řetězec |Název hello operace, která je zaznamenána. Například JobSubmitted. |
| resultType |Řetězec |Podřízený stav pro stav úlohy hello (operationName). |
| resultSignature |Řetězec |Další informace o stavu úlohy hello (operationName). |
| identity |Řetězec |Hello uživatel, který požadovanou operaci hello. Například, susan@contoso.com. |
| properties |JSON |Viz další část hello (schéma vlastnosti protokolu auditu) podrobnosti |

> [!NOTE]
> **resultType** a **resultSignature** poskytují informace o hello výsledek operace a obsahovat pouze hodnotu, pokud operace byla dokončena. Například pouze obsahovat hodnotu při **operationName** obsahuje hodnotu **JobStarted** nebo **JobEnded**.
>
>

#### <a name="audit-log-properties-schema"></a>Schéma vlastnosti protokolu auditu

| Name (Název) | Typ | Popis |
| --- | --- | --- |
| JobId |Řetězec |Úloha přiřazené toohello ID Hello |
| Název úlohy |Řetězec |Název Hello, která byla poskytnuta pro úlohy hello |
| JobRunTime |Řetězec |modul runtime Hello používá tooprocess hello úlohy |
| SubmitTime |Řetězec |Hello čas (ve formátu UTC) byla odeslána tuto úlohu hello |
| Čas spuštění |Řetězec |Hello čase hello úloha spuštění po odeslání (ve formátu UTC) |
| čas ukončení |Řetězec |Hello čas hello úloha byla ukončena |
| Paralelismus |Řetězec |Hello počet jednotek Data Lake Analytics požadovaný pro tuto úlohu při odesílání |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime**, a **paralelismus** poskytují informace o operaci. Tyto položky pouze obsahovat hodnotu, pokud které operace má spustit nebo byla dokončena. Například **SubmitTime** pouze obsahuje hodnotu po **operationName** má hodnotu hello **JobSubmitted**.

## <a name="process-hello-log-data"></a>Data protokolu zpracování hello

Azure Data Lake Analytics obsahuje ukázku tooprocess a analýze dat protokolů hello. Můžete najít hello ukázku najdete na adrese [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Další kroky
* [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md)
