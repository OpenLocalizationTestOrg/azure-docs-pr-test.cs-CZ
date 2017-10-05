---
title: "Azure AD a aplikace: přiřazování uživatelů k aplikaci | Microsoft Docs"
description: "Popisuje, jak implementovat přiřazení uživatelů pro aplikace Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 97ce69c1-4034-4e38-bd82-8caf984f6b98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: kgremban
robots: noindex
ms.openlocfilehash: 29d63bd5781dc7ef9e84840dd4b1b70222cf6892
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-and-applications-assigning-users-to-an-application"></a><span data-ttu-id="ac4bd-103">Azure AD a aplikace: přiřazování uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="ac4bd-103">Azure AD and Applications: Assigning Users to an Application</span></span>
<span data-ttu-id="ac4bd-104">Než uživatelé a skupiny můžete přiřadit k aplikaci, musí vyžadovat přiřazení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-104">Before you can assign users and groups to an application, you must require user assignment.</span></span>  <span data-ttu-id="ac4bd-105">Další informace o tom, jak vyžadovat přiřazení uživatelských najdete [nutnosti přiřazení uživatelských](active-directory-applications-guiding-developers-requiring-user-assignment.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-105">To learn how to require user assignment please see the [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

## <a name="assigning-users-to-an-application"></a><span data-ttu-id="ac4bd-106">Přiřazování uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="ac4bd-106">Assigning Users to an Application</span></span>
1. <span data-ttu-id="ac4bd-107">Přihlaste se k portálu Azure pomocí účtu správce.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-107">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="ac4bd-108">Klikněte na **všechny položky** položku v hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-108">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="ac4bd-109">Vyberte adresář, který používáte pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-109">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="ac4bd-110">Klikněte na **aplikace** kartě.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-110">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="ac4bd-111">Vyberte aplikaci ze seznamu aplikací přidružených k tomuto adresáři.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-111">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="ac4bd-112">Klikněte **uživatele a skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-112">Click the **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="ac4bd-113">Vyberte uživatele, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-113">Select the users you want to assign to the application.</span></span>
8. <span data-ttu-id="ac4bd-114">Klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-114">Click **ASSIGN**.</span></span>
9. <span data-ttu-id="ac4bd-115">Klikněte na tlačítko **Ano** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="ac4bd-115">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac4bd-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac4bd-116">Next Steps</span></span>
[!INCLUDE [guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]

