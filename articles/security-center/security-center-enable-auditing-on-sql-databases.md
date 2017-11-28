---
title: "aaaEnable auditování a zjišťování hrozeb SQL databáze v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolit auditování a zjišťování hrozeb v SQL databáze **."
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
ms.openlocfilehash: c94140acf37cabaca3e681ba5db79d6827e7b9db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a><span data-ttu-id="e6fe4-103">Povolení auditování a zjišťování hrozeb u databází SQL v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="e6fe4-103">Enable auditing and threat detection on SQL databases in Azure Security Center</span></span>
<span data-ttu-id="e6fe4-104">Azure Security Center bude doporučujeme vám zapnout auditování a zjišťování hrozeb pro všechny SQL databáze, pokud auditování a detekce hrozeb ještě není povolené.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-104">Azure Security Center will recommend that you turn on auditing and threat detection for all SQL databases if auditing and threat detection is not already enabled.</span></span> <span data-ttu-id="e6fe4-105">Auditování a hrozeb, že detekce vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="e6fe4-106">Jakmile jste zapnuli auditování můžete konfigurovat výstrahy zabezpečení tooreceive detekce hrozeb pro nastavení a e-mailů.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails tooreceive security alerts.</span></span> <span data-ttu-id="e6fe4-107">Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="e6fe4-108">To vám umožní toodetect a hrozeb toopotential reagovat, když k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-108">This enables you toodetect and respond toopotential threats as they occur.</span></span>

<span data-ttu-id="e6fe4-109">Toto doporučení platí toohello pouze; služby Azure SQL neobsahuje SQL běžících na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-109">This recommendation applies toohello Azure SQL service only; it doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="e6fe4-110">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="e6fe4-111">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="e6fe4-112">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="e6fe4-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="e6fe4-113">V hello **doporučení** vyberte **povolení auditování a detekce hrozeb v databázích SQL**.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-113">In hello **Recommendations** blade, select **Enable Auditing & Threat detection on SQL databases**.</span></span>  <span data-ttu-id="e6fe4-114">Tím se otevře hello **povolení auditování a detekce hrozeb v databázích SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-114">This opens hello **Enable Auditing & Threat detection on SQL databases** blade.</span></span>

   ![Povolení auditování pro databáze SQL][1]
2. <span data-ttu-id="e6fe4-116">Vyberte auditování databáze SQL tooenable na.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-116">Select a SQL database tooenable auditing on.</span></span> <span data-ttu-id="e6fe4-117">Tím se otevře hello **auditování a detekce hrozeb** okno.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-117">This opens hello **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="e6fe4-118">Na hello **auditování a detekce hrozeb** vyberte **ON** pod **auditování**.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-118">On hello **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Zapnout auditování a zjišťování hrozeb][2]
4. <span data-ttu-id="e6fe4-120">Postupujte podle kroků hello v [detekce hrozeb databáze SQL v hello portál Azure](../sql-database/sql-database-threat-detection-portal.md) tooturn na a konfigurace detekce hrozeb a tooconfigure hello seznam e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklých aktivit.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-120">Follow hello steps in [SQL Database Threat Detection in hello Azure portal](../sql-database/sql-database-threat-detection-portal.md) tooturn on and configure Threat Detection and tooconfigure hello list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="e6fe4-121">Viz také</span><span class="sxs-lookup"><span data-stu-id="e6fe4-121">See also</span></span>
<span data-ttu-id="e6fe4-122">Tento článek ukázal, jak tooimplement hello Security Center doporučení "Povolit auditování a detekce hrozeb v databázích SQL."</span><span class="sxs-lookup"><span data-stu-id="e6fe4-122">This article showed you how tooimplement hello Security Center recommendation "Enable Auditing & Threat detection on SQL databases."</span></span> <span data-ttu-id="e6fe4-123">toolearn Další informace o zabezpečení databáze SQL, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="e6fe4-123">toolearn more about securing your SQL database, see hello following:</span></span>

* [<span data-ttu-id="e6fe4-124">Zabezpečení služby SQL Database</span><span class="sxs-lookup"><span data-stu-id="e6fe4-124">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="e6fe4-125">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="e6fe4-125">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="e6fe4-126">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="e6fe4-127">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="e6fe4-128">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="e6fe4-129">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-129">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="e6fe4-130">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="e6fe4-131">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="e6fe4-132">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="e6fe4-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
