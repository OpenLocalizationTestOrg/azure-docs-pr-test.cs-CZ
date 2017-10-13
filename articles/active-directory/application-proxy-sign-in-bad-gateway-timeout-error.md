---
title: "Nelze získat přístup k této chybě podnikové aplikace při použití aplikace Proxy aplikace | Microsoft Docs"
description: "Jak řešit obvyklé problémy přístup s aplikací Azure AD Application Proxy."
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
ms.openlocfilehash: 78ff8763a461162cbcfa04c6a86123973271928a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>Chyba "Nelze podnikové k této aplikaci přístup" při použití aplikace Proxy aplikace

Tento článek pomoci při řešení běžných problémů s potýkají až uvidíte chybu "Tato podnikové aplikace nelze získat přístup" v aplikaci Azure AD Application Proxy.

## <a name="overview"></a>Přehled
Když se zobrazí tato chyba, stránky také sdílet stavový kód. Tento kód je jedním z následujících akcí:

-   **Vypršel časový limit brány**: Proxy aplikace služby je možné vás zastihnout konektor. To obvykle znamená potíže s přiřazením konektoru, konektor sám, nebo sítě pravidla kolem konektor.

-   **Chybná brána**: konektor se nelze spojit s back-end aplikace. To může znamenat nesprávnou konfiguraci aplikace.

-   **Je zakázané**: uživatel nemá oprávnění pro přístup k aplikaci. Tato situace může nastat, když uživatel není přiřazen k aplikaci v Azure Active Directory nebo pokud na back-end uživatel nemá oprávnění pro přístup k aplikaci.

Pokud chcete najít kód, podívejte se na text v levém dolním chybové zprávy pro pole "Kód stavu". Také vyhledejte případné poznámky v velmi dolní části stránky s další typy.

   ![Chyba vypršení časového limitu brány](./media/application-proxy/connection-problem.png)

Podrobnosti o tom, jak vyřešit hlavní příčinu tyto chyby a další podrobnosti o navrhované opravy najdete v části odpovídající níže.

## <a name="gateway-timeout-errors"></a>Chyby vypršení časového limitu brány

Vypršel časový limit brány dojde, pokud služba se pokusí kontaktovat konektor a nemůže časovém limitu. To je obvykle způsobena aplikace přiřazené ke skupině konektoru se žádný pracovní konektory nebo některé porty vyžadované službou konektoru nejsou otevřené.


## <a name="bad-gateway-errors"></a>Chybný chyby brány

Chybná brána chyby označuje, že konektor je možné vás zastihnout na back-end aplikace. Ujistěte se, že jste publikovali správnou aplikaci. Běžné chyby, které tato chyba způsobit:

-   Máte překlep nebo chybu v interní adresa URL

-   Publikování není kořenové aplikace. Například publikování <http://expenses/reimbursement> , ale při pokusu o přístup <http://expenses>

-   Problémy s konfigurací protokolu Kerberos vynuceným delegování použitím (KCD)

-   Problémy s back-end aplikace

## <a name="forbidden-errors"></a>Zakázané chyby

Pokud se zobrazí chyba zakázané, uživateli nebyla přiřazena k aplikaci. To může být buď v Azure Active Directory nebo na back-end aplikace.

Naučte se přiřazovat uživatele k aplikaci v Azure, najdete v článku [konfigurace dokumentaci](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Pokud můžete potvrdit, že uživatel je přiřazen k aplikaci v Azure, zkontrolujte konfiguraci uživatele v aplikaci back-end. Pokud používáte ověřování systému Windows omezeného delegování nebo integrovaný protokolu Kerberos, se zobrazí stránka našeho řešení potíží s použitím KCD některé pokyny.

## <a name="check-the-applications-internal-url"></a>Zkontrolujte interní adresa URL aplikace.

Jako první krok rychlý, Překontrolujte kontrola a oprava interní adresa URL otevřením aplikace prostřednictvím **podnikové aplikace, které**, pak výběrem **Proxy aplikace** nabídky. Ověřte, že toto je správná interní adresa URL, použili z vaší místní sítě pro přístup k aplikaci.

## <a name="check-the-application-is-assigned-to-a-working-connector-group"></a>Zkontrolujte, že aplikace je přiřazenou funkční konektor skupiny

K ověření aplikace je přiřazen k funkční konektor skupiny:

1.  Otevřete aplikaci na portálu přejděte na **Azure Active Directory**, kliknutím na na **podnikové aplikace, které**, pak **všechny aplikace.** Otevřete aplikaci a pak vyberte **Proxy aplikace** v levé nabídce.

2.  Podívejte se na pole konektor skupiny. Pokud nejsou žádné aktivní konektory ve skupině, zobrazí se upozornění. Pokud nevidíte žádné upozornění, přejděte na "ověřit všechny požadované porty jsou seznam povolených adres".

3.  Pokud je to nesprávné skupině konektor, použijte rozevírací dolů vyberte správné skupině a potvrďte, že už se nezobrazují žádné upozornění. Pokud je zamýšlené skupiny konektor, klikněte na tlačítko upozornění otevřete stránku s konektoru správy.

4.  Tady je několik způsobů, jak přejít k podrobnostem Další:

  * Přesunout do skupiny pro konektor služby active: Pokud máte aktivní konektor, musí patřit do této skupiny a má směrem pohledu do cílové aplikace back-end, konektor můžete přesunout do skupiny přiřazené. Chcete-li to provést, klikněte na tlačítko konektor. V poli "Konektor skupina" pomocí rozevíracího seznamu vyberte správné skupině, a klikněte na Uložit.

  * Stáhněte si nový konektor pro tuto skupinu: na této stránce můžete získat odkaz na [stáhnout nový konektor](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). Konektor musí být nainstalovaný na počítači s přímé směrem pohledu na back-end aplikace a je obvykle umístěny na stejném serveru jako aplikace. Použijte stahování konektoru odkaz ke stažení konektoru na cílovém počítači. V dalším kroku klikněte na konektor a ujistěte se, že patří do správné skupiny pomocí "Konektor skupinu" rozevíracího seznamu.

  * Prozkoumat neaktivní konektor: Pokud se konektor zobrazovat jako neaktivní, je možné vás zastihnout na službu. Toto je obvykle kvůli některé požadované porty blokován. Pokud chcete tento problém vyřešit, přesuňte na "ověřit všechny požadované porty jsou seznam povolených adres".

Po použití těchto kroků zkontrolujte, že aplikace je přiřadit ke skupině se práce konektory, testovat aplikaci znovu. Pokud ještě není funkční, pokračujte v další části.

## <a name="check-all-required-ports-are-whitelisted"></a>Zkontrolujte všechny požadované porty jsou seznam povolených adres

Pokud chcete ověřit, že všechny požadované, jsou otevřené porty, najdete v dokumentaci na Otevřít porty. Pokud jsou otevřené požadované porty, přejděte na další části.

## <a name="check-for-other-connector-errors"></a>Zkontrolujte další chyby konektoru

Pokud žádná z výše uvedeného problém vyřešit, je dalším krokem hledání problémy a chyby s konektor sám. Zobrazí se některé běžné chyby v [Poradce při potížích dokumentu](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). 

Můžete také zobrazit přímo v protokolech konektoru k identifikaci případné chyby. Mnoho z našich chybových zpráv moci sdílet více konkrétní doporučení pro opravy. Další postupy k zobrazení protokolů najdete v tématu [dokumentaci konektory](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="additional-resolutions"></a>Další řešení

Pokud se výše nepomohly problém vyřešit, existuje několik různých důvodů. Chcete-li identifikovat problém:

Pokud vaše aplikace je nakonfigurovaný na použití integrovaného ověřování systému Windows (IWA), testování aplikace bez jednotného přihlašování. Pokud ne, přechod na další odstavec. Chcete-li zkontrolovat aplikaci bez jednotného přihlašování, otevřete aplikaci prostřednictvím **podnikové aplikace,** a přejděte na **jednotné přihlašování** nabídky. Změňte rozevíracího z "Integrované ověřování systému Windows" na "Azure AD jednotné přihlašování zakázáno". 

Nyní otevřete prohlížeč a snaží o přístup k aplikaci znovu. Měla zobrazit výzva k ověřování a dostat se do aplikace. Pokud toto funguje, je ale problém s konfigurací protokolu Kerberos vynuceným delegování použitím (KCD), která umožňuje jednotné přihlašování. najdete v části řešení potíží s použitím KCD stránky.

Pokud budete pokračovat zobrazí chyba, přejděte k počítači, kde je konektor nainstalovaný, spusťte prohlížeč a se pokoušejí připojit interní adresa URL pro aplikaci. Konektor funguje jako jiného klienta ze stejného počítače. Pokud aplikace není možné dosáhnout, zjistěte, proč tento počítač se nepodařilo dosáhnout aplikace, nebo použít konektor na serveru, který se bude moct získat přístup k aplikaci.

Pokud chcete vyhledat potíže či chyby s konektor sám aplikace z počítače, může dosáhnout. Zobrazí se některé běžné chyby v [Poradce při potížích dokumentu](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). Můžete také zobrazit přímo v protokolech konektoru k identifikaci případné chyby. Mnoho z našich chybových zpráv moci sdílet více konkrétní doporučení pro opravy. Další postupy k zobrazení protokolů najdete v tématu [dokumentaci konektory](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="next-steps"></a>Další kroky
[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)
