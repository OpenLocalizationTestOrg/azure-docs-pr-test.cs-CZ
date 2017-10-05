---
title: "Přidání nebo změna role správce Azure předplatné | Microsoft Docs"
description: "Popisuje, jak přidat nebo změnit společné správce Azure, Správce služby a účet správce"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: genli
ms.openlocfilehash: da5995535d42ed52772cb09e0f4da51bbf878748
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-the-subscription-or-services"></a>Přidání nebo změna role Správce služby Azure, které spravují předplatné nebo služby
Správce Azure, který slouží ke správě vašeho předplatného Azure nebo spravuje služby Azure používá v rámci vašeho předplatného, můžete změnit. K zobrazení informací o Azure fakturace a Správa odběrů, musíte se přihlásit k [centra účtů](https://account.windowsazure.com/Home/Index) jako správce účtu. 

## <a name="add-an-admin-for-a-subscription"></a>Přidat správce pro předplatné
Správce Azure můžete přidat na portálu Azure nebo na portálu Azure classic.

**Azure Portal**

Chcete-li přidat někdo jako správce pro přihlášení k odběru na portálu Azure, můžete jim poskytnout roli vlastníka. Role vlastníka můžete spravovat pouze prostředky v odběru, který jste přiřadili. Nemá oprávnění přístupu pro další odběry. Vlastníci přidáte prostřednictvím [portál Azure](https://portal.azure.com) nemůže spravovat prostředku v [portál Azure classic](https://manage.windowsazure.com).

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra vyberte **předplatné** > *předplatné, které chcete, aby správce pro přístup k*.

    ![Snímek obrazovky zobrazující vybrané předplatné](./media/billing-add-change-azure-subscription-administrator/newselectsub.png)

3. V okně odběru vyberte **přístup k ovládacímu prvku (IAM)**.
4. Vyberte **přidat** > **Role** > **vlastníka**. Zadejte e-mailovou adresu uživatele, které chcete přidat jako vlastník, vyberte uživatele a potom vyberte **Uložit**.

    ![Snímek obrazovky zobrazující vybrané roli vlastníka](./media/billing-add-change-azure-subscription-administrator/add-role.png)

5. Pokud chcete přidat účet vlastníka jako spolusprávce, v **přístup k ovládacímu prvku (IAM)** stránky, klikněte pravým tlačítkem na uživatele a pak vyberte **přidat jako spolusprávce**. Tato funkce je nyní k dispozici na [portál Azure preview](https://preview.portal.azure.com/). 

     ![Snímek obrazovky, který přidá společné správce](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Budete muset přidat uživatele "Vlastník" jako spolusprávce, pokud uživatel potřebuje ke správě služby Azure v [portál Azure classic](https://manage.windowsazure.com/).

    Chcete-li odebrat oprávnění spolusprávcem, klikněte pravým tlačítkem na uživatele "společné správce" a pak vyberte **odebrat spolusprávcem**.

    ![Snímek obrazovky, který odebere společné správce](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)


**Portál Azure Classic**

1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com/).
2. V navigačním podokně vyberte **nastavení**> **správci**> **přidat**. </br>

    ![Snímek obrazovky, který ukazuje, jak získat na tlačítko Přidat](./media/billing-add-change-azure-subscription-administrator/addcoadmin.png)
3. Zadejte e-mailovou adresu osoby, kterou chcete přidat jako spolusprávce a pak vyberte odběr, který chcete společné správce pro přístup.</br>

    ![Snímek obrazovky, který znázorňuje vybrané předplatné ](./media/billing-add-change-azure-subscription-administrator/addcoadmin2.png)</br>

Následující e-mailová adresa se dá přidat jako Spolusprávce:

* **Microsoft Account** (dříve Windows Live ID) </br>
  Account Microsoft můžete použít k přihlášení na všechny produkty společnosti Microsoft orientované příjemce a cloudovým službám, jako je například Outlook (Hotmail), Skype (MSN), OneDrive, Windows Phone a Xbox LIVE.
* **Účtu organizace**</br>
  Účet organizace je účet, který se vytvořil v rámci Azure Active Directory. Adresa účtu organizace má tento formát:

    Uživatel @&lt;doménu&gt;. onmicrosoft.com

## <a name="change-service-administrator-for-a-subscription"></a>Změnit správce služeb pro předplatné
Pouze správce účtu může změnit správce služeb pro předplatné.

1. Přihlaste se k [centra účtů Azure](https://account.windowsazure.com/subscriptions) pomocí účtu správce.
2. Vyberte odběr, který chcete změnit.
3. Na pravé straně, vyberte **upravit předplatné** podrobnosti. </br>

    ![editsub](./media/billing-add-change-azure-subscription-administrator/editsub.png)
4. V **Správce služeb** zadejte e-mailovou adresu z nového správce služby. </br>

    ![changeSA](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

## <a name="change-the-account-administrator"></a>Změnit správce účtu
Převést vlastnictví účet Azure na jiný účet, najdete v části [přenos vlastnictví předplatného Azure](billing-subscription-transfer.md).

Důrazně doporučujeme, odstranit nebo přejmenovat e-mailová adresa správce účtu. Může se zobrazit neočekávané a nežádoucí chování s účet Azure. Nemusí být možné přihlášení do Azure pomocí daného účtu, změnit účet nebo spravovat prostředky pomocí daného účtu. 

## <a name="check-the-account-administrator-of-the-subscription"></a>Zkontrolujte správce účtu předplatného
Pokud si nejste jistí, který správce účtu je pro vaše předplatné, použijte následující kroky a zjistěte.

  1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
  2. V nabídce centra vyberte **předplatné**.
  3. Vyberte předplatné, které chcete kontrolovat, a pak hledejte v části **nastavení**.
  4. Vyberte **vlastnosti**. Správce účtu předplatného se zobrazí v **správce účtu** pole.  

## <a name="types-of-azure-admin-accounts"></a>Typy účtů správce Azure
 Správce účtu, Správce služeb a spolusprávce jsou tři druhy rolí správců v Microsoft Azure. Následující tabulka popisuje rozdíl mezi tyto tři role správce.

| Role pro správu | Omezení | Popis |
| --- | --- | --- |
| Správce účtu (AA) |1 na účet Azure |Toto je osoba, která registraci aplikace nebo koupili předplatných Azure a má oprávnění k přístupu [centra účtů](https://account.windowsazure.com/Home/Index) a provádět různé úlohy správy. Mezi ně patří schopnost vytvářet odběry, zrušit předplatné, změnit fakturace předplatného a změnit správce služeb. |
| Správce služeb (SA) |1 za předplatné Azure |Tato role je oprávnění ke správě služeb v [portál Azure](https://portal.azure.com). Ve výchozím nastavení pro nové předplatné je správce účtu také Správce služeb. |
| Spolusprávce (CA) v [portál Azure classic](https://manage.windowsazure.com) |200 na předplatné |Tato role má stejná přístupová oprávnění jako správce služeb, ale nemůže změnit přidružení předplatných k adresářům Azure. |

Azure na základě Role služby Active Directory řízení přístupu (RBAC) umožňuje uživatelům přidat k více rolím. Další informace najdete v tématu [řízení přístupu na základě Role v Azure Active Directory](../active-directory/role-based-access-control-configure.md).

## <a name="limitations-and-restrictions-for-admin-accounts"></a>Omezení a omezení pro účty správců
* Každé předplatné je spojeno s adresář služby Azure AD (také označované jako výchozí adresář,). Výchozí adresář předplatné je spojeno s, najdete [portál Azure classic](https://manage.windowsazure.com/), vyberte **nastavení** > **odběry**. Zkontrolujte ID předplatného najít výchozí adresář.
* Pokud jste přihlášeni pomocí účtu Microsoft Account, můžete pouze přidat další Accounts Microsoft nebo uživatele v adresáři výchozí jako Spolusprávce.
* Pokud jste přihlášení pomocí účtu organizace, můžete přidat další účty organizace ve vaší organizaci jako Spolusprávce. Například abby@contoso.com můžete přidat bob@contoso.com jako správce služeb nebo společné správce, ale nemůžete přidat john@notcontoso.com Pokud john@notcontoso.com je ve výchozím adresáři. Uživatelé přihlašují účty organizace můžou dál přidat Account Microsoft uživatele jako správce služeb nebo společné správce.
* Teď, když je možné se přihlásit k Azure s účtem organizace, tady jsou požadavky na účet správce služeb a spolusprávce změny:

  | Přihlášení – metoda | Přidat Account Microsoft nebo uživatelům v rámci výchozí adresář jako certifikační Autority nebo SA? | Přidat účet organizace ve stejné organizaci jako certifikační Autority nebo SA? | Přidat účet organizace v jiné organizaci, jako certifikační Autority nebo SA? |
  | --- | --- | --- | --- |
  |  Účet Microsoft |Ano |Ne |Ne |
  |  Účet organizace |Ano |Ano |Ne |

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Další informace o řízení přístupu k prostředkům a služby Active Directory
* Další informace o tom, jak je přístup k prostředkům řídí ve službě Microsoft Azure, najdete v části [Principy přístupu k prostředkům v Azure](../active-directory/active-directory-understanding-resource-access.md).
* Další informace o službě Azure Active Directory najdete v tématu [asociování předplatných Azure se službou Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) a [přiřazení rolí správce v Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.
