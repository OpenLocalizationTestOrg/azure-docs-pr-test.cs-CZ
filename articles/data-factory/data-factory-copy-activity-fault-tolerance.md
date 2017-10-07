---
title: "aaaAdd odolnost proti chybám při aktivitě kopírování objektu pro vytváření dat Azure pomocí přeskočení nekompatibilní řádků | Microsoft Docs"
description: "Zjistěte, jak tooadd odolnost proti chybám při aktivitě kopírování objektu pro vytváření dat Azure pomocí přeskočení nekompatibilní řádky během kopírování"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: e7cf6117655910844b292d340674d8d631450a81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Přidání odolnost proti chybám v aktivitě kopírování přeskočení nekompatibilní řádků

Azure Data Factory [aktivity kopírování](data-factory-data-movement-activities.md) nabízí dva způsoby, jak toohandle nekompatibilní řádků při kopírování dat mezi zdroj a jímka úložišti dat:

- Můžete přerušit a selhání hello kopie aktivity po nekompatibilní data došlo (výchozí nastavení).
- Můžete pokračovat v toocopy všechna data hello přidáním odolnost proti chybám a přeskočení nekompatibilní data řádků. Kromě toho se můžete přihlásit nekompatibilní řádky hello úložiště objektů Blob v Azure. Můžete pak zkontrolujte hello protokolu toolearn hello příčinou selhání hello, opravte hello dat na zdroji dat hello a opakujte aktivity kopírování hello.

## <a name="supported-scenarios"></a>Podporované scénáře
Aktivita kopírování podporuje tři scénáře pro zjišťování, přeskočí a protokolování nekompatibilní data:

- **Nekompatibilita mezi hello zdrojového datového typu a nativní typ jímky hello**

    Příklad: kopírování dat ze souboru CSV v objektu Blob úložiště tooa SQL databáze s definici schématu, která obsahuje tři **INT** typ sloupce. Hello řádků souboru CSV, obsahující číselná data, jako například `123,456,789` zkopírují úspěšně toohello podřízený úložiště. Ale hello řádky, které obsahují jiné než číselné hodnoty, jako například `123,456,abc` jsou rozpoznána jako nekompatibilní a se přeskočí.

- **Došlo k neshodě v hello počet sloupců mezi hello zdroj a jímka hello**

    Příklad: kopírování dat ze souboru CSV v objektu Blob úložiště tooa SQL databáze s definici schématu, která obsahuje šest sloupce. Hello souboru CSV, který řádky, které obsahují šesti sloupce jsou úspěšně zkopírovány toohello podřízený úložiště. Hello CSV souboru řádky, které obsahují více nebo méně než šest sloupce jsou rozpoznána jako nekompatibilní a se přeskočí.

- **Porušení primárního klíče při zápisu tooa relační databáze**

    Příklad: kopírování dat z databáze SQL tooa systému SQL server. Primární klíč je definována v hello jímku SQL databáze, ale žádný primární klíč je definována v systému SQL server hello zdroje. Hello duplicitní řádky, které existují v hello zdroj nesmí být zkopírovaný toohello jímky. Aktivita kopírování zkopíruje pouze hello první řádek hello zdroje dat do hello jímky. Hello řádky další zdroje, které obsahují hello duplicitní hodnotu primárního klíče jsou rozpoznána jako nekompatibilní a se přeskočí.

## <a name="configuration"></a>Konfigurace
Hello následující příklad uvádí tooconfigure definici JSON přeskočení hello nekompatibilní řádků v aktivitě kopírování:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| **enableSkipIncompatibleRow** | Povolte přeskočení nekompatibilní řádků při kopírování nebo ne. | True<br/>NEPRAVDA (výchozí) | Ne |
| **redirectIncompatibleRowSettings** | Skupinu vlastností, které lze zadat, když chcete toolog hello nekompatibilní řádků. | &nbsp; | Ne |
| **linkedServiceName** | Hello propojená služba Azure Storage toostore hello protokolu, který obsahuje řádky hello přeskočena. | Název Hello [azurestorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) propojené služby, která odkazuje toohello úložiště instance, že chcete soubor protokolu hello toostore toouse. | Ne |
| **Cesta** | Hello cesta souboru protokolu hello, který obsahuje hello přeskočen řádků. | Zadejte cestu úložiště objektů Blob hello má toouse toolog hello nekompatibilní data. Pokud nezadáte cestu, služba hello vytvoří kontejner pro vás. | Ne |

## <a name="monitoring"></a>Monitorování
Po dokončení aktivity kopírování hello při spuštění, můžete zjistit hello Počet přeskočených řádků v části Sledování hello:

![Monitorování přeskočen nekompatibilní řádků](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Pokud nakonfigurujete toolog hello nekompatibilní řádků, můžete najít soubor protokolu hello v této cestě: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` v souboru protokolu hello, můžete zobrazit hello řádky, které byly přeskočeny a hello hlavní příčinu nekompatibilita hello.

Původní data hello a odpovídající chyba hello jsou protokolovány v souboru hello. Příklad obsahu souboru protokolu hello vypadá takto:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' tootype 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. hello duplicate key value is (data4).
```

## <a name="next-steps"></a>Další kroky
toolearn Další informace o aktivitě kopírování Azure Data Factory najdete v části [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md).
