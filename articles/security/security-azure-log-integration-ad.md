---
title: aaaAzure protokolu integrace s protokoly auditu Azure Active Directory | Microsoft Docs
description: "Zjistěte, jak tooinstall hello Azure protokolu integrační služby a integraci protokoly z protokolů auditu Azure"
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
ms.openlocfilehash: 3ee8fa3b8b5e9bd33202e57ed5327cd8d3127f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a><span data-ttu-id="69ce5-103">Integrovat protokolů auditu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="69ce5-103">Integrate Azure Active Directory audit logs</span></span>

<span data-ttu-id="69ce5-104">Události auditování Azure Active Directory (Azure AD) pomáhá identifikovat privilegovaných akcí, které došlo k chybě v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="69ce5-104">Azure Active Directory (Azure AD) audit events help you identify privileged actions that occurred in Azure Active Directory.</span></span> <span data-ttu-id="69ce5-105">Zobrazí se hello typy událostí, které můžete sledovat kontrolou [události sestavy auditování Azure Active Directory](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span><span class="sxs-lookup"><span data-stu-id="69ce5-105">You can see hello types of events that you can track by reviewing [Azure Active Directory audit report events](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span></span>

> [!NOTE]
> <span data-ttu-id="69ce5-106">Před provedením hello kroky v tomto článku, je nutné si hello [Začínáme](security-azure-log-integration-get-started.md) článek a proveďte kroky hello existuje.</span><span class="sxs-lookup"><span data-stu-id="69ce5-106">Before you attempt hello steps in this article, you must review hello [Get started](security-azure-log-integration-get-started.md) article and complete hello steps there.</span></span>

## <a name="steps-toointegrate-azure-active-directory-audit-logs"></a><span data-ttu-id="69ce5-107">Protokoly auditu Azure Active directory toointegrate kroky</span><span class="sxs-lookup"><span data-stu-id="69ce5-107">Steps toointegrate Azure Active directory audit logs</span></span>

1. <span data-ttu-id="69ce5-108">Otevřete příkazový řádek se hello a spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="69ce5-108">Open hello command prompt and run this command:</span></span>

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. <span data-ttu-id="69ce5-109">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="69ce5-109">Run this command:</span></span> 
 
   ``azlog createazureid``

   <span data-ttu-id="69ce5-110">Tento příkaz zobrazí výzvu k přihlášení Azure.</span><span class="sxs-lookup"><span data-stu-id="69ce5-110">This command prompts you for your Azure login.</span></span> <span data-ttu-id="69ce5-111">Hello příkaz potom vytvoří Azure Active Directory instančního objektu v Azure AD hello klientům hello předplatná Azure, ve které hello přihlášený uživatel je správce, spolusprávce nebo vlastníka tohoto hostitele.</span><span class="sxs-lookup"><span data-stu-id="69ce5-111">hello command then creates an Azure Active Directory service principal in hello Azure AD tenants that host hello Azure subscriptions in which hello logged-in user is an administrator, a co-administrator, or an owner.</span></span> <span data-ttu-id="69ce5-112">příkaz Hello se nezdaří, pokud je pouze uživatel guest v klientovi Azure AD hello hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="69ce5-112">hello command will fail if hello logged-in user is only a guest user in hello Azure AD tenant.</span></span> <span data-ttu-id="69ce5-113">TooAzure ověřování se provádí prostřednictvím služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="69ce5-113">Authentication tooAzure is done through Azure AD.</span></span> <span data-ttu-id="69ce5-114">Vytvoření objektu služby pro integraci protokolu Azure vytvoří hello identit Azure AD, který je přiřazen přístup tooread z předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="69ce5-114">Creating a service principal for Azure Log Integration creates hello Azure AD identity that is given access tooread from Azure subscriptions.</span></span>

3. <span data-ttu-id="69ce5-115">Spusťte následující příkaz tooprovide hello vaše ID klienta.</span><span class="sxs-lookup"><span data-stu-id="69ce5-115">Run hello following command tooprovide your tenant ID.</span></span> <span data-ttu-id="69ce5-116">Je nutné toobe členem hello klienta správce role toorun hello příkaz.</span><span class="sxs-lookup"><span data-stu-id="69ce5-116">You need toobe member of hello tenant admin role toorun hello command.</span></span>

   ``Azlog.exe authorizedirectoryreader tenantId``

   <span data-ttu-id="69ce5-117">Příklad:</span><span class="sxs-lookup"><span data-stu-id="69ce5-117">Example:</span></span>

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. <span data-ttu-id="69ce5-118">Zkontrolujte, že hello následující tooconfirm složky, která hello soubory JSON protokolu auditování Azure Active Directory jsou vytvořené v nich:</span><span class="sxs-lookup"><span data-stu-id="69ce5-118">Check hello following folders tooconfirm that hello Azure Active Directory audit log JSON files are created in them:</span></span>

   * <span data-ttu-id="69ce5-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span><span class="sxs-lookup"><span data-stu-id="69ce5-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span></span>
   * <span data-ttu-id="69ce5-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span><span class="sxs-lookup"><span data-stu-id="69ce5-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span></span>

<span data-ttu-id="69ce5-121">Hello toto video ukazuje hello kroky popsané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="69ce5-121">hello following video demonstrates hello steps covered in this article:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> <span data-ttu-id="69ce5-122">Konkrétní pokyny k uvedení hello informace v souborech JSON hello do své informace o zabezpečení a událostí systému pro správu (SIEM) obraťte se na dodavatele systému SIEM.</span><span class="sxs-lookup"><span data-stu-id="69ce5-122">For specific instructions on bringing hello information in hello JSON files into your security information and event management (SIEM) system, contact your SIEM vendor.</span></span>

<span data-ttu-id="69ce5-123">Komunita pomoc je k dispozici prostřednictvím hello [fórum MSDN integrace protokolu Azure](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span><span class="sxs-lookup"><span data-stu-id="69ce5-123">Community assistance is available through hello [Azure Log Integration MSDN Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span></span> <span data-ttu-id="69ce5-124">Toto fórum umožňuje lidem v toosupport komunity protokolu integrace se službou Azure hello navzájem otázky, odpovědi, tipy a triky.</span><span class="sxs-lookup"><span data-stu-id="69ce5-124">This forum enables people in hello Azure Log Integration community toosupport each other with questions, answers, tips, and tricks.</span></span> <span data-ttu-id="69ce5-125">Kromě toho týmu Integrace protokolu Azure hello monitoruje Toto fórum a pomáhá vždy, když je to možné.</span><span class="sxs-lookup"><span data-stu-id="69ce5-125">In addition, hello Azure Log Integration team monitors this forum and helps whenever it can.</span></span>

<span data-ttu-id="69ce5-126">Můžete také otevřít [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md).</span><span class="sxs-lookup"><span data-stu-id="69ce5-126">You can also open a [support request](../azure-supportability/how-to-create-azure-support-request.md).</span></span> <span data-ttu-id="69ce5-127">Vyberte **integrace protokolu** jako hello služby, pro kterou jsou žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="69ce5-127">Select **Log Integration** as hello service for which you are requesting support.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69ce5-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69ce5-128">Next steps</span></span>
<span data-ttu-id="69ce5-129">toolearn Další informace o integraci Azure protokolu, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="69ce5-129">toolearn more about Azure Log Integration, see:</span></span>

* <span data-ttu-id="69ce5-130">[Microsoft Azure protokolu integrace pro Azure protokoly](https://www.microsoft.com/download/details.aspx?id=53324): stránka tento stažení softwaru poskytuje podrobnosti, požadavky na systém a pokyny k integraci Azure protokolu.</span><span class="sxs-lookup"><span data-stu-id="69ce5-130">[Microsoft Azure Log Integration for Azure logs](https://www.microsoft.com/download/details.aspx?id=53324): This Download Center page gives details, system requirements, and installation instructions for Azure Log Integration.</span></span>
* <span data-ttu-id="69ce5-131">[Úvod tooAzure integrace protokolu](security-azure-log-integration-overview.md): Tento článek představuje tooAzure integrace protokolu, jejích klíčových funkcích a jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="69ce5-131">[Introduction tooAzure Log Integration](security-azure-log-integration-overview.md): This article introduces you tooAzure Log Integration, its key capabilities, and how it works.</span></span>
* <span data-ttu-id="69ce5-132">[Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Tento příspěvek blogu ukazuje, jak tooconfigure integrace se službou Azure protokolu toowork s partnerských řešení Splunk, HP ArcSight a IBM QRadar.</span><span class="sxs-lookup"><span data-stu-id="69ce5-132">[Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): This blog post shows you how tooconfigure Azure Log Integration toowork with partner solutions Splunk, HP ArcSight, and IBM QRadar.</span></span>
* <span data-ttu-id="69ce5-133">[Nejčastější dotazy k integraci Azure protokolu](security-azure-log-integration-faq.md): Tento článek obsahuje odpovědi na otázky týkající se integrace se službou Azure protokolu.</span><span class="sxs-lookup"><span data-stu-id="69ce5-133">[Azure Log Integration FAQ](security-azure-log-integration-faq.md): This article answers questions about Azure Log Integration.</span></span>
* <span data-ttu-id="69ce5-134">[Výstrahy Security Center integrování integrace se službou Azure protokolu](../security-center/security-center-integrating-alerts-with-log-integration.md): Tento článek ukazuje, jak toosync Security Center výstrahy, společně s shromážděných pomocí diagnostiky Azure a protokoly auditu Azure, s vaší analýzy protokolů událostí zabezpečení virtuálního počítače nebo Řešení SIEM.</span><span class="sxs-lookup"><span data-stu-id="69ce5-134">[Integrating Security Center alerts with Azure Log Integration](../security-center/security-center-integrating-alerts-with-log-integration.md): This article shows you how toosync Security Center alerts, along with virtual machine security events collected by Azure Diagnostics and Azure audit logs, with your log analytics or SIEM solution.</span></span>
* <span data-ttu-id="69ce5-135">[Protokoly auditu nové funkce pro diagnostiky Azure a Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Tento příspěvek blogu vás seznámí tooAzure protokoly auditu a další funkce, které vám pomohou analyzovat hello operations vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="69ce5-135">[New features for Azure Diagnostics and Azure audit logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): This blog post introduces you tooAzure audit logs and other features that help you gain insights into hello operations of your Azure resources.</span></span>
