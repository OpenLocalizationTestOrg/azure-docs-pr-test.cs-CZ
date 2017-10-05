---
title: "Známý sítí v portálu Azure classic | Microsoft Docs"
description: "Konfigurací sítě známé, nemusíte mít IP adresy, které jsou vlastněny organizaci součástí Sign in z několika zeměpisných oblastí a in přihlášení z IP adres s podezřelou aktivitu sestavy."
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
ms.openlocfilehash: e4d51d1d2f09fca34d749879e21d49f785eac35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="known-networks"></a>Známé sítě

> [!div class="op_single_selector"]
> * [Portál Azure Classic](active-directory-known-networks.md)
> * [Azure Portal](active-directory-known-networks-azure-portal.md)
> 
> 


Přístup k Azure Active Directory a sestavy využití, které slouží k získat přehled o integrity a zabezpečení adresáři vaší organizace. Tyto informace a správce directory pomohou určit, kde může být bezpečnostním rizikům, tak, aby adekvátní můžete naplánovat zmírnění.

Je možné, který "*přihlášení z několika zeměpisných oblastí*"a"*přihlášení z IP adres s podezřelou aktivitou*" sestavy nesprávně příznak IP adresy, které jsou ve skutečnosti vlastněny vaší organizace. 

Může k tomu, například dojít při: 

* Uživatel ve vaší Boston se office přihlásil vzdáleně do vašeho datového centra v San Franciscu aktivuje sestavy "Přihlášení z několika zeměpisných oblastí" 
* Uživatel vaší organizace pokusí o přihlašování pomocí aktivačních událostí nesprávné heslo sestavy "Přihlášení z IP adres s podezřelou aktivitou" 

Abyste zabránili těchto případech generování sestav zavádějící zabezpečení, měli byste přidat známé rozsahy IP adres do seznamu veřejnou IP adresu vaší organizace.    

### <a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Pokud chcete přidat vaší organizace veřejné rozsahy IP adres, proveďte následující kroky:

1. Přihlášení k [portál pro správu Azure](https://manage.windowsazure.com).

2. V levém podokně klikněte na **služby Active Directory**. 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-01.png)

3. V **Directory** vyberte adresáře.

4. V nabídce v horní části, klikněte na tlačítko **konfigurace**. 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-02.png)

5. Na kartě Konfigurace, přejděte na **vaší organizace rozsahů veřejných IP adres** 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-03.png)

6. Klikněte na tlačítko **přidat rozsahy známé IP adres**.

7. V zobrazeném dialogu přidat vaše rozsahy adres a po dokončení klikněte na tlačítko se zaškrtnutím. 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-04.png)

**Další zdroje informací:**

* [Zobrazení sestav přístupů a používání](active-directory-view-access-usage-reports.md)
* [Přihlášení z IP adres s podezřelou aktivitou](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Přihlášení z několika zeměpisných oblastí](active-directory-reporting-sign-ins-from-multiple-geographies.md)

