---
title: "Azure Active Directory Domain Services: Povolení podpory pro službu SharePoint uživatelský profil | Microsoft Docs"
description: "Konfigurace Azure Active Directory Domain Services spravovaných domén toosupport profil synchronizace pro SharePoint Server"
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
ms.openlocfilehash: 9de4f810380309e8f6436fc24412701645978f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-managed-domain-toosupport-profile-synchronization-for-sharepoint-server"></a><span data-ttu-id="e589a-103">Konfigurovat synchronizaci profil toosupport spravované domény pro SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="e589a-103">Configure a managed domain toosupport profile synchronization for SharePoint Server</span></span>
<span data-ttu-id="e589a-104">SharePoint Server obsahuje služba profilu uživatele, který je používán k synchronizaci profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="e589a-104">SharePoint Server includes a User Profile Service that is used for user profile synchronization.</span></span> <span data-ttu-id="e589a-105">tooset až hello služba profilu uživatele, příslušná oprávnění potřebovat toobe udělena v doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e589a-105">tooset up hello User Profile Service, appropriate permissions need toobe granted on an Active Directory domain.</span></span> <span data-ttu-id="e589a-106">Další informace najdete v tématu [udělení oprávnění služby Active Directory Domain Services pro profil synchronizace SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span><span class="sxs-lookup"><span data-stu-id="e589a-106">For more information, see [grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span></span>

<span data-ttu-id="e589a-107">Tento článek vysvětluje způsob konfigurace Azure AD Domain Services spravovaných domén toodeploy hello synchronizace profilu uživatele Server služby SharePoint služby.</span><span class="sxs-lookup"><span data-stu-id="e589a-107">This article explains how you can configure Azure AD Domain Services managed domains toodeploy hello SharePoint Server User Profile Sync service.</span></span>

## <a name="hello-aad-dc-service-accounts-group"></a><span data-ttu-id="e589a-108">Hello účty AAD řadič domény služby skupiny</span><span class="sxs-lookup"><span data-stu-id="e589a-108">hello 'AAD DC Service Accounts' group</span></span>
<span data-ttu-id="e589a-109">Skupina zabezpečení s názvem "**účty řadiče domény služby AAD**' je k dispozici v rámci organizační jednotky, uživatele' hello na vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="e589a-109">A security group called '**AAD DC Service Accounts**' is available within hello 'Users' organizational unit on your managed domain.</span></span> <span data-ttu-id="e589a-110">Zobrazí se tato skupina v hello **Active Directory Users and Computers** modul snap-in konzoly MMC na vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="e589a-110">You can see this group in hello **Active Directory Users and Computers** MMC snap-in on your managed domain.</span></span>

![Skupina zabezpečení účtů služby AAD řadiče domény](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

<span data-ttu-id="e589a-112">Členové této skupiny zabezpečení jsou delegovaný hello následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="e589a-112">Members of this security group are delegated hello following privileges:</span></span>
- <span data-ttu-id="e589a-113">oprávnění Hello replikovat změny adresáře v kořenovém adresáři hello DSE hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="e589a-113">hello 'Replicate Directory Changes' privilege on hello root DSE of hello managed domain.</span></span>
- <span data-ttu-id="e589a-114">Hello oprávnění replikovat změny adresáře v názvovém kontextu konfigurace hello (cn = kontejneru konfigurace) hello ke správě domény.</span><span class="sxs-lookup"><span data-stu-id="e589a-114">hello 'Replicate Directory Changes' privilege on hello Configuration naming context (cn=configuration container) of hello managed domain.</span></span>

<span data-ttu-id="e589a-115">Tato skupina zabezpečení je taky jako člen předdefinované skupiny hello **Pre-Windows 2000 Compatible Access**.</span><span class="sxs-lookup"><span data-stu-id="e589a-115">This security group is also a member of hello built-in group **Pre-Windows 2000 Compatible Access**.</span></span>

![Skupina zabezpečení účtů služby AAD řadiče domény](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-toosupport-sharepoint-server-user-profile-sync"></a><span data-ttu-id="e589a-117">Povolit vaší spravované domény toosupport synchronizace profilů uživatelů serveru SharePoint</span><span class="sxs-lookup"><span data-stu-id="e589a-117">Enable your managed domain toosupport SharePoint Server user profile sync</span></span>
<span data-ttu-id="e589a-118">Můžete přidat hello účtu služby používaného pro SharePoint uživatele profil synchronizace toohello **účty řadiče domény služby AAD** skupiny.</span><span class="sxs-lookup"><span data-stu-id="e589a-118">You can add hello service account used for SharePoint user profile synchronization toohello **AAD DC Service Accounts** group.</span></span> <span data-ttu-id="e589a-119">V důsledku toho hello synchronizace účet získá odpovídající oprávnění tooreplicate změny toohello adresáře.</span><span class="sxs-lookup"><span data-stu-id="e589a-119">As a result, hello synchronization account gets adequate privileges tooreplicate changes toohello directory.</span></span> <span data-ttu-id="e589a-120">Tento krok konfigurace umožňuje toowork synchronizace profilu uživatele serveru SharePoint správně.</span><span class="sxs-lookup"><span data-stu-id="e589a-120">This configuration step enables SharePoint Server user profile sync toowork correctly.</span></span>

![Účty služby AAD DC - přidat členy](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![Účty služby AAD DC - přidat členy](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a><span data-ttu-id="e589a-123">Související obsah</span><span class="sxs-lookup"><span data-stu-id="e589a-123">Related Content</span></span>
* [<span data-ttu-id="e589a-124">Technické Reference - oprávnění Grant Active Directory Domain Services pro profil synchronizace SharePoint Server 2013</span><span class="sxs-lookup"><span data-stu-id="e589a-124">Technical Reference - Grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013</span></span>](https://technet.microsoft.com/library/hh296982.aspx)
