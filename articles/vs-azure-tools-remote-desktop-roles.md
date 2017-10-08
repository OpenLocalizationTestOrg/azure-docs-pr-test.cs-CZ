---
title: "aaaUsing vzdálené plochy s rolemi Azure | Microsoft Docs"
description: "Pomocí Azure role vzdálené plochy"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d35fd421cde8be9e3caa474db95974a54e528bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a>Pomocí Azure role vzdálené plochy
Pomocí hello Azure SDK a Vzdálená plocha mají přístup k Azure role a virtuálních počítačů, které jsou hostované v Azure. V sadě Visual Studio můžete nakonfigurovat vzdálené plochy z projektu Azure cloud service. tooenable služby Vzdálená plocha, je nutné vytvořit pracovní projekt, který obsahuje jednu nebo více rolí a pak ho publikujete tooAzure.

> [!IMPORTANT]
> By měl přístup Azure role pro řešení problémů nebo pouze vývoj. Hello účel každého virtuálního počítače je toorun určité role v aplikaci Azure, ne toorun jiné klientské aplikace. Pokud chcete toouse Azure toohost virtuální počítač, který můžete použít pro jakýkoli účel, najdete v části přístup virtuálních počítačů Azure z Průzkumníka serveru.
> 
> 

## <a name="tooenable-and-use-remote-desktop-for-an-azure-role"></a>tooenable a používání vzdálené plochy pro Role v Azure
1. V Průzkumníku řešení otevřete hello místní nabídku pro projekt cloudové služby a potom zvolte **publikovat**.
   
    Hello **publikování aplikaci Azure** zobrazí se průvodce.
   
    ![Publikování příkaz pro projekt cloudové služby](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. Na konci hello **nastavením publikování v Microsoft Azure** hello průvodce vyberte hello **povolení vzdálené plochy** pro všechny role zaškrtávací políčko. 
   
    Hello **konfigurace vzdálené plochy** zobrazí se dialogové okno.
3. Na konci hello hello **konfigurace vzdálené plochy** dialogovém okně vyberte hello **další možnosti** tlačítko. 
   
    Zobrazí rozevírací seznam, který umožňuje vytvořit nebo vybrat si certifikát tak, aby při připojování přes vzdálenou plochu můžete šifrovat přihlašovací údaje informace.
4. V rozevíracím seznamu hello, zvolte  **&lt;vytvořit >**, nebo vyberte existující šablonu ze seznamu hello. 
   
    Pokud zvolíte existující certifikát, přeskočte hello následující kroky.
   
   > [!NOTE]
   > Hello certifikáty, které potřebujete pro připojení ke vzdálené ploše se liší od hello certifikáty, které používáte pro jiné operace v Azure. certifikát Hello vzdáleného přístupu musí mít privátní klíč.
   > 
   > 
   
    Hello **vytvořit certifikát** zobrazí se dialogové okno.
   
   1. Zadejte popisný název pro nový certifikát hello a potom vyberte hello **OK** tlačítko. v rozevíracím seznamu hello se zobrazí nový certifikát Hello.
   2. V hello **konfigurace vzdálené plochy** dialogovém okně zadejte uživatelské jméno a heslo.
      
       Nelze použít existující účet. Nezadávejte správce jako hello uživatelské jméno pro nový účet hello.
      
      > [!NOTE]
      > Pokud hello heslo nesplňuje požadavky na složitost hello, červenou ikonu se zobrazí další toohello heslo textové pole. Hello heslo musí obsahovat velká písmena, malá písmena a čísla nebo symboly.
      > 
      > 
   3. Zvolte datum, na který účet hello vyprší a po které připojení ke vzdálené ploše se zablokuje.
   4. Poté, co jste zadaný všechny hello požadované informace, vyberte hello **OK** tlačítko.
      
       Několik nastavení, které umožňují přístup k vzdálené se přidají toohello soubory .cscfg a .csdef.
5. V hello **nastavením publikování v Microsoft Azure** průvodce, zvolte hello **OK** tlačítko až budete připraveni toopublish cloudové služby.
   
    Pokud si nejste připravené toopublish, zvolte hello **zrušit** tlačítko. nastavení konfigurace Hello se ukládají a cloudové služby můžete publikovat později.

## <a name="connect-tooan-azure-role-by-using-remote-desktop"></a>Připojit tooan Role v Azure pomocí vzdálené plochy
Po publikování cloudové služby na platformě Azure, můžete použít Průzkumníka serveru toolog do virtuálních počítačů hello který je hostitelem Azure. 

1. V Průzkumníku serveru rozbalte hello **Azure** uzel a potom rozbalte uzel hello pro cloudové služby a jeden z jeho role toodisplay seznam instancí.
2. Otevřete hello místní nabídky pro uzel instanci a potom zvolte **připojit pomocí vzdálené plochy**.
   
    ![Připojování přes vzdálenou plochu](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. Zadejte hello uživatelské jméno a heslo, které jste předtím vytvořili. Nyní jste přihlášeni ke vzdálené relaci.

