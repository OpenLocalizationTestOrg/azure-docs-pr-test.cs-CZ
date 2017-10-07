---
title: "aaaStage nasazení cloudové služby (Node.js) | Microsoft Docs"
description: "Zjistěte, jak toodeploy vaše aplikace Azure tooa pracovní prostředí, pak nasadíte tooa produkčním prostředí pomocí prohození virtuální IP (VIP)."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d65d26a6-b424-49cd-a88c-7ef46bb112a8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 0656c84ac444a10f152f441265f85141347bf1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="staging-an-application-in-azure"></a>Pracovní aplikace v Azure
Zabalené aplikace může být nasazený toohello pracovní prostředí v Azure toobe testována před přesunutím toohello produkční prostředí, ve které hello je přístupná v hello Internetové aplikace. Je pracovní prostředí je úplně stejně jako hello provozním prostředí, ale můžete jen přístup hello dvoufázové instalace aplikace s zkomolené adresu URL, který je generovaný Azure. Po ověření, že aplikace funguje správně, může být nasazený toohello provozním prostředí provedením prohození virtuální IP (VIP).

> [!NOTE]
> Hello kroky v tomto článku se projeví pouze toonode aplikace hostované jako cloudová služba Azure.
> 
> 

## <a name="step-1-stage-an-application"></a>Krok 1: Příprava aplikace
Tato úloha popisuje, jak toostage aplikace pomocí hello **Microsoft Azure PowerShell**.

1. Při publikování služby, stačí předat hello **-slotu** parametru hello **Publish-AzureServiceProject** rutiny.
   
   ```powershell
   Publish-AzureServiceProject -Slot staging
   ```
2. Přihlaste se toohello [portál Azure classic] a vyberte **cloudové služby**. Po hello cloudové služby se vytvoří a hello **pracovní** stav sloupce byl aktualizován příliš**systémem**, klikněte na název služby hello.
   
   ![portál zobrazení spuštěnou službu][cloud-service]
3. Vyberte hello **řídicí panel**a potom vyberte **pracovní**.
   
   ![řídicí panel cloud service][cloud-service-dashboard]
4. Poznamenejte si hodnotu hello v hello **adresa URL webu** položka toohello vpravo. název DNS Hello je zkomolené interní ID, které generuje Azure.
   
    ![Adresa url webu][cloud-service-staging-url]

Nyní můžete ověřit, zda aplikace hello správně funguje v hello pracovní prostředí pomocí hello pracovní adresa URL webu.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Krok 2: Upgrade aplikace v produkčním prostředí pomocí Prohazuje virtuální IP adresy
Po ověření hello upgradovanou verzi aplikace v testovacím prostředí, můžete rychle zpřístupnit jej v produkčním prostředí pomocí odkládací hello virtuálních IP adres (VIP) hello pracovní a provozní prostředí.

> [!NOTE]
> Tento krok předpokládá, že jste již nasazení tooproduction aplikaci a připraví hello upgradovanou verzi aplikace.
> 
> 

1. Přihlaste se k hello [portál Azure classic], klikněte na tlačítko **cloudové služby** a pak vyberte název služby hello.
2. Z hello **řídicí panel**, vyberte **pracovní**a potom klikněte na **odkládacího souboru** v hello dolní části stránky hello. Otevře se dialogové okno prohození hello.
   
   ![Dialogové okno prohození virtuální IP adresy][vip-swap-dialog]
3. Zkontrolujte hello informace a pak klikněte na tlačítko **OK**. dvě nasazení Hello začít aktualizace jako pracovní nasazení přepínače tooproduction a hello produkční nasazení přepínače toostaging hello.

Úspěšně jste připravený nasazení a upgradovat produkční nasazení přes vzájemná záměna virtuální IP adresy s nasazováním hello v pracovní.

## <a name="additional-resources"></a>Další zdroje
* [Jak tooDeploy Upgrade služby tooProduction podle vzájemná záměna virtuální IP adresy v Azure]

[portál Azure classic]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Jak tooDeploy Upgrade služby tooProduction podle vzájemná záměna virtuální IP adresy v Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
