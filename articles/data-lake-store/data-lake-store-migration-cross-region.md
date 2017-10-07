---
title: aaaAzure migrace mezi oblastmi Data Lake Store | Microsoft Docs
description: "Další informace o migraci mezi oblastmi pro Azure Data Lake Store."
services: data-lake-store
documentationcenter: 
author: swums
manager: amitkul
editor: swums
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/27/2017
ms.author: stewu
ms.openlocfilehash: 561ac821c1bd555886035867678cb685997564eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-data-lake-store-across-regions"></a>Migrovat Data Lake Store v oblastech

Jakmile Azure Data Lake Store k dispozici v nové oblastech, můžete zvolit toodo jednorázové migrace, tootake výhod hello novou oblast. Jaké tooconsider zjistěte, jak naplánovat a dokončení migrace hello.

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Další informace najdete v tématu [vytvořit účet Azure zdarma Dnes](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Data Lake Store ve dvou různých oblastech**. Další informace najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Azure Data Factory**. Další informace najdete v tématu [Úvod tooAzure Data Factory](../data-factory/data-factory-introduction.md).


## <a name="migration-considerations"></a>Posouzení migrace

Nejdřív určete hello strategii migrace, která je nejvhodnější pro aplikace, který zapíše, čte nebo dat v Data Lake Store. Když zvolíte strategie, zvažte požadavky na dostupnosti vaší aplikace a hello výpadek, k níž dojde během migrace. Nejjednodušším přístupem může být například toouse hello "navýšení a shift" cloudu migrace modelu. V tomto přístupu pozastavení při všechna vaše data jsou zkopírovány toohello novou oblast hello aplikace ve vaší stávající oblasti. Po dokončení kopírování hello obnovit svoji aplikaci v nové oblasti hello a pak odstraňte starý účet Data Lake Store hello. Je nutná odstávka během migrace hello.

tooreduce výpadku, můžete okamžitě začít příjem nová data v oblasti nové hello. Až budete mít hello minimální dat potřebných, spusťte aplikaci v nové oblasti hello. Hello pozadí pokračujte v nové oblasti hello toocopy starší data z hello existující Data Lake Store účtu toohello nový účet Data Lake Store. Když použijete tuto metodu, můžete provést novou oblast toohello hello přepínače s malým množstvím výpadky. Pokud byl zkopírován všechny hello starší data, odstraňte starý účet Data Lake Store hello.

Další důležité podrobnosti tooconsider při plánování migrace jsou:

* **Datový svazek**. hello čas a prostředky, které potřebujete k hello migrace má vliv Hello objem dat (v gigabajtech, počet hello soubory a složky a tak dále).

* **Název účtu data Lake Store**. název nového účtu Hello v nové oblasti hello musí být globálně jedinečný. Název hello starý účet Data Lake Store v oblasti Východ USA 2 může být například contosoeastus2.azuredatalakestore.net. Může být název nového účtu Data Lake Store v Severní Evropa contosonortheu.azuredatalakestore.net.

* **Nástroje pro**. Doporučujeme použít hello [aktivita služby Azure Data Factory kopie](../data-factory/data-factory-azure-datalake-connector.md) soubory toocopy Data Lake Store. Objekt pro vytváření dat podporuje přesun dat s vysokým výkonem a spolehlivostí. Uvědomte si, že objekt pro vytváření dat zkopíruje pouze hello hierarchii složek a obsah souborů hello. Je třeba toomanually použít seznamů řízení zprávy všechny přístupu (ACL), které použijete v hello starý účet toohello nový účet. Další informace, včetně cíle výkonnosti pro nejlepší možný scénáře, najdete v části hello [výkonu kopie aktivity a vyladění průvodce](../data-factory/data-factory-copy-activity-performance.md). Pokud chcete data zkopírovat rychleji, bude pravděpodobně nutné toouse další jednotky přesun dat v cloudu. Některé nástroje, jako je AdlCopy, nepodporují kopírování dat mezi oblastmi.  

* **Šířka pásma poplatky**. [Šířka pásma poplatky](https://azure.microsoft.com/en-us/pricing/details/bandwidth/) použít, protože data se přenáší z oblasti Azure.

* **Seznamy ACL na vaše data**. Zabezpečení dat v oblasti nové hello použitím seznamů řízení přístupu toofiles a složky. Další informace najdete v tématu [zabezpečení dat uložených v Azure Data Lake Store](data-lake-store-secure-data.md). Doporučujeme použít hello migrace tooupdate a upravit vaše seznamy ACL. Můžete chtít toouse podobné tooyour aktuální nastavení. Můžete zobrazit hello seznamy ACL, které jsou použité tooany souboru pomocí hello portál Azure, [rutiny prostředí PowerShell](/powershell/module/azurerm.datalakestore/get-azurermdatalakestoreitempermission), nebo sady SDK.  

* **Umístění služby analytics**. Pro nejlepší výkon, analytické služby, jako je Azure Data Lake Analytics nebo Azure HDInsight, musí být v hello stejné oblasti jako vaše data.  

## <a name="next-steps"></a>Další kroky
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
