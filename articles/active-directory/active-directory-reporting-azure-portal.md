---
title: "vytváření sestav služby Active Directory aaaAzure | Microsoft Docs"
description: "Poskytuje obecný přehled generování sestav v Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c91813acbdc4b0bfcd164169b0b575accac227d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting"></a>Generování sestav v Azure Active Directory

Pomocí generování sestav v Azure Active Directory můžete získat přehled o stavu vašeho prostředí.  
Hello zadaná data umožňuje:

- Určit, jak uživatelé využívají vaše aplikace a služby.
- Zjistit potenciální rizika, které mají vliv na stav hello prostředí
- Řešit problémy, které brání uživatelům v práci.  

Architektura pro vytváření sestav Hello spoléhá na dvě hlavní pilíře:

- Sestavy zabezpečení
- Sestavy aktivit

![Vytváření sestav](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a>Sestavy zabezpečení

Hello sestavy zabezpečení v Azure Active Directory pomoct vám tooprotect identity ve vaší organizaci.  
V Azure Active Directory existují dva typy sestav zabezpečení:

- **Uživatelé označení příznakem rizik** – hello [uživatelé označení příznakem riziko zabezpečení sestavy](active-directory-reporting-security-user-at-risk.md), získat přehled o uživatelské účty, které může být ohrožena.

- **Rizikové přihlášení** – s hello [sestavy zabezpečení rizikové přihlášení](active-directory-reporting-security-risky-sign-ins.md), můžete získat slouží jako ukazatel přihlašovací pokusy, které se možná prováděly někdo, kdo není hello legitimní vlastníka uživatelského účtu. 

**Jaké licence Azure AD potřebujete tooaccess sestavy zabezpečení?**  
Sestavy uživatelů označených příznakem rizika a rizikových přihlášení nabízí všechny edice Azure Active Directory.  
Ale hello úroveň členitost liší hello edice: 

- V hello **edice Basic a Azure Active Directory volné**, již získat seznam uživatelů, které jsou označeny k rizika a rizikové přihlášení. 

- Hello **Azure Active Directory Premium 1** edice rozšiřuje tohoto modelu tím, že také umožňuje tooexamine, některé základní riziko události, které pro každou sestavu nebyla zjištěna hello. 

- Hello **Azure Active Directory Premium 2** edice nabízí vám hello nejvíce podrobné informace o hello základní rizikových událostech a také vám umožní tooconfigure zásady zabezpečení, které automaticky odpovídat tooconfigured úrovní rizika.


## <a name="activity-reports"></a>Sestavy aktivit

V Azure Active Directory existují dva typy sestav aktivit:

- **Protokoly auditu** – hello [protokoly auditu sestava aktivit](active-directory-reporting-activity-audit-logs.md) vám poskytne přístup k historii toohello každý úkol provést ve vašem klientovi.

- **Přihlášení** – s hello [sestava aktivit přihlášení](active-directory-reporting-activity-sign-ins.md), můžete určit, kdo má provést úlohy hello hlášené sestavy protokolů auditu hello.



Hello **protokoly auditu sestavy** vám poskytne záznamů aktivit systému dodržování předpisů.
Mimo jiné hello za předpokladu, že data vám umožní tooaddress běžné scénáře, jako:

- Někdo v klienta získali přístupové skupiny pro správu tooan. Kdo jim dal přístup? 

- Chci, aby tooknow hello seznam uživatelů, přihlášení k konkrétní aplikaci od I nedávno zařazený nemá hello aplikace a chcete tooknow Pokud dobře funguje

- Chci, aby tooknow kolik heslo resetovat se děje v klienta


**Jaké licence Azure AD potřebujete sestavy protokolů auditu hello tooaccess?**  
Sestava protokoly auditu Hello je k dispozici pro funkce, pro které máte licence. Pokud máte licenci pro konkrétní funkci, máte také přístup toohello auditní informace protokolu pro ni.

Další podrobnosti najdete v tématu **porovnávání všeobecně dostupná funkce edice Free, Basic a Premium hello** v [Azure Active Directory funkce a možnosti](https://www.microsoft.com/cloud-platform/azure-active-directory-features).   



Hello **sestava aktivit přihlášení** umožňuje tootoofind odpovědi tooquestions jako například:

- Co je hello přihlášení vzor uživatele?
- Kolik uživatelů se přihlásilo za týden?
- Co je hello stav těchto přihlášení?


**Jaké licence Azure AD se budete potřebovat tooaccess hello sestava aktivit přihlášení?**  
Sestava aktivit přihlášení hello tooaccess, vašeho klienta musí mít licenci na Azure AD Premium s ním spojená.


## <a name="programmatic-access"></a>Programový přístup

Přidání toohello uživatelské rozhraní, generování sestav Azure Active Directory také nabízí [programový přístup](active-directory-reporting-api-getting-started-azure-portal.md) toohello data pro vytváření sestav. Hello data tyto sestavy může být velmi užitečná tooyour aplikace, jako je například systémů SIEM, auditování a nástroje business intelligence. rozhraní API poskytují programový přístup k datům toohello pomocí sady založené na REST API, vytváření sestav Hello Azure AD. Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů. 


## <a name="next-steps"></a>Další kroky

Pokud chcete více informací o hello tooknow různé typy sestav v Azure Active Directory, najdete v části:

- [Sestava uživatelů s příznakem rizika](active-directory-reporting-security-user-at-risk.md)
- [Sestava rizikových přihlášení](active-directory-reporting-security-risky-sign-ins.md)
- [Sestava protokolů auditu](active-directory-reporting-activity-audit-logs.md)
- [Sestava protokolů přihlášení](active-directory-reporting-activity-sign-ins.md)

Pokud chcete více informací o přístupu k hello hello data pomocí hello reporting rozhraní API pro vytváření sestav tooknow, najdete v části: 

- [Začínáme s Azure Active Directory, vytváření sestav API hello](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png