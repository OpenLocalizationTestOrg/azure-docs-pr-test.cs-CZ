---
title: "aaaCharacteristics služby Azure Active Directory klienta intercaction | Microsoft Docs"
description: "Seznámení s klienty jako plně nezávislý prostředky spravovat klienty klienta Azure Active"
services: active-tenant
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-tenant
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/27/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 57b677665c7cb4aee63f518c39d26754fe71a999
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a><span data-ttu-id="aec2d-103">Pochopit, jak více klientů služby Azure Active Directory pracovat</span><span class="sxs-lookup"><span data-stu-id="aec2d-103">Understand how multiple Azure Active Directory tenants interact</span></span>

<span data-ttu-id="aec2d-104">V Azure Active Directory (Azure AD), každý tenanta je plně nezávislý prostředek: sdílené, které je logicky nezávislý hello ostatních klientů, které spravujete.</span><span class="sxs-lookup"><span data-stu-id="aec2d-104">In Azure Active Directory (Azure AD), each tenent is a fully independent resource: a peer that is logically independent from hello other tenants that you manage.</span></span> <span data-ttu-id="aec2d-105">Není žádný vztah nadřazený podřízený mezi klienty.</span><span class="sxs-lookup"><span data-stu-id="aec2d-105">There is no parent-child relationship between tenants.</span></span> <span data-ttu-id="aec2d-106">Tato nezávislost mezi klienty zahrnuje nezávislost prostředků, nezávislost správy a nezávislost synchronizace.</span><span class="sxs-lookup"><span data-stu-id="aec2d-106">This independence between tenants includes resource independence, administrative independence, and synchronization independence.</span></span>

## <a name="resource-independence"></a><span data-ttu-id="aec2d-107">Nezávislost prostředků</span><span class="sxs-lookup"><span data-stu-id="aec2d-107">Resource independence</span></span>
* <span data-ttu-id="aec2d-108">Pokud vytvoříte nebo odstranění prostředku v jednoho klienta, nemá žádný vliv na prostředky v jiného klienta, s výjimkou hello částečné externí uživatele.</span><span class="sxs-lookup"><span data-stu-id="aec2d-108">If you create or delete a resource in one tenant, it has no impact on any resource in another tenant, with hello partial exception of external users.</span></span> 
* <span data-ttu-id="aec2d-109">Pokud použijete jeden názvů domén pomocí jednoho klienta, nelze použít s žádným jiným klientem.</span><span class="sxs-lookup"><span data-stu-id="aec2d-109">If you use one of your domain names with one tenant, it cannot be used with any other tenant.</span></span>

## <a name="administrative-independence"></a><span data-ttu-id="aec2d-110">Nezávislost správy</span><span class="sxs-lookup"><span data-stu-id="aec2d-110">Administrative independence</span></span>
<span data-ttu-id="aec2d-111">Pokud uživatele bez oprávnění správce klienta "Contoso" vytvoří testovacím klientem "Test", pak:</span><span class="sxs-lookup"><span data-stu-id="aec2d-111">If a non-administrative user of tenant 'Contoso' creates a test tenant 'Test,' then:</span></span>

* <span data-ttu-id="aec2d-112">Ve výchozím nastavení je hello uživatel, který vytvoří klienta přidat jako externí uživatel v nového klienta a roli globálního správce přiřazené hello v něm.</span><span class="sxs-lookup"><span data-stu-id="aec2d-112">By default, hello user who creates a tenant is added as an external user in that new tenant, and assigned hello global administrator role in that tenant.</span></span>
* <span data-ttu-id="aec2d-113">Správci Hello klienta "Contoso" nemají žádná přímá oprávnění správce tootenant "Test", pokud správce "Test" explicitně neudělí těchto oprávnění.</span><span class="sxs-lookup"><span data-stu-id="aec2d-113">hello administrators of tenant 'Contoso' have no direct administrative privileges tootenant 'Test,' unless an administrator of 'Test' specifically grants them these privileges.</span></span> <span data-ttu-id="aec2d-114">Však správci "Contoso" můžou řídit přístup tootenant "Test" Pokud se řídit hello uživatelský účet vytvořený Test.</span><span class="sxs-lookup"><span data-stu-id="aec2d-114">However, administrators of 'Contoso' can control access tootenant 'Test' if they control hello user account that created 'Test.'</span></span>
* <span data-ttu-id="aec2d-115">Pokud můžete přidat nebo odebrat roli správce u uživatele v jednoho klienta, změna hello nemá ovlivňují role správce hello této hello má uživatel v jiného klienta.</span><span class="sxs-lookup"><span data-stu-id="aec2d-115">If you add/remove an administrator role for a user in one tenant, hello change does not affect hello administrator roles that hello user has in another tenant.</span></span>

## <a name="synchronization-independence"></a><span data-ttu-id="aec2d-116">Nezávislost synchronizace</span><span class="sxs-lookup"><span data-stu-id="aec2d-116">Synchronization independence</span></span>
<span data-ttu-id="aec2d-117">Můžete nakonfigurovat každý Azure AD klienta nezávisle na nástroji tooget data synchronizovala z jediné instance buď:</span><span class="sxs-lookup"><span data-stu-id="aec2d-117">You can configure each Azure AD tenant independently tooget data synchronized from a single instance of either:</span></span>

* <span data-ttu-id="aec2d-118">nástroji Hello Azure AD Connect, toosynchronize dat s jednou doménovou strukturou AD.</span><span class="sxs-lookup"><span data-stu-id="aec2d-118">hello Azure AD Connect tool, toosynchronize data with a single AD forest.</span></span>
* <span data-ttu-id="aec2d-119">Hello klienta Azure Active konektor pro nástroj Forefront Identity Manager toosynchronize dat s jednou nebo více místními doménovými strukturami a/nebo zdroje dat mimo Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aec2d-119">hello Azure Active tenant Connector for Forefront Identity Manager, toosynchronize data with one or more on-premises forests, and/or non-Azure AD data sources.</span></span>

## <a name="add-an-azure-ad-tenant"></a><span data-ttu-id="aec2d-120">Přidat klienta Azure AD</span><span class="sxs-lookup"><span data-stu-id="aec2d-120">Add an Azure AD tenant</span></span>
<span data-ttu-id="aec2d-121">tooadd klient služby Azure AD v hello portál Azure, přihlaste se příliš[hello portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce Azure AD a na levé straně hello, vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="aec2d-121">tooadd an Azure AD tenant in hello Azure portal, sign in too[hello Azure portal](https://portal.azure.com) with an account that is an Azure AD global administrator, and, on hello left, select **New**.</span></span>

> [!NOTE]
> <span data-ttu-id="aec2d-122">Na rozdíl od jiných prostředků Azure klienti nejsou podřízené prostředky předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="aec2d-122">Unlike other Azure resources, your tenants are not child resources of an Azure subscription.</span></span> <span data-ttu-id="aec2d-123">Pokud vaše předplatné Azure je zrušená nebo vypršela platnost, můžete pořád přístup k datům klienta pomocí prostředí Azure PowerShell, hello Azure Graph API nebo Centrum pro správu hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="aec2d-123">If your Azure subscription is canceled or expired, you can still access your tenant data using Azure PowerShell, hello Azure Graph API, or hello Office 365 Admin Center.</span></span> <span data-ttu-id="aec2d-124">Také můžete přidružit jiné předplatné s klientem hello.</span><span class="sxs-lookup"><span data-stu-id="aec2d-124">You can also associate another subscription with hello tenant.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="aec2d-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aec2d-125">Next steps</span></span>
<span data-ttu-id="aec2d-126">Komplexní přehled o problémy s licencí Azure AD a osvědčené postupy, najdete v části [co je Azure Active klienta licencování?](active-directory-licensing-whatis-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="aec2d-126">For a broad overview of Azure AD licensing issues and best practices, see [What is Azure Active tenant licensing?](active-directory-licensing-whatis-azure-portal.md)</span></span>
