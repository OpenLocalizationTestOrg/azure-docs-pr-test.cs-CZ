---
title: "Vyžadovat přiřazení uživatelských – Azure AD | Microsoft Docs"
description: "Jak vyžadovat přiřazení uživatelů pro aplikace Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 30b78cba-1e0f-472f-8314-f2250a9b91c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
robots: noindex
ms.openlocfilehash: 079b806c041a4a21e9350342867aee581c57bf45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-and-applications-require-user-assignment"></a><span data-ttu-id="f24f0-103">Azure AD a aplikace: vyžadovat přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="f24f0-103">Azure AD and applications: Require user assignment</span></span>
## <a name="requiring-user-assignment"></a><span data-ttu-id="f24f0-104">Vyžadování přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="f24f0-104">Requiring User Assignment</span></span>
1. <span data-ttu-id="f24f0-105">Přihlaste se k portálu Azure pomocí účtu správce.</span><span class="sxs-lookup"><span data-stu-id="f24f0-105">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="f24f0-106">Klikněte na **všechny položky** položku v hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f24f0-106">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="f24f0-107">Vyberte adresář, který používáte pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f24f0-107">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="f24f0-108">Klikněte na **aplikace** kartě.</span><span class="sxs-lookup"><span data-stu-id="f24f0-108">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="f24f0-109">Vyberte aplikaci ze seznamu aplikací přidružených k tomuto adresáři.</span><span class="sxs-lookup"><span data-stu-id="f24f0-109">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="f24f0-110">Klikněte **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="f24f0-110">Click the **CONFIGURE** tab.</span></span>
7. <span data-ttu-id="f24f0-111">Změna **uživatele přiřazení vyžaduje přístup k aplikaci** přepnutí na Ano.</span><span class="sxs-lookup"><span data-stu-id="f24f0-111">Change the **User Assignment Required to Access App** toggle to Yes.</span></span>
8. <span data-ttu-id="f24f0-112">Klikněte **Uložit** tlačítko v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="f24f0-112">Click the **Save** button at the bottom of the screen.</span></span>

<span data-ttu-id="f24f0-113">Nyní je nutné přiřadit uživatele nebo skupiny do aplikace.</span><span class="sxs-lookup"><span data-stu-id="f24f0-113">You will now have to assign users and/or groups to the application.</span></span> <span data-ttu-id="f24f0-114">V tématu [přiřazování uživatelů k aplikaci](active-directory-applications-guiding-developers-assigning-users.md) a [přiřazení skupin k aplikaci](active-directory-applications-guiding-developers-assigning-groups.md).</span><span class="sxs-lookup"><span data-stu-id="f24f0-114">See [Assigning users to an application](active-directory-applications-guiding-developers-assigning-users.md) and [Assigning groups to an application](active-directory-applications-guiding-developers-assigning-groups.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f24f0-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f24f0-115">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
