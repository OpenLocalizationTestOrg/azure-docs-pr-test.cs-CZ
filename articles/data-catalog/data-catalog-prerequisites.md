---
title: "Požadavky Azure Data Catalog | Microsoft Docs"
description: "Další informace o požadavcích, že potřebujete Začínáme s Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 3fdef7bb58a5cd5dfbe4d37d9baf9c8e392ebe42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-prerequisites"></a><span data-ttu-id="6f7a2-103">Požadavky na Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="6f7a2-103">Azure Data Catalog prerequisites</span></span>

<span data-ttu-id="6f7a2-104">Budete muset postará o pár věcí, než můžete nastavit Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-104">You need to take care of a few things before you can set up Azure Data Catalog.</span></span> <span data-ttu-id="6f7a2-105">Nemusíte si dělat starosti, není tento proces trvá dlouho.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-105">Don’t worry, this process does not take long.</span></span>

## <a name="azure-subscription"></a><span data-ttu-id="6f7a2-106">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="6f7a2-106">Azure subscription</span></span>
<span data-ttu-id="6f7a2-107">Pokud chcete nastavit katalogu Data Catalog, musí být vlastníkem nebo spoluvlastník předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-107">To set up Data Catalog, you must be the owner or co-owner of an Azure subscription.</span></span>

<span data-ttu-id="6f7a2-108">Předplatná Azure, která umožňují přístup k prostředkům cloudových služeb, například Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-108">Azure subscriptions help you organize access to cloud-service resources such as Data Catalog.</span></span> <span data-ttu-id="6f7a2-109">Odběry také umožňují řídit způsob hlášené, účtují a zaplacení využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-109">Subscriptions also help you control how resource usage is reported, billed, and paid for.</span></span> <span data-ttu-id="6f7a2-110">Každý odběr může mít samostatné fakturace a platebních instalace, tak může mít odběry a plány, které se liší podle oddělení, projektů, místní office a tak dále.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-110">Each subscription can have a separate billing and payment setup, so you can have subscriptions and plans that vary by department, project, regional office, and so on.</span></span> <span data-ttu-id="6f7a2-111">Každé cloudové služby přísluší k odběru a musíte mít předplatné před instalací katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-111">Every cloud service belongs to a subscription, and you need to have a subscription before you set up Data Catalog.</span></span> <span data-ttu-id="6f7a2-112">Více informací naleznete v tématu [Správa účtů, předplatných a správních rolí](../active-directory/active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="6f7a2-112">To learn more, see [Manage accounts, subscriptions, and administrative roles](../active-directory/active-directory-assign-admin-roles.md).</span></span>

## <a name="azure-active-directory"></a><span data-ttu-id="6f7a2-113">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f7a2-113">Azure Active Directory</span></span>
<span data-ttu-id="6f7a2-114">Pokud chcete nastavit katalogu Data Catalog, musí být podepsané pomocí uživatelského účtu Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6f7a2-114">To set up Data Catalog, you must be signed in with an Azure Active Directory (Azure AD) user account.</span></span>

<span data-ttu-id="6f7a2-115">Azure AD umožní vaší firmě snadnou správu identity a přístupu, a to jak v cloudu, tak i místně.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-115">Azure AD provides an easy way for your business to manage identity and access, both in the cloud and on-premises.</span></span> <span data-ttu-id="6f7a2-116">Uživatele můžete použít pracovní nebo školní účet pro jednom přihlášení na všechny cloudové a místní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-116">Users can use a single work or school account for single sign-in to any cloud and on-premises web application.</span></span> <span data-ttu-id="6f7a2-117">Data Catalog používá Azure AD k ověřování přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-117">Data Catalog uses Azure AD to authenticate sign-in.</span></span> <span data-ttu-id="6f7a2-118">Další informace najdete v tématu [co je Azure Active Directory?](../active-directory/active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6f7a2-118">To learn more, see [What is Azure Active Directory?](../active-directory/active-directory-whatis.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6f7a2-119">Pomocí [portál Azure](http://portal.azure.com/), se můžete přihlásit se pomocí osobního účtu Microsoft nebo Azure Active Directory pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-119">By using the [Azure portal](http://portal.azure.com/), you can sign in with either a personal Microsoft account or an Azure Active Directory work or school account.</span></span> <span data-ttu-id="6f7a2-120">Nastavit Data Catalog pomocí portálu Azure nebo [katalogu Data Catalog portál](http://www.azuredatacatalog.com), musí se přihlásit účtu Azure Active Directory, nikoli osobní účet.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-120">To set up Data Catalog by using either the Azure portal or the [Data Catalog portal](http://www.azuredatacatalog.com), you must sign in with an Azure Active Directory account, not a personal account.</span></span>
>
>

## <a name="active-directory-policy-configuration"></a><span data-ttu-id="6f7a2-121">Zásady Konfigurace služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f7a2-121">Active Directory policy configuration</span></span>
<span data-ttu-id="6f7a2-122">Může nastat situace, kde se můžete přihlásit k portálu katalogu Data Catalog, ale při pokusu o přihlášení k nástroj registrace zdroje dat, dojde k chybovou zprávu, která brání přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-122">You might encounter a situation where you can sign in to the Data Catalog portal, but when you attempt to sign in to the data source registration tool, you encounter an error message that prevents you from signing in.</span></span> <span data-ttu-id="6f7a2-123">Toto chování problému může dojít pouze v případě, že jste na podnikové síti, nebo ho může dojít pouze v případě, že se připojujete z vnějšku firemní sítě.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-123">This problem behavior might occur only when you're on the company network, or it might occur only when you're connecting from outside the company network.</span></span>

<span data-ttu-id="6f7a2-124">Nástroj registrace zdroje dat používá ověřování založené na formulářích k ověření pověření uživatele pro službu Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-124">The data source registration tool uses Forms Authentication to validate your user credentials against Active Directory.</span></span> <span data-ttu-id="6f7a2-125">Můžete úspěšně přihlásit, musí správce služby Active Directory povolit ověřování pomocí formulářů v globální zásady ověřování.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-125">To help you sign in successfully, an Active Directory administrator must enable Forms Authentication in the Global Authentication Policy.</span></span>

<span data-ttu-id="6f7a2-126">V globální zásady ověřování metody ověřování se dá nastavit samostatně pro intranetové a připojení k síti extranet, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-126">In the Global Authentication Policy, authentication methods can be enabled separately for intranet and extranet connections, as shown in the following screenshot.</span></span> <span data-ttu-id="6f7a2-127">Pokud je ověřování pomocí formulářů není povoleno pro síť, ze kterého se připojujete, může dojít k chybám přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6f7a2-127">Sign-in errors might occur if Forms Authentication is not enabled for the network from which you're connecting.</span></span>

 ![Služby Active Directory globální zásady ověřování](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a><span data-ttu-id="6f7a2-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f7a2-129">Next steps</span></span>
<span data-ttu-id="6f7a2-130">Další informace najdete v tématu [konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f7a2-130">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>
