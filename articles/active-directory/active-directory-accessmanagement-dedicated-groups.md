---
title: "Vyhrazené skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Přehled o tom, jak vyhrazené skupiny fungují v Azure Active Directory a jak se vytvářejí."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: d9decd5de6a5bafc525edc5b04c82701185088ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a><span data-ttu-id="9482e-103">Vyhrazené skupiny ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9482e-103">Dedicated groups in Azure Active Directory</span></span>
<span data-ttu-id="9482e-104">Funkci vyhrazené skupiny ve službě Azure Active Directory (Azure AD), automaticky vytvoří a naplní členství ve skupinách Azure AD předdefinované.</span><span class="sxs-lookup"><span data-stu-id="9482e-104">In Azure Active Directory (Azure AD), the dedicated groups feature automatically creates and populates membership for Azure AD predefined groups.</span></span> <span data-ttu-id="9482e-105">Členové vyhrazené skupiny nelze přidat nebo odebrat pomocí portálu Azure classic, rutiny prostředí Windows PowerShell, nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="9482e-105">Members of dedicated groups cannot be added or removed using the Azure classic portal, Windows PowerShell cmdlets, or programmatically.</span></span>

> [!NOTE]
> <span data-ttu-id="9482e-106">Vyhrazené skupiny vyžadovat, že je přiřazený licenci Azure AD Premium</span><span class="sxs-lookup"><span data-stu-id="9482e-106">Dedicated groups require that an Azure AD Premium license is assigned to</span></span>
>
> * <span data-ttu-id="9482e-107">správce, který spravuje pravidlo na skupinu</span><span class="sxs-lookup"><span data-stu-id="9482e-107">the administrator who manages the rule on a group</span></span>
> * <span data-ttu-id="9482e-108">všichni uživatelé, kteří jsou pomocí pravidla Vybraní jako členové skupiny</span><span class="sxs-lookup"><span data-stu-id="9482e-108">all users who are selected by the rule to be a member of the group</span></span>
>
>

<span data-ttu-id="9482e-109">**Chcete-li povolit vyhrazené skupiny**</span><span class="sxs-lookup"><span data-stu-id="9482e-109">**To enable dedicated groups**</span></span>

1. <span data-ttu-id="9482e-110">V [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="9482e-110">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="9482e-111">Vyberte **skupiny** kartě a pak otevřete skupinu, kterou chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="9482e-111">Select the **Groups** tab, and then open the group you want to edit.</span></span>
3. <span data-ttu-id="9482e-112">Vyberte **konfigurace** kartě a poté nastavte **povolte vyhrazené skupiny** k **Ano**.</span><span class="sxs-lookup"><span data-stu-id="9482e-112">Select the **Configure** tab, and then set **Enable Dedicated Groups** to **Yes**.</span></span>

<span data-ttu-id="9482e-113">Povolit vyhrazené skupiny přepínač nastavenou na hodnotu **Ano**, můžete povolit další adresář, který chcete automaticky vytvořit vyhrazenou skupinu všichni uživatelé nastavením **povolit "Všichni uživatelé" skupina** přepnout na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="9482e-113">Once the Enable Dedicated Groups switch is set to **Yes**, you can further enable the directory to automatically create the All Users dedicated group by setting the **Enable “All Users” Group** switch to **Yes**.</span></span> <span data-ttu-id="9482e-114">Můžete pak taky upravit název této vyhrazené skupiny zadáním **zobrazovaný název "Všichni uživatelé" skupina** pole.</span><span class="sxs-lookup"><span data-stu-id="9482e-114">You can then also edit the name of this dedicated group by typing it in the **Display Name for “All Users” Group** field.</span></span>

<span data-ttu-id="9482e-115">Všichni uživatelé skupiny lze přiřadit stejná oprávnění pro všechny uživatele ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="9482e-115">The All Users group can be used to assign the same permissions to all the users in your directory.</span></span> <span data-ttu-id="9482e-116">Například můžete udělit všichni uživatelé v adresáři přístup k aplikaci SaaS přiřadit přístup pro vyhrazené skupině všichni uživatelé k této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9482e-116">For example, you can grant all users in your directory access to a SaaS application by assigning access for the All Users dedicated group to this application.</span></span>

<span data-ttu-id="9482e-117">Vyhrazené všichni uživatelé skupina obsahuje všechny uživatele v adresáři, včetně hostů a externí uživatele.</span><span class="sxs-lookup"><span data-stu-id="9482e-117">The dedicated All Users group includes all users in the directory, including guests and external users.</span></span> <span data-ttu-id="9482e-118">Pokud potřebujete skupinu, vyloučí externí uživatele, a poté se dá dosáhnout vytvořením skupiny se na základě atributů dynamické pravidlo například následující:</span><span class="sxs-lookup"><span data-stu-id="9482e-118">If you need a group that excludes external users, then you can accomplish this by creating a group with an attribute-based dynamic rule such as the following:</span></span>

                (user.userPrincipalName -notContains "#EXT#@")

<span data-ttu-id="9482e-119">Pro skupinu, která nezahrnuje všechny hosté použijte například následující pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9482e-119">For a group that excludes all Guests, use a rule such as the following:</span></span>

                (user.userType -ne "Guest")

<span data-ttu-id="9482e-120">Další informace o vytváření *rozšířených* pravidel (která můžou obsahovat několik porovnání) pro dynamické členství ve skupině najdete v článku o [používání atributů k vytvoření rozšířených pravidel](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="9482e-120">To learn about how to create *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes to create advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

### <a name="next-steps"></a><span data-ttu-id="9482e-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9482e-121">Next steps</span></span>
<span data-ttu-id="9482e-122">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9482e-122">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="9482e-123">Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9482e-123">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="9482e-124">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9482e-124">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="9482e-125">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9482e-125">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="9482e-126">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9482e-126">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
