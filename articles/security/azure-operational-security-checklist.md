---
title: "Kontrolní seznam zabezpečení Azure provozní | Microsoft Docs"
description: "Tento článek obsahuje sadu kontrolní seznam pro zabezpečení provozu Azure."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: 1877e6ab19d504c8be6130578f17b608f123e20a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-operational-security-checklist"></a><span data-ttu-id="9f5a0-103">Kontrolní seznam zabezpečení Azure provozní</span><span class="sxs-lookup"><span data-stu-id="9f5a0-103">Azure operational security checklist</span></span>
<span data-ttu-id="9f5a0-104">Nasazení aplikace v Azure je rychlý, snadný a nákladově efektivní.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-104">Deploying an application on Azure  is fast, easy, and cost-effective.</span></span> <span data-ttu-id="9f5a0-105">Před nasazením cloudových aplikací v provozním prostředí, které jsou užitečné používat kontrolní seznam pro si udělali představu o vaší aplikace podle seznamu provozního zabezpečení důležité a doporučené akce je třeba zvážit.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-105">Before deploying cloud application in production useful to have a checklist to assist in evaluating your application against a list of essential and recommended operational security actions for you to consider.</span></span>

## <a name="introduction"></a><span data-ttu-id="9f5a0-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="9f5a0-106">Introduction</span></span>

<span data-ttu-id="9f5a0-107">Azure poskytuje sada služby infrastruktury, které můžete použít k nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-107">Azure provides a suite of infrastructure services that you can use to deploy your applications.</span></span> <span data-ttu-id="9f5a0-108">Zabezpečení provozu Azure se odkazuje na služby, ovládací prvky a funkce, které jsou k dispozici uživatelům pro ochranu svá data, aplikace a dalších prostředků ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-108">Azure Operational Security refers to the services, controls, and features available to users for protecting their data, applications, and other assets in Microsoft Azure.</span></span>

-   <span data-ttu-id="9f5a0-109">Chcete-li získat maximální výhody mimo Cloudová platforma, doporučujeme využívají služby Azure a postupujte podle kontrolního seznamu.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-109">To get the maximum benefit out of the cloud platform, we recommend that you leverage Azure services and follow the checklist.</span></span>
-   <span data-ttu-id="9f5a0-110">Organizace, které investovat čas a prostředky hodnocením provozní připravenosti na svých aplikací před spuštěním mít mnohem vyšší míra spokojenost než ty, kteří nejsou.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-110">Organizations that invest time and resources assessing the operational readiness of their applications before launch have a much higher rate of satisfaction than those who don’t.</span></span> <span data-ttu-id="9f5a0-111">Při provádění této práce, může být kontrolní seznamy mechanismus neocenitelnou pomocí zajistit, že aplikace se vyhodnocují konzistentně a komplexní.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-111">When performing this work, checklists can be an invaluable mechanism to ensure that applications are evaluated consistently and holistically.</span></span>
-   <span data-ttu-id="9f5a0-112">Úroveň provozní assessment bude se liší v závislosti na úrovni vyspělosti cloudu organizace a fáze vývoje aplikace, dostupnosti potřebám a požadavkům citlivosti dat.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-112">The level of operational assessment will varies depending on the organization’s cloud maturity level and the application’s development phase, availability needs, and data sensitivity requirements.</span></span>

## <a name="checklist"></a><span data-ttu-id="9f5a0-113">Kontrolní seznam</span><span class="sxs-lookup"><span data-stu-id="9f5a0-113">Checklist</span></span>

<span data-ttu-id="9f5a0-114">Tento kontrolní seznam slouží jako pomoc podniky pečlivě promyslete různé aspekty provozní zabezpečení jako jejich nasazení sofistikované podnikové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-114">This checklist is intended to help enterprises think through various operational security considerations as they deploy sophisticated enterprise applications on Azure.</span></span> <span data-ttu-id="9f5a0-115">Ho lze také pomoc při vytváření strategie zabezpečení cloudu migrace a operace ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-115">It can also be used to help you build a secure cloud migration and operation strategy for your organization.</span></span>

|<span data-ttu-id="9f5a0-116">Kontrolní seznam kategorie</span><span class="sxs-lookup"><span data-stu-id="9f5a0-116">Checklist Category</span></span>| <span data-ttu-id="9f5a0-117">Popis</span><span class="sxs-lookup"><span data-stu-id="9f5a0-117">Description</span></span>|
| ------------ | -------- |
| [<span data-ttu-id="9f5a0-118"><br>Role zabezpečení a řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="9f5a0-118"><br>Security Roles & Access Controls</span></span>](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)|<ul><li><span data-ttu-id="9f5a0-119">Použití [řízení přístupu (RBAC) na základě Role](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) zajistit uživatelská, kterého chcete přiřadit oprávnění pro uživatele, skupiny a aplikace v určité oboru.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-119">Use [Role based access control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) to provide user-specific that used to assign permissions to users, groups, and applications at a certain scope.</span></span></li></ul> |
| [<span data-ttu-id="9f5a0-120"><br>Shromažďování dat a úložiště</span><span class="sxs-lookup"><span data-stu-id="9f5a0-120"><br>Data Collection & Storage</span></span>](https://docs.microsoft.com/azure/storage/storage-security-guide)|<ul><li><span data-ttu-id="9f5a0-121">Použít zabezpečení roviny Management k zabezpečení vašeho účtu úložiště pomocí [řízení přístupu na základě Role (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).</span><span class="sxs-lookup"><span data-stu-id="9f5a0-121">Use Management Plane Security to secure your Storage Account using [Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).</span></span></li><li><span data-ttu-id="9f5a0-122">Zabezpečení dat roviny zabezpečení přístupu k dat pomocí [podpisy sdíleného přístupu (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) a uložené zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-122">Data Plane Security to Securing Access to your Data using [Shared Access Signatures (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) and Stored Access Policies.</span></span></li><li><span data-ttu-id="9f5a0-123">Používat šifrování transportní vrstvy – pomocí protokolu HTTPS a používá šifrování [protokolu SMB (Server zprávu bloku protokoly) 3.0](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) pro [sdílené složky Azure](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).</span><span class="sxs-lookup"><span data-stu-id="9f5a0-123">Use Transport-Level Encryption – Using HTTPS and the encryption used by [SMB (Server message block protocols) 3.0](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) for [Azure File Shares](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).</span></span></li><li><span data-ttu-id="9f5a0-124">Použití [šifrování na straně klienta](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) k zabezpečení dat, která odešlete na účty úložiště, pokud požadujete výhradní kontrolu nad šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-124">Use [Client-side encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) to secure data that you send to storage accounts when you require sole control of encryption keys.</span></span> </li><li><span data-ttu-id="9f5a0-125">Použití [šifrování služby úložiště (SSE)](https://docs.microsoft.com/azure/storage/storage-service-encryption) automaticky šifrovat data ve službě Azure Storage a [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) k šifrování soubory disků virtuálních počítačů pro disky operačního systému a data.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-125">Use [Storage Service Encryption (SSE)](https://docs.microsoft.com/azure/storage/storage-service-encryption)  to automatically encrypt data in Azure Storage, and [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) to encrypt virtual machine disk files for the OS and data disks.</span></span></li><li><span data-ttu-id="9f5a0-126">Použití Azure [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics) monitorování autorizace zadejte; jako pomocí úložiště objektů Blob, můžete zobrazit, pokud mají uživatelé použít sdíleného přístupového podpisu nebo klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-126">Use Azure [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics) to monitor authorization type; like with Blob Storage, you can see if users have used a Shared Access Signature or the storage account keys.</span></span></li><li><span data-ttu-id="9f5a0-127">Použití [sdílení prostředků různých původů (CORS)](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) přístup k prostředkům úložiště z jiných domén.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-127">Use [Cross-Origin Resource Sharing (CORS)](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) to access storage resources from different domains.</span></span></li></ul> |
|[<span data-ttu-id="9f5a0-128"><br>Zásady zabezpečení a doporučení</span><span class="sxs-lookup"><span data-stu-id="9f5a0-128"><br>Security Policies & Recommendations</span></span>](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)|<ul><li><span data-ttu-id="9f5a0-129">Použití [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection) k nasazení řešení pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-129">Use [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection) to deploy endpoint solutions.</span></span></li><li><span data-ttu-id="9f5a0-130">Přidat [brány firewall webových aplikací (firewall webových aplikací)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) k zabezpečení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-130">Add a [web application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) to secure web applications.</span></span></li><li>  <span data-ttu-id="9f5a0-131">Použití [brána firewall příští generace (NGFW)](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) od partnera Microsoftu ke zvýšení ochrany vaší zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-131">Use a [next generation firewall (NGFW)](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) from a Microsoft partner to increase your security protections.</span></span> </li><li><span data-ttu-id="9f5a0-132">Použít zabezpečení kontaktní údaje pro vaše předplatné Azure; Toto [zabezpečení odpovědi Centre (střediska MSRC)](https://technet.microsoft.com/security/dn528958.aspx) kontaktuje můžete, pokud zjistí, že zákaznických údajů měl strana nezákonné nebo neoprávněný přístup k.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-132">Apply security contact details for your Azure subscription; this the [Microsoft Security Response Centre (MSRC)](https://technet.microsoft.com/security/dn528958.aspx) contacts you if it discovers that your customer data has been accessed by an unlawful or unauthorized party.</span></span></li></ul> |
| [<span data-ttu-id="9f5a0-133"><br>Správa identit a přístupu</span><span class="sxs-lookup"><span data-stu-id="9f5a0-133"><br>Identity & Access Management</span></span>](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices)|<ul><li><span data-ttu-id="9f5a0-134">[Synchronizace místního adresáře s adresářem cloudu pomocí služby Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).</span><span class="sxs-lookup"><span data-stu-id="9f5a0-134">[Synchronize your on-premises directory with your cloud directory using Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).</span></span></li><li><span data-ttu-id="9f5a0-135">Použití [jednotné přihlašování](https://azure.microsoft.com/resources/videos/overview-of-single-sign-on/) povolit uživatelům přístup k jejich SaaS aplikací založených na účet své organizace v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-135">Use [Single Sign-On](https://azure.microsoft.com/resources/videos/overview-of-single-sign-on/) to enable users to access their SaaS applications based on their organizational account in Azure AD.</span></span></li><li><span data-ttu-id="9f5a0-136">Použití [aktivita registrace pro resetování hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights) sestavy ke sledování uživatelů, kteří jsou registrace.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-136">Use the [Password Reset Registration Activity](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights) report to monitor the users that are registering.</span></span></li><li><span data-ttu-id="9f5a0-137">Povolit [vícefaktorové ověřování (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-137">Enable [multi-factor authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) for users.</span></span></li><li><span data-ttu-id="9f5a0-138">Vývojářům používat funkce zabezpečení identit pro aplikace, jako [Microsoft životního cyklu SDL (Security Development)](https://www.microsoft.com/download/details.aspx?id=12379).</span><span class="sxs-lookup"><span data-stu-id="9f5a0-138">Developers to use secure identity capabilities for apps like [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/download/details.aspx?id=12379).</span></span></li><li><span data-ttu-id="9f5a0-139">Pro podezřelé aktivity aktivně monitorování pomocí sestav Azure AD Premium anomálií a [funkcích ochrany identit Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection).</span><span class="sxs-lookup"><span data-stu-id="9f5a0-139">Actively monitor for suspicious activities by using Azure AD Premium anomaly reports and [Azure AD identity protection capability](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection).</span></span></li></ul> |
|[<span data-ttu-id="9f5a0-140"><br>Průběžné sledování zabezpečení</span><span class="sxs-lookup"><span data-stu-id="9f5a0-140"><br>Ongoing Security Monitoring</span></span>](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)|<ul><li><span data-ttu-id="9f5a0-141">Pomocí řešení pro vyhodnocení malwaru [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) nahlásit stav ochrany proti malwaru ve vaší infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-141">Use Malware Assessment Solution [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) to report on the status of antimalware protection in your infrastructure.</span></span></li><li><span data-ttu-id="9f5a0-142">Použití [aktualizovat assessment](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management) určit celkové riziko potenciální problémy se zabezpečením a jestli nebo jak důležité aktualizace jsou pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-142">Use [Update assessment](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management) to determine the overall exposure to potential security problems, and whether or how critical these updates are for your environment.</span></span></li><li><span data-ttu-id="9f5a0-143">[Identit a přístupu](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) poskytují přehled uživatele</span><span class="sxs-lookup"><span data-stu-id="9f5a0-143">The [Identity and Access](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) provide you an overview of user</span></span> </li><ul><li><span data-ttu-id="9f5a0-144">stav identity uživatele,</span><span class="sxs-lookup"><span data-stu-id="9f5a0-144">user identity state,</span></span></li><li><span data-ttu-id="9f5a0-145">Počet neúspěšných pokusů o přihlášení,</span><span class="sxs-lookup"><span data-stu-id="9f5a0-145">number of failed attempts to log on,</span></span></li><li>    <span data-ttu-id="9f5a0-146">účet uživatele, které byly použity během těmito pokusy účty, které byly uzamčen</span><span class="sxs-lookup"><span data-stu-id="9f5a0-146">the user’s account that were used during those attempts, accounts that were locked out</span></span></li> <li><span data-ttu-id="9f5a0-147">účty se změněným nebo resetování hesla</span><span class="sxs-lookup"><span data-stu-id="9f5a0-147">accounts with changed or reset password</span></span> </li><li><span data-ttu-id="9f5a0-148">Aktuálně počet účtů, které se protokolují v.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-148">Currently number of accounts that are logged in.</span></span></li></ul></ul> |
| [<span data-ttu-id="9f5a0-149"><br>Funkce zjišťování služby Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="9f5a0-149"><br>Azure Security Center detection capabilities</span></span>](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities)|<ul><li><span data-ttu-id="9f5a0-150">Použití [možností detekce](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities), k identifikaci active hrozeb cílení na vaše prostředky Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-150">Use [detection capabilities](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities), to identify active threats targeting your Microsoft Azure resources.</span></span></li><li><span data-ttu-id="9f5a0-151">Použití [integrované analýzou hrozeb](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/19/get-threat-intelligence-reports-with-azure-security-center/) vyhledává známé nesprávnými účastníky s využitím globální analýzou hrozeb z produktů společnosti Microsoft a služeb [Microsoft digitální činů jednotky (DCU)](https://www.microsoft.com/stories/cyber.aspx), [Microsoft Security Response Center (MSRC)](https://docs.microsoft.com/azure/security/azure-security-response-center)a externích informačních kanálů.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-151">Use [integrated threat intelligence](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/19/get-threat-intelligence-reports-with-azure-security-center/) that looks for known bad actors by leveraging global threat intelligence from Microsoft products and services, the [Microsoft Digital Crimes Unit (DCU)](https://www.microsoft.com/stories/cyber.aspx), the [Microsoft Security Response Center (MSRC)](https://docs.microsoft.com/azure/security/azure-security-response-center), and external feeds.</span></span></li><li><span data-ttu-id="9f5a0-152">Použití [analýzy chování](https://blogs.technet.microsoft.com/enterprisemobility/2016/06/30/ata-behavior-analysis-monitoring/) který použije známé vzorce ke zjištění škodlivé chování.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-152">Use [Behavioral analytics](https://blogs.technet.microsoft.com/enterprisemobility/2016/06/30/ata-behavior-analysis-monitoring/) that applies known patterns to discover malicious behavior.</span></span> </li><li><span data-ttu-id="9f5a0-153">Použití [detekce anomálií](https://msdn.microsoft.com/library/azure/dn913096.aspx) používající statistické profilace k sestavení historických směrného plánu.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-153">Use [Anomaly detection](https://msdn.microsoft.com/library/azure/dn913096.aspx) that uses statistical profiling to build a historical baseline.</span></span></li></ul> |
| [<span data-ttu-id="9f5a0-154"><br>Operace Developer (DevOps)</span><span class="sxs-lookup"><span data-stu-id="9f5a0-154"><br>Developer Operations (DevOps)</span></span>](https://docs.microsoft.com/azure/architecture/checklist/dev-ops)|<ul><li><span data-ttu-id="9f5a0-155">[Infrastruktura jako kód (IaC)](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) postupem, který umožňuje automatizace a ověření vytvoření a rušením sítí a virtuálních počítačů pomáhají s doručováním hostování platformy zabezpečené, stabilní aplikace je.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-155">[Infrastructure as Code (IaC)](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) is a practice, which enables the automation and validation of creation and teardown of networks and virtual machines to help with delivering secure, stable application hosting platforms.</span></span></li><li><span data-ttu-id="9f5a0-156">[Průběžnou integraci a nasazení](https://www.visualstudio.com/docs/build/overview) Probíhající sloučení a testování kódu, což vede k hledání defekty již v rané fázi.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-156">[Continuous Integration and Deployment](https://www.visualstudio.com/docs/build/overview) drive the ongoing merging and testing of code, which leads to finding defects early.</span></span> </li><li><span data-ttu-id="9f5a0-157">[Správa verzí](https://msdn.microsoft.com/library/vs/alm/release/overview) spravovat automatizované nasazení každé fáze vašeho kanálu.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-157">[Release Management](https://msdn.microsoft.com/library/vs/alm/release/overview) Manage automated deployments through each stage of your pipeline.</span></span></li><li><span data-ttu-id="9f5a0-158">[Monitorování výkonu aplikace](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) spuštěných aplikací, včetně produkčního prostředí pro stavu aplikace jako i jako zákazník využití pomoc organizacím formuláře předpoklad a rychle ověření nebo disprove strategie.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-158">[App Performance Monitoring](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) of running applications including production environments for application health as well as customer usage help organizations form a hypothesis and quickly validate or disprove strategies.</span></span></li><li><span data-ttu-id="9f5a0-159">Pomocí [zátěžové testování & automatickému škálování](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) problémy s výkonem jsme můžete najít v naší aplikaci ke zlepšení kvality nasazení a je třeba zajistit vaší aplikace je vždy nahoru a k dispozici pro firmy.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-159">Using [Load Testing & Auto-Scale](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) we can find performance problems in our app to improve deployment quality and to make sure our app is always up or available to cater to the business needs.</span></span></li></ul> |


## <a name="conclusion"></a><span data-ttu-id="9f5a0-160">Závěr</span><span class="sxs-lookup"><span data-stu-id="9f5a0-160">Conclusion</span></span>
<span data-ttu-id="9f5a0-161">Mnoho organizací mít úspěšně nasadit a provozovat jejich cloudových aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-161">Many organizations have successfully deployed and operated their cloud applications on Azure.</span></span> <span data-ttu-id="9f5a0-162">Kontrolní seznamy, poskytuje několik kontrolní seznamy, které je nezbytné, zvýrazněte a umožňují zvýšit pravděpodobnost úspěšné nasazení a před bez operací.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-162">The checklists provided highlight several checklists that is essential and help you to increase the likelihood of successful deployments and frustration-free operations.</span></span> <span data-ttu-id="9f5a0-163">Důrazně doporučujeme těchto provozních a strategických aspektů stávající a nové nasazení aplikace na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-163">We highly recommend these operational and strategic considerations for your existing and new application deployments on Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f5a0-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f5a0-164">Next steps</span></span>
<span data-ttu-id="9f5a0-165">V tomto dokumentu jste se seznámili s řešením Zabezpečení a audit v OMS.</span><span class="sxs-lookup"><span data-stu-id="9f5a0-165">In this document, you were introduced to OMS Security and Audit solution.</span></span> <span data-ttu-id="9f5a0-166">Další informace o zabezpečení v OMS najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="9f5a0-166">To learn more about OMS Security, see the following articles:</span></span>

- <span data-ttu-id="9f5a0-167">[Přehled Operations Management Suite (OMS)](https://docs.microsoft.com/en-us/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="9f5a0-167">[Operations Management Suite (OMS) overview](https://docs.microsoft.com/en-us/azure/operations-management-suite/operations-management-suite-overview).</span></span>
- <span data-ttu-id="9f5a0-168">[Návrh a provozní zabezpečení](https://www.microsoft.com/trustcenter/security/designopsecurity).</span><span class="sxs-lookup"><span data-stu-id="9f5a0-168">[Design and operational security](https://www.microsoft.com/trustcenter/security/designopsecurity).</span></span>
- <span data-ttu-id="9f5a0-169">[Azure Security Center plánování a operace](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide).</span><span class="sxs-lookup"><span data-stu-id="9f5a0-169">[Azure Security Center planning and operations](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide).</span></span>