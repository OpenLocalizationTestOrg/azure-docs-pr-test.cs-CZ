---
title: "aaaLicensing Microsoft® technologie Smooth Streaming klienta portování Kit"
description: "Informace o tom, jak toolicensing hello Microsoft® technologie Smooth Streaming klienta portování Kit."
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
ms.openlocfilehash: 56c3dccda73dd02207bb4dbe8109ba6fda917a6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a><span data-ttu-id="79950-103">Licencování Smooth streamování klienta Microsoft® portování Kit</span><span class="sxs-lookup"><span data-stu-id="79950-103">Licensing Microsoft® Smooth Streaming Client Porting Kit</span></span>
## <a name="overview"></a><span data-ttu-id="79950-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="79950-104">Overview</span></span>
<span data-ttu-id="79950-105">Microsoft technologie Smooth Streaming klienta portování Kit (**SSPK** pro zkrácení) je technologie Smooth Streaming implementace klienta, která je optimalizovaná toohelp vložených výrobci zařízení, kabel a mobilní operátory, poskytovatelé přístupu k obsahu, telefonního sluchátka výrobců, nezávislí dodavatelé softwaru (ISV) a řešení zprostředkovatelé toocreate produktů a služeb pro streamování s adaptivní streamování obsah ve formátu technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="79950-105">Microsoft Smooth Streaming Client Porting Kit (**SSPK** for short) is a Smooth Streaming client implementation that is optimized toohelp embedded device manufacturers, cable and mobile operators, content service providers, handset manufacturers, independent software vendors (ISVs), and solution providers toocreate products and services for streaming adaptive streaming content in Smooth Streaming format.</span></span> <span data-ttu-id="79950-106">SSPK je na zařízení a platformy nezávislé implementace technologie Smooth Streaming klienta, který může přenést hello držitel licence tooany zařízení a platformy.</span><span class="sxs-lookup"><span data-stu-id="79950-106">SSPK is a device and platform independent implementation of Smooth Streaming client that can be ported by hello licensee tooany device and platform.</span></span> 

<span data-ttu-id="79950-107">Uvedeném níž je nejvyšší úrovni architektura a pole IIS technologie Smooth Streaming portování Kit je implementace klienta funkce Smooth Streaming hello od společnosti Microsoft a obsahuje všechny hello základní logiku pro přehrávání obsahu, technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="79950-107">Included below is a high level architecture and IIS Smooth Streaming Porting Kit box is hello Smooth Streaming Client implementation provided by Microsoft and includes all hello core logic for playback of Smooth Streaming content.</span></span> <span data-ttu-id="79950-108">To je poté přenést partnery pro určité zařízení nebo platformu implementací příslušná rozhraní.</span><span class="sxs-lookup"><span data-stu-id="79950-108">This is then ported by partners for a specific device or platform by implementing appropriate interfaces.</span></span> 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a><span data-ttu-id="79950-110">Popis</span><span class="sxs-lookup"><span data-stu-id="79950-110">Description</span></span>
<span data-ttu-id="79950-111">SSPK je licencovaná podle podmínek, které nabízí vynikající obchodní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="79950-111">SSPK is licensed on terms that offer excellent business value.</span></span> <span data-ttu-id="79950-112">Licence SSPK poskytuje oborový hello s:</span><span class="sxs-lookup"><span data-stu-id="79950-112">SSPK license provides hello industry with:</span></span>

* <span data-ttu-id="79950-113">Technologie Smooth Streaming portování Kit zdroje v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="79950-113">Smooth Streaming Porting Kit source in C++</span></span> 
  * <span data-ttu-id="79950-114">implementuje funkce klienta funkce Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="79950-114">implements Smooth Streaming Client functionality</span></span>
  * <span data-ttu-id="79950-115">Přidá formátu analýzy heuristiky, ukládání do vyrovnávací paměti logiku atd.</span><span class="sxs-lookup"><span data-stu-id="79950-115">adds format parsing, heuristics, buffering logic, etc.</span></span>
* <span data-ttu-id="79950-116">Aplikace přehrávače rozhraní API</span><span class="sxs-lookup"><span data-stu-id="79950-116">Player application APIs</span></span> 
  * <span data-ttu-id="79950-117">programovací rozhraní pro interakci s aplikací media player</span><span class="sxs-lookup"><span data-stu-id="79950-117">programming interfaces for interaction with a media player application</span></span>
* <span data-ttu-id="79950-118">Platforma Abstraction Layer (PAL) rozhraní</span><span class="sxs-lookup"><span data-stu-id="79950-118">Platform Abstraction Layer (PAL) Interface</span></span> 
  * <span data-ttu-id="79950-119">programovací rozhraní pro interakci s hello operačního systému (vláken, sockets)</span><span class="sxs-lookup"><span data-stu-id="79950-119">programming interfaces for interaction with hello operating system (threads, sockets)</span></span>
* <span data-ttu-id="79950-120">Hardware abstraktní vrstvu HAL rozhraní</span><span class="sxs-lookup"><span data-stu-id="79950-120">Hardware Abstraction Layer (HAL) Interface</span></span> 
  * <span data-ttu-id="79950-121">programovací rozhraní pro interakci s hardwarem A / dekodérů V (dekódování, vykreslování)</span><span class="sxs-lookup"><span data-stu-id="79950-121">programming interfaces for interaction with hardware A/V decoders (decoding, rendering)</span></span>
* <span data-ttu-id="79950-122">Rozhraní digitální Rights Management (DRM)</span><span class="sxs-lookup"><span data-stu-id="79950-122">Digital Rights Management (DRM) Interface</span></span> 
  * <span data-ttu-id="79950-123">programovací rozhraní pro zpracování DRM prostřednictvím hello DRM Abstraction Layer (DAL)</span><span class="sxs-lookup"><span data-stu-id="79950-123">programming interfaces for handling DRM through hello DRM Abstraction Layer (DAL)</span></span>
  * <span data-ttu-id="79950-124">Microsoft PlayReady portování Kit dodává samostatně, ale integruje prostřednictvím tohoto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="79950-124">Microsoft PlayReady Porting Kit ships separately but integrates through this interface.</span></span> <span data-ttu-id="79950-125">Další informace o licencování Microsoft PlayReady Device, klikněte na tlačítko [zde](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span><span class="sxs-lookup"><span data-stu-id="79950-125">For more details on Microsoft PlayReady Device licensing, click [here](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span></span>
* <span data-ttu-id="79950-126">Implementace ukázky</span><span class="sxs-lookup"><span data-stu-id="79950-126">Implementation samples</span></span> 
  * <span data-ttu-id="79950-127">Ukázka PAL implementace pro Linux</span><span class="sxs-lookup"><span data-stu-id="79950-127">sample PAL implementation for Linux</span></span>
  * <span data-ttu-id="79950-128">Ukázka HAL implementace pro GStreamer</span><span class="sxs-lookup"><span data-stu-id="79950-128">sample HAL implementation for GStreamer</span></span>

## <a name="licensing-options"></a><span data-ttu-id="79950-129">Možnosti licencování</span><span class="sxs-lookup"><span data-stu-id="79950-129">Licensing Options</span></span>
<span data-ttu-id="79950-130">Microsoft technologie Smooth Streaming klienta portování Kit je k dispozici toolicensees provedené v dvě odlišné licenční smlouvy: jeden pro vývoj technologie Smooth Streaming produkty klienta provizorní a druhý pro distribuci technologie Smooth Streaming klienta konečné produkty tooend uživatele.</span><span class="sxs-lookup"><span data-stu-id="79950-130">Microsoft Smooth Streaming Client Porting Kit is made available toolicensees under two distinct license agreements: one for developing Smooth Streaming Client Interim Products and another for distributing Smooth Streaming Client Final Products tooend users.</span></span>

* <span data-ttu-id="79950-131">Pro čipovou sadou výrobců, integrátory nebo nezávislé výrobce softwaru (ISV) požadující zdrojový kód – portování kit toodevelop provizorní produkty Microsoft technologie Smooth Streaming klienta portování sady **licenci produktu provizorní** by měl být spuštěn.</span><span class="sxs-lookup"><span data-stu-id="79950-131">For chipset manufacturers, system integrators, or independent software vendors (ISVs) who require a source code porting kit toodevelop Interim Products, a Microsoft Smooth Streaming Client Porting Kit **Interim Product License** should be executed.</span></span>
* <span data-ttu-id="79950-132">Pro výrobce zařízení nebo nezávislí výrobci softwaru, kteří potřebují distribuční oprávnění pro uživatele, technologie Smooth Streaming klienta konečné produkty tooend, hello Microsoft technologie Smooth Streaming klienta portování Kit **konečné licenci produktu** by měl být spuštěn.</span><span class="sxs-lookup"><span data-stu-id="79950-132">For device manufacturers or ISVs who require distribution rights for Smooth Streaming Client Final Products tooend users, hello Microsoft Smooth Streaming Client Porting Kit **Final Product License** should be executed.</span></span>

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a><span data-ttu-id="79950-133">Microsoft klienta funkce Smooth Streaming portování licenci produktu provizorní Kit</span><span class="sxs-lookup"><span data-stu-id="79950-133">Microsoft Smooth Streaming Client Porting Kit Interim Product License</span></span>
<span data-ttu-id="79950-134">V rámci této licence Microsoft nabízí technologie Smooth Streaming klienta portování sadu pro a hello toodevelop práva potřebná duševní vlastnictví a distribuovat technologie Smooth Streaming produkty klienta provizorní tooother technologie Smooth Streaming klienta portování Kit zařízení držitele licence, Distribuujte technologie Smooth Streaming klienta konečné produkty.</span><span class="sxs-lookup"><span data-stu-id="79950-134">Under this license, Microsoft offers a Smooth Streaming Client Porting Kit and hello necessary intellectual property rights toodevelop and distribute Smooth Streaming Client Interim Products tooother Smooth Streaming Client Porting Kit device licensees that distribute Smooth Streaming Client Final Products.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="79950-135">Struktura poplatků</span><span class="sxs-lookup"><span data-stu-id="79950-135">Fee structure</span></span>
<span data-ttu-id="79950-136">Jednorázové licenční poplatek 50 000 USA Kč poskytuje přístup toohello technologie Smooth Streaming klienta portování Kit.</span><span class="sxs-lookup"><span data-stu-id="79950-136">A U.S. $50,000 one-time license fee provides access toohello Smooth Streaming Client Porting Kit.</span></span> 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a><span data-ttu-id="79950-137">Microsoft klienta funkce Smooth Streaming portování licenci produktu konečné Kit</span><span class="sxs-lookup"><span data-stu-id="79950-137">Microsoft Smooth Streaming Client Porting Kit Final Product License</span></span>
<span data-ttu-id="79950-138">Pod tuto licenci společnost Microsoft nabízí všechny potřebné duševní vlastnictví práva tooreceive technologie Smooth Streaming produkty klienta provizorní z jiných držitele licence technologie Smooth Streaming klienta portování Kit a toodistribute značky společnosti technologie Smooth Streaming klienta konečné Uživatelé tooend produkty.</span><span class="sxs-lookup"><span data-stu-id="79950-138">Under this license, Microsoft offers all necessary intellectual property rights tooreceive Smooth Streaming Client Interim Products from other Smooth Streaming Client Porting Kit licensees and toodistribute company-branded Smooth Streaming Client Final Products tooend users.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="79950-139">Struktura poplatků</span><span class="sxs-lookup"><span data-stu-id="79950-139">Fee structure</span></span>
<span data-ttu-id="79950-140">Hello technologie Smooth Streaming konečné produkt Klient se nabízí v rámci modelu Licencovaní jako v části:</span><span class="sxs-lookup"><span data-stu-id="79950-140">hello Smooth Streaming Client Final Product is offered under a royalty model as under:</span></span>

* <span data-ttu-id="79950-141">dodaný 0.10 za implementaci zařízení</span><span class="sxs-lookup"><span data-stu-id="79950-141">$0.10 per device implementation shipped</span></span>
* <span data-ttu-id="79950-142">Licencovaní Hello je omezená na 50 000 $ každý rok</span><span class="sxs-lookup"><span data-stu-id="79950-142">hello royalty is capped at $50,000 each year</span></span>
* <span data-ttu-id="79950-143">Žádné licencované pro prvních 10 000 zařízení implementace každý rok</span><span class="sxs-lookup"><span data-stu-id="79950-143">No royalty for first 10,000 device implementations each year</span></span> 

## <a name="licensing-procedure-and-sspk-access"></a><span data-ttu-id="79950-144">Licencování postupu a SSPK přístup</span><span class="sxs-lookup"><span data-stu-id="79950-144">Licensing Procedure and SSPK access</span></span>
<span data-ttu-id="79950-145">E-mailem [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) pro všechny licencování dotazy.</span><span class="sxs-lookup"><span data-stu-id="79950-145">Please email [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) for all licensing queries.</span></span>

<span data-ttu-id="79950-146">Hello [SSPK distribuční portál](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) je přístupný tooregistered provizorní držitele licence.</span><span class="sxs-lookup"><span data-stu-id="79950-146">hello [SSPK Distribution portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) is accessible tooregistered Interim licensees.</span></span>

<span data-ttu-id="79950-147">Provizorní a finální SSPK držitele licence můžete odeslat technické dotazy příliš[smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="79950-147">Interim and Final SSPK licensees can submit technical questions too[smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).</span></span>

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a><span data-ttu-id="79950-148">Microsoft funkce Smooth Streaming držitele licence smlouvy dočasné produktu klienta</span><span class="sxs-lookup"><span data-stu-id="79950-148">Microsoft Smooth Streaming Client Interim Product Agreement Licensees</span></span>
* <span data-ttu-id="79950-149">Adroit obchodní řešení, Inc</span><span class="sxs-lookup"><span data-stu-id="79950-149">Adroit Business Solutions, Inc</span></span>
* <span data-ttu-id="79950-150">Pokročilé SA digitální všesměrové vysílání</span><span class="sxs-lookup"><span data-stu-id="79950-150">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="79950-151">AirTies Kablosuz Iletism Sanayive zprávy Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="79950-151">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="79950-152">Albis technologie Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-152">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="79950-153">Alticast Corporation</span><span class="sxs-lookup"><span data-stu-id="79950-153">Alticast Corporation</span></span>
* <span data-ttu-id="79950-154">Digitální Amazon služby, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-154">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="79950-155">Technologie Arion, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-155">Arion Technology, Inc.</span></span>
* <span data-ttu-id="79950-156">Software multimédia AVC Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-156">AVC Multimedia Software Co., Ltd.</span></span>
* <span data-ttu-id="79950-157">Cavium, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-157">Cavium, Inc.</span></span>
* <span data-ttu-id="79950-158">Zakoupení Corporation EchoStar</span><span class="sxs-lookup"><span data-stu-id="79950-158">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="79950-159">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-159">Enseo, Inc.</span></span>
* <span data-ttu-id="79950-160">Fluendo S.A.</span><span class="sxs-lookup"><span data-stu-id="79950-160">Fluendo S.A.</span></span>
* <span data-ttu-id="79950-161">HANDAN BroadInfoCom Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-161">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="79950-162">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="79950-162">Infomir GMBH</span></span>
* <span data-ttu-id="79950-163">Irdeto USA Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-163">Irdeto USA Inc.</span></span>
* <span data-ttu-id="79950-164">iWEDIA S.A.</span><span class="sxs-lookup"><span data-stu-id="79950-164">iWEDIA S.A.</span></span> 
* <span data-ttu-id="79950-165">Globální služby BV Liberty</span><span class="sxs-lookup"><span data-stu-id="79950-165">Liberty Global Services BV</span></span>
* <span data-ttu-id="79950-166">MediaTek Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-166">MediaTek Inc.</span></span>
* <span data-ttu-id="79950-167">MStar Co, Ltd</span><span class="sxs-lookup"><span data-stu-id="79950-167">MStar Co, Ltd</span></span>
* <span data-ttu-id="79950-168">NINTENDO Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-168">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="79950-169">OpenTV, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-169">OpenTV, Inc.</span></span>
* <span data-ttu-id="79950-170">Omezené digitální šafrán</span><span class="sxs-lookup"><span data-stu-id="79950-170">Saffron Digital Limited</span></span>
* <span data-ttu-id="79950-171">Elektrický Changhong Sichuan Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="79950-171">Sichuan Changhong Electric Co., Ltd</span></span>
* <span data-ttu-id="79950-172">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="79950-172">SoftAtHome</span></span>
* <span data-ttu-id="79950-173">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="79950-173">Sony Corporation</span></span>
* <span data-ttu-id="79950-174">Tatung Technology, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-174">Tatung Technology Inc.</span></span>
* <span data-ttu-id="79950-175">TCL Technoly Electronics (Huizhou) Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-175">TCL Technoly Electronics (Huizhou) Co., Ltd.</span></span>
* <span data-ttu-id="79950-176">Horní vítězství investice, Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-176">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="79950-177">Vestel Elektronik Sanayi sunout Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="79950-177">Vestel Elektronik Sanayi ve Ticaret A.S.</span></span>
* <span data-ttu-id="79950-178">VisualOn, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-178">VisualOn, Inc.</span></span>
* <span data-ttu-id="79950-179">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="79950-179">ZTE Corporation</span></span>

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a><span data-ttu-id="79950-180">Microsoft funkce Smooth Streaming držitele licence smlouvu konečného produktu klienta</span><span class="sxs-lookup"><span data-stu-id="79950-180">Microsoft Smooth Streaming Client Final Product Agreement Licensees</span></span>
* <span data-ttu-id="79950-181">Pokročilé SA digitální všesměrové vysílání</span><span class="sxs-lookup"><span data-stu-id="79950-181">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="79950-182">AirTies Kablosuz Iletism Sanayive zprávy Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="79950-182">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="79950-183">Albis technologie Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-183">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="79950-184">Digitální Amazon služby, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-184">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="79950-185">Technologie AmTRAN Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-185">AmTRAN Technology Co., Ltd.</span></span>
* <span data-ttu-id="79950-186">Arcadyan technologie Corporation</span><span class="sxs-lookup"><span data-stu-id="79950-186">Arcadyan Technology Corporation</span></span>
* <span data-ttu-id="79950-187">Technologie Arion, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-187">Arion Technology, Inc.</span></span>
* <span data-ttu-id="79950-188">SÍŤ SAN ELEKTRONİK ATMACA.</span><span class="sxs-lookup"><span data-stu-id="79950-188">ATMACA ELEKTRONİK SAN.</span></span> <span data-ttu-id="79950-189">JSTE TİC.</span><span class="sxs-lookup"><span data-stu-id="79950-189">VE TİC.</span></span> <span data-ttu-id="79950-190">A.Ş</span><span class="sxs-lookup"><span data-stu-id="79950-190">A.Ş</span></span>
* <span data-ttu-id="79950-191">Britské Sky všesměrové vysílání omezené</span><span class="sxs-lookup"><span data-stu-id="79950-191">British Sky Broadcasting Limited</span></span>
* <span data-ttu-id="79950-192">CastPal technologie Inc., Shenzhen</span><span class="sxs-lookup"><span data-stu-id="79950-192">CastPal Technology Inc., Shenzhen</span></span>
* <span data-ttu-id="79950-193">Compal Electronics, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-193">Compal Electronics, Inc.</span></span>
* <span data-ttu-id="79950-194">Digitální Dongguan AV technologie Corp., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-194">Dongguan Digital AV Technology Corp., Ltd.</span></span>
* <span data-ttu-id="79950-195">Zakoupení Corporation EchoStar</span><span class="sxs-lookup"><span data-stu-id="79950-195">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="79950-196">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-196">Enseo, Inc.</span></span>
* <span data-ttu-id="79950-197">Omezené filmy Filmflex</span><span class="sxs-lookup"><span data-stu-id="79950-197">Filmflex Movies Limited</span></span>
* <span data-ttu-id="79950-198">Fluendo S.A.</span><span class="sxs-lookup"><span data-stu-id="79950-198">Fluendo S.A.</span></span>
* <span data-ttu-id="79950-199">Omezené inovace Gibson</span><span class="sxs-lookup"><span data-stu-id="79950-199">Gibson Innovations Limited</span></span>
* <span data-ttu-id="79950-200">Informace o Applicantion Haier S.R.L</span><span class="sxs-lookup"><span data-stu-id="79950-200">Haier Information Applicantion S.R.L</span></span>
* <span data-ttu-id="79950-201">HANDAN BroadInfoCom Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-201">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="79950-202">Mezinárodní Hisense Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-202">Hisense International Co., Ltd.</span></span> 
* <span data-ttu-id="79950-203">Homecast Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="79950-203">Homecast Co.,Ltd</span></span>
* <span data-ttu-id="79950-204">Svazek Hai přesnost odvětví Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-204">Hon Hai Precision Industry Co., Ltd.</span></span>
* <span data-ttu-id="79950-205">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="79950-205">Infomir GMBH</span></span>
* <span data-ttu-id="79950-206">Kaonmedia Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-206">Kaonmedia Co., Ltd.</span></span>
* <span data-ttu-id="79950-207">KDDI Corporation</span><span class="sxs-lookup"><span data-stu-id="79950-207">KDDI Corporation</span></span>
* <span data-ttu-id="79950-208">NINTENDO Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-208">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="79950-209">SA oranžová</span><span class="sxs-lookup"><span data-stu-id="79950-209">Orange SA</span></span>
* <span data-ttu-id="79950-210">Omezené digitální šafrán</span><span class="sxs-lookup"><span data-stu-id="79950-210">Saffron Digital Limited</span></span>
* <span data-ttu-id="79950-211">Širokopásmové připojení Sagemcom SAS</span><span class="sxs-lookup"><span data-stu-id="79950-211">Sagemcom Broadband SAS</span></span>
* <span data-ttu-id="79950-212">Shenzhen Coship Electronics CO., LTD</span><span class="sxs-lookup"><span data-stu-id="79950-212">Shenzhen Coship Electronics CO., LTD</span></span>
* <span data-ttu-id="79950-213">Elektrický Jiuzhou Shenzhen Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="79950-213">Shenzhen Jiuzhou Electric Co.,Ltd</span></span>
* <span data-ttu-id="79950-214">Shenzhen Skyworth digitální technologie Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="79950-214">Shenzhen Skyworth Digital Technology Co., Ltd</span></span>
* <span data-ttu-id="79950-215">Elektrický Changhong Sichuan Co., Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-215">Sichuan Changhong Electric Co., Ltd.</span></span>
* <span data-ttu-id="79950-216">Skardin průmyslové Corp.</span><span class="sxs-lookup"><span data-stu-id="79950-216">Skardin Industrial Corp.</span></span>
* <span data-ttu-id="79950-217">SKY Německo Fernsehen GmbH & Co. KG</span><span class="sxs-lookup"><span data-stu-id="79950-217">Sky Deutschland Fernsehen GmbH & Co. KG</span></span>
* <span data-ttu-id="79950-218">SmarDTV S.A.</span><span class="sxs-lookup"><span data-stu-id="79950-218">SmarDTV S.A.</span></span>
* <span data-ttu-id="79950-219">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="79950-219">SoftAtHome</span></span>
* <span data-ttu-id="79950-220">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="79950-220">Sony Corporation</span></span>
* <span data-ttu-id="79950-221">Omezené TCL zámořské Marketing (moři komerční zboží)</span><span class="sxs-lookup"><span data-stu-id="79950-221">TCL Overseas Marketing (Macao Commercial Offshore) Limited</span></span>
* <span data-ttu-id="79950-222">Technicolor doručení technologie SAS</span><span class="sxs-lookup"><span data-stu-id="79950-222">Technicolor Delivery Technologies, SAS</span></span>
* <span data-ttu-id="79950-223">Tongfang globální Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-223">Tongfang Global Ltd.</span></span>
* <span data-ttu-id="79950-224">Horní vítězství investice, Ltd.</span><span class="sxs-lookup"><span data-stu-id="79950-224">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="79950-225">Produkty Toshiba Lifestyle & Corporation služby</span><span class="sxs-lookup"><span data-stu-id="79950-225">Toshiba Lifestyle Products & Services Corporation</span></span>
* <span data-ttu-id="79950-226">Univerzální Corporation média /Slovakia/ s.r.o.</span><span class="sxs-lookup"><span data-stu-id="79950-226">Universal Media Corporation /Slovakia/ s.r.o.</span></span>
* <span data-ttu-id="79950-227">VIZIO, Inc.</span><span class="sxs-lookup"><span data-stu-id="79950-227">VIZIO, Inc.</span></span>
* <span data-ttu-id="79950-228">Wistron Corporation</span><span class="sxs-lookup"><span data-stu-id="79950-228">Wistron Corporation</span></span>
* <span data-ttu-id="79950-229">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="79950-229">ZTE Corporation</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="79950-230">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="79950-230">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="79950-231">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="79950-231">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

