---
title: "aaaAzure Data Lake Analytics kvótami | Microsoft Docs"
description: "Zjistěte, jak tooadjust a zvyšte kvótu omezení v Azure Data Lake Analytics (ADLA) účty."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 875c4d00e0c57414031e50754495c02162bdca48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a>Maximální kvóty Azure Data Lake Analytics

Zjistěte, jak tooadjust a zvyšte kvótu omezení v Azure Data Lake Analytics (ADLA) účty. Znalost těchto mezních hodnot vám mohou pomoci porozumět chování vaší úlohy U-SQL. Kvótami všechny jsou logicky, takže může zvýšit maximální limit hello oslovit toous.

## <a name="azure-subscriptions-limits"></a>Omezení předplatných Azure

**Maximální počet ADLA účty podle předplatného:** 5

 Toto je maximální počet ADLA účty, které můžete vytvořit na jedno předplatné hello. Pokud se pokusíte toocreate šestý ADLA účet, bude dojde k chybě "Bylo dosaženo maximální počet hello účty Data Lake Analytics, povolené (5) v oblasti pod názvem odběru". V takovém případě Odstraňte nepoužívané účty ADLA nebo oslovení toous podle [otevřením lístku podpory](#increase-maximum-quota-limits).

## <a name="adla-account-limits"></a>Limity účtu ADLA

**Maximální počet jednotek Analytics (Austrálie) na účet:** 250

Toto je maximální počet hello Austrálie, které můžou běžet současně ve vašem účtu. Pokud váš celkový počet spuštění Austrálie pro všechny úlohy překračuje tento limit, novější úlohy se zařadí do fronty automaticky. Například:

* Pokud máte pouze jednu úlohu s 250 Austrálie při odesílání druhý úlohy se budou čekat ve frontě úloh hello hello první úloha dokončena.
* Pokud již máte pět spuštěné úlohy a každý používá 50 Austrálie při odesílání šesté úlohu, která potřebuje 20 Austrálie čeká ve frontě úloh hello, dokud jsou 20 Austrálie k dispozici.

**Maximální počet souběžných úloh U-SQL na účet:** 20

Toto je maximální počet úloh, které můžou běžet současně ve vašem účtu hello. Vyšší než tato hodnota novější úlohy se zařadí do fronty automaticky.

## <a name="adjust-adla-quota-limits-per-account"></a>Upravit ADLA kvótami každý účet

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte existující účet ADLA.
3. Klikněte na **Vlastnosti**.
4. Upravit **paralelismus** a **souběžných úloh** toosuit vašim potřebám.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a>Zvýšit maximální kvóty

1. Otevřete žádost o podporu na portálu Azure.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Vyberte typ problému hello **kvóty**.
3. Vyberte vaše **předplatné** (ujistěte se, není "zkušební" předplatné).
4. Vyberte typ kvóty **Data Lake Analytics**.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. V okně problém hello popisují požadovanou zvyšte limit s **podrobnosti** z Proč potřebujete tuto kapacitu navíc.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. Ověřte kontaktní informace a vytvořte žádost o podporu hello.

Společnost Microsoft nezkontroluje vaši žádost a pokusí tooaccommodate vaše podnikání co nejdříve.

## <a name="next-steps"></a>Další kroky

* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Správa Azure Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-manage-use-powershell.md)
* [Monitorování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
