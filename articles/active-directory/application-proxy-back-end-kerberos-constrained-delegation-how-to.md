---
title: "aaaHow tooconfigure toouse aplikace Proxy aplikace omezené delegování Kerberos | Microsoft Docs"
description: "Jak tooconfigure omezené delegování Kerberos pro aplikaci Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 93dac264ff1d3b99dead1d9c759c37c59373cbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application-toouse-kerberos-constrained-delegation"></a>Jak tooconfigure toouse aplikace Proxy aplikace omezené delegování Kerberos

Hello dostupné metody pro dosažení toopublished jednotné přihlašování, které aplikace můžou poněkud lišit od tooapplication aplikace a jednu z možností hello, Proxy aplikace služby Azure nabízí vpravo předinstalované hello, je pomocí protokolu Kerberos vynuceným delegování použitím (KCD). Toto je, kde hostitel konektor je nakonfigurované tooperform omezené kerberos ověření toobackend aplikacemi, jménem uživatelů.

Hello skutečné postup pro povolení použitím KCD je relativně jednoduché a obvykle vyžaduje více než obecné znalosti o hello různými součástmi a tok ověřování, která usnadňuje jednotné přihlašování. Hledání, že poskytují toohelp informace o řešení potíží s scénáře, kde použitím KCD SSO nebude fungovat podle očekávání, může být Zhuštěno.

Tento článek jako takový pokusí tooprovide jediný bod odkaz, který by měly pomoci při řešení potíží a samoobslužné napravit některé nalezené hello nejběžnějších problémů, které vidíte. Na hello stejný čas nabídka Další pokyny pro diagnostiku hello složitější a problémových implementace.

Všimněte si, že v tomto článku umožňuje hello následující předpoklady:

-   Proxy aplikace služby Azure nasazený dle naše [dokumentace](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) a obecné přístup toonon použitím KCD aplikace funguje podle očekávání.

-   Hello publikované cílová aplikace je založena na službě IIS a společnosti Microsoft implementace protokolu kerberos.

-   hostitelé serveru a aplikace Hello nacházet v jedné doméně služby Active Directory. Podrobné informace o pro různé scénáře domény a doménové struktury naleznete v našem [dokument White Paper použitím KCD](http://aka.ms/KCDPaper).

-   Hello subjektu aplikace je publikována v Azure klienta povoleno předběžné ověřování, uživatelé se očekává, že a ověřování na základě tooauthenticate tooAzure prostřednictvím formulářů. Scénáře ověřování klienta bohaté nejsou uvedené v tomto článku, ale je možné přidat v určitém okamžiku v budoucnu hello.

## <a name="prerequisites"></a>Požadavky

Proxy aplikace služby Azure se dá nasadit na prakticky libovolný typ infrastruktury nebo prostředí a hello architektury už nepochybně lišit tooorganization organizace. Mezi nejběžnější příčiny hello použitím KCD související s problémy s nejsou hello prostředí, sami, ale spíše jednoduché chybná konfigurace nebo obecné dohledu.

Z tohoto důvodu naše Rady, jak je vždy toostart tím, že zajistí jste splnili všechny hello předpoklady nastíněny v našem hlavní [pomocí použitím KCD jednotné přihlašování, článek Proxy aplikace hello](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) před spuštěním řešení potíží.

Zejména hello části na konfiguraci použitím KCD na 2012R2, aktivuje se podstatně liší přístup tooconfiguring použitím KCD v dřívějších verzích systému Windows, ale také při se s vědomím několik další důležité informace:

-   Je běžné pro tooopen serveru člena domény zabezpečený kanál dialogové okno s konkrétním řadičem domény. Pak přesuňte tooanother dialogové okno v každém okamžiku, takže konektor hostitele obecně by nemělo být schopný toocommunicate s omezeným přístupem toobeing se pouze na určité místní lokality řadiče domény.

-   Podobně jako toohello výše bodu scénáře napříč doménami spoléhají na referenční seznamy, které budou řídit tooDCs hostitele konektoru, která se mohou nacházet mimo hraniční hello místní sítě. V tomto scénáři, který je stejně důležité toomake se, že jste se také umožňuje provoz a vyšší tooDCs, které představují další příslušných domén, nebo s jiným delegování nezdaří.

-   Kde je to možné, vyhněte se umístění všech aktivních zařízení IP adresy nebo ID mezi hostiteli v konektoru a řadiče domény, jako jsou někdy přes nežádoucí a narušovat přenos pomocí protokolu RPC jádra

Tolik, kolik rádi bychom znali tooresolve problémy rychle a efektivně, může trvat dobu, kde je to možné které by měl zkuste a otestovat delegování v hello nejjednodušší scénářů. Dobrý den další proměnné, které můžete zavést, hello více můžete mít toocontend s. Například omezení testování jeden konektor tooa můžete uložit drahocenný čas a možné přidat další konektory hello problémy byl vyřešen.

Některé faktorech prostředí může taky způsobovat tooan problém, takže znovu Pokud možné zkuste a minimalizovat úplné minimální tooa hello architektura pro testování. Například, nejsou nesprávně nakonfigurované interní brány firewall seznamy ACL, takže pokud je to možné mít veškerý provoz z konektoru přímo přes toohello řadičů domény a back-end aplikace povolená. 

Ve skutečnosti hello absolutní nejlepší místo tooposition konektory je co tootheir cíle jako může být. S vložené den brány firewall a přitom se jednoduše testování přidá nepotřebné složitost a může prodloužit jenom vaše šetření.

Ano co se považuje za problém použitím KCD přesto? Dobře existuje několik classic indikace selhání a hello první známky problém obvykle projevují v přihlašování použitím KCD hello prohlížeče.

   ![Chyba konfigurace nesprávný použitím KCD](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Ověřování se nezdařilo z důvodu toomissing oprávnění](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

všechny, které opatřeny hello stejné příznakem selhání tooperform jednotné přihlašování a v důsledku odepření hello aplikace toohello přístup uživatelů.

## <a name="troubleshooting"></a>Řešení potíží

Jak pak řešení závisí na hello problém a zjištěnými příznaků. Před přechodem všechny další by doporučujeme hello následující odkazy, jak budou obsahovat užitečné informace, které jste ještě nemusí mít pocházet napříč:

-   [Řešení potíží s Proxy aplikace problémy a chybové zprávy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot)

-   [Chyby protokolu Kerberos a symptomy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#kerberos-errors)

-   [Práce s SSO při místní a cloudové identity nejsou identické](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd#working-with-sso-when-on-premises-and-cloud-identities-are-not-identical)

Pokud máte k dispozici to daleko, pak hello hlavní problém výborný existuje. Je třeba toodig hlubší, takže začněte tím, že oddělíte hello toku do tří různých fází, které lze řešit potíže s.

**Předběžné ověření klienta** -hello externího uživatele ověřování tooAzure prostřednictvím prohlížeče.

Je možné toopre-ověření je nutné pro jednotné přihlašování použitím KCD toofunction tooAzure. To by měla otestovat a řešit nejdříve, pokud jsou nějaké problémy. Všimněte si, že předběžné ověřování fáze hello nejsou žádná vztah tooKCD nebo hello publikované aplikace. Měla by být poměrně snadno toocorrect případné nesrovnalosti podle správností kontrola hello subjektu účet existuje v Azure a že není zakázána nebo blokované. odpovědi na chybu Hello v prohlížeči hello je obvykle popisný dostatečně toounderstand hello příčina. Pokud si nejste jisti, můžete také zkontrolovat našich dalších tooverify dokumentace Poradce při potížích.

**Delegování služby** -hello konektor Azure Proxy získání lístků služby protokolu kerberos z KDC (Kerberos Distribution Center) jménem uživatele.

Hello externí komunikace mezi klientem hello a hello Azure front-endu by měl mít žádný vliv na použitím KCD jakkoli, jiné než zajistit, že funguje. Toto je tak hello služba Azure Proxy může být zadaný s platné ID uživatele, který být použité tooobtain lístek protokolu kerberos. Bez této použitím KCD není možné a dojde k selhání.

Jak je uvedeno nahoře, hello prohlížeče chybové zprávy obvykle nabízejí některé funkční různá vodítka na Proč se nedaří věcí. Zkontrolujte že toonote hello ID aktivity a časové razítko v odpovědi hello jako to umožní toocorrelate hello chování tooactual události v protokolu událostí hello Azure Proxy.

   ![Chyba konfigurace nesprávný použitím KCD](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

A protokolu událostí zaznamenané hello odpovídající položky hello by se zobrazit jako události 13019 nebo 12027. Můžete najít hello konektor protokoly událostí v **protokoly aplikací a služeb** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt; **Konektor**&gt;**správce**.

   ![Událost 13019 z protokolu událostí aplikace Proxy](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![12027 události z protokolu událostí aplikace Proxy](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

-   Pro aplikace hello adresa a není záznam CName použít záznam v interní DNS

-   Znovu zkontrolujte, které hostují konektor hello byla udělena hello práva toodelegate toohello určený název SPN pro cílový účet a že **používající libovolný protokol pro ověřování** je vybrána. To je popsaná v našem hlavní [článku Konfigurace jednotného přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd)

-   Ověřte, že je po vydání pouze jednu instanci hello SPN v existence ve službě AD **setspn - x** z příkazový řádek na všechny domény členského hostitele

-   Zkontrolujte toosee, pokud je zásada domény vynucené toolimit hello [maximální velikost protokolu kerberos vydaných tokenů](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/), protože tento konektor hello zabránit získat token, pokud nalezen toobe nadměrné

Trasování sítě zaznamenávání hello výměnách mezi hello konektor hostitele a domény služby KDC by pak hello další nejlepší krok při získávání více nízkou úroveň podrobností na hello problémy. Můžete najít v [podrobné informace Poradce při potížích dokumentu](https://aka.ms/proxytshootpaper).

Pokud lístků spokojeni, měli byste vidět událost v protokolech hello s informacemi o tom, ověření se nezdařilo z důvodu toohello aplikace vrací 401. To běžně značí, že cílová aplikace hello odmítat lístku, takže pokračujte hello následující další fáze.

**Cílová aplikace** -hello příjemce lístek protokolu kerberos hello poskytované hello konektoru

Tento konektor hello fáze je očekávané toohave odesílány protokolu kerberos služby lístku toohello back-end, jako hlavičku v rámci hello první žádostí na aplikace.

-   Pomocí aplikace hello interní adresa URL definované v hello portál, ověřte, že aplikace hello dostupné přímo z prohlížeče hello na hostiteli konektor hello. Potom může přihlásit úspěšně. Podrobnosti o to naleznete na stránce Poradce při potížích konektor hello.

-   Stále v hostiteli konektor hello, potvrďte, že hello ověřování mezi hello prohlížeče a aplikace hello je pomocí protokolu kerberos, jedním z následujících hello:

1.  Spuštění nástrojů pro vývojáře (**F12**) v Internet Exploreru, nebo použijte [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) z hostitele konektor hello. Přejděte toohello aplikace pomocí hello interní adresa URL a zkontrolujte hello nabízí hlavičky WWW ověření vrácený v odpovědi hello z aplikace hello, tooensure, která buď vyjednávání nebo protokolu kerberos je k dispozici. Vrátí objekt blob následné protokolu kerberos se v reakci hello z prohlížeče toohello hello aplikace obvykle začínáte s **JÍ**, takže toto je dobrá indikace toho Kerberos se v play. NTLM na hello druhé straně vždy začínat **TlRMTVNTUAAB**, který čte NTLMSSP při dekódovat z formátu Base64. Pokud se zobrazí **TlRMTVNTUAAB** při spuštění hello objektu hello blob, to znamená, že se protokol Kerberos **není** k dispozici. Pokud to nevidíte, pomocí protokolu Kerberos je pravděpodobně k dispozici.

  * Všimněte si, pokud používáte aplikaci Fiddler, tato metoda by vyžadovaly dočasně zakázat rozšířené ochrany na aplikace hello konfigurace ve službě IIS.

     ![Okno kontroly sítě prohlížeče](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    *Obrázek:* vzhledem k tomu, že to nezačíná TIRMTVNTUAAB, jedná se o příklad Kerberos je k dispozici. Jedná se o příklad objektu Blob protokolu Kerberos, který nezačíná JÍ.

2.  NTLM dočasně odebrání seznam zprostředkovatelů hello na webu a přístup aplikace IIS přímo z prohlížeče IE na hostiteli konektor. Pomocí protokolu NTLM již v seznamu zprostředkovatelů hello musí být schopný tooaccess aplikace hello pouze pomocí protokolu Kerberos. Pokud tato možnost selže, pak navrhující, že došlo k potížím s konfigurací aplikace hello a ověřování protokolu Kerberos nefunguje.

Protokolu Kerberos není k dispozici, aplikace hello kontroly ověřování, které vyjednávání nastavení v IIS toomake, že je uvedena nejhornější prostřednictvím NTLM pod ní. (Není Negotiate: ověřování protokolu kerberos nebo Negotiate: protokolu PKU2U). Pokračujte pouze v případě protokolu Kerberos je funkční.

   ![Zprostředkovatelé ověřování systému Windows](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
-   Pomocí protokolu Kerberos a NTLM na místě umožňuje nyní dočasně zakázat předběžné ověřování pro aplikaci hello hello portálu. V tématu, pokud je dostupný na hello Internetu pomocí hello externí adresu URL. By měl být výzvami tooauthenticate a musí být schopný toodo, tak s hello účet hello používá v předchozím kroku. Pokud ne, to znamená problém s back-end aplikace hello a není použitím KCD vůbec.

-   Nyní znovu povolte předběžné ověření hello portálu a ověřit prostřednictvím Azure tak, že pokus tooconnect toohello aplikace prostřednictvím externí URL. Pokud se nezdařila jednotné přihlašování, měla zobrazit zpráva zakázané chyby v prohlížeči hello plus událost 13022 v protokolu hello:

    *Microsoft AAD Application Proxy Connector se nemůže ověřit uživatele hello, protože hello back-end server odpoví tooKerberos pokusy o přihlášení s chybou HTTP 401.*

    ![Je zakázané chyba 401 HTTTP](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

-   Zkontrolujte hello IIS aplikace tooensure hello nakonfigurovaný fond aplikací je nakonfigurovaná toouse hello stejný účet, který hello SPN nakonfiguroval proti ve službě AD: navigace ve službě IIS, jak je uvedeno dále

    ![Okno Konfigurace aplikace IIS](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

    Znáte hello identity vydejte následující hello z se, že tento účet jednoznačně nastavena hello SPN dotyčném vyzvat toomake cmd. Například **setspn – q http/spn.wacketywack.com**

    ![SetSPN příkazové okno](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

-   Zkontrolujte, že hello SPN definované vzhledem aplikace hello nastavení portálu hello je hello stejný hlavní název služby, který je nakonfigurovaný s hello cílový AD účet používá fond aplikací dané aplikace hello

   ![Hlavní název služby konfigurací na portálu Azure](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
-   Přejděte do služby IIS a vyberte hello **Editor konfigurací** možnost hello aplikace a přejděte příliš**system.webServer/security/authentication/windowsAuthentication** toomake se, že **UseAppPoolCredentials** nastavena tootrue

   ![Fondy aplikací IIS konfigurace přihlašovacích údajů – možnost](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

Aniž by byly užitečné v zlepšení výkonu hello operací protokolu Kerberos, a režimu jádra povolena také způsobí, že hello lístku hello požadovaná služba toobe dešifrovat pomocí účtu počítače. To se označuje taky jako místní systém hello, takže nutnosti přerušení tootrue tuto sadu použitím KCD, když je aplikace hello hostované na více serverech ve farmě.

-   Jako další kontrolu, můžete také toodisable hello **rozšířené** ochrany příliš. Byly došlo k scénáře, kde to se ukázalo jako toobreak použitím KCD při povolené ve velmi specifickém konfigurace, kde je aplikace publikována jako dílčí složku hello výchozí webové stránky. Tato samotné je nakonfigurován pro pouze anonymní ověřování ponechat hello celý dialogová okna šedá, které naznačují, že podřízené objekty nebude dědí všechny aktivní nastavení. Ale kde to možné, bude vždy doporučujeme mít tuto funkci povolíte, proto by nestálo otestovat, ale nezapomeňte toorestore tento tooenabled.

Tyto další kontroly by měl mít umístí můžete sledovat toostart pomocí k publikované aplikaci. Můžete přejít k tématu a typu číselník až další konektory, které jsou také nakonfigurována toodelegate, ale pokud žádné další věci pak by doporučujeme číst z našich podrobnější návod technické [hello kompletní Příručka pro řešení potíží s Azure AD Application Proxy server](https://aka.ms/proxytshootpaper)

Pokud jste stále nelze tooprogress problém, podporu by být vyšší než radostí tooassist a pokračovat z tohoto umístění. Vytvořit lístek podpory přímo v rámci portálu hello a naše technici bude oslovení tooyou.

## <a name="other-scenarios"></a>Další scénáře

-   Proxy aplikace služby Azure vyžádá lístek protokolu Kerberos před odesláním jeho žádost tooan aplikace. Některé 3. stran aplikace například Tableau Server nelíbí tuto metodu ověřování a místo očekává více konvenční hello místní tootake jednání,. první požadavek Hello je anonymní, což toorespond aplikace hello s hello typy ověřování, které podporuje prostřednictvím 401.

-   Double směrování ověřování – běžně používá ve scénářích, kde vrstvené aplikace s back-end a před koncem, oba vyžadují ověřování, jako je služba SQL Reporting Services.

## <a name="next-steps"></a>Další kroky
[Nakonfigurujte omezené delegování protokolu kerberos (použitím KCD) na spravované doméně](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-enable-kcd)
