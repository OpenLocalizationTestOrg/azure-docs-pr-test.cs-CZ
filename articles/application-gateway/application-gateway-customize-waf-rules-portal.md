---
title: "aaaCustomize webové aplikace pravidla brány firewall v Azure Application Gateway - portálu Azure | Microsoft Docs"
description: "Tento článek obsahuje informace o tom, jak pravidla brány firewall webových aplikací toocustomize v aplikační brány s hello portálu Azure."
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
ms.openlocfilehash: 36a999279e0370b9f803e12257856a56753b23a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="82c4c-103">Přizpůsobení pravidel brány firewall webových aplikací prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="82c4c-103">Customize web application firewall rules through hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="82c4c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="82c4c-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="82c4c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="82c4c-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="82c4c-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="82c4c-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="82c4c-107">brány firewall webových aplikací Hello Azure Application Gateway (firewall webových aplikací) poskytuje ochranu pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="82c4c-107">hello Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="82c4c-108">Tyto ochrany jsou poskytovány hello aplikace otevřete webový projekt zabezpečení (OWASP) nastavit pravidlo jádra (řádku).</span><span class="sxs-lookup"><span data-stu-id="82c4c-108">These protections are provided by hello Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="82c4c-109">Některá pravidla můžete způsobit falešně pozitivních zjištění a blokování skutečné provozu.</span><span class="sxs-lookup"><span data-stu-id="82c4c-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="82c4c-110">Z tohoto důvodu Application Gateway poskytuje hello schopností toocustomize pravidlo skupiny a pravidel.</span><span class="sxs-lookup"><span data-stu-id="82c4c-110">For this reason, Application Gateway provides hello capability toocustomize rule groups and rules.</span></span> <span data-ttu-id="82c4c-111">Další informace o hello konkrétní pravidlo skupiny a pravidel najdete v tématu [seznam webových aplikací brány firewall řádku pravidlo skupiny a pravidel](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="82c4c-111">For more information on hello specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

>[!NOTE]
> <span data-ttu-id="82c4c-112">Pokud svoji službu application gateway nepoužívá hello firewall webových aplikací vrstvy, hello možnost tooupgrade hello aplikace brány toohello firewall webových aplikací úroveň se zobrazí v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="82c4c-112">If your application gateway is not using hello WAF tier, hello option tooupgrade hello application gateway toohello WAF tier appears in hello right pane.</span></span> 

![Povolit firewall webových aplikací][fig1]

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="82c4c-114">Zobrazit skupiny pravidla a pravidla</span><span class="sxs-lookup"><span data-stu-id="82c4c-114">View rule groups and rules</span></span>

<span data-ttu-id="82c4c-115">**skupiny tooview pravidla a pravidla**</span><span class="sxs-lookup"><span data-stu-id="82c4c-115">**tooview rule groups and rules**</span></span>
   1. <span data-ttu-id="82c4c-116">Procházet toohello aplikační bránu a potom vyberte **brány firewall webových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="82c4c-116">Browse toohello application gateway, and then select **Web application firewall**.</span></span>  
   2. <span data-ttu-id="82c4c-117">Vyberte **rozšířeného pravidla konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="82c4c-117">Select **Advanced rule configuration**.</span></span>  
   <span data-ttu-id="82c4c-118">Toto zobrazení uvádí tabulku na stránku hello všech skupin pravidlo hello součástí hello zvolené sady pravidel.</span><span class="sxs-lookup"><span data-stu-id="82c4c-118">This view shows a table on hello page of all hello rule groups provided with hello chosen rule set.</span></span> <span data-ttu-id="82c4c-119">Jsou vybrány všechny hello pravidlo zaškrtávacích políček.</span><span class="sxs-lookup"><span data-stu-id="82c4c-119">All of hello rule's check boxes are selected.</span></span>

![Konfigurace zakázaná pravidla][1]

## <a name="search-for-rules-toodisable"></a><span data-ttu-id="82c4c-121">Vyhledejte toodisable pravidla</span><span class="sxs-lookup"><span data-stu-id="82c4c-121">Search for rules toodisable</span></span>

<span data-ttu-id="82c4c-122">Hello **webové aplikace, nastavení brány firewall** okno poskytuje možnost hello toofilter hello pravidla prostřednictvím fulltextové vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="82c4c-122">hello **Web application firewall settings** blade provides hello capability toofilter hello rules through a text search.</span></span> <span data-ttu-id="82c4c-123">výsledek Hello zobrazí pouze skupiny hello pravidla a pravidla, které obsahují text hello, kterého jste hledali.</span><span class="sxs-lookup"><span data-stu-id="82c4c-123">hello result displays only hello rule groups and rules that contain hello text you searched for.</span></span>

![Vyhledejte pravidla][2]

## <a name="disable-rule-groups-and-rules"></a><span data-ttu-id="82c4c-125">Zakázat pravidlo skupiny a pravidel</span><span class="sxs-lookup"><span data-stu-id="82c4c-125">Disable rule groups and rules</span></span>

<span data-ttu-id="82c4c-126">Pokud vaše jste zakázání pravidla, můžete zakázat skupinu celé pravidlo nebo konkrétní pravidla v rámci jedné nebo více skupin pravidlo.</span><span class="sxs-lookup"><span data-stu-id="82c4c-126">When your're disabling rules, you can disable an entire rule group or specific rules under one or more rule groups.</span></span> 

<span data-ttu-id="82c4c-127">**toodisable pravidlo skupiny nebo konkrétní pravidla**</span><span class="sxs-lookup"><span data-stu-id="82c4c-127">**toodisable rule groups or specific rules**</span></span>

   1. <span data-ttu-id="82c4c-128">Hledat hello pravidla nebo skupiny pravidel, které chcete toodisable.</span><span class="sxs-lookup"><span data-stu-id="82c4c-128">Search for hello rules or rule groups that you want toodisable.</span></span>
   2. <span data-ttu-id="82c4c-129">Zrušte zaškrtnutí políček hello hello pravidla, které chcete toodisable.</span><span class="sxs-lookup"><span data-stu-id="82c4c-129">Clear hello check boxes for hello rules that you want toodisable.</span></span> 
   2. <span data-ttu-id="82c4c-130">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="82c4c-130">Select **Save**.</span></span> 

![Uložit změny][3]

## <a name="next-steps"></a><span data-ttu-id="82c4c-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82c4c-132">Next steps</span></span>

<span data-ttu-id="82c4c-133">Po dokončení konfigurace zakázaná pravidla, dozvíte, jak tooview protokolů firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="82c4c-133">After you configure your disabled rules, you can learn how tooview your WAF logs.</span></span> <span data-ttu-id="82c4c-134">Další informace najdete v tématu [diagnostics Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="82c4c-134">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
