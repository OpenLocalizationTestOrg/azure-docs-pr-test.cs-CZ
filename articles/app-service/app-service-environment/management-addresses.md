---
title: "Adresy pro správu Azure App Service Environment"
description: "Zobrazí seznam adres správy používá k příkazu služby App Service Environment"
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
ms.openlocfilehash: e97a084772fd16252d925b62498d2e696629a25d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="app-service-environment-management-addresses"></a><span data-ttu-id="f8595-103">Adresy pro správu App Service Environment</span><span class="sxs-lookup"><span data-stu-id="f8595-103">App Service Environment management addresses</span></span>

<span data-ttu-id="f8595-104">Environment(ASE) služby aplikace je nasazení služby Azure App Service na podsíť ve virtuální síti Azure (VNet).</span><span class="sxs-lookup"><span data-stu-id="f8595-104">The App Service Environment(ASE) is a deployment of the Azure App Service into a subnet in your Azure Virtual Network (VNet).</span></span>  <span data-ttu-id="f8595-105">App Service Environment musí být přístupné ze služby Azure App Service, tak, aby bylo možné jej spravovat.</span><span class="sxs-lookup"><span data-stu-id="f8595-105">The ASE must be accessible from the Azure App Service so that it can be managed.</span></span>  <span data-ttu-id="f8595-106">Tento provoz správy App Service Environment prochází sítí pod kontrolou uživatele.</span><span class="sxs-lookup"><span data-stu-id="f8595-106">This ASE management traffic traverses the user controlled network.</span></span>  <span data-ttu-id="f8595-107">Pochází ze serverů pro správu služby Azure App Service na veřejné VIP, který je přidružen App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="f8595-107">It comes from Azure App Service management servers to the public VIP that is associated with the ASE.</span></span>  <span data-ttu-id="f8595-108">Podrobnosti o App Service Environment sítě závislosti číst [sítě aspekty a App Service Environment][networking].</span><span class="sxs-lookup"><span data-stu-id="f8595-108">For details on the ASE networking dependencies read [Networking considerations and the App Service Environment][networking].</span></span>  <span data-ttu-id="f8595-109">Obecné informace o App Service Environment můžete spustit v [Úvod do služby App Service Environment][intro].</span><span class="sxs-lookup"><span data-stu-id="f8595-109">For general information on the ASE you can start with [Introduction to the App Service Environment][intro].</span></span>

<span data-ttu-id="f8595-110">Tento dokument uvádí zdrojové IP adresy pro přenos pro správu na App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="f8595-110">This document lists the source IPs for management traffic to the ASE.</span></span> <span data-ttu-id="f8595-111">Tyto adresy můžete použít k vytvoření skupin zabezpečení sítě pro zamknout příchozí provoz nebo je používat v směrovací tabulky podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f8595-111">You can use these addresses to create Network Security Groups to lock down incoming traffic or use them in Route Tables as needed.</span></span>  <span data-ttu-id="f8595-112">Tyto informace používat, budete muset použít:</span><span class="sxs-lookup"><span data-stu-id="f8595-112">To use this information you need to use:</span></span>

* <span data-ttu-id="f8595-113">IP adresy, které jsou uvedené pro všechny oblasti</span><span class="sxs-lookup"><span data-stu-id="f8595-113">the IP addresses that are listed for All regions</span></span>
* <span data-ttu-id="f8595-114">IP adresy, které odpovídají oblast, která vaše App Service Environment je nasazena do.</span><span class="sxs-lookup"><span data-stu-id="f8595-114">the IP addresses that match to the region that your ASE is deployed into.</span></span>

<span data-ttu-id="f8595-115">Příchozí přenosy dat správy pochází z těchto IP adres na portech 454 a 455.</span><span class="sxs-lookup"><span data-stu-id="f8595-115">The incoming management traffic comes in from these IP addresses to ports 454 and 455.</span></span>

| <span data-ttu-id="f8595-116">Oblast</span><span class="sxs-lookup"><span data-stu-id="f8595-116">Region</span></span> | <span data-ttu-id="f8595-117">Adresy</span><span class="sxs-lookup"><span data-stu-id="f8595-117">Addresses</span></span> |
|--------|-----------|
| <span data-ttu-id="f8595-118">Všechny oblasti</span><span class="sxs-lookup"><span data-stu-id="f8595-118">All regions</span></span> | <span data-ttu-id="f8595-119">70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141</span><span class="sxs-lookup"><span data-stu-id="f8595-119">70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141</span></span> |
| <span data-ttu-id="f8595-120">Střed USA – jih & střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="f8595-120">South Central US & North Central US</span></span> | <span data-ttu-id="f8595-121">23.102.188.65, 191.236.154.88</span><span class="sxs-lookup"><span data-stu-id="f8595-121">23.102.188.65, 191.236.154.88</span></span> |
| <span data-ttu-id="f8595-122">Austrálie – jihovýchod & Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="f8595-122">Australia Southeast & Australia East</span></span> | <span data-ttu-id="f8595-123">23.101.234.41, 104.210.90.65</span><span class="sxs-lookup"><span data-stu-id="f8595-123">23.101.234.41, 104.210.90.65</span></span> |
| <span data-ttu-id="f8595-124">USA – západ & USA – východ</span><span class="sxs-lookup"><span data-stu-id="f8595-124">US West & US East</span></span> | <span data-ttu-id="f8595-125">104.45.227.37, 191.236.60.72</span><span class="sxs-lookup"><span data-stu-id="f8595-125">104.45.227.37, 191.236.60.72</span></span> |
| <span data-ttu-id="f8595-126">Západní Evropa & Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="f8595-126">West Europe & North Europe</span></span> | <span data-ttu-id="f8595-127">191.233.94.45, 191.237.222.191</span><span class="sxs-lookup"><span data-stu-id="f8595-127">191.233.94.45, 191.237.222.191</span></span> |
| <span data-ttu-id="f8595-128">Střed USA – západ & západní USA 2</span><span class="sxs-lookup"><span data-stu-id="f8595-128">West Central US & West US 2</span></span> | <span data-ttu-id="f8595-129">13.78.148.75, 13.66.225.188</span><span class="sxs-lookup"><span data-stu-id="f8595-129">13.78.148.75, 13.66.225.188</span></span> |
| <span data-ttu-id="f8595-130">Střed USA & východní USA 2</span><span class="sxs-lookup"><span data-stu-id="f8595-130">Central US & East US 2</span></span> | <span data-ttu-id="f8595-131">104.43.165.73, 104.46.108.135</span><span class="sxs-lookup"><span data-stu-id="f8595-131">104.43.165.73, 104.46.108.135</span></span> |
| <span data-ttu-id="f8595-132">Východní Asie & Asie a Tichomoří – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="f8595-132">East Asia & Southeast Asia</span></span> | <span data-ttu-id="f8595-133">23.99.115.5, 104.215.158.33</span><span class="sxs-lookup"><span data-stu-id="f8595-133">23.99.115.5, 104.215.158.33</span></span> |
| <span data-ttu-id="f8595-134">Japonsko – východ & Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="f8595-134">Japan East & Japan West</span></span> | <span data-ttu-id="f8595-135">104.41.185.116, 191.239.104.48</span><span class="sxs-lookup"><span data-stu-id="f8595-135">104.41.185.116, 191.239.104.48</span></span> |
| <span data-ttu-id="f8595-136">Střední Kanada & Východní Kanada</span><span class="sxs-lookup"><span data-stu-id="f8595-136">Canada Central & Canada East</span></span> | <span data-ttu-id="f8595-137">40.85.230.101, 40.86.229.100</span><span class="sxs-lookup"><span data-stu-id="f8595-137">40.85.230.101, 40.86.229.100</span></span> |
| <span data-ttu-id="f8595-138">Spojené království – západ a Spojené království – jih</span><span class="sxs-lookup"><span data-stu-id="f8595-138">UK West & UK South</span></span> | <span data-ttu-id="f8595-139">51.141.8.34, 51.140.185.75</span><span class="sxs-lookup"><span data-stu-id="f8595-139">51.141.8.34, 51.140.185.75</span></span> |
| <span data-ttu-id="f8595-140">Korejská Jižní & Korejská – střed</span><span class="sxs-lookup"><span data-stu-id="f8595-140">Korea South & Korea Central</span></span> | <span data-ttu-id="f8595-141">52.231.200.177, 52.231.32.117</span><span class="sxs-lookup"><span data-stu-id="f8595-141">52.231.200.177, 52.231.32.117</span></span> |
| <span data-ttu-id="f8595-142">Brazílie – jih & střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="f8595-142">Brazil South & South Central US</span></span>| <span data-ttu-id="f8595-143">104.41.46.178, 23.102.188.65</span><span class="sxs-lookup"><span data-stu-id="f8595-143">104.41.46.178, 23.102.188.65</span></span> |
| <span data-ttu-id="f8595-144">Střed & Indie – jih</span><span class="sxs-lookup"><span data-stu-id="f8595-144">Central India & South India</span></span> | <span data-ttu-id="f8595-145">104.211.98.24, 104.211.225.66</span><span class="sxs-lookup"><span data-stu-id="f8595-145">104.211.98.24, 104.211.225.66</span></span> |
| <span data-ttu-id="f8595-146">Indie – Západ, Indie & Indie – jih</span><span class="sxs-lookup"><span data-stu-id="f8595-146">West India & South India</span></span> | <span data-ttu-id="f8595-147">104.211.160.229, 104.211.225.66</span><span class="sxs-lookup"><span data-stu-id="f8595-147">104.211.160.229, 104.211.225.66</span></span> |


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
