---
title: "aaaHow přesměrujete zařízení USB v Azure Remoteappu? | Dokumentace Microsoftu"
description: "Zjistěte, jak toouse přesměrování pro zařízení USB v Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 191d98af-2f5a-4307-9042-aae0e4049f9f
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 661b90c0910167d76ac3886b5af7a32d00b3943f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Jak je přesměrovat zařízení USB v Azure Remoteappu?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Přesměrování zařízení umožňuje uživatelům používat hello USB zařízení připojených tootheir počítače nebo tabletu hello aplikace v Azure Remoteappu. Například pokud jste sdíleli Skype pomocí Azure Remoteappu, vaši uživatelé musí mít toouse toobe kamery jejich zařízení.

Než přejdete na další, ujistěte se přečíst informace o přesměrování USB hello v [pomocí přesměrování v Azure Remoteappu](remoteapp-redirection.md). Však hello doporučené nusbdevicestoredirect:s: * nebude fungovat pro webové kamery USB a nemusí fungovat pro některé tiskárny USB nebo multifunkčních zařízení USB. Návrh a z bezpečnostních důvodů hello Azure RemoteApp má správce tooenable přesměrování identifikátor GUID třídy zařízení nebo ID instance zařízení vaši uživatelé mohli používat tato zařízení.

I když v tomto článku bude zmíněn webové kamery přesměrování, můžete použít podobné tiskárny USB tooredirect přístup a ostatní multifunkčních zařízení USB, které nejsou přesměrován podle hello **nusbdevicestoredirect:s:*** příkaz.

## <a name="redirection-options-for-usb-devices"></a>Možnosti přesměrování pro zařízení USB
Azure RemoteApp používá velmi podobné mechanismy pro přesměrování zařízení USB, jako hello těch, které jsou k dispozici pro služby Vzdálená plocha. Hello základní technologii vám umožní vybrat metodu hello správné přesměrování pro dané zařízení, tooget hello nejlepší z obou vysoké úrovně a přesměrování zařízení RemoteFX USB pomocí hello **usbdevicestoredirect:s:** příkaz. Existují čtyři elementy toothis příkaz:

| Pořadí zpracování | Parametr | Popis |
| --- | --- | --- |
| 1 |* |Vybere všechna zařízení, která nejsou zachyceny vysoké úrovně přesměrování. Poznámka: standardně * nefunguje pro webové kamery USB. |
| {Třída zařízení GUID} |Vybere všechna zařízení, které odpovídají zadané hello třída zařízení. | |
| USB\InstanceID |Vybere zařízení USB zadané pro hello zadané ID instance. | |
| 2 |-USB\Instance ID |Odebere hello nastavení přesměrování pro zadané zařízení hello. |

## <a name="redirecting-a-usb-device-by-using-hello-device-class-guid"></a>Přesměrování USB zařízení s použitím třídy zařízení hello GUID
Existují dva způsoby třídu zařízení hello toofind identifikátor GUID, který lze použít pro přesměrování. 

první možnost Hello je toouse hello [systémem definovaná zařízení instalace třídy dostupné tooVendors](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Vyberte hello třída, která nejvíce odpovídá implementované hello zařízení připojených toohello místního počítače. Pro digitální fotoaparáty může se jednat Imaging zařízení nebo zařízení zaznamenat Video třída. Toodo budete potřebovat některé experimenty s hello zařízení třídy toofind hello správný, aby byl identifikátor GUID, který funguje s místně hello třídy připojen zařízení USB (v našem případu hello webové kamery).

Lepší způsob nebo hello druhé možnosti je toofollow určité zařízení tyto kroky toofind hello třídy identifikátor GUID:

1. Otevřete hello Správce zařízení, vyhledejte hello zařízení, která bude přesměrován a pravým tlačítkem myši a pak otevřete vlastnosti hello.
   ![Otevřete hello Správce zařízení](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Na hello **podrobnosti** , zvolte vlastnost hello **identifikátor Guid třídy**. Hello hodnotu, která se zobrazí je hello identifikátor GUID třídy pro tento typ zařízení.
   ![Vlastnosti fotoaparát](./media/remoteapp-usbredir/ra-classguid.png)
3. Použijte hello identifikátor Guid třídy hodnotu tooredirect zařízení, která ho.

Například:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Můžete kombinovat více přesměrování zařízení v hello stejné rutiny. Příklad: tooredirect místní úložiště a USB webové kamery, rutina vypadá takto:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Pokud nastavíte přesměrování zařízení podle třídy GUID zprávy jsou přesměrovány všechna zařízení, které odpovídají, že třída identifikátor GUID v hello zadané kolekce. Například, pokud existují v místní síti hello více počítačů, které mají hello stejné webové kamery USB, můžete spustit tooredirect jedné rutiny všechny hello web kamery.

## <a name="redirecting-a-usb-device-by-using-hello-device-instance-id"></a>Přesměrování USB zařízení pomocí ID instance zařízení hello
Pokud chcete jemněji odstupňované řízení a chcete toocontrol přesměrování pro zařízení, můžete použít hello **USB\InstanceID** parametr přesměrování.

Hello nejtěžší součástí tato metoda je hledání ID hello USB zařízení instance. Počítač toohello a hello určitým zařízením USB, budete potřebovat přístup. Potom postupujte podle těchto kroků:

1. Povolit přesměrování zařízení hello v relaci vzdálené plochy, jak je popsáno v [jak můžete I používat vlastní zařízení a prostředků v relaci vzdálené plochy?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Otevřete připojení ke vzdálené ploše a klikněte na tlačítko **zobrazit možnosti**.
3. Klikněte na tlačítko **uložit jako** toosave hello aktuální připojení nastavení tooan soubor RDP.  
    ![Uložit nastavení hello jako souboru RDP](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Zvolte název souboru a umístění, například MyConnection.rdp a tento PC\Documents a uložte soubor hello.
5. Otevřete hello MyConnection.rdp souboru pomocí textového editoru a najděte hello instance ID hello zařízení chcete tooredirect.

Teď můžete použijte hello instance ID v hello následující rutiny:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že v přidání toorating v tomto článku a přidání komentářů pod, článkem můžete provést samotný článek toohello změny? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek a klikněte na tlačítko **upravit na Githubu** toomake změny – ty, převzato toous ke kontrole, a potom po na nich, uvidíte změny a vylepšení přímo tady.

