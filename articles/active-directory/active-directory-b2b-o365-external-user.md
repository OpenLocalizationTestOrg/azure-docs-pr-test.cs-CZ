---
title: "Externí sdílení Office 365 a Azure Active Directory s B2B spolupráci | Microsoft Docs"
description: "referenční dokumentace pro Azure Active Directory s B2B spolupráce mapování deklarace identity"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: cad0ce8f745f3d6ca14436fd714b08c60de0e459
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="9d8db-103">Externí sdílení Office 365 a spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="9d8db-103">Office 365 external sharing and Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="9d8db-104">Externí sdílení v Office 365 (OneDrive, SharePoint Online, Unified skupin atd.) a spolupráce B2B Azure Active Directory (Azure AD) jsou technicky samé.</span><span class="sxs-lookup"><span data-stu-id="9d8db-104">External sharing in Office 365 (OneDrive, SharePoint Online, Unified Groups, etc.) and Azure Active Directory (Azure AD) B2B collaboration are technically the same thing.</span></span> <span data-ttu-id="9d8db-105">Všechny externí sdílení (kromě OneDrive nebo SharePoint Online), včetně hostů v skupiny Office 365, už používá pozvánku spolupráce Azure AD B2B rozhraní API pro sdílení.</span><span class="sxs-lookup"><span data-stu-id="9d8db-105">All external sharing (except OneDrive/SharePoint Online), including guests in Office 365 Groups, already uses the Azure AD B2B collaboration invitation APIs for sharing.</span></span>

<span data-ttu-id="9d8db-106">OneDrive nebo SharePoint Online má správce samostatné pozvánku.</span><span class="sxs-lookup"><span data-stu-id="9d8db-106">OneDrive/SharePoint Online has a separate invitation manager.</span></span> <span data-ttu-id="9d8db-107">Podpora pro externí sdílení v OneDrive nebo SharePoint Online spuštěna před jeho podporu vyvinul Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9d8db-107">Support for external sharing in OneDrive/SharePoint Online started before Azure AD developed its support.</span></span> <span data-ttu-id="9d8db-108">V čase OneDrive nebo SharePoint Online externí sdílení rozlišeny několik funkcí a mnoha milionů uživatelů, kteří používají produktu je v vytvořená sdílení vzor.</span><span class="sxs-lookup"><span data-stu-id="9d8db-108">Over time, OneDrive/SharePoint Online external sharing has accrued several features and many millions of users who use the product's in-built sharing pattern.</span></span> <span data-ttu-id="9d8db-109">Existují však určité jemně rozdíly mezi OneDrive nebo SharePoint Online externí sdílení jak funguje a jak funguje spolupráce Azure AD B2B:</span><span class="sxs-lookup"><span data-stu-id="9d8db-109">However, there are some subtle differences between how OneDrive/SharePoint Online external sharing works and how Azure AD B2B collaboration works:</span></span>

- <span data-ttu-id="9d8db-110">OneDrive nebo SharePoint Online přidá uživatele do adresáře po uživatelé mají uplatněn svoje pozvánky.</span><span class="sxs-lookup"><span data-stu-id="9d8db-110">OneDrive/SharePoint Online adds users to the directory after users have redeemed their invitations.</span></span> <span data-ttu-id="9d8db-111">Ano před uplatnění, nevidíte uživatele na portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9d8db-111">So, before redemption, you don't see the user in Azure AD portal.</span></span> <span data-ttu-id="9d8db-112">Pokud jiný web vyzývá uživatel do té doby, vygeneruje se nová pozvánka.</span><span class="sxs-lookup"><span data-stu-id="9d8db-112">If another site invites a user in the meantime, a new invitation is generated.</span></span> <span data-ttu-id="9d8db-113">Však při použití spolupráce Azure AD B2B, uživatelé jsou okamžitě přidány na pozvánku, aby zobrazovala až everywhere.</span><span class="sxs-lookup"><span data-stu-id="9d8db-113">However, when you use Azure AD B2B collaboration, users are added immediately on invitation so that they show up everywhere.</span></span>

- <span data-ttu-id="9d8db-114">Uplatnění dojde v OneDrive nebo SharePoint Online vypadá jinak než dojde v spolupráce Azure AD B2B.</span><span class="sxs-lookup"><span data-stu-id="9d8db-114">The redemption experience in OneDrive/SharePoint Online looks different from the experience in Azure AD B2B collaboration.</span></span> <span data-ttu-id="9d8db-115">Jakmile uživatel uplatňuje pozvánku, vypadat podobně zkušenosti.</span><span class="sxs-lookup"><span data-stu-id="9d8db-115">After a user redeems an invitation, the experiences look alike.</span></span>

- <span data-ttu-id="9d8db-116">Azure AD s B2B spolupráce pozvánku, uživatelé mohou být zachyceny z OneDrive nebo SharePoint Online sdílení dialogová okna.</span><span class="sxs-lookup"><span data-stu-id="9d8db-116">Azure AD B2B collaboration invited users can be picked from OneDrive/SharePoint Online sharing dialog boxes.</span></span> <span data-ttu-id="9d8db-117">OneDrive nebo SharePoint Online pozvat uživatele také zobrazí v Azure AD poté, co se uplatnit své pozvánky.</span><span class="sxs-lookup"><span data-stu-id="9d8db-117">OneDrive/SharePoint Online invited users also show up in Azure AD after they redeem their invitations.</span></span>

- <span data-ttu-id="9d8db-118">Chcete-li spravovat externí sdílení v OneDrive nebo SharePoint Online pomocí spolupráce Azure AD B2B, nastavte OneDrive nebo SharePoint Online externí sdílení nastavení **pouze povolit sdílení s externími uživateli již v adresáři**.</span><span class="sxs-lookup"><span data-stu-id="9d8db-118">To manage external sharing in OneDrive/SharePoint Online with Azure AD B2B collaboration, set the OneDrive/SharePoint Online external sharing setting to **Only allow sharing with external users already in the directory**.</span></span> <span data-ttu-id="9d8db-119">Uživatelé přejít na externě sdílené weby a vyberte z externími spolupracovníky přidané správce.</span><span class="sxs-lookup"><span data-stu-id="9d8db-119">Users can go to externally shared sites and pick from external collaborators that the admin has added.</span></span> <span data-ttu-id="9d8db-120">Správce můžete přidat externí spolupracovníky prostřednictvím pozvánku spolupráce B2B rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9d8db-120">The admin can add the external collaborators through the B2B collaboration invitation APIs.</span></span>

![OneDrive nebo SharePoint Online externí sdílení nastavení](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a><span data-ttu-id="9d8db-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d8db-122">Next steps</span></span>

<span data-ttu-id="9d8db-123">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="9d8db-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="9d8db-124">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="9d8db-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="9d8db-125">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9d8db-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="9d8db-126">Přidání uživatele spolupráce B2B k roli</span><span class="sxs-lookup"><span data-stu-id="9d8db-126">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="9d8db-127">Delegovat pozvánek spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9d8db-127">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="9d8db-128">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="9d8db-128">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="9d8db-129">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9d8db-129">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="9d8db-130">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="9d8db-130">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="9d8db-131">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9d8db-131">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="9d8db-132">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="9d8db-132">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="9d8db-133">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9d8db-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
