---
title: "aaaNext kroků pro správu přístupu pomocí skupin | Microsoft Docs"
description: "Jak rozšířené-pro správu skupin zabezpečení a jak toouse tyto skupiny toomanage přístup tooa prostředků."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 44350a3c-8ea1-4da1-aaac-7fc53933dd21
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4fd55f893290fac3551a130f29bd12709cf551e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-owners-for-a-group"></a>Správa vlastníků pro skupinu
Jakmile vlastník prostředku přiřazena přístup tooa prostředků tooan Azure AD skupiny, hello členství ve skupině hello spravuje vlastník skupiny hello. Hello vlastník prostředku deleguje efektivně hello oprávnění tooassign uživatelé toohello prostředků toohello vlastník skupiny hello.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. 

## <a name="assigning-group-ownership"></a>Přiřazení vlastnictví skupin
**tooadd skupinu tooa vlastníka**

1. V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.
2. Vyberte hello **skupiny** kartu a pak otevřete hello skupinou, kterou chcete tooadd vlastníkům.
3. Vyberte **přidat vlastníky**.
4. Na hello **přidat vlastníky** stránky, vyberte hello uživatele chcete tooadd jako hello vlastník této skupiny a zajistěte, aby toto jméno přidat i toohello **vybrané** podokně.

**tooremove vlastníka ze skupiny**

1. V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.
2. Vyberte hello **skupiny** kartu a pak otevřete hello skupiny, které chcete tooremove vlastníka z.
3. Vyberte hello **vlastníky** kartě.
4. Vyberte hello vlastníka má tooremove z této skupiny a pak vyberte **odebrat**.

## <a name="additional-information"></a>Další informace
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
