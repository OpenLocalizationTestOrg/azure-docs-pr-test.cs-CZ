---
title: "aaaGet spuštění pomocí podmíněného přístupu v Azure Active Directory | Microsoft Docs"
description: "Otestujte podmíněný přístup pomocí podmínku umístění."
services: active-directory
keywords: "podmíněný přístup tooapps, podmíněného přístupu s Azure AD, zabezpečení přístupu k prostředkům toocompany, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4521f5a34f5882e026f5e58a7127d8c55cba2f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a>Začínáme s podmíněným přístupem v Azure Active Directory

Podmíněný přístup je funkce služby Azure Active Directory, která umožňuje vám podmínky toodefine pod nimiž oprávněným uživatelům přístup k vaší aplikace. 

Toto téma obsahuje pokyny pro testování podmíněného přístupu na základě podmínky umístění ve vašem prostředí.  


## <a name="scenario-description"></a>Popis scénáře

Jeden požadavek na běžné v mnoha organizacích je tooonly vyžadovat vícefaktorové ověřování pro přístup k tooapps, který není provést z podnikové sítě intranet hello. S Azure Active Directory můžete snadno dosažení tohoto cíle konfigurací zásady podmíněného přístupu na základě polohy. Toto téma poskytuje podrobné pokyny ke konfiguraci související zásad. Hello využívá zásady [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) toodistinguish mezi pokusy o přístup z podnikové hello žádost je intranetu a všechny ostatní umístění.


## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto tématu se předpokládá, že jste obeznámeni s koncepty hello uvedených v [podmíněného přístupu Azure Active Directory](active-directory-conditional-access-azure-portal.md).

tootest tento scénář, potřebujete:

- Vytvoření zkušebního uživatele 

- Přiřadit Azure AD Premium licence toohello testovacího uživatele

- Nakonfigurujte spravovanou aplikaci a přiřaďte tooit vaše testovací uživatele

- Konfigurovat důvěryhodné IP adresy

Pokud potřebujete další podrobnosti o důvěryhodné IP adresy, přečtěte si [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="policy-configuration-steps"></a>Postup konfigurace zásad

**tooconfigure zásady podmíněného přístupu, udělat:**

1. V hello portál Azure, na hello levé navigační panel, klikněte na **Azure Active Directory**. 

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. Na hello **Azure Active Directory** okno, v hello **spravovat** klikněte na tlačítko **podmíněného přístupu**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. Na hello **podmíněného přístupu** okno, tooopen hello **nový** klikněte na okno na hello nástrojů v horní části hello **přidat**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. Na hello **nový** okno, v hello **název** textovému poli, zadejte název zásady.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. V hello **přiřazení** klikněte na tlačítko **uživatelů a skupin**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. Na hello **uživatelů a skupin** okně provést hello následující kroky:

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    a. Klikněte na tlačítko **vyberte uživatele a skupiny**.

    b. Klikněte na **Vybrat**.

    c. Na hello **vyberte** okně vyberte svého testovacího uživatele a pak klikněte na tlačítko **vyberte**.

    d. Na hello **uživatelů a skupin** okně klikněte na tlačítko **provádí**.

7. Na hello **nový** okno, tooopen hello **cloudových aplikací** okno, v hello **přiřazení** klikněte na tlačítko **cloudových aplikací**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. Na hello **cloudových aplikací** okně provést hello následující kroky:

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    a. Klikněte na tlačítko **vybrat aplikace**.

    b. Klikněte na **Vybrat**.

    c. Na hello **vyberte** okně vyberte cloudové aplikace a pak klikněte na tlačítko **vyberte**.

    d. Na hello **cloudových aplikací** okně klikněte na tlačítko **provádí**.

9. Na hello **nový** okno, tooopen hello **podmínky** okno, v hello **přiřazení** klikněte na tlačítko **podmínky**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. Na hello **podmínky** okno, tooopen hello **umístění** okně klikněte na tlačítko **umístění**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. Na hello **umístění** okně provést hello následující kroky:

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    a. V části **konfigurace**, klikněte na tlačítko **Ano**.

    b. V části **zahrnout**, klikněte na tlačítko **všech umístění**.

    c. Klikněte na tlačítko **vyloučit**a potom klikněte na **všechny důvěryhodné IP adresy**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    d. Klikněte na **Done** (Hotovo).

12. Na hello **podmínky** okně klikněte na tlačítko **provádí**.

13. Na hello **nový** okno, tooopen hello **Grant** okno, v hello **ovládací prvky** klikněte na tlačítko **Grant**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Na hello **Grant** okně provést hello následující kroky:

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Vyberte **vyžadovat vícefaktorové ověřování**.

    b. Klikněte na **Vybrat**.

15. Na hello **nový** okno, v části **povolit zásady**, klikněte na tlačítko **na**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Na hello **nový** okně klikněte na tlačítko **vytvořit**.


## <a name="testing-hello-policy"></a>Testování zásad hello

tootest zásady vaší aplikace by měla přístup ze zařízení, která: 

1. IP adresu, která je v rámci vaší nakonfigurované důvěryhodné IP adresy 

1. IP adresu, která není v rámci vaší nakonfigurované důvěryhodné IP adresy

Služba Multi-Factor authentication musí být pouze požadované při pokusu o připojení, která byla vytvořená ze zařízení, které není v rámci vaší nakonfigurované důvěryhodné IP adresy. 


## <a name="next-steps"></a>Další kroky

Pokud chcete toolearn více informací o podmíněného přístupu, přečtěte si téma [podmíněného přístupu Azure Active Directory](active-directory-conditional-access-azure-portal.md).

