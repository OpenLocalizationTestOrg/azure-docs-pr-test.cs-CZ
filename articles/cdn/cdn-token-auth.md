---
title: "prostředky Azure CDN aaaSecuring pomocí tokenu ověřování | Microsoft Docs"
description: "Pomocí ověřování tokenem toosecure přístup tooyour Azure CDN prostředky."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/11/2016
ms.author: mezha
ms.openlocfilehash: 5865bcb8eed7ced834970d52d30136252039265f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Zabezpečení prostředků Azure CDN se ověření pomocí tokenu

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

##<a name="overview"></a>Přehled

Ověření pomocí tokenu je mechanismus, který vám umožní tooprevent Azure CDN z obsluhující prostředky toounauthorized klientů.  To se obvykle provádí tooprevent "hotlinking" obsahu, kde používá jiný web, často zpráva Tabule, vaše prostředky bez povolení.  To může mít vliv na vaše náklady na doručování obsahu. Povolením této funkce na CDN se ověřit požadavky edge CDN POP před doručením hello obsah. 

## <a name="how-it-works"></a>Jak to funguje

Ověření pomocí tokenu ověřuje, že požadavky generované jako důvěryhodný web vyžadováním toocontain žádosti o token hodnotu obsahující kódovaného informace o hello žadatel. Obsah se pouze zpracuje toorequester hello s kódováním informace plnění hello požadavky, jinak požadavky budou odepřeny. Můžete nastavit požadavek hello pomocí jednoho nebo více parametrů níže.

- Země: Povolit nebo odmítnout požadavky, které pocházely ze zadaného zemích.  [Seznam platných kódů.](https://msdn.microsoft.com/library/mt761717.aspx) 
- Adresa URL: Povolte jenom zadaný toorequest asset nebo cestu.  
- Hostitele: Povolit nebo odepřít pomocí zadaní hostitelé v hlavičce žádosti hello požadavky.
- Odkazující server: Povolit nebo odepřít toorequest odkazující zadaný server.
- IP adresa: pouze povolit požadavky, které pochází z konkrétní IP adresu nebo podsíť protokolu IP.
- Protokol: Povolit nebo blokovat žádosti na základě protokolu hello používá toorequest hello obsah.
- Čas vypršení platnosti: přiřaďte datum a čas doby tooensure, odkaz pouze zůstávají platná po omezenou dobu.

Podívejte se na příklad podrobnou konfiguraci jednotlivých parametrů.

## <a name="reference-architecture"></a>Referenční architektura

Najdete níže referenční architektura nastavení ověření pomocí tokenu na CDN toowork s vaší webové aplikace.

![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Ověření tokenu logiky na koncový bod CDN
    
Tento graf popisuje, jak Azure CDN ověří požadavek klienta po ověření tokenu je nakonfigurovaná na koncový bod CDN.

![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Nastavení ověření pomocí tokenu

1. Z hello [portál Azure](https://portal.azure.com), procházet profil CDN tooyour a pak klikněte na tlačítko hello **spravovat** tlačítko toolaunch hello doplňkovém portálu.

    ![Tlačítko Spravovat okno profil CDN](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Pozastavte ukazatel myši nad **HTTP velké**a potom klikněte na **tokenu ověřování** v plovoucím panelem hello. Nastavíte šifrovací klíč a parametry šifrování na této kartě.

    1. Zadejte jedinečný šifrovací klíč pro **primární klíč**.  Zadejte jinou pro **zálohování klíče**

        ![CDN tokenu ověřování Instalační klíč](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Nastavit parametry šifrování nástrojem šifrování (povolit nebo odepřít požadavky na základě čas vypršení platnosti, země, odkazující server, protokolu, IP adresa klienta. Můžete použít libovolnou kombinaci.)

        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - ES-vyprší: přiřadí čas vypršení platnosti tokenu po zadaném časovém období. Požadavky na odeslání po dobu vypršení platnosti hello budou odepřeny. Tento parametr používá časové razítko systému Unix (podle počet sekund od standardní epoch 1/1/1970 00:00:00 GMT. Můžete použít nástroje online tooprovide převod mezi (běžný čas) a Unix času.)  Například pokud si chcete tooset tokenu toobe hello vypršení platnosti na 12/31. prosinci 2016 12:00:00 GMT, použijte hello Unix čas: 1483185600 jak je uvedeno níže:
    
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - Povolit adresu url ES: vám umožní tootailor tokeny tooa konkrétní prostředek nebo cestu. Omezuje přístup toorequests, jehož adresa URL začínat konkrétní relativní cesty. Můžete zadat více cest jednotlivé cesty oddělte čárkou. URL – adresy rozlišují velká a malá písmena. V závislosti na hello požadavek můžete nastavit jinou hodnotu tooprovide různé úrovně přístupu. Níže je několik scénářů:
        
            Pokud máte adresu URL: http://www.mydomain.com/pictures/city/strasbourg.png. Vstupní hodnota najdete v části "" a jeho přístup na úrovni odpovídajícím způsobem

            1. Vstupní hodnota "/": všechny požadavky budou povoleny.
            2. Vstupní hodnota "/ obrázků": všechny hello následující požadavky vám umožní.
            
                - http://www.mydomain.com/Pictures.PNG
                - http://www.mydomain.com/Pictures/City/strasbourg.PNG
                - http://www.mydomain.com/picturesnew/City/strasbourgh.PNG
            3. Vstupní hodnota "/ obrázky /": pouze požadavky pro /pictures/ bude možné.
            4. Vstupní hodnota "/ pictures/city/strasbourg.png": pouze umožňuje požadavku pro tento prostředek
    
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - Povolit země ES: umožňuje pouze požadavky, které pocházejí z jedné nebo více zadaný zemí. Požadavky, které pocházejí z jiných zemí budou odepřeny. Použijte tooset kód země hello parametry a každý kód země oddělíte čárkou. Například pokud chcete přístup tooallow z USA a Francie, zadejte nám FR ve sloupci hello jako níže.  
        
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - Odepřít země ES: odmítne požadavky, které pochází z jedné nebo více zadaný zemí. Požadavky, které pocházejí z jiných zemí bude možné. Použijte tooset kód země hello parametry a každý kód země oddělíte čárkou. Například pokud chcete přístup toodeny z USA a Francie, zadejte nám FR ve sloupci hello.
    
        - Povolit ref ES: umožňuje pouze požadavky z odkazující zadaný server. Odkazující server identifikuje hello webové stránky, která propojené toohello prostředků požadovanou adresu URL hello. Hodnota parametru odkazující server Hello by neměla zahrnovat hello protokolu. Můžete zadat název hostitele nebo na konkrétní cestu na tento název hostitele. Můžete také přidat více odkazujících serverů v rámci jeden parametr oddělení každé z nich oddělujte čárkami. Pokud jste zadali hodnotu odkazující server, ale informace odkazující server hello neposílají v žádosti o hello z důvodu konfigurace prohlížeče toosome, tyto požadavky budou odepřeny ve výchozím nastavení. Můžete přiřadit "Chybí" nebo prázdná hodnota v parametru tooallow hello tyto požadavky s chybějícími informacemi o odkazující server. Můžete také použít "*. consoto.com" tooallow všechny subdomény consoto.com.  Například pokud chcete přístup tooallow pro žádosti od www.consoto.com, všem dílčím doménám consoto2.com a erquests s prázdné nebo chybějící reffers, zadejte hodnotu níže:
        
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - Odepřít ref ES: odmítne požadavky z odkazující zadaný server. Najdete toodetails a příklad v parametru "ES ref povolit".
         
        - umožňují proto – ES: pouze povoluje požadavky z zadaný protokol. Například http nebo https.
        
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - ES proto – odepřít: odmítne požadavky z zadaný protokol. Například http nebo https.
    
        - Když ES: omezuje přístup toospecified žadatel IP adresu. Jsou podporovány adresy IPV4 a IPV6. Můžete zadat jednu žádost IP adresu nebo podsíť protokolu IP.
            
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Můžete otestovat váš token s nástrojem pro dešifrování hello.

    4. Můžete také upravit hello typ odpovědi, který bude vrácen toouser po odepření požadavku. Ve výchozím nastavení používáme 403.

3. Klikněte na tlačítko **stroj pravidel** v části **HTTP velké**. Budete používat tato karta toodefine cesty tooapply hello funkce, povolit funkci hello ověření pomocí tokenu a povolit další ověření tokenu související možnosti.

    - Použití "Pokud" sloupec toodefine asset nebo cestu, že chcete tooapply ověření tokenu. 
    - Klikněte na tlačítko tooadd "Tokenu ověřování" z hello funkce rozevírací tooenable tokenu ověřování.
        
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. V hello **stroj pravidel** se nachází několik dalších možností můžete povolit.
    
    - Token denial kód ověřování: Určuje hello typ odpovědi, který bude vrácen toouser po odepření požadavku. Pravidla zde nastaveno přepíše hello denial kód nastavení na kartě hello tokenu ověřování.
    - Ignorovat tokenu ověřování: Určuje, zda bude token toovalidate adresa URL použitá malá a velká písmena.
    - Parametr tokenu ověřování: Přejmenovat hello tokenu ověřování dotazu parametr řetězce zobrazující v hello požadovaná adresa URL. 
        
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Váš token zabezpečení, které je aplikace, která generuje token pro funkce na základě tokenu ověřování můžete přizpůsobit. Zdrojový kód je přístupná zde v [Githubu](https://github.com/VerizonDigital/ectoken).
Dostupné jazyky patří:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN funkce a zprostředkovatele ceny

V tématu hello [přehled CDN](cdn-overview.md) tématu.
