---
title: "Zřizování uživatelů k aplikaci Azure AD Galerie je pořízení hodiny nebo víc | Microsoft Docs"
description: "Jak zjistit, proč zajišťování, které vaše aplikace může trvá déle, než jste očekávali"
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
ms.openlocfilehash: 183d60cbbbc8d02f7cd3cacc160453c45717ef0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a><span data-ttu-id="b2185-103">Zřizování uživatelů k aplikaci Azure AD Galerie je pořízení hodin nebo více</span><span class="sxs-lookup"><span data-stu-id="b2185-103">User provisioning to an Azure AD Gallery application is taking hours or more</span></span>

<span data-ttu-id="b2185-104">Při prvním povolení automatické zřizování pro aplikace, počáteční synchronizace může trvat od 20 minut několik hodin v závislosti na velikosti adresáře služby Azure AD a počet uživatelů v oboru pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="b2185-104">When first enabling automatic provisioning for an application, the initial sync can take anywhere from 20 minutes to several hours, depending on the size of the Azure AD directory and the number of users in scope for provisioning.</span></span> 

<span data-ttu-id="b2185-105">Následné synchronizace po počáteční synchronizace být rychlejší jako službu zřizování ukládá vodoznaky, které představují stav obou systémů po počáteční synchronizaci, zvýšení výkonu následné synchronizace.</span><span class="sxs-lookup"><span data-stu-id="b2185-105">Subsequent syncs after the initial sync be faster, as the provisioning service stores watermarks that represent the state of both systems after the initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-to-improve-provisioning-performance"></a><span data-ttu-id="b2185-106">Jak vylepšit výkon zřizování</span><span class="sxs-lookup"><span data-stu-id="b2185-106">How to improve provisioning performance</span></span>

<span data-ttu-id="b2185-107">Pokud počáteční synchronizace trvá víc než několik hodin, je jednou z věcí, které může provádět ke zlepšení výkonu:</span><span class="sxs-lookup"><span data-stu-id="b2185-107">If the initial sync is taking more than a few hours, there is one thing you can do to improve performance:</span></span>

-   <span data-ttu-id="b2185-108">**Filtry oboru uživatele.**</span><span class="sxs-lookup"><span data-stu-id="b2185-108">**User scoping filters.**</span></span> <span data-ttu-id="b2185-109">Oboru filtrů umožňují optimalizovat data, která službu zřizování extrahuje z Azure AD pomocí filtrování uživatelů na základě hodnot určitým atributem.</span><span class="sxs-lookup"><span data-stu-id="b2185-109">Scoping filters allow you to fine tune the data that the provisioning service extracts from Azure AD by filtering out users based on specific attribute values.</span></span> <span data-ttu-id="b2185-110">Další informace o filtry oborů najdete v tématu [zřizování aplikace na základě atributů s filtry oborů](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span><span class="sxs-lookup"><span data-stu-id="b2185-110">For more information on scoping filters, see [Attribute-based application provisioning with scoping filters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2185-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2185-111">Next steps</span></span>
[<span data-ttu-id="b2185-112">Automatizovat uživatele zajišťování a rušení zajištění pro aplikace SaaS ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b2185-112">Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)

