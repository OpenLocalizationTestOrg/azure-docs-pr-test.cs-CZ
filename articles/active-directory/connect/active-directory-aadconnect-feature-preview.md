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
ms.openlocfilehash: bcfc710861b19d8f86f094ced0d1c691e0911f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="more-details-about-features-in-preview"></a><span data-ttu-id="1907f-103">Další informace o funkcích ve verzi preview</span><span class="sxs-lookup"><span data-stu-id="1907f-103">More details about features in preview</span></span>
<span data-ttu-id="1907f-104">Toto téma popisuje, jak funkce toouse aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="1907f-104">This topic describes how toouse features currently in preview.</span></span>

## <a name="group-writeback"></a><span data-ttu-id="1907f-105">Zpětný zápis skupin</span><span class="sxs-lookup"><span data-stu-id="1907f-105">Group writeback</span></span>
<span data-ttu-id="1907f-106">Hello možnost pro zpětný zápis skupin v volitelné funkce vám umožňuje toowriteback **skupiny Office 365** tooa doménová struktura s Exchange nainstalované.</span><span class="sxs-lookup"><span data-stu-id="1907f-106">hello option for group writeback in optional features allows you toowriteback **Office 365 Groups** tooa forest with Exchange installed.</span></span> <span data-ttu-id="1907f-107">Toto je skupinu, která řídí se vždy hlavním v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="1907f-107">This is a group that is always mastered in hello cloud.</span></span> <span data-ttu-id="1907f-108">Pokud máte místní Exchange, pak můžete napsat zpět tyto tooon místní skupiny uživatelů s poštovní schránky systému Exchange místně odesílat a přijímat e-maily z těchto skupin.</span><span class="sxs-lookup"><span data-stu-id="1907f-108">If you have Exchange on-premises, then you can write back these groups tooon-premises so users with an on-premises Exchange mailbox can send and receive emails from these groups.</span></span>

<span data-ttu-id="1907f-109">Další informace o skupiny Office 365 a jak toouse je možné najít [zde](http://aka.ms/O365g).</span><span class="sxs-lookup"><span data-stu-id="1907f-109">More information about Office 365 Groups and how toouse them can be found [here](http://aka.ms/O365g).</span></span>

<span data-ttu-id="1907f-110">Skupiny služby Office 365 je reprezentován jako distribuční skupiny v místní službě AD DS.</span><span class="sxs-lookup"><span data-stu-id="1907f-110">An Office 365 group is represented as a distribution group in on-premises AD DS.</span></span> <span data-ttu-id="1907f-111">Vaše místní Exchange server musí být na serveru Exchange 2013 kumulativní aktualizaci 8 (vydané v března 2015) nebo toorecognize Exchange 2016 tento nový typ skupiny.</span><span class="sxs-lookup"><span data-stu-id="1907f-111">Your on-premises Exchange server must be on Exchange 2013 cumulative update 8 (released in March 2015) or Exchange 2016 toorecognize this new group type.</span></span>

<span data-ttu-id="1907f-112">**Poznámky k verzi Preview hello**</span><span class="sxs-lookup"><span data-stu-id="1907f-112">**Notes during hello preview**</span></span>

* <span data-ttu-id="1907f-113">Hello adresu kniha atribut není aktuálně ve verzi preview hello naplněna.</span><span class="sxs-lookup"><span data-stu-id="1907f-113">hello address book attribute is currently not populated in hello preview.</span></span> <span data-ttu-id="1907f-114">Bez tohoto atributu není zobrazená v hello GAL hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="1907f-114">Without this attribute, hello group is not visible in hello GAL.</span></span> <span data-ttu-id="1907f-115">Hello toopopulate nejjednodušší způsob, jak tento atribut je rutiny Exchange PowerShell hello toouse `update-recipient`.</span><span class="sxs-lookup"><span data-stu-id="1907f-115">hello easiest way toopopulate this attribute is toouse hello Exchange PowerShell cmdlet `update-recipient`.</span></span>
* <span data-ttu-id="1907f-116">Pouze doménové struktury se schématem systému Exchange hello jsou platné cíle pro skupiny.</span><span class="sxs-lookup"><span data-stu-id="1907f-116">Only forests with hello Exchange schema are valid targets for groups.</span></span> <span data-ttu-id="1907f-117">Pokud byla zjištěna žádná Exchange, pak zpětný zápis skupin není možné tooenable.</span><span class="sxs-lookup"><span data-stu-id="1907f-117">If no Exchange was detected, then group writeback is not possible tooenable.</span></span>
* <span data-ttu-id="1907f-118">Aktuálně jsou podporovány pouze jednou doménovou strukturou nasazení organizaci Exchange.</span><span class="sxs-lookup"><span data-stu-id="1907f-118">Only single-forest Exchange organization deployments are currently supported.</span></span> <span data-ttu-id="1907f-119">Pokud máte více než jednu organizaci místní Exchange, bude nutné do místní GALSync řešení pro tyto skupiny tooappear v vaší jiných doménových strukturách.</span><span class="sxs-lookup"><span data-stu-id="1907f-119">If you have more than one Exchange organization on-premises, then you need an on-premises GALSync solution for these groups tooappear in your other forests.</span></span>
* <span data-ttu-id="1907f-120">Funkce zpětného zápisu skupiny Hello nezpracovává skupiny zabezpečení nebo distribuční skupiny.</span><span class="sxs-lookup"><span data-stu-id="1907f-120">hello Group writeback feature does not handle security groups or distribution groups.</span></span>

> [!NOTE]
> <span data-ttu-id="1907f-121">Předplatné tooAzure AD Premium je vyžadován pro zpětný zápis skupin.</span><span class="sxs-lookup"><span data-stu-id="1907f-121">A subscription tooAzure AD Premium is required for group writeback.</span></span>
> 
>

## <a name="user-writeback"></a><span data-ttu-id="1907f-122">Zpětný zápis uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1907f-122">User writeback</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1907f-123">Hello funkce preview zpětný zápis uživatelů byla odebrána v hello srpen 2015 update tooAzure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="1907f-123">hello user writeback preview feature was removed in hello August 2015 update tooAzure AD Connect.</span></span> <span data-ttu-id="1907f-124">Pokud jste ho povolili, měli byste zakázat tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1907f-124">If you have enabled it, then you should disable this feature.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="1907f-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1907f-125">Next steps</span></span>
<span data-ttu-id="1907f-126">Pokračovat vaše [vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="1907f-126">Continue your [Custom installation of Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span></span>

<span data-ttu-id="1907f-127">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="1907f-127">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
