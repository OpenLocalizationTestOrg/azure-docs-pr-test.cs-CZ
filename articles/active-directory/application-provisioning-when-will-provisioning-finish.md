---
title: "aaaUser zřizování tooan Azure AD application Gallery je pořízení hodiny nebo víc | Microsoft Docs"
description: "Jak toofind out proč zřizování aplikace tooyour možná trvá déle, než jste očekávání"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 0658f041724c91ddd1997cc7759393b46680f13a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-provisioning-tooan-azure-ad-gallery-application-is-taking-hours-or-more"></a><span data-ttu-id="8c640-103">Zřizování aplikace Galerie tooan Azure AD uživatelů je pořízení hodin nebo více</span><span class="sxs-lookup"><span data-stu-id="8c640-103">User provisioning tooan Azure AD Gallery application is taking hours or more</span></span>

<span data-ttu-id="8c640-104">Při prvním povolení automatické zřizování pro aplikace, hello počáteční synchronizace může trvat hodiny tooseveral 20 minut, v závislosti na velikosti hello hello adresář Azure AD a hello počet uživatelů v oboru pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="8c640-104">When first enabling automatic provisioning for an application, hello initial sync can take anywhere from 20 minutes tooseveral hours, depending on hello size of hello Azure AD directory and hello number of users in scope for provisioning.</span></span> 

<span data-ttu-id="8c640-105">Následné synchronizace po počáteční synchronizaci hello být rychlejší jako hello zřizování služby ukládá vodoznaky, které představují hello stav obou systémů po počáteční synchronizaci hello, zvýšení výkonu následné synchronizace.</span><span class="sxs-lookup"><span data-stu-id="8c640-105">Subsequent syncs after hello initial sync be faster, as hello provisioning service stores watermarks that represent hello state of both systems after hello initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-tooimprove-provisioning-performance"></a><span data-ttu-id="8c640-106">Jak tooimprove zřizování výkonu</span><span class="sxs-lookup"><span data-stu-id="8c640-106">How tooimprove provisioning performance</span></span>

<span data-ttu-id="8c640-107">Pokud počáteční synchronizace hello trvá víc než několik hodin, je jednou z věcí můžete provést tooimprove výkonu:</span><span class="sxs-lookup"><span data-stu-id="8c640-107">If hello initial sync is taking more than a few hours, there is one thing you can do tooimprove performance:</span></span>

-   <span data-ttu-id="8c640-108">**Filtry oboru uživatele.**</span><span class="sxs-lookup"><span data-stu-id="8c640-108">**User scoping filters.**</span></span> <span data-ttu-id="8c640-109">Oboru filtrů povolit toofine tune hello data, která hello zřizování služby extrahuje z Azure AD pomocí filtrování uživatelů na základě hodnot určitým atributem.</span><span class="sxs-lookup"><span data-stu-id="8c640-109">Scoping filters allow you toofine tune hello data that hello provisioning service extracts from Azure AD by filtering out users based on specific attribute values.</span></span> <span data-ttu-id="8c640-110">Další informace o filtry oborů najdete v tématu [zřizování aplikace na základě atributů s filtry oborů](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span><span class="sxs-lookup"><span data-stu-id="8c640-110">For more information on scoping filters, see [Attribute-based application provisioning with scoping filters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c640-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c640-111">Next steps</span></span>
[<span data-ttu-id="8c640-112">Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8c640-112">Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)

