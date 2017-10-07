---
title: aaaTranslate odkazy a Proxy aplikace Azure AD adresy URL | Microsoft Docs
description: "Popisuje hello základní informace o Azure AD Application Proxy konektory."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7ec2b9fb01617067cf5d676037877bf72c19217b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Přesměrování pevně zakódované odkazy k aplikacím publikovaným pomocí proxy aplikace služby Azure AD

Proxy aplikace služby Azure AD díky vaše místní aplikace k dispozici toousers, kdo jsou vzdálené nebo na jejich vlastní zařízení. Některé aplikace, ale měla vyvinuté pomocí místních odkazů v hello HTML. Tyto odkazy nefungují správně, při aplikace hello používá vzdáleně. Až budete mít několik místní aplikace bodu tooeach jiné, uživatelé očekávají, že tookeep odkazy hello pracovat, když nejsou v hello office. 

Hello nejlepší způsob, jak toomake se, že odkazy fungovaly hello stejné prostředí uvnitř a mimo podnikovou síť je tooconfigure hello externí adresy URL vaší aplikace toobe hello stejné jako jejich interní adresy URL. Použití [vlastní domény](active-directory-application-proxy-custom-domains.md) tooconfigure vaše externí adresy URL toohave podnikové doméně název místo hello výchozí doméně proxy aplikace.

Pokud nemůžete použít vlastní domény ve vašem klientovi, hello odkaz překlad funkce proxy aplikací zajišťuje vaše odkazy práce bez ohledu na to, kde jsou vaši uživatelé. Pokud máte aplikace, které bod přímo toointernal koncových bodů nebo porty, můžete namapovat tyto interní toohello adresy URL publikovat externí adresy URL Proxy aplikace. Pokud je povoleno překlad odkaz, a hledá Proxy aplikací pomocí jazyka HTML, CSS a vyberte značky jazyka JavaScript pro publikované interní odkazy. Potom hello Proxy aplikace služby znamená, že je tak, aby uživatelé získají bez přerušení prostředí.

>[!NOTE]
>Hello odkaz překlad funkce je pro klienty, kteří pro ať důvodu, nemůžete použít vlastní domény toohave hello stejné interní a externí adresy URL pro svoje aplikace. Před povolením této funkce najdete v části Pokud [vlastních domén v Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) může fungovat pro vás.
>
>Pokud potřebujete tooconfigure s odkaz překlad aplikace hello služby SharePoint, naleznete v [konfigurace mapování alternativních adres URL pro službu SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx) pro jiné odkazy toomapping přístup.

## <a name="how-link-translation-works"></a>Jak funguje překlad propojení

Po ověření když hello proxy serveru předá hello aplikace dat toohello uživatele, Proxy aplikace prohledá hello aplikace pro pevně zakódované odkazy a nahradí je jejich odpovídající publikovaná externí adresy URL.

Proxy aplikace se předpokládá, že aplikace jsou kódování ve formátu UTF-8. Pokud to není hello případ, zadejte typ kódování hello v hlavičce http odpovědi jako `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Odkazy, které se vztahuje?

funkce překladu propojení Hello pouze vyhledá odkazy, které jsou v kódu značek v textu hello aplikace. Proxy aplikace má samostatné funkce pro překlad adres URL nebo soubory cookie v záhlaví. 

Existují dva typy běžné interní odkazů v místním aplikacím:

- **Relativní odkazy interní** tento bod tooa sdíleného prostředku do místního souboru struktury jako `/claims/claims.html`. Tyto odkazy se automaticky fungovat v aplikacích, které jsou publikované prostřednictvím Proxy aplikace a pokračovat toowork s nebo bez překladu odkaz. 
- **Vnitřní propojení pevně zakódované** tooother místní aplikace, jako je `http://expenses` nebo publikovat soubory, jako jsou `http://expenses/logo.jpg`. Hello odkaz překlad funkce funguje na interní odkazy pevně zakódované a změní jejich toopoint toohello externí adresy URL vyžadující toogo prostřednictvím vzdálených uživatelů.

### <a name="how-do-apps-link-tooeach-other"></a>Jak aplikace propojit tooeach další?

Překlad odkaz je povolené pro každou aplikaci tak, aby budete mít kontrolu nad hello činnost koncového uživatele pro aplikaci na úrovni hello. Zapnout překlad odkaz pro aplikaci, když chcete, aby odkazy hello *z* přeložit toobe této aplikace, není odkazy *k* tuto aplikaci. 

Předpokládejme například, že máte tři aplikacích publikovaných prostřednictvím Proxy aplikace, že všechny odkaz tooeach jiných: výhody, náklady a cesta. Je čtvrtá aplikaci a zpětnou vazbu, která není publikována prostřednictvím Proxy aplikace.

Když povolíte překlad odkaz pro aplikaci hello výhody, tooExpenses hello odkazy a cesta jsou přesměrovaného toohello externí adresy URL pro tyto aplikace, ale hello odkaz tooFeedback není přesměrován, protože neexistuje žádná externí adresa URL. Odkazy z výdajů a cesta back tooBenefits nefungují, protože odkaz překlad nebyla povolena pro tyto dvě aplikace.

![Odkazy z aplikací tooother výhody, pokud je povoleno překlad odkaz](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Odkazy, které nejsou přeloženy?

tooimprove výkonu a zabezpečení, nejsou některé odkazy přeložit:

- Odkazy není uvnitř značky kódu. 
- Odkazy nejsou v HTML, CSS a JavaScript. 
- Vnitřní propojení otevřít z jiných aplikací. Odkazy budou odesílat prostřednictvím e-mailu nebo pomocí rychlé zprávy nebo součástí jiné dokumenty, nebude možné přeložit. Hello uživatelé potřebovat tooknow toogo toohello externí URL.

Pokud potřebujete toosupport jednu z těchto dvou scénářů, použijte hello stejné interní a externí adresy URL místo překlad odkaz.  

## <a name="enable-link-translation"></a>Povolit překlad odkaz

Začínáme s překlad odkaz je stejně snadná jako kliknutí na tlačítko:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.
2. Přejděte příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** > hello vyberte aplikaci, které se mají toomanage > **Proxy aplikace**.
3. Zapnout **překládat adresy URL v textu aplikace** příliš**Ano**.

   ![Vyberte Ano tootranslate adresy URL v textu aplikace](./media/application-proxy-link-translation/select_yes.png).
4. Vyberte **Uložit** tooapply změny.

Teď když vaši uživatelé přístup k této aplikaci, hello proxy automaticky vyhledá interní adresy URL, které byly publikovány prostřednictvím Proxy aplikace na klientovi.

## <a name="send-feedback"></a>Pošlete svůj názor

Chceme, aby vaše nápovědy toomake tento pracovní funkce pro všechny aplikace. Jsme hledání více než 30 značky v kódu HTML a CSS a jsou vzhledem k tomu, které toosupport případech JavaScript. Pokud máte příkladem generovaného odkazy, které nejsou se překlad vztahuje, pošlete fragmentu kódu příliš[zpětnou vazbu Proxy aplikací](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Další kroky
[Použití vlastních domén s Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) toohave hello stejnou interní a externí adresu URL

[Konfigurace mapování alternativních adres URL pro službu SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx)
