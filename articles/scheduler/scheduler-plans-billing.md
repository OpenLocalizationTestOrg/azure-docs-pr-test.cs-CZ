---
title: "aaaPlans a fakturace ve službě Azure Scheduler"
description: "Plány a fakturace ve Azure Scheduler"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 051b8eeb1ea19678b3cef4db3237ebf04c8b0e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plans-and-billing-in-azure-scheduler"></a>Plány a fakturace ve Azure Scheduler
## <a name="job-collection-plans"></a>Plány kolekce úloh
Kolekce úloh jsou fakturovatelné entity hello ve službě Azure Scheduler. Kolekce úloh obsahují určitý počet úloh a mají tři plány – Free, Standard a Premium –, které jsou popsány níže.

| **Plán kolekce úloh** | **Maximální počet úloh na kolekci úloh** | **Maximální počet opakování** | **Kolekce maximální počet úloh na předplatné** | **Omezení** |
|:--- |:--- |:--- |:--- |:--- |
| **Volné** |5 úloh na kolekci úloh |Jednou za hodinu. Nelze provést úlohy častěji než jednou za hodinu |Předplatné je povoleno až too1 bezplatná kolekce úloh |Nelze použít [objekt odchozí autorizace HTTP](scheduler-outbound-authentication.md) |
| **Standard** |50 úloh na kolekci úloh |Jednou za minutu. Nelze provést úlohy častěji než jednou za minutu |Předplatné je povoleno do kolekce too100 standardní úloh |Sada funkcí přístup toofull scheduleru |
| **P10 úrovně Premium** |50 úloh na kolekci úloh |Jednou za minutu. Nelze provést úlohy častěji než jednou za minutu |Předplatné je povoleno až too10 000 kolekcí úloh P10 úrovně Premium. <a href="mailto:wapteams@microsoft.com">Kontaktujte nás</a> Další informace. |Sada funkcí přístup toofull scheduleru |
| **P20 Premium** |1000 úloh na kolekci úloh |Jednou za minutu. Nelze provést úlohy častěji než jednou za minutu |Předplatné je povoleno až too10 000 P20 Premium kolekcí úloh. <a href="mailto:wapteams@microsoft.com">Kontaktujte nás</a> Další informace. |Sada funkcí přístup toofull scheduleru |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Upgrady a dírám plány kolekce úloh
Můžete upgradovat nebo starší verzi plán kolekce úloh kdykoli mezi hello plánech Free, Standard a Premium. Ale při přechod na starší verzi tooa bezplatná kolekce úloh, hello přechod na starší verzi může selhat pro jednu z následujících důvodů hello:

* Kolekce úloh free již existuje v rámci předplatného hello
* Úloha v kolekci úloh hello má vyšší opakování, než je povoleno pro úlohy v kolekcích úloh free. maximální počet opakování Hello povolené v kolekci úloh free je jednou za hodinu
* Existuje více než 5 úloh v kolekci úloh hello
* V kolekci úloh hello má protokolu HTTP nebo HTTPS akci, která se používá [objekt odchozí autorizace HTTP](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Plány fakturace a Azure
Předplatné není účtován bezplatné kolekce úloh. Pokud máte více než 100 kolekce standardní úloh (10 standardní fakturace jednotky), pak je lepší toohave pozornosti všechny kolekce úloh v plánu premium hello.

Pokud máte jednu kolekci standardní úlohy a kolekce úloh jeden premium, jste fakturovaná jednu standardní fakturace jednotku *a* jednu jednotku fakturace premium. Hello účtuje poplatky za služby Plánovač na základě počtu hello aktivní úloha kolekce, které jsou nastavené tooeither standard nebo premium; To se vysvětluje dále v následujících dvou částech hello.

## <a name="standard-billable-units"></a>Standardní fakturovatelný jednotky
Standardní fakturovatelný jednotka může obsahovat až too10 standardní úlohy kolekce. Vzhledem k tomu, že kolekce standardní úloh může obsahovat maximálně too50 úloh na kolekci úloh, umožňuje jednu jednotku standardní fakturace předplatného toohave až too500 úlohy – až tooalmost 22 milionů úlohy spuštěních za měsíc.

Pokud je mezi 1 a 10 kolekce standardní úloh, platit budete pro 1 jednotka standard fakturace. Pokud máte mezi kolekcí standardní úloh 11 a 20, platit budete pro 2 standardní fakturace jednotky. Pokud máte 21 až 30 standardní úlohy kolekce, platit budete pro 3 standardní fakturace jednotky a tak dále.

## <a name="p10-premium-billable-units"></a>P10 úrovně Fakturovatelný jednotky Premium
Fakturovatelný jednotka premium P10 může obsahovat až too10 000 kolekcí úloh P10 úrovně premium. Vzhledem k tomu, že kolekce P10 premium úloh může obsahovat maximálně too50 úloh na kolekci úloh, jednu jednotku fakturace premium umožňuje předplatné toohave až too500 000 úloh – až tooalmost 22 miliardy úlohy spuštěních za měsíc.

Pokud je mezi 1 a 10 000 kolekcí úloh premium, platit budete pro 1 jednotka premium fakturace P10. Pokud máte mezi 10,001 a kolekce úloh 20 000 premium, vám budou fakturovat 2 P10 premium fakturace jednotky a tak dále.

Proto P10 premium úlohu, kterou kolekce mají hello stejné funkce jako kolekce standardní úlohy hello ale poskytují zalomení cenu v případě, že vaše aplikace vyžaduje velké množství kolekcí úloh.

## <a name="p20-premium-billable-units"></a>P20 Fakturovatelný jednotky úrovni Premium
Fakturovatelný jednotka premium P20 může obsahovat až too5 000 kolekcí úloh premium P20. Vzhledem k tomu, že kolekce úloh premium P20 může mít až too1 000 úloh na kolekci úloh umožňuje jednu jednotku fakturace premium předplatné toohave až too5, 000, 000 úloh – až tooalmost 220 miliardy úlohy spuštěních za měsíc.

Kolekce úloh premium p20 poskytuje hello stejné schopnosti jako kolekce úloh P10 úrovně premium, ale také podporuje větší počet úloh na kolekci úloh a vyšší celkový počet úloh celkové než P10 premium povolení jste toohave další škálovatelnost.

## <a name="billing-and-active-status"></a>Fakturace a aktivní stav
Kolekce úloh jsou vždy aktivní, pokud celé předplatné přešel do některé dočasné zakázaném stavu z důvodu toobilling problémy. Hello pouze tooensure způsobem, že není účtován kolekce úloh je tooeither nastavit toohello *volné* plán nebo toodelete kolekce úloh hello.

I když může zakažte všechny úlohy v kolekci úloh v rámci jedné operace, nezmění hello fakturace stav kolekce úloh hello – kolekce úloh hello bude *stále* platit. Kolekce úloh prázdný podobně, jsou považovány za aktivní a bude účtován.

## <a name="pricing"></a>Ceny
Podrobnosti o cenách najdete v tématu [ceny Plánovač](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Viz také
 [Co je Scheduler?](scheduler-intro.md)

 [Koncepty, terminologie a hierarchie entit Azure Scheduleru](scheduler-concepts-terms.md)

 [Začněte používat plánovače v hello portálu Azure](scheduler-get-started-portal.md)

 [REST API Azure Scheduleru – referenční informace](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro Azure Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Vysoká dostupnost a spolehlivost Azure Scheduleru](scheduler-high-availability-reliability.md)

 [Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru](scheduler-limits-defaults-errors.md)

 [Odchozí ověření Azure Scheduleru](scheduler-outbound-authentication.md)

