---
title: "Azure Active Directory B2C: Zakázat ověření e-mailu během registrace příjemce | Microsoft Docs"
description: "Téma ukázka, jak toodisable e-mailu ověření během registrace v Azure Active Directory B2C příjemce"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 433f32b8-96d2-4113-aa82-efcf42fa9827
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/06/2017
ms.author: parakhj
ms.openlocfilehash: a8a42eddcb577725f04d70e1b1ebbebf10b5937c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Zakázání e-mailu ověření během registrace příjemce
Když je povolené, poskytuje Azure Active Directory (Azure AD) B2C a příjemce hello toosign možnost pro aplikace pomocí e-mailovou adresu a vytvoření místního účtu. Azure AD B2C zajišťuje platné e-mailové adresy, tím, že příjemci tooverify je během procesu registrace hello. Zabrání také škodlivý automatizovaného procesu z generování falešných účty pro aplikace hello.

Někteří vývojáři aplikace raději tooskip e-mailu ověření během procesu registrace hello a místo toho mít příjemce ověřit e-mailovou adresu hello později. toosupport to Azure AD B2C můžete být nakonfigurované toodisable ověření e-mailu. To vytvoří hladší procesu registrace a poskytuje vývojářům hello flexibilitu toodifferentiate hello příjemci, které ověření e-mailové adresy z těchto příjemci, které ještě nebyly.

Zásady registrace mají ve výchozím nastavení zapnutá ověření e-mailu. Použití hello následující kroky tooturn ho vypnout:

1. [Postupujte podle těchto kroků toonavigate toohello B2C okno s funkcemi na hello portál Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klikněte na tlačítko **registrace zásady** nebo **zásady registrace nebo přihlášení** v závislosti na tom, co jste nakonfigurovali pro registraci.
3. Klikněte na vaše zásady (například "B2C_1_SiUp") tooopen ho. Klikněte na tlačítko **upravit** hello horní části okna hello.
4. Klikněte na tlačítko **přizpůsobení uživatelského rozhraní stránky**.
5. Klikněte na tlačítko **stránku pro přihlášení místní účet**.
6. Klikněte na tlačítko **e-mailovou adresu** v hello **název** sloupce pod hello **atributy registrace** části.
7. Přepnutí hello **vyžadovat ověření** možnost příliš**ne**.
8. Klikněte na tlačítko **OK** dolnímu hello, dokud se nedostanete hello **upravit zásady** okno.
9. Klikněte na tlačítko **Uložit** hello horní části okna hello. Hotovo!

> [!NOTE]
> Zakázání e-mailu ověření v procesu registrace hello může způsobit, že toospam. Pokud zakážete hello výchozí nastavení, doporučujeme přidání vlastního ověřovacího systému.
> 
> 

Snažíme se vždy otevřete toofeedback a návrhy! Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor hello dolní části stránky hello. Pro žádosti o funkce, přidejte je příliš[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
