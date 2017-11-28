---
title: "aaaOperations Management Suite zabezpečení a Audit řešení Data zabezpečení | Microsoft Docs"
description: "Tento dokument popisuje způsob správy a ochrany dat v řešení Zabezpečení a audit pro Operations Management Suite."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 9c4181b3b491e4f7f0c57d7252eca78a819722d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a><span data-ttu-id="a8bd1-103">Zabezpečení dat v řešení Zabezpečení a audit pro Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="a8bd1-103">Operations Management Suite Security and Audit solution data security</span></span>
<span data-ttu-id="a8bd1-104">toohelp zákazníkům zabránit, zjistit a reagovat toothreats, [Operations Management Suite (OMS) zabezpečení a Audit řešení](operations-management-suite-overview.md) shromažďuje a zpracovává data o vašich prostředků, která zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="a8bd1-104">toohelp customers prevent, detect, and respond toothreats, [Operations Management Suite  (OMS) Security and Audit Solution](operations-management-suite-overview.md) collects and processes data about your resources, which includes:</span></span>

* <span data-ttu-id="a8bd1-105">Protokol událostí zabezpečení</span><span class="sxs-lookup"><span data-stu-id="a8bd1-105">Security event log</span></span>
* <span data-ttu-id="a8bd1-106">Události Trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="a8bd1-106">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="a8bd1-107">Události auditování AppLocker</span><span class="sxs-lookup"><span data-stu-id="a8bd1-107">AppLocker auditing events</span></span>
* <span data-ttu-id="a8bd1-108">Protokol brány Windows Firewall</span><span class="sxs-lookup"><span data-stu-id="a8bd1-108">Windows Firewall log</span></span>
* <span data-ttu-id="a8bd1-109">Události Rozšířené analýzy hrozeb</span><span class="sxs-lookup"><span data-stu-id="a8bd1-109">Advanced Threat Analytics events</span></span>
* <span data-ttu-id="a8bd1-110">Výsledky základního vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="a8bd1-110">Results of baseline assessment</span></span>
* <span data-ttu-id="a8bd1-111">Výsledky antimalwarového vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="a8bd1-111">Results of antimalware assessment</span></span>
* <span data-ttu-id="a8bd1-112">Výsledky vyhodnocování aktualizací a oprav</span><span class="sxs-lookup"><span data-stu-id="a8bd1-112">Results of update/patch assessment</span></span>
* <span data-ttu-id="a8bd1-113">Datové proudy systémové protokoly, které jsou explicitně povoleno na hello agenta</span><span class="sxs-lookup"><span data-stu-id="a8bd1-113">Syslogs streams that are explicitly enabled on hello agent</span></span>

<span data-ttu-id="a8bd1-114">Provedeme silné závazky tooprotect hello ochrany osobních údajů a zabezpečení tato data.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-114">We make strong commitments tooprotect hello privacy and security of this data.</span></span> <span data-ttu-id="a8bd1-115">Microsoft dodržuje pravidla dodržování předpisů a zabezpečení toostrict – z kódování toooperating služby.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-115">Microsoft adheres toostrict compliance and security guidelines—from coding toooperating a service.</span></span>
<span data-ttu-id="a8bd1-116">Tento článek popisuje způsob správy a ochrany dat v řešení Zabezpečení a audit v OMS.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-116">This article explains how data is managed and safeguarded in OMS Security and Audit Solution.</span></span>

## <a name="data-sources"></a><span data-ttu-id="a8bd1-117">Zdroje dat</span><span class="sxs-lookup"><span data-stu-id="a8bd1-117">Data sources</span></span>
<span data-ttu-id="a8bd1-118">OMS zabezpečení a Audit řešení analyzovat data ze své virtuální počítače a fyzické počítače, kde je nainstalován hello OMS Agent.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-118">OMS Security and Audit Solution analyze data from your Virtual Machines and physical computers where hello OMS Agent is installed.</span></span> <span data-ttu-id="a8bd1-119">Řešení Zabezpečení a audit v OMS může shromažďovat informace o konfiguraci událostí zabezpečení, jako jsou například protokoly událostí a auditu systému Windows, protokoly služby IIS a zprávy syslog.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-119">OMS Security and Audit Solution can collect configuration information about security events, such as Windows event, audit logs, IIS logs and syslog messages.</span></span> <span data-ttu-id="a8bd1-120">Mezi příklady těchto dat patří: typ a verze operačního systému, spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID tenanta.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-120">Examples of such data are: operating system type and version, running processes, machine name, IP addresses, logged in user, and tenant ID.</span></span>  

## <a name="data-protection"></a><span data-ttu-id="a8bd1-121">Ochrana dat</span><span class="sxs-lookup"><span data-stu-id="a8bd1-121">Data protection</span></span>
<span data-ttu-id="a8bd1-122">**Oddělení dat**: Data se ukládají na jednotlivých součástí v rámci služby hello logicky samostatné.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-122">**Data segregation**: Data is kept logically separate on each component throughout hello service.</span></span> <span data-ttu-id="a8bd1-123">Všechna data jsou označená podle organizace.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-123">All data is tagged per organization.</span></span> <span data-ttu-id="a8bd1-124">Toto značení přetrvává v průběhu cyklu hello dat a je požadováno v jednotlivých vrstvách služby hello.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-124">This tagging persists throughout hello data lifecycle, and it is enforced at each layer of hello service.</span></span> 

<span data-ttu-id="a8bd1-125">**Přístup k datům**: tooprovide doporučení zabezpečení a prozkoumat potenciální ohrožení zabezpečení, pracovníky společnosti Microsoft může přistupovat k informacím shromažďovaných či analyzovat služby.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-125">**Data access**: tooprovide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="a8bd1-126">Jsme splňovat toohello [Microsoft Online Services podmínky](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [prohlášení o ochraně osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), který stavu, že Microsoft nebude používat Data zákazníků nebo odvození informací z něj jakémukoliv inzerování nebo podobné obchodní účely.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-126">We adhere toohello [Microsoft Online Services Terms](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) and [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), which state that Microsoft will not use Customer Data or derive information from it for any advertising or similar commercial purposes.</span></span> <span data-ttu-id="a8bd1-127">doporučení zabezpečení tooprovide a prozkoumat potenciální ohrožení zabezpečení, pracovníky společnosti Microsoft může přistupovat k informacím shromažďovaných či analyzovat služby.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-127">tooprovide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="a8bd1-128">Data zákazníků používáme pouze jako potřebné tooprovide vám Azure služby, včetně účely, které jsou kompatibilní s poskytování těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-128">We only use Customer Data as needed tooprovide you with Azure services, including purposes compatible with providing those services.</span></span> <span data-ttu-id="a8bd1-129">Je-li zachovat všechna práva tooyour vlastní data.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-129">You retain all rights tooyour own data.</span></span>

<span data-ttu-id="a8bd1-130">**Data použití**: Společnost Microsoft používá vzory a analýzou hrozeb vidět napříč více klienty tooenhance naše funkce prevence a detekce; jsme učinit v souladu s závazky týkajícími se ochrany osobních údajů hello popsané v našem [ochrany osobních údajů Příkaz](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8bd1-130">**Data use**: Microsoft uses patterns and threat intelligence seen across multiple tenants tooenhance our prevention and detection capabilities; we do so in accordance with hello privacy commitments described in our [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="a8bd1-131">Umístění dat je konfigurována na úrovni pracovní prostor OMS hello, při vytváření pracovního prostoru hello, která je součástí hello počáteční OMS zabezpečení a Audit proces konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-131">Data location is configured at hello OMS workspace level, during hello workspace creation, which is part of hello initial OMS Security and Audit configuration process.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="a8bd1-132">Viz také</span><span class="sxs-lookup"><span data-stu-id="a8bd1-132">See also</span></span>
<span data-ttu-id="a8bd1-133">V tomto dokumentu jste se dozvěděli, jakým způsobem probíhá správa a ochrana dat v OMS.</span><span class="sxs-lookup"><span data-stu-id="a8bd1-133">In this document, you learned how data is managed and safeguarded in OMS.</span></span> <span data-ttu-id="a8bd1-134">toolearn informace o OMS zabezpečení a Audit řešení, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="a8bd1-134">toolearn more about OMS Security and Audit solution, see:</span></span>

* [<span data-ttu-id="a8bd1-135">Přehled Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="a8bd1-135">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="a8bd1-136">Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení</span><span class="sxs-lookup"><span data-stu-id="a8bd1-136">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="a8bd1-137">Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="a8bd1-137">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

