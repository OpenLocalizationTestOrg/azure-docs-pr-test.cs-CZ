---
title: "aaaHow toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 846c7b3497c8842947c5b406f4362b9e06785b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-saml-based-single-sign-on-tooapplications-in-azure-active-directory"></a>Jak toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory
Při ladění aplikace založené na SAML integrace, je často užitečné toouse nástroje, jako je [Fiddler](http://www.telerik.com/fiddler) toosee hello požadavku a odpovědi SAML hello a hello skutečné SAML token, který je vydán toohello aplikace SAML. Prozkoumáním tokenu SAML hello zajistěte, aby všechny hello atributy, hello uživatelské jméno v předmětu hello SAML a hello vystavitele URI přicházejí prostřednictvím podle očekávání.

![][1]

Hello odpověď z Azure AD, který obsahuje hello SAML token je obvykle hello jeden, který se nachází za přesměrování HTTP 302 z https://login.windows.net a je nakonfigurován odeslané toohello **adresa URL odpovědi** aplikace hello. 

Hello SAML token můžete zobrazit výběrem tohoto řádku a potom výběrem hello **inspektoři > WebForms** kartě v pravém panelu hello. Zde, klikněte pravým tlačítkem na hello **SAMLResponse** hodnotu a vyberte **odeslat tooTextWizard**. Potom vyberte **z formátu Base64** z hello **transformace** nabídky toodecode hello token a zobrazit jeho obsah.

**Poznámka:**: toosee hello obsah této HTTP požadavků, Fiddler můžete být vyzváni tooconfigure dešifrování přenosy HTTPS, které budete potřebovat toodo.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurace jednoho přihlášení tooapplications které nejsou v galerii aplikací Azure Active Directory hello](../active-directory-saas-custom-apps.md)
* [Jak tooCustomize deklarace identity vystavené v hello tokenu SAML pro Pre-Integrated aplikace](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png