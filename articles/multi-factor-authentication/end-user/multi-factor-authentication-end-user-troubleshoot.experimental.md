---
title: "aaaTroubleshoot dvoustupňové ověřování | Microsoft Docs"
description: "Tento dokument se poskytují uživatelům informace o jaké toodo Pokud se problém se službou Azure Multi-Factor Authentication."
services: multi-factor-authentication
keywords: "vícefaktorové ověřování klienta, problém s ověřováním, ID korelace"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: end-user
ms.openlocfilehash: 65c25fb70bebeb5eff15ffe63ce11a65f5cdb1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-help-with-two-step-verification"></a>Získat pomoc s dvoustupňové ověření
Tento článek obsahuje odpovědi hello nejběžnější otázky, které uživatelé požádat o dvoustupňovém ověřování. 

## <a name="why-do-i-have-tooperform-two-step-verification-can-i-turn-it-off"></a>Proč mají tooperform dvoustupňové ověřování? Můžete vypnout jeho nastavení?

Dvoustupňové ověření je funkce zabezpečení, které vaše organizace zvolili toouse tooprotect vaše účty. Je bezpečnější než pouhým heslem, protože závisí na dvě formy ověřování: něco znáte a něco s vámi máte. Hello něco, co víte, že je vaše heslo. Dobrý den, který je něco, které máte s vámi telefonu nebo zařízení, které obvykle mají s vámi. Pokud váš účet je chráněný pomocí dvoustupňové ověření, kyberzločinci nemůžete se přihlásit jako vám i v případě, že získají heslo. Se nelze přihlásit, protože nemají přístup tooyour phone. 

Společnost Microsoft nabízí dvoustupňové ověření, ale vaše organizace rozhodne toouse hello funkce. Nelze chodit Pokud vaše IT oddělení vyžaduje, aby se z vás, stejně jako jste nemohou výslovně nesouhlasit pomocí tooprotect heslo účtu. 

Pokud máte dvoustupňové ověření zapnuté pro svůj osobní účet Microsoft a chcete toochange nastavení, přečtěte si [o dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) místo. 

## <a name="i-dont-have-my-phone-with-me-today"></a>Není k dispozici telefon se mi ještě dnes

Některé dnů, po který necháte telefonu v domácnostech, ale stále nutné toosign v v práci. Hello první věcí, kterou byste měli je přihlášení pomocí různých ověření metoda. Při registraci pro dvoustupňové ověření, se můžete nastavit více než jeden telefonní číslo? tootry přihlásit jinou metodu, postupujte takto:

1. Přihlaste se jako obvykle.
2. Po otevření hello dvoustupňové ověření stránky zvolte **použít jinou možností ověření**.

   ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Vyberte možnost ověření hello chcete toouse. 
  - Pokud nemáte přístup tooyour alternativní metody buď, obraťte se na vaše IT oddělení tooget pomoc s přihlášením tooyour účtu.
  - Pokud máte přístup tooyour alternativní metody, pokračujte dvoustupňové ověřování.

Pokud nevidíte hello **použít jinou možností ověření** odkaz, pak to znamená, že jste nenastavili alternativní metody při první registraci pro dvoustupňové ověření. Obraťte se na vaše IT oddělení tooget pomoc s přihlášením tooyour účtu. Ujistěte se, když jste přihlášení, příliš[spravovat nastavení](multi-factor-authentication-end-user-manage-settings.md) tooadd další ověřovací metody pro další použití. 

## <a name="i-lost-my-phone-or-got-a-new-number"></a>I ztratíte telefon nebo získali nové číslo
Existují dva způsoby tooget zpět v tooyour účtu. Hello toosign pomocí vaší alternativní číslo telefonu pro ověření, je nejprve, pokud jste nastavili jednu. Hello druhou je tooask vaše IT oddělení tooclear nastavení.

Pokud váš telefon byl ztráty nebo odcizení, doporučujeme také, že se zjistit vaše IT oddělení. Budete potřebují tooreset vašeho hesla aplikací a zrušte všechny zapamatovaných zařízení. 

### <a name="use-an-alternate-phone-number"></a>Použít alternativní telefonní číslo
Pokud nastavíte více možností ověřování, jako jsou záložní telefonní číslo nebo ověřovací aplikace na jiném zařízení, použijte jeden z nich toosign v.

toosign pomocí hello alternativní telefonní číslo, postupujte takto:

1. Přihlaste se jako obvykle.
2. Když výzvami toofurther ověřit váš účet, zvolte **použít jinou možností ověření**.
   
   ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Vyberte hello telefonní číslo nebo zařízení, které máte přístup.
4. Poté, co si zpět ve vašem účtu [spravovat nastavení](multi-factor-authentication-end-user-manage-settings.md) toochange ověřování telefonní číslo.

### <a name="clear-your-settings"></a>Clear – nastavení
Pokud jste nenakonfigurovali telefonní číslo sekundárního ověřování, musíte toocontact o pomoc vaše oddělení IT. Mají jasné, takže hello nastavení je další čas jste přihlášení, zobrazí se výzva příliš[zaregistrovat pro dvoustupňové ověření](multi-factor-authentication-end-user-first-time.md) znovu.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>I doručována text nebo volání na telefon
Tady je několik důvodů, proč mohou zkuste toosign v, ale není přijímat textu hello nebo telefonní hovor. Pokud jste úspěšně přijme texty nebo telefonních hovorů tooyour telefonní v posledních hello, pak tento problém je pravděpodobně problém se zprostředkovatel hello telefon, není váš účet. Ujistěte se, že máte signál dobrý buňky. A pokud se pokoušíte tooreceive textová zpráva, ujistěte se, že budete mít tooreceive textové zprávy. Požádejte friend toocall jste nebo text jste jako testu. 

Pokud jste čekali několik minut, než text nebo volání, hello nejrychlejší způsob, jak tooget do vašeho účtu je tootry jinou možnost.

1. Vyberte **použít jinou možností ověření** na stránku hello, která čeká na ověření.
   
    ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Vyberte hello telefonní číslo doručení metodu nebo chcete toouse.
   
    Pokud jste dostali více ověřovací kódy, použijte hello nejnovější jeden.

Pokud nemáte nakonfigurován jinou metodu, obraťte se na oddělení IT a požádejte je tooclear nastavení. Hello příště přihlásíte, budete vyzváni příliš[nastavení služby Multi-Factor authentication](multi-factor-authentication-end-user-first-time.md) znovu.

Pokud máte často zpoždění kvůli toobad buňky signál, doporučujeme, abyste použili hello [aplikaci Microsoft Authenticator](microsoft-authenticator-app-how-to.md) na vašem smartphonu. aplikace Hello může generovat náhodné zabezpečovací kódy, které používají toosign v, a tyto kódy nevyžadují žádné buňky signál nebo připojení k Internetu.

## <a name="app-passwords-are-not-working"></a>Hesla aplikací nejsou práce
Zkontrolujte, zda jste správně zadali heslo aplikace hello. heslo aplikace Hello generované nahrazuje normální heslo, ale jenom pro starší aplikací klasické pracovní plochy, které nepodporují dvoustupňové ověřování. Pokud stále nefunguje, zkuste přihlášení a [vytvořit nové heslo aplikace](multi-factor-authentication-end-user-app-passwords.md).  Pokud stále nepomůže, obraťte se na oddělení IT a potom kliknul [odstranit vašich dosavadních hesel aplikací](../multi-factor-authentication-manage-users-and-devices.md) a vytvořte novou.

## <a name="i-didnt-find-an-answer-toomy-problem"></a>Nenašli I toomy problém s odpovědí.
Pokud jste se pokusili postup řešení, ale jsou stále spuštěná na problémy, obraťte se na oddělení IT. Měly by být možné tooassist je.

## <a name="related-topics"></a>Související témata
* [Spravovat nastavení pro dvoustupňové ověření](multi-factor-authentication-end-user-manage-settings.md)  
* [Nejčastější dotazy k aplikaci Microsoft Authenticator](microsoft-authenticator-app-faq.md)

