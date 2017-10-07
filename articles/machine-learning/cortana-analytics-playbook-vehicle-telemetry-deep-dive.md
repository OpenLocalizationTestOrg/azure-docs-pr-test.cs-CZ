---
title: "podrobně aaaDeep předpovědi vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti hello Cortana Intelligence toogain v reálném čase a prediktivní Statistika na vehicle stavu a řídí zvyklosti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: ba1448a5081762292561f904d9ec54617c9a5330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-hello-solution"></a><span data-ttu-id="46456-103">Vehicle telemetrie analytics řešení playbook: podrobné informace do řešení hello</span><span class="sxs-lookup"><span data-stu-id="46456-103">Vehicle telemetry analytics solution playbook: deep dive into hello solution</span></span>
<span data-ttu-id="46456-104">To **nabídky** odkazy toohello části této playbook:</span><span class="sxs-lookup"><span data-stu-id="46456-104">This **menu** links toohello sections of this playbook:</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="46456-105">Tato část podrobněji zanalyzuje data do v jednotlivých fázích hello znázorněný v hello architektury řešení s pokyny a ukazatele pro přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="46456-105">This section drills down into each of hello stages depicted in hello Solution Architecture with instructions and pointers for customization.</span></span> 

## <a name="data-sources"></a><span data-ttu-id="46456-106">Zdroje dat</span><span class="sxs-lookup"><span data-stu-id="46456-106">Data Sources</span></span>
<span data-ttu-id="46456-107">řešení Hello používá dvou různých zdrojů dat.:</span><span class="sxs-lookup"><span data-stu-id="46456-107">hello solution uses two different data sources:</span></span>

* <span data-ttu-id="46456-108">**simulated vehicle signály a diagnostiky datovou sadu** a</span><span class="sxs-lookup"><span data-stu-id="46456-108">**simulated vehicle signals and diagnostic dataset** and</span></span> 
* <span data-ttu-id="46456-109">**vehicle katalogu**</span><span class="sxs-lookup"><span data-stu-id="46456-109">**vehicle catalog**</span></span>

<span data-ttu-id="46456-110">Vehicle telematika jsme je součástí tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="46456-110">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="46456-111">Vysílá diagnostické informace a signály odpovídající toohello stav hello vehicle a toohello řídí vzor k danému bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="46456-111">It emits diagnostic information and signals corresponding toohello state of hello vehicle and toohello driving pattern at a given point in time.</span></span> <span data-ttu-id="46456-112">Klikněte na tlačítko [Vehicle telematika simulátoru](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle telematika simulátoru Visual Studio řešení** přizpůsobení podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="46456-112">Click [Vehicle Telematics Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle Telematics Simulator Visual Studio Solution** for customizations based on your requirements.</span></span> <span data-ttu-id="46456-113">Hello vehicle katalogu obsahuje odkaz na datovou sadu s VIN toomodel mapování.</span><span class="sxs-lookup"><span data-stu-id="46456-113">hello vehicle catalog contains a reference dataset with a VIN toomodel mapping.</span></span>

![Vehicle telematika simulátoru](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

<span data-ttu-id="46456-115">*Obrázek 1 – Vehicle telematika simulátoru*</span><span class="sxs-lookup"><span data-stu-id="46456-115">*Figure 1 – Vehicle Telematics Simulator*</span></span>

<span data-ttu-id="46456-116">Toto je datovou sadu formátu JSON, který obsahuje následující schéma hello.</span><span class="sxs-lookup"><span data-stu-id="46456-116">This is a JSON-formatted dataset that contains hello following schema.</span></span>

| <span data-ttu-id="46456-117">Sloupec</span><span class="sxs-lookup"><span data-stu-id="46456-117">Column</span></span> | <span data-ttu-id="46456-118">Popis</span><span class="sxs-lookup"><span data-stu-id="46456-118">Description</span></span> | <span data-ttu-id="46456-119">Hodnoty</span><span class="sxs-lookup"><span data-stu-id="46456-119">Values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="46456-120">VIN</span><span class="sxs-lookup"><span data-stu-id="46456-120">VIN</span></span> |<span data-ttu-id="46456-121">Náhodně generované identifikační číslo</span><span class="sxs-lookup"><span data-stu-id="46456-121">Randomly generated Vehicle Identification Number</span></span> |<span data-ttu-id="46456-122">To se získávají z hlavní seznam 10 000 náhodně generované vehicle identifikačními čísly.</span><span class="sxs-lookup"><span data-stu-id="46456-122">This is obtained from a master list of 10,000 randomly generated vehicle identification numbers.</span></span> |
| <span data-ttu-id="46456-123">Mimo teploty</span><span class="sxs-lookup"><span data-stu-id="46456-123">Outside temperature</span></span> |<span data-ttu-id="46456-124">Hello mimo teploty, které řídí hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-124">hello outside temperature where hello vehicle is driving</span></span> |<span data-ttu-id="46456-125">Náhodně generované číslo od 0 – 100</span><span class="sxs-lookup"><span data-stu-id="46456-125">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="46456-126">Modul teploty</span><span class="sxs-lookup"><span data-stu-id="46456-126">Engine temperature</span></span> |<span data-ttu-id="46456-127">modul teplota Hello hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-127">hello engine temperature of hello vehicle</span></span> |<span data-ttu-id="46456-128">Náhodně generované číslo od 0-500</span><span class="sxs-lookup"><span data-stu-id="46456-128">Randomly generated number from 0-500</span></span> |
| <span data-ttu-id="46456-129">Rychlost</span><span class="sxs-lookup"><span data-stu-id="46456-129">Speed</span></span> |<span data-ttu-id="46456-130">rychlost Hello modul, na které hello vehicle řídí</span><span class="sxs-lookup"><span data-stu-id="46456-130">hello engine speed at which hello vehicle is driving</span></span> |<span data-ttu-id="46456-131">Náhodně generované číslo od 0 – 100</span><span class="sxs-lookup"><span data-stu-id="46456-131">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="46456-132">Paliva</span><span class="sxs-lookup"><span data-stu-id="46456-132">Fuel</span></span> |<span data-ttu-id="46456-133">úroveň paliva Hello hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-133">hello fuel level of hello vehicle</span></span> |<span data-ttu-id="46456-134">Náhodně generované číslo od 0 až 100 (určuje procento úrovně paliva)</span><span class="sxs-lookup"><span data-stu-id="46456-134">Randomly generated number from 0-100 (indicates fuel level percentage)</span></span> |
| <span data-ttu-id="46456-135">EngineOil</span><span class="sxs-lookup"><span data-stu-id="46456-135">EngineOil</span></span> |<span data-ttu-id="46456-136">Hello modul těžba ropy úroveň hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-136">hello engine oil level of hello vehicle</span></span> |<span data-ttu-id="46456-137">Náhodně generované číslo od 0 až 100 (určuje procento úrovně těžba ropy modul)</span><span class="sxs-lookup"><span data-stu-id="46456-137">Randomly generated number from 0-100 (indicates engine oil level percentage)</span></span> |
| <span data-ttu-id="46456-138">Můžete zadat přetížení</span><span class="sxs-lookup"><span data-stu-id="46456-138">Tire pressure</span></span> |<span data-ttu-id="46456-139">můžete zadat tlak Hello hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-139">hello tire pressure of hello vehicle</span></span> |<span data-ttu-id="46456-140">Náhodně generované číslo od 0 – 50 (určuje procento úrovně zatížení můžete zadat)</span><span class="sxs-lookup"><span data-stu-id="46456-140">Randomly generated number from 0-50 (indicates tire pressure level percentage)</span></span> |
| <span data-ttu-id="46456-141">Počítadlo kilometrů</span><span class="sxs-lookup"><span data-stu-id="46456-141">Odometer</span></span> |<span data-ttu-id="46456-142">čtení počítadlo kilometrů Hello vozidel hello</span><span class="sxs-lookup"><span data-stu-id="46456-142">hello odometer reading of hello vehicle</span></span> |<span data-ttu-id="46456-143">Náhodně generované číslo od 0 200000</span><span class="sxs-lookup"><span data-stu-id="46456-143">Randomly generated number from 0-200000</span></span> |
| <span data-ttu-id="46456-144">Accelerator_pedal_position</span><span class="sxs-lookup"><span data-stu-id="46456-144">Accelerator_pedal_position</span></span> |<span data-ttu-id="46456-145">Hello akcelerátoru Pedálové pozici hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-145">hello accelerator pedal position of hello vehicle</span></span> |<span data-ttu-id="46456-146">Náhodně generované číslo od 0 až 100 (určuje procento úrovně akcelerátoru)</span><span class="sxs-lookup"><span data-stu-id="46456-146">Randomly generated number from 0-100 (indicates accelerator level percentage)</span></span> |
| <span data-ttu-id="46456-147">Parking_brake_status</span><span class="sxs-lookup"><span data-stu-id="46456-147">Parking_brake_status</span></span> |<span data-ttu-id="46456-148">Určuje, zda text hello vehicle zaparkováno nebo ne</span><span class="sxs-lookup"><span data-stu-id="46456-148">Indicates whether hello vehicle is parked or not</span></span> |<span data-ttu-id="46456-149">True nebo False</span><span class="sxs-lookup"><span data-stu-id="46456-149">True or False</span></span> |
| <span data-ttu-id="46456-150">Headlamp_status</span><span class="sxs-lookup"><span data-stu-id="46456-150">Headlamp_status</span></span> |<span data-ttu-id="46456-151">Určuje, kde je hello světlometu na nebo ne</span><span class="sxs-lookup"><span data-stu-id="46456-151">Indicates where hello headlamp is on or not</span></span> |<span data-ttu-id="46456-152">True nebo False</span><span class="sxs-lookup"><span data-stu-id="46456-152">True or False</span></span> |
| <span data-ttu-id="46456-153">Brake_pedal_status</span><span class="sxs-lookup"><span data-stu-id="46456-153">Brake_pedal_status</span></span> |<span data-ttu-id="46456-154">Určuje, zda text hello brzdového pedálu stisknutí nebo ne</span><span class="sxs-lookup"><span data-stu-id="46456-154">Indicates whether hello brake pedal is pressed or not</span></span> |<span data-ttu-id="46456-155">True nebo False</span><span class="sxs-lookup"><span data-stu-id="46456-155">True or False</span></span> |
| <span data-ttu-id="46456-156">Transmission_gear_position</span><span class="sxs-lookup"><span data-stu-id="46456-156">Transmission_gear_position</span></span> |<span data-ttu-id="46456-157">Hello přenosu ozubené kolečko pozici hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-157">hello transmission gear position of hello vehicle</span></span> |<span data-ttu-id="46456-158">Stavy: první, druhá, třetí a čtvrté, páté, šestého, sedmá, osmého</span><span class="sxs-lookup"><span data-stu-id="46456-158">States: first, second, third, fourth, fifth, sixth, seventh, eighth</span></span> |
| <span data-ttu-id="46456-159">Ignition_status</span><span class="sxs-lookup"><span data-stu-id="46456-159">Ignition_status</span></span> |<span data-ttu-id="46456-160">Určuje, zda je spuštěná nebo zastavená hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-160">Indicates whether hello vehicle is running or stopped</span></span> |<span data-ttu-id="46456-161">True nebo False</span><span class="sxs-lookup"><span data-stu-id="46456-161">True or False</span></span> |
| <span data-ttu-id="46456-162">Windshield_wiper_status</span><span class="sxs-lookup"><span data-stu-id="46456-162">Windshield_wiper_status</span></span> |<span data-ttu-id="46456-163">Určuje, zda stírání čelního skla hello zapnutý, nebo ne</span><span class="sxs-lookup"><span data-stu-id="46456-163">Indicates whether hello windshield wiper is turned or not</span></span> |<span data-ttu-id="46456-164">True nebo False</span><span class="sxs-lookup"><span data-stu-id="46456-164">True or False</span></span> |
| <span data-ttu-id="46456-165">ABS</span><span class="sxs-lookup"><span data-stu-id="46456-165">ABS</span></span> |<span data-ttu-id="46456-166">Určuje, zda je nebo není zařazen ABS</span><span class="sxs-lookup"><span data-stu-id="46456-166">Indicates whether ABS is engaged or not</span></span> |<span data-ttu-id="46456-167">True nebo False</span><span class="sxs-lookup"><span data-stu-id="46456-167">True or False</span></span> |
| <span data-ttu-id="46456-168">časové razítko</span><span class="sxs-lookup"><span data-stu-id="46456-168">Timestamp</span></span> |<span data-ttu-id="46456-169">časové razítko Hello při vytvoření hello datového bodu</span><span class="sxs-lookup"><span data-stu-id="46456-169">hello timestamp when hello data point is created</span></span> |<span data-ttu-id="46456-170">Datum</span><span class="sxs-lookup"><span data-stu-id="46456-170">Date</span></span> |
| <span data-ttu-id="46456-171">Město</span><span class="sxs-lookup"><span data-stu-id="46456-171">City</span></span> |<span data-ttu-id="46456-172">umístění Hello hello vehicle</span><span class="sxs-lookup"><span data-stu-id="46456-172">hello location of hello vehicle</span></span> |<span data-ttu-id="46456-173">4 města v tomto řešení: Bellevue, Redmond, Sammamish, Praha</span><span class="sxs-lookup"><span data-stu-id="46456-173">4 cities in this solution: Bellevue, Redmond, Sammamish, Seattle</span></span> |

<span data-ttu-id="46456-174">Hello vehicle modelu referenční datová sada obsahuje VIN toohello modelu mapování.</span><span class="sxs-lookup"><span data-stu-id="46456-174">hello vehicle model reference dataset contains VIN toohello model mapping.</span></span> 

| <span data-ttu-id="46456-175">VIN</span><span class="sxs-lookup"><span data-stu-id="46456-175">VIN</span></span> | <span data-ttu-id="46456-176">Model</span><span class="sxs-lookup"><span data-stu-id="46456-176">Model</span></span> |
| --- | --- |
| <span data-ttu-id="46456-177">FHL3O1SA4IEHB4WU1</span><span class="sxs-lookup"><span data-stu-id="46456-177">FHL3O1SA4IEHB4WU1</span></span> |<span data-ttu-id="46456-178">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="46456-178">Sedan</span></span> |
| <span data-ttu-id="46456-179">8J0U8XCPRGW4Z3NQE</span><span class="sxs-lookup"><span data-stu-id="46456-179">8J0U8XCPRGW4Z3NQE</span></span> |<span data-ttu-id="46456-180">Hybridní</span><span class="sxs-lookup"><span data-stu-id="46456-180">Hybrid</span></span> |
| <span data-ttu-id="46456-181">WORG68Z2PLTNZDBI7</span><span class="sxs-lookup"><span data-stu-id="46456-181">WORG68Z2PLTNZDBI7</span></span> |<span data-ttu-id="46456-182">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="46456-182">Family Saloon</span></span> |
| <span data-ttu-id="46456-183">JTHMYHQTEPP4WBMRN</span><span class="sxs-lookup"><span data-stu-id="46456-183">JTHMYHQTEPP4WBMRN</span></span> |<span data-ttu-id="46456-184">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="46456-184">Sedan</span></span> |
| <span data-ttu-id="46456-185">W9FTHG27LZN1YWO0Y</span><span class="sxs-lookup"><span data-stu-id="46456-185">W9FTHG27LZN1YWO0Y</span></span> |<span data-ttu-id="46456-186">Hybridní</span><span class="sxs-lookup"><span data-stu-id="46456-186">Hybrid</span></span> |
| <span data-ttu-id="46456-187">MHTP9N792PHK08WJM</span><span class="sxs-lookup"><span data-stu-id="46456-187">MHTP9N792PHK08WJM</span></span> |<span data-ttu-id="46456-188">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="46456-188">Family Saloon</span></span> |
| <span data-ttu-id="46456-189">EI4QXI2AXVQQING4I</span><span class="sxs-lookup"><span data-stu-id="46456-189">EI4QXI2AXVQQING4I</span></span> |<span data-ttu-id="46456-190">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="46456-190">Sedan</span></span> |
| <span data-ttu-id="46456-191">5KKR2VB4WHQH97PF8</span><span class="sxs-lookup"><span data-stu-id="46456-191">5KKR2VB4WHQH97PF8</span></span> |<span data-ttu-id="46456-192">Hybridní</span><span class="sxs-lookup"><span data-stu-id="46456-192">Hybrid</span></span> |
| <span data-ttu-id="46456-193">W9NSZ423XZHAONYXB</span><span class="sxs-lookup"><span data-stu-id="46456-193">W9NSZ423XZHAONYXB</span></span> |<span data-ttu-id="46456-194">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="46456-194">Family Saloon</span></span> |
| <span data-ttu-id="46456-195">26WJSGHX4MA5ROHNL</span><span class="sxs-lookup"><span data-stu-id="46456-195">26WJSGHX4MA5ROHNL</span></span> |<span data-ttu-id="46456-196">Převoditelné</span><span class="sxs-lookup"><span data-stu-id="46456-196">Convertible</span></span> |
| <span data-ttu-id="46456-197">GHLUB6ONKMOSI7E77</span><span class="sxs-lookup"><span data-stu-id="46456-197">GHLUB6ONKMOSI7E77</span></span> |<span data-ttu-id="46456-198">Kombi</span><span class="sxs-lookup"><span data-stu-id="46456-198">Station Wagon</span></span> |
| <span data-ttu-id="46456-199">9C2RHVRVLMEJDBXLP</span><span class="sxs-lookup"><span data-stu-id="46456-199">9C2RHVRVLMEJDBXLP</span></span> |<span data-ttu-id="46456-200">Compact Car</span><span class="sxs-lookup"><span data-stu-id="46456-200">Compact Car</span></span> |
| <span data-ttu-id="46456-201">BRNHVMZOUJ6EOCP32</span><span class="sxs-lookup"><span data-stu-id="46456-201">BRNHVMZOUJ6EOCP32</span></span> |<span data-ttu-id="46456-202">Malé SUV</span><span class="sxs-lookup"><span data-stu-id="46456-202">Small SUV</span></span> |
| <span data-ttu-id="46456-203">VCYVW0WUZNBTM594J</span><span class="sxs-lookup"><span data-stu-id="46456-203">VCYVW0WUZNBTM594J</span></span> |<span data-ttu-id="46456-204">Auto sportu</span><span class="sxs-lookup"><span data-stu-id="46456-204">Sports Car</span></span> |
| <span data-ttu-id="46456-205">HNVCE6YFZSA5M82NY</span><span class="sxs-lookup"><span data-stu-id="46456-205">HNVCE6YFZSA5M82NY</span></span> |<span data-ttu-id="46456-206">Střední SUV</span><span class="sxs-lookup"><span data-stu-id="46456-206">Medium SUV</span></span> |
| <span data-ttu-id="46456-207">4R30FOR7NUOBL05GJ</span><span class="sxs-lookup"><span data-stu-id="46456-207">4R30FOR7NUOBL05GJ</span></span> |<span data-ttu-id="46456-208">Kombi</span><span class="sxs-lookup"><span data-stu-id="46456-208">Station Wagon</span></span> |
| <span data-ttu-id="46456-209">WYNIIY42VKV6OQS1J</span><span class="sxs-lookup"><span data-stu-id="46456-209">WYNIIY42VKV6OQS1J</span></span> |<span data-ttu-id="46456-210">Velké SUV</span><span class="sxs-lookup"><span data-stu-id="46456-210">Large SUV</span></span> |
| <span data-ttu-id="46456-211">8Y5QKG27QET1RBK7I</span><span class="sxs-lookup"><span data-stu-id="46456-211">8Y5QKG27QET1RBK7I</span></span> |<span data-ttu-id="46456-212">Velké SUV</span><span class="sxs-lookup"><span data-stu-id="46456-212">Large SUV</span></span> |
| <span data-ttu-id="46456-213">DF6OX2WSRA6511BVG</span><span class="sxs-lookup"><span data-stu-id="46456-213">DF6OX2WSRA6511BVG</span></span> |<span data-ttu-id="46456-214">Kupé</span><span class="sxs-lookup"><span data-stu-id="46456-214">Coupe</span></span> |
| <span data-ttu-id="46456-215">Z2EOZWZBXAEW3E60T</span><span class="sxs-lookup"><span data-stu-id="46456-215">Z2EOZWZBXAEW3E60T</span></span> |<span data-ttu-id="46456-216">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="46456-216">Sedan</span></span> |
| <span data-ttu-id="46456-217">M4TV6IEALD5QDS3IR</span><span class="sxs-lookup"><span data-stu-id="46456-217">M4TV6IEALD5QDS3IR</span></span> |<span data-ttu-id="46456-218">Hybridní</span><span class="sxs-lookup"><span data-stu-id="46456-218">Hybrid</span></span> |
| <span data-ttu-id="46456-219">VHRA1Y2TGTA84F00H</span><span class="sxs-lookup"><span data-stu-id="46456-219">VHRA1Y2TGTA84F00H</span></span> |<span data-ttu-id="46456-220">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="46456-220">Family Saloon</span></span> |
| <span data-ttu-id="46456-221">R0JAUHT1L1R3BIKI0</span><span class="sxs-lookup"><span data-stu-id="46456-221">R0JAUHT1L1R3BIKI0</span></span> |<span data-ttu-id="46456-222">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="46456-222">Sedan</span></span> |
| <span data-ttu-id="46456-223">9230C202Z60XX84AU</span><span class="sxs-lookup"><span data-stu-id="46456-223">9230C202Z60XX84AU</span></span> |<span data-ttu-id="46456-224">Hybridní</span><span class="sxs-lookup"><span data-stu-id="46456-224">Hybrid</span></span> |
| <span data-ttu-id="46456-225">T8DNDN5UDCWL7M72H</span><span class="sxs-lookup"><span data-stu-id="46456-225">T8DNDN5UDCWL7M72H</span></span> |<span data-ttu-id="46456-226">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="46456-226">Family Saloon</span></span> |
| <span data-ttu-id="46456-227">4WPYRUZII5YV7YA42</span><span class="sxs-lookup"><span data-stu-id="46456-227">4WPYRUZII5YV7YA42</span></span> |<span data-ttu-id="46456-228">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="46456-228">Sedan</span></span> |
| <span data-ttu-id="46456-229">D1ZVY26UV2BFGHZNO</span><span class="sxs-lookup"><span data-stu-id="46456-229">D1ZVY26UV2BFGHZNO</span></span> |<span data-ttu-id="46456-230">Hybridní</span><span class="sxs-lookup"><span data-stu-id="46456-230">Hybrid</span></span> |
| <span data-ttu-id="46456-231">XUF99EW9OIQOMV7Q7</span><span class="sxs-lookup"><span data-stu-id="46456-231">XUF99EW9OIQOMV7Q7</span></span> |<span data-ttu-id="46456-232">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="46456-232">Family Saloon</span></span> |
| <span data-ttu-id="46456-233">8OMCL3LGI7XNCC21U</span><span class="sxs-lookup"><span data-stu-id="46456-233">8OMCL3LGI7XNCC21U</span></span> |<span data-ttu-id="46456-234">Převoditelné</span><span class="sxs-lookup"><span data-stu-id="46456-234">Convertible</span></span> |
| <span data-ttu-id="46456-235">…….</span><span class="sxs-lookup"><span data-stu-id="46456-235">…….</span></span> | |

### <a name="references"></a><span data-ttu-id="46456-236">Odkazy</span><span class="sxs-lookup"><span data-stu-id="46456-236">References</span></span>
[<span data-ttu-id="46456-237">Řešení vehicle telematika simulátoru sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46456-237">Vehicle Telematics Simulator Visual Studio Solution</span></span>](http://go.microsoft.com/fwlink/?LinkId=717075) 

[<span data-ttu-id="46456-238">Centra událostí Azure</span><span class="sxs-lookup"><span data-stu-id="46456-238">Azure Event Hub</span></span>](https://azure.microsoft.com/services/event-hubs/)

[<span data-ttu-id="46456-239">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="46456-239">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a><span data-ttu-id="46456-240">Přijímání</span><span class="sxs-lookup"><span data-stu-id="46456-240">Ingestion</span></span>
<span data-ttu-id="46456-241">Kombinace Azure Event Hubs, Stream Analytics a objektu pro vytváření dat jsou využity tooingest hello vehicle signály, hello diagnostických událostí a v reálném čase a analýza služby batch.</span><span class="sxs-lookup"><span data-stu-id="46456-241">Combinations of Azure Event Hubs, Stream Analytics, and Data Factory are leveraged tooingest hello vehicle signals, hello diagnostic events, and real-time and batch analytics.</span></span> <span data-ttu-id="46456-242">Všechny tyto součásti vytvoříte a nakonfigurujete jako součást nasazení řešení hello.</span><span class="sxs-lookup"><span data-stu-id="46456-242">All these components are created and configured as part of hello solution deployment.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="46456-243">Analýzy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="46456-243">Real-time analysis</span></span>
<span data-ttu-id="46456-244">Hello událostí generovaných hello Vehicle telematika simulátoru jsou publikované pomocí centra událostí toohello hello SDK centra událostí.</span><span class="sxs-lookup"><span data-stu-id="46456-244">hello events generated by hello Vehicle Telematics Simulator are published toohello Event Hub using hello Event Hub SDK.</span></span> <span data-ttu-id="46456-245">Úloha Stream Analytics Hello ingestuje tyto události z hello centra událostí a procesy hello data v reálném čase tooanalyze hello vehicle stavu.</span><span class="sxs-lookup"><span data-stu-id="46456-245">hello Stream Analytics job ingests these events from hello Event Hub and processes hello data in real time tooanalyze hello vehicle health.</span></span> 

![Řídicí panel centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

<span data-ttu-id="46456-247">*Obrázek 4 – řídicí panel centra událostí*</span><span class="sxs-lookup"><span data-stu-id="46456-247">*Figure 4 - Event Hub dashboard*</span></span>

![Stream Analytics úlohy zpracování dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

<span data-ttu-id="46456-249">*Obrázek 5 – úloha Stream Analytics zpracování dat*</span><span class="sxs-lookup"><span data-stu-id="46456-249">*Figure 5 - Stream Analytics job processing data*</span></span>

<span data-ttu-id="46456-250">Úloha Stream Analytics Hello;</span><span class="sxs-lookup"><span data-stu-id="46456-250">hello Stream Analytics job;</span></span>

* <span data-ttu-id="46456-251">ingestuje data z hello centra událostí</span><span class="sxs-lookup"><span data-stu-id="46456-251">ingests data from hello Event Hub</span></span> 
* <span data-ttu-id="46456-252">provede připojení k s hello referenční data toomap hello vehicle VIN toohello odpovídající model</span><span class="sxs-lookup"><span data-stu-id="46456-252">performs a join with hello reference data toomap hello vehicle VIN toohello corresponding model</span></span> 
* <span data-ttu-id="46456-253">dál je do Azure blob storage analytics bohaté batch.</span><span class="sxs-lookup"><span data-stu-id="46456-253">persists them into Azure blob storage for rich batch analytics.</span></span> 

<span data-ttu-id="46456-254">Následující dotaz služby Stream Analytics Hello je použité toopersist hello data do úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="46456-254">hello following Stream Analytics query is used toopersist hello data into Azure blob storage.</span></span> 

![Dotaz úlohy Stream Analytics přijímání dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

<span data-ttu-id="46456-256">*Obrázek 6 - Stream Analytics úlohy dotazu pro přijímání dat*</span><span class="sxs-lookup"><span data-stu-id="46456-256">*Figure 6 - Stream Analytics job query for data ingestion*</span></span>

### <a name="batch-analysis"></a><span data-ttu-id="46456-257">Dávková analýza</span><span class="sxs-lookup"><span data-stu-id="46456-257">Batch analysis</span></span>
<span data-ttu-id="46456-258">Také jsme generují další svazek simulované vehicle signály a diagnostiky datové sady pro širší batch analýzu.</span><span class="sxs-lookup"><span data-stu-id="46456-258">We are also generating an additional volume of simulated vehicle signals and diagnostic dataset for richer batch analytics.</span></span> <span data-ttu-id="46456-259">Toto je požadovaná tooensure dobrý reprezentativní datový svazek pro dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="46456-259">This is required tooensure a good representative data volume for batch processing.</span></span> <span data-ttu-id="46456-260">Pro tento účel používáme kanál s názvem "PrepareSampleDataPipeline" v toogenerate pracovní postup Azure Data Factory hello za jeden rok simulované vehicle signály a diagnostiky datové sady.</span><span class="sxs-lookup"><span data-stu-id="46456-260">For this purpose, we are using a pipeline named "PrepareSampleDataPipeline" in hello Azure Data Factory workflow toogenerate one year's worth of simulated vehicle signals and diagnostic dataset.</span></span> <span data-ttu-id="46456-261">Klikněte na tlačítko [vlastní aktivity služby Data Factory](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello aktivity DotNet vlastní objekt pro vytváření dat řešení sady Visual Studio pro přizpůsobení podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="46456-261">Click [Data Factory custom activity](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello Data Factory custom DotNet activity Visual Studio solution for customizations based on your requirements.</span></span> 

![Příprava ukázková data pro dávkové zpracování pracovního postupu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

<span data-ttu-id="46456-263">*Obrázek 7: Příprava ukázková data pro zpracování pracovní postup služby batch*</span><span class="sxs-lookup"><span data-stu-id="46456-263">*Figure 7 - Prepare Sample data for batch processing workflow*</span></span>

<span data-ttu-id="46456-264">Hello kanálu se skládá z vlastní .net ADF aktivity, zobrazit tady:</span><span class="sxs-lookup"><span data-stu-id="46456-264">hello pipeline consists of a custom ADF .Net Activity, show here:</span></span>

![PrepareSampleDataPipeline aktivity](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

<span data-ttu-id="46456-266">*Obrázek 8 - PrepareSampleDataPipeline*</span><span class="sxs-lookup"><span data-stu-id="46456-266">*Figure 8 - PrepareSampleDataPipeline*</span></span>

<span data-ttu-id="46456-267">Jakmile hello kanálu provede úspěšně a datovou sadu "RawCarEventsTable" je označen jako "Připravený", jeden rok vhodné simulované vehicle signálů a diagnostických dat vytváří.</span><span class="sxs-lookup"><span data-stu-id="46456-267">Once hello pipeline executes successfully and "RawCarEventsTable" dataset is marked "Ready", one-year worth of simulated vehicle signals and diagnostic data are produced.</span></span> <span data-ttu-id="46456-268">Zobrazí hello následující složku a soubor vytvořený v účtu úložiště v kontejneru "connectedcar" hello:</span><span class="sxs-lookup"><span data-stu-id="46456-268">You see hello following folder and file created in your storage account under hello "connectedcar" container:</span></span>

![Výstup PrepareSampleDataPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

<span data-ttu-id="46456-270">*Obrázek 9 - PrepareSampleDataPipeline výstup*</span><span class="sxs-lookup"><span data-stu-id="46456-270">*Figure 9 - PrepareSampleDataPipeline Output*</span></span>

### <a name="references"></a><span data-ttu-id="46456-271">Odkazy</span><span class="sxs-lookup"><span data-stu-id="46456-271">References</span></span>
[<span data-ttu-id="46456-272">Azure SDK centra událostí pro přijímání datového proudu</span><span class="sxs-lookup"><span data-stu-id="46456-272">Azure Event Hub SDK for stream ingestion</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

<span data-ttu-id="46456-273">[Možnosti přesun dat Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
[aktivita služby Azure Data Factory DotNet.](../data-factory/data-factory-use-custom-activities.md)</span><span class="sxs-lookup"><span data-stu-id="46456-273">[Azure Data Factory data movement capabilities](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet Activity](../data-factory/data-factory-use-custom-activities.md)</span></span>

[<span data-ttu-id="46456-274">Řešení sady visual studio Azure Data Factory DotNet aktivity pro přípravu ukázková data</span><span class="sxs-lookup"><span data-stu-id="46456-274">Azure Data Factory DotNet activity visual studio solution for preparing sample data</span></span>](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-hello-dataset"></a><span data-ttu-id="46456-275">Oddíl hello datové sady</span><span class="sxs-lookup"><span data-stu-id="46456-275">Partition hello dataset</span></span>
<span data-ttu-id="46456-276">signály nezpracovaná částečně strukturovaných vehicle Hello a diagnostiky datové sady jsou rozděleny do oddílů v kroku přípravy hello dat do formátu měsíc roku.</span><span class="sxs-lookup"><span data-stu-id="46456-276">hello raw semi-structured vehicle signals and diagnostic dataset are partitioned in hello data preparation step into a YEAR/MONTH format.</span></span> <span data-ttu-id="46456-277">Toto rozdělení do oddílů zvýší úroveň efektivnější dotazování a škálovatelné dlouhodobé úložiště povolením odolnost převzetí z jednoho objektu blob účet toohello vedle jako první účet hello zaplní.</span><span class="sxs-lookup"><span data-stu-id="46456-277">This partitioning promotes more efficient querying and scalable long-term storage by enabling fault-over from one blob account toohello next as hello first account fills up.</span></span> 

>[!NOTE] 
><span data-ttu-id="46456-278">Tento krok v řešení hello je použít jenom toobatch zpracování.</span><span class="sxs-lookup"><span data-stu-id="46456-278">This step in hello solution is applicable only toobatch processing.</span></span>

<span data-ttu-id="46456-279">Vstupní a výstupní data správy dat:</span><span class="sxs-lookup"><span data-stu-id="46456-279">Input and output data data management:</span></span>

* <span data-ttu-id="46456-280">Hello **výstupní data** (s názvem bez přípony *PartitionedCarEventsTable*) se ukládají jako toobe na dlouhou dobu jako formulář základní / "rawest" hello dat v hello zákazníka "Data Lake".</span><span class="sxs-lookup"><span data-stu-id="46456-280">hello **output data** (labeled *PartitionedCarEventsTable*) is toobe kept for a long period of time as hello foundational/"rawest" form of data in hello customer's "Data Lake".</span></span> 
* <span data-ttu-id="46456-281">Hello **vstupní data** toothis kanálu by obvykle odstraněn jako hello výstupních dat má úplné věrnosti toohello vstup – stačí uložené (oddíly) lépe pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="46456-281">hello **input data** toothis pipeline would typically be discarded as hello output data has full fidelity toohello input - it's just stored (partitioned) better for subsequent use.</span></span>

![Pracovní postup události car oddílu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

<span data-ttu-id="46456-283">*Obrázek 10: pracovní postup události Car oddílu*</span><span class="sxs-lookup"><span data-stu-id="46456-283">*Figure 10 – Partition Car Events workflow*</span></span>

<span data-ttu-id="46456-284">nezpracovaná data Hello je rozdělen na oddíly pomocí aktivitu Hive HDInsight v "PartitionCarEventsPipeline".</span><span class="sxs-lookup"><span data-stu-id="46456-284">hello raw data is partitioned using a Hive HDInsight activity in "PartitionCarEventsPipeline".</span></span> <span data-ttu-id="46456-285">Ukázková data Hello vygenerované v roce v kroku 1 je rozdělena na oddíly pomocí měsíc roku.</span><span class="sxs-lookup"><span data-stu-id="46456-285">hello sample data generated in step 1 for a year is partitioned by YEAR/MONTH.</span></span> <span data-ttu-id="46456-286">Hello oddíly jsou signály použité toogenerate vehicle a diagnostických dat pro každý měsíc (celkový počet 12 oddíly) v roce.</span><span class="sxs-lookup"><span data-stu-id="46456-286">hello partitions are used toogenerate vehicle signals and diagnostic data for each month (total 12 partitions) of a year.</span></span> 

![PartitionCarEventsPipeline aktivity](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

<span data-ttu-id="46456-288">*Obrázek 11 – PartitionCarEventsPipeline*</span><span class="sxs-lookup"><span data-stu-id="46456-288">*Figure 11 - PartitionCarEventsPipeline*</span></span>

<span data-ttu-id="46456-289">***Skript PartitionConnectedCarEvents Hive***</span><span class="sxs-lookup"><span data-stu-id="46456-289">***PartitionConnectedCarEvents Hive Script***</span></span>

<span data-ttu-id="46456-290">Hello následující skript Hive, s názvem "partitioncarevents.hql", slouží k vytváření oddílů a je umístěn ve složce "\demo\src\connectedcar\scripts" hello hello stáhnout zip.</span><span class="sxs-lookup"><span data-stu-id="46456-290">hello following Hive script, named "partitioncarevents.hql", is used for partitioning and is located in hello "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

<span data-ttu-id="46456-291">Jakmile hello kanálu proveden úspěšně, zobrazí následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar" hello hello.</span><span class="sxs-lookup"><span data-stu-id="46456-291">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![Oddílů výstup](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

<span data-ttu-id="46456-293">*Obrázek 12 - oddílů výstup*</span><span class="sxs-lookup"><span data-stu-id="46456-293">*Figure 12 - Partitioned Output*</span></span>

<span data-ttu-id="46456-294">Hello dat je nyní optimalizovaná, správu a připraveno pro další zpracování toogain bohaté batch statistiky.</span><span class="sxs-lookup"><span data-stu-id="46456-294">hello data is now optimized, is more manageable and ready for further processing toogain rich batch insights.</span></span> 

## <a name="data-analysis"></a><span data-ttu-id="46456-295">Analýza dat</span><span class="sxs-lookup"><span data-stu-id="46456-295">Data Analysis</span></span>
<span data-ttu-id="46456-296">V této části, uvidíte, jak toocombine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory a Azure HDInsight pro bohaté pokročilé analýzy vehicle stavu a řídí zvyklosti.</span><span class="sxs-lookup"><span data-stu-id="46456-296">In this section, you see how toocombine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory, and Azure HDInsight for rich advanced analytics on vehicle health and driving habits.</span></span> <span data-ttu-id="46456-297">Existují tři pododdíly tady:</span><span class="sxs-lookup"><span data-stu-id="46456-297">There are three subsections here:</span></span>

1. <span data-ttu-id="46456-298">**Strojového učení**: Tato část obsahuje informace o hello experimentu detekce anomálií, který jsme použili v vozidel toopredict toto řešení nutnosti údržby údržby a nutnosti navrácení z důvodu problémů toosafety vozidel.</span><span class="sxs-lookup"><span data-stu-id="46456-298">**Machine Learning**: This subsection contains information on hello anomaly detection experiment that we have used in this solution toopredict vehicles requiring servicing maintenance and vehicles requiring recalls due toosafety issues.</span></span>
2. <span data-ttu-id="46456-299">**Analýzy v reálném čase**: Tato část obsahuje informace týkající se analýzy hello v reálném čase pomocí hello Stream Analytics Query Language a zprovozňování hello strojové učení experiment v reálném čase pomocí vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="46456-299">**Real-time analysis**: This subsection contains information regarding hello real-time analytics using hello Stream Analytics Query Language and operationalizing hello machine learning experiment in real time using a custom application.</span></span>
3. <span data-ttu-id="46456-300">**Batch analysis**: Tato část obsahuje informace týkající se hello transformaci a zpracování dat hello batch pomocí Azure HDInsight nebo Azure Machine Learning operationalized službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="46456-300">**Batch analysis**: This subsection contains information regarding hello transforming and processing of hello batch data using Azure HDInsight and Azure Machine Learning operationalized by Azure Data Factory.</span></span>

### <a name="machine-learning"></a><span data-ttu-id="46456-301">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="46456-301">Machine Learning</span></span>
<span data-ttu-id="46456-302">Zde Naším cílem je toopredict hello vozidel, které vyžadují údržby nebo odvolání na základě statistik určité stavu.</span><span class="sxs-lookup"><span data-stu-id="46456-302">Our goal here is toopredict hello vehicles that require maintenance or recall based on certain heath statistics.</span></span> <span data-ttu-id="46456-303">Provedeme následující předpoklady hello</span><span class="sxs-lookup"><span data-stu-id="46456-303">We make hello following assumptions</span></span>

* <span data-ttu-id="46456-304">Pokud platí jedna z následujících tří podmínek hello, vyžadují hello vozidel **obsluhy údržby**:</span><span class="sxs-lookup"><span data-stu-id="46456-304">If one of hello following three conditions are true, hello vehicles require **servicing maintenance**:</span></span>
  
  * <span data-ttu-id="46456-305">Můžete zadat naléhavost je nízká</span><span class="sxs-lookup"><span data-stu-id="46456-305">Tire pressure is low</span></span>
  * <span data-ttu-id="46456-306">Modul těžba ropy úroveň je nízká</span><span class="sxs-lookup"><span data-stu-id="46456-306">Engine oil level is low</span></span>
  * <span data-ttu-id="46456-307">Modul teploty je vysoká.</span><span class="sxs-lookup"><span data-stu-id="46456-307">Engine temperature is high</span></span>
* <span data-ttu-id="46456-308">Pokud platí jedna z následujících podmínek hello, může mít hello vozidel **problém zabezpečení** a vyžadovat **odvolání**:</span><span class="sxs-lookup"><span data-stu-id="46456-308">If one of hello following conditions are true, hello vehicles may have a **safety issue** and require **recall**:</span></span>
  
  * <span data-ttu-id="46456-309">Modul teploty je vysoká, ale mimo teploty je nízká</span><span class="sxs-lookup"><span data-stu-id="46456-309">Engine temperature is high but outside temperature is low</span></span>
  * <span data-ttu-id="46456-310">Modul teploty je nízký, ale mimo teploty je vysoká.</span><span class="sxs-lookup"><span data-stu-id="46456-310">Engine temperature is low but outside temperature is high</span></span>

<span data-ttu-id="46456-311">Na základě hello předchozí požadavků, jsme vytvořili dva samostatné modely toodetect anomálií, jeden pro zjišťování vehicle údržby a jeden pro zjišťování pro vyvolání vehicle.</span><span class="sxs-lookup"><span data-stu-id="46456-311">Based on hello previous requirements, we have created two separate models toodetect anomalies, one for vehicle maintenance detection, and one for vehicle recall detection.</span></span> <span data-ttu-id="46456-312">V obou těchto modelech hello předdefinované hlavní součásti Analysis (PCA) algoritmus se používá pro detekce anomálií.</span><span class="sxs-lookup"><span data-stu-id="46456-312">In both these models, hello built-in Principal Component Analysis (PCA) algorithm is used for anomaly detection.</span></span> 

<span data-ttu-id="46456-313">**Model pro odhalování údržby**</span><span class="sxs-lookup"><span data-stu-id="46456-313">**Maintenance detection model**</span></span>

<span data-ttu-id="46456-314">Pokud jeden z tři indikátory - zatížení můžete zadat, těžba ropy modul nebo modul teploty - splňuje jeho odpovídající stav, hello údržby detekce modelu sestavy anomálií.</span><span class="sxs-lookup"><span data-stu-id="46456-314">If one of three indicators - tire pressure, engine oil, or engine temperature - satisfies its respective condition, hello maintenance detection model reports an anomaly.</span></span> <span data-ttu-id="46456-315">V důsledku toho pouze potřebujeme tooconsider těchto tří proměnných v sestavení hello modelu.</span><span class="sxs-lookup"><span data-stu-id="46456-315">As a result, we only need tooconsider these three variables in building hello model.</span></span> <span data-ttu-id="46456-316">V našem experimentu v Azure Machine Learning, nejprve používáme **výběr sloupců v datové sadě** modulu tooextract těchto tří proměnných.</span><span class="sxs-lookup"><span data-stu-id="46456-316">In our experiment in Azure Machine Learning, we first use a **Select Columns in Dataset** module tooextract these three variables.</span></span> <span data-ttu-id="46456-317">Další používáme hello anomálií založený na PCA detekce modulu toobuild hello anomálií detekce modelu.</span><span class="sxs-lookup"><span data-stu-id="46456-317">Next we use hello PCA-based anomaly detection module toobuild hello anomaly detection model.</span></span> 

<span data-ttu-id="46456-318">Hlavní součásti Analysis (PCA) je zavedených technika v machine learning, které můžou být použité toofeature výběr, klasifikace a anomálií detekce.</span><span class="sxs-lookup"><span data-stu-id="46456-318">Principal Component Analysis (PCA) is an established technique in machine learning that can be applied toofeature selection, classification, and anomaly detection.</span></span> <span data-ttu-id="46456-319">PCA převede sadu případ obsahující pravděpodobně korelační proměnné, do sady hodnot nazývané jako hlavní komponenty.</span><span class="sxs-lookup"><span data-stu-id="46456-319">PCA converts a set of case containing possibly correlated variables, into a set of values called principal components.</span></span> <span data-ttu-id="46456-320">Hello klíče představu o tom, na základě PCA modelování je tooproject dat na nižší dimenzí místa, aby funkce a anomálií, lze snadno identifikovat.</span><span class="sxs-lookup"><span data-stu-id="46456-320">hello key idea of PCA-based modeling is tooproject data onto a lower-dimensional space so that features and anomalies can be more easily identified.</span></span>

<span data-ttu-id="46456-321">Pro každý nový vstup příliš hello model pro odhalování, detekce anomálií hello nejprve vypočítá jeho projekce na hello eigenvectors a pak výpočtů hello normalized obnovu chyby.</span><span class="sxs-lookup"><span data-stu-id="46456-321">For each new input too hello detection model, hello anomaly detector first computes its projection on hello eigenvectors, and then computes hello normalized reconstruction error.</span></span> <span data-ttu-id="46456-322">Tato chyba normalizovaný je hello anomálií skóre.</span><span class="sxs-lookup"><span data-stu-id="46456-322">This normalized error is hello anomaly score.</span></span> <span data-ttu-id="46456-323">Hello vyšší hello chyba hello více neobvyklé hello instance je.</span><span class="sxs-lookup"><span data-stu-id="46456-323">hello higher hello error, hello more anomalous hello instance is.</span></span> 

<span data-ttu-id="46456-324">V hello údržby zjišťování problému každý záznam lze považovat za bod v prostorovém 3 prostoru definovaný zatížení můžete zadat, modul těžba ropy a teploty modul souřadnice.</span><span class="sxs-lookup"><span data-stu-id="46456-324">In hello maintenance detection problem, each record can be considered as a point in a 3-dimensional space defined by tire pressure, engine oil, and engine temperature coordinates.</span></span> <span data-ttu-id="46456-325">toocapture tyto anomálií jsme projektu hello původní data v prostoru 3 dimenzí hello na 2 dimenzí prostor pomocí PCA.</span><span class="sxs-lookup"><span data-stu-id="46456-325">toocapture these anomalies, we can project hello original data in hello 3-dimensional space onto a 2-dimensional space using PCA.</span></span> <span data-ttu-id="46456-326">Proto jsme nastavte parametr hello počet toouse součásti v PCA toobe 2.</span><span class="sxs-lookup"><span data-stu-id="46456-326">Thus, we set hello parameter Number of components toouse in PCA toobe 2.</span></span> <span data-ttu-id="46456-327">Tento parametr hraje důležitou roli při uplatňování detekce anomálií založený na PCA.</span><span class="sxs-lookup"><span data-stu-id="46456-327">This parameter plays an important role in applying PCA-based anomaly detection.</span></span> <span data-ttu-id="46456-328">Po plánování dat pomocí PCA abychom mohli snadno identifikovat tyto anomálií.</span><span class="sxs-lookup"><span data-stu-id="46456-328">After projecting data using PCA, we can identify these anomalies more easily.</span></span>

<span data-ttu-id="46456-329">**Model detekce anomálií odvolání** v modelu detekce anomálií hello odvolání používáme hello výběr sloupců v datové sadě a anomálií založený na PCA moduly rozpoznávání podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="46456-329">**Recall anomaly detection model** In hello recall anomaly detection model, we use hello Select Columns in Dataset and PCA-based anomaly detection modules in a similar way.</span></span> <span data-ttu-id="46456-330">Konkrétně jsme nejprve extrahovat tří proměnných - teplota motoru, mimo teploty a rychlosti – pomocí hello **výběr sloupců v datové sadě** modulu.</span><span class="sxs-lookup"><span data-stu-id="46456-330">Specifically, we first extract three variables - engine temperature, outside temperature, and speed - using hello **Select Columns in Dataset** module.</span></span> <span data-ttu-id="46456-331">Můžeme také zahrnovat hello rychlost proměnná vzhledem k tomu, že teplota motoru hello je obvykle korelační toohello rychlost.</span><span class="sxs-lookup"><span data-stu-id="46456-331">We also include hello speed variable since hello engine temperature typically is correlated toohello speed.</span></span> <span data-ttu-id="46456-332">Další používáme anomálií založený na PCA detekce modulu tooproject hello data z hello 3 dimenzí prostoru na 2 dimenzí.</span><span class="sxs-lookup"><span data-stu-id="46456-332">Next we use PCA-based anomaly detection module tooproject hello data from hello 3-dimensional space onto a 2-dimensional space.</span></span> <span data-ttu-id="46456-333">jsou splněna kritéria pro vyvolání Hello a proto hello vehicle při vysoce negativně korelační modul teploty a mimo teploty vyžaduje odvolání.</span><span class="sxs-lookup"><span data-stu-id="46456-333">hello recall criteria are satisfied and so hello vehicle requires recall when engine temperature and outside temperature are highly negatively correlated.</span></span> <span data-ttu-id="46456-334">Pomocí algoritmu detekce anomálií založený na PCA, jsme po provedení PCA zachytit hello anomálií.</span><span class="sxs-lookup"><span data-stu-id="46456-334">Using PCA-based anomaly detection algorithm, we can capture hello anomalies after performing PCA.</span></span> 

<span data-ttu-id="46456-335">Cvičení buď modelu, potřebujeme toouse normální data, která nevyžaduje údržby nebo odvolání jako model detekce anomálií založený na PCA tootrain hello aplikace hello vstupní data.</span><span class="sxs-lookup"><span data-stu-id="46456-335">When training either model, we need toouse normal data, which does not require maintenance or recall as hello input data tootrain hello PCA-based anomaly detection model.</span></span> <span data-ttu-id="46456-336">V hello vyhodnocování experiment použijeme toodetect modelu detekce anomálií hello Trénink zda hello vehicle vyžaduje údržby nebo odvolání.</span><span class="sxs-lookup"><span data-stu-id="46456-336">In hello scoring experiment, we use hello trained anomaly detection model toodetect whether or not hello vehicle requires maintenance or recall.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="46456-337">Analýzy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="46456-337">Real-time analysis</span></span>
<span data-ttu-id="46456-338">Následující dotaz SQL Stream Analytics Hello slouží tooget hello průměr všech hello parametry důležité vehicle jako rychlosti, úroveň paliva, modul teploty, počítadlo kilometrů čtení, můžete zadat naléhavost, modul těžba ropy úroveň a dalších.</span><span class="sxs-lookup"><span data-stu-id="46456-338">hello following Stream Analytics SQL Query is used tooget hello average of all hello important vehicle parameters like vehicle speed, fuel level, engine temperature, odometer reading, tire pressure, engine oil level, and others.</span></span> <span data-ttu-id="46456-339">Hello průměry jsou použité toodetect anomálií, vydávání výstrah a určit hello celkového stavu podmínky vozidel provozovat v určité oblasti a pak ho korelovat toodemographics.</span><span class="sxs-lookup"><span data-stu-id="46456-339">hello averages are used toodetect anomalies, issue alerts, and determine hello overall health conditions of vehicles operated in specific region and then correlate it toodemographics.</span></span> 

![Stream Analytics query pro zpracování v reálném čase](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

<span data-ttu-id="46456-341">*Obrázek 13: dotaz služby Stream Analytics ke zpracování v reálném čase*</span><span class="sxs-lookup"><span data-stu-id="46456-341">*Figure 13 – Stream Analytics query for real-time processing*</span></span>

<span data-ttu-id="46456-342">Všechny průměry hello jsou vypočítávány přes TumblingWindow 3 sekundu.</span><span class="sxs-lookup"><span data-stu-id="46456-342">All hello averages are calculated over a 3-second TumblingWindow.</span></span> <span data-ttu-id="46456-343">TubmlingWindow se používá v tomto případě vzhledem k tomu, že je nutné, aby se a souvislý časové intervaly.</span><span class="sxs-lookup"><span data-stu-id="46456-343">We are using TubmlingWindow in this case since we require non-overlapping and contiguous time intervals.</span></span> 

<span data-ttu-id="46456-344">Klikněte na tlačítko Další informace o všechny možnosti "Oddílová" hello v Azure Stream Analytics toolearn [Oddílová (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span><span class="sxs-lookup"><span data-stu-id="46456-344">toolearn more about all hello "Windowing" capabilities in Azure Stream Analytics, click [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span></span>

<span data-ttu-id="46456-345">**V reálném čase předpovědi**</span><span class="sxs-lookup"><span data-stu-id="46456-345">**Real-time prediction**</span></span>

<span data-ttu-id="46456-346">Aplikace je součástí model hello řešení toooperationalize hello machine learning v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="46456-346">An application is included as part of hello solution toooperationalize hello machine learning model in real time.</span></span> <span data-ttu-id="46456-347">Tuto aplikaci s názvem "RealTimeDashboardApp" je vytvořen a nakonfigurován jako součást nasazení řešení hello.</span><span class="sxs-lookup"><span data-stu-id="46456-347">This application called “RealTimeDashboardApp” is created and configured as part of hello solution deployment.</span></span> <span data-ttu-id="46456-348">aplikace Hello provádí hello následující:</span><span class="sxs-lookup"><span data-stu-id="46456-348">hello application performs hello following:</span></span>

1. <span data-ttu-id="46456-349">Naslouchá instance tooan centra událostí, kde Stream Analytics je publikování hello událostí ve tvaru nepřetržitě.</span><span class="sxs-lookup"><span data-stu-id="46456-349">Listens tooan Event Hub instance where Stream Analytics is publishing hello events in a pattern continuously.</span></span> <span data-ttu-id="46456-350">![Stream Analytics dotazu pro publikování dat hello](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *obrázek 14: Stream Analytics dotazu pro publikování dat tooan hello výstup instance centra událostí*</span><span class="sxs-lookup"><span data-stu-id="46456-350">![Stream Analytics query for publishing hello data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *Figure 14 – Stream Analytics query for publishing hello data tooan output Event Hub instance*</span></span> 
2. <span data-ttu-id="46456-351">Pro každou událost, která obdrží tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="46456-351">For every event that this application receives:</span></span> 
   
   * <span data-ttu-id="46456-352">Data hello procesů používá koncový bod Machine Learning požadavků a odpovědí vyhodnocování (záznamy RR).</span><span class="sxs-lookup"><span data-stu-id="46456-352">Processes hello data using Machine Learning Request-Response Scoring (RRS) endpoint.</span></span> <span data-ttu-id="46456-353">Hello záznamy o prostředku koncového bodu je automaticky publikován jako součást nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="46456-353">hello RRS endpoint is automatically published as part of hello deployment.</span></span>
   * <span data-ttu-id="46456-354">výstup RRS Hello je datová sada publikované tooa Power BI pomocí nabízených hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="46456-354">hello RRS output is published tooa Power BI dataset using hello push APIs.</span></span>

<span data-ttu-id="46456-355">Tento vzor je také použít tooscenarios, ve kterém chcete toointegrate obchodní (LoB) aplikace s tokem hello analýzu v reálném čase, pro scénáře, jako je výstrahy, oznámení a zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="46456-355">This pattern is also applicable tooscenarios in which you want toointegrate a Line of Business (LoB) application with hello real-time analytics flow, for scenarios such as alerts, notifications, and messaging.</span></span>

<span data-ttu-id="46456-356">Klikněte na tlačítko [RealtimeDashboardApp stažení](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello řešení sady Visual Studio RealtimeDashboardApp pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="46456-356">Click [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello RealtimeDashboardApp Visual Studio solution for customizations.</span></span> 

<span data-ttu-id="46456-357">**tooexecute hello aplikace řídicího panelu v reálném čase**</span><span class="sxs-lookup"><span data-stu-id="46456-357">**tooexecute hello Real-time Dashboard Application**</span></span>
1. <span data-ttu-id="46456-358">Extrahování a uložit místně ![RealtimeDashboardApp složky](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *obrázek 16 – RealtimeDashboardApp složky*</span><span class="sxs-lookup"><span data-stu-id="46456-358">Extract and save locally ![RealtimeDashboardApp folder](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *Figure 16 – RealtimeDashboardApp folder*</span></span>  
2. <span data-ttu-id="46456-359">Spuštění aplikace hello RealtimeDashboardApp.exe</span><span class="sxs-lookup"><span data-stu-id="46456-359">Execute hello application RealtimeDashboardApp.exe</span></span>
3. <span data-ttu-id="46456-360">Zadejte platné přihlašovací údaje Power BI, přihlášení a klikněte na tlačítko Přijmout</span><span class="sxs-lookup"><span data-stu-id="46456-360">Provide valid Power BI credentials, sign in and click Accept</span></span> ![Řídicí panel v reálném čase aplikace přihlášení tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Aplikace v reálném čase řídicího panelu dokončit přihlášení tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

<span data-ttu-id="46456-363">*Obrázek 17 – RealtimeDashboardApp: Přihlášení tooPower BI*</span><span class="sxs-lookup"><span data-stu-id="46456-363">*Figure 17 – RealtimeDashboardApp: Sign-in tooPower BI*</span></span>

>[!NOTE] 
><span data-ttu-id="46456-364">Pokud chcete datovou sadu Power BI hello tooflush, spusťte hello RealtimeDashboardApp s parametrem "flushdata" hello:</span><span class="sxs-lookup"><span data-stu-id="46456-364">If you want tooflush hello Power BI dataset, execute hello RealtimeDashboardApp with hello "flushdata" parameter:</span></span> 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a><span data-ttu-id="46456-365">Dávková analýza</span><span class="sxs-lookup"><span data-stu-id="46456-365">Batch analysis</span></span>
<span data-ttu-id="46456-366">Hello cílem je, že tooshow jak motory Contoso využívá hello výpočtů Azure možnosti tooharness velkých objemů dat toogain detailní přehled na řízení vzorek, chování využití a stavu vehicle.</span><span class="sxs-lookup"><span data-stu-id="46456-366">hello goal here is tooshow how Contoso Motors utilizes hello Azure compute capabilities tooharness big data toogain rich insights on driving pattern, usage behavior, and vehicle health.</span></span> <span data-ttu-id="46456-367">Díky tomu je možné:</span><span class="sxs-lookup"><span data-stu-id="46456-367">This makes it possible to:</span></span>

* <span data-ttu-id="46456-368">Zlepšovat prostředí zákazníků hello a nastavit jej jako levnější tím, že poskytuje přehled na řídí zvyklosti a efektivní řízení chování paliva</span><span class="sxs-lookup"><span data-stu-id="46456-368">Improve hello customer experience and make it cheaper by providing insights on driving habits and fuel efficient driving behaviors</span></span>
* <span data-ttu-id="46456-369">Další informace proaktivně o zákazníků a jejich řízení patters toogovern obchodních rozhodnutí a poskytují hello nejlépe v třída produkty a služby</span><span class="sxs-lookup"><span data-stu-id="46456-369">Learn proactively about customers and their driving patters toogovern business decisions and provide hello best in class products & services</span></span>

<span data-ttu-id="46456-370">V tomto řešení jsme cílení hello následující metriky:</span><span class="sxs-lookup"><span data-stu-id="46456-370">In this solution, we are targeting hello following metrics:</span></span>

1. <span data-ttu-id="46456-371">**Agresivní řídí chování**: identifikuje hello trend hello modely, umístění, podporovat jeho podmínky a čas hello roku toogain Statistika agresivní řízení vzory.</span><span class="sxs-lookup"><span data-stu-id="46456-371">**Aggressive driving behavior**: Identifies hello trend of hello models, locations, driving conditions, and time of hello year toogain insights on aggressive driving patterns.</span></span> <span data-ttu-id="46456-372">Contoso motory pro můžete použít tyto přehledy marketingových kampaní, řídí přizpůsobené nové funkce a pojišťovnictví na základě využití.</span><span class="sxs-lookup"><span data-stu-id="46456-372">Contoso Motors can use these insights for marketing campaigns, driving new personalized features and usage-based insurance.</span></span>
2. <span data-ttu-id="46456-373">**Paliva efektivní řízení chování**: identifikuje hello trend hello modely, umístění, podporovat jeho podmínky a čas hello roku toogain Statistika paliva efektivní řízení vzory.</span><span class="sxs-lookup"><span data-stu-id="46456-373">**Fuel efficient driving behavior**: Identifies hello trend of hello models, locations, driving conditions, and time of hello year toogain insights on fuel efficient driving patterns.</span></span> <span data-ttu-id="46456-374">Contoso motory pro můžete použít tyto přehledy marketingových kampaní, řídí nové funkce a proaktivní reporting toohello ovladače pro náklady platné a prostředí zvyklosti popisný řízení.</span><span class="sxs-lookup"><span data-stu-id="46456-374">Contoso Motors can use these insights for marketing campaigns, driving new features and proactive reporting toohello drivers for cost effective and environment friendly driving habits.</span></span> 
3. <span data-ttu-id="46456-375">**Odvolat modely**: identifikuje modely nutnosti navrácení podle zprovozňování hello strojové učení detekce anomálií experimentu</span><span class="sxs-lookup"><span data-stu-id="46456-375">**Recall models**: Identifies models requiring recalls by operationalizing hello anomaly detection machine learning experiment</span></span>

<span data-ttu-id="46456-376">Podívejme se na podrobnosti hello jednotlivých tyto metriky</span><span class="sxs-lookup"><span data-stu-id="46456-376">Let's look into hello details of each of these metrics,</span></span>

<span data-ttu-id="46456-377">**Agresivní řízení vzor**</span><span class="sxs-lookup"><span data-stu-id="46456-377">**Aggressive driving pattern**</span></span>

<span data-ttu-id="46456-378">Hello vehicle signály rozdělena na oddíly a diagnostických dat jsou zpracovávány v hello kanál s názvem "AggresiveDrivingPatternPipeline" pomocí Hive toodetermine hello modely, umístění, vehicle, řídí podmínky a dalších parametrů, které jádro vykazuje agresivní řídí vzor.</span><span class="sxs-lookup"><span data-stu-id="46456-378">hello partitioned vehicle signals and diagnostic data are processed in hello pipeline named "AggresiveDrivingPatternPipeline" using Hive toodetermine hello models, location, vehicle, driving conditions, and other parameters that exhibits aggressive driving pattern.</span></span>

<span data-ttu-id="46456-379">![Agresivní řízení pracovního postupu vzor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*obrázek 18 – agresivní řízení pracovního postupu vzor*</span><span class="sxs-lookup"><span data-stu-id="46456-379">![Aggressive driving pattern workflow](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*Figure 18 – Aggressive driving pattern workflow*</span></span>


<span data-ttu-id="46456-380">***Agresivní řízení dotaz Hive vzor***</span><span class="sxs-lookup"><span data-stu-id="46456-380">***Aggressive driving pattern Hive query***</span></span>

<span data-ttu-id="46456-381">Hello skriptu Hive s názvem "aggresivedriving.hql" použít pro analýzu agresivní řízení vzor podmínka je umístěný ve složce "\demo\src\connectedcar\scripts" hello stáhnout zip.</span><span class="sxs-lookup"><span data-stu-id="46456-381">hello Hive script named "aggresivedriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


<span data-ttu-id="46456-382">Používá kombinaci hello vehicle na pozici přenosu ozubené kolečko, brzdy Pedálové stav a rychlost toodetect reckless/agresivní řídí chování v závislosti na brzdění vzor vysokou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="46456-382">It uses hello combination of vehicle's transmission gear position, brake pedal status, and speed toodetect reckless/aggressive driving behavior based on braking pattern at high speed.</span></span> 

<span data-ttu-id="46456-383">Jakmile hello kanálu proveden úspěšně, zobrazí následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar" hello hello.</span><span class="sxs-lookup"><span data-stu-id="46456-383">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![Výstup AggressiveDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

<span data-ttu-id="46456-385">*Obrázek 19 – AggressiveDrivingPatternPipeline výstup*</span><span class="sxs-lookup"><span data-stu-id="46456-385">*Figure 19 – AggressiveDrivingPatternPipeline output*</span></span>

<span data-ttu-id="46456-386">**Efektivní řízení vzor paliva**</span><span class="sxs-lookup"><span data-stu-id="46456-386">**Fuel efficient driving pattern**</span></span>

<span data-ttu-id="46456-387">Hello vehicle signály rozdělena na oddíly a diagnostických dat jsou zpracovány v hello kanál s názvem "FuelEfficientDrivingPatternPipeline".</span><span class="sxs-lookup"><span data-stu-id="46456-387">hello partitioned vehicle signals and diagnostic data are processed in hello pipeline named "FuelEfficientDrivingPatternPipeline".</span></span> <span data-ttu-id="46456-388">Hive je použité toodetermine hello modely, umístění, vehicle, jízdních podmínek a další vlastnosti, které vykazují paliva efektivní řízení vzorem.</span><span class="sxs-lookup"><span data-stu-id="46456-388">Hive is used toodetermine hello models, location, vehicle, driving conditions, and other properties that exhibit fuel efficient driving pattern.</span></span>

![Zvýšení řízení vzor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

<span data-ttu-id="46456-390">*Obrázek 20 – zvýšení řízení pracovního postupu vzor*</span><span class="sxs-lookup"><span data-stu-id="46456-390">*Figure 20 – Fuel-efficient driving pattern workflow*</span></span>

<span data-ttu-id="46456-391">***Paliva efektivní řízení vzor dotaz Hive***</span><span class="sxs-lookup"><span data-stu-id="46456-391">***Fuel efficient driving pattern Hive query***</span></span>

<span data-ttu-id="46456-392">Hello skriptu Hive s názvem "fuelefficientdriving.hql" použít pro analýzu agresivní řízení vzor podmínka je umístěný ve složce "\demo\src\connectedcar\scripts" hello stáhnout zip.</span><span class="sxs-lookup"><span data-stu-id="46456-392">hello Hive script named "fuelefficientdriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


<span data-ttu-id="46456-393">Používá kombinaci hello přenosu ozubené kolečko pozice vehicle na, brzdy Pedálové stav, rychlost a akcelerátoru Pedálové pozice toodetect paliva efektivní řízení chování podle akcelerace, brzdění, a rychlost vzory.</span><span class="sxs-lookup"><span data-stu-id="46456-393">It uses hello combination of vehicle's transmission gear position, brake pedal status, speed, and accelerator pedal position toodetect fuel efficient driving behavior based on acceleration, braking, and speed patterns.</span></span> 

<span data-ttu-id="46456-394">Jakmile hello kanálu proveden úspěšně, zobrazí následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar" hello hello.</span><span class="sxs-lookup"><span data-stu-id="46456-394">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![Výstup FuelEfficientDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

<span data-ttu-id="46456-396">*Obrázek 21 – FuelEfficientDrivingPatternPipeline výstup*</span><span class="sxs-lookup"><span data-stu-id="46456-396">*Figure 21 – FuelEfficientDrivingPatternPipeline output*</span></span>

<span data-ttu-id="46456-397">**Odvolat předpovědi**</span><span class="sxs-lookup"><span data-stu-id="46456-397">**Recall Predictions**</span></span>

<span data-ttu-id="46456-398">Hello experimentu strojového učení je zřízený a publikovat jako webovou službu, jako součást nasazení řešení hello.</span><span class="sxs-lookup"><span data-stu-id="46456-398">hello machine learning experiment is provisioned and published as a web service as part of hello solution deployment.</span></span> <span data-ttu-id="46456-399">v tomto pracovním postupu, zaregistrován jako služba data factory propojené a operationalized pomocí data factory aktivita dávkového vyhodnocování se využívají záznamy Hello dávkového vyhodnocování koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="46456-399">hello batch scoring end point is leveraged in this workflow, registered as a data factory linked service and operationalized using data factory batch scoring activity.</span></span>

![Koncový bod Machine Learning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

<span data-ttu-id="46456-401">*Obrázek 22 – strojového učení koncový bod registrován jako propojené služby v datové továrně*</span><span class="sxs-lookup"><span data-stu-id="46456-401">*Figure 22 – Machine learning endpoint registered as a linked service in data factory*</span></span>

<span data-ttu-id="46456-402">Hello registrovanou propojenou službu se používá v hello DetectAnomalyPipeline tooscore hello data pomocí modelu detekce anomálií hello.</span><span class="sxs-lookup"><span data-stu-id="46456-402">hello registered linked service is used in hello DetectAnomalyPipeline tooscore hello data using hello anomaly detection model.</span></span> 

![Počítač Learning aktivita dávkového vyhodnocování v datové továrně](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

<span data-ttu-id="46456-404">*Obrázek 23 – Azure Machine Learning dávkového vyhodnocování aktivity v objektu pro vytváření dat*</span><span class="sxs-lookup"><span data-stu-id="46456-404">*Figure 23 – Azure Machine Learning Batch Scoring activity in data factory*</span></span> 

<span data-ttu-id="46456-405">Je několik kroků provést u tohoto kanálu pro přípravu dat tak, aby může být operationalized s hello dávkového vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="46456-405">There are few steps performed in this pipeline for data preparation so that it can be operationalized with hello batch scoring web service.</span></span> 

![DetectAnomalyPipeline pro predikci vozidel nutnosti navrácení](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

<span data-ttu-id="46456-407">*Obrázek 24 – DetectAnomalyPipeline pro predikci vozidel nutnosti navrácení*</span><span class="sxs-lookup"><span data-stu-id="46456-407">*Figure 24 – DetectAnomalyPipeline for predicting vehicles requiring recalls*</span></span> 

<span data-ttu-id="46456-408">***Dotaz Hive detekce anomálií***</span><span class="sxs-lookup"><span data-stu-id="46456-408">***Anomaly detection Hive query***</span></span>

<span data-ttu-id="46456-409">Po dokončení hello vyhodnocování aktivita HDInsight je použité tooprocess a hello agregovaná data, která jsou klasifikovány jako anomálií modelem hello se skóre pravděpodobnosti 0,60 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="46456-409">Once hello scoring is completed, an HDInsight activity is used tooprocess and aggregate hello data that are categorized as anomalies by hello model with a probability score of 0.60 or higher.</span></span>

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


<span data-ttu-id="46456-410">Jakmile hello kanálu proveden úspěšně, zobrazí následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar" hello hello.</span><span class="sxs-lookup"><span data-stu-id="46456-410">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![Výstup DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

<span data-ttu-id="46456-412">*Obrázek 25 – DetectAnomalyPipeline výstup*</span><span class="sxs-lookup"><span data-stu-id="46456-412">*Figure 25 – DetectAnomalyPipeline output*</span></span>

## <a name="publish"></a><span data-ttu-id="46456-413">Publikování</span><span class="sxs-lookup"><span data-stu-id="46456-413">Publish</span></span>

### <a name="real-time-analysis"></a><span data-ttu-id="46456-414">Analýzy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="46456-414">Real-time analysis</span></span>
<span data-ttu-id="46456-415">Jeden z hello dotazy v úloze Stream Analytics hello publikuje hello události tooan výstup instance centra událostí.</span><span class="sxs-lookup"><span data-stu-id="46456-415">One of hello queries in hello Stream Analytics job publishes hello events tooan output Event Hub instance.</span></span> 

![Úlohy Stream Analytics publikuje tooan výstup instance centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

<span data-ttu-id="46456-417">*Obrázek 26 – úloha Stream Analytics publikuje tooan výstup instance centra událostí*</span><span class="sxs-lookup"><span data-stu-id="46456-417">*Figure 26 – Stream Analytics job publishes tooan output Event Hub instance*</span></span>

![Stream Analytics query toopublish toohello výstup instance centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

<span data-ttu-id="46456-419">*Obrázek 27 – Stream Analytics query toopublish toohello výstup instance centra událostí*</span><span class="sxs-lookup"><span data-stu-id="46456-419">*Figure 27 – Stream Analytics query toopublish toohello output Event Hub instance*</span></span>

<span data-ttu-id="46456-420">Tento datový proud událostí, které se spotřebovávají hello RealTimeDashboardApp součástí hello řešení.</span><span class="sxs-lookup"><span data-stu-id="46456-420">This stream of events is consumed by hello RealTimeDashboardApp included in hello solution.</span></span> <span data-ttu-id="46456-421">Tato aplikace využívá hello Machine Learning požadavků a odpovědí webové služby pro vyhodnocování v reálném čase a publikuje hello datovou sadu Power BI tooa Výsledná data pro používání.</span><span class="sxs-lookup"><span data-stu-id="46456-421">This application leverages hello Machine Learning Request-Response web service for real-time scoring and publishes hello resultant data tooa Power BI dataset for consumption.</span></span> 

### <a name="batch-analysis"></a><span data-ttu-id="46456-422">Dávková analýza</span><span class="sxs-lookup"><span data-stu-id="46456-422">Batch analysis</span></span>
<span data-ttu-id="46456-423">výsledky Hello hello batch a v reálném čase zpracování jsou publikované toohello tabulky Azure SQL Database pro používání.</span><span class="sxs-lookup"><span data-stu-id="46456-423">hello results of hello batch and real-time processing are published toohello Azure SQL Database tables for consumption.</span></span> <span data-ttu-id="46456-424">Hello Azure SQL Server, databáze a tabulky hello se vytvoří automaticky v rámci hello instalační skript.</span><span class="sxs-lookup"><span data-stu-id="46456-424">hello Azure SQL Server, Database, and hello tables are created automatically as part of hello setup script.</span></span> 

![Výsledky zpracování dávky zkopírujte toodata Tržiště pracovního postupu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

<span data-ttu-id="46456-426">*Obrázek 28 – dávkového zpracování výsledků zkopírování toodata Tržiště pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="46456-426">*Figure 28 – Batch processing results copy toodata mart workflow*</span></span>

![Úlohy Stream Analytics publikuje toodata Tržiště](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

<span data-ttu-id="46456-428">*Obrázek 29 – úloha Stream Analytics publikuje toodata Tržiště*</span><span class="sxs-lookup"><span data-stu-id="46456-428">*Figure 29 – Stream Analytics job publishes toodata mart*</span></span>

![Datové Tržiště nastavení v úloze Stream Analytics](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

<span data-ttu-id="46456-430">*Obrázek 30 – datové Tržiště nastavení v úloze Stream Analytics*</span><span class="sxs-lookup"><span data-stu-id="46456-430">*Figure 30 – Data mart setting in Stream Analytics job*</span></span>

## <a name="consume"></a><span data-ttu-id="46456-431">Konzumace</span><span class="sxs-lookup"><span data-stu-id="46456-431">Consume</span></span>
<span data-ttu-id="46456-432">Power BI poskytuje toto řešení bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="46456-432">Power BI gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="46456-433">Kliknutím sem získáte podrobné pokyny k nastavení hello Power BI sestavy a řídicí panel hello.</span><span class="sxs-lookup"><span data-stu-id="46456-433">Click here for detailed instructions on setting up hello Power BI reports and hello dashboard.</span></span> <span data-ttu-id="46456-434">řídicí panel konečné Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="46456-434">hello final dashboard looks like this:</span></span>

![Řídicí panel Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

<span data-ttu-id="46456-436">*Obrázek 31 - řídicí panel Power BI*</span><span class="sxs-lookup"><span data-stu-id="46456-436">*Figure 31 - Power BI Dashboard*</span></span>

## <a name="summary"></a><span data-ttu-id="46456-437">Souhrn</span><span class="sxs-lookup"><span data-stu-id="46456-437">Summary</span></span>
<span data-ttu-id="46456-438">Tento dokument obsahuje podrobné procházení Dobrý den řešení analýzy Vehicle Telemetrie.</span><span class="sxs-lookup"><span data-stu-id="46456-438">This document contains a detailed drill-down of hello Vehicle Telemetry Analytics Solution.</span></span> <span data-ttu-id="46456-439">To umožňující prezentovat vzor architektura lambda v reálném čase a dávky analytics s předpovědi a akce.</span><span class="sxs-lookup"><span data-stu-id="46456-439">This showcases a lambda architecture pattern for real-time and batch analytics with predictions and actions.</span></span> <span data-ttu-id="46456-440">Tento vzor platí tooa širokou škálu případy použití, které vyžadují aktivní cesta (v reálném čase) a analýzy neaktivní trase (batch).</span><span class="sxs-lookup"><span data-stu-id="46456-440">This pattern applies tooa wide range of use cases that require hot path (real-time) and cold path (batch) analytics.</span></span> 

