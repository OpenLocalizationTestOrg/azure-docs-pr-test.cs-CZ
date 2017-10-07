---
title: "koncovými body Azure CDN aaaTroubleshooting, které vracejí stav 404 | Microsoft Docs"
description: "Řešení potíží s 404 kódy odpovědí s koncovými body Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 450bfbd641c869cfd88169a12c4b69819eaa7c26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Poradce při potížích s koncovými body sítě CDN, které vrací stav 404
Tento článek vám pomůže vyřešit problémy s [koncové body CDN](cdn-create-new-endpoint.md) vrácení chyb 404.

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello MSDN Azure a hello Stack Overflow fóra](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na **získat podporu**.

## <a name="symptom"></a>Příznaky
Vytvoření profilu CDN a koncového bodu, ale obsah asi není k dispozici na hello CDN toobe.  Uživatelé, kteří se pokusí tooaccess obsah prostřednictvím hello CDN URL zobrazí stavové kódy HTTP 404. 

## <a name="cause"></a>Příčina
Existuje několik možných příčin, včetně:

* původ souboru Hello není viditelné toohello CDN
* koncový bod Hello je špatně nakonfigurovaný, způsobuje hello CDN toolook nesprávné místo hello
* Hello hostitele zamítá hello Hlavička hostitele z hello CDN
* koncový bod Hello neproběhla toopropagate čas v rámci hello CDN

## <a name="troubleshooting-steps"></a>Řešení potíží
> [!IMPORTANT]
> Po vytvoření koncového bodu CDN, nebude okamžitě k dispozici pro použití, jak dlouho trvá dobu hello registrace toopropagate prostřednictvím hello CDN.  Pro <b>Azure CDN společnosti Akamai</b> profily, šíření obvykle dokončení během jedné minuty.  V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.  Pokud dokončíte hello kroky v tomto dokumentu a že stále dochází k odpovědi 404, zvažte čeká se na pár hodin toocheck znovu před otevřením lístku podpory.
> 
> 

### <a name="check-hello-origin-file"></a>Zkontrolujte hello zdrojového souboru
Nejdřív jsme hello ověřit tento soubor hello chceme, aby v mezipaměti je k dispozici na našem původ a je veřejně přístupná.  Hello nejrychlejší způsob, jak toodo, který je v relaci v privátní nebo Incognito tooopen prohlížeč a přejděte přímo toohello souboru.  Právě zadejte nebo vložte adresu URL hello do textového pole adresy hello a najdete v části Pokud, výsledkem hello souborů, které očekáváte.  V tomto příkladu toouse mít v účtu Azure Storage, přístupná na soubor kliknu `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Jak vidíte, předá úspěšně hello testu.

![Úspěšné!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Při jde hello nejrychlejší a nejjednodušší způsob, jak tooverify souboru je veřejně dostupné, může poskytnout některé konfigurace sítě ve vaší organizaci, hello dojem, že tento soubor je veřejně dostupné, jestliže je ve skutečnosti jenom viditelné toousers (vaší sítě i když je hostovaná v Azure).  Pokud máte externího prohlížeče, ze kterého můžete otestovat, například mobilní zařízení, které není připojené síti tooyour organizace, nebo virtuální počítač v Azure, která je nejvhodnější.
> 
> 

### <a name="check-hello-origin-settings"></a>Zkontrolujte nastavení pro počátek hello
Teď, když jsme si ověřit hello soubor je veřejně dostupný na Internetu Dobrý den, bylo by měl ověřit naše nastavení počátek.  V hello [portálu Azure](https://portal.azure.com), vyhledejte profil CDN tooyour a klikněte na koncový bod hello se řešení potíží.  V důsledku hello **koncový bod** okně klikněte na počátku hello.  

![Okno koncový bod se zvýrazněnou počátek](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Hello **původu** otevře se okno. 

![Okno počátek](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Typ původu a název hostitele
Ověřte hello **typ původu** je odstraňte a ověřte hello **název počátečního hostitele**.  V tomto příkladu `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, část názvu hostitele hello hello adresa URL je `cdndocdemo.blob.core.windows.net`.  Jak je vidět na snímku obrazovky hello je to v pořádku.  Pro úložiště Azure, webové aplikace a původu cloudové služby, hello **název počátečního hostitele** pole je rozevíracího seznamu, takže jsme nepotřebujete tooworry o kontrola pravopisu správně.  Pokud používáte vlastní původ, je však *absolutně nezbytné* vašeho názvu hostitele je napsán správně!

#### <a name="http-and-https-ports"></a>Porty HTTP a HTTPS
je technologie Hello zde toocheck jiných věcí vaší **HTTP** a **HTTPS porty**.  Ve většině případů 80 a 443 jsou správné, a bude vyžadovat žádné změny.  Ale pokud hello zdrojový server naslouchá na jiný port, který bude nutné toobe, která je tady.  Pokud si nejste jistí, podívejte se jen na hello adresu URL pro zdrojový soubor.  Hello HTTP a HTTPS specifikace zadejte jako hello výchozí porty 80 a 443. V mé URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, není port určen, tak, aby se předpokládá výchozí hello 443 a nastavení jsou správné.  

Ale vyslovení hello adresu URL pro váš zdrojový soubor, který jste dříve neotestovali je `http://www.contoso.com:8080/file.txt`.  Poznámka: hello `:8080` na konci hello hello hostname segmentu.  Hello prohlížeče toouse portu, která oznamuje `8080` tooconnect toohello webový server na `www.contoso.com`, takže budete potřebovat tooenter 8080 v hello **HTTP port** pole.  Je důležité toonote, že tato nastavení portů ovlivňují jenom co koncový bod hello port používá tooretrieve informace z počátku hello.

> [!NOTE]
> **Azure CDN společnosti Akamai** koncové body neumožňují hello plný rozsah portů pro zdroje.  Seznam nepovolených portů původu najdete v tématu [Povolené porty původu Azure CDN společnosti Akamai](https://msdn.microsoft.com/library/mt757337.aspx).  
> 
> 

### <a name="check-hello-endpoint-settings"></a>Zkontrolujte nastavení pro koncový bod hello
Zpět na hello **koncový bod** okně klikněte na tlačítko hello **konfigurace** tlačítko.

![Koncový bod okno s tlačítkem konfigurace](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Hello koncového bodu **konfigurace** otevře se okno.

![Konfigurace okna](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoly
Pro **protokoly**, ověřte, zda je vybrán protokol hello používá hello klientům.  Hello stejný protokol používaná klientem hello bude hello jeden používá tooaccess hello počátek, proto je důležité toohave hello počátek porty v předchozí části hello správně nakonfigurován.  koncový bod Hello naslouchá jenom na hello výchozí porty HTTP a HTTPS (80 a 443), bez ohledu na porty počátek hello.

Umožňuje vrátit tooour hypotetický příklad s `http://www.contoso.com:8080/file.txt`.  Jak budete si pamatovat, zadaný Contoso `8080` jako jejich HTTP port, ale také Předpokládejme, že zadaný `44300` jako jejich port HTTPS.  Pokud vytvořené koncový bod s názvem `contoso`, jejich hostitele koncového bodu CDN by `contoso.azureedge.net`.  Žádost o `http://contoso.azureedge.net/file.txt` je požadavek HTTP, takže koncový bod hello využije HTTP na portu 8080 tooretrieve z počátku hello.  Žádost o zabezpečené pomocí protokolu HTTPS, `https://contoso.azureedge.net/file.txt`, by způsobilo toouse hello koncový bod HTTPS na portu 44300 při načíst hello souboru z počátku hello.

#### <a name="origin-host-header"></a>Hlavičku hostitele zdroje
Hello **hlavičky hostitele počátku** je hello hodnota hlavičky hostitele odesílaná toohello počátek s každou žádostí.  Ve většině případů to by měl být hello stejné jako hello **název počátečního hostitele** jsme ověřit dříve.  Nesprávnou hodnotu v tomto poli obvykle nezpůsobí stav 404, ale je pravděpodobně toocause ostatní 4xx stavy, v závislosti na tom, jaké hello počátek očekává.

#### <a name="origin-path"></a>Cesty ke zdroji
A konečně, bylo by měl ověřit naše **cesty ke zdroji**.  Ve výchozím nastavení je toto pole prázdné.  Toto pole měli používat jenom Pokud chcete, aby toonarrow hello oboru hello hostované počátek prostředků chcete toomake k dispozici na hello CDN.  

Například v mé koncový bod, měla být všechny prostředky na můj účet toobe úložiště k dispozici, tak I zbývajících **cesty ke zdroji** prázdné.  To znamená, že žádost o příliš`https://cdndocdemo.azureedge.net/publicblob/lorem.txt` výsledkem připojení z mé koncového bodu příliš`cdndocdemo.core.windows.net` který požadavky `/publicblob/lorem.txt`.  Podobně žádost o `https://cdndocdemo.azureedge.net/donotcache/status.png` výsledkem požadavku koncového bodu hello `/donotcache/status.png` z počátku hello.

Ale co když nemá být toouse hello CDN pro každou cestu na můj počátek?  Řekněme I pouze chtěli tooexpose hello `publicblob` cesta.  Pokud lze zadávat */publicblob* v mé **cesty ke zdroji** pole, které způsobí, že koncový bod tooinsert hello */publicblob* před každým požadavkem prováděné toohello původu.  To znamená, že hello žádost o `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` bude nyní ve skutečnosti trvat hello požadavek část adresy URL, hello, `/publicblob/lorem.txt`a připojte `/publicblob` toohello začátku. Výsledkem je žádost o `/publicblob/publicblob/lorem.txt` z počátku hello.  Pokud tato cesta se nepřekládá tooan skutečný soubor, počátek hello vrátí 404 stavu.  ve skutečnosti Hello správnou adresu URL tooretrieve lorem.txt v tomto příkladu by `https://cdndocdemo.azureedge.net/lorem.txt`.  Všimněte si, že jsme neobsahují hello */publicblob* cesta vůbec, protože část požadavku hello hello adresa URL je `/lorem.txt` a přidá koncový bod hello `/publicblob`, což `/publicblob/lorem.txt` hello požadavek předávány toohello původu .

