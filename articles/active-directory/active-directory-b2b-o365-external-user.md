---
title: "aaaOffice 365 externí sdílení a spolupráce Azure Active Directory s B2B | Microsoft Docs"
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
ms.openlocfilehash: 60452b27b328453eda729bd839c982b479cb6f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="a2701-103">Externí sdílení Office 365 a spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="a2701-103">Office 365 external sharing and Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="a2701-104">Externí sdílení v Office 365 (OneDrive, SharePoint Online, Unified skupin atd.) a Azure Active Directory (Azure AD) B2B spolupráce jsou technicky hello stejné věcí.</span><span class="sxs-lookup"><span data-stu-id="a2701-104">External sharing in Office 365 (OneDrive, SharePoint Online, Unified Groups, etc.) and Azure Active Directory (Azure AD) B2B collaboration are technically hello same thing.</span></span> <span data-ttu-id="a2701-105">Všechny externí sdílení (kromě OneDrive nebo SharePoint Online), včetně hostů v skupiny Office 365, už používá pozvánku spolupráce Azure AD B2B hello rozhraní API pro sdílení.</span><span class="sxs-lookup"><span data-stu-id="a2701-105">All external sharing (except OneDrive/SharePoint Online), including guests in Office 365 Groups, already uses hello Azure AD B2B collaboration invitation APIs for sharing.</span></span>

<span data-ttu-id="a2701-106">OneDrive nebo SharePoint Online má správce samostatné pozvánku.</span><span class="sxs-lookup"><span data-stu-id="a2701-106">OneDrive/SharePoint Online has a separate invitation manager.</span></span> <span data-ttu-id="a2701-107">Podpora pro externí sdílení v OneDrive nebo SharePoint Online spuštěna před jeho podporu vyvinul Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a2701-107">Support for external sharing in OneDrive/SharePoint Online started before Azure AD developed its support.</span></span> <span data-ttu-id="a2701-108">V čase OneDrive nebo SharePoint Online externí sdílení rozlišeny několik funkcí a mnoha milionů uživatelů, kteří používají hello produkt je v vytvořená sdílení vzor.</span><span class="sxs-lookup"><span data-stu-id="a2701-108">Over time, OneDrive/SharePoint Online external sharing has accrued several features and many millions of users who use hello product's in-built sharing pattern.</span></span> <span data-ttu-id="a2701-109">Existují však určité jemně rozdíly mezi OneDrive nebo SharePoint Online externí sdílení jak funguje a jak funguje spolupráce Azure AD B2B:</span><span class="sxs-lookup"><span data-stu-id="a2701-109">However, there are some subtle differences between how OneDrive/SharePoint Online external sharing works and how Azure AD B2B collaboration works:</span></span>

- <span data-ttu-id="a2701-110">OneDrive nebo SharePoint Online přidá uživatele toohello adresář po uživatelé mají uplatněn svoje pozvánky.</span><span class="sxs-lookup"><span data-stu-id="a2701-110">OneDrive/SharePoint Online adds users toohello directory after users have redeemed their invitations.</span></span> <span data-ttu-id="a2701-111">Ano před uplatnění, nevidíte hello uživatele na portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a2701-111">So, before redemption, you don't see hello user in Azure AD portal.</span></span> <span data-ttu-id="a2701-112">Pokud jiný web vyzývá uživatele v hello té doby, vygeneruje se nová pozvánka.</span><span class="sxs-lookup"><span data-stu-id="a2701-112">If another site invites a user in hello meantime, a new invitation is generated.</span></span> <span data-ttu-id="a2701-113">Však při použití spolupráce Azure AD B2B, uživatelé jsou okamžitě přidány na pozvánku, aby zobrazovala až everywhere.</span><span class="sxs-lookup"><span data-stu-id="a2701-113">However, when you use Azure AD B2B collaboration, users are added immediately on invitation so that they show up everywhere.</span></span>

- <span data-ttu-id="a2701-114">Hello uplatnění prostředí na OneDrive nebo SharePoint Online vypadá jinak než hello prostředí v rámci spolupráce Azure AD B2B.</span><span class="sxs-lookup"><span data-stu-id="a2701-114">hello redemption experience in OneDrive/SharePoint Online looks different from hello experience in Azure AD B2B collaboration.</span></span> <span data-ttu-id="a2701-115">Jakmile uživatel uplatňuje pozvánku, vypadat podobně hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="a2701-115">After a user redeems an invitation, hello experiences look alike.</span></span>

- <span data-ttu-id="a2701-116">Azure AD s B2B spolupráce pozvánku, uživatelé mohou být zachyceny z OneDrive nebo SharePoint Online sdílení dialogová okna.</span><span class="sxs-lookup"><span data-stu-id="a2701-116">Azure AD B2B collaboration invited users can be picked from OneDrive/SharePoint Online sharing dialog boxes.</span></span> <span data-ttu-id="a2701-117">OneDrive nebo SharePoint Online pozvat uživatele také zobrazí v Azure AD poté, co se uplatnit své pozvánky.</span><span class="sxs-lookup"><span data-stu-id="a2701-117">OneDrive/SharePoint Online invited users also show up in Azure AD after they redeem their invitations.</span></span>

- <span data-ttu-id="a2701-118">toomanage externí sdílení v OneDrive nebo SharePoint Online pomocí spolupráce Azure AD B2B nastavit hello OneDrive nebo SharePoint Online externí sdílení nastavení příliš**pouze povolit sdílení s externími uživateli již v adresáři hello**.</span><span class="sxs-lookup"><span data-stu-id="a2701-118">toomanage external sharing in OneDrive/SharePoint Online with Azure AD B2B collaboration, set hello OneDrive/SharePoint Online external sharing setting too**Only allow sharing with external users already in hello directory**.</span></span> <span data-ttu-id="a2701-119">Mohou uživatelé tooexternally sdílené lokality a vybrat z externími spolupracovníky, které hello Správce přidal.</span><span class="sxs-lookup"><span data-stu-id="a2701-119">Users can go tooexternally shared sites and pick from external collaborators that hello admin has added.</span></span> <span data-ttu-id="a2701-120">Dobrý den, správce můžete přidat hello externími spolupracovníky prostřednictvím hello pozvánku spolupráce B2B rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2701-120">hello admin can add hello external collaborators through hello B2B collaboration invitation APIs.</span></span>

![Hello OneDrive nebo SharePoint Online externí sdílení nastavení](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a><span data-ttu-id="a2701-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2701-122">Next steps</span></span>

<span data-ttu-id="a2701-123">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="a2701-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="a2701-124">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="a2701-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="a2701-125">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="a2701-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="a2701-126">Přidání role uživatele tooa spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="a2701-126">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="a2701-127">Delegovat pozvánek spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="a2701-127">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="a2701-128">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="a2701-128">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="a2701-129">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a2701-129">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="a2701-130">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="a2701-130">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="a2701-131">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="a2701-131">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="a2701-132">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="a2701-132">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="a2701-133">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="a2701-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
