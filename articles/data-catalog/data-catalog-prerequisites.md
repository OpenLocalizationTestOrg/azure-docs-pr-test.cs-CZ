---
title: "požadavky aaaAzure katalogu Data Catalog | Microsoft Docs"
description: "Další informace o hello požadavky, které že budete potřebovat tooget začít s Azure Data Catalog."
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
ms.openlocfilehash: 0c8e768e5846c61b542b746d7ad80121725a9ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-prerequisites"></a><span data-ttu-id="c6c71-103">Požadavky na Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="c6c71-103">Azure Data Catalog prerequisites</span></span>

<span data-ttu-id="c6c71-104">Před nastavením Azure Data Catalog musíte tootake péče o pár věcí.</span><span class="sxs-lookup"><span data-stu-id="c6c71-104">You need tootake care of a few things before you can set up Azure Data Catalog.</span></span> <span data-ttu-id="c6c71-105">Nemusíte si dělat starosti, není tento proces trvá dlouho.</span><span class="sxs-lookup"><span data-stu-id="c6c71-105">Don’t worry, this process does not take long.</span></span>

## <a name="azure-subscription"></a><span data-ttu-id="c6c71-106">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="c6c71-106">Azure subscription</span></span>
<span data-ttu-id="c6c71-107">tooset do katalogu Data Catalog, musí být vlastníkem hello nebo spoluvlastník předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c6c71-107">tooset up Data Catalog, you must be hello owner or co-owner of an Azure subscription.</span></span>

<span data-ttu-id="c6c71-108">Předplatná Azure můžete uspořádat přístup k prostředkům služby toocloud například katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="c6c71-108">Azure subscriptions help you organize access toocloud-service resources such as Data Catalog.</span></span> <span data-ttu-id="c6c71-109">Odběry také umožňují řídit způsob hlášené, účtují a zaplacení využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="c6c71-109">Subscriptions also help you control how resource usage is reported, billed, and paid for.</span></span> <span data-ttu-id="c6c71-110">Každý odběr může mít samostatné fakturace a platebních instalace, tak může mít odběry a plány, které se liší podle oddělení, projektů, místní office a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c6c71-110">Each subscription can have a separate billing and payment setup, so you can have subscriptions and plans that vary by department, project, regional office, and so on.</span></span> <span data-ttu-id="c6c71-111">Každé cloudové služby patří tooa předplatného a potřebujete toohave předplatné před nastavením Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="c6c71-111">Every cloud service belongs tooa subscription, and you need toohave a subscription before you set up Data Catalog.</span></span> <span data-ttu-id="c6c71-112">Další, najdete v části toolearn [spravovat účty, odběry a správu role](../active-directory/active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="c6c71-112">toolearn more, see [Manage accounts, subscriptions, and administrative roles](../active-directory/active-directory-assign-admin-roles.md).</span></span>

## <a name="azure-active-directory"></a><span data-ttu-id="c6c71-113">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6c71-113">Azure Active Directory</span></span>
<span data-ttu-id="c6c71-114">tooset do katalogu Data Catalog můžete musí být podepsané pomocí uživatelského účtu Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c6c71-114">tooset up Data Catalog, you must be signed in with an Azure Active Directory (Azure AD) user account.</span></span>

<span data-ttu-id="c6c71-115">Azure AD poskytuje snadný způsob pro obchodní toomanage identity a přístup v hello cloudové i místní.</span><span class="sxs-lookup"><span data-stu-id="c6c71-115">Azure AD provides an easy way for your business toomanage identity and access, both in hello cloud and on-premises.</span></span> <span data-ttu-id="c6c71-116">Uživatele můžete použít pracovní nebo školní účet pro tooany přihlášení cloud a místní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6c71-116">Users can use a single work or school account for single sign-in tooany cloud and on-premises web application.</span></span> <span data-ttu-id="c6c71-117">Data Catalog používá přihlášení tooauthenticate Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6c71-117">Data Catalog uses Azure AD tooauthenticate sign-in.</span></span> <span data-ttu-id="c6c71-118">Další, najdete v části toolearn [co je Azure Active Directory?](../active-directory/active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c6c71-118">toolearn more, see [What is Azure Active Directory?](../active-directory/active-directory-whatis.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c6c71-119">Pomocí hello [portál Azure](http://portal.azure.com/), se můžete přihlásit se pomocí osobního účtu Microsoft nebo Azure Active Directory pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="c6c71-119">By using hello [Azure portal](http://portal.azure.com/), you can sign in with either a personal Microsoft account or an Azure Active Directory work or school account.</span></span> <span data-ttu-id="c6c71-120">tooset do katalogu Data Catalog pomocí buď hello portálu Azure nebo hello [katalogu Data Catalog portál](http://www.azuredatacatalog.com), musí se přihlásit účtu Azure Active Directory, nikoli osobní účet.</span><span class="sxs-lookup"><span data-stu-id="c6c71-120">tooset up Data Catalog by using either hello Azure portal or hello [Data Catalog portal](http://www.azuredatacatalog.com), you must sign in with an Azure Active Directory account, not a personal account.</span></span>
>
>

## <a name="active-directory-policy-configuration"></a><span data-ttu-id="c6c71-121">Zásady Konfigurace služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6c71-121">Active Directory policy configuration</span></span>
<span data-ttu-id="c6c71-122">Tam, kde se můžete přihlásit toohello katalogu Data Catalog portálu, ale když se pokusíte toosign v toohello nástroj registrace zdroje dat se může nastat situace, dojde k chybovou zprávu, která brání přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c6c71-122">You might encounter a situation where you can sign in toohello Data Catalog portal, but when you attempt toosign in toohello data source registration tool, you encounter an error message that prevents you from signing in.</span></span> <span data-ttu-id="c6c71-123">Toto chování problému může dojít pouze v případě, že jste na podnikové síti hello nebo může dojít, pouze pokud se připojujete ze mimo hello podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="c6c71-123">This problem behavior might occur only when you're on hello company network, or it might occur only when you're connecting from outside hello company network.</span></span>

<span data-ttu-id="c6c71-124">Nástroj registrace zdroje dat Hello používá ověřování pomocí formulářů toovalidate pověření uživatele pro službu Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c6c71-124">hello data source registration tool uses Forms Authentication toovalidate your user credentials against Active Directory.</span></span> <span data-ttu-id="c6c71-125">toohelp úspěšně přihlášení, Správce služby Active Directory musí povolit ověřování pomocí formulářů v hello globální zásady ověřování.</span><span class="sxs-lookup"><span data-stu-id="c6c71-125">toohelp you sign in successfully, an Active Directory administrator must enable Forms Authentication in hello Global Authentication Policy.</span></span>

<span data-ttu-id="c6c71-126">V hello globální zásady ověřování metody ověřování může být povoleno samostatně intranetu a extranetu, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="c6c71-126">In hello Global Authentication Policy, authentication methods can be enabled separately for intranet and extranet connections, as shown in hello following screenshot.</span></span> <span data-ttu-id="c6c71-127">Pokud je ověřování pomocí formulářů není povoleno pro hello síť, ze kterého se připojujete, může dojít k chybám přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c6c71-127">Sign-in errors might occur if Forms Authentication is not enabled for hello network from which you're connecting.</span></span>

 ![Služby Active Directory globální zásady ověřování](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a><span data-ttu-id="c6c71-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6c71-129">Next steps</span></span>
<span data-ttu-id="c6c71-130">Další informace najdete v tématu [konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6c71-130">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>
