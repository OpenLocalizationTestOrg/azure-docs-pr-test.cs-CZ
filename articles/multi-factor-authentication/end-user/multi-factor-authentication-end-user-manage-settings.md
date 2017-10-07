---
title: "aaaManage nastavení dvoustupňového ověřování | Microsoft Docs"
description: "Spravujte, jak pomocí Azure Multi-Factor Authentication, včetně změnu kontaktních informací nebo konfigurace zařízení."
services: multi-factor-authentication
keywords: "vícefaktorové ověřování klienta, problém s ověřováním, ID korelace"
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 2c974b08c584943f3c5a6b9bf16497d1706e8329
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a>Spravovat nastavení pro dvoustupňové ověření
Tento článek obsahuje odpovědi na otázky týkající se nastavení tooupdate pro dvoustupňové ověření nebo vícefaktorové ověřování. Pokud máte problémy s přihlášením tooyour účet, podívejte se příliš[došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md) Poradce při potížích.

## <a name="where-toofind-hello-settings-page"></a>Kde toofind hello nastavení stránky
V závislosti na tom, jak vaše společnost nastavit ověřování Azure Multi-Factor Authentication jsou na několika místech, kde můžete změnit nastavení jako vaše telefonní číslo.

Pokud váš správce IT odeslat na konkrétní adresy URL nebo kroky toomanage dvoustupňové ověření, postupujte podle těchto pokynů. Hello pokynů, jinak hodnota by měla fungovat pro ostatní uživatelé. Pokud tyto pokyny, ale nezobrazí hello stejné možnosti, které znamená, že vaše škola nebo přizpůsobit svoje vlastní portál. Požádejte správce pro portál Azure Multi-Factor Authentication tooyour odkaz hello.

1. Přihlaste se příliš[https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. V pravém horním hello vyberte název účtu a pak vyberte **profil**.  
3. Vyberte **dalšího ověření zabezpečení**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Hello další bezpečnostní ověření stránka načte s nastavením.

    ![Ověření](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-toochange-my-phone-number-or-add-a-secondary-number"></a>Chcete toochange telefonního čísla nebo přidat sekundární číslo
Je důležité tooconfigure telefonní číslo sekundární ověřování.  Protože vaše primární telefonní číslo a mobilní aplikace se pravděpodobně na hello stejné phone, hello sekundární telefonní číslo je jedinou možností hello, nebudou se moct tooget zpátky do vašeho účtu, pokud dojde ke ztrátě nebo odcizení vašeho telefonu.

> [!NOTE]
> Pokud nemáte přístup tooyour primární telefonní číslo a potřebujete pomoc, získávání v tooyour účtu, najdete v našich témata nápovědy v [došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md).  

**toochange vaší primární telefonní číslo:**  

1. Na stránce pro hello další bezpečnostní ověření vyberte hello textové pole s vaší aktuální telefonní číslo a upravit ho pomocí nové telefonní číslo.  
2. Vyberte **Uložit**.  
3. Pokud je číslo hello, který používáte pro vaše upřednostňovanou možnost ověřování, musíte tooverify hello nové číslo předtím, než jste jej uložili.  

**tooadd záložní telefonní číslo:**  

1. Na stránce další bezpečnostní ověření hello políčko hello vedle příliš**telefon pro alternativní ověření.**  
2. Zadejte do textového pole hello sekundární telefonní číslo.  
3. Vyberte **Uložit** a dokončení změny.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>Vyžadovat dvoustupňové ověření znovu v zařízení, které jste označili jako důvěryhodný

V závislosti na nastavení organizace může mít zaškrtávací políčko, která uvádí, že "nezobrazovat dotaz dalších **X** dní" při provádění dvoustupňové ověření v prohlížeči. Pokud toto políčko zaškrtněte a pak ztráty zařízení nebo myslíte, že je váš účet ohrožená, obnovíte dvoustupňové ověření tooall zařízení. 

1. Na stránce další bezpečnostní ověření hello vyberte **obnovení služby Multi-Factor authentication u dřív důvěryhodných zařízení**.
2. Hello při příštím přihlášení na libovolném zařízení, budete výzvami tooperform dvoustupňové ověřování. 

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-tooa-new-one"></a>Jak vyčistit Microsoft Authenticator z původního zařízení a přesunout tooa novým?
Když odinstalujete aplikaci hello ze zařízení nebo zařízení hello resetování, neodebere hello aktivace na back-end hello. Další informace najdete v tématu [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Další kroky
* Získat tipy pro odstraňování potíží a nápovědy na [došlo k potížím s dvoustupňové ověření](multi-factor-authentication-end-user-troubleshoot.md)
* Nastavit [hesla aplikací](multi-factor-authentication-end-user-app-passwords.md) pro všechny aplikace, které nepodporují dvoustupňové ověřování.
