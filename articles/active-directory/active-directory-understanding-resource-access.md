---
title: "aaaUnderstanding přístup k prostředkům v Azure | Microsoft Docs"
description: "Toto téma vysvětluje koncepty o používání správce předplatného, toocontrol přístupu k prostředkům v hello plné verzi portálu Azure"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 06b9c4166bdea849faae67cae3146c6c278deb97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-resource-access-in-azure"></a>Principy přístupu k prostředkům v Azure
> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. poskytuje technologie Hello portál Azure [řízení přístupu na základě role](role-based-access-control-configure.md) , přesněji můžete spravovat prostředky Azure.
> 
> 

V říjnu 2013 hello portál Azure classic a rozhraní API pro správu služby byly integraci s Azure Active Directory v pořadí toolay hello základ pro zlepšení hello uživatelské prostředí pro správu přístup k prostředkům tooAzure. Azure Active Directory již poskytuje skvělé funkcí, jako je Správa uživatelů, synchronizace adresářů na místě, vícefaktorového ověřování a řízení přístupu aplikace. Samozřejmě to má také být k dispozici pro správu prostředků Azure plošným.

Spustí se z hlediska fakturační řízení přístupu v Azure. Hello vlastníka účtu Azure, získat přístup, když přejdete hello [centra účtů Azure](https://account.windowsazure.com/subscriptions), je hello správce účtu (AA). Předplatné je kontejner pro fakturaci, ale také slouží jako hranice zabezpečení: každého předplatného se službu správce (SA), který můžete přidat, odebrat a úpravy prostředků Azure v tomto předplatném pomocí hello [portál Azure classic](https://manage.windowsazure.com/). Výchozí SA Hello nového předplatného je hello AA, ale hello AA můžete změnit hello SA v hello centra účtů Azure.

<br><br>![Účty Azure][1]

Odběry mají také přidružení s adresářem. Hello directory definuje sadu uživatelů. Může jít o uživatele z hello práci nebo ve škole, který vytvořil adresář hello nebo může se jednat o externí uživatele (to znamená, Accounts Microsoft). Odběry jsou přístupné pro podmnožinu těchto directory uživatelů, kteří mají přiřazený jako služba správce nebo Spolusprávce (CA); Hello pouze výjimkou je, že starší verze důvodů Accounts Microsoft (dříve Windows Live ID) může být přiřazen jako SA nebo certifikační Autority bez se nachází v adresáři hello.

<br><br>![Řízení přístupu v Azure][2]

Funkce v rámci hello portál Azure classic umožňuje SAs, které jsou podepsané pomocí Account Microsoft toochange hello adresáře, který je přidružen předplatné pomocí hello **upravit adresář** příkaz na hello **Odběry** stránky v **nastavení**. Všimněte si, že tato operace má dopad na řízení přístupu hello daného předplatného.

> [!NOTE]
> Hello **upravit adresář** příkaz v hello Azure classic portál není k dispozici toousers, kteří jsou přihlášení pomocí pracovního nebo školního účtu, protože tyto účty můžete přihlásit pouze toohello directory toowhich patří.
> 
> 

<br><br>![Tok přihlášení jednoduchá uživatele][3]

V případě jednoduchého hello bude organizace (například Contoso) vynutit fakturace a řízení přístupu v hello stejné sady předplatných. To znamená adresář hello je přidružené toosubscriptions, které jsou vlastněny jeden účet Azure. Po úspěšném přihlášení toohello portál Azure classic se uživatelům zobrazí dvě kolekce prostředků (znázorněný v oranžová v předchozí ilustraci hello):

* Adresáře, kde existuje uživatelského účtu (jako zdroj nebo přidat jako cizí objekt zabezpečení). Všimněte si, že tento adresář hello používá pro přihlášení není relevantní toothis výpočtů, tak adresářů se vždy zobrazí bez ohledu na to, kde jste se přihlásili.
* Prostředky, které jsou součástí odběry, které jsou přidružené k adresářem hello používá pro přihlášení a který hello uživatelů mají přístup k (kde se jedná SA nebo certifikační Autority).

<br><br>![Uživatel s více předplatnými a adresáři][4]

Pomocí filtru hello předplatné mít uživatelé s odběry v několika adresářích hello možnost tooswitch hello aktuální kontext hello portál Azure classic. V části hello zahrnuje výsledek v jiném adresáři tooa samostatné přihlášení, ale toho dosahuje bezproblémově pomocí jednotného přihlašování (SSO).

Operace, například přesun prostředků mezi předplatnými může být obtížnější v důsledku toto zobrazení jednoho adresářového předplatných. tooperform hello prostředků přenosu, bude pravděpodobně nutné použít toofirst hello **upravit adresář** příkaz na stránce předplatných hello **nastavení** tooassociate hello odběry toohello stejný adresář .

## <a name="next-steps"></a>Další kroky
* Další informace o toolearn jak správci toochange pro předplatné Azure, najdete v části [jak tooadd nebo změna role Správce služby Azure](../billing/billing-add-change-azure-subscription-administrator.md)
* Další informace o tom, jak Azure Active Directory má vztah tooyour předplatné Azure, najdete v části [asociování předplatných Azure se službou Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* Další informace o tom, tooassign role ve službě Azure AD, najdete v části [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png
