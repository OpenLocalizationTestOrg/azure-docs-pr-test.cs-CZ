---
title: "Přiřadit skupiny aplikace Azure AD | Microsoft Docs"
description: "Popisuje, jak implementovat přiřazení skupiny pro aplikace Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: e0b0b87a454db96747f024e81882fe83d62fdbe2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="assign-azure-active-directory-groups-to-an-application"></a><span data-ttu-id="13d87-103">Přiřazení skupiny Azure Active Directory k aplikaci</span><span class="sxs-lookup"><span data-stu-id="13d87-103">Assign Azure Active Directory groups to an application</span></span>
<span data-ttu-id="13d87-104">Než uživatelé a skupiny můžete přiřadit k aplikaci, musí vyžadovat přiřazení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="13d87-104">Before you can assign users and groups to an application, you must require user assignment.</span></span> <span data-ttu-id="13d87-105">Další postupy vyžadují přiřazení uživatelů najdete v tématu [nutnosti přiřazení uživatelských](active-directory-applications-guiding-developers-requiring-user-assignment.md) článku.</span><span class="sxs-lookup"><span data-stu-id="13d87-105">To learn how to require user assignment, see the [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

<span data-ttu-id="13d87-106">Tento článek předpokládá, že jste již vytvořili skupiny ve službě active directory, který používáte pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="13d87-106">This article assumes that you have already created groups in the active directory you are using for this application.</span></span>

## <a name="assigning-groups-to-an-application"></a><span data-ttu-id="13d87-107">Přiřazení skupiny k aplikaci</span><span class="sxs-lookup"><span data-stu-id="13d87-107">Assigning Groups to an Application</span></span>
1. <span data-ttu-id="13d87-108">Přihlaste se k portálu Azure pomocí účtu správce.</span><span class="sxs-lookup"><span data-stu-id="13d87-108">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="13d87-109">Klikněte na **všechny položky** položku v hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="13d87-109">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="13d87-110">Vyberte adresář, který používáte pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="13d87-110">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="13d87-111">Klikněte na **aplikace** kartě.</span><span class="sxs-lookup"><span data-stu-id="13d87-111">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="13d87-112">Vyberte aplikaci ze seznamu aplikací přidružených k tomuto adresáři.</span><span class="sxs-lookup"><span data-stu-id="13d87-112">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="13d87-113">Klikněte **uživatele a skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="13d87-113">Click the **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="13d87-114">Filtrování seznamu skupin ve službě active directory pomocí **skupiny** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="13d87-114">Filter the list of groups in your active directory by using the **Groups** dropdown list.</span></span>
8. <span data-ttu-id="13d87-115">Vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="13d87-115">Select the group.</span></span>
9. <span data-ttu-id="13d87-116">Klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="13d87-116">Click **ASSIGN**.</span></span>
10. <span data-ttu-id="13d87-117">Klikněte na tlačítko **Ano** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="13d87-117">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13d87-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13d87-118">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
