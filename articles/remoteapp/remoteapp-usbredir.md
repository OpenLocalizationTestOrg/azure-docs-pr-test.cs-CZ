---
title: "Jak je přesměrovat zařízení USB v Azure Remoteappu? | Dokumentace Microsoftu"
description: "Naučte se používat přesměrování pro zařízení USB v Azure Remoteappu."
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
ms.openlocfilehash: 3d7165d2c3dafe87b829e588b9e7f2c377552a35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Jak je přesměrovat zařízení USB v Azure Remoteappu?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Přesměrování zařízení umožňuje uživatelům používat zařízení USB připojená k jejich počítače nebo tabletu s aplikacemi v Azure Remoteappu. Například pokud jste sdíleli Skype pomocí Azure Remoteappu, uživatelé potřebovat moct používejte kamery jejich zařízení.

Než přejdete na další, ujistěte se přečíst informace o přesměrování USB v [pomocí přesměrování v Azure Remoteappu](remoteapp-redirection.md). Ale doporučená nusbdevicestoredirect:s: * nebude fungovat pro webové kamery USB a nemusí fungovat pro některé tiskárny USB nebo multifunkčních zařízení USB. Návrh a z bezpečnostních důvodů Azure RemoteApp správce musí povolit přesměrování identifikátor GUID třídy zařízení nebo ID instance zařízení vaši uživatelé mohli používat tato zařízení.

I když v tomto článku bude zmíněn webové kamery přesměrování, přesměrování tiskárny USB a jiná zařízení multifunkčních USB, které nejsou přesměrován podle můžete podobný postup **nusbdevicestoredirect:s:*** příkaz.

## <a name="redirection-options-for-usb-devices"></a>Možnosti přesměrování pro zařízení USB
Azure RemoteApp používá velmi podobné mechanismy pro přesměrování zařízení USB jako těm, které jsou k dispozici pro vzdálenou plochu. Základní technologii umožňuje vybrat metodu správné přesměrování pro dané zařízení, chcete-li získat nejlepší z obou vysoké úrovně a zařízení RemoteFX USB pomocí přesměrování **usbdevicestoredirect:s:** příkaz. Existují čtyři elementy pro tento příkaz:

| Pořadí zpracování | Parametr | Popis |
| --- | --- | --- |
| 1 |* |Vybere všechna zařízení, která nejsou zachyceny vysoké úrovně přesměrování. Poznámka: standardně * nefunguje pro webové kamery USB. |
| {Třída zařízení GUID} |Vybere všechna zařízení, které odpovídají třída zadané zařízení. | |
| USB\InstanceID |Vybere zařízení USB, zadaný pro danou instanci ID. | |
| 2 |-USB\Instance ID |Odebere nastavení přesměrování pro zadané zařízení. |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Přesměrování USB zařízení s použitím třídy zařízení GUID
Existují dva způsoby jak najít identifikátor GUID, který lze použít pro přesměrování třídy zařízení. 

První možností je používat [systémem definovaná zařízení instalace třídy k dispozici pro dodavatele](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Vyberte třídu, která nejvíce odpovídá implementované zařízení připojené k místnímu počítači. Pro digitální fotoaparáty může se jednat Imaging zařízení nebo zařízení zaznamenat Video třída. Budete muset provést některé experimenty s třídami zařízení najít třídu správný identifikátor GUID, který pracuje s místně připojené USB zařízení (v našem případě webové kamery).

Lepší způsob nebo druhou možnost, je pomocí těchto kroků najít identifikátor GUID třídy určité zařízení:

1. Otevřete Správce zařízení, vyhledejte zařízení, které bude přesměrován a pravým tlačítkem myši a pak otevřete vlastnosti.
   ![Otevřete Správce zařízení](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Na **podrobnosti** , zvolte vlastnost **Guid – třída**. Hodnota, která se zobrazí je identifikátor GUID třídy pro tento typ zařízení.
   ![Vlastnosti fotoaparát](./media/remoteapp-usbredir/ra-classguid.png)
3. Hodnota Guid třída použijete k přesměrování zařízení, která ho.

Například:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Můžete kombinovat více přesměrování zařízení ve stejné rutiny. Příklad: přesměrovat místní úložiště a webové kamery USB, rutina vypadá takto:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Pokud nastavíte přesměrování zařízení podle třídy GUID zprávy jsou přesměrovány všechna zařízení, které odpovídají třídy identifikátor GUID v zadané kolekci. Například pokud existují více počítačů v místní síti, které mají stejné webové kamery USB, můžete spustit jedné rutiny pro všechny webové kamery přesměrování.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Přesměrování USB zařízení pomocí instance ID zařízení
Pokud chcete další jemně odstupňovanou kontrolu a chcete řídit přesměrování pro zařízení, můžete použít **USB\InstanceID** parametr přesměrování.

Nejtěžší součástí tato metoda je hledání ID USB zařízení instance. Budete potřebovat přístup k počítači a určitým zařízením USB. Potom postupujte podle těchto kroků:

1. Povolit přesměrování zařízení během relace vzdálené plochy, jak je popsáno v [jak můžete I používat vlastní zařízení a prostředků v relaci vzdálené plochy?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Otevřete připojení ke vzdálené ploše a klikněte na tlačítko **zobrazit možnosti**.
3. Klikněte na tlačítko **uložit jako** uložit aktuální nastavení připojení do souboru protokolu RDP.  
    ![Uložte nastavení do souboru protokolu RDP](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Zvolte název souboru a umístění, například MyConnection.rdp a tento PC\Documents a uložte soubor.
5. Otevřete soubor MyConnection.rdp pomocí textového editoru a najděte ID instance zařízení, které chcete přesměrovat.

Teď můžete použijte instance ID v následující rutiny:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že kromě hodnocení tohoto článku a přidání komentářů pod článkem také můžete měnit samotný článek? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek článku, klikněte na **Upravit na GitHubu** a proveďte změny – změny se nám odešlou ke kontrole a po jejich schválení je doplníme do článku.

