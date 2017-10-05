---
title: "Povolení auditování a zjišťování hrozeb na serverech SQL v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** povolení auditování a detekce hrozeb na SQL servery **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 042fca4d-7dab-4172-8614-e8c21ccb4960
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: 660b537aef8d175a478ff93d60b8391d55fc92ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a><span data-ttu-id="bcbb7-103">Povolení auditování a zjišťování hrozeb na serverech SQL v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="bcbb7-103">Enable auditing and threat detection on SQL servers in Azure Security Center</span></span>
<span data-ttu-id="bcbb7-104">Azure Security Center bude vhodné zapnout auditování a detekce hrozeb pro všechny databáze na serverech Azure SQL, pokud auditování již není povolena.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-104">Azure Security Center will recommend that you turn on auditing and threat detection for all databases on your Azure SQL servers if auditing is not already enabled.</span></span> <span data-ttu-id="bcbb7-105">Auditování a hrozeb, že detekce vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="bcbb7-106">Jakmile jste zapnuli auditování můžete nakonfigurovat nastavení detekce hrozeb a e-mailů, aby výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails to receive security alerts.</span></span> <span data-ttu-id="bcbb7-107">Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální ohrožení databáze.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-107">Threat Detection detects anomalous database activities indicating potential security threats to the database.</span></span> <span data-ttu-id="bcbb7-108">To vám umožňuje zjistit a reagovat na potenciální hrozby, kdy k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-108">This enables you to detect and respond to potential threats as they occur.</span></span>

<span data-ttu-id="bcbb7-109">Toto doporučení se vztahují ke službě Azure SQL pouze; neobsahuje SQL Server běžící na virtuálních počítačů ve službách infrastruktury Azure (Azure IaaS).</span><span class="sxs-lookup"><span data-stu-id="bcbb7-109">This recommendation applies to the Azure SQL service only; it doesn’t include SQL Server running on your virtual machines in Azure Infrastructure Services (Azure IaaS).</span></span>

> [!NOTE]
> <span data-ttu-id="bcbb7-110">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="bcbb7-111">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="bcbb7-112">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="bcbb7-112">Implement the recommendation</span></span>
1. <span data-ttu-id="bcbb7-113">V **doporučení** vyberte **povolení auditování a detekce hrozeb na serverech SQL**.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-113">In the **Recommendations** blade, select **Enable Auditing & Threat detection on SQL servers**.</span></span>  <span data-ttu-id="bcbb7-114">Tím se otevře **povolení auditování a detekce hrozeb na serverech SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-114">This opens the **Enable Auditing & Threat detection on SQL servers** blade.</span></span>

   ![Povolení auditování pro servery SQL][1]
2. <span data-ttu-id="bcbb7-116">Zvolte server SQL pro povolení detekce hrozeb a auditování na.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-116">Select a SQL server to enable auditing and threat detection on.</span></span> <span data-ttu-id="bcbb7-117">Tím se otevře **auditování a detekce hrozeb** okno.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-117">This opens the **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="bcbb7-118">Na **auditování a detekce hrozeb** vyberte **ON** v části **auditování**.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-118">On the **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Zapnout nastavení auditování][2]
4. <span data-ttu-id="bcbb7-120">Postupujte podle kroků v [auditování databáze SQL na webu Azure portal](../sql-database/sql-database-auditing-portal.md) konfigurace úložiště, kde bude uložena vaše protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-120">Follow the steps in [SQL database auditing in the Azure portal](../sql-database/sql-database-auditing-portal.md) to configure storage where your audit logs will be stored.</span></span> <span data-ttu-id="bcbb7-121">Účet úložiště odběru pro shromažďování dat je výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-121">The subscription's storage account for data collection is the default storage account.</span></span>
5. <span data-ttu-id="bcbb7-122">Postupujte podle kroků v [začít pracovat s detekce hrozeb databáze SQL](../sql-database/sql-database-threat-detection.md) zapnout a nakonfigurovat detekce hrozeb a ke konfiguraci seznamu e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklých aktivit.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-122">Follow the steps in [Get started with SQL Database Threat Detection](../sql-database/sql-database-threat-detection.md) to turn on and configure Threat Detection and to configure the list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="bcbb7-123">Viz také</span><span class="sxs-lookup"><span data-stu-id="bcbb7-123">See also</span></span>
<span data-ttu-id="bcbb7-124">Tento článek vám ukázal, jak implementovat Security Center doporučení "Povolit auditování a zjišťování hrozeb na serverech SQL."</span><span class="sxs-lookup"><span data-stu-id="bcbb7-124">This article showed you how to implement the Security Center recommendation "Enable auditing and threat detection on SQL servers."</span></span> <span data-ttu-id="bcbb7-125">Další informace o zabezpečení databáze SQL, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="bcbb7-125">To learn more about securing your SQL database, see the following:</span></span>

* [<span data-ttu-id="bcbb7-126">Zabezpečení služby SQL Database</span><span class="sxs-lookup"><span data-stu-id="bcbb7-126">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="bcbb7-127">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="bcbb7-127">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="bcbb7-128">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="bcbb7-129">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="bcbb7-130">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="bcbb7-131">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-131">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="bcbb7-132">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="bcbb7-133">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="bcbb7-134">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.</span><span class="sxs-lookup"><span data-stu-id="bcbb7-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
