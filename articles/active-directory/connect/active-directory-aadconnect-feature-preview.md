---
title: 'Azure AD Connect: Funkce ve verzi preview | Microsoft Docs'
description: "Toto téma popisuje v další podrobnosti o funkcích, které jsou ve verzi preview v Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: cbf8f729d0ebfb271bb0d8702ac043442b42c262
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="more-details-about-features-in-preview"></a><span data-ttu-id="a903d-103">Další informace o funkcích ve verzi preview</span><span class="sxs-lookup"><span data-stu-id="a903d-103">More details about features in preview</span></span>
<span data-ttu-id="a903d-104">Toto téma popisuje, jak používat funkce, které jsou aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="a903d-104">This topic describes how to use features currently in preview.</span></span>

## <a name="group-writeback"></a><span data-ttu-id="a903d-105">Zpětný zápis skupin</span><span class="sxs-lookup"><span data-stu-id="a903d-105">Group writeback</span></span>
<span data-ttu-id="a903d-106">Možnosti pro zpětný zápis skupin v volitelné funkce umožňuje zpětný zápis **skupiny Office 365** do doménové struktury s Exchange nainstalované.</span><span class="sxs-lookup"><span data-stu-id="a903d-106">The option for group writeback in optional features allows you to writeback **Office 365 Groups** to a forest with Exchange installed.</span></span> <span data-ttu-id="a903d-107">Toto je skupinu, která řídí se vždy hlavním v cloudu.</span><span class="sxs-lookup"><span data-stu-id="a903d-107">This is a group that is always mastered in the cloud.</span></span> <span data-ttu-id="a903d-108">Pokud máte místní Exchange, pak můžete napsat zpět tyto skupiny do místního uživatele s poštovní schránky systému Exchange místně odesílat a přijímat e-maily z těchto skupin.</span><span class="sxs-lookup"><span data-stu-id="a903d-108">If you have Exchange on-premises, then you can write back these groups to on-premises so users with an on-premises Exchange mailbox can send and receive emails from these groups.</span></span>

<span data-ttu-id="a903d-109">Další informace o skupiny Office 365 a jejich použití naleznete [zde](http://aka.ms/O365g).</span><span class="sxs-lookup"><span data-stu-id="a903d-109">More information about Office 365 Groups and how to use them can be found [here](http://aka.ms/O365g).</span></span>

<span data-ttu-id="a903d-110">Skupiny služby Office 365 je reprezentován jako distribuční skupiny v místní službě AD DS.</span><span class="sxs-lookup"><span data-stu-id="a903d-110">An Office 365 group is represented as a distribution group in on-premises AD DS.</span></span> <span data-ttu-id="a903d-111">Vaše místní Exchange server musí být na serveru Exchange 2013 kumulativní aktualizaci 8 (vydané v března 2015) nebo Exchange 2016 rozpoznat tento nový typ skupiny.</span><span class="sxs-lookup"><span data-stu-id="a903d-111">Your on-premises Exchange server must be on Exchange 2013 cumulative update 8 (released in March 2015) or Exchange 2016 to recognize this new group type.</span></span>

<span data-ttu-id="a903d-112">**Poznámky k ve verzi Preview**</span><span class="sxs-lookup"><span data-stu-id="a903d-112">**Notes during the preview**</span></span>

* <span data-ttu-id="a903d-113">Atribut kniha adresa není aktuálně naplněna ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="a903d-113">The address book attribute is currently not populated in the preview.</span></span> <span data-ttu-id="a903d-114">Bez tohoto atributu skupiny není zobrazená v globálním seznamu adres.</span><span class="sxs-lookup"><span data-stu-id="a903d-114">Without this attribute, the group is not visible in the GAL.</span></span> <span data-ttu-id="a903d-115">Nejjednodušší způsob, jak naplnit tento atribut je pomocí rutiny Exchange PowerShell `update-recipient`.</span><span class="sxs-lookup"><span data-stu-id="a903d-115">The easiest way to populate this attribute is to use the Exchange PowerShell cmdlet `update-recipient`.</span></span>
* <span data-ttu-id="a903d-116">Pouze doménové struktury se schématem systému Exchange jsou platné cíle pro skupiny.</span><span class="sxs-lookup"><span data-stu-id="a903d-116">Only forests with the Exchange schema are valid targets for groups.</span></span> <span data-ttu-id="a903d-117">Pokud byla zjištěna žádná Exchange, není zpětný zápis skupin povolit.</span><span class="sxs-lookup"><span data-stu-id="a903d-117">If no Exchange was detected, then group writeback is not possible to enable.</span></span>
* <span data-ttu-id="a903d-118">Aktuálně jsou podporovány pouze jednou doménovou strukturou nasazení organizaci Exchange.</span><span class="sxs-lookup"><span data-stu-id="a903d-118">Only single-forest Exchange organization deployments are currently supported.</span></span> <span data-ttu-id="a903d-119">Pokud máte více než jednu organizaci místní Exchange, musíte do místní GALSync řešení pro tyto skupiny se objeví ve vašem jiných doménových strukturách.</span><span class="sxs-lookup"><span data-stu-id="a903d-119">If you have more than one Exchange organization on-premises, then you need an on-premises GALSync solution for these groups to appear in your other forests.</span></span>
* <span data-ttu-id="a903d-120">Funkce zpětného zápisu skupiny nezpracovává skupiny zabezpečení nebo distribuční skupiny.</span><span class="sxs-lookup"><span data-stu-id="a903d-120">The Group writeback feature does not handle security groups or distribution groups.</span></span>

> [!NOTE]
> <span data-ttu-id="a903d-121">Předplatné služby Azure AD Premium je vyžadována pro zpětný zápis skupin.</span><span class="sxs-lookup"><span data-stu-id="a903d-121">A subscription to Azure AD Premium is required for group writeback.</span></span>
> 
>

## <a name="user-writeback"></a><span data-ttu-id="a903d-122">Zpětný zápis uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a903d-122">User writeback</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a903d-123">Funkce preview zpětný zápis uživatelů byla odebrána v srpnu 2015 aktualizaci na Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="a903d-123">The user writeback preview feature was removed in the August 2015 update to Azure AD Connect.</span></span> <span data-ttu-id="a903d-124">Pokud jste ho povolili, měli byste zakázat tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a903d-124">If you have enabled it, then you should disable this feature.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="a903d-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a903d-125">Next steps</span></span>
<span data-ttu-id="a903d-126">Pokračovat vaše [vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="a903d-126">Continue your [Custom installation of Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span></span>

<span data-ttu-id="a903d-127">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="a903d-127">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
