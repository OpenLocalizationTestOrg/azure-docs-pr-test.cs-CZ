---
title: "Azure Active Directory Domain Services: Povolení podpory pro službu SharePoint uživatelský profil | Microsoft Docs"
description: "Konfigurace Azure Active Directory Domain Services spravovaných domén na podporu synchronizace profilu pro SharePoint Server"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: c3c923b5c9cd652f0c5b0ec98c1cda740f180122
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a><span data-ttu-id="678a5-103">Konfigurace spravované doméně kvůli podpoře synchronizace profilu pro SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="678a5-103">Configure a managed domain to support profile synchronization for SharePoint Server</span></span>
<span data-ttu-id="678a5-104">SharePoint Server obsahuje služba profilu uživatele, který je používán k synchronizaci profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="678a5-104">SharePoint Server includes a User Profile Service that is used for user profile synchronization.</span></span> <span data-ttu-id="678a5-105">Pokud chcete nastavit službu profilu uživatele, nutné příslušná oprávnění udělit v doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="678a5-105">To set up the User Profile Service, appropriate permissions need to be granted on an Active Directory domain.</span></span> <span data-ttu-id="678a5-106">Další informace najdete v tématu [udělení oprávnění služby Active Directory Domain Services pro profil synchronizace SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span><span class="sxs-lookup"><span data-stu-id="678a5-106">For more information, see [grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span></span>

<span data-ttu-id="678a5-107">Tento článek vysvětluje způsob konfigurace Azure AD Domain Services spravovaných domén na nasazení služby synchronizace profilů uživatelů serveru SharePoint.</span><span class="sxs-lookup"><span data-stu-id="678a5-107">This article explains how you can configure Azure AD Domain Services managed domains to deploy the SharePoint Server User Profile Sync service.</span></span>

## <a name="the-aad-dc-service-accounts-group"></a><span data-ttu-id="678a5-108">Účty AAD řadič domény služby skupiny</span><span class="sxs-lookup"><span data-stu-id="678a5-108">The 'AAD DC Service Accounts' group</span></span>
<span data-ttu-id="678a5-109">Skupina zabezpečení s názvem "**účty řadiče domény služby AAD**' je k dispozici v rámci organizační jednotky, uživatele' na vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="678a5-109">A security group called '**AAD DC Service Accounts**' is available within the 'Users' organizational unit on your managed domain.</span></span> <span data-ttu-id="678a5-110">Zobrazí se v této skupině **Active Directory Users and Computers** modul snap-in konzoly MMC na vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="678a5-110">You can see this group in the **Active Directory Users and Computers** MMC snap-in on your managed domain.</span></span>

![Skupina zabezpečení účtů služby AAD řadiče domény](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

<span data-ttu-id="678a5-112">Členové této skupiny zabezpečení jsou delegovanými následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="678a5-112">Members of this security group are delegated the following privileges:</span></span>
- <span data-ttu-id="678a5-113">Oprávnění k replikaci změn adresáře v kořenovém adresáři DSE spravované domény.</span><span class="sxs-lookup"><span data-stu-id="678a5-113">The 'Replicate Directory Changes' privilege on the root DSE of the managed domain.</span></span>
- <span data-ttu-id="678a5-114">Oprávnění k replikaci adresáře změny v názvovém kontextu konfigurace (cn = kontejneru konfigurace) spravované domény.</span><span class="sxs-lookup"><span data-stu-id="678a5-114">The 'Replicate Directory Changes' privilege on the Configuration naming context (cn=configuration container) of the managed domain.</span></span>

<span data-ttu-id="678a5-115">Tato skupina zabezpečení je taky člen předdefinované skupiny **Pre-Windows 2000 Compatible Access**.</span><span class="sxs-lookup"><span data-stu-id="678a5-115">This security group is also a member of the built-in group **Pre-Windows 2000 Compatible Access**.</span></span>

![Skupina zabezpečení účtů služby AAD řadiče domény](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a><span data-ttu-id="678a5-117">Povolit vaší spravované domény pro podporu serveru SharePoint synchronizace profilů uživatelů</span><span class="sxs-lookup"><span data-stu-id="678a5-117">Enable your managed domain to support SharePoint Server user profile sync</span></span>
<span data-ttu-id="678a5-118">Přidáním účtu služby používaného synchronizace profilu uživatele služby SharePoint **účty řadiče domény služby AAD** skupiny.</span><span class="sxs-lookup"><span data-stu-id="678a5-118">You can add the service account used for SharePoint user profile synchronization to the **AAD DC Service Accounts** group.</span></span> <span data-ttu-id="678a5-119">V důsledku toho účet synchronizace získá odpovídající oprávnění k replikaci změn do adresáře.</span><span class="sxs-lookup"><span data-stu-id="678a5-119">As a result, the synchronization account gets adequate privileges to replicate changes to the directory.</span></span> <span data-ttu-id="678a5-120">Tento krok konfigurace umožňuje synchronizace profilů uživatelů serveru SharePoint správně fungovat.</span><span class="sxs-lookup"><span data-stu-id="678a5-120">This configuration step enables SharePoint Server user profile sync to work correctly.</span></span>

![Účty služby AAD DC - přidat členy](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![Účty služby AAD DC - přidat členy](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a><span data-ttu-id="678a5-123">Související obsah</span><span class="sxs-lookup"><span data-stu-id="678a5-123">Related Content</span></span>
* [<span data-ttu-id="678a5-124">Technické Reference - oprávnění Grant Active Directory Domain Services pro profil synchronizace SharePoint Server 2013</span><span class="sxs-lookup"><span data-stu-id="678a5-124">Technical Reference - Grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013</span></span>](https://technet.microsoft.com/library/hh296982.aspx)
