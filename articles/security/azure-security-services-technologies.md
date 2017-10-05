---
title: "Zabezpečení Azure službách a technologiích | Microsoft Docs"
description: "Tento článek poskytuje kurátorované seznam zabezpečení Azure službách a technologiích."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2016
ms.author: yurid
ms.openlocfilehash: 0bea62a43cf6cac9132fe64f2d6c54e52def4c55
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-security-services-and-technologies"></a><span data-ttu-id="ceacc-103">Zabezpečení Azure službách a technologiích</span><span class="sxs-lookup"><span data-stu-id="ceacc-103">Azure Security Services and Technologies</span></span>
<span data-ttu-id="ceacc-104">V našem diskuze s aktuálním a budoucím Azure zákazníků jsme se často dotaz "máte seznam všech zabezpečení souvisejících službách a technologiích, které Azure nabízí?"</span><span class="sxs-lookup"><span data-stu-id="ceacc-104">In our discussions with current and future Azure customers, we’re often asked “do you have a list of all the security related services and technologies that Azure has to offer?”</span></span>

<span data-ttu-id="ceacc-105">Jsme uvědomit, že když vyhodnocujete technické možnosti poskytovatele cloudové služby, je vhodné mít tento seznam k dispozici, můžete proniknout dolů hlubší Pokud je pro vás správný čas.</span><span class="sxs-lookup"><span data-stu-id="ceacc-105">We understand that when you’re evaluating your cloud service provider technical options, it’s helpful to have such a list available that you can use to dig down deeper when the time is right for you.</span></span>

<span data-ttu-id="ceacc-106">Toto je naše počáteční úsilí na poskytování seznamu.</span><span class="sxs-lookup"><span data-stu-id="ceacc-106">The following is our initial effort at providing a list.</span></span> <span data-ttu-id="ceacc-107">Tento seznam bude v průběhu času změnit a růst, stejně jako Azure.</span><span class="sxs-lookup"><span data-stu-id="ceacc-107">Over time, this list will change and grow, just as Azure does.</span></span> <span data-ttu-id="ceacc-108">V seznamu je zařazený do kategorie a seznamu kategorií se taky zvýší v čase.</span><span class="sxs-lookup"><span data-stu-id="ceacc-108">The list is categorized, and the list of categories will also grow over time.</span></span> <span data-ttu-id="ceacc-109">Ujistěte se, že zkontrolujte tuto stránku v pravidelných intervalech zůstane aktuální na našem související se zabezpečením službách a technologiích.</span><span class="sxs-lookup"><span data-stu-id="ceacc-109">Make sure to check this page on a regular basis to stay up-to-date on our security-related services and technologies.</span></span>

## <a name="azure-security---general"></a><span data-ttu-id="ceacc-110">Azure Security – obecné</span><span class="sxs-lookup"><span data-stu-id="ceacc-110">Azure Security - General</span></span>
* [<span data-ttu-id="ceacc-111">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ceacc-111">Azure Security Center</span></span>](https://azure.microsoft.com/documentation/services/security-center/)
* [<span data-ttu-id="ceacc-112">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ceacc-112">Azure Key Vault</span></span>](https://azure.microsoft.com/documentation/services/key-vault/)
* [<span data-ttu-id="ceacc-113">Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="ceacc-113">Azure Disk Encryption</span></span>](azure-security-disk-encryption.md)
* [<span data-ttu-id="ceacc-114">Log Analytics</span><span class="sxs-lookup"><span data-stu-id="ceacc-114">Log Analytics</span></span>](../log-analytics/log-analytics-overview.md)
* [<span data-ttu-id="ceacc-115">Prostředí Azure pro vývoj/testování</span><span class="sxs-lookup"><span data-stu-id="ceacc-115">Azure Dev/Test Labs</span></span>](https://azure.microsoft.com/documentation/services/devtest-lab/)

## <a name="azure-storage-security"></a><span data-ttu-id="ceacc-116">Zabezpečení úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ceacc-116">Azure Storage Security</span></span>
* [<span data-ttu-id="ceacc-117">Šifrování služby úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ceacc-117">Azure Storage Service Encryption</span></span>](../storage/common/storage-service-encryption.md)
* [<span data-ttu-id="ceacc-118">StorSimple šifrované hybridní úložiště</span><span class="sxs-lookup"><span data-stu-id="ceacc-118">StorSimple Encrypted Hybrid Storage</span></span>](https://azure.microsoft.com/documentation/services/storsimple/)
* [<span data-ttu-id="ceacc-119">Azure šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="ceacc-119">Azure Client-Side Encryption</span></span>](../storage/common/storage-client-side-encryption.md)
* [<span data-ttu-id="ceacc-120">Sdílené přístupové podpisy úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ceacc-120">Azure Storage Shared Access Signatures</span></span>](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="ceacc-121">Klíče účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ceacc-121">Azure Storage Account Keys</span></span>](../storage/common/storage-create-storage-account.md)
* [<span data-ttu-id="ceacc-122">Azure sdílené složky s šifrování protokolu SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="ceacc-122">Azure File shares with SMB 3.0 Encryption</span></span>](../storage/files/storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="ceacc-123">Azure Storage Analytics</span><span class="sxs-lookup"><span data-stu-id="ceacc-123">Azure Storage Analytics</span></span>](https://msdn.microsoft.com/library/hh343270.aspx)

## <a name="azure-database-security"></a><span data-ttu-id="ceacc-124">Zabezpečení databáze Azure</span><span class="sxs-lookup"><span data-stu-id="ceacc-124">Azure Database Security</span></span>
* [<span data-ttu-id="ceacc-125">Brány Firewall Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ceacc-125">Azure SQL Firewall</span></span>](../sql-database/sql-database-firewall-configure.md)
* [<span data-ttu-id="ceacc-126">Šifrování na úrovni buněk Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ceacc-126">Azure SQL Cell Level Encryption</span></span>](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)
* [<span data-ttu-id="ceacc-127">Šifrování připojení Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ceacc-127">Azure SQL Connection Encryption</span></span>](../sql-database/sql-database-control-access.md)
* [<span data-ttu-id="ceacc-128">Ověřování Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ceacc-128">Azure SQL Authentication</span></span>](../sql-database/sql-database-control-access.md)
* [<span data-ttu-id="ceacc-129">Azure SQL vždy šifrování</span><span class="sxs-lookup"><span data-stu-id="ceacc-129">Azure SQL Always Encryption</span></span>](https://msdn.microsoft.com/library/mt163865.aspx)
* [<span data-ttu-id="ceacc-130">Šifrování na úrovni sloupce Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ceacc-130">Azure SQL Column Level Encryption</span></span>](https://msdn.microsoft.com/library/ms179331.aspx)
* [<span data-ttu-id="ceacc-131">Azure SQL transparentní šifrování dat</span><span class="sxs-lookup"><span data-stu-id="ceacc-131">Azure SQL Transparent Data Encryption</span></span>](https://msdn.microsoft.com/library/dn948096.aspx)
* [<span data-ttu-id="ceacc-132">Databáze Azure SQL auditování</span><span class="sxs-lookup"><span data-stu-id="ceacc-132">Azure SQL Database Auditing</span></span>](../sql-database/sql-database-auditing.md)

## <a name="azure-identity-and-access-management"></a><span data-ttu-id="ceacc-133">Azure identita a správa přístupu</span><span class="sxs-lookup"><span data-stu-id="ceacc-133">Azure Identity and Access Management</span></span>
* [<span data-ttu-id="ceacc-134">Řízení přístupu na základě Azure Role</span><span class="sxs-lookup"><span data-stu-id="ceacc-134">Azure Role Based Access Control</span></span>](../active-directory/role-based-access-control-configure.md)
* [<span data-ttu-id="ceacc-135">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ceacc-135">Azure Active Directory</span></span>](../active-directory/active-directory-whatis.md)
* [<span data-ttu-id="ceacc-136">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="ceacc-136">Azure Active Directory B2C</span></span>](../active-directory-b2c/active-directory-b2c-get-started.md)
* [<span data-ttu-id="ceacc-137">Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="ceacc-137">Azure Active Directory Domain Services</span></span>](../active-directory-domain-services/active-directory-ds-overview.md)
* [<span data-ttu-id="ceacc-138">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="ceacc-138">Azure Multi-Factor Authentication</span></span>](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="backup-and-disaster-recovery"></a><span data-ttu-id="ceacc-139">Zálohování a zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="ceacc-139">Backup and Disaster Recovery</span></span>
* [<span data-ttu-id="ceacc-140">Azure Backup</span><span class="sxs-lookup"><span data-stu-id="ceacc-140">Azure Backup</span></span>](https://azure.microsoft.com/documentation/services/backup/)
* [<span data-ttu-id="ceacc-141">Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="ceacc-141">Azure Site Recovery</span></span>](https://azure.microsoft.com/documentation/services/site-recovery/)

## <a name="azure-networking"></a><span data-ttu-id="ceacc-142">Sítě Azure</span><span class="sxs-lookup"><span data-stu-id="ceacc-142">Azure Networking</span></span>
* [<span data-ttu-id="ceacc-143">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="ceacc-143">Network Security Groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="ceacc-144">Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="ceacc-144">Azure VPN Gateway</span></span>](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [<span data-ttu-id="ceacc-145">Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="ceacc-145">Azure Application Gateway</span></span>](../application-gateway/application-gateway-introduction.md)
* [<span data-ttu-id="ceacc-146">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="ceacc-146">Azure Load Balancer</span></span>](../load-balancer/load-balancer-overview.md)
* [<span data-ttu-id="ceacc-147">Azure ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="ceacc-147">Azure ExpressRoute</span></span>](../expressroute/expressroute-introduction.md)
* [<span data-ttu-id="ceacc-148">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="ceacc-148">Azure Traffic Manager</span></span>](../traffic-manager/traffic-manager-overview.md)
* [<span data-ttu-id="ceacc-149">Proxy aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="ceacc-149">Azure Application Proxy</span></span>](../active-directory/active-directory-application-proxy-enable.md)
