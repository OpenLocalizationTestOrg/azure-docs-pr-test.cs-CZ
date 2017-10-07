---
title: "aaaMicrosoft ověřovací telefonní přihlášení - účty Azure a Microsoft | Microsoft Docs"
description: "Použijte váš telefon toosign v tooyour účtu Microsoft místo zadávání hesla. Tento článek obsahuje nejčastější dotazy k odpovědi na tuto funkci."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: a4911ff580b3ffa078299ad706d099330b75a2e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-with-your-phone-not-your-password"></a>Přihlaste se pomocí telefonu není heslo

aplikace Microsoft Authenticator Hello pomáhá vám zabezpečit vaše účty provedením dvoustupňové ověření po zadání hesla. Ale víte, že ho můžete zcela nahradit hello heslo pro svůj osobní účet Microsoft? 

Tato funkce je k dispozici na zařízení iOS a Android a funguje s osobní účty Microsoft. 

## <a name="how-it-works"></a>Jak to funguje

Řada z vás používat aplikaci Microsoft Authenticator hello pro dvoustupňové ověření při přihlašování tooyour účtu Microsoft. Zadejte heslo a potom přejděte toohello aplikace tooeither schválit oznámení nebo získat ověřovací kód. Phone přihlášení přeskočte hello heslo a udělejte všechny ověření identity na váš telefon. Vzhledem k tomu, že přihlášení phone je typ dvoustupňové ověření, musíte stále tooprovide věcí, které znáte a věcí, máte tooverify vaši identitu. Hello phone pořád hello věcí, kterou máte, a PIN kód nebo biometrické klíč váš telefon je hello, co víte. 

## <a name="how-tooget-started"></a>Způsob spuštění tooget

toosign v tooyour osobní účet Microsoft s telefonu, postupujte takto: 

1. Povolte přihlášení phone pro váš účet. 

  - Pokud nemáte aplikaci Microsoft Authenticator hello ještě, nainstalujte a přidejte svůj osobní účet Microsoft podle toohello kroky hello [Microsoft Authenticator stránky](microsoft-authenticator-app-how-to.md). Nově přidaný účty jsou automaticky povolené, takže jste dobrý toogo.

  - Pokud už používáte Microsoft Authenticator pro dvoustupňové ověření, vyberte svůj účet z domovské stránky aplikace hello a vyberte **povolit přihlášení phone** z rozevírací nabídky hello.

  >[!NOTE] 
  >tooprotect vašeho účtu, potřebujeme kód PIN nebo biometrické zámku na vašem zařízení. Když necháte telefonu odemčený, hello aplikace se zobrazí žádost o žádostí tooset až zámek před povolením phone přihlásit. 

3. Většina stránek, které by normálně zadejte heslo účtu Microsoft mají odkaz s informacemi o tom **místo toho použít aplikaci**. Vyberte tento odkaz toosign pomocí telefonu. 

4. Microsoft odesílá oznámení tooyour telefon. Schvalte hello oznámení toosign v tooyour účtu.   

## <a name="faq"></a>Nejčastější dotazy 

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Jak se přihlásit telefon bezpečnější než zadáním hesla?  

Většina lidí dnes přihlašovací tooweb weby nebo aplikace pomocí uživatelského jména a hesla.  Hesla se bohužel často ztráty, krádeže nebo uhádnout hackerů. Při nastavování toosign aplikace Microsoft Authenticator hello v jsme vygenerovat klíč na váš telefon, který můžete odemknout svůj účet. Jsme chránit tento klíč se hello PIN kód nebo biometrické, že už používáte na váš telefon.  Když se přihlásíte pomocí telefonu, tento klíč je použité tooprove vaší identity bezpečně se dvěma faktory – hello phone sám a vaše možnost toounlock ho. 

Hello klíč používaný je podobné toohello klíčů používaných v systému Windows Hello a specifikace FIDO Alliance UAF hello. Vaše bio dat je pouze použili tooprotect hello klíč místně a je nikdy odeslat nebo uložené v cloudu hello. 
 
### <a name="where-can-i-use-my-phone-tooreplace-my-password-and-where-would-i-still-need-hello-password"></a>Kde je možné používat Můj telefon tooreplace hesla a kde stále je nutné heslo hello?  

V současné době hello phone přihlášení funkce funguje pouze s webové aplikace a služby, které jsou zapnuté osobní účty Microsoft, iOS nebo Android aplikace, které používají osobní účet Microsoft a aplikace na Windows 10, které používají osobní účet Microsoft. Při přihlášení tooone tyto weby nebo aplikace na stránce hello kde obvykle zadat heslo je odkaz, který je uveden **místo toho použít aplikaci**. 

Přihlášení Phone nelze použít toounlock na počítači s Windows, XBOX nebo všechny plochy verze aplikací Microsoftu, jako je například aplikace Office v tuto chvíli. 
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>To nahradit dvoustupňové ověřování? Měli vypnout jeho nastavení?   

V některých případech. Pracujeme na rozšíření hello oboru phone přihlásit, ale zatím stále existují místech ekosystém Microsoft hello, které ji nepodporují. Na těchto místech stále používáme dvoustupňové ověřování pro zabezpečené přihlašování. Z tohoto důvodu Ne, byste neměli vypínat dvoustupňové ověřování pro váš účet. 
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-do-i-have-tooapprove-two-notifications"></a>V pořádku Pokud I pro můj účet zapnuté dvoustupňové ověření, je nutné provést tooapprove dvou oznámení?

Ne, můžete nebude. Přihlášení tooyour účet Microsoft s telefonu se počítá jako dvoustupňové ověřování. Místo zadání heslo, pak schválení oznámení prokázání své identity tím zároveň budete vědět, jak toounlock telefonu a potom schválení oznámení. Nebude odešleme vám druhý tooapprove oznámení.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>Co když I ztratíte telefon nebo nemáte s mi, jak lze zobrazit Můj účet?  

Vždy můžete kliknout na **místo toho použít heslo** na přihlašovací stránce tooswitch hello zpět toousing heslo. Uvědomte si, že pokud používáte dvoustupňové ověření, je stále nutné druhý metoda tooverify vaše přihlášení. Proto důrazně doporučujeme toomake jistotu, že mají navíc, aktuální bezpečnostní údaje na vašem účtu. Můžete spravovat vaše bezpečnostní údaje na adrese https://account.live.com/proofs/manage. 
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-tooentering-my-password"></a>Jak přestat používat tuto funkci a vraťte tooentering hesla?

Klikněte na tlačítko **místo toho použít heslo** při přihlašování. Jsme mějte na paměti, poslední volba a nabízejí který ve výchozím nastavení hello při příštím přihlášení. Pokud chcete někdy toogo back toosigning pomocí telefonu, klikněte na tlačítko **místo toho použít aplikaci**. 
 
### <a name="can-i-use-hello-app-toosign-in-tooall-my-accounts-with-microsoft"></a>Můžete použít toosign aplikace hello v tooall Moje účty s Microsoft?   
Tato funkce je k dispozici pro osobní účty Microsoft pouze v tuto chvíli. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Může I Přihlaste se k počítači s telefon?  
U počítačů doporučujeme, abyste přihlášení pomocí Windows Hello ve Windows 10 pomocí vaší řez, otisk prstu nebo PIN kód.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>Můžete Moje Windows Phone přihlásit?  
V tuto chvíli jsme nejsou vývoj tuto funkci pro hello Microsoft Authenticator na Windows Phone. 

## <a name="next-steps"></a>Další kroky
Pokud jste ještě aplikaci Microsoft Authenticator hello, ji rezervovat. je k dispozici pro aplikace Hello [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), a přihlášení phone jsou dostupné v aplikaci Microsoft Authenticator hello pro [Android](http://go.microsoft.com/fwlink/?Linkid=825072) a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Pokud máte dotazy týkající se aplikace hello obecně, prohlédněte si hello [Microsoft Authenticator nejčastější dotazy](microsoft-authenticator-app-faq.md)
