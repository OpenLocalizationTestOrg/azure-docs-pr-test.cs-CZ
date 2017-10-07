---
title: "Sestava přihlašování aaaRisky hello Azure Active Directory portálu | Microsoft Docs"
description: "Další informace o hello rizikové přihlášení sestavy v portálu Azure Active Directory hello"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d8df5cafea6b38f3e364c24a6aff599abe088e88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="risky-sign-ins-report-in-hello-azure-active-directory-portal"></a>Sestava rizikové přihlášení na portálu Azure Active Directory hello

Pomocí sestavy hello zabezpečení v Azure Active Directory (Azure AD) je možné získat přehled o hello pravděpodobnost ohroženými uživatelskými účty ve vašem prostředí. 

Azure AD detekuje podezřelé akce, které jsou související tooyour uživatelské účty. Pro každou zjištěnou akci se vytvoří záznam nazvaný *riziková událost*. Další podrobnosti najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md). 

Hello zjistila rizikových událostí jsou použité toocalculate:

- **Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu. Další podrobnosti najdete v tématu [Riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins). 

- **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další podrobnosti najdete v tématu [Uživatelé označení příznakem rizika](active-directory-identityprotection.md#users-flagged-for-risk).  

V [hello portál Azure](https://portal.azure.com), můžete najít hello zabezpečení hlásí hello **Azure Active Directory** okno v hello **zabezpečení** části. 

![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a>Jaké licence Azure AD potřebujete tooaccess sestavy zabezpečení?  

Sestavy rizikových přihlášení nabízí všechny edice Azure Active Directory.  
Ale hello úroveň členitost liší hello edice: 

- V hello **edice Basic a Azure Active Directory volné**, již získat seznam rizikových přihlášení. 

- Hello **Azure Active Directory Premium 1** edice rozšiřuje tohoto modelu tím, že také umožňuje tooexamine, některé základní riziko události, které pro každou sestavu nebyla zjištěna hello. 

- Hello **Azure Active Directory Premium 2** edice nabízí vám hello nejvíce podrobné informace o všech základní rizikových událostech a také vám umožní tooconfigure zásady zabezpečení, které automaticky odpovídat tooconfigured úrovní rizika.



## <a name="azure-active-directory-free-and-basic-edition"></a>Edice Free a Basic služby Azure Active Directory

Hello Azure Active Directory volné a Základní edice poskytují seznam rizikových přihlášení, které byly nalezeny pro vaše uživatele. Tato sestava uvádí:

- **Uživatel** – hello název hello uživatele, který byl použit během hello přihlášení operace
- **IP** -hello IP adresa hello zařízení, která byla použité tooconnect tooAzure služby Active Directory
- **Umístění** -hello umístění použít tooconnect tooAzure služby Active Directory
- **Čas přihlášení** -hello čas, kdy provést přihlášení hello
- **Stav** -hello stav přihlášení hello


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/01.png)

Založené na šetření hello rizikové sign-in, můžete poskytnout zpětnou vazbu tooAzure služby Active Directory v podobě hello následující akce:

- Vyřešit
- Označit jako falešně pozitivní
- Ignorovat
- Znovu aktivovat

![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/21.png)

další informace najdete v tématu věnovaném [ručnímu zavření rizikových událostí](active-directory-identityprotection.md#closing-risk-events-manually).

V tomto dialogovém okně máte možnost:

- Vyhledávat prostředky
- Stáhněte si data sestavy hello


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a>Edice Premium služby Azure Active Directory

Sestava Hello rizikové přihlášení v hello Azure Active Directory premium edicích nabízí:

- Agregovat informace o hello [rizik typů událostí](active-directory-identity-protection-risk-events.md) , o kterých bylo zjištěno

- Zprávu o možnost toodownload hello


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/456.png)


Při výběru rizikové události získáte zobrazení podrobné sestavy pro tuto rizikovou událost, které umožňuje následující akce:

- Možnost tooconfigure [uživatele riziko nápravy zásad](active-directory-identityprotection.md#user-risk-security-policy)  

- Zkontrolujte hello detekce časovou osu pro hello riziko událostí  

- Kontrola seznamu uživatelů, pro které byla riziková událost zjištěná

- [Ruční zavření rizikových událostí](active-directory-identityprotection.md#closing-risk-events-manually) nebo reaktivace ručně zavřené rizikové události 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/457.png)

Po výběru uživatele získáte podrobné zobrazení sestavy pro tohoto uživatele, které vám umožňuje:

- Zobrazit všechny přihlášení otevřete hello

- Resetování hesla hello uživatele

- Zavřít všechny události

- Prozkoumejte hlášené riziko události pro uživatele hello. 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/324.png)


tooinvestigate riziko události, vyberte ze seznamu hello.  
Tím se otevře hello **podrobnosti** okno pro tuto událost riziko. Na hello **podrobnosti** okno, máte možnost tooeither hello [ruční zavření riziko událostí](active-directory-identityprotection.md#closing-risk-events-manually) nebo znovu aktivovat ručně uzavřené riziko událostí. 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a>Další kroky

- Další informace o Azure Active Directory Identity Protection najdete v tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

