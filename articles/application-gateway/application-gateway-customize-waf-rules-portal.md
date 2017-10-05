---
title: "Přizpůsobení pravidel brány firewall webových aplikací v Azure Application Gateway - portálu Azure | Microsoft Docs"
description: "Tento článek obsahuje informace o tom, jak přizpůsobit webovou pravidla aplikace brány firewall v Application Gateway pomocí portálu Azure."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: gwallace
ms.openlocfilehash: cdcbadbc3765dfc583c26e1b1453863d421c9a72
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a><span data-ttu-id="7b3bd-103">Přizpůsobení pravidel brány firewall webových aplikací prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7b3bd-103">Customize web application firewall rules through the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7b3bd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7b3bd-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="7b3bd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7b3bd-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="7b3bd-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7b3bd-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="7b3bd-107">Azure Application Gateway brány firewall webových aplikací (firewall webových aplikací) poskytuje ochranu pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-107">The Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="7b3bd-108">Tyto ochrany jsou poskytovány pomocí aplikace otevřete webový projekt zabezpečení (OWASP) základní pravidlo nastavit CRS ().</span><span class="sxs-lookup"><span data-stu-id="7b3bd-108">These protections are provided by the Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="7b3bd-109">Některá pravidla můžete způsobit falešně pozitivních zjištění a blokování skutečné provozu.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="7b3bd-110">Z tohoto důvodu Application Gateway poskytuje schopnost přizpůsobit skupiny pravidla a pravidla.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-110">For this reason, Application Gateway provides the capability to customize rule groups and rules.</span></span> <span data-ttu-id="7b3bd-111">Další informace o konkrétní pravidlo skupiny a pravidel najdete v tématu [seznam webových aplikací brány firewall řádku pravidlo skupiny a pravidel](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="7b3bd-111">For more information on the specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

>[!NOTE]
> <span data-ttu-id="7b3bd-112">Pokud svoji službu application gateway nepoužívá vrstvě firewall webových aplikací, se zobrazí v pravém podokně možnost upgradu služby application gateway k vrstvě firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-112">If your application gateway is not using the WAF tier, the option to upgrade the application gateway to the WAF tier appears in the right pane.</span></span> 

![Povolit firewall webových aplikací][fig1]

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="7b3bd-114">Zobrazit skupiny pravidla a pravidla</span><span class="sxs-lookup"><span data-stu-id="7b3bd-114">View rule groups and rules</span></span>

<span data-ttu-id="7b3bd-115">**Chcete-li zobrazit skupiny pravidla a pravidla**</span><span class="sxs-lookup"><span data-stu-id="7b3bd-115">**To view rule groups and rules**</span></span>
   1. <span data-ttu-id="7b3bd-116">Přejděte do služby application gateway a potom vyberte **brány firewall webových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-116">Browse to the application gateway, and then select **Web application firewall**.</span></span>  
   2. <span data-ttu-id="7b3bd-117">Vyberte **rozšířeného pravidla konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-117">Select **Advanced rule configuration**.</span></span>  
   <span data-ttu-id="7b3bd-118">Toto zobrazení uvádí tabulku na stránce skupiny pravidel zadat sadou vybrané pravidlo.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-118">This view shows a table on the page of all the rule groups provided with the chosen rule set.</span></span> <span data-ttu-id="7b3bd-119">Jsou vybrány všechny pravidla zaškrtávacích políček.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-119">All of the rule's check boxes are selected.</span></span>

![Konfigurace zakázaná pravidla][1]

## <a name="search-for-rules-to-disable"></a><span data-ttu-id="7b3bd-121">Vyhledejte pravidla mají být zakázána</span><span class="sxs-lookup"><span data-stu-id="7b3bd-121">Search for rules to disable</span></span>

<span data-ttu-id="7b3bd-122">**Webové aplikace, nastavení brány firewall** okno poskytuje možnost filtrovat pravidla prostřednictvím fulltextové vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-122">The **Web application firewall settings** blade provides the capability to filter the rules through a text search.</span></span> <span data-ttu-id="7b3bd-123">Výsledek zobrazí pouze skupiny pravidla a pravidla, které obsahují text, který jste hledali.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-123">The result displays only the rule groups and rules that contain the text you searched for.</span></span>

![Vyhledejte pravidla][2]

## <a name="disable-rule-groups-and-rules"></a><span data-ttu-id="7b3bd-125">Zakázat pravidlo skupiny a pravidel</span><span class="sxs-lookup"><span data-stu-id="7b3bd-125">Disable rule groups and rules</span></span>

<span data-ttu-id="7b3bd-126">Pokud vaše jste zakázání pravidla, můžete zakázat skupinu celé pravidlo nebo konkrétní pravidla v rámci jedné nebo více skupin pravidlo.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-126">When your're disabling rules, you can disable an entire rule group or specific rules under one or more rule groups.</span></span> 

<span data-ttu-id="7b3bd-127">**Chcete-li zakázat pravidlo skupiny nebo konkrétní pravidla**</span><span class="sxs-lookup"><span data-stu-id="7b3bd-127">**To disable rule groups or specific rules**</span></span>

   1. <span data-ttu-id="7b3bd-128">Vyhledat pravidla nebo skupiny pravidel, které chcete zakázat.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-128">Search for the rules or rule groups that you want to disable.</span></span>
   2. <span data-ttu-id="7b3bd-129">Zrušte zaškrtnutí políčka pro pravidla, která chcete zakázat.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-129">Clear the check boxes for the rules that you want to disable.</span></span> 
   2. <span data-ttu-id="7b3bd-130">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-130">Select **Save**.</span></span> 

![Uložit změny][3]

## <a name="next-steps"></a><span data-ttu-id="7b3bd-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b3bd-132">Next steps</span></span>

<span data-ttu-id="7b3bd-133">Po dokončení konfigurace zakázaná pravidla, můžete naučit k zobrazení protokolů firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7b3bd-133">After you configure your disabled rules, you can learn how to view your WAF logs.</span></span> <span data-ttu-id="7b3bd-134">Další informace najdete v tématu [diagnostics Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="7b3bd-134">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png