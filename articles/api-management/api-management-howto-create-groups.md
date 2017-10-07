---
title: "aaaManage vývojářským účtům používání skupin v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak účtů toomanage vývojáře pomocí skupin v Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c46e010e41d9705ae161dcd60d734a76d19c9e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-groups-toomanage-developer-accounts-in-azure-api-management"></a>Jak toocreate a používání skupin toomanage vývojáře účty v Azure API Management
Skupiny ve službě API Management jsou použité toomanage hello viditelnost toodevelopers produkty. Produkty jsou první učiněna viditelné toogroups, a pak mohou vývojáři v těchto skupinách zobrazovat a odebírat toohello produkty, které jsou spojeny se skupinami hello. 

API Management má následující neměnné systémové skupiny hello.

* **Správci** – členy této skupiny jsou správci předplatného Azure. Správci spravují instance služby API Management, vytváření hello rozhraní API, operace a produkty, které používají vývojáři.
* **Vývojáři** – do této skupiny patří ověření uživatelé portálu pro vývojáře. Vývojáři jsou zákazníci hello, kteří vytvářejí aplikace pomocí vašich rozhraní API. Vývojáři mají přístup k portálu pro vývojáře toohello a vytvářet aplikace, které volají operace rozhraní API hello.
* **Hosté** -neověření uživatelé portálu pro vývojáře, například potenciální zákazníci, kteří navštěvují portál pro vývojáře hello patří instance API Management do této skupiny. Je možné udělit určité jen pro čtení přístup, jako je například hello možnost tooview rozhraní API, ale není volat.

V přidání toothese systémových skupin můžou správci vytvářet vlastní skupiny nebo [využívat externí skupiny v přidružených klientech služby Azure Active Directory][leverage external groups in associated Azure Active Directory tenants]. Vlastní a externí skupiny můžete používat společně se systémovými skupinami, která poskytuje vývojářům viditelnost a přístup k tooAPI produkty. Například může vytvořit jednu vlastní skupinu pro vývojáře spojené s konkrétní partnerské organizaci účty a povolit jim přístup toohello rozhraní API z produktu, který obsahuje jenom příslušná rozhraní API. Uživatel může být členem několika skupin.

Tato příručka ukazuje, jak přidat nové skupiny a přiřadit je k produktů a vývojáři správci instance API Management.

> [!NOTE]
> Kromě toho toocreating a Správa skupin v hello portálu vydavatele, můžete vytvořit a spravovat vaše skupiny pomocí rozhraní API REST API správy hello [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.
> 
> 

## <a name="create-group"></a>Vytvořit skupinu
toocreate novou skupinu, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management. Tím přejdete portál vydavatele toohello API Management.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

Klikněte na tlačítko **skupiny** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **přidat skupinu**.

![Přidat nové skupiny][api-management-add-group]

Zadejte jedinečný název pro skupinu hello a volitelný popis a klikněte na tlačítko **Uložit**.

![Přidat nové skupiny][api-management-add-group-window]

Hello nové skupiny se zobrazí v hello skupiny kartě tooedit hello **název** nebo **popis** hello skupiny, klikněte na název hello hello skupiny v seznamu hello. toodelete hello skupiny, klikněte na tlačítko **odstranit**.

![Přidat skupinu][api-management-new-group]

Teď, když hello skupina vytvořená, může být související s produkty a vývojáři.

## <a name="associate-group-product"></a>Spojit skupinu s produktem
Klikněte na tlačítko tooassociate skupinu s produktem, **produkty** z hello **API Management** nabídce hello left a potom klikněte na název hello požadované produktu hello.

![Nastavit viditelnost][api-management-add-group-to-product]

Vyberte hello **viditelnost** kartě tooadd a odebrat skupiny a tooview hello aktuální skupiny hello produktu. tooadd nebo odebrat skupiny, zaškrtněte nebo zrušte zaškrtnutí políček hello pro potřeby hello skupiny a klikněte na tlačítko **Uložit**.

![Nastavit viditelnost][api-management-add-group-to-product-visibility]

> [!NOTE]
> tooadd skupiny Azure Active Directory, najdete v části [jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory v Azure API Management](api-management-howto-aad.md).
> 
> tooconfigure skupiny z hello **viditelnost** pro produkt, klikněte na **spravovat skupiny**.
> 
> 

Jakmile produkt je přidružena ke skupině, vývojáři v této skupině můžou zobrazovat a odebírat toohello produktu.

## <a name="associate-group-developer"></a>Přidružení skupin k vývojářům
tooassociate skupin s vývojáři, klikněte na **uživatelé** z hello **API Management** nabídky na levé hello a poté hello políčko vedle položky hello vývojáři chcete tooassociate se skupinou.

![Přidat toogroup vývojáře][api-management-add-group-to-developer]

Po hello potřeby, se kontroluje vývojáři, klikněte na požadovanou skupinu hello v hello **přidat tooGroup** rozevíracího seznamu. Vývojářům můžete odebrat ze skupiny pomocí hello **odebrat ze skupiny** rozevíracího seznamu. 

![Vývojáři][api-management-add-group-to-developer-saved]

Po přidání přidružení hello mezi hello developer a hello skupiny, můžete ji zobrazit v hello **uživatelé** kartě.

## <a name="next-steps"></a>Další kroky
* Po přidání skupiny tooa vývojář se můžou zobrazovat a odebírat produkty toohello přidružené k této skupině. Další informace najdete v tématu [vytvoření a publikování produktu v Azure API Management][How create and publish a product in Azure API Management],
* Kromě toho toocreating a Správa skupin v hello portálu vydavatele, můžete vytvořit a spravovat vaše skupiny pomocí rozhraní API REST API správy hello [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
