---
title: "Azure Active Directory B2C: Zakázat ověření e-mailu během registrace příjemce | Microsoft Docs"
description: "Téma, který ukazuje, jak zakázat ověření e-mailu během registrace v Azure Active Directory B2C příjemce"
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
ms.openlocfilehash: d8e44a8aade60d21734477d60bccc2bd5194436e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Zakázání e-mailu ověření během registrace příjemce
Když je povolené, Azure Active Directory (Azure AD) B2C dává možnost zaregistrovat pro aplikace pomocí e-mailovou adresu a vytvoření místního účtu příjemce. Azure AD B2C zajišťuje platné e-mailové adresy, tím, že příjemci k ověření je během procesu registrace. Zabrání také škodlivý automatizovaného procesu z generování falešných účty pro aplikace.

Někteří vývojáři aplikace přednost přeskočit ověření e-mailu během procesu registrace a místo toho mít příjemce e-mailovou adresu ověřte později. Za tímto účelem lze nakonfigurovat Azure AD B2C zakázat ověření e-mailu. To vytvoří hladší procesu registrace a poskytuje vývojářům možnost odlišit od příjemce, které ověření e-mailové adresy z těchto příjemci, které ještě nebyly.

Zásady registrace mají ve výchozím nastavení zapnutá ověření e-mailu. Chcete-li vypnout pomocí následujících kroků:

1. [Postupujte podle těchto kroků přejděte do okna s funkcemi B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klikněte na tlačítko **registrace zásady** nebo **zásady registrace nebo přihlášení** v závislosti na tom, co jste nakonfigurovali pro registraci.
3. Klikněte na tlačítko vaše zásady (například "B2C_1_SiUp") a ten se otevře. Klikněte na tlačítko **upravit** v horní části okna.
4. Klikněte na tlačítko **přizpůsobení uživatelského rozhraní stránky**.
5. Klikněte na tlačítko **stránku pro přihlášení místní účet**.
6. Klikněte na tlačítko **e-mailovou adresu** v **název** sloupce pod **atributy registrace** části.
7. Přepnutí **vyžadovat ověření** možnost k **ne**.
8. Klikněte na tlačítko **OK** v dolní části, dokud se nedostanete **upravit zásady** okno.
9. Klikněte na tlačítko **Uložit** v horní části okna. Hotovo!

> [!NOTE]
> Zakázání e-mailu ověření v procesu registrace může vést k zasílání nevyžádané pošty. Pokud zakážete výchozí nastavení, doporučujeme přidání vlastního ověřovacího systému.
> 
> 

Snažíme se vždy otevřený a názory a návrhy! Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor v dolní části stránky. Pro žádosti o funkce, přidejte je do [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).