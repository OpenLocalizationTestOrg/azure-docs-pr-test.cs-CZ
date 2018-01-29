---
title: "Řešení potíží s dvoustupňové ověřování | Microsoft Docs"
description: "Tento dokument se poskytují uživatelům informace o co dělat, pokud se problém se službou Azure Multi-Factor Authentication."
services: multi-factor-authentication
keywords: "vícefaktorové ověřování klienta, problém s ověřováním, ID korelace"
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: barlan
ms.reviewer: richagi
ms.custom: end-user
ms.openlocfilehash: 20a90aa36b727b18fb37aaf658da884b5997cd44
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="get-help-with-two-step-verification"></a>Získat pomoc s dvoustupňové ověření
Tento článek obsahuje odpovědi na nejčastější otázky, které uživatelé požádat o dvoustupňovém ověřování. 

## <a name="why-do-i-have-to-perform-two-step-verification-can-i-turn-it-off"></a>Proč musím provádět dvoustupňové ověřování? Můžete vypnout jeho nastavení?

Dvoustupňové ověření je funkce zabezpečení, která organizaci rozhodli používat k ochraně svých účtů. Je bezpečnější než pouhým heslem, protože závisí na dvě formy ověřování: něco znáte a něco s vámi máte. Něco, co víte, je jím vaše heslo. Je něco, co máte s vámi telefonu nebo zařízení, které obvykle mají s vámi. Pokud váš účet je chráněný pomocí dvoustupňové ověření, kyberzločinci nemůžete se přihlásit jako vám i v případě, že získají heslo. Se nelze přihlásit, protože nemají přístup k telefonu. 

Společnost Microsoft nabízí dvoustupňové ověření, ale vaše organizace rozhodne použít funkci. Nelze chodit Pokud podpoře společnosti vyžaduje, aby se z vás, stejně jako nemůže vyjádření výslovného nesouhlasu s heslem abyste ochránili svůj účet. 

Pokud máte dvoustupňové ověření zapnuté pro svůj osobní účet Microsoft a chcete-li změnit nastavení, přečtěte si [o dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) místo. 

## <a name="i-dont-have-my-phone-with-me-today"></a>Není k dispozici telefon se mi ještě dnes

Některé dnů, po který necháte telefonu v domácnostech, ale pořád je potřeba se přihlásit v práci. Je první věcí, kterou byste měli přihlášení pomocí různých ověření metoda. Při registraci pro dvoustupňové ověření, se můžete nastavit více než jeden telefonní číslo? Pokud chcete vyzkoušet, přihlášení pomocí jinou metodu, postupujte takto:

1. Přihlaste se jako obvykle.
2. Po otevření stránky dvoustupňové ověření, vyberte **použít jinou možností ověření**.

   ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Vyberte možnost ověření, kterou chcete použít. 
  - Pokud nemáte přístup k vaší alternativní metody buď, obraťte se na vaše společnost podporovat získat pomoc s přihlášením k vašemu účtu.
  - Pokud máte přístup k vaší alternativní metody, pokračujte dvoustupňové ověřování.

Pokud nevidíte **použít jinou možností ověření** odkaz, pak to znamená, že jste nenastavili alternativní metody při první registraci pro dvoustupňové ověření. Obraťte se na podporu společnosti získat pomoc s přihlášením k vašemu účtu. Jakmile jste přihlášení, nezapomeňte [spravovat nastavení](multi-factor-authentication-end-user-manage-settings.md) přidat další ověřovací metody pro další použití. 

## <a name="i-lost-my-phone-or-got-a-new-number"></a>I ztratíte telefon nebo získali nové číslo
Existují dva způsoby dostat se zpátky do vašeho účtu. První je se přihlásit pomocí vašeho alternativní číslo telefonu pro ověření, pokud jste nastavili jednu. Druhá je požádejte podporu společnosti vymazat nastavení.

Pokud váš telefon byl ztráty nebo odcizení, doporučujeme také, že se zjistit podpory vaší společnosti. Budete potřebovat resetovat hesla aplikace a zrušte všechny zapamatovaných zařízení. 

### <a name="use-an-alternate-phone-number"></a>Použít alternativní telefonní číslo
Pokud nastavíte více možností ověřování, jako jsou záložní telefonní číslo nebo ověřovací aplikace na jiném zařízení, použijte jednu z nich k přihlášení.

K přihlášení pomocí alternativní telefonní číslo, postupujte takto:

1. Přihlaste se jako obvykle.
2. Po zobrazení výzvy provést ještě další ověření vašeho účtu, vyberte **použít jinou možností ověření**.
   
   ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Vyberte telefonní číslo nebo zařízení, které máte přístup.
4. Poté, co si zpět ve vašem účtu [spravovat nastavení](multi-factor-authentication-end-user-manage-settings.md) Chcete-li změnit číslo telefonu pro ověření.

### <a name="clear-your-settings"></a>Clear – nastavení
Pokud jste nenakonfigurovali telefonní číslo sekundárního ověřování, budete muset požádejte o pomoc podporu společnosti. Vymazat je mají nastavení, při příštím přihlášení, zobrazí se výzva k [zaregistrovat pro dvoustupňové ověření](multi-factor-authentication-end-user-first-time.md) znovu.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>I doručována text nebo volání na telefon
Tady je několik důvodů, proč můžete se pokusit přihlásit, ale není zobrazí text nebo telefonní hovor. Pokud jste úspěšně texty nebo obdrželi telefonní hovory na váš telefon v minulosti, pak tento problém se pravděpodobně problém s poskytovateli telefonní není váš účet. Ujistěte se, že máte signál dobrý buňky. A pokud se pokoušíte zaslána textová zpráva, ujistěte se, že budete moci přijímat textové zprávy. Požádejte friend volat jste nebo text jste jako testu. 

Pokud jste čekali několik minut, než text nebo volání, nejrychlejší způsob, jak dostat do vašeho účtu je zkuste jinou možnost.

1. Vyberte **použít jinou možností ověření** na stránku, která čeká na ověření.
   
    ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Vyberte telefonní číslo nebo doručení metodu, kterou chcete použít.
   
    Pokud jste dostali více ověřovací kódy, použijte nejnovější.

Pokud nemáte nakonfigurován jinou metodu, obraťte se na podporu společnosti a požádejte je o smazat nastavení. Při příštím přihlášení, zobrazí se výzva k [nastavení služby Multi-Factor authentication](multi-factor-authentication-end-user-first-time.md) znovu.

Pokud máte často zpoždění z důvodu chybné buňky signál, doporučujeme použít [aplikaci Microsoft Authenticator](microsoft-authenticator-app-how-to.md) na vašem smartphonu. Aplikace může generovat náhodné zabezpečovací kódy, které používáte k přihlášení a tyto kódy nevyžadují žádné buňky signál nebo připojení k Internetu.

## <a name="app-passwords-are-not-working"></a>Hesla aplikací nejsou práce
Zkontrolujte, zda jste správně zadali heslo aplikace. Heslo generovaného aplikace nahrazuje normální heslo, ale jenom pro starší aplikací klasické pracovní plochy, které nepodporují dvoustupňové ověřování. Pokud stále nefunguje, zkuste přihlášení a [vytvořit nové heslo aplikace](multi-factor-authentication-end-user-app-passwords.md).  Pokud stále nepomůže, obraťte se na podporu společnosti a potom kliknul [odstranit vašich dosavadních hesel aplikací](../multi-factor-authentication-manage-users-and-devices.md) a vytvořte novou.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Odpověď na Můj problém I nebyl nalezen.
Pokud jste se pokusili postup řešení, ale jsou stále spuštěná na problémy, obraťte se na podporu vaší společnosti. Jejich by mohli pomoct.

## <a name="related-topics"></a>Související témata
* [Spravovat nastavení pro dvoustupňové ověření](multi-factor-authentication-end-user-manage-settings.md)  
* [Nejčastější dotazy k aplikaci Microsoft Authenticator](microsoft-authenticator-app-faq.md)

