---
title: Certifikace Microsoft Azure pro SAP | Microsoft Docs
description: "Aktualizovaný seznam aktuální konfigurace a certifikátů SAP na platformě Azure."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: rclaus
ms.custom: 
ms.openlocfilehash: e4d5c78299903659a18aa9b8d04495e215bcca0d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a><span data-ttu-id="31438-103">Certifikace SAP a konfigurace systémem Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="31438-103">SAP certifications and configurations running on Microsoft Azure</span></span>

<span data-ttu-id="31438-104">SAP a Microsoft mají dlouhou spolupráci ve silné spolupráci, která má oboustranně zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="31438-104">SAP and Microsoft have a long history of working together in a strong partnership that has mutual benefits for their customers.</span></span> <span data-ttu-id="31438-105">Společnost Microsoft průběžně aktualizuje jeho platforma a odeslání nové podrobnosti certifikační SAP za účelem zajištění Microsoft Azure je nejlepší platformě, na které se mají spouštět úlohy SAP.</span><span class="sxs-lookup"><span data-stu-id="31438-105">Microsoft is constantly updating its platform and submitting new certification details to SAP in order to ensure Microsoft Azure is the best platform on which to run your SAP workloads.</span></span> <span data-ttu-id="31438-106">Následující tabulky popisují naše podporované konfigurace a seznam narůstají certifikace.</span><span class="sxs-lookup"><span data-stu-id="31438-106">The following tables outline our supported configurations and list of growing certifications.</span></span> 

## <a name="sap-hana-certifications"></a><span data-ttu-id="31438-107">Certifikace SAP HANA</span><span class="sxs-lookup"><span data-stu-id="31438-107">SAP HANA certifications</span></span>

| <span data-ttu-id="31438-108">Produkt SAP</span><span class="sxs-lookup"><span data-stu-id="31438-108">SAP Product</span></span> | <span data-ttu-id="31438-109">Podporovaný operační systém</span><span class="sxs-lookup"><span data-stu-id="31438-109">Supported OS</span></span> | <span data-ttu-id="31438-110">Nabídky Azure</span><span class="sxs-lookup"><span data-stu-id="31438-110">Azure Offerings</span></span> |
| --- | --- | --- |
| <span data-ttu-id="31438-111">SAP HANA Developer Edition (včetně klientský software HANA skládá z SQLODBC, pouze ODBO – Windows, rozhraní ODBC, ovladače JDBC, HANA studio a HANA databáze)</span><span class="sxs-lookup"><span data-stu-id="31438-111">SAP HANA Developer Edition (including the HANA client software comprised of SQLODBC, ODBO-Windows only, ODBC, JDBC drivers, HANA studio, and HANA database)</span></span> |<span data-ttu-id="31438-112">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="31438-112">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> | <span data-ttu-id="31438-113">Počítač D-Series rodiny</span><span class="sxs-lookup"><span data-stu-id="31438-113">D-Series VM family</span></span> |
| <span data-ttu-id="31438-114">HANA One</span><span class="sxs-lookup"><span data-stu-id="31438-114">HANA One</span></span> |<span data-ttu-id="31438-115">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="31438-115">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="31438-116">DS14_v2 (při všeobecné dostupnosti)</span><span class="sxs-lookup"><span data-stu-id="31438-116">DS14_v2 (upon general availability)</span></span> |
| <span data-ttu-id="31438-117">SAP HANA S NEBO 4</span><span class="sxs-lookup"><span data-stu-id="31438-117">SAP S/4 HANA</span></span> |<span data-ttu-id="31438-118">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="31438-118">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="31438-119">Řízené dostupnost pro GS5, SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="31438-119">Controlled Availability for GS5, SAP HANA on Azure (Large instances)</span></span> |
| <span data-ttu-id="31438-120">Suite on HANA, OLTP</span><span class="sxs-lookup"><span data-stu-id="31438-120">Suite on HANA, OLTP</span></span> |<span data-ttu-id="31438-121">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="31438-121">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="31438-122">GS5 pro nasazení jednoho uzlu pro scénáře nevýrobní prostředí SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="31438-122">GS5 for single node deployments for non-production scenarios, SAP HANA on Azure (Large instances)</span></span> |
| <span data-ttu-id="31438-123">HANA Enterprise for BW, OLAP</span><span class="sxs-lookup"><span data-stu-id="31438-123">HANA Enterprise for BW, OLAP</span></span> |<span data-ttu-id="31438-124">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="31438-124">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="31438-125">GS5 pro jeden uzel nasazení SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="31438-125">GS5 for single node deployments, SAP HANA on Azure (Large instances)</span></span> |
| <span data-ttu-id="31438-126">SAP HANA BW NEBO 4</span><span class="sxs-lookup"><span data-stu-id="31438-126">SAP BW/4 HANA</span></span> |<span data-ttu-id="31438-127">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="31438-127">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="31438-128">GS5 pro jeden uzel nasazení SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="31438-128">GS5 for single node deployments, SAP HANA on Azure (Large instances)</span></span> |

## <a name="sap-netweaver-certifications"></a><span data-ttu-id="31438-129">Certifikace SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="31438-129">SAP NetWeaver certifications</span></span>
<span data-ttu-id="31438-130">Microsoft Azure má certifikaci pro následující produkty SAP, se zárukou plné podpory od Microsoftu i SAPu.</span><span class="sxs-lookup"><span data-stu-id="31438-130">Microsoft Azure is certified for the following SAP products, with full support from Microsoft and SAP.</span></span>

| <span data-ttu-id="31438-131">Produkt SAP</span><span class="sxs-lookup"><span data-stu-id="31438-131">SAP Product</span></span> | <span data-ttu-id="31438-132">Hostovaného operačního systému</span><span class="sxs-lookup"><span data-stu-id="31438-132">Guest OS</span></span> | <span data-ttu-id="31438-133">RDBMS</span><span class="sxs-lookup"><span data-stu-id="31438-133">RDBMS</span></span> | <span data-ttu-id="31438-134">Typy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="31438-134">Virtual Machine Types</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="31438-135">SAP Business Suite Software</span><span class="sxs-lookup"><span data-stu-id="31438-135">SAP Business Suite Software</span></span> |<span data-ttu-id="31438-136">Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="31438-136">Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux</span></span> |<span data-ttu-id="31438-137">SQL Server, Oracle (Windows a Oracle Linux pouze), DB2, SAP App Service Environment</span><span class="sxs-lookup"><span data-stu-id="31438-137">SQL Server, Oracle (Windows and Oracle Linux only), DB2, SAP ASE</span></span> |<span data-ttu-id="31438-138">A5 na A11 D11 k D14 DS11 k DS14, DS11_v2 k DS15_v2, spravuje organizace GS1 k GS5</span><span class="sxs-lookup"><span data-stu-id="31438-138">A5 to A11, D11 to D14, DS11 to DS14, DS11_v2 to DS15_v2, GS1 to GS5</span></span> |
| <span data-ttu-id="31438-139">SAP Business All-in-One</span><span class="sxs-lookup"><span data-stu-id="31438-139">SAP Business All-in-One</span></span> |<span data-ttu-id="31438-140">Windows, operačního systému SUSE Linux Enterprise, Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="31438-140">Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux</span></span> |<span data-ttu-id="31438-141">SQL Server, Oracle (Windows a Oracle Linux pouze), DB2, SAP App Service Environment</span><span class="sxs-lookup"><span data-stu-id="31438-141">SQL Server, Oracle (Windows and Oracle Linux only), DB2, SAP ASE</span></span> |<span data-ttu-id="31438-142">A5 na A11 D11 k D14 DS11 k DS14, DS11_v2 k DS15_v2, spravuje organizace GS1 k GS5</span><span class="sxs-lookup"><span data-stu-id="31438-142">A5 to A11, D11 to D14, DS11 to DS14, DS11_v2 to DS15_v2, GS1 to GS5</span></span> |
| <span data-ttu-id="31438-143">SAP BusinessObjects BI</span><span class="sxs-lookup"><span data-stu-id="31438-143">SAP BusinessObjects BI</span></span> |<span data-ttu-id="31438-144">Windows</span><span class="sxs-lookup"><span data-stu-id="31438-144">Windows</span></span> |<span data-ttu-id="31438-145">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="31438-145">N/A</span></span> |<span data-ttu-id="31438-146">A5 na A11 D11 k D14 DS11 k DS14, DS11_v2 k DS15_v2, spravuje organizace GS1 k GS5</span><span class="sxs-lookup"><span data-stu-id="31438-146">A5 to A11, D11 to D14, DS11 to DS14, DS11_v2 to DS15_v2, GS1 to GS5</span></span> |
| <span data-ttu-id="31438-147">SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="31438-147">SAP NetWeaver</span></span> |<span data-ttu-id="31438-148">Windows, operačního systému SUSE Linux Enterprise, Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="31438-148">Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux</span></span> |<span data-ttu-id="31438-149">SQL Server, Oracle (Windows a Oracle Linux pouze), DB2, SAP App Service Environment</span><span class="sxs-lookup"><span data-stu-id="31438-149">SQL Server, Oracle (Windows and Oracle Linux only), DB2, SAP ASE</span></span> |<span data-ttu-id="31438-150">A5 na A11 D11 k D14 DS11 k DS14, DS11_v2 k DS15_v2, spravuje organizace GS1 k GS5</span><span class="sxs-lookup"><span data-stu-id="31438-150">A5 to A11, D11 to D14, DS11 to DS14, DS11_v2 to DS15_v2, GS1 to GS5</span></span> |