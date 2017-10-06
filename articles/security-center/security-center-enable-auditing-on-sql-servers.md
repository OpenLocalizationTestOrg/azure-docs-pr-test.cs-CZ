---
title: "aaaEnable auditování a zjišťování hrozeb na serverech SQL v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolení auditování a detekce hrozeb na SQL servery **."
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
ms.openlocfilehash: b082c48cdbc386f14e677f4e13a7f306f37fd0e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a><span data-ttu-id="de7fc-103">Povolení auditování a zjišťování hrozeb na serverech SQL v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="de7fc-103">Enable auditing and threat detection on SQL servers in Azure Security Center</span></span>
<span data-ttu-id="de7fc-104">Azure Security Center bude vhodné zapnout auditování a detekce hrozeb pro všechny databáze na serverech Azure SQL, pokud auditování již není povolena.</span><span class="sxs-lookup"><span data-stu-id="de7fc-104">Azure Security Center will recommend that you turn on auditing and threat detection for all databases on your Azure SQL servers if auditing is not already enabled.</span></span> <span data-ttu-id="de7fc-105">Auditování a hrozeb, že detekce vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="de7fc-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="de7fc-106">Jakmile jste zapnuli auditování můžete konfigurovat výstrahy zabezpečení tooreceive detekce hrozeb pro nastavení a e-mailů.</span><span class="sxs-lookup"><span data-stu-id="de7fc-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails tooreceive security alerts.</span></span> <span data-ttu-id="de7fc-107">Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="de7fc-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="de7fc-108">To vám umožní toodetect a hrozeb toopotential reagovat, když k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="de7fc-108">This enables you toodetect and respond toopotential threats as they occur.</span></span>

<span data-ttu-id="de7fc-109">Toto doporučení platí toohello pouze; služby Azure SQL neobsahuje SQL Server běžící na virtuálních počítačů ve službách infrastruktury Azure (Azure IaaS).</span><span class="sxs-lookup"><span data-stu-id="de7fc-109">This recommendation applies toohello Azure SQL service only; it doesn’t include SQL Server running on your virtual machines in Azure Infrastructure Services (Azure IaaS).</span></span>

> [!NOTE]
> <span data-ttu-id="de7fc-110">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="de7fc-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="de7fc-111">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="de7fc-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="de7fc-112">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="de7fc-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="de7fc-113">V hello **doporučení** vyberte **povolení auditování a detekce hrozeb na serverech SQL**.</span><span class="sxs-lookup"><span data-stu-id="de7fc-113">In hello **Recommendations** blade, select **Enable Auditing & Threat detection on SQL servers**.</span></span>  <span data-ttu-id="de7fc-114">Tím se otevře hello **povolení auditování a detekce hrozeb na serverech SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="de7fc-114">This opens hello **Enable Auditing & Threat detection on SQL servers** blade.</span></span>

   ![Povolení auditování pro servery SQL][1]
2. <span data-ttu-id="de7fc-116">Vyberte SQL server tooenable auditování a zjišťování hrozeb na.</span><span class="sxs-lookup"><span data-stu-id="de7fc-116">Select a SQL server tooenable auditing and threat detection on.</span></span> <span data-ttu-id="de7fc-117">Tím se otevře hello **auditování a detekce hrozeb** okno.</span><span class="sxs-lookup"><span data-stu-id="de7fc-117">This opens hello **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="de7fc-118">Na hello **auditování a detekce hrozeb** vyberte **ON** pod **auditování**.</span><span class="sxs-lookup"><span data-stu-id="de7fc-118">On hello **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Zapnout nastavení auditování][2]
4. <span data-ttu-id="de7fc-120">Postupujte podle kroků hello v [auditování databáze SQL v portálu Azure hello](../sql-database/sql-database-auditing-portal.md) tooconfigure úložiště, kde bude uložena vaše protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="de7fc-120">Follow hello steps in [SQL database auditing in hello Azure portal](../sql-database/sql-database-auditing-portal.md) tooconfigure storage where your audit logs will be stored.</span></span> <span data-ttu-id="de7fc-121">Hello odběru účet úložiště pro shromažďování dat je hello výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="de7fc-121">hello subscription's storage account for data collection is hello default storage account.</span></span>
5. <span data-ttu-id="de7fc-122">Postupujte podle kroků hello v [začít pracovat s detekce hrozeb databáze SQL](../sql-database/sql-database-threat-detection.md) tooturn na a konfigurace detekce hrozeb a tooconfigure hello seznam e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklých aktivit.</span><span class="sxs-lookup"><span data-stu-id="de7fc-122">Follow hello steps in [Get started with SQL Database Threat Detection](../sql-database/sql-database-threat-detection.md) tooturn on and configure Threat Detection and tooconfigure hello list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="de7fc-123">Viz také</span><span class="sxs-lookup"><span data-stu-id="de7fc-123">See also</span></span>
<span data-ttu-id="de7fc-124">Tento článek ukázal, jak tooimplement hello Security Center doporučení "Povolení auditování a detekce na serverech SQL hrozby."</span><span class="sxs-lookup"><span data-stu-id="de7fc-124">This article showed you how tooimplement hello Security Center recommendation "Enable auditing and threat detection on SQL servers."</span></span> <span data-ttu-id="de7fc-125">toolearn Další informace o zabezpečení databáze SQL, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="de7fc-125">toolearn more about securing your SQL database, see hello following:</span></span>

* [<span data-ttu-id="de7fc-126">Zabezpečení služby SQL Database</span><span class="sxs-lookup"><span data-stu-id="de7fc-126">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="de7fc-127">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="de7fc-127">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="de7fc-128">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="de7fc-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="de7fc-129">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="de7fc-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="de7fc-130">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="de7fc-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="de7fc-131">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="de7fc-131">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="de7fc-132">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="de7fc-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="de7fc-133">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="de7fc-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="de7fc-134">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="de7fc-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
