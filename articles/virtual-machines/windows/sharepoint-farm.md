---
title: "Vytvoření serverové farmy služby SharePoint v Azure | Microsoft Docs"
description: "Rychle vytvořte novou farmu služby SharePoint 2013 nebo SharePoint 2016 v Azure pomocí portálu Azure marketplace."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 89b124da-019d-4179-86dd-ad418d05a4f2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 00648ee35962b22fb7eeceaa6d9df7305c801abf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-sharepoint-server-farms-using-the-azure-portal-marketplace"></a>Vytvoření serverové farmy služby SharePoint pomocí portálu Azure marketplace

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a>Farmy služby SharePoint 2013
Marketplace portálu Microsoft Azure můžete rychle vytvořit předem nakonfigurované farmy služby SharePoint Server 2013. To můžete ušetřit mnoho času když potřebujete základní nebo vysokou dostupností farmy služby SharePoint pro prostředí pro vývoj/testování nebo hodnocení SharePoint Server 2013 jako řešení spolupráce pro vaši organizaci.

> [!NOTE]
> **Serverové farmy služby SharePoint** byla odebrána položka v Azure Marketplace portálu Azure. Byla nahrazena **jiný - HA farmy serverů Sharepointu 2013** a **farmy služby SharePoint 2013 HA** položky.
>
>

Základní farmy služby SharePoint se skládá z tři virtuální počítače v této konfiguraci.

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

Tuto konfiguraci farmy můžete použít pro zjednodušené nastavení pro vývoj aplikací služby SharePoint nebo prvního hodnocení produktu SharePoint 2013.

Pokud chcete vytvořit základní farmy služby SharePoint (třemi servery):

1. Klikněte na tlačítko [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klikněte na tlačítko **nasazení**.
3. Na **jiný - HA farmy serverů Sharepointu 2013** podokně klikněte na tlačítko **vytvořit**.
4. Zadat nastavení na kroky **vytvořit jiný - HA farmy serverů Sharepointu 2013** podokně a pak klikněte na tlačítko **vytvořit**.

Vysoká dostupnost farmy služby SharePoint se skládá z devíti virtuální počítače v této konfiguraci.

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

Tuto konfiguraci farmy můžete použít k testování větší objemy klienta, vysokou dostupnost externí web služby SharePoint a skupin dostupnosti AlwaysOn SQL serveru pro farmu služby SharePoint. Tuto konfiguraci můžete použít také pro vývoj aplikací pro SharePoint v prostředí s vysokou dostupností.

Pokud chcete vytvořit farmu služby SharePoint vysokou dostupnost (devět server):

1. Klikněte na tlačítko [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klikněte na tlačítko **nasazení**.
3. Na **farmy služby SharePoint 2013 HA** podokně klikněte na tlačítko **vytvořit**.
4. Zadat nastavení na sedm kroky **vytvořit farmu služby SharePoint 2013 HA** podokně a pak klikněte na tlačítko **vytvořit**.

> [!NOTE]
> Nelze vytvořit **jiný - HA farmy serverů Sharepointu 2013** nebo **farmy služby SharePoint 2013 HA** Azure bezplatnou zkušební verzi.
>
>

Portál Azure vytvoří obě tyto farmy v čistě cloudové virtuální síť s straně Internetu webová služba. Neexistuje žádné připojení site-to-site VPN nebo ExpressRoute zpět k síti vaší organizace.

> [!NOTE]
> Když vytvoříte základní nebo farmy služby SharePoint vysokou dostupností pomocí portálu Azure, nemůžete zadat existující skupinu prostředků. Toto omezení obejít, vytvořte tyto farmy s prostředím Azure PowerShell. Další informace najdete v tématu [vytvořit SharePoint 2013 pro vývoj/testování farmy s prostředím Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).
>
>

## <a name="sharepoint-2016-farms"></a>Farmy služby SharePoint 2016
V tématu [v tomto článku](https://technet.microsoft.com/library/mt723354.aspx) pokyny k vytvoření následujících farmy služby SharePoint Server 2016 jedním serverem.

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>Správa farmy služby SharePoint
Můžete spravovat servery tyto farmy prostřednictvím připojení ke vzdálené ploše. Další informace najdete v tématu [Přihlaste se k virtuálnímu počítači](quick-create-portal.md#connect-to-virtual-machine).

Z lokality centrální správy SharePoint můžete nakonfigurovat svoje lokality aplikací služby SharePoint a další funkce. Další informace najdete v tématu [konfigurace služby SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Další kroky
* Zjišťovat další [konfigurace služby SharePoint](https://technet.microsoft.com/library/dn635309.aspx) ve službách infrastruktury Azure.
