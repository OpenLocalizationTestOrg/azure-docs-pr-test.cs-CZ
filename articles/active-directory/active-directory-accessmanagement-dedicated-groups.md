---
title: aaaDedicated skupin v Azure Active Directory | Microsoft Docs
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
ms.openlocfilehash: 8feec6e1a4e6b384470392d3043caeeec2b03dd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a><span data-ttu-id="6cbe3-103">Vyhrazené skupiny ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6cbe3-103">Dedicated groups in Azure Active Directory</span></span>
<span data-ttu-id="6cbe3-104">V Azure Active Directory (Azure AD) funkce vyhrazené skupiny hello automaticky vytvoří a naplní členství ve skupinách Azure AD předdefinované.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-104">In Azure Active Directory (Azure AD), hello dedicated groups feature automatically creates and populates membership for Azure AD predefined groups.</span></span> <span data-ttu-id="6cbe3-105">Nelze přidat členy vyhrazené skupiny nebo odebrané pomocí hello Azure classic portálu, rutiny prostředí Windows PowerShell, nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-105">Members of dedicated groups cannot be added or removed using hello Azure classic portal, Windows PowerShell cmdlets, or programmatically.</span></span>

> [!NOTE]
> <span data-ttu-id="6cbe3-106">Vyhrazené skupiny vyžadovat, že je přiřazený licenci Azure AD Premium</span><span class="sxs-lookup"><span data-stu-id="6cbe3-106">Dedicated groups require that an Azure AD Premium license is assigned to</span></span>
>
> * <span data-ttu-id="6cbe3-107">Hello správce, který spravuje hello pravidlo pro skupinu</span><span class="sxs-lookup"><span data-stu-id="6cbe3-107">hello administrator who manages hello rule on a group</span></span>
> * <span data-ttu-id="6cbe3-108">všichni uživatelé, kteří ve hello jsou vybrané pravidlo toobe členem skupiny hello</span><span class="sxs-lookup"><span data-stu-id="6cbe3-108">all users who are selected by hello rule toobe a member of hello group</span></span>
>
>

<span data-ttu-id="6cbe3-109">**tooenable vyhrazené skupiny**</span><span class="sxs-lookup"><span data-stu-id="6cbe3-109">**tooenable dedicated groups**</span></span>

1. <span data-ttu-id="6cbe3-110">V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-110">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="6cbe3-111">Vyberte hello **skupiny** kartu a pak otevřete hello skupiny chcete tooedit.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-111">Select hello **Groups** tab, and then open hello group you want tooedit.</span></span>
3. <span data-ttu-id="6cbe3-112">Vyberte hello **konfigurace** kartě a poté nastavte **povolte vyhrazené skupiny** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-112">Select hello **Configure** tab, and then set **Enable Dedicated Groups** too**Yes**.</span></span>

<span data-ttu-id="6cbe3-113">Jakmile hello přepínač Povolit vyhrazené skupiny je nastaven příliš**Ano**, můžete povolit další hello directory tooautomatically vytvořit hello všichni uživatelé vyhrazené skupiny nastavení hello **povolit "Všichni uživatelé" skupiny** přepínač příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-113">Once hello Enable Dedicated Groups switch is set too**Yes**, you can further enable hello directory tooautomatically create hello All Users dedicated group by setting hello **Enable “All Users” Group** switch too**Yes**.</span></span> <span data-ttu-id="6cbe3-114">Pak se taky dají upravovat hello název této vyhrazené skupiny zadáním názvu do hello **zobrazovaný název "Všichni uživatelé" skupina** pole.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-114">You can then also edit hello name of this dedicated group by typing it in hello **Display Name for “All Users” Group** field.</span></span>

<span data-ttu-id="6cbe3-115">Hello všechny uživatele skupiny lze tooassign hello stejné oprávnění tooall hello uživatelé v adresáři.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-115">hello All Users group can be used tooassign hello same permissions tooall hello users in your directory.</span></span> <span data-ttu-id="6cbe3-116">Například můžete udělit všichni uživatelé ve vaší directory přístup tooa aplikace SaaS přiřadit přístup pro hello všichni uživatelé vyhrazenou skupinu toothis aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-116">For example, you can grant all users in your directory access tooa SaaS application by assigning access for hello All Users dedicated group toothis application.</span></span>

<span data-ttu-id="6cbe3-117">Hello vyhrazené všichni uživatelé skupina obsahuje všechny uživatele v adresáři hello, včetně hostů a externí uživatele.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-117">hello dedicated All Users group includes all users in hello directory, including guests and external users.</span></span> <span data-ttu-id="6cbe3-118">Pokud potřebujete skupinu, vyloučí externí uživatele, a poté se dá dosáhnout vytvořením skupiny se na základě atributů dynamické pravidlo například hello následující:</span><span class="sxs-lookup"><span data-stu-id="6cbe3-118">If you need a group that excludes external users, then you can accomplish this by creating a group with an attribute-based dynamic rule such as hello following:</span></span>

                (user.userPrincipalName -notContains "#EXT#@")

<span data-ttu-id="6cbe3-119">Pro skupinu, která nezahrnuje všechny hosté použijte pravidlo například hello následující:</span><span class="sxs-lookup"><span data-stu-id="6cbe3-119">For a group that excludes all Guests, use a rule such as hello following:</span></span>

                (user.userType -ne "Guest")

<span data-ttu-id="6cbe3-120">toolearn o tom, toocreate *rozšířené* pravidel (která může obsahovat několik porovnání) pro dynamické členství ve skupině, najdete v části [pravidel pomocí atributů toocreate rozšířené](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="6cbe3-120">toolearn about how toocreate *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes toocreate advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

### <a name="next-steps"></a><span data-ttu-id="6cbe3-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6cbe3-121">Next steps</span></span>
<span data-ttu-id="6cbe3-122">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6cbe3-122">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="6cbe3-123">Správa přístupu tooresources pomocí skupin Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6cbe3-123">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="6cbe3-124">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6cbe3-124">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="6cbe3-125">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6cbe3-125">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="6cbe3-126">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6cbe3-126">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
