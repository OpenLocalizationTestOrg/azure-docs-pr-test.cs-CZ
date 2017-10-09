---
title: "Azure AD Connect: Předávací ověřování – agenty ověřování upgradu preview | Microsoft Docs"
description: "Tento článek popisuje, jak tooupgrade konfiguraci předávací ověřování Azure Active Directory (Azure AD)."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 1695326b182278944e9f1584da72b18862b6ef27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure předávací ověřování služby Active Directory: Agenti ověřování upgradu preview

## <a name="overview"></a>Přehled

Tento článek je pro zákazníky používající Azure AD předávací ověřování prostřednictvím preview. Jsme nedávno aktualizovaného (a přejmenované) hello software agenta ověřování. Je nutné too_manually_ upgradu preview ověřování agentů nainstalovaných na místní servery. Tento ruční upgrade je jednorázová akce. Všechny budoucí aktualizace tooAuthentication agenti jsou automatické. Hello důvodů tooupgrade jsou následující:

- verze preview Hello ověřování agentů nebude přijímat žádné další zabezpečení nebo opravy chyb.
-   verze preview Hello ověřování agentů nelze nainstalovat na další servery, pro zajištění vysoké dostupnosti.

## <a name="check-versions-of-your-authentication-agents"></a>Kontrola verze agentů ověřování

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>Krok 1: Zkontrolujte, kde jsou nainstalovány agenty ověřování

Postupujte podle těchto kroků toocheck nainstalovanou agenty ověřování:

1. Přihlaste se toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) s přihlašovacími údaji hello globální správce pro vašeho klienta.
2. Vyberte **Azure Active Directory** na levé straně hello.
3. Vyberte **Azure AD Connect**. 
4. Vyberte **předávací ověřování**. Toto okno obsahuje seznam serverů hello nainstalovanou agenty ověřování.

![Azure Active Directory Centrum pro správu – okno předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-hello-versions-of-your-authentication-agents"></a>Krok 2: Kontrola hello verze agentů ověřování

verze hello toocheck ověřování agentů, na každém serveru v předchozím kroku, hello identifikuje postupujte podle těchto pokynů:

1. Přejděte příliš**ovládací panely -> programy -> programy a funkce** hello na místní server.
2. Pokud je položka pro "**agenta služby Microsoft Azure AD Connect ověřování**", tootake není třeba žádné akce na tomto serveru.
3. Pokud je položka pro "**Microsoft Azure AD Application Proxy Connector**", verze 1.5.132.0 nebo starší, musíte toomanually upgradu na tomto serveru.

![Verze Preview ověřování agenta](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-toofollow-before-starting-hello-upgrade"></a>A osvědčených postupů toofollow před zahájením upgradu hello

Před upgradem, ujistěte se, že máte následující položky jsou zavedené hello:

1. **Vytvořit účet globálního správce jenom pro cloud**: neupgradujete bez nutnosti jenom pro cloud toouse účet globálního správce v případě nouze kde agenty předávací ověřování nefungují správně. Další informace o [přidání účtu globálního správce jenom pro cloud](../active-directory-users-create-azure-portal.md). Tento krok je velmi důležité a zajistí, že nezůstanete z vašeho klienta.
2.  **Zajištění vysoké dostupnosti**: Pokud není dokončen dříve, nainstalovat samostatnou druhý ověřovací Agent tooprovide vysoká dostupnost pro žádostí o přihlášení, použití těchto [pokyny](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="upgrading-hello-authentication-agent-on-your-azure-ad-connect-server"></a>Upgrade hello Agent ověřování na serveru Azure AD Connect

Azure AD Connect nutné upgradovat před upgradem hello Agent ověřování na hello stejný server. Na hlavním i pracovní servery Azure AD Connect, postupujte takto:

1. **Upgrade Azure AD Connect**: následovat po této [článku](./active-directory-aadconnect-upgrade-previous-version.md) a upgrade verze toohello nejnovější Azure AD Connect.
2. **Odinstalujte verzi preview hello hello Agent ověřování**: stažení [tento skript prostředí PowerShell](https://aka.ms/rmpreviewagent) a potom ho spusťte jako správce na serveru hello.
3. **Stáhněte nejnovější verzi hello Agent ověřování hello (verze 1.5.193.0 nebo novější)**: Přihlaste toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí přihlašovacích údajů globálního správce vašeho klienta. Vyberte **Azure Active Directory -> Azure AD Connect -> předávací ověřování -> stáhnout agenta**. Přijměte hello podmínky služby a stáhnout nejnovější verzi hello Agent ověřování hello.
4. **Nainstalujte nejnovější verzi hello Agent ověřování hello**: hello spustit spustitelný soubor stáhli v kroku 3. Zadejte pověření pro globálního správce vašeho klienta po zobrazení výzvy.
5. **Ověřte, že nejnovější verzi hello byly nainstalovány.**: jak je znázorněno před, přejděte příliš**ovládací panely -> programy -> programy a funkce** a ověřte, zda je položka pro "**Microsoft Azure AD Connect Agent ověřování**".

>[!NOTE]
>Pokud zaškrtnete hello předávací ověřování okně hello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) po dokončení předchozích kroků hello, uvidíte dvě položky ověřování agenta na server - jednu položku zobrazující hello Agent ověřování jako **Active** a další jako hello **neaktivní**. Toto je _očekává_. Hello **neaktivní** položku automaticky vyřazen po několik dní.

## <a name="upgrading-hello-authentication-agent-on-other-servers"></a>Upgrade hello Agent ověřování na jiných serverech

Postupujte podle těchto kroků tooupgrade ověřování agentů na jiných serverech (kde Azure AD Connect není nainstalovaná):

1. **Odinstalujte verzi preview hello hello Agent ověřování**: stažení [tento skript prostředí PowerShell](https://aka.ms/rmpreviewagent) a potom ho spusťte jako správce na serveru hello.
2. **Stáhněte nejnovější verzi hello Agent ověřování hello (verze 1.5.193.0 nebo novější)**: Přihlaste toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí přihlašovacích údajů globálního správce vašeho klienta. Vyberte **Azure Active Directory -> Azure AD Connect -> předávací ověřování -> stáhnout agenta**. Přijměte podmínky hello služby a stáhnout nejnovější verzi hello.
3. **Nainstalujte nejnovější verzi hello Agent ověřování hello**: hello spustit spustitelný soubor stáhli v kroku 2. Zadejte pověření pro globálního správce vašeho klienta po zobrazení výzvy.
4. **Ověřte, že nejnovější verzi hello byly nainstalovány.**: jak je znázorněno před, přejděte příliš**ovládací panely -> programy -> programy a funkce** a ověřte, zda je volána položku **Microsoft Azure AD Connect Agent ověřování**.

>[!NOTE]
>Pokud zaškrtnete hello předávací ověřování okně hello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) po dokončení předchozích kroků hello, uvidíte dvě položky ověřování agenta na server - jednu položku zobrazující hello Agent ověřování jako **Active** a další jako hello **neaktivní**. Toto je _očekává_. Hello **neaktivní** položku automaticky vyřazen po několik dní.

## <a name="next-steps"></a>Další kroky
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
