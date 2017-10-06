---
title: "aaaTroubleshooting dynamické členství ve skupinách | Microsoft Docs"
description: "Tipy k řešení potíží pro dynamické členství ve skupinách v Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: d792fc406288844e2c5dbe3702c2c9870d09473e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a><span data-ttu-id="e3d36-103">Řešení potíží s dynamickým členstvím ve skupinách</span><span class="sxs-lookup"><span data-stu-id="e3d36-103">Troubleshooting dynamic memberships for groups</span></span>
<span data-ttu-id="e3d36-104">**I nakonfigurovali pravidlo na skupinu, ale žádné členství ve skupině hello aktualizovat**</span><span class="sxs-lookup"><span data-stu-id="e3d36-104">**I configured a rule on a group but no memberships get updated in hello group**</span></span><br/><span data-ttu-id="e3d36-105">Ověřte, že hello **povolit delegovaná Správa skupin** nastavení je příliš**Ano** v hello **konfigurace** kartě. Toto nastavení se zobrazí jenom v případě, že jste přihlášeni jako uživatel toowhom licenci Azure Active Directory Premium je přiřazen.</span><span class="sxs-lookup"><span data-stu-id="e3d36-105">Verify that hello **Enable delegated group management** setting is set too**Yes** in hello **Configure** tab. You will see this setting only if you are signed in as a user toowhom an Azure Active Directory Premium license is assigned.</span></span> <span data-ttu-id="e3d36-106">Ověřte hodnoty hello atributů uživatele na pravidlo hello: existují uživatelé, kteří odpovídají hello pravidlo?</span><span class="sxs-lookup"><span data-stu-id="e3d36-106">Verify hello values for user attributes on hello rule: are there users that satisfy hello rule?</span></span>

<span data-ttu-id="e3d36-107">**I nakonfigurovali pravidlo, ale teď se odeberou hello stávající členy pravidlo hello**</span><span class="sxs-lookup"><span data-stu-id="e3d36-107">**I configured a rule, but now hello existing members of hello rule are removed**</span></span><br/><span data-ttu-id="e3d36-108">Toto je očekávané chování.</span><span class="sxs-lookup"><span data-stu-id="e3d36-108">This is expected behavior.</span></span> <span data-ttu-id="e3d36-109">Stávající členy skupiny hello se odeberou, když je pravidlem povolené nebo změnit.</span><span class="sxs-lookup"><span data-stu-id="e3d36-109">Existing members of hello group are removed when a rule is enabled or changed.</span></span> <span data-ttu-id="e3d36-110">Uživatelé Hello vrácená z vyhodnocení hello pravidla jsou přidány jako členové skupiny toohello.</span><span class="sxs-lookup"><span data-stu-id="e3d36-110">hello users returned from evaluation of hello rule are added as members toohello group.</span></span>     

<span data-ttu-id="e3d36-111">**Není zobrazen, že změní okamžitě při přidání nebo změna pravidla, proč ne?**</span><span class="sxs-lookup"><span data-stu-id="e3d36-111">**I don’t see membership changes instantly when I add or change a rule, why not?**</span></span><br/><span data-ttu-id="e3d36-112">Vyhodnocování členství vyhrazené se provádí pravidelně v procesu asynchronní pozadí.</span><span class="sxs-lookup"><span data-stu-id="e3d36-112">Dedicated membership evaluation is done periodically in an asynchronous background process.</span></span> <span data-ttu-id="e3d36-113">Jak dlouho trvá proces hello je určen podle hello počet uživatelů v velikost vašeho adresáře a hello hello skupiny vytvořeny v důsledku hello pravidlo.</span><span class="sxs-lookup"><span data-stu-id="e3d36-113">How long hello process takes is determined by hello number of users in your directory and hello size of hello group created as a result of hello rule.</span></span> <span data-ttu-id="e3d36-114">Obvykle adresáře s malý počet uživatelů se zobrazí změn členství ve skupinách hello za méně než několik minut.</span><span class="sxs-lookup"><span data-stu-id="e3d36-114">Typically, directories with small numbers of users will see hello group membership changes in less than a few minutes.</span></span> <span data-ttu-id="e3d36-115">Adresáře s velký počet uživatelů, může trvat 30 minut nebo déle toopopulate.</span><span class="sxs-lookup"><span data-stu-id="e3d36-115">Directories with a large number of users can take 30 minutes or longer toopopulate.</span></span>

### <a name="next-steps"></a><span data-ttu-id="e3d36-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3d36-116">Next steps</span></span>
<span data-ttu-id="e3d36-117">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e3d36-117">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="e3d36-118">Správa přístupu tooresources pomocí skupin Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e3d36-118">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="e3d36-119">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e3d36-119">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="e3d36-120">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e3d36-120">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="e3d36-121">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e3d36-121">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
