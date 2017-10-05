---
title: "Azure Active Directory B2C: Vytvoření klienta Azure Active Directory B2C | Microsoft Docs"
description: "Téma o tom, jak vytvořit klienta Azure Active Directory B2C"
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
ms.openlocfilehash: 1a7eb94e3c74aa0dc187a6d203ba0cf885b97c4d
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-the-azure-portal"></a><span data-ttu-id="ec932-103">Vytvoření klienta Azure Active Directory B2C na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ec932-103">Create an Azure Active Directory B2C tenant in the Azure portal</span></span>

<span data-ttu-id="ec932-104">Upravená Sipi.</span><span class="sxs-lookup"><span data-stu-id="ec932-104">Edited by Sipi.</span></span>

<span data-ttu-id="ec932-105">Tento rychlý start vám pomůže vytvořit klienta Microsoft Azure Active Directory (Azure AD) B2C za několik minut.</span><span class="sxs-lookup"><span data-stu-id="ec932-105">This Quickstart helps you create a Microsoft Azure Active Directory (Azure AD) B2C tenant in just a few minutes.</span></span> <span data-ttu-id="ec932-106">Jakmile budete hotovi, máte použít pro registraci aplikace B2C klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="ec932-106">When you're finished, you have a B2C tenant to use for registering B2C applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec932-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec932-107">Prerequisites</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-to-azure"></a><span data-ttu-id="ec932-108">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="ec932-108">Log in to Azure</span></span>

<span data-ttu-id="ec932-109">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ec932-109">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="ec932-110">Vytvoření tenanta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="ec932-110">Create an Azure AD B2C tenant</span></span>

<span data-ttu-id="ec932-111">Funkce B2C nelze povolit v existující klienty.</span><span class="sxs-lookup"><span data-stu-id="ec932-111">B2C features can't be enabled in your existing tenants.</span></span> <span data-ttu-id="ec932-112">Budete muset vytvořit klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ec932-112">You need to create an Azure AD B2C tenant.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

<span data-ttu-id="ec932-113">Blahopřejeme, jste vytvořili klienta služby Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="ec932-113">Congratulations, you have created an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="ec932-114">Jste globální správce klienta.</span><span class="sxs-lookup"><span data-stu-id="ec932-114">You are a Global Administrator of the tenant.</span></span> <span data-ttu-id="ec932-115">Podle potřeby můžete přidat další globální správce.</span><span class="sxs-lookup"><span data-stu-id="ec932-115">You can add other Global Administrators as required.</span></span> <span data-ttu-id="ec932-116">Chcete-li přepnout do nového klienta, klikněte na tlačítko *spravovat odkaz na vaši novou klienta*.</span><span class="sxs-lookup"><span data-stu-id="ec932-116">To switch to your new tenant, click the *manage your new tenant link*.</span></span>

![Spravovat odkaz na vaši nového klienta](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> <span data-ttu-id="ec932-118">Pokud máte v úmyslu použít klienta B2C u produkční aplikace, najdete v článku [produkční škálování oproti preview B2C klienty](active-directory-b2c-reference-tenant-type.md).</span><span class="sxs-lookup"><span data-stu-id="ec932-118">If you are planning to use a B2C tenant for a production app, read the article on [production-scale vs. preview B2C tenants](active-directory-b2c-reference-tenant-type.md).</span></span> <span data-ttu-id="ec932-119">Existují známé problémy při odstranění existujícího klienta B2C a znovu ho vytvořte se stejným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="ec932-119">There are known issues when you delete an existing B2C tenant and re-create it with the same domain name.</span></span> <span data-ttu-id="ec932-120">Budete muset vytvořit klienta B2C s jiným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="ec932-120">You need to create a B2C tenant with a different domain name.</span></span>
>
>

## <a name="link-your-tenant-to-your-subscription"></a><span data-ttu-id="ec932-121">Odkaz vašeho klienta do vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="ec932-121">Link your tenant to your subscription</span></span>

<span data-ttu-id="ec932-122">Potřebujete připojit k předplatnému Azure povolit všechny funkce B2C a platit poplatky za používání klienta služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ec932-122">You need to link your Azure AD B2C tenant to your Azure subscription to enable all B2C functionality and pay for usage charges.</span></span> <span data-ttu-id="ec932-123">Další informace, přečtěte si [v tomto článku](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="ec932-123">To learn more, read [this article](active-directory-b2c-how-to-enable-billing.md).</span></span> <span data-ttu-id="ec932-124">Pokud jste k předplatnému Azure není váš klient Azure AD B2C, některé funkce se blokovat a zobrazí se zpráva upozornění ("žádné předplatné odkaz na tohoto klienta B2C nebo předplatné potřebám pozornost.") v nastavení B2C.</span><span class="sxs-lookup"><span data-stu-id="ec932-124">If you don't link your Azure AD B2C tenant to your Azure subscription, some functionality is blocked and, you see a warning message ("No Subscription linked to this B2C tenant or the Subscription needs your attention.") in the B2C settings.</span></span> <span data-ttu-id="ec932-125">Je důležité provést tento krok, ještě před odesláním aplikace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec932-125">It is important that you take this step before you ship your apps into production.</span></span>

## <a name="easy-access-to-settings"></a><span data-ttu-id="ec932-126">Snadný přístup k nastavení</span><span class="sxs-lookup"><span data-stu-id="ec932-126">Easy access to settings</span></span>

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

<span data-ttu-id="ec932-127">Můžete taky přejít v okně zadáním `Azure AD B2C` v **vyhledávání prostředků** v horní části portálu.</span><span class="sxs-lookup"><span data-stu-id="ec932-127">You can also access the blade by entering `Azure AD B2C` in **Search resources** at the top of the portal.</span></span> <span data-ttu-id="ec932-128">V seznamu výsledků vyberte **Azure AD B2C** přístup v okně Nastavení B2C.</span><span class="sxs-lookup"><span data-stu-id="ec932-128">In the results list, select **Azure AD B2C** to access the B2C settings blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec932-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec932-129">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ec932-130">B2C aplikaci zaregistrovat do vašeho klienta B2C</span><span class="sxs-lookup"><span data-stu-id="ec932-130">Register your B2C application in a B2C tenant</span></span>](active-directory-b2c-app-registration.md)
