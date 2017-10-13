---
title: "Přehled Azure Resource health | Microsoft Docs"
description: "Přehled o stavu prostředků Azure"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: 040d58a81a9b41fe660e4276d698bf884f90bb6c
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="azure-resource-health-overview"></a>Přehled stavu prostředků Azure.
 
Resource Health pomáhá při diagnostice a získání podpory v případě, že potíže s Azure ovlivňují vaše prostředky. Informuje o aktuálním a dřívějším stavu prostředků a pomáhá zmírnit problémy. Resource Health poskytuje technickou podporu, když potřebujete pomoc při potížích se službami Azure.

Zatímco [stavu Azure](https://status.azure.com) informující o problémů služby, které ovlivňují širokou škálu Azure zákazníků, stav prostředku vám poskytne přizpůsobené řídicí panel stavu prostředků. Stav prostředků ukazuje všechny časy, které prostředky byly v minulosti kvůli problémům s služby Azure k dispozici. Zjednodušuje porozumět, pokud byl došlo k porušení SLA. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Co se považuje za zdroj a jak funguje stav prostředku rozhodne, zda prostředek je v pořádku, či nikoliv?
Prostředek se o instanci typu prostředku, které nabízí služby Azure pomocí Azure Resource Manager, například: virtuálního počítače, webovou aplikaci nebo v databázi SQL.

Stav prostředku spoléhá na signály vysílaných různých služeb Azure k vyhodnocení, pokud prostředek je v pořádku, či nikoli. Pokud prostředek není v pořádku, analyzuje stav prostředku dodatečné informace k určení příčiny problému. Také určuje opatření Microsoft trvá vyřešit problém, nebo jaké akce může trvat vyřešit příčinu problému. 

Úplný seznam typů prostředků a stavu zkontroluje [stavu prostředků Azure](resource-health-checks-resource-types.md) další podrobnosti o tom, jak se hodnotí stavu.

## <a name="health-status-provided-by-resource-health"></a>Stav poskytované stav prostředků
Stav prostředku je jedním z následujících stavů:

### <a name="available"></a>Dostupné
Služba zjistila všechny události, které mají vliv stav prostředku. V případech, kdy prostředek se obnovila neplánované výpadky během posledních 24 hodin se zobrazí **nedávno obnovena** oznámení.

![Prostředek stavu k dispozici virtuálního počítače](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Není k dispozici
Služba zjistila probíhající platformy nebo jiné platformy událost stavu prostředku, které mají vliv.

#### <a name="platform-events"></a>Události platformy
Tyto události jsou aktivovány více součástí infrastruktury Azure a jsou oba plánované akce, jako je plánované údržby a neočekávané incidenty jako restartování neplánované hostitele.

Stav prostředku poskytuje další podrobnosti o události procesu obnovení a můžete se obrátit na podporu i v případě, že nemáte active podpory smlouvy společnosti Microsoft.

![Prostředek stavu není k dispozici virtuální počítač z důvodu události platformy](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Události jiné platformy
Tyto události jsou aktivovány akce prováděné uživateli, například zastavení virtuálního počítače nebo dosažení maximálního počtu připojení k Redis Cache.

![Prostředek stavu není k dispozici virtuální počítač z důvodu události jiné platformy](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Neznámý
Tento stav označuje, že stav prostředku nedostal informace o tento prostředek pro více než 10 minut. Přestože tento stav není spolehlivý Indikace stavu prostředku, je bod důležitých dat ve proces řešení potíží:
* Pokud prostředek běží podle očekávání stav prostředku se aktualizuje a k dispozici po několika minutách.
* Pokud dochází k potížím s hledaným prostředkem, neznámý stav může naznačovat, že je prostředek dopad na událost samotné platformy.

![Stav prostředků, které se neznámý virtuálního počítače](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a>Nesprávný stav sestavy
Pokud v libovolném okamžiku budete mít dojem, aktuální stav je nesprávný, vám může dejte nám vědět kliknutím **sestavy nesprávný stav**. V případech, kde je postiženo Azure problém doporučujeme kontaktovat podporu v okně prostředků stavu. 

![Stav prostředku nesprávný stav sestavy](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Historické informace
Dostanete až na 14 dní data historie stavu kliknutím **zobrazit historii** v okně prostředků stavu. 

![Stav prostředku historie sestavy](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Začínáme
Chcete-li otevřít Resource health jeden prostředek
1.  Přihlaste se k portálu Azure.
2.  Přejděte do prostředku.
3.  V nabídce prostředků se nachází na levé straně klikněte na tlačítko **stav prostředku**.

![Otevřete v okně prostředků stav prostředků](./media/resource-health-overview/from-resource-blade.png)

Stav prostředku můžete taky přejít kliknutím **další služby**a zadáte **stav prostředku** do textového pole filtru otevřete **Nápověda a podpora** okno. Nakonec klikněte na tlačítko [ **stav prostředku**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Otevřete stav prostředku z další služby](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Další kroky

Podívejte se na tyto prostředky Další informace o stavu prostředků:
-  [Typy prostředků a stav kontrol ve stavu prostředků Azure.](resource-health-checks-resource-types.md)
-  [Nejčastější dotazy týkající se stavu prostředků Azure.](resource-health-faq.md)




