---
title: aaaFrequently dotazy pro Azure Data Lake Store | Microsoft Docs
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
ms.openlocfilehash: eeabdeef18f3b72901bd1a14283f85dd9c0ead44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Nejčastější dotazy k Azure Data Lake Storu
V tomto článku se dozvíte o nejčastější dotazy související tooAzure Data Lake Store.

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>Jak mohu ještě více chránit svá data před katastrofami na úrovni celé oblasti nebo před náhodným odstraněním?
Hello data ve vašem účtu Azure Data Lake Store je odolný tootransient selhání hardwaru v rámci oblasti prostřednictvím automatizované repliky. To zajišťuje odolnost a vysokou dostupnost, schůzku hello smlouvy SLA pro Azure Data Lake Store. Zde je některé pokyny na tom, jak toofurther chránit vaše data z výjimečných celou oblast výpadků nebo náhodným odstraněním.

### <a name="disaster-recovery-guidance"></a>Pokyny pro zotavení po havárii
Je důležité pro každý zákazník tooprepare vlastní plánu zotavení po havárii. Naleznete toohello dokumentace k Azure pod toobuild vašem plánu zotavení po havárii. Tady jsou některé prostředky, které vám můžou s vytvořením vlastního plánu pomoct.

* [Zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technické pokyny k odolnosti Azure](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>Osvědčené postupy
Doporučujeme zkopírovat vaše důležitá data tooanother účtu Data Lake Store v jiné oblasti s četnost zarovnán toohello potřebám vašeho plánu zotavení po havárii. Existují různé metody toocopy dat včetně [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [prostředí Azure PowerShell](data-lake-store-get-started-powershell.md) nebo [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md). Azure Data Factory je užitečná služba pro opakované vytváření a nasazování kanálů k přesunu dat.

Pokud oblastního výpadku, můžete poté přistoupit vaše data v hello oblasti, kde byla hello data zkopírována. Můžete monitorovat hello [řídicím panelu stavu služeb Azure](https://azure.microsoft.com/status/) toodetermine hello stav služby Azure přes hello zeměkouli.

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Pokyny pro obnovení v případě poškození nebo nechtěného odstranění dat
I když Azure Data Lake Store zajišťuje odolnost dat prostřednictvím automatizovaných replik, nemůže vaší aplikaci (nebo vývojářům/uživatelům) zabránit v poškození nebo nechtěném odstranění dat.

#### <a name="best-practices"></a>Osvědčené postupy
tooprevent náhodným odstraněním, doporučujeme nejprve nastavit zásady hello správný přístup pro účet Data Lake Store.  To zahrnuje použití [prostředků Azure zámky](../azure-resource-manager/resource-group-lock-resources.md) toolock dolů důležité prostředky a také použití účtu a soubor řízení úrovně přístupu, které jsou pomocí hello k dispozici [funkce zabezpečení Data Lake Store](data-lake-store-security-overview.md). Doporučujeme také pravidelně vytvářet kopie důležitých dat pomocí [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShellu](data-lake-store-get-started-powershell.md) nebo [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) v jiném účtu Data Lake Store, složce nebo předplatném Azure.  To může být použité toorecover z dat poškození nebo odstranění incidentu. Azure Data Factory je užitečná služba pro opakované vytváření a nasazování kanálů k přesunu dat.

Můžete také povolit organizace [protokolování diagnostiky](data-lake-store-diagnostic-logs.md) pro jejich Azure Data Lake Store účtu toocollect záznamy auditu přístupu data, která poskytuje informace o kdo může mít odstranit nebo aktualizovat soubor.

## <a name="next-steps"></a>Další kroky
* [Začínáme s Azure Data Lake Storem](data-lake-store-get-started-portal.md)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)

