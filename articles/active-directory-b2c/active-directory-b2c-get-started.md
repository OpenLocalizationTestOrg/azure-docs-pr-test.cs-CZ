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
# <a name="create-an-azure-active-directory-b2c-tenant-in-hello-azure-portal"></a><span data-ttu-id="fe37b-103">Vytvoření klienta Azure Active Directory B2C v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fe37b-103">Create an Azure Active Directory B2C tenant in hello Azure portal</span></span>

<span data-ttu-id="fe37b-104">Upravená Sipi.</span><span class="sxs-lookup"><span data-stu-id="fe37b-104">Edited by Sipi.</span></span>

<span data-ttu-id="fe37b-105">Tento rychlý start vám pomůže vytvořit klienta Microsoft Azure Active Directory (Azure AD) B2C za několik minut.</span><span class="sxs-lookup"><span data-stu-id="fe37b-105">This Quickstart helps you create a Microsoft Azure Active Directory (Azure AD) B2C tenant in just a few minutes.</span></span> <span data-ttu-id="fe37b-106">Jakmile budete hotovi, máte toouse klienta B2C pro registraci aplikace B2C.</span><span class="sxs-lookup"><span data-stu-id="fe37b-106">When you're finished, you have a B2C tenant toouse for registering B2C applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe37b-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fe37b-107">Prerequisites</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-tooazure"></a><span data-ttu-id="fe37b-108">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="fe37b-108">Log in tooAzure</span></span>

<span data-ttu-id="fe37b-109">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fe37b-109">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="fe37b-110">Vytvoření tenanta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="fe37b-110">Create an Azure AD B2C tenant</span></span>

<span data-ttu-id="fe37b-111">Funkce B2C nelze povolit v existující klienty.</span><span class="sxs-lookup"><span data-stu-id="fe37b-111">B2C features can't be enabled in your existing tenants.</span></span> <span data-ttu-id="fe37b-112">Je nutné toocreate klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="fe37b-112">You need toocreate an Azure AD B2C tenant.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

<span data-ttu-id="fe37b-113">Blahopřejeme, jste vytvořili klienta služby Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="fe37b-113">Congratulations, you have created an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="fe37b-114">Jste globálním správcem klienta hello.</span><span class="sxs-lookup"><span data-stu-id="fe37b-114">You are a Global Administrator of hello tenant.</span></span> <span data-ttu-id="fe37b-115">Podle potřeby můžete přidat další globální správce.</span><span class="sxs-lookup"><span data-stu-id="fe37b-115">You can add other Global Administrators as required.</span></span> <span data-ttu-id="fe37b-116">tooswitch tooyour nového klienta, klikněte na tlačítko hello *spravovat odkaz na vaši novou klienta*.</span><span class="sxs-lookup"><span data-stu-id="fe37b-116">tooswitch tooyour new tenant, click hello *manage your new tenant link*.</span></span>

![Spravovat odkaz na vaši nového klienta](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> <span data-ttu-id="fe37b-118">Pokud plánujete toouse klienta B2C u produkční aplikace, přečtěte si článek hello [produkční škálování oproti preview B2C klienty](active-directory-b2c-reference-tenant-type.md).</span><span class="sxs-lookup"><span data-stu-id="fe37b-118">If you are planning toouse a B2C tenant for a production app, read hello article on [production-scale vs. preview B2C tenants](active-directory-b2c-reference-tenant-type.md).</span></span> <span data-ttu-id="fe37b-119">Existují známé problémy při odstranění existující B2C klienta a znovu ji vytvořte s hello stejným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="fe37b-119">There are known issues when you delete an existing B2C tenant and re-create it with hello same domain name.</span></span> <span data-ttu-id="fe37b-120">Je nutné toocreate klienta B2C s jiným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="fe37b-120">You need toocreate a B2C tenant with a different domain name.</span></span>
>
>

## <a name="link-your-tenant-tooyour-subscription"></a><span data-ttu-id="fe37b-121">Odkaz tooyour předplatného klienta</span><span class="sxs-lookup"><span data-stu-id="fe37b-121">Link your tenant tooyour subscription</span></span>

<span data-ttu-id="fe37b-122">Je třeba toolink vaše Azure AD B2C klienta všechny funkce B2C tooenable tooyour předplatného Azure a platit poplatky za používání.</span><span class="sxs-lookup"><span data-stu-id="fe37b-122">You need toolink your Azure AD B2C tenant tooyour Azure subscription tooenable all B2C functionality and pay for usage charges.</span></span> <span data-ttu-id="fe37b-123">toolearn víc, přečtěte si [v tomto článku](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="fe37b-123">toolearn more, read [this article](active-directory-b2c-how-to-enable-billing.md).</span></span> <span data-ttu-id="fe37b-124">Když nemáte propojení vaší tooyour klienta Azure AD B2C předplatné Azure, některé funkce se blokovat a zobrazí se zpráva upozornění ("žádné předplatné propojené toothis B2C klienta nebo hello předplatné vyžaduje pozornost.") v nastavení hello B2C.</span><span class="sxs-lookup"><span data-stu-id="fe37b-124">If you don't link your Azure AD B2C tenant tooyour Azure subscription, some functionality is blocked and, you see a warning message ("No Subscription linked toothis B2C tenant or hello Subscription needs your attention.") in hello B2C settings.</span></span> <span data-ttu-id="fe37b-125">Je důležité provést tento krok, ještě před odesláním aplikace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="fe37b-125">It is important that you take this step before you ship your apps into production.</span></span>

## <a name="easy-access-toosettings"></a><span data-ttu-id="fe37b-126">Snadný přístup toosettings</span><span class="sxs-lookup"><span data-stu-id="fe37b-126">Easy access toosettings</span></span>

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

<span data-ttu-id="fe37b-127">Můžete taky přejít hello okno zadáním `Azure AD B2C` v **vyhledávání prostředků** hello horní části portálu hello.</span><span class="sxs-lookup"><span data-stu-id="fe37b-127">You can also access hello blade by entering `Azure AD B2C` in **Search resources** at hello top of hello portal.</span></span> <span data-ttu-id="fe37b-128">V seznamu výsledků hello vyberte **Azure AD B2C** tooaccess hello okno nastavení B2C.</span><span class="sxs-lookup"><span data-stu-id="fe37b-128">In hello results list, select **Azure AD B2C** tooaccess hello B2C settings blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe37b-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe37b-129">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fe37b-130">B2C aplikaci zaregistrovat do vašeho klienta B2C</span><span class="sxs-lookup"><span data-stu-id="fe37b-130">Register your B2C application in a B2C tenant</span></span>](active-directory-b2c-app-registration.md)
