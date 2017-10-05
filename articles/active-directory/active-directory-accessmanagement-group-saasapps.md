---
title: "Pomocí skupiny pro správu přístupu k aplikacím SaaS | Microsoft Docs"
description: "Jak používat skupin v Azure Active Directory Premium nebo Basic přiřazení přístupu k aplikacím SaaS, které jsou integrované s Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro;oldportal
ms.openlocfilehash: d350011ee9fc5ced9ddb16993f68d3c840a645a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Použití skupiny ke správě přístupu k aplikacím SaaS
Pomocí Azure Active Directory (Azure AD) s licencí Azure AD Premium nebo Azure AD Basic, můžete použít skupiny přiřazení přístupu k aplikaci SaaS, která je integrovaná s Azure AD. Například pokud chcete přiřadit přístup pro marketingové oddělení používat pět různých aplikace SaaS, je můžete vytvořit skupinu, která obsahuje uživatelé v marketingovém oddělení a potom přidělit této skupině pro těchto pět aplikace SaaS, které jsou vyžadovány z marketingového oddělení. Tímto způsobem můžete ušetřit čas tím Správa členství z marketingového oddělení na jednom místě. Uživatelé pak přiřazené k aplikaci při jsou přidány jako členové skupiny marketing a jejich přiřazení odebrali z aplikace, když jsou odebrány ze skupiny marketing.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. 

Tato funkce slouží se stovkami aplikací, které můžete přidat z v galerii aplikací Azure AD.

**Přiřadit přístup pro skupinu k aplikaci SaaS**

1. V [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory** na navigačním panelu na levé straně.
2. Vyberte **Directory** kartě a pak otevřete adresář, ve které chcete přiřadit přístup pro skupinu k aplikaci SaaS.
3. Vyberte **aplikace** kartě. Vyberte aplikaci, která jste přidali v galerii aplikací a klikněte **uživatelů a skupin** kartě.
4. Na **uživatelů a skupin** ve **počínaje** pole, zadejte název skupiny, ke kterému chcete přiřadit přístup a potom vyberte zaškrtnutí v pravém horním rohu. Stačí zadejte první část název skupiny.
5. Vyberte skupinu, a poté vyberte položku **přiřadit přístup** tlačítko. Vyberte **Ano** až se zobrazí zpráva o potvrzení. Vnořené členství ve skupinách momentálně není podporované v případě přiřazování k aplikacím podle skupiny.
6. Uvidíte taky, kteří uživatelé jsou přiřazeny k aplikaci, buď přímo, nebo členství ve skupině. Chcete-li to provést, změňte **zobrazit rozevírací z "Skupinami"** k **'Všechny uživatele'**. V seznamu jsou uživatelé v adresáři a jestli je každý uživatel přiřazené k aplikaci. V seznamu také ukazuje, jestli přiřazené uživatele přiřazené k aplikaci přímo (zobrazené jako "Direct, typ přiřazení) nebo na základě členství ve skupině (typ přiřazení zobrazí jako 'Zděděné.')

> [!NOTE]
> Na kartě uživatelů a skupin najdete v až poté, co jste povolili Azure AD Premium nebo Azure AD Basic.
>
>

### <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
