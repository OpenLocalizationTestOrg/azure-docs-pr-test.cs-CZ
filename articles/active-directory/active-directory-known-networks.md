---
title: "aaaKnown sítě v hello portál Azure classic | Microsoft Docs"
description: "Konfigurací sítě známé, nemusíte mít IP adresy, které jsou vlastněny organizaci součástí hello in přihlášení z několika zeměpisných oblastí a in přihlášení z IP adres s podezřelou aktivitu sestavy."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: ec636cdda172cd3baeb1e606dd8d6e1949fbc63b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="known-networks"></a>Známé sítě

> [!div class="op_single_selector"]
> * [Portál Azure Classic](active-directory-known-networks.md)
> * [Azure Portal](active-directory-known-networks-azure-portal.md)
> 
> 


Můžete použít Azure Active Directory přístup a použití sestav toogain přehled hello integrity a zabezpečení adresáře vaší organizace. Tyto informace a správce directory pomohou určit, kde může být bezpečnostním rizikům, tak, aby se adekvátní naplánovat toomitigate těchto rizik.

Je možné, že hello "*přihlášení z několika zeměpisných oblastí*"a"*přihlášení z IP adres s podezřelou aktivitou*" sestavy nesprávně příznak IP adresy, které jsou ve skutečnosti vlastněny vaší organizace. 

Může k tomu, například dojít při: 

* Uživatel v centrále Boston má přihlášený vzdáleně tooyour datového centra v Brno aktivační události hello "Sign in z několika zeměpisných oblastí" sestavy 
* Uživatel vaší organizace pokusí toosign v s hello nesprávné heslo aktivační události "Sign in z IP adres s podezřelou aktivitou" sestavy 

sestavy tooprevent těchto případech generování zavádějící zabezpečení, měli byste přidat IP adresu rozsahy toohello seznam se známými a veřejné IP adresy vaší organizace.    

### <a name="tooadd-your-organizations-public-ip-address-ranges-perform-hello-following-steps"></a>rozsahy tooadd veřejnou IP adresu vaší organizace, proveďte následující kroky hello:

1. Přihlášení toohello [portál pro správu Azure](https://manage.windowsazure.com).

2. V levém podokně hello, klikněte na **služby Active Directory**. 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-01.png)

3. V hello **Directory** vyberte adresáře.

4. V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**. 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-02.png)

5. Na kartě Konfigurace hello přejděte příliš**vaší organizace rozsahů veřejných IP adres** 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-03.png)

6. Klikněte na tlačítko **přidat rozsahy známé IP adres**.

7. Přidejte vaše rozsahy adres v hello dialogu, který se zobrazí a potom klepněte na tlačítko zaškrtnutí hello po dokončení. 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-04.png)

**Další zdroje informací:**

* [Zobrazení sestav přístupů a používání](active-directory-view-access-usage-reports.md)
* [Přihlášení z IP adres s podezřelou aktivitou](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Přihlášení z několika zeměpisných oblastí](active-directory-reporting-sign-ins-from-multiple-geographies.md)

