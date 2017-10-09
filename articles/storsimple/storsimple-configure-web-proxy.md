---
title: "aaaSet až webového proxy serveru pro zařízení StorSimple | Microsoft Docs"
description: "Zjistěte, jak toouse Windows Powershellu pro StorSimple tooconfigure webové nastavení proxy serveru pro zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6c2ca351-a7c6-4da6-ab5e-c081e6d08261
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: c0213eb2a1902ff994147bb16593f0ff300eb70c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>Konfigurace webového proxy serveru pro zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz popisuje, jak toouse Windows Powershellu pro StorSimple tooconfigure a zobrazení webové nastavení proxy serveru pro zařízení StorSimple. nastavení webového proxy serveru Hello používají zařízení StorSimple hello při komunikaci s hello cloudu. Webový proxy server je použité tooadd další vrstvu zabezpečení, filtrování obsahu, mezipaměti tooease nároky na šířku pásma nebo dokonce pomůže s analytics.

Webový proxy server je volitelná konfigurace pro zařízení StorSimple. Můžete nakonfigurovat webový proxy server jenom prostřednictvím Windows Powershellu pro StorSimple. Konfigurace Hello je dvoustupňový proces takto:

1. Je nejprve nakonfigurovat nastavení webového proxy serveru pomocí Průvodce instalací hello nebo prostředí Windows PowerShell pro StorSimple rutiny.
2. Potom povolíte hello nakonfigurované nastavení webového proxy serveru pomocí prostředí Windows PowerShell pro StorSimple rutiny.

Po dokončení konfigurace webového proxy serveru hello můžete zobrazit hello nakonfigurované nastavení proxy serveru webové služby Microsoft Azure StorSimple Manager hello i hello prostředí Windows PowerShell pro StorSimple. 

Po přečtení tohoto kurzu, budete moci:

* Nakonfigurovat webový proxy server pomocí Průvodce instalací a rutin
* Povolit webový proxy server pomocí rutin
* Zobrazit nastavení webového proxy serveru v hello portál Azure classic
* Řešení chyb během konfigurace webového proxy serveru

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Nakonfigurovat webový proxy server pomocí prostředí Windows PowerShell pro StorSimple
Můžete použít některý z následujících nastavení webového proxy serveru tooconfigure hello:

* Instalační program tooguide Průvodce vás provede kroky konfigurace hello.
* Rutiny Windows Powershellu pro StorSimple.

Každá z těchto metod je popsané v následující části hello.

## <a name="configure-web-proxy-via-hello-setup-wizard"></a>Nakonfigurovat webový proxy server pomocí Průvodce instalací hello
Hello instalace Průvodce tooguide, který vás provede hello kroky můžete použít pro konfigurace webového proxy serveru. Proveďte následující kroky tooconfigure webový proxy server na vašem zařízení hello.

#### <a name="tooconfigure-web-proxy-via-hello-setup-wizard"></a>tooconfigure webový proxy server pomocí Průvodce instalací hello
1. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup** a zadejte hello **hesla správce zařízení**. Typ hello následující příkaz toostart relaci Průvodce instalací:
   
    `Invoke-HcsSetupWizard`
2. Pokud je to hello poprvé, co jste použili hello Průvodce instalací pro registraci zařízení, budete potřebovat tooconfigure všechna nastavení sítě hello požadované dokud se nedostanete hello konfigurace webového proxy serveru. Pokud vaše zařízení je již zaregistrován, může přijmout všechny hello nakonfigurovaná nastavení sítě, dokud se nedostanete hello konfigurace webového proxy serveru. V Průvodci instalací hello při výzvami tooconfigure webové nastavení proxy serveru, zadejte **Ano**.
3. Pro hello **adresa URL proxy serveru webové**, zadejte hello IP adresu nebo plně kvalifikovaný název domény (FQDN) vaší webový proxy server a hello číslo portu TCP, které chcete toouse vaše zařízení, při komunikaci s cloudem hello hello. Hello použijte následující formát:
   
    `http://<IP address or FQDN of hello web proxy server>:<TCP port number>`
   
    Ve výchozím nastavení je zadané číslo portu TCP 8080.
4. Vyberte typ ověřování hello jako **NTLM**, **základní**, nebo **žádné**. Základní je hello alespoň zabezpečeného ověřování pro hello konfiguraci proxy serveru. NT LAN Manager (NTLM) je vysoce zabezpečené a komplexní ověřovací protokol, který používá třícestné systému zasílání zpráv (někdy čtyři Pokud je potřeba další integrity) tooauthenticate uživatele. Hello výchozí ověřování je NTLM. Další informace najdete v tématu [základní](http://hc.apache.org/httpclient-3.x/authentication.html) a [ověřování protokolem NTLM](http://hc.apache.org/httpclient-3.x/authentication.html). 
   
   > [!IMPORTANT]
   > **V hello služby StorSimple Manager monitorování grafy hello zařízení nefungují, pokud základní nebo v konfiguraci proxy serveru pro zařízení hello hello je povolené ověřování NTLM. Pro hello monitorování toowork grafy, budete potřebovat tooensure, ověření je nastaven tooNONE.**
   > 
   > 
5. Pokud používáte ověřování, zadejte **uživatelské jméno Proxy webové** a **heslo pro proxy server webové**. Budete také potřebovat tooconfirm hello heslo.
   
    ![Nakonfigurovat webový proxy server na StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Pokud registrujete zařízení pro hello poprvé, pokračujte s registrací hello. Pokud vaše zařízení už je registrovaná, hello průvodce bude ukončen. Hello nakonfigurované nastavení se uloží.

Webový proxy server bude nyní být rovněž povolena. Můžete přeskočit hello [Povolit proxy server webové](#enable-web-proxy) krok a přejít přímo příliš[zobrazit nastavení webového proxy serveru v hello portál Azure classic](#view-web-proxy-settings-in-the-azure-classic-portal).

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Konfigurace webového proxy serveru pomocí Windows Powershellu pro StorSimple rutiny
Nastavení webového proxy serveru tooconfigure jiný způsob, jak je prostřednictvím hello Windows Powershellu pro StorSimple rutiny. Proveďte následující kroky tooconfigure webový proxy server hello.

#### <a name="tooconfigure-web-proxy-via-cmdlets"></a>tooconfigure webový proxy server pomocí rutiny
1. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**. Po zobrazení výzvy zadejte hello **hesla správce zařízení**. výchozí heslo Hello je `Password1`.
2. Hello příkazového řádku zadejte:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    Zadejte a potvrďte heslo hello po zobrazení výzvy, jak je uvedeno níže.
   
    ![Nakonfigurovat webový proxy server na StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

Hello webový proxy server je nyní nakonfigurována a je třeba toobe povolena.

## <a name="enable-web-proxy"></a>Povolit webový proxy server
Webový proxy server je ve výchozím nastavení zakázán. Po dokončení konfigurace nastavení proxy serveru webové hello zařízení StorSimple, potřebujete toouse hello prostředí Windows PowerShell pro nastavení StorSimple tooenable hello webového proxy serveru.

> [!NOTE]
> **Tento krok nebude nutné, pokud jste použili hello instalace Průvodce tooconfigure webový proxy server. Webový proxy server je automaticky povolené ve výchozím nastavení po relaci Průvodce instalací.**
> 
> 

Proveďte následující kroky v prostředí Windows PowerShell pro StorSimple tooenable webový proxy server na vašem zařízení hello:

#### <a name="tooenable-web-proxy"></a>tooenable webový proxy server
1. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**. Po zobrazení výzvy zadejte hello **hesla správce zařízení**. výchozí heslo Hello je `Password1`.
2. Hello příkazového řádku zadejte:
   
    `Enable-HcsWebProxy`
   
    Nyní jste povolili hello konfigurace webového proxy serveru v zařízení StorSimple.
   
    ![Nakonfigurovat webový proxy server na StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-hello-azure-classic-portal"></a>Zobrazit nastavení webového proxy serveru v hello portál Azure classic
nastavení webového proxy serveru Hello jsou nakonfigurovány pomocí rozhraní Windows PowerShell hello a nemůže být změněn z portálu classic hello. Tyto nakonfigurované nastavení můžete, ale zobrazit na portálu classic hello. Proveďte následující kroky tooview webový proxy server hello.

#### <a name="tooview-web-proxy-settings"></a>nastavení webového proxy serveru tooview
1. Přejděte příliš**služby StorSimple Manager > zařízení**. Vyberte a klikněte na zařízení a potom přejděte příliš**konfigurace**.
2. Posuňte se dolů na hello **konfigurace** stránky příliš**webové nastavení proxy serveru** části. Hello nakonfigurované nastavení webového proxy serveru můžete zobrazit v zařízení StorSimple, jak je uvedeno níže.
   
    ![Proxy webové zobrazení na portálu pro správu](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)

## <a name="errors-during-web-proxy-configuration"></a>Chyby během konfigurace webového proxy serveru
Pokud nastavení hello webového proxy serveru správně nakonfigurována, budou chybové zprávy zobrazené toohello uživatele v prostředí Windows PowerShell pro StorSimple. Hello následující tabulka popisuje některé z těchto chybových zpráv, jejich možných příčin a doporučené akce.

| Sériové č. | Chyba HRESULT kódu | Možné příčiny | Doporučená akce |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |Příkaz spuštěn z hello pasivní řadiče a není možné toocommunicate s řadič active hello. |Spusťte příkaz hello řadič active hello. příkaz hello toorun z hello pasivní řadiče, budete potřebovat toofix hello připojení z pasivní tooactive řadiče. Pokud toto připojení není funkční, budete potřebovat tooengage Microsoft Support. |
| 2. |0x800710dd - identifikátor hello operaci není platný |Nastavení proxy serveru nejsou podporovány na virtuální zařízení StorSimple. |Nastavení proxy serveru nejsou podporovány na virtuální zařízení StorSimple. To se dá nakonfigurovat jenom na fyzického zařízení StorSimple. |
| 3. |0x80070057 – neplatný parametr |Jeden z parametrů hello zadaných pro hello nastavení proxy serveru není platný. |Hello identifikátor URI není k dispozici ve správném formátu. Hello použijte následující formát:`http://<IP address or FQDN of hello web proxy server>:<TCP port number>` |
| 4. |0x800706ba - server RPC není k dispozici |Hello hlavní příčinou je jedna z následujících hello:</br></br>Cluster není nahoru.</br></br>DataPath služba není spuštěna.</br></br>příkaz Hello spustí z pasivní řadiče a není možné toocommunicate s řadič active hello. |Prosím provozovat tooensure Microsoft Support, který hello clusteru je zapnutý a je spuštěna služba datapath.</br></br>Spusťte příkaz hello z aktivního řadiče hello. Pokud chcete, aby příkaz hello toorun z hello pasivní řadiče, budete potřebovat tooensure hello pasivní řadič může komunikovat se službou active řadiče hello. Pokud toto připojení není funkční, budete potřebovat tooengage Microsoft Support. |
| 5. |0X800706be - RPC volání se nezdařilo. |Clusteru je mimo provoz. |Je prosím provozovat tooensure Microsoft Support, který hello cluster v provozu. |
| 6. |0x8007138f – prostředek clusteru nebyl nalezen |Prostředek clusteru služby platformy nebyla nalezena. To může nastat při instalaci hello nebyla správná. |Může být nutné tooperform obnovit v zařízení výrobní nastavení. Může být nutné toocreate prostředek platformy. Microsoft Support Požádejte prosím o dalších krocích. |
| 7. |0x8007138c – prostředek clusteru není online |Prostředky clusteru platformy nebo datapath nejsou online. |Kontaktujte Microsoft Support toohelp prosím zajistěte, aby byly prostředek hello datapath a platforma služby online. |

> [!NOTE]
> * Hello výše seznam chybových zpráv není vyčerpávající. 
> * Nastavení proxy serveru související tooweb chyby se nezobrazí v hello portál Azure classic služby StorSimple Manager. Pokud nastane problém s webový proxy server po dokončení konfigurace hello, změní stav zařízení hello se příliš**Offline** portálu classic hello. |
> 
> 

## <a name="next-steps"></a>Další kroky
* Pokud máte problémy při nasazení zařízení nebo konfigurace nastavení webového proxy serveru, podívejte se příliš[řešení potíží s nasazením zařízení StorSimple](storsimple-troubleshoot-deployment.md).
* jak toouse hello služby StorSimple Manager, přejděte příliš toolearn[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

