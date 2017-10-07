---
title: aaaChoose mezi Azure MFA cloudu nebo server | Microsoft Docs
description: "Zvolte řešení hello vícefaktorového ověřování zabezpečení, které je pro vás nejvhodnější tím, že požádá, co se I při toosecure a kde jsou Moji uživatelé nachází.  Pak vyberte cloud, server MFA nebo AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: bd9639e5f744f586d9143c6e90b105ed645eecb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="choose-hello-azure-multi-factor-authentication-solution-for-you"></a>Výběr hello Azure Multi-Factor Authentication řešení pro vás
Vzhledem k tomu, že existuje několik typů Azure Multi-Factor Authentication (MFA), musíme odpovědět několik toofigure otázky, na která verze je správné jeden toouse hello.  Jsou to tyto otázky:

* [Co se pokouším toosecure](#what-am-i-trying-to-secure)
* [Kde se nachází uživatelé hello](#where-are-the-users-located)
* [Jaké funkce budu potřebovat?](#what-featured-do-i-need)

Hello následující části obsahují informace o určení každý z těchto odpovědi.

## <a name="what-am-i-trying-toosecure"></a>Co se pokouším toosecure?
řešení toodetermine hello správné dvoustupňové ověření, nejprve musíme odpovědět hello otázku, co se vám snažíme toosecure pomocí druhé metody ověřování.  Jedná se o aplikaci, která je v Azure?  Nebo systém vzdáleného přístupu?  Tak, že určíte, co se pokoušíme toosecure, jsme zodpovědět hello otázku, kde potřebuje služba Multi-Factor Authentication toobe povolena.  

| Co se vám snažíme toosecure | MFA v cloudu hello | Server MFA |
| --- |:---:|:---:|
| Vlastní aplikace Microsoftu |● |● |
| Aplikace SaaS v galerii aplikací hello |● |  |
| Webové aplikace publikované prostřednictvím proxy aplikace Azure AD |● |  |
| Aplikace služby IIS nepublikované prostřednictvím proxy aplikace Azure AD | |● |
| Vzdálený přístup, jako je například síť VPN, RDG | ● | ● |

## <a name="where-are-hello-users-located"></a>Kde se nachází uživatelé hello
V dalším kroku prohlížení, kde se nachází naši uživatelé pomáhá toodetermine hello správné řešení toouse, zda v hello cloudu nebo místně pomocí hello MFA serveru.

| Umístění uživatele | MFA v cloudu hello | Server MFA |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD a místní AD využívající federaci se službou AD FS |● |● |
| Azure AD a místní AD pomocí DirSync Azure AD Sync Azure AD Connect – bez synchronizace hesla |● |● |
| Azure AD a místní AD pomocí DirSync Azure AD Sync Azure AD Connect – se synchronizací hesla |● | |
| Místní služby Active Directory | |● |

## <a name="what-features-do-i-need"></a>Jaké funkce budu potřebovat?
Hello následující tabulka porovnává hello funkce, které jsou dostupné pomocí služby Multi-Factor Authentication v cloudu hello a s hello aplikace Multi-Factor Authentication Server.

| Funkce | MFA v cloudu hello | Server MFA |
| --- |:---:|:---:|
| Oznámení mobilní aplikace jako druhý faktor | ● | ● |
| Kód ověření mobilní aplikace jako druhý faktor | ● | ● |
| Telefonní hovor jako druhý faktor | ● | ● |
| Jednosměrné služby SMS jako druhý faktor | ● | ● |
| Obousměrné služby SMS jako druhý faktor | | ● |
| Tokeny hardwaru jako druhý faktor | | ● |
| Hesla aplikací pro klienty Office 365, kteří nepodporují MFA | ● | |
| Kontrola správce nad metodami ověřování | ● | ● |
| Režim kódu PIN | | ● |
| Výstraha podvodů |● | ● |
| Sestavy MFA |● | ● |
| Jednorázové potlačení | | ● |
| Vlastní přivítání pro telefonní hovory | ● | ● |
| Přizpůsobitelné ID volajícího pro telefonní hovory | ● | ● |
| Důvěryhodné IP adresy | ● | ● |
| Zapamatovat MFA pro důvěryhodná zařízení | ● | |
| Podmíněný přístup | ● | ● |
| Mezipaměť |  | ● |

## <a name="next-steps"></a>Další kroky

Teď, když jsme zjistili, jestli toouse cloudové služby Multi-Factor authentication nebo hello MFA serveru místně, můžete začít s nastavováním a používáním Azure Multi-Factor Authentication. **Vyberte ikonu hello, která představuje váš scénář**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Server](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</center>
