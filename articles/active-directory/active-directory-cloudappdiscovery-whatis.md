---
title: "aaaFinding nespravované cloudových aplikací s Cloud App Discovery. | Microsoft Docs"
description: "Poskytuje informace o hledání a Správa aplikací pomocí Cloud App Discovery, jaké jsou výhody hello a jak to funguje."
services: active-directory
keywords: "cloud app discovery,. Správa aplikací"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 50c24af9bb400e4be11f4ad2d1de13d26f5467bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a><span data-ttu-id="6519e-104">Hledání nespravované cloudových aplikací s Cloud App Discovery</span><span class="sxs-lookup"><span data-stu-id="6519e-104">Finding unmanaged cloud applications with Cloud App Discovery</span></span>
## <a name="overview"></a><span data-ttu-id="6519e-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="6519e-105">Overview</span></span>
<span data-ttu-id="6519e-106">V rámci moderní firmy IT oddělení nemají často informace o všech cloudových aplikací hello používat členové jejich organizace toodo práci.</span><span class="sxs-lookup"><span data-stu-id="6519e-106">In modern enterprises, IT departments are often not aware of all hello cloud applications that members of their organization use toodo their work.</span></span> <span data-ttu-id="6519e-107">Je snadno toosee, proč by správci mají obavy o neoprávněný přístup k datům toocorporate, možné úniku a další bezpečnostní rizika.</span><span class="sxs-lookup"><span data-stu-id="6519e-107">It is easy toosee why administrators would have concerns about unauthorized access toocorporate data, possible data leakage and other security risks.</span></span> <span data-ttu-id="6519e-108">Tento nedostatek povědomí o můžete nastavit, vytváření plánu pro práci s těchto rizik zabezpečení pravděpodobně složitý.</span><span class="sxs-lookup"><span data-stu-id="6519e-108">This lack of awareness can make creating a plan for dealing with these security risks seem daunting.</span></span>

<span data-ttu-id="6519e-109">Cloud App Discovery je funkce Premium Azure Active Directory (AD), jehož pomocí můžete toodiscover cloudových aplikací používá hello lidé ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="6519e-109">Cloud App Discovery is a feature of Azure Active Directory (AD) Premium that enables you toodiscover cloud applications being used by hello people in your organization.</span></span>

<span data-ttu-id="6519e-110">**Cloud App Discovery můžete:**</span><span class="sxs-lookup"><span data-stu-id="6519e-110">**With Cloud App Discovery, you can:**</span></span>

* <span data-ttu-id="6519e-111">Najít cloudové aplikace hello používá a měřit toto využití počet uživatelů, objemu přenosů nebo počet webové požadavky toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6519e-111">Find hello cloud applications being used and measure that usage by number of users, volume of traffic or number of web requests toohello application.</span></span>
* <span data-ttu-id="6519e-112">Identifikujte hello uživatele, kteří používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6519e-112">Identify hello users that are using an application.</span></span>
* <span data-ttu-id="6519e-113">Exportujte data pro offline analýzu.</span><span class="sxs-lookup"><span data-stu-id="6519e-113">Export data for offline analysis.</span></span>
* <span data-ttu-id="6519e-114">Přepněte tyto aplikace pod kontrolou IT a povolení jednotného přihlašování na pro správu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6519e-114">Bring these applications under IT control and enable single sign on for user management.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="6519e-115">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="6519e-115">How it works</span></span>
1. <span data-ttu-id="6519e-116">Agenti využití aplikace jsou nainstalovány na počítače uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6519e-116">Application usage agents are installed on user's computers.</span></span>
2. <span data-ttu-id="6519e-117">aplikace informace o využití Hello zachycenou hello agentů je odesílán prostřednictvím zabezpečeného, šifrovaný kanál toohello cloudové aplikace zjišťování služby.</span><span class="sxs-lookup"><span data-stu-id="6519e-117">hello application usage information captured by hello agents is sent over a secure, encrypted channel toohello cloud app discovery service.</span></span>
3. <span data-ttu-id="6519e-118">Hello služby Cloud App Discovery vyhodnotí hello dat a generuje sestavy.</span><span class="sxs-lookup"><span data-stu-id="6519e-118">hello Cloud App Discovery service evaluates hello data and generates reports.</span></span>

![Diagram cloud App Discovery](./media/active-directory-cloudappdiscovery/cad01.png)

<span data-ttu-id="6519e-120">tooget pracovat s Cloud App Discovery, najdete v části [získávání spuštěna s Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span><span class="sxs-lookup"><span data-stu-id="6519e-120">tooget started with Cloud App Discovery, see [Getting Started With Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span></span>

## <a name="related-articles"></a><span data-ttu-id="6519e-121">Související články</span><span class="sxs-lookup"><span data-stu-id="6519e-121">Related articles</span></span>
* [<span data-ttu-id="6519e-122">Cloud App Discovery zabezpečení a důležité informace o ochraně osobních údajů</span><span class="sxs-lookup"><span data-stu-id="6519e-122">Cloud App Discovery Security and Privacy Considerations</span></span>](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [<span data-ttu-id="6519e-123">Příručka pro nasazení zásad skupiny zjišťování cloudové aplikace</span><span class="sxs-lookup"><span data-stu-id="6519e-123">Cloud App Discovery Group Policy Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [<span data-ttu-id="6519e-124">Průvodce nasazením cloud App Discovery System Center</span><span class="sxs-lookup"><span data-stu-id="6519e-124">Cloud App Discovery System Center Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [<span data-ttu-id="6519e-125">Nastavení registru Cloud App Discovery na Proxy servery s vlastní porty</span><span class="sxs-lookup"><span data-stu-id="6519e-125">Cloud App Discovery Registry Settings for Proxy Servers with Custom Ports</span></span>](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [<span data-ttu-id="6519e-126">Protokol změn agenta cloud App Discovery.</span><span class="sxs-lookup"><span data-stu-id="6519e-126">Cloud App Discovery Agent Changelog </span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [<span data-ttu-id="6519e-127">Nejčastější dotazy k cloud App Discovery.</span><span class="sxs-lookup"><span data-stu-id="6519e-127">Cloud App Discovery Frequently Asked Questions</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [<span data-ttu-id="6519e-128">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6519e-128">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

