---
title: "Vytvoření klienta Azure Active Directory B2C | Microsoft Docs"
description: "Téma o tom, jak vytvořit klienta Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: patricka
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/07/2017
ms.author: parja
ms.openlocfilehash: afca6cf8f19c9b96de292881582e27133e35f096
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-the-azure-portal"></a>Vytvoření klienta Azure Active Directory B2C na portálu Azure

Tento rychlý start vám pomůže vytvořit klienta Microsoft Azure Active Directory (Azure AD) B2C za několik minut. Jakmile budete hotovi, máte klienta B2C (označované taky jako adresář) používat pro registraci aplikace B2C.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Vytvoření tenanta Azure AD B2C

Funkce B2C nelze povolit v existující klienty. Budete muset vytvořit klienta Azure AD B2C.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Blahopřejeme, jste vytvořili klienta služby Azure Active Directory B2C. Jste globální správce klienta. Podle potřeby můžete přidat další globální správce. Chcete-li přepnout do nového klienta, klikněte na tlačítko *spravovat odkaz na vaši novou klienta*.

![Spravovat odkaz na vaši nového klienta](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Pokud máte v úmyslu použít klienta B2C u produkční aplikace, najdete v článku [produkční škálování oproti preview B2C klienty](active-directory-b2c-reference-tenant-type.md). Existují známé problémy při odstranění existujícího klienta B2C a znovu ho vytvořte se stejným názvem domény. Budete muset vytvořit klienta B2C s jiným názvem domény.
>
>

## <a name="link-your-tenant-to-your-subscription"></a>Odkaz vašeho klienta do vašeho předplatného

Potřebujete připojit k předplatnému Azure povolit všechny funkce B2C a platit poplatky za používání klienta služby Azure AD B2C. Další informace, přečtěte si [v tomto článku](active-directory-b2c-how-to-enable-billing.md). Pokud jste k předplatnému Azure není váš klient Azure AD B2C, některé funkce se blokovat a zobrazí se zpráva upozornění ("žádné předplatné odkaz na tohoto klienta B2C nebo předplatné potřebám pozornost.") v nastavení B2C. Je důležité provést tento krok, ještě před odesláním aplikace do produkčního prostředí.

## <a name="easy-access-to-settings"></a>Snadný přístup k nastavení

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

Můžete taky přejít v okně zadáním `Azure AD B2C` v **vyhledávání prostředků** v horní části portálu. V seznamu výsledků vyberte **Azure AD B2C** přístup v okně Nastavení B2C.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [B2C aplikaci zaregistrovat do vašeho klienta B2C](active-directory-b2c-app-registration.md)