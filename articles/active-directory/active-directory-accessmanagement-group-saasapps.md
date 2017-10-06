---
title: "aaaUsing tooSaaS přístup toomanage skupiny aplikací | Microsoft Docs"
description: "Jak toouse skupin v Azure Active Directory Premium nebo Basic tooassign přistupovat k tooSaaS aplikace, které jsou integrované s Azure Active Directory."
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
ms.openlocfilehash: f45ea4472b3d88e8ea514af3db31b4cc9ea68d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-group-toomanage-access-toosaas-applications"></a>Pomocí skupiny toomanage přístup k tooSaaS aplikacím
Pomocí Azure Active Directory (Azure AD) s licencí Azure AD Premium nebo Azure AD Basic, můžete použít skupiny tooassign přístup tooa SaaS aplikace, která je integrovaná se službou Azure AD. Například pokud chcete přístup tooassign pro hello marketingového oddělení toouse pět různých aplikací SaaS, můžete vytvořit skupinu, která obsahuje hello uživatelé v marketingovém oddělení hello a pak mu přiřaďte tuto skupinu toothese pět SaaS aplikace, které jsou potřebuje hello marketingového oddělení. Tímto způsobem můžete ušetřit čas tím Správa členství hello hello marketingové oddělení na jednom místě. Uživatelé pak jsou přiřazeni toohello aplikace když budou přidány jako členové skupiny marketing hello, a jejich přiřazení odebrali z aplikace hello, když jsou odebrány z hello marketingová skupina.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. 

Tato funkce slouží se stovkami aplikací, které můžete přidat z v rámci hello galerii aplikací Azure AD.

**tooassign přístup pro skupiny tooa aplikace SaaS**

1. V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory** na hello navigačním panelu na levé straně hello.
2. Vyberte hello **Directory** kartu a pak otevřete hello adresář, ve kterém má tooassign přístup pro skupiny tooa aplikaci SaaS.
3. Vyberte hello **aplikace** kartě. Vyberte aplikaci, která jste přidali z hello galerii aplikací a pak klikněte na hello **uživatelů a skupin** kartě.
4. Na hello **uživatelů a skupin** na kartě hello **počínaje** pole, zadejte název hello toowhich hello skupiny, které má tooassign přístup, a pak vyberte hello zaškrtnutí v pravé horní části hello. Potřebujete jenom tootype hello první část název skupiny.
5. Vyberte skupiny hello a pak vyberte hello **přiřadit přístup** tlačítko. Vyberte **Ano** až se zobrazí potvrzovací zpráva hello. Vnořené členství ve skupinách nejsou podporovány pro přiřazení na základě skupiny tooapplications v tuto chvíli.
6. Můžete také zjistit, uživatelů, kteří jsou přiřazeny toohello aplikace, buď přímo, nebo členství ve skupině. toodo se změna hello **zobrazit rozevírací z "Skupinami"** příliš**'Všechny uživatele'**. Hello zobrazuje uživatele v adresáři hello a jestli je každý uživatel přiřazen toohello aplikace. seznam Hello také ukazuje, jestli uživatelé hello přiřazené přiřazené přímo toohello aplikaci (zobrazené jako "Direct, typ přiřazení), nebo na základě členství ve skupině (typ přiřazení zobrazí jako 'Zděděné.')

> [!NOTE]
> Až poté, co jste povolili Azure AD Premium nebo Azure AD Basic, můžete zobrazit hello uživatelé a skupiny.
>
>

### <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
