---
title: "Povolení auditování a zjišťování hrozeb u databází SQL v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** povolit auditování a zjišťování hrozeb v SQL databáze **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: 8f4febdaa4497fee0dc690b59cd6eaa415c5e5cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a><span data-ttu-id="ce366-103">Povolení auditování a zjišťování hrozeb u databází SQL v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ce366-103">Enable auditing and threat detection on SQL databases in Azure Security Center</span></span>
<span data-ttu-id="ce366-104">Azure Security Center bude doporučujeme vám zapnout auditování a zjišťování hrozeb pro všechny SQL databáze, pokud auditování a detekce hrozeb ještě není povolené.</span><span class="sxs-lookup"><span data-stu-id="ce366-104">Azure Security Center will recommend that you turn on auditing and threat detection for all SQL databases if auditing and threat detection is not already enabled.</span></span> <span data-ttu-id="ce366-105">Auditování a hrozeb, že detekce vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="ce366-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="ce366-106">Jakmile jste zapnuli auditování můžete nakonfigurovat nastavení detekce hrozeb a e-mailů, aby výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="ce366-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails to receive security alerts.</span></span> <span data-ttu-id="ce366-107">Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální ohrožení databáze.</span><span class="sxs-lookup"><span data-stu-id="ce366-107">Threat Detection detects anomalous database activities indicating potential security threats to the database.</span></span> <span data-ttu-id="ce366-108">To vám umožňuje zjistit a reagovat na potenciální hrozby, kdy k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="ce366-108">This enables you to detect and respond to potential threats as they occur.</span></span>

<span data-ttu-id="ce366-109">Toto doporučení se vztahují ke službě Azure SQL pouze; neobsahuje SQL běžících na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="ce366-109">This recommendation applies to the Azure SQL service only; it doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="ce366-110">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="ce366-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="ce366-111">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="ce366-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="ce366-112">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="ce366-112">Implement the recommendation</span></span>
1. <span data-ttu-id="ce366-113">V **doporučení** vyberte **povolení auditování a detekce hrozeb v databázích SQL**.</span><span class="sxs-lookup"><span data-stu-id="ce366-113">In the **Recommendations** blade, select **Enable Auditing & Threat detection on SQL databases**.</span></span>  <span data-ttu-id="ce366-114">Tím se otevře **povolení auditování a detekce hrozeb v databázích SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="ce366-114">This opens the **Enable Auditing & Threat detection on SQL databases** blade.</span></span>

   ![Povolení auditování pro databáze SQL][1]
2. <span data-ttu-id="ce366-116">Vyberte databázi SQL, chcete-li povolit auditování.</span><span class="sxs-lookup"><span data-stu-id="ce366-116">Select a SQL database to enable auditing on.</span></span> <span data-ttu-id="ce366-117">Tím se otevře **auditování a detekce hrozeb** okno.</span><span class="sxs-lookup"><span data-stu-id="ce366-117">This opens the **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="ce366-118">Na **auditování a detekce hrozeb** vyberte **ON** v části **auditování**.</span><span class="sxs-lookup"><span data-stu-id="ce366-118">On the **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Zapnout auditování a zjišťování hrozeb][2]
4. <span data-ttu-id="ce366-120">Postupujte podle kroků v [detekce hrozeb databáze SQL na webu Azure portal](../sql-database/sql-database-threat-detection-portal.md) zapnout a nakonfigurovat detekce hrozeb a ke konfiguraci seznamu e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklých aktivit.</span><span class="sxs-lookup"><span data-stu-id="ce366-120">Follow the steps in [SQL Database Threat Detection in the Azure portal](../sql-database/sql-database-threat-detection-portal.md) to turn on and configure Threat Detection and to configure the list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="ce366-121">Viz také</span><span class="sxs-lookup"><span data-stu-id="ce366-121">See also</span></span>
<span data-ttu-id="ce366-122">Tento článek vám ukázal, jak provést doporučení Security Center "Povolit auditování a detekce hrozeb v databázích SQL."</span><span class="sxs-lookup"><span data-stu-id="ce366-122">This article showed you how to implement the Security Center recommendation "Enable Auditing & Threat detection on SQL databases."</span></span> <span data-ttu-id="ce366-123">Další informace o zabezpečení databáze SQL, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="ce366-123">To learn more about securing your SQL database, see the following:</span></span>

* [<span data-ttu-id="ce366-124">Zabezpečení služby SQL Database</span><span class="sxs-lookup"><span data-stu-id="ce366-124">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="ce366-125">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="ce366-125">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="ce366-126">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="ce366-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="ce366-127">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="ce366-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="ce366-128">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="ce366-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="ce366-129">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="ce366-129">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="ce366-130">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="ce366-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="ce366-131">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="ce366-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="ce366-132">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.</span><span class="sxs-lookup"><span data-stu-id="ce366-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
