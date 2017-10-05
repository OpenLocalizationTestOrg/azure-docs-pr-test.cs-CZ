---
title: "Povolit transparentní šifrování dat v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** povolit transparentní dat šifrování **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2a2963affdbff3710ad08f86c6ed4e6304335559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a><span data-ttu-id="2d071-103">Povolit transparentní šifrování dat v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="2d071-103">Enable Transparent Data Encryption in Azure Security Center</span></span>
<span data-ttu-id="2d071-104">Azure Security Center doporučí, povolte šifrování transparentní dat (TDE) u databází SQL, pokud již není povolené šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="2d071-104">Azure Security Center will recommend that you enable Transparent Data Encryption (TDE) on SQL databases if TDE is not already enabled.</span></span> <span data-ttu-id="2d071-105">Šifrování TDE chrání vaše data a pomáhá splnit požadavky na dodržování předpisů šifrováním vaší databáze, přidružených záloh a souborů protokolů transakci v klidu, bez nutnosti změny do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d071-105">TDE protects your data and helps you meet compliance requirements by encrypting your database, associated backups, and transaction log files at rest, without requiring changes to your application.</span></span> <span data-ttu-id="2d071-106">Další informace najdete v další [transparentní šifrování dat s Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span><span class="sxs-lookup"><span data-stu-id="2d071-106">To learn more see [Transparent Data Encryption with Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span></span>

<span data-ttu-id="2d071-107">Toto doporučení se vztahují ke službě Azure SQL pouze; neobsahuje SQL běžících na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="2d071-107">This recommendation applies to the Azure SQL service only; doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="2d071-108">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="2d071-108">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="2d071-109">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="2d071-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="2d071-110">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="2d071-110">Implement the recommendation</span></span>
1. <span data-ttu-id="2d071-111">V **doporučení** vyberte **povolit transparentní šifrování dat**.</span><span class="sxs-lookup"><span data-stu-id="2d071-111">In the **Recommendations** blade, select **Enable Transparent Data Encryption**.</span></span>
   <span data-ttu-id="2d071-112">![Povolení transparentního šifrování dat][1]</span><span class="sxs-lookup"><span data-stu-id="2d071-112">![Enable Transparent Data Encryption][1]</span></span>
2. <span data-ttu-id="2d071-113">Tím se otevře **povolit transparentní šifrování dat v databázích SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="2d071-113">This opens the **Enable Transparent Data Encryption on SQL databases** blade.</span></span> <span data-ttu-id="2d071-114">Vyberte databázi SQL, chcete-li povolit šifrování TDE na.</span><span class="sxs-lookup"><span data-stu-id="2d071-114">Select a SQL database to enable TDE on.</span></span>
   <span data-ttu-id="2d071-115">![Vyberte databázi SQL, povolit šifrování TDE na][2]</span><span class="sxs-lookup"><span data-stu-id="2d071-115">![Select SQL DB to enable TDE on][2]</span></span>
3. <span data-ttu-id="2d071-116">Na **transparentní šifrování dat** vyberte **ON** pod šifrování dat a vyberte **Uložit** na pásu karet na horní části okna.</span><span class="sxs-lookup"><span data-stu-id="2d071-116">On the **Transparent data encryption** blade, select **ON** under Data encryption and select **Save** in the top ribbon of the blade.</span></span>
   <span data-ttu-id="2d071-117">![Zapnout šifrování TDE][3]</span><span class="sxs-lookup"><span data-stu-id="2d071-117">![Turn on TDE][3]</span></span>

   <span data-ttu-id="2d071-118">Jakmile povolíte šifrování TDE na vybranou databázi SQL, **stav šifrování** se změní na **šifrovaný**.</span><span class="sxs-lookup"><span data-stu-id="2d071-118">Once TDE is enabled on the selected SQL database, the **Encryption status** will change to **Encrypted**.</span></span>    

   ![Stav šifrování][4]

## <a name="see-also"></a><span data-ttu-id="2d071-120">Viz také</span><span class="sxs-lookup"><span data-stu-id="2d071-120">See also</span></span>
<span data-ttu-id="2d071-121">Tento článek vám ukázal, jak provést doporučení Security Center "Povolit transparentní šifrování dat."</span><span class="sxs-lookup"><span data-stu-id="2d071-121">This article showed you how to implement the Security Center recommendation "Enable Transparent Data Encryption."</span></span> <span data-ttu-id="2d071-122">Další informace o šifrování TDE SQL, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="2d071-122">To learn more about SQL TDE, see the following:</span></span>

* [<span data-ttu-id="2d071-123">Transparentní šifrování dat s databází Azure SQL</span><span class="sxs-lookup"><span data-stu-id="2d071-123">Transparent Data Encryption with Azure SQL Database</span></span>](https://msdn.microsoft.com/library/dn948096)
* [<span data-ttu-id="2d071-124">Začínáme s transparentní šifrování dat (TDE)</span><span class="sxs-lookup"><span data-stu-id="2d071-124">Get started with Transparent Data Encryption (TDE)</span></span>](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

<span data-ttu-id="2d071-125">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="2d071-125">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="2d071-126">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2d071-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="2d071-127">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="2d071-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="2d071-128">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="2d071-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="2d071-129">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="2d071-129">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="2d071-130">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="2d071-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="2d071-131">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="2d071-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="2d071-132">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.</span><span class="sxs-lookup"><span data-stu-id="2d071-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
