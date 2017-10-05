---
title: "Nejčastější dotazy k Azure Data Lake Storu | Dokumentace Microsoftu"
description: "Pokyny k odstraňování potíží nebo zmírnění problémů s Azure Data Lake Storem"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf7fd555-3e30-43ce-b28c-c3ad0a241fdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 44aaec9dc145b47b809a3ad4bc244eb9dfead24c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Nejčastější dotazy k Azure Data Lake Storu
V tomto článku se dozvíte o nejčastějších dotazech týkajících se Azure Data Lake Storu.

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>Jak mohu ještě více chránit svá data před katastrofami na úrovni celé oblasti nebo před náhodným odstraněním?
Data ve vašem účtu Azure Data Lake Store jsou odolná vůči krátkodobému selhání hardwaru v rámci oblasti díky automatizovaným replikám. Tím se zajišťuje odolnost a vysoká dostupnost splňující podmínky smlouvy SLA pro Azure Data Lake Store. Nabízíme pokyny, jak data ještě více ochránit před výjimečnými výpadky celé oblasti nebo nechtěným odstraněním.

### <a name="disaster-recovery-guidance"></a>Pokyny pro zotavení po havárii
Je důležité, aby si každý zákazník připravil vlastní plán zotavení po havárii. Pokud chcete vytvořit vlastní plán zotavení po havárii, použijte prosím dokumentaci k Azure níže. Tady jsou některé prostředky, které vám můžou s vytvořením vlastního plánu pomoct.

* [Zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technické pokyny k odolnosti Azure](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>Osvědčené postupy
Doporučujeme kopírovat důležitá data do jiného účtu Data Lake Store v jiné oblasti tak často, jak odpovídá potřebám vašeho plánu zotavení po havárii. Pro kopírování dat existují nejrůznější způsoby včetně [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShellu](data-lake-store-get-started-powershell.md) nebo [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md). Azure Data Factory je užitečná služba pro opakované vytváření a nasazování kanálů k přesunu dat.

Pokud dojde k výpadku oblasti, máte pak k datům přístup v oblasti, kam se data zkopírovala. Stav služeb Azure na celém světě můžete sledovat prostřednictvím [řídicího panelu stavu Azure](https://azure.microsoft.com/status/).

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Pokyny pro obnovení v případě poškození nebo nechtěného odstranění dat
I když Azure Data Lake Store zajišťuje odolnost dat prostřednictvím automatizovaných replik, nemůže vaší aplikaci (nebo vývojářům/uživatelům) zabránit v poškození nebo nechtěném odstranění dat.

#### <a name="best-practices"></a>Osvědčené postupy
Aby se zabránilo nechtěnému odstranění, doporučujeme nejdříve nastavit správné zásady přístupu k účtu Data Lake Store.  To zahrnuje použití [ámků prostředků Azure](../azure-resource-manager/resource-group-lock-resources.md) k uzamčení důležitých prostředků a také použití řízení přístupu na úrovni souborů a rolí pomocí dostupných [funkcí zabezpečení služby Data Lake Store](data-lake-store-security-overview.md). Doporučujeme také pravidelně vytvářet kopie důležitých dat pomocí [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShellu](data-lake-store-get-started-powershell.md) nebo [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) v jiném účtu Data Lake Store, složce nebo předplatném Azure.  Kopie pak můžete využít k obnovení poškozených nebo odstraněných dat. Azure Data Factory je užitečná služba pro opakované vytváření a nasazování kanálů k přesunu dat.

Organizace také můžou povolit [protokolování diagnostiky](data-lake-store-diagnostic-logs.md) pro účet Azure Data Lake Store ke shromažďování záznamů pro audit přístupu k datům. Ty poskytují informace o tom, kdo mohl soubor odstranit nebo aktualizovat.

## <a name="next-steps"></a>Další kroky
* [Začínáme s Azure Data Lake Storem](data-lake-store-get-started-portal.md)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)

