---
title: "serverové farmy služby SharePoint aaaCreate v Azure | Microsoft Docs"
description: "Rychle vytvořte novou farmu služby SharePoint 2013 nebo SharePoint 2016 v Azure pomocí portálu Azure marketplace hello."
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
ms.openlocfilehash: d71c7177d9b99cf377bab767342508007285b452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-sharepoint-server-farms-using-hello-azure-portal-marketplace"></a>Vytvoření serverové farmy služby SharePoint pomocí hello portálu Azure marketplace

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a>Farmy služby SharePoint 2013
Hello Microsoft Azure Marketplace portálu můžete rychle vytvořit předem nakonfigurované farmy služby SharePoint Server 2013. To můžete ušetřit mnoho času když potřebujete základní nebo vysokou dostupností farmy služby SharePoint pro prostředí pro vývoj/testování nebo hodnocení SharePoint Server 2013 jako řešení spolupráce pro vaši organizaci.

> [!NOTE]
> Hello **serverové farmy služby SharePoint** byla odebrána položka v hello Azure Marketplace hello portálu Azure. Byla nahrazena hello **jiný - HA farmy serverů Sharepointu 2013** a **farmy služby SharePoint 2013 HA** položky.
>
>

Hello základní farmy služby SharePoint se skládá z tři virtuální počítače v této konfiguraci.

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

Tuto konfiguraci farmy můžete použít pro zjednodušené nastavení pro vývoj aplikací služby SharePoint nebo prvního hodnocení produktu SharePoint 2013.

toocreate hello základní (Tříserverová) farmy služby SharePoint:

1. Klikněte na tlačítko [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klikněte na tlačítko **nasazení**.
3. Na hello **jiný - HA farmy serverů Sharepointu 2013** podokně klikněte na tlačítko **vytvořit**.
4. Zadat nastavení na hello kroky hello **vytvořit jiný - HA farmy serverů Sharepointu 2013** podokně a pak klikněte na tlačítko **vytvořit**.

devět virtuálních počítačů v této konfiguraci se skládá farmy služby SharePoint Hello vysokou dostupnost.

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

Tato konfigurace farmy tootest vyšší klientů zatížení, vysokou dostupnost hello externí web služby SharePoint a skupin dostupnosti AlwaysOn systému SQL Server můžete použít pro farmu služby SharePoint. Tuto konfiguraci můžete použít také pro vývoj aplikací pro SharePoint v prostředí s vysokou dostupností.

farma služby SharePoint se toocreate hello vysokou dostupnost (devět servery):

1. Klikněte na tlačítko [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klikněte na tlačítko **nasazení**.
3. Na hello **farmy služby SharePoint 2013 HA** podokně klikněte na tlačítko **vytvořit**.
4. Zadat nastavení na hello sedm kroků hello **vytvořit farmu služby SharePoint 2013 HA** podokně a pak klikněte na tlačítko **vytvořit**.

> [!NOTE]
> Nelze vytvořit hello **jiný - HA farmy serverů Sharepointu 2013** nebo **farmy služby SharePoint 2013 HA** Azure bezplatnou zkušební verzi.
>
>

Hello portál Azure vytvoří obě tyto farmy v čistě cloudové virtuální síť s straně Internetu webová služba. Neexistuje žádné site-to-site VPN nebo ExpressRoute připojení back tooyour síti organizace.

> [!NOTE]
> Pokud vytvoříte hello základní nebo farmy služby SharePoint vysokou dostupností pomocí hello portálu Azure, nemůžete zadat existující skupinu prostředků. toowork obejít toto omezení, vytvořte tyto farmy s prostředím Azure PowerShell. Další informace najdete v tématu [vytvořit SharePoint 2013 pro vývoj/testování farmy s prostředím Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).
>
>

## <a name="sharepoint-2016-farms"></a>Farmy služby SharePoint 2016
V tématu [v tomto článku](https://technet.microsoft.com/library/mt723354.aspx) hello pokyny toobuild hello po jednom serveru SharePoint Server 2016 farmy.

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-hello-sharepoint-farms"></a>Správa farmy služby SharePoint hello
Můžete spravovat servery hello tyto farmy prostřednictvím připojení ke vzdálené ploše. Další informace najdete v tématu [protokolu na virtuálním počítači toohello](quick-create-portal.md#connect-to-virtual-machine).

Z lokality centrální správy SharePoint hello můžete nakonfigurovat svoje lokality aplikací služby SharePoint a další funkce. Další informace najdete v tématu [konfigurace služby SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Další kroky
* Zjišťovat další [konfigurace služby SharePoint](https://technet.microsoft.com/library/dn635309.aspx) ve službách infrastruktury Azure.
