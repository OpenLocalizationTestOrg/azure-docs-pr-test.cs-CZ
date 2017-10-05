---
title: "Zabezpečení dat v řešení Zabezpečení a audit pro Operations Management Suite | Dokumentace Microsoftu"
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
ms.openlocfilehash: 3b6327b1f5150f32afd71639f32c55d823f1d1f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a><span data-ttu-id="1d589-103">Zabezpečení dat v řešení Zabezpečení a audit pro Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="1d589-103">Operations Management Suite Security and Audit solution data security</span></span>
<span data-ttu-id="1d589-104">[Řešení Zabezpečení a audit pro Operations Management Suite (OMS)](operations-management-suite-overview.md) pomáhá zákazníkům předcházet hrozbám, rozpoznat je a reagovat na ně tím, že shromažďuje a zpracovává data o prostředcích, k nimž patří:</span><span class="sxs-lookup"><span data-stu-id="1d589-104">To help customers prevent, detect, and respond to threats, [Operations Management Suite  (OMS) Security and Audit Solution](operations-management-suite-overview.md) collects and processes data about your resources, which includes:</span></span>

* <span data-ttu-id="1d589-105">Protokol událostí zabezpečení</span><span class="sxs-lookup"><span data-stu-id="1d589-105">Security event log</span></span>
* <span data-ttu-id="1d589-106">Události Trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="1d589-106">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="1d589-107">Události auditování AppLocker</span><span class="sxs-lookup"><span data-stu-id="1d589-107">AppLocker auditing events</span></span>
* <span data-ttu-id="1d589-108">Protokol brány Windows Firewall</span><span class="sxs-lookup"><span data-stu-id="1d589-108">Windows Firewall log</span></span>
* <span data-ttu-id="1d589-109">Události Rozšířené analýzy hrozeb</span><span class="sxs-lookup"><span data-stu-id="1d589-109">Advanced Threat Analytics events</span></span>
* <span data-ttu-id="1d589-110">Výsledky základního vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="1d589-110">Results of baseline assessment</span></span>
* <span data-ttu-id="1d589-111">Výsledky antimalwarového vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="1d589-111">Results of antimalware assessment</span></span>
* <span data-ttu-id="1d589-112">Výsledky vyhodnocování aktualizací a oprav</span><span class="sxs-lookup"><span data-stu-id="1d589-112">Results of update/patch assessment</span></span>
* <span data-ttu-id="1d589-113">Datové proudy syslog explicitně povolené v agentu</span><span class="sxs-lookup"><span data-stu-id="1d589-113">Syslogs streams that are explicitly enabled on the agent</span></span>

<span data-ttu-id="1d589-114">Zavázali jsme se, že soukromí a bezpečnost těchto dat budeme chránit.</span><span class="sxs-lookup"><span data-stu-id="1d589-114">We make strong commitments to protect the privacy and security of this data.</span></span> <span data-ttu-id="1d589-115">Společnost Microsoft dodržuje přísné pokyny pro dodržování předpisů a zabezpečení – od psaní kódu po provoz služeb.</span><span class="sxs-lookup"><span data-stu-id="1d589-115">Microsoft adheres to strict compliance and security guidelines—from coding to operating a service.</span></span>
<span data-ttu-id="1d589-116">Tento článek popisuje způsob správy a ochrany dat v řešení Zabezpečení a audit v OMS.</span><span class="sxs-lookup"><span data-stu-id="1d589-116">This article explains how data is managed and safeguarded in OMS Security and Audit Solution.</span></span>

## <a name="data-sources"></a><span data-ttu-id="1d589-117">Zdroje dat</span><span class="sxs-lookup"><span data-stu-id="1d589-117">Data sources</span></span>
<span data-ttu-id="1d589-118">Řešení Zabezpečení a audit v OMS analyzuje data z virtuálních a fyzických počítačů s nainstalovaným agentem OMS.</span><span class="sxs-lookup"><span data-stu-id="1d589-118">OMS Security and Audit Solution analyze data from your Virtual Machines and physical computers where the OMS Agent is installed.</span></span> <span data-ttu-id="1d589-119">Řešení Zabezpečení a audit v OMS může shromažďovat informace o konfiguraci událostí zabezpečení, jako jsou například protokoly událostí a auditu systému Windows, protokoly služby IIS a zprávy syslog.</span><span class="sxs-lookup"><span data-stu-id="1d589-119">OMS Security and Audit Solution can collect configuration information about security events, such as Windows event, audit logs, IIS logs and syslog messages.</span></span> <span data-ttu-id="1d589-120">Mezi příklady těchto dat patří: typ a verze operačního systému, spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID tenanta.</span><span class="sxs-lookup"><span data-stu-id="1d589-120">Examples of such data are: operating system type and version, running processes, machine name, IP addresses, logged in user, and tenant ID.</span></span>  

## <a name="data-protection"></a><span data-ttu-id="1d589-121">Ochrana dat</span><span class="sxs-lookup"><span data-stu-id="1d589-121">Data protection</span></span>
<span data-ttu-id="1d589-122">**Oddělení dat**: Data se v rámci služby ukládají logicky oddělená pro jednotlivé komponenty.</span><span class="sxs-lookup"><span data-stu-id="1d589-122">**Data segregation**: Data is kept logically separate on each component throughout the service.</span></span> <span data-ttu-id="1d589-123">Všechna data jsou označená podle organizace.</span><span class="sxs-lookup"><span data-stu-id="1d589-123">All data is tagged per organization.</span></span> <span data-ttu-id="1d589-124">Toto značení přetrvává v průběhu celého životního cyklu dat a je vyžadováno na každé úrovni služby.</span><span class="sxs-lookup"><span data-stu-id="1d589-124">This tagging persists throughout the data lifecycle, and it is enforced at each layer of the service.</span></span> 

<span data-ttu-id="1d589-125">**Přístup k datům**: Aby bylo možné poskytovat doporučení týkající se zabezpečení a prošetřovat potenciální ohrožení zabezpečení, mají pracovníci společnosti Microsoft přístup k informacím shromažďovaným nebo analyzovaným službami.</span><span class="sxs-lookup"><span data-stu-id="1d589-125">**Data access**: To provide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="1d589-126">Dodržujeme [Podmínky online služeb společnosti Microsoft](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [Prohlášení o zásadách ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), ve kterých je uvedeno, že společnost Microsoft nebude informace o zákaznících používat ani z nich odvozovat další informace pro reklamní nebo podobné obchodní účely.</span><span class="sxs-lookup"><span data-stu-id="1d589-126">We adhere to the [Microsoft Online Services Terms](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) and [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), which state that Microsoft will not use Customer Data or derive information from it for any advertising or similar commercial purposes.</span></span> <span data-ttu-id="1d589-127">Aby bylo možné poskytovat doporučení týkající se zabezpečení a prošetřovat potenciální ohrožení zabezpečení, mají pracovníci společnosti Microsoft přístup k informacím shromažďovaným nebo analyzovaným službami.</span><span class="sxs-lookup"><span data-stu-id="1d589-127">To provide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="1d589-128">Informace o zákaznících podle potřeby používáme pouze k poskytování služeb Azure a k účelům slučitelným s poskytováním těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="1d589-128">We only use Customer Data as needed to provide you with Azure services, including purposes compatible with providing those services.</span></span> <span data-ttu-id="1d589-129">Všechna práva na vaše data zůstávají ve vašem vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="1d589-129">You retain all rights to your own data.</span></span>

<span data-ttu-id="1d589-130">**Použití dat**: Společnost Microsoft vylepšuje své schopnosti prevence a detekce pomocí schémat a analýzy hrozeb napříč několika klienty. Činíme tak v souladu se závazky k ochraně osobních údajů popsanými v našem [Prohlášení o zásadách ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d589-130">**Data use**: Microsoft uses patterns and threat intelligence seen across multiple tenants to enhance our prevention and detection capabilities; we do so in accordance with the privacy commitments described in our [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="1d589-131">Umístění dat je konfigurováno na úrovni pracovního prostoru OMS během jeho vytváření, což je součástí procesu prvotní konfigurace řešení Zabezpečení a audit pro OMS.</span><span class="sxs-lookup"><span data-stu-id="1d589-131">Data location is configured at the OMS workspace level, during the workspace creation, which is part of the initial OMS Security and Audit configuration process.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="1d589-132">Viz také</span><span class="sxs-lookup"><span data-stu-id="1d589-132">See also</span></span>
<span data-ttu-id="1d589-133">V tomto dokumentu jste se dozvěděli, jakým způsobem probíhá správa a ochrana dat v OMS.</span><span class="sxs-lookup"><span data-stu-id="1d589-133">In this document, you learned how data is managed and safeguarded in OMS.</span></span> <span data-ttu-id="1d589-134">Další informace o řešení Zabezpečení a audit v OMS najdete v tématech:</span><span class="sxs-lookup"><span data-stu-id="1d589-134">To learn more about OMS Security and Audit solution, see:</span></span>

* [<span data-ttu-id="1d589-135">Přehled Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="1d589-135">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="1d589-136">Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="1d589-136">Monitoring and Responding to Security Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="1d589-137">Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="1d589-137">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

