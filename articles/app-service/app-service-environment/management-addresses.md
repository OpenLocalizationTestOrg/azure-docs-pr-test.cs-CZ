---
title: "adresy pro správu aaaAzure App Service Environment"
description: "Seznamy hello správy adresy použity toocommand služby App Service Environment"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: b34b6266dc3a35915421b14bf34eddc07c2825c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-management-addresses"></a><span data-ttu-id="cf6ea-103">Adresy pro správu App Service Environment</span><span class="sxs-lookup"><span data-stu-id="cf6ea-103">App Service Environment management addresses</span></span>

<span data-ttu-id="cf6ea-104">Hello App Service Environment(ASE) je nasazení hello Azure App Service na podsíť ve virtuální síti Azure (VNet).</span><span class="sxs-lookup"><span data-stu-id="cf6ea-104">hello App Service Environment(ASE) is a deployment of hello Azure App Service into a subnet in your Azure Virtual Network (VNet).</span></span>  <span data-ttu-id="cf6ea-105">Hello App Service Environment musí být dostupný z hello Azure App Service, tak, aby bylo možné jej spravovat.</span><span class="sxs-lookup"><span data-stu-id="cf6ea-105">hello ASE must be accessible from hello Azure App Service so that it can be managed.</span></span>  <span data-ttu-id="cf6ea-106">Tento provoz správy App Service Environment prochází hello pod kontrolou uživatele sítě.</span><span class="sxs-lookup"><span data-stu-id="cf6ea-106">This ASE management traffic traverses hello user controlled network.</span></span>  <span data-ttu-id="cf6ea-107">Pochází ze služby Azure App Service management servery toohello veřejné VIP přidružený hello App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="cf6ea-107">It comes from Azure App Service management servers toohello public VIP that is associated with hello ASE.</span></span>  <span data-ttu-id="cf6ea-108">Podrobnosti o App Service Environment hello sítě závislosti číst [sítě aspekty a hello App Service Environment][networking].</span><span class="sxs-lookup"><span data-stu-id="cf6ea-108">For details on hello ASE networking dependencies read [Networking considerations and hello App Service Environment][networking].</span></span>  <span data-ttu-id="cf6ea-109">Obecné informace o App Service Environment hello můžete spustit v [toohello Úvod App Service Environment][intro].</span><span class="sxs-lookup"><span data-stu-id="cf6ea-109">For general information on hello ASE you can start with [Introduction toohello App Service Environment][intro].</span></span>

<span data-ttu-id="cf6ea-110">Tento dokument uvádí hello zdrojové IP adresy pro správu provoz toohello App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="cf6ea-110">This document lists hello source IPs for management traffic toohello ASE.</span></span> <span data-ttu-id="cf6ea-111">Můžete použít tyto adresy toocreate skupin zabezpečení sítě toolock dolů příchozí provoz nebo je podle potřeby použijte v směrovací tabulky.</span><span class="sxs-lookup"><span data-stu-id="cf6ea-111">You can use these addresses toocreate Network Security Groups toolock down incoming traffic or use them in Route Tables as needed.</span></span>  <span data-ttu-id="cf6ea-112">toouse tyto informace budete potřebovat toouse:</span><span class="sxs-lookup"><span data-stu-id="cf6ea-112">toouse this information you need toouse:</span></span>

* <span data-ttu-id="cf6ea-113">Hello IP adresy, které jsou uvedeny pro všechny oblasti</span><span class="sxs-lookup"><span data-stu-id="cf6ea-113">hello IP addresses that are listed for All regions</span></span>
* <span data-ttu-id="cf6ea-114">Hello IP adresy, že shoda toohello oblast, kterou vaše App Service Environment je nasazený do.</span><span class="sxs-lookup"><span data-stu-id="cf6ea-114">hello IP addresses that match toohello region that your ASE is deployed into.</span></span>

<span data-ttu-id="cf6ea-115">příchozí provoz správy Hello pochází z těchto IP adres tooports 454 a 455.</span><span class="sxs-lookup"><span data-stu-id="cf6ea-115">hello incoming management traffic comes in from these IP addresses tooports 454 and 455.</span></span>

| <span data-ttu-id="cf6ea-116">Oblast</span><span class="sxs-lookup"><span data-stu-id="cf6ea-116">Region</span></span> | <span data-ttu-id="cf6ea-117">Adresy</span><span class="sxs-lookup"><span data-stu-id="cf6ea-117">Addresses</span></span> |
|--------|-----------|
| <span data-ttu-id="cf6ea-118">Všechny oblasti</span><span class="sxs-lookup"><span data-stu-id="cf6ea-118">All regions</span></span> | <span data-ttu-id="cf6ea-119">70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141</span><span class="sxs-lookup"><span data-stu-id="cf6ea-119">70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141</span></span> |
| <span data-ttu-id="cf6ea-120">Střed USA – jih & střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="cf6ea-120">South Central US & North Central US</span></span> | <span data-ttu-id="cf6ea-121">23.102.188.65, 191.236.154.88</span><span class="sxs-lookup"><span data-stu-id="cf6ea-121">23.102.188.65, 191.236.154.88</span></span> |
| <span data-ttu-id="cf6ea-122">Austrálie – jihovýchod & Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="cf6ea-122">Australia Southeast & Australia East</span></span> | <span data-ttu-id="cf6ea-123">23.101.234.41, 104.210.90.65</span><span class="sxs-lookup"><span data-stu-id="cf6ea-123">23.101.234.41, 104.210.90.65</span></span> |
| <span data-ttu-id="cf6ea-124">USA – západ & USA – východ</span><span class="sxs-lookup"><span data-stu-id="cf6ea-124">US West & US East</span></span> | <span data-ttu-id="cf6ea-125">104.45.227.37, 191.236.60.72</span><span class="sxs-lookup"><span data-stu-id="cf6ea-125">104.45.227.37, 191.236.60.72</span></span> |
| <span data-ttu-id="cf6ea-126">Západní Evropa & Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="cf6ea-126">West Europe & North Europe</span></span> | <span data-ttu-id="cf6ea-127">191.233.94.45, 191.237.222.191</span><span class="sxs-lookup"><span data-stu-id="cf6ea-127">191.233.94.45, 191.237.222.191</span></span> |
| <span data-ttu-id="cf6ea-128">Střed USA – západ & západní USA 2</span><span class="sxs-lookup"><span data-stu-id="cf6ea-128">West Central US & West US 2</span></span> | <span data-ttu-id="cf6ea-129">13.78.148.75, 13.66.225.188</span><span class="sxs-lookup"><span data-stu-id="cf6ea-129">13.78.148.75, 13.66.225.188</span></span> |
| <span data-ttu-id="cf6ea-130">Střed USA & východní USA 2</span><span class="sxs-lookup"><span data-stu-id="cf6ea-130">Central US & East US 2</span></span> | <span data-ttu-id="cf6ea-131">104.43.165.73, 104.46.108.135</span><span class="sxs-lookup"><span data-stu-id="cf6ea-131">104.43.165.73, 104.46.108.135</span></span> |
| <span data-ttu-id="cf6ea-132">Východní Asie & Asie a Tichomoří – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="cf6ea-132">East Asia & Southeast Asia</span></span> | <span data-ttu-id="cf6ea-133">23.99.115.5, 104.215.158.33</span><span class="sxs-lookup"><span data-stu-id="cf6ea-133">23.99.115.5, 104.215.158.33</span></span> |
| <span data-ttu-id="cf6ea-134">Japonsko – východ & Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="cf6ea-134">Japan East & Japan West</span></span> | <span data-ttu-id="cf6ea-135">104.41.185.116, 191.239.104.48</span><span class="sxs-lookup"><span data-stu-id="cf6ea-135">104.41.185.116, 191.239.104.48</span></span> |
| <span data-ttu-id="cf6ea-136">Střední Kanada & Východní Kanada</span><span class="sxs-lookup"><span data-stu-id="cf6ea-136">Canada Central & Canada East</span></span> | <span data-ttu-id="cf6ea-137">40.85.230.101, 40.86.229.100</span><span class="sxs-lookup"><span data-stu-id="cf6ea-137">40.85.230.101, 40.86.229.100</span></span> |
| <span data-ttu-id="cf6ea-138">Spojené království – západ a Spojené království – jih</span><span class="sxs-lookup"><span data-stu-id="cf6ea-138">UK West & UK South</span></span> | <span data-ttu-id="cf6ea-139">51.141.8.34, 51.140.185.75</span><span class="sxs-lookup"><span data-stu-id="cf6ea-139">51.141.8.34, 51.140.185.75</span></span> |
| <span data-ttu-id="cf6ea-140">Korejská Jižní & Korejská – střed</span><span class="sxs-lookup"><span data-stu-id="cf6ea-140">Korea South & Korea Central</span></span> | <span data-ttu-id="cf6ea-141">52.231.200.177, 52.231.32.117</span><span class="sxs-lookup"><span data-stu-id="cf6ea-141">52.231.200.177, 52.231.32.117</span></span> |
| <span data-ttu-id="cf6ea-142">Brazílie – jih & střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="cf6ea-142">Brazil South & South Central US</span></span>| <span data-ttu-id="cf6ea-143">104.41.46.178, 23.102.188.65</span><span class="sxs-lookup"><span data-stu-id="cf6ea-143">104.41.46.178, 23.102.188.65</span></span> |
| <span data-ttu-id="cf6ea-144">Střed & Indie – jih</span><span class="sxs-lookup"><span data-stu-id="cf6ea-144">Central India & South India</span></span> | <span data-ttu-id="cf6ea-145">104.211.98.24, 104.211.225.66</span><span class="sxs-lookup"><span data-stu-id="cf6ea-145">104.211.98.24, 104.211.225.66</span></span> |
| <span data-ttu-id="cf6ea-146">Indie – Západ, Indie & Indie – jih</span><span class="sxs-lookup"><span data-stu-id="cf6ea-146">West India & South India</span></span> | <span data-ttu-id="cf6ea-147">104.211.160.229, 104.211.225.66</span><span class="sxs-lookup"><span data-stu-id="cf6ea-147">104.211.160.229, 104.211.225.66</span></span> |


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
