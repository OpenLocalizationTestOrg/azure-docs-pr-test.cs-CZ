---
title: "Pomocí přesměrování v Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak konfigurovat a používat přesměrování v Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 2c8c867f-4907-4f2e-9ccd-2eb82bb5b837
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: b5a65d129225fde46e3b090bc3cd9427989005ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a>Pomocí přesměrování v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Přesměrování zařízení umožňuje uživatelům interakci s vzdálené aplikace pomocí zařízení připojených k jejich místním počítači, telefon nebo tablet. Například pokud jste zadali Skype pomocí Azure Remoteappu, uživatel musí fotoaparát nainstalovaný na počítači pro práci s Skype. To platí také pro tiskárny, řečníci, monitorování a periferních zařízení připojená k rozsahu portu USB.

Vzdálené aplikace RemoteApp využívá protokol RDP (Remote Desktop) a RemoteFX zadejte přesměrování.

## <a name="what-redirection-is-enabled-by-default"></a>Ve výchozím nastavení je povoleno jaké přesměrování?
Když používáte vzdálené aplikace RemoteApp, jsou ve výchozím nastavení povolené následující přesměrování. Informace v závorkách zobrazit nastavení protokolu RDP.

* Přehrávání zvuků v místním počítači (**hrát na tomto počítači**). (audiomode:i:0)
* Zaznamenání zvuku z místního počítače a odeslat do vzdáleného počítače (**záznamů z tohoto počítače**). (audiocapturemode:i:1)
* Používat místní tiskárny (redirectprinters:i:1)
* Porty COM (redirectcomports:i:1)
* Zařízení čipovou kartou (redirectsmartcards:i:1)
* Schránka (možnost zkopírujte a vložte) (redirectclipboard:i:1)
* Vymazat typ písma vyhlazování (povolit vyhlazení písma: i:1)
* Přesměrování všechna podporovaná zařízení Plug and Play. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Jaké další přesměrování je k dispozici?
Ve výchozím nastavení jsou zakázány dvě možnosti přesměrování:

* Přesměrování jednotky (mapování jednotky): stát mapované jednotky v této relaci vzdálené jednotky místního počítače. Díky tomu můžete uložit nebo otevřít soubory z místní jednotky během práce ve vzdálené relaci.
* Přesměrování USB: můžete použít zařízení USB připojených do místního počítače v této relaci vzdálené.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Změňte nastavení přesměrování v Remoteappu
Nastavení přesměrování zařízení pro kolekci můžete změnit pomocí Microsoft Azure PowerShell SDK. Po instalaci nové prostředí PowerShell a sady SDK se ho napřed nakonfigurovat ke správě svého předplatného, jak je popsáno v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Nastavit vlastní vlastnosti protokolu RDP pak použijte příkaz podobný následujícímu:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Všimněte si, že  *`n*  se používá jako oddělovač mezi jednotlivé vlastnosti.)

Chcete-li získat seznam, které jsou nakonfigurované jaké vlastní vlastnosti protokolu RDP, spusťte následující rutinu. Všimněte si, jestli se zobrazují pouze vlastní vlastnosti jako výstup výsledků a ne výchozí vlastnosti:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Pokud nastavíte vlastní vlastnosti je třeba zadat všechny vlastní vlastnosti pokaždé, když; v opačném případě vrátí nastavení na zakázáno.   

### <a name="common-examples"></a>Běžné příklady
Chcete-li povolit přesměrování jednotky použijte následující rutinu:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

Tuto rutinu použijte k povolení USB a jednotku přesměrování:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Chcete-li zakázat sdílení schránky, použijte tuto rutinu:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> Nezapomeňte úplně Odhlásit všechny uživatele v kolekci (a ne jenom odpojte je) před testovacího změnu. Aby se uživatelé jsou zcela odhlášením, přejděte na **relací** v kolekci v portálu Azure a odhlásit všechny uživatele, kterým se odpojí nebo přihlášení. Někdy může trvat několik sekund místní jednotky, které se zobrazí v Průzkumníku v rámci relace.
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Změňte nastavení přesměrování USB na vašeho klienta Windows
Pokud chcete používat přesměrování USB v počítači, který se připojuje k vzdálené aplikace RemoteApp, existují 2 akce, které je třeba provést. 1 - Správce musí povolit přesměrování USB na úrovni kolekce pomocí prostředí Azure PowerShell. 2 – u jednotlivých zařízení, ve které chcete používat přesměrování USB budete muset povolit zásady skupiny, které povolí ho. Tento krok bude muset být provedeno pro každý uživatel, který chce používat přesměrování USB.

> [!NOTE]
> Přesměrování USB s Azure Remoteappem je podporována pouze pro počítače se systémem Windows.
> 
> 

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>Povolit přesměrování USB pro kolekci vzdálené aplikace RemoteApp
Chcete povolit přesměrování USB na úrovni kolekce použijte následující rutinu:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Povolit přesměrování USB pro klientský počítač
Konfigurace nastavení přesměrování USB v počítači:

1. Otevřete Editor zásad skupiny místní (GPEDIT. MSC). (Spuštění gpedit.msc z příkazového řádku).
2. Otevřete **počítače složce Konfigurace počítače\Zásady\Šablony pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel připojení k ploše Client\RemoteFX USB zařízení přesměrování**.
3. Klikněte dvakrát na **povolit RDP přesměrování jiné podporovaná zařízení RemoteFX USB z tohoto počítače**.
4. Vyberte **povoleno**a potom vyberte **správců a uživatelů v přístupová práva přesměrování USB RemoteFX**.
5. Otevřete příkazový řádek s oprávněním správce a spusťte následující příkaz:
   
        gpupdate /force
6. Restartujte počítač.

Můžete taky nástroje pro správu zásad skupiny k vytvoření a použití zásad přesměrování USB pro všechny počítače ve vaší doméně:

1. Přihlaste se k řadiči domény jako správce domény.
2. Otevřete konzolu pro správu zásad skupiny. (Klikněte na tlačítko **Start > Nástroje pro správu > Správa zásad skupiny**.)
3. Přejděte na doménu nebo organizační jednotku, pro který chcete vytvořit zásadu.
4. Klikněte pravým tlačítkem na **výchozí zásady domény**a potom klikněte na **upravit**.
5. Otevřete **počítače složce Konfigurace počítače\Zásady\Šablony pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel připojení k ploše Client\RemoteFX USB zařízení přesměrování**.
6. Klikněte dvakrát na **povolit RDP přesměrování jiné podporovaná zařízení RemoteFX USB z tohoto počítače**.
7. Vyberte **povoleno**a potom vyberte **správců a uživatelů v přístupová práva přesměrování USB RemoteFX**.
8. Klikněte na **OK**.  

