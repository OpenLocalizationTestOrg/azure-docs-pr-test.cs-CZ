---
title: "instalace rozšíření prohlížeče hello aplikace přístup k panelu aaaProblem | Microsoft Docs"
description: "Jak běžné chyby toofix došlo při instalaci rozšíření prohlížeče panely přístup hello"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 5f750d12c5f9b405ec4f81596d5cc5e0a48f9a62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-access-panel-browser-extension"></a>Problém instalace rozšíření prohlížeče hello aplikace přístup k panelu

Hello přístupový Panel je, že je webový portál, který umožňuje uživatel, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spouštět cloudové aplikace tento správce hello Azure AD udělil přístup. Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím hello přístupového panelu. Hello přístupový Panel je oddělená od hello portál Azure a nevyžaduje, aby uživatelé toohave předplatné Azure.

toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele. Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>Splňuje požadavky na prohlížeč pro hello přístupového panelu

Hello přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele. Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:

-   Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější

-   Hraniční Windows 10 Anniversary Edition nebo novější. 

-   Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější

-   Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Jak tooinstall hello rozšíření přístup k panelu prohlížeče

tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:

1.  Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.

3.  V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.

4.  Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení. **Přidat** hello rozšíření tooyour prohlížeče.

5.  Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.

6.  Po instalaci **restartujte** relaci prohlížeče.

7.  Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO

Může také stáhnout hello rozšíření pro Chrome a hraniční z hello přímé odkazy níže:

-   [Rozšíření pro Chrome přístup panely](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření panelu přístup Edge](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Nastavení zásad skupiny pro Internet Explorer

Můžete nastavit zásady skupiny, které umožňují tooremotely instalace hello přístupový Panel rozšíření pro Internet Explorer na počítačích uživatelů.

Hello požadavky patří:

-   Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a připojení uživatelů počítačů tooyour domény.

-   Musíte mít hello "Upravit nastavení" oprávnění tooedit hello objekt zásad skupiny (GPO). Ve výchozím nastavení, toto oprávnění mají členové hello následující skupiny zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners. [Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Postupujte podle kurzu hello [jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md) pro podrobné pokyny, jak tooconfigure hello zásad skupiny a nasadit ji toousers.

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a>Řešení potíží s hello přístupového panelu v aplikaci Internet Explorer

Postupujte podle hello [Poradce při potížích hello rozšíření panely přístup pro prohlížeč Internet Explorer](active-directory-saas-ie-troubleshooting.md) Průvodce pro přístup k nástroj pro diagnostiku a podrobné informace o konfiguraci hello rozšíření pro aplikaci Internet Explorer.

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a>Pokud tyto kroky řešení potíží Nepokoušejte se vyřešit problém hello

Otevřete lístek podpory se hello, pokud je k dispozici následující informace:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   TenantID

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další kroky
[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
