---
title: "aaaUnderstand vaše Azure podrobné využití | Microsoft Docs"
description: "Zjistěte, jak tooread a pochopit hello části vaší podrobné informace o použití sdíleného svazku clusteru pro vaše předplatné Azure"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: tonguyen
ms.openlocfilehash: c9284bf94bfa9f36cdd5d39e653a35def7c1aa34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-terms-on-your-microsoft-azure-detailed-usage-charges"></a>Pochopit podmínky na vaše poplatky Azure podrobné informace o použití Microsoftu 
Hello podrobné informace o použití, který obsahuje soubor CSV poplatky za každý den a úrovni využití měření poplatky za hello aktuální fakturační období. 

tooget souboru podrobné informace o použití najdete v části [jak tooget vaše Azure fakturace fakturace a dat o denním využití](billing-download-azure-invoice-daily-usage-date.md).
Je k dispozici ve formátu souboru hodnot oddělených čárkami (.csv), který můžete otevřít v aplikaci tabulky. Pokud se zobrazují dvě verze, které jsou k dispozici, stáhněte si verze 2. Který je hello nejnovější formát souboru.

Poplatky za používání jsou hello celkový **měsíční** poplatky na předplatném. Poplatky za používání nemáte vzít v úvahu žádné kredity ani slevy.

## <a name="detailed-terms-and-descriptions-of-your-detailed-usage-file"></a>Podrobné podmínky a popisy podrobné informace o použití souboru
Hello následující části popisují důležité termíny hello ukazuje verze 2 hello podrobné informace o použití souboru.

### <a name="statement"></a>Příkaz
Hello horní části hello podrobné použití sdíleného svazku clusteru souboru ukazuje hello služby, které jste použili při hello měsíc fakturačního období. Hello následující tabulka uvádí hello podmínky a popisy uvedené v této sekci.

| Označení | Popis |
| --- | --- |
|Fakturační období |Pokud jste použili hello měřidla fakturační období Hello |
|Kategorie měření |Identifikuje hello nejvyšší úrovně služby pro použití hello |
|Podkategorie měření |Definuje typ hello služba Azure, která může mít vliv na rychlost hello |
|Název měření |Identifikuje hello měrné jednotky pro měření hello spotřebovávanou |
|Oblast měření |Určuje umístění hello hello datacenter pro určité služby, které jsou za cenu na základě umístění datového centra |
|Skladová jednotka (SKU) |Určuje identifikátor hello jedinečných systémových pro každý Azure měření |
|Jednotka |Identifikuje hello jednotku, která je účtován hello služby v. Například GB, hodin, 10 000 s. |
|Spotřebované množství |Hello množství měření hello používá během fakturačního období hello |
|Zahrnuté množství |Hello množství hello monitorování, která je zahrnutá ve vaší aktuální fakturační období bez poplatku |
|Překročené množství |Ukazuje hello rozdíl mezi hello objemu spotřebovaného a hello zahrnuté množství. Fakturuje se toto množství. Průběžné platby nabídky se žádné zahrnuté množství s nabídkou hello je tato celková hello stejná hodnota jako hello spotřebované množství. |
|V rámci závazku |Zobrazuje hello měření poplatky, které jsou odečten od vaší závazků částky přidružené vaši nabídku 6 nebo 12 měsíců. Měřicí poplatky jsou odčítat v chronologickém pořadí. |
|Měna |Hello měny používanou ve vaší aktuální fakturační období |
|Překročení |Zobrazuje hello měření poplatky, které překračují velikost vašeho závazků přidružené vaši nabídku 6 nebo 12 měsíců |
|Sazba závazku |Zobrazuje rychlost závazků hello na základě hello celkový závazků částky přidružené vaši nabídku 6 nebo 12 měsíců |
|Sazba |Hello sazba, která se vám účtovat na jednotku fakturovatelného času |
|Hodnota |Zobrazuje výsledek hello součin hodnot hello Nadlimitní množství sloupec podle sloupce míry hello. Pokud hello objemu spotřebovaného nepřekročí hello zahrnuté množství, není nijak zpoplatněn v tomto sloupci. |

### <a name="daily-usage"></a>Denní využití

Hello denní využití části ze souboru CSV hello zobrazuje podrobnosti o použití ovlivňující hello fakturační sazby. Hello následující tabulka uvádí hello podmínky a popisy uvedené v této sekci.

| Označení | Popis |
| --- | --- |
|Datum využití |Hello datum, kdy byl použit hello měření |
|Kategorie měření |Identifikuje hello nejvyšší úrovně služby, pro kterou toto použití patří |
|ID měření |Hello účtují měření identifikátor, který byl použit tooprice fakturace využití |
|Podkategorie měření |Definuje typ hello služby Azure, který může mít vliv na rychlost hello |
|Název měření |Identifikuje hello měrné jednotky pro měření hello spotřebovávanou |
|Oblast měření |Určuje umístění hello hello datacenter pro určité služby, které jsou za cenu na základě umístění datového centra |
|Jednotka |Určuje jednotku hello tohoto měřidla hello je účtován v. Například GB, hodin, 10 000 s. |
|Spotřebované množství |Hello množství hello monitorování, které bylo spotřebováno pro daný den |
|Umístění prostředku |Identifikuje hello datacenter se spuštěným systémem měření hello |
|Spotřebovaná služba |Hello služby platformy Azure, který jste použili |
|Skupina prostředků |v které hello nasazené měření běží ve skupině prostředků Hello. <br/><br/>Další informace naleznete v tématu [Přehled Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
|ID instance | identifikátor Hello hello měření. <br/><br/> identifikátor Hello obsahuje hello název, který jste zadali pro měření hello při vytváření. Je název prostředku hello buď hello nebo hello plně kvalifikovaný ID prostředku. Další informace najdete v tématu [rozhraní API služby Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources). |
|Značky | Značka přiřadit toohello měření. Pomocí značky toogroup fakturace záznamů.<br/><br/>Můžete například značky toodistribute náklady hello oddělení, který používá hello měření. Služby, které podporují generování značek jsou virtuální počítače, úložiště a síťové služby, které jsou zajišťované pomocí hello [rozhraní API služby Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources). Další informace najdete v tématu [uspořádání prostředků Azure pomocí značek](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Další informace |Metadata specifická pro služby. Například bitové kopie typ pro virtuální počítač. |
|Informace o službě 1 |název projektu Hello patří služby hello tooon vaše předplatné |
|Informace o službě 2 |Starší verze pole, které jsou zaznamenány volitelná metadata specifickou pro službu |

## <a name="how-do-i-make-sure-that-hello-charges-in-my-detailed-usage-file-are-correct"></a>Jak mohu učinit se, že poplatky hello v mé podrobné informace o použití souboru jsou správné?
Pokud je v souboru podrobné informace o použití, která chcete podrobnosti na zpoplatněny, přečtěte si téma [porozumět vaší faktuře pro Microsoft Azure.](./billing-understand-your-bill.md)

## <a name="external"></a>Co externí poplatky za služby?
Externích služeb (také označované jako Marketplace objednávky) jsou k dispozici nezávislé služby dodavatelé a se účtují samostatně. poplatky za Hello nezobrazovat hello Azure faktury. Další, najdete v části toolearn [pochopit Azure externí poplatky za](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?) získat rychle vyřešit problém.
