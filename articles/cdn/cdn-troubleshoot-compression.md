---
title: "kompresí souborů aaaTroubleshooting v Azure CDN | Microsoft Docs"
description: "Vyřešte problémy s kompresí souborů Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f00b98beaf6b3b3cd30108ece65a8191edc06ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-file-compression"></a>Poradce při potížích s kompresí souborů CDN
Tento článek vám pomůže vyřešit problémy s [komprese souboru CDN](cdn-improve-performance.md).

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello MSDN Azure a hello Stack Overflow fóra](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na tlačítko **získat podporu**.

## <a name="symptom"></a>Příznaky
Je povolená komprese pro svůj koncový bod, ale soubory se vrací nekomprimované.

> [!TIP]
> toocheck zda vaše soubory se vrací komprimovaný, je nutné toouse jako nástroj [Fiddler](http://www.telerik.com/fiddler) nebo prohlížeče [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Hlavičky odpovědi zkontrolujte hello HTTP vrátil s vaší mezipaměti CDN obsahu.  Pokud je záhlaví s názvem `Content-Encoding` s hodnotou **gzip**, **bzip2**, nebo **deflate**, obsah je komprimován.
> 
> ![Kódování obsahu hlaviček](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Příčina
Existuje několik možných příčin, včetně:

* Hello požadovaného obsahu nejsou vhodné pro kompresi.
* Není povolená komprese hello požadovaný typ souboru.
* požadavek HTTP Hello nezahrnuli hlavičku požaduje typ platný komprese.

## <a name="troubleshooting-steps"></a>Řešení potíží
> [!TIP]
> Stejně jako u nasazení nové koncové body, provést změny konfigurace CDN některé čas toopropagate přes síť hello.  Obvykle změny se použijí během 90 minut.  Pokud je to hello poprvé, co jste nastavili komprese pro koncový bod CDN, měli byste zvážit čeká se, že rozšíření bodů POP toohello nastavení komprese hello 1 – 2 hodiny toobe. 
> 
> 

### <a name="verify-hello-request"></a>Ověřit požadavek hello
Nejdřív by měl provedeme kontrolu rychlé správností hello požadavku.  Můžete použít v prohlížeči na [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello požadavků zasílaných.

* Ověřte hello odesílá se požadavek adresy URL koncového bodu tooyour, `<endpointname>.azureedge.net`a ne do zdrojového umístění.
* Ověřit požadavek hello obsahuje **Accept-Encoding** záhlaví a hello hodnotu této hlavičky obsahuje **gzip**, **deflate**, nebo **bzip2** .

> [!NOTE]
> **Azure CDN společnosti Akamai** profily podporuje se jen **gzip** kódování.
> 
> 

![Hlavičky žádosti CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Ověřte nastavení komprese (profil CDN úrovně Standard)
> [!NOTE]
> Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN Standard od společnosti Verizon** nebo **Azure CDN Standard od společnosti Akamai** profilu. 
> 
> 

Přejděte tooyour koncového bodu v hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko hello **konfigurace** tlačítko.

* Ověřte, že je povolená komprese.
* Ověřte hello typ MIME pro hello obsahu toobe komprimované je obsažena v seznamu hello komprimované formátů.

![Nastavení komprese CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Ověřte nastavení komprese (profil Premium CDN)
> [!NOTE]
> Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN Premium od společnosti Verizon** profilu.
> 
> 

Přejděte tooyour koncového bodu v hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko hello **spravovat** tlačítko.  Otevře se Hello doplňkovém portálu.  Hover přes hello **HTTP velké** kartu a potom hover přes hello **nastavení mezipaměti** plovoucím panelem.  Klikněte na tlačítko **komprese**. 

* Ověřte, že je povolená komprese.
* Ověřte hello **typy souborů** seznam obsahuje seznam oddělený čárkami (bez mezer) typů standardu MIME.
* Ověřte hello typ MIME pro hello obsahu toobe komprimované je obsažena v seznamu hello komprimované formátů.

![Nastavení komprese CDN premium](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-hello-content-is-cached"></a>Ověřte, zda text hello obsah se uloží do mezipaměti
> [!NOTE]
> Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN společnosti Verizon** profil (Standard nebo Premium).
> 
> 

Používání nástrojů pro vývojáře v prohlížeči, zkontrolujte, že hello odpověď záhlaví tooensure hello souboru je uložené v mezipaměti v hello oblasti, kde jsou požadovány.

* Zkontrolujte hello **Server** hlavičky odpovědi.  Hlavička Hello by měl mít formát hello **platformy (ID POP nebo serveru)**, jak je vidět v hello následující ukázka.
* Zkontrolujte hello **X mezipaměti** hlavičky odpovědi.  záhlaví Hello měli přečíst **DOSÁHL**.  

![Hlavičky odpovědi CDN](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-hello-file-meets-hello-size-requirements"></a>Ověřte, zda text hello soubor splňuje požadavky na velikost hello
> [!NOTE]
> Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN společnosti Verizon** profil (Standard nebo Premium).
> 
> 

toobe vhodné pro kompresi, soubor, musí splňovat následující požadavky na velikost hello:

* Větší než 128 bajtů.
* Menší než 1 MB.

### <a name="check-hello-request-at-hello-origin-server-for-a-via-header"></a>Zkontrolujte hello požadavek na původní server hello pro **prostřednictvím** záhlaví
Hello **prostřednictvím** hlavičky protokolu HTTP označuje toohello webový server, který hello požadavku je předávána pomocí serveru proxy.  Webové servery Microsoft IIS ve výchozím nastavení Nekomprimovat odpovědí hello požadavek obsahuje **prostřednictvím** záhlaví.  toooverride toto chování, proveďte následující hello:

* **Služby IIS 6**: [nastavit HcNoCompressionForProxies = "FALSE" ve vlastnostech hello metabáze služby IIS](https://msdn.microsoft.com/library/ms525390.aspx)
* **Službu IIS 7 a vyšší**: [nastavit obě **noCompressionForHttp10** a **noCompressionForProxies** tooFalse v konfiguraci serveru hello](http://www.iis.net/configreference/system.webserver/httpcompression)

