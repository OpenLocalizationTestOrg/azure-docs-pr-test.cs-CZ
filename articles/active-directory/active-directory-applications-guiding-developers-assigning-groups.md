---
title: "aaaAssign skupin aplikací tooAzure AD | Microsoft Docs"
description: "Jak tooimplement skupiny přiřazení pro aplikace Azure."
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
ms.openlocfilehash: 086619df09c13bf259afc3128d45ed804b99e519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-azure-active-directory-groups-tooan-application"></a><span data-ttu-id="2a01b-103">Přiřazení skupiny Azure Active Directory tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="2a01b-103">Assign Azure Active Directory groups tooan application</span></span>
<span data-ttu-id="2a01b-104">Předtím, než můžete přiřadit uživatele a skupiny tooan aplikace, musí vyžadovat přiřazení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2a01b-104">Before you can assign users and groups tooan application, you must require user assignment.</span></span> <span data-ttu-id="2a01b-105">toolearn jak přiřazení uživatelských toorequire, najdete v části hello [nutnosti přiřazení uživatelských](active-directory-applications-guiding-developers-requiring-user-assignment.md) článku.</span><span class="sxs-lookup"><span data-stu-id="2a01b-105">toolearn how toorequire user assignment, see hello [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

<span data-ttu-id="2a01b-106">Tento článek předpokládá, že jste již vytvořili skupiny ve službě active directory hello, který používáte pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a01b-106">This article assumes that you have already created groups in hello active directory you are using for this application.</span></span>

## <a name="assigning-groups-tooan-application"></a><span data-ttu-id="2a01b-107">Přiřazení skupiny tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="2a01b-107">Assigning Groups tooan Application</span></span>
1. <span data-ttu-id="2a01b-108">Přihlaste se toohello portálu Azure pomocí účtu správce.</span><span class="sxs-lookup"><span data-stu-id="2a01b-108">Log in toohello Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="2a01b-109">Klikněte na hello **všechny položky** položku v hlavní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="2a01b-109">Click on hello **All Items** item in hello main menu.</span></span>
3. <span data-ttu-id="2a01b-110">Vyberte hello adresář, který používáte pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="2a01b-110">Choose hello directory you are using for hello application.</span></span>
4. <span data-ttu-id="2a01b-111">Klikněte na hello **aplikace** kartě.</span><span class="sxs-lookup"><span data-stu-id="2a01b-111">Click on hello **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="2a01b-112">Vyberte aplikaci hello hello seznamu aplikací přidružených k tomuto adresáři.</span><span class="sxs-lookup"><span data-stu-id="2a01b-112">Select hello application from hello list of applications associated with this directory.</span></span>
6. <span data-ttu-id="2a01b-113">Klikněte na tlačítko hello **uživatele a skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="2a01b-113">Click hello **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="2a01b-114">Filtr hello seznam skupin ve vašem active directory pomocí hello **skupiny** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="2a01b-114">Filter hello list of groups in your active directory by using hello **Groups** dropdown list.</span></span>
8. <span data-ttu-id="2a01b-115">Vyberte skupinu hello.</span><span class="sxs-lookup"><span data-stu-id="2a01b-115">Select hello group.</span></span>
9. <span data-ttu-id="2a01b-116">Klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="2a01b-116">Click **ASSIGN**.</span></span>
10. <span data-ttu-id="2a01b-117">Klikněte na tlačítko **Ano** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="2a01b-117">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a01b-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a01b-118">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
