---
title: "aaaUsers příznakem pro sestavu riziko zabezpečení Azure Active Directory portálu hello | Microsoft Docs"
description: "Další informace o hello uživatelé označení příznakem pro sestavu riziko zabezpečení Azure Active Directory portálu hello"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5077cd61d6119745a85ed712623904633a151331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="users-flagged-for-risk-security-report-in-hello-azure-active-directory-portal"></a>Uživatelé označení příznakem pro sestavu riziko zabezpečení Azure Active Directory portálu hello

Pomocí sestavy zabezpečení hello v hello Azure Active Directory (Azure AD) je možné získat přehled o hello pravděpodobnost ohroženými uživatelskými účty ve vašem prostředí. 

Azure Active Directory zjistí podezřelou akce, které jsou související tooyour uživatelské účty. Pro každou zjištěnou akci se vytvoří záznam nazvaný *riziková událost*. Další informace najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md). 

Hello zjistila rizikových událostí jsou použité toocalculate:

- **Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu. Další informace najdete v tématu popisujícím [riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins). 

- **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další informace najdete v tématu popisujícím [uživatele označené příznakem rizika](active-directory-identityprotection.md#users-flagged-for-risk).  

V hello portálu Azure, můžete najít hello zabezpečení hlásí hello **Azure Active Directory** okno v hello **zabezpečení** části.  

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a>Jaké licence Azure AD potřebujete tooaccess sestavy zabezpečení?  

Sestavy uživatelů označených příznakem rizika nabízí všechny edice Azure Active Directory.  
Ale hello úroveň členitost liší hello edice: 

- V hello **edice Basic a Azure Active Directory volné**, již získat seznam uživatelů, které jsou označeny k riziko. 

- Hello **Azure Active Directory Premium 1** edice rozšiřuje tohoto modelu tím, že také umožňuje tooexamine, některé základní riziko události, které pro každou sestavu nebyla zjištěna hello. 

- Hello **Azure Active Directory Premium 2** edice nabízí hello nejvíce podrobné informace o všech základní rizikových událostech a umožní vám tooconfigure zásady zabezpečení, které automaticky odpovídat tooconfigured rizik úrovně.



## <a name="azure-active-directory-free-and-basic-edition"></a>Edice Free a Basic služby Azure Active Directory

Hello uživatelé označení příznakem pro sestavu riziko v edicích volné a basic služby Azure Active Directory hello vám poskytne seznam uživatelské účty, které může být ohrožena. 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/03.png)

Výběr uživatele, otevře se okno data související uživatelské hello.
Pro uživatele, kteří jsou v ohrožení můžete zkontrolovat historie přihlašování hello uživatelů a resetování hesla hello v případě potřeby.

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/46.png)


V tomto dialogovém okně máte možnost:

- Stažení sestavy hello

- Vyhledávat uživatele

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a>Edice Premium služby Azure Active Directory

Hello uživatelé označení příznakem pro sestavu riziko v edicích hello Azure Active Directory premium nabízí:

- [Seznam uživatelských účtů](active-directory-identityprotection.md#users-flagged-for-risk), které mohly být napadeny nebo vyzrazeny 

- Agregovat informace o hello [rizik typů událostí](active-directory-identity-protection-risk-events.md) , o kterých bylo zjištěno

- Zprávu o možnost toodownload hello

- Možnost tooconfigure [uživatele riziko nápravy zásad](active-directory-identityprotection.md#user-risk-security-policy)  


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/71.png)

Po výběru uživatele získáte podrobné zobrazení sestavy pro tohoto uživatele, které vám umožňuje:

- Zobrazit všechny přihlášení otevřete hello

- Resetování hesla hello uživatele

- Zavřít všechny události

- Prozkoumejte hlášené riziko události pro uživatele hello. 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/324.png)


tooinvestigate riziko události, vyberte jednu z hello seznamu tooopen hello **podrobnosti** okno pro tuto událost riziko. Na hello **podrobnosti** okno, máte možnost tooeither hello [ruční zavření riziko událostí](active-directory-identityprotection.md#closing-risk-events-manually) nebo znovu aktivovat ručně uzavřené riziko událostí. 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a>Další kroky

- Další informace o Azure Active Directory Identity Protection najdete v tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

