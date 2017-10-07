---
title: "AAA neočekávané chybě při provádění souhlasu tooan aplikace | Microsoft Docs"
description: "Popisuje chyby, které se můžou vyskytnout během procesu hello souhlas tooan aplikace a co můžete dělat o nich"
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
ms.openlocfilehash: dfee35f3a10e3cc4313109cedd972499452320f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-error-when-performing-consent-tooan-application"></a>Neočekávaná chyba při provádění souhlasu tooan aplikace

Tento článek popisuje chyby, které se můžou vyskytnout během procesu hello souhlas tooan aplikace. Pokud Poradce při potížích neočekávané souhlasu vyzve není obsahovat chybové zprávy naleznete na adrese [scénáře ověřování pro Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Mnoho aplikací, které se integrují s Azure Active Directory vyžadují oprávnění tooaccess další prostředky ve toofunction pořadí. Když tyto prostředky jsou také integrované s Azure Active Directory, je je často požadovaná pomocí hello běžné tooaccess oprávnění souhlas framework. 

Výsledkem výzva k povolení spuštění se zobrazuje, který obvykle probíhá hello poprvé, aplikace se používá, ale může dojít také při následné použití aplikace hello.

Určitých podmínek musí být pro oprávnění toohello tooconsent uživatele, které aplikace vyžaduje na hodnotu true. Pokud nejsou tyto podmínky splněny, může dojít k různých chybám. Mezi ně patří:

## <a name="requesting-not-authorized-permissions-error"></a>Požaduje chybě není autorizovaný oprávnění
* **AADSTS90093:** &lt;clientAppDisplayName&gt; požaduje jeden nebo více oprávnění, která není autorizovaný toogrant. Obraťte se na správce, který může souhlas toothis aplikací vaším jménem.

K této chybě dojde, když se uživatel, který není správcem společnosti pokusí toouse aplikace, která vyžaduje oprávnění, které můžete udělit pouze správce. Tuto chybu můžete vyřešit správcem udělení přístupu aplikace toohello jménem své organizace.

## <a name="policy-prevents-granting-permissions-error"></a>Zásada brání udělení oprávnění chyby
* **AADSTS90093:** správce &lt;tenantDisplayName&gt; nastavil zásadu, která brání udělení &lt;název aplikace&gt; hello vyžaduje oprávnění. Obraťte se na správce systému &lt;tenantDisplayName&gt;, který můžete udělit oprávnění aplikace toothis vaším jménem.

Tato chyba nastane, když správce společnosti vypne hello schopnost tooapplications tooconsent uživatele, pak pokusu toouse aplikaci, která vyžaduje souhlas uživatele bez oprávnění správce. Tuto chybu můžete vyřešit správcem udělení přístupu aplikace toohello jménem své organizace.

## <a name="intermittent-problem-error"></a>Chyba občasný problém
* **AADSTS90090:** vypadá došlo občasný problém záznam hello oprávnění, které jste se pokusili toogrant příliš&lt;clientAppDisplayName&gt;. Zkuste to znovu později.

Tato chyba znamená, že došlo k problému straně přerušované služby. Dají se vyřešit pokusem tooconsent toohello aplikaci znovu.

## <a name="resource-not-available-error"></a>Prostředek není k dispozici – chyba
* **AADSTS65005:** aplikace hello &lt;clientAppDisplayName&gt; požadovaná oprávnění tooaccess prostředek &lt;resourceAppDisplayName&gt; není k dispozici. 

Obraťte se na vývojáře aplikace hello.

##  <a name="resource-not-available-in-tenant-error"></a>Prostředek není k dispozici v Chyba klienta
* **AADSTS65005:** &lt;clientAppDisplayName&gt; požaduje přístup k prostředku tooa &lt;resourceAppDisplayName&gt; není k dispozici ve vaší organizaci &lt; tenantDisplayName&gt;. 

Ujistěte se, že tento prostředek je k dispozici nebo se obraťte na správce &lt;tenantDisplayName&gt;.

## <a name="permissions-mismatch-error"></a>Chybová zpráva o neshodě oprávnění
* **AADSTS65005:** aplikace hello požadovaný prostředek tooaccess souhlasu &lt;resourceAppDisplayName&gt;. Tento požadavek se nezdařil, protože neodpovídá jak byla aplikace hello předem nakonfigurovaná během registrace aplikací. Obraťte se na hello aplikace vendor.* *

Všechny tyto chyby nastane, když aplikace hello uživatel se pokouší tooconsent toois požaduje oprávnění tooaccess prostředků aplikace, který nebyl nalezen v adresáři organizace hello (klientů). Tato situace může nastat z několika důvodů:

-   Vývojář aplikace Hello klient má své aplikace nesprávně nakonfigurované, způsobuje ho toorequest přístup tooan neplatný prostředek. Vývojář aplikace hello v takovém případě musíte aktualizovat konfiguraci hello hello klienta aplikace tooresolve tento problém.

-   Instanční objekt reprezentující hello cílová prostředků aplikace v organizaci hello neexistuje nebo existovala v posledních hello ale byla odebrána. tooresolve-li tento problém, hlavní název služby pro hello prostředků aplikace musí být zřízená v organizaci hello, takže hello klientská aplikace může požádat o oprávnění tooit. To může dojít v počtu způsoby, v závislosti na typu hello aplikace, včetně:

    -   Získávání odběr pro hello prostředků aplikace (Microsoft publikované aplikace)

    -   Souhlas toohello prostředků aplikace

    -   Udělení oprávnění aplikací hello prostřednictvím hello portálu Azure

    -   Přidání aplikace hello z hello Azure AD Application Gallery

## <a name="next-steps"></a>Další kroky 

[Aplikace, oprávnění a souhlasu v Azure Active Directory (koncový bod v1)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Obory, oprávnění a souhlasu v hello Azure Active Directory (koncový bod v2.0)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


