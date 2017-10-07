---
title: "aaaAdd nebo změna role správce Azure předplatné | Microsoft Docs"
description: "Popisuje, jak tooadd nebo změňte společné správce Azure, Správce služby a účet správce"
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
ms.openlocfilehash: 14eaecf2dbfd7152775ac3552bf3a7ae3db596b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-hello-subscription-or-services"></a>Přidání nebo změna role Správce služby Azure, které spravují předplatné hello nebo služby
Můžete změnit hello Azure správce, který slouží ke správě vašeho předplatného Azure nebo spravuje hello používá v rámci vašeho předplatného služby Azure. tooview Azure fakturační informace a spravovat odběry, musíte se přihlásit toohello [centra účtů](https://account.windowsazure.com/Home/Index) jako hello správce účtu. 

## <a name="add-an-admin-for-a-subscription"></a>Přidat správce pro předplatné
Správce Azure můžete přidat v hello portál Azure nebo v hello portál Azure classic.

**Azure Portal**

tooadd někdo jako správce pro předplatné za hello portálu Azure, můžete jim poskytnout roli vlastníka hello. role vlastník Hello může spravovat pouze hello prostředků v předplatném hello, který jste přiřadili. Nemá odběry tooother oprávnění přístupu. Hello vlastníky přidáte prostřednictvím hello [portál Azure](https://portal.azure.com) nemůže spravovat prostředku v hello [portál Azure classic](https://manage.windowsazure.com).

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello vyberte **předplatné** > *hello předplatné, které chcete hello správce tooaccess*.

    ![Snímek obrazovky zobrazující vybrané předplatné](./media/billing-add-change-azure-subscription-administrator/newselectsub.png)

3. V okně předplatné hello vyberte **přístup k ovládacímu prvku (IAM)**.
4. Vyberte **přidat** > **Role** > **vlastníka**. Zadejte e-mailovou adresu hello hello uživatele chcete tooadd jako vlastník, vyberte hello uživatele a potom vyberte **Uložit**.

    ![Snímek obrazovky zobrazující vybrané roli vlastníka hello](./media/billing-add-change-azure-subscription-administrator/add-role.png)

5. Pokud chcete účet vlastníka hello tooadd jako spolusprávce v hello **přístup k ovládacímu prvku (IAM)** stránky, klikněte pravým tlačítkem na hello uživatele a pak vyberte **přidat jako spolusprávce**. Tato funkce je nyní k dispozici na [portál Azure preview](https://preview.portal.azure.com/). 

     ![Snímek obrazovky, který přidá společné správce](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Budete potřebovat tooadd hello "Vlastník" uživatele jako spolusprávce Pokud hello uživatel potřebuje toomanage hello služby Azure v [portál Azure classic](https://manage.windowsazure.com/).

    tooremove hello společné správce oprávnění, klikněte pravým tlačítkem na uživatele hello "společné správce" a pak vyberte **odebrat spolusprávcem**.

    ![Snímek obrazovky, který odebere společné správce](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)


**Portál Azure Classic**

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/).
2. V navigačním podokně hello vyberte **nastavení**> **správci**> **přidat**. </br>

    ![Snímek obrazovky, který ukazuje, jak přidat tooget toohello tlačítko](./media/billing-add-change-azure-subscription-administrator/addcoadmin.png)
3. Zadejte e-mailovou adresu hello hello osoby, které chcete používat jako spolusprávce a pak vyberte hello předplatné, které chcete hello spolusprávcem tooaccess tooadd.</br>

    ![Snímek obrazovky, který znázorňuje vybrané předplatné ](./media/billing-add-change-azure-subscription-administrator/addcoadmin2.png)</br>

Následující e-mailovou adresu Hello se dá přidat jako Spolusprávce:

* **Microsoft Account** (dříve Windows Live ID) </br>
  Můžete použít Microsoft Account toosign ve tooall spotřebitelské produkty společnosti Microsoft a cloudovým službám, jako je například Outlook (Hotmail), Skype (MSN), OneDrive, Windows Phone a Xbox LIVE.
* **Účtu organizace**</br>
  Účet organizace je účet, který se vytvořil v rámci Azure Active Directory. Adresa Hello účet organizace má tento formát:

    Uživatel @&lt;doménu&gt;. onmicrosoft.com

## <a name="change-service-administrator-for-a-subscription"></a>Změnit správce služeb pro předplatné
Pouze hello účet správce, můžete změnit hello Správce služeb pro předplatné.

1. Přihlaste se příliš[centra účtů Azure](https://account.windowsazure.com/subscriptions) pomocí hello správce účtu.
2. Vyberte předplatné hello chcete toochange.
3. Na pravé straně hello, vyberte **upravit předplatné** podrobnosti. </br>

    ![editsub](./media/billing-add-change-azure-subscription-administrator/editsub.png)
4. V hello **Správce služeb** zadejte e-mailovou adresu hello hello nového správce služby. </br>

    ![changeSA](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

## <a name="change-hello-account-administrator"></a>Změnit hello správce účtu
tootransfer vlastnictví hello účet tooanother účet Azure, najdete v části [přenos vlastnictví předplatného Azure](billing-subscription-transfer.md).

Důrazně doporučujeme, odstranit nebo přejmenovat e-mailová adresa správce účtu hello. Může se zobrazit neočekávané a nežádoucí chování s hello účet Azure. Nemusí být možné tooAzure přihlášení pomocí daného účtu, změnit účet toohello nebo spravovat prostředky pomocí daného účtu. 

## <a name="check-hello-account-administrator-of-hello-subscription"></a>Zkontrolujte hello správce účtu předplatného hello
Pokud si nejste jistí, který správce účtu hello je pro vaše předplatné, použijte následující kroky toofind out hello.

  1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
  2. V nabídce centra hello vyberte **předplatné**.
  3. Vyberte předplatné hello chcete toocheck a pak hledejte v části **nastavení**.
  4. Vyberte **vlastnosti**. Správce účtu Hello hello předplatného se zobrazí v hello **správce účtu** pole.  

## <a name="types-of-azure-admin-accounts"></a>Typy účtů správce Azure
 Správce účtu, Správce služeb a spolusprávce jsou hello tři druhy rolí správců v Microsoft Azure. Hello následující tabulka popisuje hello rozdíl mezi tyto tři role správce.

| Role pro správu | Omezení | Popis |
| --- | --- | --- |
| Správce účtu (AA) |1 na účet Azure |Toto je hello osobě, která registraci aplikace nebo koupili předplatných Azure a je autorizovaný tooaccess hello [centra účtů](https://account.windowsazure.com/Home/Index) a provádět různé úlohy správy. Tyto patří, je možné toocreate odběry, zrušit předplatné, změňte hello fakturace předplatného a změnit hello Správce služeb. |
| Správce služeb (SA) |1 za předplatné Azure |Tato role je autorizovaný toomanage služeb v hello [portál Azure](https://portal.azure.com). Ve výchozím nastavení pro nové předplatné je hello správce účtu také hello Správce služeb. |
| Spolusprávce (CA) v hello [portál Azure classic](https://manage.windowsazure.com) |200 na předplatné |Tato role má hello stejné jako hello Správce služeb přistupovat ke oprávnění, ale nelze změnit přidružení hello odběry tooAzure adresářů. |

Azure na základě Role služby Active Directory řízení přístupu (RBAC) umožňuje uživatelům toobe přidat toomultiple role. Další informace najdete v tématu [řízení přístupu na základě Role v Azure Active Directory](../active-directory/role-based-access-control-configure.md).

## <a name="limitations-and-restrictions-for-admin-accounts"></a>Omezení a omezení pro účty správců
* Každé předplatné je spojeno s adresář služby Azure AD (také označované jako hello výchozí adresář). toofind hello výchozí adresář hello předplatného je přidružen přejděte toohello [portál Azure classic](https://manage.windowsazure.com/), vyberte **nastavení** > **odběry**. Zkontrolujte hello předplatné ID toofind hello výchozí adresář.
* Pokud jste přihlášeni pomocí účtu Microsoft Account, můžete pouze přidat další Accounts Microsoft nebo uživatelům v rámci hello výchozí adresář jako Spolusprávce.
* Pokud jste přihlášení pomocí účtu organizace, můžete přidat další účty organizace ve vaší organizaci jako Spolusprávce. Například abby@contoso.com můžete přidat bob@contoso.com jako správce služeb nebo společné správce, ale nemůžete přidat john@notcontoso.com Pokud john@notcontoso.com je ve výchozím adresáři. Uživatelé přihlašují účty organizace můžou pokračovat tooadd Account Microsoft uživatele jako správce služeb nebo společné správce.
* Teď, když je možné toosign v tooAzure s účtem organizace, zde jsou hello změny tooService správce a spolusprávce požadavky na účet:

  | Přihlášení – metoda | Přidat Account Microsoft nebo uživatelům v rámci výchozí adresář jako certifikační Autority nebo SA? | Přidat účet organizace v hello stejné organizaci jako certifikační Autority nebo SA? | Přidat účet organizace v jiné organizaci, jako certifikační Autority nebo SA? |
  | --- | --- | --- | --- |
  |  Účet Microsoft |Ano |Ne |Ne |
  |  Účet organizace |Ano |Ano |Ne |

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Další informace o řízení přístupu k prostředkům a služby Active Directory
* toolearn Další informace o tom, jak je přístup k prostředkům řídí ve službě Microsoft Azure, najdete v části [Principy přístupu k prostředkům v Azure](../active-directory/active-directory-understanding-resource-access.md).
* Další informace o službě Azure Active Directory najdete v tématu [asociování předplatných Azure se službou Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) a [přiřazení rolí správce v Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.
