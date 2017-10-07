---
title: "AAA \"nelze získat přístup k této chybě podnikové aplikace při použití aplikace Proxy aplikace | Microsoft Docs\""
description: "Jak běžné přístup tooresolve problémy s aplikací Azure AD Application Proxy."
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
ms.openlocfilehash: 490b106b7d774ee43fc076cc5d082997a1df85e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>Chyba "Nelze podnikové k této aplikaci přístup" při použití aplikace Proxy aplikace

Tento článek vám pomohou tootroubleshoot běžné problémy potýkají až uvidíte chybu "Tato podnikové aplikace nelze získat přístup" v aplikaci Azure AD Application Proxy.

## <a name="overview"></a>Přehled
Když se zobrazí tato chyba, stránku hello také sdílet stavový kód. Tento kód je jedním z následujících hello:

-   **Vypršel časový limit brány**: hello Proxy aplikace služby je nelze tooreach hello konektor. To obvykle znamená potíže s přiřazení hello konektoru, konektor sám nebo hello sítě pravidla kolem hello konektor.

-   **Chybná brána**: hello konektor je nelze tooreach hello back-end aplikace. To může znamenat nesprávnou konfiguraci aplikace hello.

-   **Je zakázané**: hello uživatel není autorizovaný tooaccess hello aplikace. Tomu může dojít, když uživatel hello není přiřazen toohello aplikace v Azure Active Directory nebo pokud na back-end hello hello uživatel nemá oprávnění tooaccess hello aplikace.

toofind hello kód, podívejte se na text hello v hello spodní levé části hello chybovou zprávu pro pole "Stavový kód" hello. Také vyhledejte případné poznámky v hello velmi dolní části stránky hello s další typy.

   ![Chyba vypršení časového limitu brány](./media/application-proxy/connection-problem.png)

Podrobnosti o tom, jak tootroubleshoot hello hlavní příčinu tyto chyby a další podrobnosti o navrhované opravy najdete v části hello odpovídající části.

## <a name="gateway-timeout-errors"></a>Chyby vypršení časového limitu brány

Časovému limitu brány nastane, když služba hello pokusí tooreach hello konektoru a nelze toowithin hello časový limit okno. To je obvykle způsobena aplikace přiřazené tooa konektor skupiny s konektory žádné pracovní nebo některé porty vyžadované službou hello konektor nejsou otevřené.


## <a name="bad-gateway-errors"></a>Chybný chyby brány

Chybná brána chyba znamená, že konektor hello je nelze tooreach hello back-end aplikace. Ujistěte se, že jste publikovali hello správnou aplikaci. Běžné chyby, které tato chyba způsobit:

-   Máte překlep nebo chybu v interní adresa URL hello

-   Není publikování hello kořenové aplikace hello. Například publikování <http://expenses/reimbursement> , ale při tooaccess <http://expenses>

-   Problémy s konfigurací protokolu Kerberos vynuceným delegování použitím (KCD) hello

-   Problémy s back-end aplikace hello

## <a name="forbidden-errors"></a>Zakázané chyby

Pokud se zobrazí chyba zakázané, hello uživateli nebyla přiřazena toohello aplikace. To může být buď v Azure Active Directory nebo na back-end aplikace hello.

toolearn jak tooassign uživatelé toohello aplikaci v Azure, najdete v části hello [konfigurace dokumentaci](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Pokud potvrdíte, že uživatel hello je přiřazen toohello aplikaci v Azure, zkontrolujte konfiguraci uživatele hello v back-end aplikace hello. Pokud používáte ověřování systému Windows omezeného delegování nebo integrovaný protokolu Kerberos, se zobrazí stránka našeho řešení potíží s použitím KCD některé pokyny.

## <a name="check-hello-applications-internal-url"></a>Zkontrolujte interní adresa URL aplikace hello

Jako první krok rychlý, Překontrolujte kontrola a oprava interní adresa URL hello otevřením aplikace hello prostřednictvím **podnikové aplikace, které**, pak výběrem hello **Proxy aplikace** nabídky. Ověřte, že toto je hello správnou interní adresu URL, hello používá z vaší místní síti tooaccess hello aplikace.

## <a name="check-hello-application-is-assigned-tooa-working-connector-group"></a>Zkontrolujte, že aplikace hello je přiřazenou tooa práce konektor skupiny

aplikace hello tooverify přiřazen tooa práce konektor skupiny:

1.  Otevřete aplikaci hello portálu hello přechodem příliš**Azure Active Directory**, kliknutím na na **podnikové aplikace, které**, pak **všechny aplikace.** Otevřete aplikaci hello a pak vyberte **Proxy aplikace** hello levé nabídce.

2.  Podívejte se na pole skupiny konektor hello. Pokud skupina hello neobsahuje žádné aktivní konektory, zobrazí se upozornění. Pokud nevidíte žádné upozornění, přesuňte příliš "ověřit všechny požadované porty jsou seznam povolených adres".

3.  Pokud je to hello nesprávné skupině konektor pomocí hello rozevíracího seznamu správný skupiny hello tooselect a potvrďte, že už se nezobrazují žádné upozornění. Pokud je to hello určena skupiny konektor, klikněte na tlačítko hello hello stránka tooopen zprávy upozornění se správou konektor.

4.  Tady je několik způsobů toodrill v další:

  * Konektor služby active přesunout do skupiny hello: Pokud máte active konektor, který by měly patřit toothis skupiny a má směrem pohledu toohello cílová back-end aplikace, hello konektoru můžete přesunout do skupiny hello přiřazen. toodo proto, klikněte na hello konektor. V poli "Konektor skupinu" hello použijte správnou skupinu hello hello tooselect rozevíracího seznamu a klikněte na Uložit.

  * Stáhněte si nový konektor pro tuto skupinu: na této stránce můžete získat odkaz hello příliš[stáhnout nový konektor](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). hello Hello konektor potřebám toobe nainstalovaná na počítači s přímé směrem pohledu toohello back-end aplikace a je obvykle umístěny na stejném serveru jako aplikace hello. Použití hello stáhnout konektor odkaz toodownload konektor na hello cílový počítač. V dalším kroku klikněte hello konektoru a použijte toomake rozevíracího seznamu "Konektor skupinu" hello se, že patří toohello správné skupiny.

  * Prozkoumat neaktivní konektor: Pokud se konektor zobrazovat jako neaktivní, je nelze tooreach hello služby. To je obvykle kvůli toosome požadované porty blokován. toosolve tento problém, přesuňte na příliš "ověřit všechny požadované porty jsou seznam povolených adres".

Po dokončení těchto kroků tooensure hello aplikace je skupina přiřazené tooa s práce konektory, testovací hello aplikaci znovu. Pokud ještě není funkční, pokračujte v další části toohello.

## <a name="check-all-required-ports-are-whitelisted"></a>Zkontrolujte všechny požadované porty jsou seznam povolených adres

tooverify všechny požadované porty jsou otevřená, dokumentaci k naší na Otevřít porty. Pokud všechny hello jsou otevřené požadované porty, přesuňte toohello další části.

## <a name="check-for-other-connector-errors"></a>Zkontrolujte další chyby konektoru

Pokud žádná z výše uvedených hello hello problém vyřešit, je dalším krokem hello toolook problémy a chyby s hello konektor sám. Zobrazí se některé běžné chyby v hello [Poradce při potížích dokumentu](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). 

Můžete také zobrazit přímo na hello konektor protokoly tooidentify případné chyby. Mnoho z našich chybových zpráv být schopný tooshare více konkrétní doporučení pro opravy. jak zjistit, protokoly hello tooview toolearn [dokumentaci konektory](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="additional-resolutions"></a>Další řešení

Pokud hello výše nebyla hello problém odstranit, existuje několik různých možných příčin. problém tooidentify hello:

Pokud je vaše aplikace nakonfigurovaná toouse integrované ověřování systému Windows (IWA), testovací aplikace hello bez jednotného přihlašování. Pokud ne, přesuňte toohello další odstavce. aplikace hello toocheck bez jednotného přihlašování, otevřete aplikaci prostřednictvím **podnikové aplikace,** a přejděte toohello **jednotné přihlašování** nabídky. Změna hello rozevírací nabídku od "Integrované ověřování systému Windows" příliš "Azure AD jednotné přihlašování zakázáno". 

Nyní otevřete prohlížeč a zkuste to tooaccess hello aplikaci znovu. Měla zobrazit výzva k ověřování a získání do aplikace hello. Pokud toto funguje, je ale hello problém s konfigurací hello protokolu Kerberos vynuceným delegování použitím (KCD), která umožňuje hello jednotné přihlašování. najdete v části řešení potíží s použitím KCD stránku hello.

Pokud budete pokračovat toosee hello chyby, přejděte toohello počítače, kde hello konektor je nainstalována, otevřete prohlížeč a pokus o tooreach hello interní adresa URL použitá pro aplikaci hello. Hello konektor funguje jako jiného klienta z hello stejný počítač. Pokud nebudou moci připojit aplikace hello, zjistěte, proč tento počítač je aplikace hello nelze tooreach nebo použít konektor na serveru, který je možné tooaccess hello aplikace.

Pokud aplikace hello z tohoto počítače, toolook problémy a chyby s hello konektor sám může dosáhnout. Zobrazí se některé běžné chyby v hello [Poradce při potížích dokumentu](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). Můžete také zobrazit přímo na hello konektor protokoly tooidentify případné chyby. Mnoho z našich chybových zpráv být schopný tooshare více konkrétní doporučení pro opravy. jak zjistit, protokoly hello tooview toolearn [dokumentaci konektory](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="next-steps"></a>Další kroky
[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)
