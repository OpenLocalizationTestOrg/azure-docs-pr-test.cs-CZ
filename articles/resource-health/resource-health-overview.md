---
title: "Přehled stavu prostředků aaaAzure | Microsoft Docs"
description: "Přehled o stavu prostředků Azure"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 06/01/2016
ms.author: BernardoAMunoz
ms.openlocfilehash: b920241b2f64a0695ba2c32e9ccb0c2c3739986d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-overview"></a>Přehled stavu prostředků Azure.
 
Resource Health pomáhá při diagnostice a získání podpory v případě, že potíže s Azure ovlivňují vaše prostředky. To informující o hello aktuální a starší stav svých prostředků a pomáhá zmírnit problémy. Resource Health poskytuje technickou podporu, když potřebujete pomoc při potížích se službami Azure.

Zatímco [stavu Azure](https://status.azure.com) informující o problémů služby, které ovlivňují širokou škálu Azure zákazníků, stav prostředku vám poskytne přizpůsobené řídicí panel hello stav svých prostředků. Stav prostředku se dozvíte, všechny časy hello vaše prostředky byly k dispozici v hello po splatnosti tooAzure problémů služby. To umožňuje snadno můžete toounderstand Pokud porušení SLA. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Co se považuje za zdroj a jak funguje stav prostředku rozhodne, zda prostředek je v pořádku, či nikoliv?
Prostředek se o instanci typu prostředku, které nabízí služby Azure pomocí Azure Resource Manager, například: virtuálního počítače, webovou aplikaci nebo v databázi SQL.

Stav prostředku spoléhá na signály vysílaných tooassess hello různé služby Azure, pokud prostředek je v pořádku, či nikoli. Pokud prostředek není v pořádku, analyzuje stav prostředku hello zdroj dalších informací toodetermine hello problém. Také identifikuje akce, které společnost Microsoft provádí toofix hello problém nebo jaké akce může trvat tooaddress hello způsobí, že hello problému. 

Zkontrolujte hello úplný seznam typů prostředků a stavu kontroluje [stavu prostředků Azure](resource-health-checks-resource-types.md) další podrobnosti o tom, jak se hodnotí stavu.

## <a name="health-status-provided-by-resource-health"></a>Stav poskytované stav prostředků
Hello stav prostředku je jedním z hello následující stavy:

### <a name="available"></a>Dostupné
Služba Hello nezjistila všechny události, které mají vliv hello stavu prostředku hello. V případech, kde hello prostředků se zotavil z neplánované výpadky během hello posledních 24 hodin, zobrazí se hello **nedávno obnovena** oznámení.

![Prostředek stavu k dispozici virtuálního počítače](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Není k dispozici
Hello služba zjistila probíhající platformy nebo události bez platformy hello stav hello prostředků, které mají vliv.

#### <a name="platform-events"></a>Události platformy
Tyto události jsou aktivovány více součástí hello infrastrukturu Azure a jsou oba plánované akce, jako je plánované údržby a neočekávané incidenty jako restartování neplánované hostitele.

Stav prostředku poskytuje další podrobnosti pro hello událost, proces obnovení hello a umožní vám toocontact podpory, i když nebudou mít aktivní podpory smlouvy společnosti Microsoft.

![Prostředek stavu není k dispozici virtuální počítač z důvodu tooplatform událostí](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Události jiné platformy
Tyto události jsou aktivovány akce prováděné uživateli, například zastavení virtuálního počítače nebo dosažení maximálního počtu připojení tooa Redis Cache hello.

![Prostředek stavu není k dispozici virtuální počítač z důvodu události toonon platformy](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Neznámý
Tento stav označuje, že stav prostředku nedostal informace o tento prostředek pro více než 10 minut. Přestože tento stav není spolehlivý údaj o hello stav hello prostředku, je bod důležitých dat ve hello procesu odstraňování potíží:
* Pokud prostředek hello běží jako očekávané hello stav prostředku hello zaktualizuje tooAvailable po několika minutách.
* Pokud dochází k potížím s hello prostředků, hello Neznámý stav může naznačují, že je událost v platformě hello dopad hello prostředků.

![Stav prostředků, které se neznámý virtuálního počítače](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a>Nesprávný stav sestavy
Pokud v libovolném okamžiku budete mít dojem, aktuální stav hello je nesprávný, vám může dejte nám vědět kliknutím **sestavy nesprávný stav**. V případech, kde je postiženo Azure problém doporučujeme vám toocontact podpory z okna stavu prostředků hello. 

![Stav prostředku nesprávný stav sestavy](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Historické informace
Dostupné až too14 dnů od data historie stavu klepnutím na **zobrazit historii** v okně stavu prostředků hello. 

![Stav prostředku historie sestavy](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Začínáme
tooopen Resource health jeden prostředek
1.  Přihlaste se do hello portálu Azure.
2.  Přejděte tooyour prostředků.
3.  V nabídce prostředků hello nachází na levé straně hello, klikněte na tlačítko **stav prostředku**.

![Otevřete v okně prostředků stav prostředků](./media/resource-health-overview/from-resource-blade.png)

Stav prostředku můžete taky přejít kliknutím **další služby**a zadáte **stav prostředku** ve filtru textové pole tooopen hello **Nápověda a podpora** okno. Nakonec klikněte na tlačítko [ **stav prostředku**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Otevřete stav prostředku z další služby](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Další kroky

Podívejte se na tyto prostředky toolearn Další informace o stavu prostředků:
-  [Typy prostředků a stav kontrol ve stavu prostředků Azure.](resource-health-checks-resource-types.md)
-  [Nejčastější dotazy týkající se stavu prostředků Azure.](resource-health-faq.md)




