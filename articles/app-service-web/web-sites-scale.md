---
title: aaaScale aplikaci v Azure | Microsoft Docs
description: "Zjistěte, jak tooscale aplikaci v Azure App Service tooadd kapacitu a funkce."
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
ms.openlocfilehash: 97f46f77aeef95aec90d38e8396a023820e3caeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-up-an-app-in-azure"></a>Škálování aplikace v Azure
Tento článek ukazuje, jak tooscale vaší aplikace v Azure App Service. Existují dva pracovní postupy pro škálování, škálování nahoru a škálování a tento článek vysvětluje hello rozšiřování škálování využívajících pracovního postupu.

* [Vertikální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): získat další procesoru, paměti, místa na disku a speciálních funkcí, jako vyhrazené virtuální počítače (VM), vlastní domény a certifikáty, přípravné sloty, automatické škálování a další. Škálovat tak, že změníte hello cenovou úroveň plánu služby App Service, který je součástí vaší aplikace.
* [Horizontální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): zvyšte počet hello instancí virtuálních počítačů, které spuštění aplikace.
  Je možné škálovat na tooas mnoho jako 20 instance, v závislosti na cenovou úroveň. [Prostředí App Service](../app-service/app-service-app-service-environments-readme.md) v **Premium** vrstvy další zvýší vaše instance too50 počet Škálováním na více systémů. Další informace o škálování najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md). Zde najdete na více systémů jak toouse automatické škálování, což je počet instancí tooscale automaticky na základě předdefinovaných pravidel a plány.

Hello škálování nastavení proveďte pouze sekund tooapply a vliv na všechny aplikace ve vaší [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
Nevytvářejí vyžadují jste toochange kódu ani znovu nasaďte aplikaci.

Informace o cenách hello a funkce jednotlivých plány služby App Service najdete v tématu [podrobnosti o cenách na aplikaci služby](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Předtím, než přepnete plán služby App Service z hello **volné** úroveň, je nutné nejprve odebrat hello [limitů útraty](https://azure.microsoft.com/pricing/spending-limits/) nastavené pro vaše předplatné Azure. tooview nebo změňte možnosti pro vaše předplatné Microsoft Azure App Service najdete v části [předplatná Microsoft Azure][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Škálování cenové úrovně
1. V prohlížeči otevřete hello [portál Azure][portal].
2. V okně vaší aplikace, klikněte na tlačítko **všechna nastavení**a potom klikněte na **vertikálně navýšit kapacitu**.
   
    ![Přejděte tooscale si aplikaci Azure.][ChooseWHP]
3. Zvolte úroveň a pak klikněte na **vyberte**.
   
    Hello **oznámení** karta bude flash zelená **úspěch** po dokončení operace hello.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Škálování související informační zdroje
Pokud vaše aplikace závisí na jiné služby, například Azure SQL Database nebo úložiště Azure, můžete škálovat také tyto prostředky na základě potřeb. Tyto prostředky nejsou změněna pomocí hello plán služby App Service a musí být změněna samostatně.

1. V **Essentials**, klikněte na tlačítko hello **skupiny prostředků** odkaz.
   
    ![Škálování souvisejících prostředků Azure aplikace](./media/web-sites-scale/RGEssentialsLink.png)
2. V hello **Souhrn** součástí hello **skupiny prostředků** okno, klepněte na prostředků, které chcete tooscale. Hello následující snímek obrazovky ukazuje prostředků databáze SQL a prostředek služby Azure Storage.
   
    ![Přejděte tooresource skupiny okno tooscale si aplikaci Azure](./media/web-sites-scale/ResourceGroup.png)
3. Pro databáze SQL prostředek, klikněte na tlačítko **nastavení** > **cenová úroveň** tooscale hello cenová úroveň.
   
    ![Škálování hello SQL databáze back-end pro vaše aplikace Azure](./media/web-sites-scale/ScaleDatabase.png)
   
    Můžete také zapnout [geografická replikace](../sql-database/sql-database-geo-replication-overview.md) pro instanci databáze SQL.
   
    Prostředek Azure Storage, klikněte na tlačítko **nastavení** > **konfigurace** tooscale vaše možnosti úložiště.
   
    ![Škálování účtu úložiště Azure hello používá vaše aplikace Azure](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a>Další informace o funkcích vývojáře
V závislosti na hello cenovou úroveň hello následující funkce orientovaných na vývojáře jsou k dispozici:

### <a name="bitness"></a>Počet bitů
* Hello **základní**, **standardní**, a **Premium** vrstev podporovat 64bitové a 32bitové aplikace.
* Hello **volné** a **sdílené** plán vrstev podporují pouze 32bitové aplikace.

### <a name="debugger-support"></a>Podpora ladění
* Ladicí program podpora je k dispozici pro hello **volné**, **sdílené**, a **základní** režimy na jedno připojení na plán služby App Service.
* Ladicí program podpora je k dispozici pro hello **standardní** a **Premium** režimy v pěti souběžných připojení na plán služby App Service.

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a>Další informace o dalších funkcích
* Podrobné informace o všech hello zbývající funkcí v hello služby App Service plánů, včetně ceny a funkce, které vás zajímají tooall uživatelů (včetně vývojáři), najdete v části [podrobnosti o cenách na aplikaci služby](https://azure.microsoft.com/pricing/details/web-sites/).

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/) kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Je vyžadována žádná platební karta a bez jakýchkoli závazků.
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a>Další kroky
* tooget začít s Azure, najdete v části [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/).
* Informace o cenách, podpory a SLA najdete na adrese hello následující odkazy.
  
    [Podrobnosti o cenách přenos dat](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Plánům podpory Azure Microsoft](https://azure.microsoft.com/support/plans/)
  
    [smlouvám o úrovni služeb](https://azure.microsoft.com/support/legal/sla/)
  
    [Podrobnosti o cenách na SQL databázi](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Virtuální počítač a velikost cloudových služeb pro Microsoft Azure][vmsizes]
  
    [Podrobnosti o cenách služby App Service](https://azure.microsoft.com/pricing/details/app-service/)
  
    [Podrobnosti o - připojení SSL cenách služby App Service](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* Informace o službě Azure App Service osvědčených postupů, včetně vytváření škálovatelná a flexibilní architektuře, najdete v části [osvědčené postupy: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).
* Videa o škálování aplikací App Service naleznete v hello následující prostředky:
  
  * [Když tooScale weby Azure - s Stefan Schackow](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
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
