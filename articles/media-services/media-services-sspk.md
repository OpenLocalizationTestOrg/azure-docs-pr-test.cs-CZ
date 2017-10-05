---
title: "Licencování Smooth streamování klienta Microsoft® portování Kit"
description: "Další informace o tom k licencování Microsoft® technologie Smooth Streaming klienta portování Kit."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: xpouyat
ms.openlocfilehash: b5a36ac6771bef220afe29446cd56c1b65a498d9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a><span data-ttu-id="dd66a-103">Licencování Smooth streamování klienta Microsoft® portování Kit</span><span class="sxs-lookup"><span data-stu-id="dd66a-103">Licensing Microsoft® Smooth Streaming Client Porting Kit</span></span>
## <a name="overview"></a><span data-ttu-id="dd66a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="dd66a-104">Overview</span></span>
<span data-ttu-id="dd66a-105">Microsoft technologie Smooth Streaming klienta portování Kit (**SSPK** pro zkrácení) je technologie Smooth Streaming implementace klienta, která je optimalizovaná tak, aby pomůže výrobců vložená zařízení, kabel a mobilní operátory, poskytovatelé přístupu k obsahu, telefonního sluchátka výrobců nezávislí dodavatelé softwaru (ISV) a poskytovatelé řešení k vytvoření produktů a služeb pro streamování s adaptivní streamování obsah ve formátu technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="dd66a-105">Microsoft Smooth Streaming Client Porting Kit (**SSPK** for short) is a Smooth Streaming client implementation that is optimized to help embedded device manufacturers, cable and mobile operators, content service providers, handset manufacturers, independent software vendors (ISVs), and solution providers to create products and services for streaming adaptive streaming content in Smooth Streaming format.</span></span> <span data-ttu-id="dd66a-106">SSPK je na zařízení a platformy nezávislé implementace technologie Smooth Streaming klienta, který může přenést držitel licence na žádný zařízení a platformy.</span><span class="sxs-lookup"><span data-stu-id="dd66a-106">SSPK is a device and platform independent implementation of Smooth Streaming client that can be ported by the licensee to any device and platform.</span></span> 

<span data-ttu-id="dd66a-107">Uvedeném níž je nejvyšší úrovni architektura a pole IIS technologie Smooth Streaming portování Kit je implementace klienta funkce Smooth Streaming od společnosti Microsoft a obsahuje všechny základní logiku pro přehrávání obsahu, technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="dd66a-107">Included below is a high level architecture and IIS Smooth Streaming Porting Kit box is the Smooth Streaming Client implementation provided by Microsoft and includes all the core logic for playback of Smooth Streaming content.</span></span> <span data-ttu-id="dd66a-108">To je poté přenést partnery pro určité zařízení nebo platformu implementací příslušná rozhraní.</span><span class="sxs-lookup"><span data-stu-id="dd66a-108">This is then ported by partners for a specific device or platform by implementing appropriate interfaces.</span></span> 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a><span data-ttu-id="dd66a-110">Popis</span><span class="sxs-lookup"><span data-stu-id="dd66a-110">Description</span></span>
<span data-ttu-id="dd66a-111">SSPK je licencovaná podle podmínek, které nabízí vynikající obchodní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dd66a-111">SSPK is licensed on terms that offer excellent business value.</span></span> <span data-ttu-id="dd66a-112">Licence SSPK poskytuje oborový s:</span><span class="sxs-lookup"><span data-stu-id="dd66a-112">SSPK license provides the industry with:</span></span>

* <span data-ttu-id="dd66a-113">Technologie Smooth Streaming portování Kit zdroje v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="dd66a-113">Smooth Streaming Porting Kit source in C++</span></span> 
  * <span data-ttu-id="dd66a-114">implementuje funkce klienta funkce Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="dd66a-114">implements Smooth Streaming Client functionality</span></span>
  * <span data-ttu-id="dd66a-115">Přidá formátu analýzy heuristiky, ukládání do vyrovnávací paměti logiku atd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-115">adds format parsing, heuristics, buffering logic, etc.</span></span>
* <span data-ttu-id="dd66a-116">Aplikace přehrávače rozhraní API</span><span class="sxs-lookup"><span data-stu-id="dd66a-116">Player application APIs</span></span> 
  * <span data-ttu-id="dd66a-117">programovací rozhraní pro interakci s aplikací media player</span><span class="sxs-lookup"><span data-stu-id="dd66a-117">programming interfaces for interaction with a media player application</span></span>
* <span data-ttu-id="dd66a-118">Platforma Abstraction Layer (PAL) rozhraní</span><span class="sxs-lookup"><span data-stu-id="dd66a-118">Platform Abstraction Layer (PAL) Interface</span></span> 
  * <span data-ttu-id="dd66a-119">programovací rozhraní pro interakci s operačním systémem (vláken, sockets)</span><span class="sxs-lookup"><span data-stu-id="dd66a-119">programming interfaces for interaction with the operating system (threads, sockets)</span></span>
* <span data-ttu-id="dd66a-120">Hardware abstraktní vrstvu HAL rozhraní</span><span class="sxs-lookup"><span data-stu-id="dd66a-120">Hardware Abstraction Layer (HAL) Interface</span></span> 
  * <span data-ttu-id="dd66a-121">programovací rozhraní pro interakci s hardwarem A / dekodérů V (dekódování, vykreslování)</span><span class="sxs-lookup"><span data-stu-id="dd66a-121">programming interfaces for interaction with hardware A/V decoders (decoding, rendering)</span></span>
* <span data-ttu-id="dd66a-122">Rozhraní digitální Rights Management (DRM)</span><span class="sxs-lookup"><span data-stu-id="dd66a-122">Digital Rights Management (DRM) Interface</span></span> 
  * <span data-ttu-id="dd66a-123">programovací rozhraní pro zpracování DRM prostřednictvím DRM Abstraction Layer (DAL)</span><span class="sxs-lookup"><span data-stu-id="dd66a-123">programming interfaces for handling DRM through the DRM Abstraction Layer (DAL)</span></span>
  * <span data-ttu-id="dd66a-124">Microsoft PlayReady portování Kit dodává samostatně, ale integruje prostřednictvím tohoto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="dd66a-124">Microsoft PlayReady Porting Kit ships separately but integrates through this interface.</span></span> <span data-ttu-id="dd66a-125">Další informace o licencování Microsoft PlayReady Device, klikněte na tlačítko [zde](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span><span class="sxs-lookup"><span data-stu-id="dd66a-125">For more details on Microsoft PlayReady Device licensing, click [here](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span></span>
* <span data-ttu-id="dd66a-126">Implementace ukázky</span><span class="sxs-lookup"><span data-stu-id="dd66a-126">Implementation samples</span></span> 
  * <span data-ttu-id="dd66a-127">Ukázka PAL implementace pro Linux</span><span class="sxs-lookup"><span data-stu-id="dd66a-127">sample PAL implementation for Linux</span></span>
  * <span data-ttu-id="dd66a-128">Ukázka HAL implementace pro GStreamer</span><span class="sxs-lookup"><span data-stu-id="dd66a-128">sample HAL implementation for GStreamer</span></span>

## <a name="licensing-options"></a><span data-ttu-id="dd66a-129">Možnosti licencování</span><span class="sxs-lookup"><span data-stu-id="dd66a-129">Licensing Options</span></span>
<span data-ttu-id="dd66a-130">Microsoft technologie Smooth Streaming klienta portování Kit je k dispozici pro držitele licence v rámci dvou různých licenční smlouvy: jeden pro vývoj technologie Smooth Streaming produkty klienta provizorní a druhý pro distribuci technologie Smooth Streaming klienta konečné produkty pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="dd66a-130">Microsoft Smooth Streaming Client Porting Kit is made available to licensees under two distinct license agreements: one for developing Smooth Streaming Client Interim Products and another for distributing Smooth Streaming Client Final Products to end users.</span></span>

* <span data-ttu-id="dd66a-131">Pro čipovou sadou výrobců, integrátory nebo nezávislé softwaru dodavatelé (ISV), kteří vyžadují zdroj kódu přenosem kit vyvíjet provizorní produkty, Microsoft technologie Smooth Streaming klienta portování Kit **licenci produktu provizorní** by měl být spuštěn.</span><span class="sxs-lookup"><span data-stu-id="dd66a-131">For chipset manufacturers, system integrators, or independent software vendors (ISVs) who require a source code porting kit to develop Interim Products, a Microsoft Smooth Streaming Client Porting Kit **Interim Product License** should be executed.</span></span>
* <span data-ttu-id="dd66a-132">Pro výrobce zařízení nebo nezávislí výrobci softwaru, kteří potřebují oprávnění pro distribuční produktů technologie Smooth Streaming klienta konečné koncovým uživatelům, technologie Smooth Streaming klienta portování sadu Kit Microsoft **konečné licenci produktu** by měl být spuštěn.</span><span class="sxs-lookup"><span data-stu-id="dd66a-132">For device manufacturers or ISVs who require distribution rights for Smooth Streaming Client Final Products to end users, the Microsoft Smooth Streaming Client Porting Kit **Final Product License** should be executed.</span></span>

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a><span data-ttu-id="dd66a-133">Microsoft klienta funkce Smooth Streaming portování licenci produktu provizorní Kit</span><span class="sxs-lookup"><span data-stu-id="dd66a-133">Microsoft Smooth Streaming Client Porting Kit Interim Product License</span></span>
<span data-ttu-id="dd66a-134">V části tuto licenci společnost Microsoft nabízí technologie Smooth Streaming klienta portování sady a potřebné duševnímu vlastnictví vyvinout a distribuovat technologie Smooth Streaming produkty klienta provizorní pro držitele licence jiné technologie Smooth Streaming klienta portování Kit zařízení, Distribuujte technologie Smooth Streaming klienta konečné produkty.</span><span class="sxs-lookup"><span data-stu-id="dd66a-134">Under this license, Microsoft offers a Smooth Streaming Client Porting Kit and the necessary intellectual property rights to develop and distribute Smooth Streaming Client Interim Products to other Smooth Streaming Client Porting Kit device licensees that distribute Smooth Streaming Client Final Products.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="dd66a-135">Struktura poplatků</span><span class="sxs-lookup"><span data-stu-id="dd66a-135">Fee structure</span></span>
<span data-ttu-id="dd66a-136">Jednorázové licenční poplatek 50 000 USA Kč poskytuje přístup k sadě technologie Smooth Streaming klienta portování Kit.</span><span class="sxs-lookup"><span data-stu-id="dd66a-136">A U.S. $50,000 one-time license fee provides access to the Smooth Streaming Client Porting Kit.</span></span> 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a><span data-ttu-id="dd66a-137">Microsoft klienta funkce Smooth Streaming portování licenci produktu konečné Kit</span><span class="sxs-lookup"><span data-stu-id="dd66a-137">Microsoft Smooth Streaming Client Porting Kit Final Product License</span></span>
<span data-ttu-id="dd66a-138">V části tuto licenci společnost Microsoft nabízí všechny potřebné duševnímu vlastnictví přijímat technologie Smooth Streaming produkty klienta provizorní z jiné technologie Smooth Streaming klienta portování Kit držitele licence a distribuovat značky společnosti technologie Smooth Streaming klienta konečné Produkty pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="dd66a-138">Under this license, Microsoft offers all necessary intellectual property rights to receive Smooth Streaming Client Interim Products from other Smooth Streaming Client Porting Kit licensees and to distribute company-branded Smooth Streaming Client Final Products to end users.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="dd66a-139">Struktura poplatků</span><span class="sxs-lookup"><span data-stu-id="dd66a-139">Fee structure</span></span>
<span data-ttu-id="dd66a-140">Technologie Smooth Streaming konečné produktu klienta se nabízí v rámci modelu Licencovaní jako v části:</span><span class="sxs-lookup"><span data-stu-id="dd66a-140">The Smooth Streaming Client Final Product is offered under a royalty model as under:</span></span>

* <span data-ttu-id="dd66a-141">dodaný 0.10 za implementaci zařízení</span><span class="sxs-lookup"><span data-stu-id="dd66a-141">$0.10 per device implementation shipped</span></span>
* <span data-ttu-id="dd66a-142">Licencovaní je omezená na 50 000 $ každý rok</span><span class="sxs-lookup"><span data-stu-id="dd66a-142">The royalty is capped at $50,000 each year</span></span>
* <span data-ttu-id="dd66a-143">Žádné licencované pro prvních 10 000 zařízení implementace každý rok</span><span class="sxs-lookup"><span data-stu-id="dd66a-143">No royalty for first 10,000 device implementations each year</span></span> 

## <a name="licensing-procedure-and-sspk-access"></a><span data-ttu-id="dd66a-144">Licencování postupu a SSPK přístup</span><span class="sxs-lookup"><span data-stu-id="dd66a-144">Licensing Procedure and SSPK access</span></span>
<span data-ttu-id="dd66a-145">E-mailem [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) pro všechny licencování dotazy.</span><span class="sxs-lookup"><span data-stu-id="dd66a-145">Please email [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) for all licensing queries.</span></span>

<span data-ttu-id="dd66a-146">[SSPK distribuční portál](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) je pro držitele licence registrované provizorní dostupné.</span><span class="sxs-lookup"><span data-stu-id="dd66a-146">The [SSPK Distribution portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) is accessible to registered Interim licensees.</span></span>

<span data-ttu-id="dd66a-147">Provizorní a finální SSPK držitele licence můžete odeslat technické dotazy k [ smoothpk@microsoft.com ](mailto:smoothpk@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="dd66a-147">Interim and Final SSPK licensees can submit technical questions to [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).</span></span>

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a><span data-ttu-id="dd66a-148">Microsoft funkce Smooth Streaming držitele licence smlouvy dočasné produktu klienta</span><span class="sxs-lookup"><span data-stu-id="dd66a-148">Microsoft Smooth Streaming Client Interim Product Agreement Licensees</span></span>
* <span data-ttu-id="dd66a-149">Adroit obchodní řešení, Inc</span><span class="sxs-lookup"><span data-stu-id="dd66a-149">Adroit Business Solutions, Inc</span></span>
* <span data-ttu-id="dd66a-150">Pokročilé SA digitální všesměrové vysílání</span><span class="sxs-lookup"><span data-stu-id="dd66a-150">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="dd66a-151">AirTies Kablosuz Iletism Sanayive zprávy Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="dd66a-151">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="dd66a-152">Albis technologie Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-152">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="dd66a-153">Alticast Corporation</span><span class="sxs-lookup"><span data-stu-id="dd66a-153">Alticast Corporation</span></span>
* <span data-ttu-id="dd66a-154">Digitální Amazon služby, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-154">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="dd66a-155">Technologie Arion, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-155">Arion Technology, Inc.</span></span>
* <span data-ttu-id="dd66a-156">Software multimédia AVC Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-156">AVC Multimedia Software Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-157">Cavium, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-157">Cavium, Inc.</span></span>
* <span data-ttu-id="dd66a-158">Zakoupení Corporation EchoStar</span><span class="sxs-lookup"><span data-stu-id="dd66a-158">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="dd66a-159">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-159">Enseo, Inc.</span></span>
* <span data-ttu-id="dd66a-160">Fluendo S.A.</span><span class="sxs-lookup"><span data-stu-id="dd66a-160">Fluendo S.A.</span></span>
* <span data-ttu-id="dd66a-161">HANDAN BroadInfoCom Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-161">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-162">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="dd66a-162">Infomir GMBH</span></span>
* <span data-ttu-id="dd66a-163">Irdeto USA Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-163">Irdeto USA Inc.</span></span>
* <span data-ttu-id="dd66a-164">iWEDIA S.A.</span><span class="sxs-lookup"><span data-stu-id="dd66a-164">iWEDIA S.A.</span></span> 
* <span data-ttu-id="dd66a-165">Globální služby BV Liberty</span><span class="sxs-lookup"><span data-stu-id="dd66a-165">Liberty Global Services BV</span></span>
* <span data-ttu-id="dd66a-166">MediaTek Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-166">MediaTek Inc.</span></span>
* <span data-ttu-id="dd66a-167">MStar Co, Ltd</span><span class="sxs-lookup"><span data-stu-id="dd66a-167">MStar Co, Ltd</span></span>
* <span data-ttu-id="dd66a-168">NINTENDO Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-168">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-169">OpenTV, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-169">OpenTV, Inc.</span></span>
* <span data-ttu-id="dd66a-170">Omezené digitální šafrán</span><span class="sxs-lookup"><span data-stu-id="dd66a-170">Saffron Digital Limited</span></span>
* <span data-ttu-id="dd66a-171">Elektrický Changhong Sichuan Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="dd66a-171">Sichuan Changhong Electric Co., Ltd</span></span>
* <span data-ttu-id="dd66a-172">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="dd66a-172">SoftAtHome</span></span>
* <span data-ttu-id="dd66a-173">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="dd66a-173">Sony Corporation</span></span>
* <span data-ttu-id="dd66a-174">Tatung Technology, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-174">Tatung Technology Inc.</span></span>
* <span data-ttu-id="dd66a-175">TCL Technoly Electronics (Huizhou) Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-175">TCL Technoly Electronics (Huizhou) Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-176">Horní vítězství investice, Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-176">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="dd66a-177">Vestel Elektronik Sanayi sunout Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="dd66a-177">Vestel Elektronik Sanayi ve Ticaret A.S.</span></span>
* <span data-ttu-id="dd66a-178">VisualOn, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-178">VisualOn, Inc.</span></span>
* <span data-ttu-id="dd66a-179">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="dd66a-179">ZTE Corporation</span></span>

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a><span data-ttu-id="dd66a-180">Microsoft funkce Smooth Streaming držitele licence smlouvu konečného produktu klienta</span><span class="sxs-lookup"><span data-stu-id="dd66a-180">Microsoft Smooth Streaming Client Final Product Agreement Licensees</span></span>
* <span data-ttu-id="dd66a-181">Pokročilé SA digitální všesměrové vysílání</span><span class="sxs-lookup"><span data-stu-id="dd66a-181">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="dd66a-182">AirTies Kablosuz Iletism Sanayive zprávy Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="dd66a-182">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="dd66a-183">Albis technologie Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-183">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="dd66a-184">Digitální Amazon služby, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-184">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="dd66a-185">Technologie AmTRAN Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-185">AmTRAN Technology Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-186">Arcadyan technologie Corporation</span><span class="sxs-lookup"><span data-stu-id="dd66a-186">Arcadyan Technology Corporation</span></span>
* <span data-ttu-id="dd66a-187">Technologie Arion, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-187">Arion Technology, Inc.</span></span>
* <span data-ttu-id="dd66a-188">SÍŤ SAN ELEKTRONİK ATMACA.</span><span class="sxs-lookup"><span data-stu-id="dd66a-188">ATMACA ELEKTRONİK SAN.</span></span> <span data-ttu-id="dd66a-189">JSTE TİC.</span><span class="sxs-lookup"><span data-stu-id="dd66a-189">VE TİC.</span></span> <span data-ttu-id="dd66a-190">A.Ş</span><span class="sxs-lookup"><span data-stu-id="dd66a-190">A.Ş</span></span>
* <span data-ttu-id="dd66a-191">Britské Sky všesměrové vysílání omezené</span><span class="sxs-lookup"><span data-stu-id="dd66a-191">British Sky Broadcasting Limited</span></span>
* <span data-ttu-id="dd66a-192">CastPal technologie Inc., Shenzhen</span><span class="sxs-lookup"><span data-stu-id="dd66a-192">CastPal Technology Inc., Shenzhen</span></span>
* <span data-ttu-id="dd66a-193">Compal Electronics, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-193">Compal Electronics, Inc.</span></span>
* <span data-ttu-id="dd66a-194">Digitální Dongguan AV technologie Corp., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-194">Dongguan Digital AV Technology Corp., Ltd.</span></span>
* <span data-ttu-id="dd66a-195">Zakoupení Corporation EchoStar</span><span class="sxs-lookup"><span data-stu-id="dd66a-195">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="dd66a-196">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-196">Enseo, Inc.</span></span>
* <span data-ttu-id="dd66a-197">Omezené filmy Filmflex</span><span class="sxs-lookup"><span data-stu-id="dd66a-197">Filmflex Movies Limited</span></span>
* <span data-ttu-id="dd66a-198">Fluendo S.A.</span><span class="sxs-lookup"><span data-stu-id="dd66a-198">Fluendo S.A.</span></span>
* <span data-ttu-id="dd66a-199">Omezené inovace Gibson</span><span class="sxs-lookup"><span data-stu-id="dd66a-199">Gibson Innovations Limited</span></span>
* <span data-ttu-id="dd66a-200">Informace o Applicantion Haier S.R.L</span><span class="sxs-lookup"><span data-stu-id="dd66a-200">Haier Information Applicantion S.R.L</span></span>
* <span data-ttu-id="dd66a-201">HANDAN BroadInfoCom Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-201">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-202">Mezinárodní Hisense Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-202">Hisense International Co., Ltd.</span></span> 
* <span data-ttu-id="dd66a-203">Homecast Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="dd66a-203">Homecast Co.,Ltd</span></span>
* <span data-ttu-id="dd66a-204">Svazek Hai přesnost odvětví Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-204">Hon Hai Precision Industry Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-205">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="dd66a-205">Infomir GMBH</span></span>
* <span data-ttu-id="dd66a-206">Kaonmedia Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-206">Kaonmedia Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-207">KDDI Corporation</span><span class="sxs-lookup"><span data-stu-id="dd66a-207">KDDI Corporation</span></span>
* <span data-ttu-id="dd66a-208">NINTENDO Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-208">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-209">SA oranžová</span><span class="sxs-lookup"><span data-stu-id="dd66a-209">Orange SA</span></span>
* <span data-ttu-id="dd66a-210">Omezené digitální šafrán</span><span class="sxs-lookup"><span data-stu-id="dd66a-210">Saffron Digital Limited</span></span>
* <span data-ttu-id="dd66a-211">Širokopásmové připojení Sagemcom SAS</span><span class="sxs-lookup"><span data-stu-id="dd66a-211">Sagemcom Broadband SAS</span></span>
* <span data-ttu-id="dd66a-212">Shenzhen Coship Electronics CO., LTD</span><span class="sxs-lookup"><span data-stu-id="dd66a-212">Shenzhen Coship Electronics CO., LTD</span></span>
* <span data-ttu-id="dd66a-213">Elektrický Jiuzhou Shenzhen Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="dd66a-213">Shenzhen Jiuzhou Electric Co.,Ltd</span></span>
* <span data-ttu-id="dd66a-214">Shenzhen Skyworth digitální technologie Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="dd66a-214">Shenzhen Skyworth Digital Technology Co., Ltd</span></span>
* <span data-ttu-id="dd66a-215">Elektrický Changhong Sichuan Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-215">Sichuan Changhong Electric Co., Ltd.</span></span>
* <span data-ttu-id="dd66a-216">Skardin průmyslové Corp.</span><span class="sxs-lookup"><span data-stu-id="dd66a-216">Skardin Industrial Corp.</span></span>
* <span data-ttu-id="dd66a-217">SKY Německo Fernsehen GmbH & Co. KG</span><span class="sxs-lookup"><span data-stu-id="dd66a-217">Sky Deutschland Fernsehen GmbH & Co. KG</span></span>
* <span data-ttu-id="dd66a-218">SmarDTV S.A.</span><span class="sxs-lookup"><span data-stu-id="dd66a-218">SmarDTV S.A.</span></span>
* <span data-ttu-id="dd66a-219">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="dd66a-219">SoftAtHome</span></span>
* <span data-ttu-id="dd66a-220">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="dd66a-220">Sony Corporation</span></span>
* <span data-ttu-id="dd66a-221">Omezené TCL zámořské Marketing (moři komerční zboží)</span><span class="sxs-lookup"><span data-stu-id="dd66a-221">TCL Overseas Marketing (Macao Commercial Offshore) Limited</span></span>
* <span data-ttu-id="dd66a-222">Technicolor doručení technologie SAS</span><span class="sxs-lookup"><span data-stu-id="dd66a-222">Technicolor Delivery Technologies, SAS</span></span>
* <span data-ttu-id="dd66a-223">Tongfang globální Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-223">Tongfang Global Ltd.</span></span>
* <span data-ttu-id="dd66a-224">Horní vítězství investice, Ltd.</span><span class="sxs-lookup"><span data-stu-id="dd66a-224">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="dd66a-225">Produkty Toshiba Lifestyle & Corporation služby</span><span class="sxs-lookup"><span data-stu-id="dd66a-225">Toshiba Lifestyle Products & Services Corporation</span></span>
* <span data-ttu-id="dd66a-226">Univerzální Corporation média /Slovakia/ s.r.o.</span><span class="sxs-lookup"><span data-stu-id="dd66a-226">Universal Media Corporation /Slovakia/ s.r.o.</span></span>
* <span data-ttu-id="dd66a-227">VIZIO, Inc.</span><span class="sxs-lookup"><span data-stu-id="dd66a-227">VIZIO, Inc.</span></span>
* <span data-ttu-id="dd66a-228">Wistron Corporation</span><span class="sxs-lookup"><span data-stu-id="dd66a-228">Wistron Corporation</span></span>
* <span data-ttu-id="dd66a-229">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="dd66a-229">ZTE Corporation</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="dd66a-230">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="dd66a-230">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="dd66a-231">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="dd66a-231">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

