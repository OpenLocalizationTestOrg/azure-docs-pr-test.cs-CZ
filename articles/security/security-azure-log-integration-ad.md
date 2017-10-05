---
title: "Protokol integrace se službou Azure s protokoly auditu Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak nainstalovat službu protokolu integrace se službou Azure a integrovat protokoly z protokolů auditu Azure"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/08/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 8a1295cc86057ed72940e774d0bd423d61142e31
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a><span data-ttu-id="ff3da-103">Integrovat protokolů auditu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff3da-103">Integrate Azure Active Directory audit logs</span></span>

<span data-ttu-id="ff3da-104">Události auditování Azure Active Directory (Azure AD) pomáhá identifikovat privilegovaných akcí, které došlo k chybě v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ff3da-104">Azure Active Directory (Azure AD) audit events help you identify privileged actions that occurred in Azure Active Directory.</span></span> <span data-ttu-id="ff3da-105">Zobrazí typy událostí, které můžete sledovat kontrolou [události sestavy auditování Azure Active Directory](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span><span class="sxs-lookup"><span data-stu-id="ff3da-105">You can see the types of events that you can track by reviewing [Azure Active Directory audit report events](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span></span>

> [!NOTE]
> <span data-ttu-id="ff3da-106">Před provedením kroků v tomto článku, je nutné si [Začínáme](security-azure-log-integration-get-started.md) článek a dokončete existuje.</span><span class="sxs-lookup"><span data-stu-id="ff3da-106">Before you attempt the steps in this article, you must review the [Get started](security-azure-log-integration-get-started.md) article and complete the steps there.</span></span>

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a><span data-ttu-id="ff3da-107">Protokoly auditu kroky pro integraci služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="ff3da-107">Steps to integrate Azure Active directory audit logs</span></span>

1. <span data-ttu-id="ff3da-108">Otevřete příkazový řádek a spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="ff3da-108">Open the command prompt and run this command:</span></span>

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. <span data-ttu-id="ff3da-109">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="ff3da-109">Run this command:</span></span> 
 
   ``azlog createazureid``

   <span data-ttu-id="ff3da-110">Tento příkaz zobrazí výzvu k přihlášení Azure.</span><span class="sxs-lookup"><span data-stu-id="ff3da-110">This command prompts you for your Azure login.</span></span> <span data-ttu-id="ff3da-111">Příkaz vytvoří Azure Active Directory instančního objektu v klienty Azure AD, které jsou hostiteli předplatná Azure, ve kterých přihlášený uživatel je správce, spolusprávce nebo vlastníka.</span><span class="sxs-lookup"><span data-stu-id="ff3da-111">The command then creates an Azure Active Directory service principal in the Azure AD tenants that host the Azure subscriptions in which the logged-in user is an administrator, a co-administrator, or an owner.</span></span> <span data-ttu-id="ff3da-112">Příkaz se nezdaří, pokud je uživatel přihlášený pouze uživatel guest v klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff3da-112">The command will fail if the logged-in user is only a guest user in the Azure AD tenant.</span></span> <span data-ttu-id="ff3da-113">Ověřování do Azure se provádí prostřednictvím služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff3da-113">Authentication to Azure is done through Azure AD.</span></span> <span data-ttu-id="ff3da-114">Vytvoření objektu služby pro integraci protokolu Azure vytvoří Azure AD identity, který je přiřazen přístup ke čtení z předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="ff3da-114">Creating a service principal for Azure Log Integration creates the Azure AD identity that is given access to read from Azure subscriptions.</span></span>

3. <span data-ttu-id="ff3da-115">Spusťte následující příkaz k poskytování vaše ID klienta.</span><span class="sxs-lookup"><span data-stu-id="ff3da-115">Run the following command to provide your tenant ID.</span></span> <span data-ttu-id="ff3da-116">Musíte být členem role Správce klientů ke spuštění příkazu.</span><span class="sxs-lookup"><span data-stu-id="ff3da-116">You need to be member of the tenant admin role to run the command.</span></span>

   ``Azlog.exe authorizedirectoryreader tenantId``

   <span data-ttu-id="ff3da-117">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ff3da-117">Example:</span></span>

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. <span data-ttu-id="ff3da-118">Zkontrolujte následující složky pro potvrzení, že jsou v nich vytvořeny soubory JSON protokolů auditu Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="ff3da-118">Check the following folders to confirm that the Azure Active Directory audit log JSON files are created in them:</span></span>

   * <span data-ttu-id="ff3da-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span><span class="sxs-lookup"><span data-stu-id="ff3da-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span></span>
   * <span data-ttu-id="ff3da-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span><span class="sxs-lookup"><span data-stu-id="ff3da-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span></span>

<span data-ttu-id="ff3da-121">Následující video ukazuje kroky popsané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="ff3da-121">The following video demonstrates the steps covered in this article:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> <span data-ttu-id="ff3da-122">Konkrétní pokyny k uvedení informací v souborech JSON do své informace o zabezpečení a událostí systému pro správu (SIEM) obraťte se na dodavatele systému SIEM.</span><span class="sxs-lookup"><span data-stu-id="ff3da-122">For specific instructions on bringing the information in the JSON files into your security information and event management (SIEM) system, contact your SIEM vendor.</span></span>

<span data-ttu-id="ff3da-123">Komunita pomoc je k dispozici prostřednictvím [fórum MSDN integrace protokolu Azure](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span><span class="sxs-lookup"><span data-stu-id="ff3da-123">Community assistance is available through the [Azure Log Integration MSDN Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span></span> <span data-ttu-id="ff3da-124">Toto fórum umožňuje členové komunity protokolu integrace se službou Azure pro podporu navzájem otázky, odpovědi, tipy a triky.</span><span class="sxs-lookup"><span data-stu-id="ff3da-124">This forum enables people in the Azure Log Integration community to support each other with questions, answers, tips, and tricks.</span></span> <span data-ttu-id="ff3da-125">Kromě toho týmem protokolu integrace se službou Azure sleduje toto fórum a pomáhá vždy, když je to možné.</span><span class="sxs-lookup"><span data-stu-id="ff3da-125">In addition, the Azure Log Integration team monitors this forum and helps whenever it can.</span></span>

<span data-ttu-id="ff3da-126">Můžete také otevřít [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md).</span><span class="sxs-lookup"><span data-stu-id="ff3da-126">You can also open a [support request](../azure-supportability/how-to-create-azure-support-request.md).</span></span> <span data-ttu-id="ff3da-127">Vyberte **integrace protokolu** jako službu, pro kterou jsou žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="ff3da-127">Select **Log Integration** as the service for which you are requesting support.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff3da-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff3da-128">Next steps</span></span>
<span data-ttu-id="ff3da-129">Další informace o integraci Azure protokolu najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="ff3da-129">To learn more about Azure Log Integration, see:</span></span>

* <span data-ttu-id="ff3da-130">[Microsoft Azure protokolu integrace pro Azure protokoly](https://www.microsoft.com/download/details.aspx?id=53324): stránka tento stažení softwaru poskytuje podrobnosti, požadavky na systém a pokyny k integraci Azure protokolu.</span><span class="sxs-lookup"><span data-stu-id="ff3da-130">[Microsoft Azure Log Integration for Azure logs](https://www.microsoft.com/download/details.aspx?id=53324): This Download Center page gives details, system requirements, and installation instructions for Azure Log Integration.</span></span>
* <span data-ttu-id="ff3da-131">[Úvod do integrace se službou Azure protokolu](security-azure-log-integration-overview.md): Tento článek vás seznámí s protokolu integrace se službou Azure, jejích klíčových funkcích a jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="ff3da-131">[Introduction to Azure Log Integration](security-azure-log-integration-overview.md): This article introduces you to Azure Log Integration, its key capabilities, and how it works.</span></span>
* <span data-ttu-id="ff3da-132">[Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Tento příspěvek blogu ukazuje, jak nakonfigurovat integraci Azure protokolu pro práci s partnerských řešení Splunk, HP ArcSight a IBM QRadar.</span><span class="sxs-lookup"><span data-stu-id="ff3da-132">[Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): This blog post shows you how to configure Azure Log Integration to work with partner solutions Splunk, HP ArcSight, and IBM QRadar.</span></span>
* <span data-ttu-id="ff3da-133">[Nejčastější dotazy k integraci Azure protokolu](security-azure-log-integration-faq.md): Tento článek obsahuje odpovědi na otázky týkající se integrace se službou Azure protokolu.</span><span class="sxs-lookup"><span data-stu-id="ff3da-133">[Azure Log Integration FAQ](security-azure-log-integration-faq.md): This article answers questions about Azure Log Integration.</span></span>
* <span data-ttu-id="ff3da-134">[Výstrahy Security Center integrování integrace se službou Azure protokolu](../security-center/security-center-integrating-alerts-with-log-integration.md): v tomto článku se dozvíte, jak synchronizovat výstrahy Security Center, společně s zabezpečení virtuálního počítače shromažďovány události pomocí diagnostiky Azure a Azure auditu protokoly s vaší analýzy protokolů nebo Řešení SIEM.</span><span class="sxs-lookup"><span data-stu-id="ff3da-134">[Integrating Security Center alerts with Azure Log Integration](../security-center/security-center-integrating-alerts-with-log-integration.md): This article shows you how to sync Security Center alerts, along with virtual machine security events collected by Azure Diagnostics and Azure audit logs, with your log analytics or SIEM solution.</span></span>
* <span data-ttu-id="ff3da-135">[Protokoly auditu nové funkce pro diagnostiky Azure a Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Tento příspěvek blogu vás seznámí s protokoly auditu Azure a další funkce, které vám pomůžou získat přehled o činnosti vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="ff3da-135">[New features for Azure Diagnostics and Azure audit logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): This blog post introduces you to Azure audit logs and other features that help you gain insights into the operations of your Azure resources.</span></span>
