---
title: "aaaEnable transparentní šifrování dat v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolit transparentní dat šifrování **."
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
ms.openlocfilehash: 94c6e9a1feddaa48faac6c835d416c4d131cd5c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a><span data-ttu-id="29f2f-103">Povolit transparentní šifrování dat v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="29f2f-103">Enable Transparent Data Encryption in Azure Security Center</span></span>
<span data-ttu-id="29f2f-104">Azure Security Center doporučí, povolte šifrování transparentní dat (TDE) u databází SQL, pokud již není povolené šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="29f2f-104">Azure Security Center will recommend that you enable Transparent Data Encryption (TDE) on SQL databases if TDE is not already enabled.</span></span> <span data-ttu-id="29f2f-105">Šifrování TDE chrání vaše data a pomáhá splnit požadavky na dodržování předpisů šifrováním vaší databáze, přidružených záloh a souborů protokolů transakci v klidu, bez nutnosti změny tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="29f2f-105">TDE protects your data and helps you meet compliance requirements by encrypting your database, associated backups, and transaction log files at rest, without requiring changes tooyour application.</span></span> <span data-ttu-id="29f2f-106">víc najdete v části toolearn [transparentní šifrování dat s Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span><span class="sxs-lookup"><span data-stu-id="29f2f-106">toolearn more see [Transparent Data Encryption with Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span></span>

<span data-ttu-id="29f2f-107">Toto doporučení platí toohello pouze; služby Azure SQL neobsahuje SQL běžících na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="29f2f-107">This recommendation applies toohello Azure SQL service only; doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="29f2f-108">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="29f2f-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="29f2f-109">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="29f2f-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="29f2f-110">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="29f2f-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="29f2f-111">V hello **doporučení** vyberte **povolit transparentní šifrování dat**.</span><span class="sxs-lookup"><span data-stu-id="29f2f-111">In hello **Recommendations** blade, select **Enable Transparent Data Encryption**.</span></span>
   <span data-ttu-id="29f2f-112">![Povolení transparentního šifrování dat][1]</span><span class="sxs-lookup"><span data-stu-id="29f2f-112">![Enable Transparent Data Encryption][1]</span></span>
2. <span data-ttu-id="29f2f-113">Tím se otevře hello **povolit transparentní šifrování dat v databázích SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="29f2f-113">This opens hello **Enable Transparent Data Encryption on SQL databases** blade.</span></span> <span data-ttu-id="29f2f-114">Vyberte SQL databázi tooenable TDE na.</span><span class="sxs-lookup"><span data-stu-id="29f2f-114">Select a SQL database tooenable TDE on.</span></span>
   <span data-ttu-id="29f2f-115">![Vyberte databázi SQL tooenable TDE na][2]</span><span class="sxs-lookup"><span data-stu-id="29f2f-115">![Select SQL DB tooenable TDE on][2]</span></span>
3. <span data-ttu-id="29f2f-116">Na hello **transparentní šifrování dat** vyberte **ON** pod šifrování dat a vyberte **Uložit** nejvyšší pásu karet hello hello okna.</span><span class="sxs-lookup"><span data-stu-id="29f2f-116">On hello **Transparent data encryption** blade, select **ON** under Data encryption and select **Save** in hello top ribbon of hello blade.</span></span>
   <span data-ttu-id="29f2f-117">![Zapnout šifrování TDE][3]</span><span class="sxs-lookup"><span data-stu-id="29f2f-117">![Turn on TDE][3]</span></span>

   <span data-ttu-id="29f2f-118">Jakmile je povolené šifrování TDE na hello vybrané databáze SQL, hello **stav šifrování** změní příliš**šifrovaný**.</span><span class="sxs-lookup"><span data-stu-id="29f2f-118">Once TDE is enabled on hello selected SQL database, hello **Encryption status** will change too**Encrypted**.</span></span>    

   ![Stav šifrování][4]

## <a name="see-also"></a><span data-ttu-id="29f2f-120">Viz také</span><span class="sxs-lookup"><span data-stu-id="29f2f-120">See also</span></span>
<span data-ttu-id="29f2f-121">Tento článek ukázal, jak tooimplement hello Security Center doporučení "Povolit transparentní šifrování dat."</span><span class="sxs-lookup"><span data-stu-id="29f2f-121">This article showed you how tooimplement hello Security Center recommendation "Enable Transparent Data Encryption."</span></span> <span data-ttu-id="29f2f-122">toolearn Další informace o šifrování TDE SQL, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="29f2f-122">toolearn more about SQL TDE, see hello following:</span></span>

* [<span data-ttu-id="29f2f-123">Transparentní šifrování dat s databází Azure SQL</span><span class="sxs-lookup"><span data-stu-id="29f2f-123">Transparent Data Encryption with Azure SQL Database</span></span>](https://msdn.microsoft.com/library/dn948096)
* [<span data-ttu-id="29f2f-124">Začínáme s transparentní šifrování dat (TDE)</span><span class="sxs-lookup"><span data-stu-id="29f2f-124">Get started with Transparent Data Encryption (TDE)</span></span>](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

<span data-ttu-id="29f2f-125">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="29f2f-125">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="29f2f-126">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="29f2f-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="29f2f-127">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="29f2f-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="29f2f-128">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="29f2f-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="29f2f-129">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="29f2f-129">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="29f2f-130">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="29f2f-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="29f2f-131">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="29f2f-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="29f2f-132">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="29f2f-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
