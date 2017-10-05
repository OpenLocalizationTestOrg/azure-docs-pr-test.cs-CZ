---
title: "Škálování aplikace v Azure | Microsoft Docs"
description: "Zjistěte, jak škálování aplikace v Azure App Service k přidání kapacity a funkcí."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.openlocfilehash: 75ddbacbd4dd14597b786d26f0730477f6c85811
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-up-an-app-in-azure"></a>Škálování aplikace v Azure
Tento článek ukazuje, jak škálování aplikace v Azure App Service. Existují dva pracovní postupy pro škálování, škálování nahoru a škálování a tento článek vysvětluje rozšiřování škálování využívajících pracovního postupu.

* [Vertikální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): získat další procesoru, paměti, místa na disku a speciálních funkcí, jako vyhrazené virtuální počítače (VM), vlastní domény a certifikáty, přípravné sloty, automatické škálování a další. Škálovat změnou cenové úrovně plánu služby App Service, který je součástí vaší aplikace.
* [Horizontální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): zvyšte počet instancí virtuálního počítače, které spuštění aplikace.
  Můžete škálovat na až 20 instance, v závislosti na cenovou úroveň. [Prostředí App Service](../app-service/app-service-app-service-environments-readme.md) v **Premium** vrstvy další zvýší spočítat Škálováním na více systémů na 50 instancí. Další informace o škálování najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md). Zde najdete informace o tom, jak používat automatické škálování, což je pro škálování počtu instancí automaticky na základě předdefinovaných pravidel a plány.

Nastavení škálování trvat jenom sekund použít a vliv na všechny aplikace ve vaší [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
Změna kódu nebo znovu nasadit aplikace nevyžadují.

Informace o cenách a funkce jednotlivých plány služby App Service najdete v tématu [podrobnosti o cenách na aplikaci služby](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Předtím, než přepnete z plán služby App Service **volné** úroveň, je nutné nejprve odebrat [limitů útraty](https://azure.microsoft.com/pricing/spending-limits/) nastavené pro vaše předplatné Azure. Zobrazit nebo změnit možnosti pro vaše předplatné Microsoft Azure App Service naleznete v tématu [předplatná Microsoft Azure][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Škálování cenové úrovně
1. V prohlížeči otevřete [portál Azure][portal].
2. V okně vaší aplikace, klikněte na tlačítko **všechna nastavení**a potom klikněte na **vertikálně navýšit kapacitu**.
   
    ![Přejděte do škálovat aplikaci Azure.][ChooseWHP]
3. Zvolte úroveň a pak klikněte na **vyberte**.
   
    **Oznámení** karta bude flash zelená **úspěch** po dokončení operace.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Škálování související informační zdroje
Pokud vaše aplikace závisí na jiné služby, například Azure SQL Database nebo úložiště Azure, můžete škálovat také tyto prostředky na základě potřeb. Tyto prostředky nejsou změněna pomocí plán služby App Service a musí být změněna samostatně.

1. V **Essentials**, klikněte **skupiny prostředků** odkaz.
   
    ![Škálování souvisejících prostředků Azure aplikace](./media/web-sites-scale/RGEssentialsLink.png)
2. V **Souhrn** součástí **skupiny prostředků** okno, klepněte na prostředek, který chcete škálovat. Následující snímek obrazovky ukazuje prostředků databáze SQL a prostředek služby Azure Storage.
   
    ![Přejděte do okna prostředků skupiny škálování aplikace Azure](./media/web-sites-scale/ResourceGroup.png)
3. Pro databáze SQL prostředek, klikněte na tlačítko **nastavení** > **cenová úroveň** škálování cenové úrovně.
   
    ![Škálování SQL databáze back-end pro vaše aplikace Azure](./media/web-sites-scale/ScaleDatabase.png)
   
    Můžete také zapnout [geografická replikace](../sql-database/sql-database-geo-replication-overview.md) pro instanci databáze SQL.
   
    Prostředek Azure Storage, klikněte na tlačítko **nastavení** > **konfigurace** škálování možnosti úložiště.
   
    ![Škálování účtu úložiště Azure používat aplikaci Azure](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a>Další informace o funkcích vývojáře
V závislosti na cenovou úroveň jsou k dispozici následující funkce orientovaných na vývojáře:

### <a name="bitness"></a>Počet bitů
* **Základní**, **standardní**, a **Premium** vrstev podporovat 64bitové a 32bitové aplikace.
* **Volné** a **sdílené** plán vrstev podporují pouze 32bitové aplikace.

### <a name="debugger-support"></a>Podpora ladění
* Ladicí program podpora je k dispozici pro **volné**, **sdílené**, a **základní** režimy na jedno připojení na plán služby App Service.
* Ladicí program podpora je k dispozici pro **standardní** a **Premium** režimy v pěti souběžných připojení na plán služby App Service.

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a>Další informace o dalších funkcích
* Podrobné informace o všechny ostatní funkce ve službě App Service plánů, včetně ceny a funkcích pro všechny uživatele (včetně vývojáři), najdete v části [podrobnosti o cenách na aplikaci služby](https://azure.microsoft.com/pricing/details/web-sites/).

> [!NOTE]
> Pokud chcete začít pracovat s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/) kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Je vyžadována žádná platební karta a bez jakýchkoli závazků.
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a>Další kroky
* Chcete-li začít s Azure, přečtěte si téma [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/).
* Informace o cenách, podpory a smlouvy o úrovni služeb získáte pomocí následujících odkazů.
  
    [Podrobnosti o cenách přenos dat](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Plánům podpory Azure Microsoft](https://azure.microsoft.com/support/plans/)
  
    [smlouvám o úrovni služeb](https://azure.microsoft.com/support/legal/sla/)
  
    [Podrobnosti o cenách na SQL databázi](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Virtuální počítač a velikost cloudových služeb pro Microsoft Azure][vmsizes]
  
    [Podrobnosti o cenách služby App Service](https://azure.microsoft.com/pricing/details/app-service/)
  
    [Podrobnosti o - připojení SSL cenách služby App Service](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* Informace o službě Azure App Service osvědčených postupů, včetně vytváření škálovatelná a flexibilní architektuře, najdete v části [osvědčené postupy: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).
* Videa o škálování služby App Service aplikace najdete v následujících zdrojích informací:
  
  * [Kdy škálovat weby Azure - s Stefan Schackow](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Automatické škálování webů Azure, využití procesoru nebo naplánované – s Stefan Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [Jak Azure měřítko weby - s Stefan Schackow](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
