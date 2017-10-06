---
title: "Azure Active Directory B2C: Vytvoření klienta Azure Active Directory B2C | Microsoft Docs"
description: "Téma o tom, jak toocreate Azure Active Directory B2C klienta"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: patricka
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/07/2017
ms.author: swkrish
ms.openlocfilehash: e8b257d66c1f66ffb84f5d3d21b30b42eddcbac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-hello-azure-portal"></a>Vytvoření klienta Azure Active Directory B2C v hello portálu Azure

Upravená Sipi.

Tento rychlý start vám pomůže vytvořit klienta Microsoft Azure Active Directory (Azure AD) B2C za několik minut. Jakmile budete hotovi, máte toouse klienta B2C pro registraci aplikace B2C.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Přihlaste se toohello [portál Azure](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Vytvoření tenanta Azure AD B2C

Funkce B2C nelze povolit v existující klienty. Je nutné toocreate klienta Azure AD B2C.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Blahopřejeme, jste vytvořili klienta služby Azure Active Directory B2C. Jste globálním správcem klienta hello. Podle potřeby můžete přidat další globální správce. tooswitch tooyour nového klienta, klikněte na tlačítko hello *spravovat odkaz na vaši novou klienta*.

![Spravovat odkaz na vaši nového klienta](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Pokud plánujete toouse klienta B2C u produkční aplikace, přečtěte si článek hello [produkční škálování oproti preview B2C klienty](active-directory-b2c-reference-tenant-type.md). Existují známé problémy při odstranění existující B2C klienta a znovu ji vytvořte s hello stejným názvem domény. Je nutné toocreate klienta B2C s jiným názvem domény.
>
>

## <a name="link-your-tenant-tooyour-subscription"></a>Odkaz tooyour předplatného klienta

Je třeba toolink vaše Azure AD B2C klienta všechny funkce B2C tooenable tooyour předplatného Azure a platit poplatky za používání. toolearn víc, přečtěte si [v tomto článku](active-directory-b2c-how-to-enable-billing.md). Když nemáte propojení vaší tooyour klienta Azure AD B2C předplatné Azure, některé funkce se blokovat a zobrazí se zpráva upozornění ("žádné předplatné propojené toothis B2C klienta nebo hello předplatné vyžaduje pozornost.") v nastavení hello B2C. Je důležité provést tento krok, ještě před odesláním aplikace do produkčního prostředí.

## <a name="easy-access-toosettings"></a>Snadný přístup toosettings

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

Můžete taky přejít hello okno zadáním `Azure AD B2C` v **vyhledávání prostředků** hello horní části portálu hello. V seznamu výsledků hello vyberte **Azure AD B2C** tooaccess hello okno nastavení B2C.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [B2C aplikaci zaregistrovat do vašeho klienta B2C](active-directory-b2c-app-registration.md)
