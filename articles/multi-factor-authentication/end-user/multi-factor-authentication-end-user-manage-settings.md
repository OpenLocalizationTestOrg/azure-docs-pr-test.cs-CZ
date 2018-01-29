---
title: "Spravovat nastavení dvoustupňového ověřování | Microsoft Docs"
description: "Spravujte, jak pomocí Azure Multi-Factor Authentication, včetně změnu kontaktních informací nebo konfigurace zařízení."
services: multi-factor-authentication
keywords: "vícefaktorové ověřování klienta, problém s ověřováním, ID korelace"
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 8d84574283aa0c94ce303b0a7e3bde335c0eb2b8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a>Spravovat nastavení pro dvoustupňové ověření
Tento článek obsahuje odpovědi na dotazy týkající se aktualizovat nastavení pro dvoustupňové ověření nebo vícefaktorové ověřování. Pokud máte problémy s přihlášení ke svému účtu, podívejte se na [došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md) Poradce při potížích.

## <a name="where-to-find-the-settings-page"></a>Kde najít nastavení stránky
V závislosti na tom, jak vaše společnost nastavit ověřování Azure Multi-Factor Authentication jsou na několika místech, kde můžete změnit nastavení jako vaše telefonní číslo.

Pokud vaše společnost podporují odeslaná konkrétní adresy URL nebo kroky, které chcete spravovat dvoustupňové ověření, postupujte podle těchto pokynů. Následující pokyny, jinak hodnota by měla fungovat pro ostatní uživatelé. Pokud tyto pokyny, ale nevidíte stejné možnosti, to znamená, že vaše škola nebo přizpůsobit svoje vlastní portál. Požádejte správce pro odkaz na portál Azure Multi-Factor Authentication.

1. Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. V pravém horním rohu vyberte název účtu a pak vyberte **profil**.  
3. Vyberte **dalšího ověření zabezpečení**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Další bezpečnostní ověření stránka načte s nastavením.

    ![Ověření](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Chcete změnit své telefonní číslo nebo přidat sekundární číslo
Je potřeba nakonfigurovat telefonní číslo sekundární ověřování.  Protože vaše primární telefonní číslo a mobilní aplikace jsou pravděpodobně na stejné telefonu, sekundární telefonní číslo je jediným způsobem, jak bude moct vrátit ke svému účtu, pokud dojde ke ztrátě nebo odcizení vašeho telefonu.

> [!NOTE]
> Pokud nebudete mít přístup k vaší primární telefonní číslo a potřebujete pomoc, získávání ve ke svému účtu, najdete v našich témata nápovědy v [došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md).  

**Chcete-li změnit primární telefonní číslo:**  

1. Na stránce další bezpečnostní ověření vyberte textové pole s vaší aktuální telefonní číslo a upravit ho pomocí nové telefonní číslo.  
2. Vyberte **Uložit**.  
3. Pokud je to počet, který používáte pro vaše upřednostňovanou možnost ověřování, je nutné ověřit nové číslo předtím, než jste jej uložili.  

**Chcete-li přidat sekundární telefonní číslo:**  

1. Na stránce další bezpečnostní ověření, zaškrtněte políčko vedle **telefon pro alternativní ověření.**  
2. Do textového pole zadejte sekundární telefonní číslo.  
3. Vyberte **Uložit** a dokončení změny.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>Vyžadovat dvoustupňové ověření znovu v zařízení, které jste označili jako důvěryhodný

V závislosti na nastavení organizace může mít zaškrtávací políčko, která uvádí, že "nezobrazovat dotaz dalších **X** dní" při provádění dvoustupňové ověření v prohlížeči. Pokud toto políčko zaškrtněte a pak ztráty zařízení nebo myslíte, že je váš účet ohrožená, obnovíte dvoustupňové ověřování pro všechna vaše zařízení.

1. Na stránce další bezpečnostní ověření, vyberte **obnovení služby Multi-Factor authentication u dřív důvěryhodných zařízení**.
2. Při příštím přihlášení na libovolném zařízení, budete vyzváni k provedení dvoustupňové ověřování.

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Jak vyčistit Microsoft Authenticator z původního zařízení a přesunout do nového?
Po odinstalaci aplikace ze zařízení nebo se zařízení resetovat, neodebere aktivace na back-end. Další informace najdete v tématu [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Další kroky
* Získat tipy pro odstraňování potíží a nápovědy na [došlo k potížím s dvoustupňové ověření](multi-factor-authentication-end-user-troubleshoot.md)
* Nastavit [hesla aplikací](multi-factor-authentication-end-user-app-passwords.md) pro všechny aplikace, které nepodporují dvoustupňové ověřování.
