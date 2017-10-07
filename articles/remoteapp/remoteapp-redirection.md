---
title: "přesměrování aaaUsing v Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure a používání přesměrování v Remoteappu"
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
ms.openlocfilehash: d5739a75cf606bd971268da67b2c5ff0fe5fe19b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a>Pomocí přesměrování v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Přesměrování zařízení umožňuje uživatelům interakci s vzdálené aplikace pomocí hello zařízení připojených tootheir místního počítače, telefon nebo tablet. Například pokud jste zadali Skype pomocí Azure Remoteappu, uživatel musí hello fotoaparát nainstalovaný na jejich počítač toowork s Skype. To platí také pro tiskárny, řečníci, monitorování a periferních zařízení připojená k rozsahu portu USB.

Vzdálené aplikace RemoteApp využívá hello protokolu RDP (Remote Desktop) a RemoteFX tooprovide přesměrování.

## <a name="what-redirection-is-enabled-by-default"></a>Ve výchozím nastavení je povoleno jaké přesměrování?
Když používáte vzdálené aplikace RemoteApp, jsou ve výchozím nastavení povolené hello následující přesměrování. nastavení protokolu RDP hello zobrazovat informace Hello v závorkách.

* Přehrávání zvuků na místním počítači hello (**hrát na tomto počítači**). (audiomode:i:0)
* Zaznamenat zvuk z místního počítače hello a odeslání toohello vzdáleného počítače (**záznamů z tohoto počítače**). (audiocapturemode:i:1)
* Tisk toolocal tiskárny (redirectprinters:i:1)
* Porty COM (redirectcomports:i:1)
* Zařízení čipovou kartou (redirectsmartcards:i:1)
* Schránka (možnost toocopy a vložení) (redirectclipboard:i:1)
* Vymazat typ písma vyhlazování (povolit vyhlazení písma: i:1)
* Přesměrování všechna podporovaná zařízení Plug and Play. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Jaké další přesměrování je k dispozici?
Ve výchozím nastavení jsou zakázány dvě možnosti přesměrování:

* Přesměrování jednotky (mapování jednotky): jednotky místního počítače stane mapované jednotky ve vzdálené relaci hello. Díky tomu můžete uložit nebo otevřít soubory z místní jednotky během práce ve vzdálené relaci hello.
* Přesměrování USB: hello USB zařízení připojených tooyour místního počítače můžete použít v relaci vzdálené hello.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Změňte nastavení přesměrování v Remoteappu
Nastavení přesměrování hello zařízení pro kolekci můžete změnit pomocí hello Microsoft Azure PowerShell SDK. Po instalaci hello nové prostředí PowerShell a sady SDK, ho napřed nakonfigurovat předplatné jako popsané v toomanage [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Pak použijte příkaz podobné toohello následující tooset hello vlastní RDP vlastnosti:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Všimněte si, že * `n * se používá jako oddělovač mezi jednotlivé vlastnosti.)

tooget seznam nakonfigurovaných jaké vlastní vlastnosti protokolu RDP, spusťte následující rutinu hello. Upozorňujeme, že pouze vlastní vlastnosti jsou uvedeny jako výstup výsledky a není hello výchozí vlastnosti:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Pokud nastavíte vlastní vlastnosti je třeba zadat všechny vlastní vlastnosti pokaždé, když; v opačném případě hello nastavení vrací toodisabled.   

### <a name="common-examples"></a>Běžné příklady
Použijte následující rutinu tooenable jednotky přesměrování hello:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

Použijte tuto rutinu tooenable přesměrování USB a jednotky:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Použijte tuto rutinu toodisable schránky sdílení:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> Být jisti toocompletely odhlášení všechny uživatele v kolekci hello (a ne jenom odpojte je) před testovacího hello změnu. tooensure uživatelé úplně odhlášeni, přejděte toohello **relací** v kolekci hello v hello portál Azure a odhlásit všechny uživatele, kterým se odpojí nebo přihlášení. Někdy může trvat několik sekund, tooshow hello místní jednotky v Průzkumníku v rámci relace hello.
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Změňte nastavení přesměrování USB na vašeho klienta Windows
Pokud chcete toouse přesměrování USB v počítači, který se připojuje tooRemoteApp, existují 2 akce, které je třeba toohappen. 1 - Správce musí tooenable přesměrování USB na úrovni kolekce hello pomocí prostředí Azure PowerShell. 2 – u jednotlivých zařízení, kam chcete toouse přesměrování USB je nutné tooenable zásad skupiny, která umožňuje ho. Tento krok bude nutné provést pro každý uživatel, který chce přesměrování USB toouse toobe.

> [!NOTE]
> Přesměrování USB s Azure Remoteappem je podporována pouze pro počítače se systémem Windows.
> 
> 

### <a name="enable-usb-redirection-for-hello-remoteapp-collection"></a>Povolit přesměrování USB pro hello kolekce vzdálené aplikace RemoteApp
Použijte následující rutinu tooenable USB přesměrování na úrovni kolekce hello hello:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-hello-client-computer"></a>Povolit přesměrování USB hello klientských počítačů
nastavení přesměrování USB tooconfigure ve vašem počítači:

1. Otevřete hello místní Editor zásad skupiny (GPEDIT. MSC). (Spuštění gpedit.msc z příkazového řádku).
2. Otevřete **počítače složce Konfigurace počítače\Zásady\Šablony pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel připojení k ploše Client\RemoteFX USB zařízení přesměrování**.
3. Klikněte dvakrát na **povolit RDP přesměrování jiné podporovaná zařízení RemoteFX USB z tohoto počítače**.
4. Vyberte **povoleno**a potom vyberte **správců a uživatelů v hello RemoteFX USB přesměrování přístupová práva**.
5. Otevřete příkazový řádek s oprávněním správce a spusťte následující příkaz hello:
   
        gpupdate /force
6. Restartujte počítač hello.

Můžete také použít toocreate nástroj pro správu zásad skupiny hello a použití zásad přesměrování hello USB pro všechny počítače ve vaší doméně:

1. Přihlaste se k řadiči domény hello jako správce domény hello.
2. Otevřete hello konzoly pro správu zásad skupiny. (Klikněte na tlačítko **Start > Nástroje pro správu > Správa zásad skupiny**.)
3. Přejděte toohello doménu nebo organizační jednotku, pro které chcete toocreate hello zásad.
4. Klikněte pravým tlačítkem na **výchozí zásady domény**a potom klikněte na **upravit**.
5. Otevřete **počítače složce Konfigurace počítače\Zásady\Šablony pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel připojení k ploše Client\RemoteFX USB zařízení přesměrování**.
6. Klikněte dvakrát na **povolit RDP přesměrování jiné podporovaná zařízení RemoteFX USB z tohoto počítače**.
7. Vyberte **povoleno**a potom vyberte **správců a uživatelů v hello RemoteFX USB přesměrování přístupová práva**.
8. Klikněte na **OK**.  

