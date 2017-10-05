---
title: "Podrobné informace do předpovědi vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti Cortana Intelligence získat přehledy v reálném čase a prediktivní na vehicle stavu a řídí zvyklosti."
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
ms.openlocfilehash: 0a4dba58445cf0fd9fd8f51d443576bacd92251b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a><span data-ttu-id="4667c-103">Scénář řešení analýzy telemetrie vozidla: podrobné informace o řešení</span><span class="sxs-lookup"><span data-stu-id="4667c-103">Vehicle telemetry analytics solution playbook: deep dive into the solution</span></span>
<span data-ttu-id="4667c-104">To **nabídky** odkazy na části této playbook:</span><span class="sxs-lookup"><span data-stu-id="4667c-104">This **menu** links to the sections of this playbook:</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="4667c-105">Tato část podrobněji zanalyzuje data do každé z fází znázorněný v architektuře řešení s pokyny a ukazatele pro přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="4667c-105">This section drills down into each of the stages depicted in the Solution Architecture with instructions and pointers for customization.</span></span> 

## <a name="data-sources"></a><span data-ttu-id="4667c-106">Zdroje dat</span><span class="sxs-lookup"><span data-stu-id="4667c-106">Data Sources</span></span>
<span data-ttu-id="4667c-107">Toto řešení využívá dva různé datové zdroje:</span><span class="sxs-lookup"><span data-stu-id="4667c-107">The solution uses two different data sources:</span></span>

* <span data-ttu-id="4667c-108">**simulated vehicle signály a diagnostiky datovou sadu** a</span><span class="sxs-lookup"><span data-stu-id="4667c-108">**simulated vehicle signals and diagnostic dataset** and</span></span> 
* <span data-ttu-id="4667c-109">**vehicle katalogu**</span><span class="sxs-lookup"><span data-stu-id="4667c-109">**vehicle catalog**</span></span>

<span data-ttu-id="4667c-110">Vehicle telematika jsme je součástí tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="4667c-110">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="4667c-111">Vysílá diagnostické informace a signály odpovídající stavu nástroj a podporovat jeho vzor k danému bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="4667c-111">It emits diagnostic information and signals corresponding to the state of the vehicle and to the driving pattern at a given point in time.</span></span> <span data-ttu-id="4667c-112">Klikněte na tlačítko [Vehicle telematika simulátoru](http://go.microsoft.com/fwlink/?LinkId=717075) ke stažení **Vehicle telematika simulátoru Visual Studio řešení** přizpůsobení podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="4667c-112">Click [Vehicle Telematics Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) to download the **Vehicle Telematics Simulator Visual Studio Solution** for customizations based on your requirements.</span></span> <span data-ttu-id="4667c-113">Katalogu vehicle obsahuje odkaz na datovou sadu s VIN pro mapování modelu.</span><span class="sxs-lookup"><span data-stu-id="4667c-113">The vehicle catalog contains a reference dataset with a VIN to model mapping.</span></span>

![Vehicle telematika simulátoru](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

<span data-ttu-id="4667c-115">*Obrázek 1 – Vehicle telematika simulátoru*</span><span class="sxs-lookup"><span data-stu-id="4667c-115">*Figure 1 – Vehicle Telematics Simulator*</span></span>

<span data-ttu-id="4667c-116">Toto je datová sada formátu JSON, který obsahuje následující schéma.</span><span class="sxs-lookup"><span data-stu-id="4667c-116">This is a JSON-formatted dataset that contains the following schema.</span></span>

| <span data-ttu-id="4667c-117">Sloupec</span><span class="sxs-lookup"><span data-stu-id="4667c-117">Column</span></span> | <span data-ttu-id="4667c-118">Popis</span><span class="sxs-lookup"><span data-stu-id="4667c-118">Description</span></span> | <span data-ttu-id="4667c-119">Hodnoty</span><span class="sxs-lookup"><span data-stu-id="4667c-119">Values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4667c-120">VIN</span><span class="sxs-lookup"><span data-stu-id="4667c-120">VIN</span></span> |<span data-ttu-id="4667c-121">Náhodně generované identifikační číslo</span><span class="sxs-lookup"><span data-stu-id="4667c-121">Randomly generated Vehicle Identification Number</span></span> |<span data-ttu-id="4667c-122">To se získávají z hlavní seznam 10 000 náhodně generované vehicle identifikačními čísly.</span><span class="sxs-lookup"><span data-stu-id="4667c-122">This is obtained from a master list of 10,000 randomly generated vehicle identification numbers.</span></span> |
| <span data-ttu-id="4667c-123">Mimo teploty</span><span class="sxs-lookup"><span data-stu-id="4667c-123">Outside temperature</span></span> |<span data-ttu-id="4667c-124">Mimo teploty, kde je nástroj Řízení</span><span class="sxs-lookup"><span data-stu-id="4667c-124">The outside temperature where the vehicle is driving</span></span> |<span data-ttu-id="4667c-125">Náhodně generované číslo od 0 – 100</span><span class="sxs-lookup"><span data-stu-id="4667c-125">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="4667c-126">Modul teploty</span><span class="sxs-lookup"><span data-stu-id="4667c-126">Engine temperature</span></span> |<span data-ttu-id="4667c-127">Modul teplota nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-127">The engine temperature of the vehicle</span></span> |<span data-ttu-id="4667c-128">Náhodně generované číslo od 0-500</span><span class="sxs-lookup"><span data-stu-id="4667c-128">Randomly generated number from 0-500</span></span> |
| <span data-ttu-id="4667c-129">Rychlost</span><span class="sxs-lookup"><span data-stu-id="4667c-129">Speed</span></span> |<span data-ttu-id="4667c-130">Modul rychlost, jakou se řídí nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-130">The engine speed at which the vehicle is driving</span></span> |<span data-ttu-id="4667c-131">Náhodně generované číslo od 0 – 100</span><span class="sxs-lookup"><span data-stu-id="4667c-131">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="4667c-132">Paliva</span><span class="sxs-lookup"><span data-stu-id="4667c-132">Fuel</span></span> |<span data-ttu-id="4667c-133">Úroveň paliva nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-133">The fuel level of the vehicle</span></span> |<span data-ttu-id="4667c-134">Náhodně generované číslo od 0 až 100 (určuje procento úrovně paliva)</span><span class="sxs-lookup"><span data-stu-id="4667c-134">Randomly generated number from 0-100 (indicates fuel level percentage)</span></span> |
| <span data-ttu-id="4667c-135">EngineOil</span><span class="sxs-lookup"><span data-stu-id="4667c-135">EngineOil</span></span> |<span data-ttu-id="4667c-136">Úroveň modulu těžba ropy nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-136">The engine oil level of the vehicle</span></span> |<span data-ttu-id="4667c-137">Náhodně generované číslo od 0 až 100 (určuje procento úrovně těžba ropy modul)</span><span class="sxs-lookup"><span data-stu-id="4667c-137">Randomly generated number from 0-100 (indicates engine oil level percentage)</span></span> |
| <span data-ttu-id="4667c-138">Můžete zadat přetížení</span><span class="sxs-lookup"><span data-stu-id="4667c-138">Tire pressure</span></span> |<span data-ttu-id="4667c-139">Můžete zadat tlak nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-139">The tire pressure of the vehicle</span></span> |<span data-ttu-id="4667c-140">Náhodně generované číslo od 0 – 50 (určuje procento úrovně zatížení můžete zadat)</span><span class="sxs-lookup"><span data-stu-id="4667c-140">Randomly generated number from 0-50 (indicates tire pressure level percentage)</span></span> |
| <span data-ttu-id="4667c-141">Počítadlo kilometrů</span><span class="sxs-lookup"><span data-stu-id="4667c-141">Odometer</span></span> |<span data-ttu-id="4667c-142">Čtení počítadlo kilometrů nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-142">The odometer reading of the vehicle</span></span> |<span data-ttu-id="4667c-143">Náhodně generované číslo od 0 200000</span><span class="sxs-lookup"><span data-stu-id="4667c-143">Randomly generated number from 0-200000</span></span> |
| <span data-ttu-id="4667c-144">Accelerator_pedal_position</span><span class="sxs-lookup"><span data-stu-id="4667c-144">Accelerator_pedal_position</span></span> |<span data-ttu-id="4667c-145">Nástroj Pedálové pozici akcelerátoru</span><span class="sxs-lookup"><span data-stu-id="4667c-145">The accelerator pedal position of the vehicle</span></span> |<span data-ttu-id="4667c-146">Náhodně generované číslo od 0 až 100 (určuje procento úrovně akcelerátoru)</span><span class="sxs-lookup"><span data-stu-id="4667c-146">Randomly generated number from 0-100 (indicates accelerator level percentage)</span></span> |
| <span data-ttu-id="4667c-147">Parking_brake_status</span><span class="sxs-lookup"><span data-stu-id="4667c-147">Parking_brake_status</span></span> |<span data-ttu-id="4667c-148">Určuje, zda nástroj zaparkováno nebo ne</span><span class="sxs-lookup"><span data-stu-id="4667c-148">Indicates whether the vehicle is parked or not</span></span> |<span data-ttu-id="4667c-149">True nebo False</span><span class="sxs-lookup"><span data-stu-id="4667c-149">True or False</span></span> |
| <span data-ttu-id="4667c-150">Headlamp_status</span><span class="sxs-lookup"><span data-stu-id="4667c-150">Headlamp_status</span></span> |<span data-ttu-id="4667c-151">Určuje, kde je světlometu na nebo ne</span><span class="sxs-lookup"><span data-stu-id="4667c-151">Indicates where the headlamp is on or not</span></span> |<span data-ttu-id="4667c-152">True nebo False</span><span class="sxs-lookup"><span data-stu-id="4667c-152">True or False</span></span> |
| <span data-ttu-id="4667c-153">Brake_pedal_status</span><span class="sxs-lookup"><span data-stu-id="4667c-153">Brake_pedal_status</span></span> |<span data-ttu-id="4667c-154">Určuje, zda je nebo není stisknuta brzdového pedálu</span><span class="sxs-lookup"><span data-stu-id="4667c-154">Indicates whether the brake pedal is pressed or not</span></span> |<span data-ttu-id="4667c-155">True nebo False</span><span class="sxs-lookup"><span data-stu-id="4667c-155">True or False</span></span> |
| <span data-ttu-id="4667c-156">Transmission_gear_position</span><span class="sxs-lookup"><span data-stu-id="4667c-156">Transmission_gear_position</span></span> |<span data-ttu-id="4667c-157">Pozice ozubené kolečko přenosu nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-157">The transmission gear position of the vehicle</span></span> |<span data-ttu-id="4667c-158">Stavy: první, druhá, třetí a čtvrté, páté, šestého, sedmá, osmého</span><span class="sxs-lookup"><span data-stu-id="4667c-158">States: first, second, third, fourth, fifth, sixth, seventh, eighth</span></span> |
| <span data-ttu-id="4667c-159">Ignition_status</span><span class="sxs-lookup"><span data-stu-id="4667c-159">Ignition_status</span></span> |<span data-ttu-id="4667c-160">Určuje, zda je spuštěná nebo zastavená nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-160">Indicates whether the vehicle is running or stopped</span></span> |<span data-ttu-id="4667c-161">True nebo False</span><span class="sxs-lookup"><span data-stu-id="4667c-161">True or False</span></span> |
| <span data-ttu-id="4667c-162">Windshield_wiper_status</span><span class="sxs-lookup"><span data-stu-id="4667c-162">Windshield_wiper_status</span></span> |<span data-ttu-id="4667c-163">Určuje, zda stírání čelního skla zapnutý, nebo ne</span><span class="sxs-lookup"><span data-stu-id="4667c-163">Indicates whether the windshield wiper is turned or not</span></span> |<span data-ttu-id="4667c-164">True nebo False</span><span class="sxs-lookup"><span data-stu-id="4667c-164">True or False</span></span> |
| <span data-ttu-id="4667c-165">ABS</span><span class="sxs-lookup"><span data-stu-id="4667c-165">ABS</span></span> |<span data-ttu-id="4667c-166">Určuje, zda je nebo není zařazen ABS</span><span class="sxs-lookup"><span data-stu-id="4667c-166">Indicates whether ABS is engaged or not</span></span> |<span data-ttu-id="4667c-167">True nebo False</span><span class="sxs-lookup"><span data-stu-id="4667c-167">True or False</span></span> |
| <span data-ttu-id="4667c-168">časové razítko</span><span class="sxs-lookup"><span data-stu-id="4667c-168">Timestamp</span></span> |<span data-ttu-id="4667c-169">Časové razítko vytvoření datového bodu</span><span class="sxs-lookup"><span data-stu-id="4667c-169">The timestamp when the data point is created</span></span> |<span data-ttu-id="4667c-170">Datum</span><span class="sxs-lookup"><span data-stu-id="4667c-170">Date</span></span> |
| <span data-ttu-id="4667c-171">Město</span><span class="sxs-lookup"><span data-stu-id="4667c-171">City</span></span> |<span data-ttu-id="4667c-172">Umístění nástroj</span><span class="sxs-lookup"><span data-stu-id="4667c-172">The location of the vehicle</span></span> |<span data-ttu-id="4667c-173">4 města v tomto řešení: Bellevue, Redmond, Sammamish, Praha</span><span class="sxs-lookup"><span data-stu-id="4667c-173">4 cities in this solution: Bellevue, Redmond, Sammamish, Seattle</span></span> |

<span data-ttu-id="4667c-174">Vehicle modelu referenční datová sada obsahuje VIN mapování modelu.</span><span class="sxs-lookup"><span data-stu-id="4667c-174">The vehicle model reference dataset contains VIN to the model mapping.</span></span> 

| <span data-ttu-id="4667c-175">VIN</span><span class="sxs-lookup"><span data-stu-id="4667c-175">VIN</span></span> | <span data-ttu-id="4667c-176">Model</span><span class="sxs-lookup"><span data-stu-id="4667c-176">Model</span></span> |
| --- | --- |
| <span data-ttu-id="4667c-177">FHL3O1SA4IEHB4WU1</span><span class="sxs-lookup"><span data-stu-id="4667c-177">FHL3O1SA4IEHB4WU1</span></span> |<span data-ttu-id="4667c-178">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="4667c-178">Sedan</span></span> |
| <span data-ttu-id="4667c-179">8J0U8XCPRGW4Z3NQE</span><span class="sxs-lookup"><span data-stu-id="4667c-179">8J0U8XCPRGW4Z3NQE</span></span> |<span data-ttu-id="4667c-180">Hybridní</span><span class="sxs-lookup"><span data-stu-id="4667c-180">Hybrid</span></span> |
| <span data-ttu-id="4667c-181">WORG68Z2PLTNZDBI7</span><span class="sxs-lookup"><span data-stu-id="4667c-181">WORG68Z2PLTNZDBI7</span></span> |<span data-ttu-id="4667c-182">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="4667c-182">Family Saloon</span></span> |
| <span data-ttu-id="4667c-183">JTHMYHQTEPP4WBMRN</span><span class="sxs-lookup"><span data-stu-id="4667c-183">JTHMYHQTEPP4WBMRN</span></span> |<span data-ttu-id="4667c-184">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="4667c-184">Sedan</span></span> |
| <span data-ttu-id="4667c-185">W9FTHG27LZN1YWO0Y</span><span class="sxs-lookup"><span data-stu-id="4667c-185">W9FTHG27LZN1YWO0Y</span></span> |<span data-ttu-id="4667c-186">Hybridní</span><span class="sxs-lookup"><span data-stu-id="4667c-186">Hybrid</span></span> |
| <span data-ttu-id="4667c-187">MHTP9N792PHK08WJM</span><span class="sxs-lookup"><span data-stu-id="4667c-187">MHTP9N792PHK08WJM</span></span> |<span data-ttu-id="4667c-188">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="4667c-188">Family Saloon</span></span> |
| <span data-ttu-id="4667c-189">EI4QXI2AXVQQING4I</span><span class="sxs-lookup"><span data-stu-id="4667c-189">EI4QXI2AXVQQING4I</span></span> |<span data-ttu-id="4667c-190">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="4667c-190">Sedan</span></span> |
| <span data-ttu-id="4667c-191">5KKR2VB4WHQH97PF8</span><span class="sxs-lookup"><span data-stu-id="4667c-191">5KKR2VB4WHQH97PF8</span></span> |<span data-ttu-id="4667c-192">Hybridní</span><span class="sxs-lookup"><span data-stu-id="4667c-192">Hybrid</span></span> |
| <span data-ttu-id="4667c-193">W9NSZ423XZHAONYXB</span><span class="sxs-lookup"><span data-stu-id="4667c-193">W9NSZ423XZHAONYXB</span></span> |<span data-ttu-id="4667c-194">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="4667c-194">Family Saloon</span></span> |
| <span data-ttu-id="4667c-195">26WJSGHX4MA5ROHNL</span><span class="sxs-lookup"><span data-stu-id="4667c-195">26WJSGHX4MA5ROHNL</span></span> |<span data-ttu-id="4667c-196">Převoditelné</span><span class="sxs-lookup"><span data-stu-id="4667c-196">Convertible</span></span> |
| <span data-ttu-id="4667c-197">GHLUB6ONKMOSI7E77</span><span class="sxs-lookup"><span data-stu-id="4667c-197">GHLUB6ONKMOSI7E77</span></span> |<span data-ttu-id="4667c-198">Kombi</span><span class="sxs-lookup"><span data-stu-id="4667c-198">Station Wagon</span></span> |
| <span data-ttu-id="4667c-199">9C2RHVRVLMEJDBXLP</span><span class="sxs-lookup"><span data-stu-id="4667c-199">9C2RHVRVLMEJDBXLP</span></span> |<span data-ttu-id="4667c-200">Compact Car</span><span class="sxs-lookup"><span data-stu-id="4667c-200">Compact Car</span></span> |
| <span data-ttu-id="4667c-201">BRNHVMZOUJ6EOCP32</span><span class="sxs-lookup"><span data-stu-id="4667c-201">BRNHVMZOUJ6EOCP32</span></span> |<span data-ttu-id="4667c-202">Malé SUV</span><span class="sxs-lookup"><span data-stu-id="4667c-202">Small SUV</span></span> |
| <span data-ttu-id="4667c-203">VCYVW0WUZNBTM594J</span><span class="sxs-lookup"><span data-stu-id="4667c-203">VCYVW0WUZNBTM594J</span></span> |<span data-ttu-id="4667c-204">Auto sportu</span><span class="sxs-lookup"><span data-stu-id="4667c-204">Sports Car</span></span> |
| <span data-ttu-id="4667c-205">HNVCE6YFZSA5M82NY</span><span class="sxs-lookup"><span data-stu-id="4667c-205">HNVCE6YFZSA5M82NY</span></span> |<span data-ttu-id="4667c-206">Střední SUV</span><span class="sxs-lookup"><span data-stu-id="4667c-206">Medium SUV</span></span> |
| <span data-ttu-id="4667c-207">4R30FOR7NUOBL05GJ</span><span class="sxs-lookup"><span data-stu-id="4667c-207">4R30FOR7NUOBL05GJ</span></span> |<span data-ttu-id="4667c-208">Kombi</span><span class="sxs-lookup"><span data-stu-id="4667c-208">Station Wagon</span></span> |
| <span data-ttu-id="4667c-209">WYNIIY42VKV6OQS1J</span><span class="sxs-lookup"><span data-stu-id="4667c-209">WYNIIY42VKV6OQS1J</span></span> |<span data-ttu-id="4667c-210">Velké SUV</span><span class="sxs-lookup"><span data-stu-id="4667c-210">Large SUV</span></span> |
| <span data-ttu-id="4667c-211">8Y5QKG27QET1RBK7I</span><span class="sxs-lookup"><span data-stu-id="4667c-211">8Y5QKG27QET1RBK7I</span></span> |<span data-ttu-id="4667c-212">Velké SUV</span><span class="sxs-lookup"><span data-stu-id="4667c-212">Large SUV</span></span> |
| <span data-ttu-id="4667c-213">DF6OX2WSRA6511BVG</span><span class="sxs-lookup"><span data-stu-id="4667c-213">DF6OX2WSRA6511BVG</span></span> |<span data-ttu-id="4667c-214">Kupé</span><span class="sxs-lookup"><span data-stu-id="4667c-214">Coupe</span></span> |
| <span data-ttu-id="4667c-215">Z2EOZWZBXAEW3E60T</span><span class="sxs-lookup"><span data-stu-id="4667c-215">Z2EOZWZBXAEW3E60T</span></span> |<span data-ttu-id="4667c-216">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="4667c-216">Sedan</span></span> |
| <span data-ttu-id="4667c-217">M4TV6IEALD5QDS3IR</span><span class="sxs-lookup"><span data-stu-id="4667c-217">M4TV6IEALD5QDS3IR</span></span> |<span data-ttu-id="4667c-218">Hybridní</span><span class="sxs-lookup"><span data-stu-id="4667c-218">Hybrid</span></span> |
| <span data-ttu-id="4667c-219">VHRA1Y2TGTA84F00H</span><span class="sxs-lookup"><span data-stu-id="4667c-219">VHRA1Y2TGTA84F00H</span></span> |<span data-ttu-id="4667c-220">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="4667c-220">Family Saloon</span></span> |
| <span data-ttu-id="4667c-221">R0JAUHT1L1R3BIKI0</span><span class="sxs-lookup"><span data-stu-id="4667c-221">R0JAUHT1L1R3BIKI0</span></span> |<span data-ttu-id="4667c-222">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="4667c-222">Sedan</span></span> |
| <span data-ttu-id="4667c-223">9230C202Z60XX84AU</span><span class="sxs-lookup"><span data-stu-id="4667c-223">9230C202Z60XX84AU</span></span> |<span data-ttu-id="4667c-224">Hybridní</span><span class="sxs-lookup"><span data-stu-id="4667c-224">Hybrid</span></span> |
| <span data-ttu-id="4667c-225">T8DNDN5UDCWL7M72H</span><span class="sxs-lookup"><span data-stu-id="4667c-225">T8DNDN5UDCWL7M72H</span></span> |<span data-ttu-id="4667c-226">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="4667c-226">Family Saloon</span></span> |
| <span data-ttu-id="4667c-227">4WPYRUZII5YV7YA42</span><span class="sxs-lookup"><span data-stu-id="4667c-227">4WPYRUZII5YV7YA42</span></span> |<span data-ttu-id="4667c-228">Limuzíny</span><span class="sxs-lookup"><span data-stu-id="4667c-228">Sedan</span></span> |
| <span data-ttu-id="4667c-229">D1ZVY26UV2BFGHZNO</span><span class="sxs-lookup"><span data-stu-id="4667c-229">D1ZVY26UV2BFGHZNO</span></span> |<span data-ttu-id="4667c-230">Hybridní</span><span class="sxs-lookup"><span data-stu-id="4667c-230">Hybrid</span></span> |
| <span data-ttu-id="4667c-231">XUF99EW9OIQOMV7Q7</span><span class="sxs-lookup"><span data-stu-id="4667c-231">XUF99EW9OIQOMV7Q7</span></span> |<span data-ttu-id="4667c-232">Rodiny Sedan</span><span class="sxs-lookup"><span data-stu-id="4667c-232">Family Saloon</span></span> |
| <span data-ttu-id="4667c-233">8OMCL3LGI7XNCC21U</span><span class="sxs-lookup"><span data-stu-id="4667c-233">8OMCL3LGI7XNCC21U</span></span> |<span data-ttu-id="4667c-234">Převoditelné</span><span class="sxs-lookup"><span data-stu-id="4667c-234">Convertible</span></span> |
| <span data-ttu-id="4667c-235">…….</span><span class="sxs-lookup"><span data-stu-id="4667c-235">…….</span></span> | |

### <a name="references"></a><span data-ttu-id="4667c-236">Odkazy</span><span class="sxs-lookup"><span data-stu-id="4667c-236">References</span></span>
[<span data-ttu-id="4667c-237">Řešení vehicle telematika simulátoru sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4667c-237">Vehicle Telematics Simulator Visual Studio Solution</span></span>](http://go.microsoft.com/fwlink/?LinkId=717075) 

[<span data-ttu-id="4667c-238">Centra událostí Azure</span><span class="sxs-lookup"><span data-stu-id="4667c-238">Azure Event Hub</span></span>](https://azure.microsoft.com/services/event-hubs/)

[<span data-ttu-id="4667c-239">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4667c-239">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a><span data-ttu-id="4667c-240">Přijímání</span><span class="sxs-lookup"><span data-stu-id="4667c-240">Ingestion</span></span>
<span data-ttu-id="4667c-241">Kombinace Azure Event Hubs, Stream Analytics a objektu pro vytváření dat jsou využity k ingestování signály vehicle, diagnostických událostí a v reálném čase a analýza služby batch.</span><span class="sxs-lookup"><span data-stu-id="4667c-241">Combinations of Azure Event Hubs, Stream Analytics, and Data Factory are leveraged to ingest the vehicle signals, the diagnostic events, and real-time and batch analytics.</span></span> <span data-ttu-id="4667c-242">Všechny tyto součásti vytvoříte a nakonfigurujete jako součást nasazení řešení.</span><span class="sxs-lookup"><span data-stu-id="4667c-242">All these components are created and configured as part of the solution deployment.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="4667c-243">Analýzy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="4667c-243">Real-time analysis</span></span>
<span data-ttu-id="4667c-244">Události vygenerované pomocí simulátoru telematika Vehicle jsou publikovány do centra událostí pomocí sady SDK centra událostí.</span><span class="sxs-lookup"><span data-stu-id="4667c-244">The events generated by the Vehicle Telematics Simulator are published to the Event Hub using the Event Hub SDK.</span></span> <span data-ttu-id="4667c-245">Úloha Stream Analytics ingestuje tyto události z centra událostí a zpracovává data v reálném čase analyzovat vehicle stavu.</span><span class="sxs-lookup"><span data-stu-id="4667c-245">The Stream Analytics job ingests these events from the Event Hub and processes the data in real time to analyze the vehicle health.</span></span> 

![Řídicí panel centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

<span data-ttu-id="4667c-247">*Obrázek 4 – řídicí panel centra událostí*</span><span class="sxs-lookup"><span data-stu-id="4667c-247">*Figure 4 - Event Hub dashboard*</span></span>

![Stream Analytics úlohy zpracování dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

<span data-ttu-id="4667c-249">*Obrázek 5 – úloha Stream Analytics zpracování dat*</span><span class="sxs-lookup"><span data-stu-id="4667c-249">*Figure 5 - Stream Analytics job processing data*</span></span>

<span data-ttu-id="4667c-250">Úloha Stream Analytics;</span><span class="sxs-lookup"><span data-stu-id="4667c-250">The Stream Analytics job;</span></span>

* <span data-ttu-id="4667c-251">ingestuje dat z centra událostí</span><span class="sxs-lookup"><span data-stu-id="4667c-251">ingests data from the Event Hub</span></span> 
* <span data-ttu-id="4667c-252">provede připojení k u referenčních dat pro mapování vehicle VIN odpovídající modelu</span><span class="sxs-lookup"><span data-stu-id="4667c-252">performs a join with the reference data to map the vehicle VIN to the corresponding model</span></span> 
* <span data-ttu-id="4667c-253">dál je do Azure blob storage analytics bohaté batch.</span><span class="sxs-lookup"><span data-stu-id="4667c-253">persists them into Azure blob storage for rich batch analytics.</span></span> 

<span data-ttu-id="4667c-254">Následující dotaz služby Stream Analytics se používá k uchování dat do úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="4667c-254">The following Stream Analytics query is used to persist the data into Azure blob storage.</span></span> 

![Dotaz úlohy Stream Analytics přijímání dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

<span data-ttu-id="4667c-256">*Obrázek 6 - Stream Analytics úlohy dotazu pro přijímání dat*</span><span class="sxs-lookup"><span data-stu-id="4667c-256">*Figure 6 - Stream Analytics job query for data ingestion*</span></span>

### <a name="batch-analysis"></a><span data-ttu-id="4667c-257">Dávková analýza</span><span class="sxs-lookup"><span data-stu-id="4667c-257">Batch analysis</span></span>
<span data-ttu-id="4667c-258">Také jsme generují další svazek simulované vehicle signály a diagnostiky datové sady pro širší batch analýzu.</span><span class="sxs-lookup"><span data-stu-id="4667c-258">We are also generating an additional volume of simulated vehicle signals and diagnostic dataset for richer batch analytics.</span></span> <span data-ttu-id="4667c-259">To je potřeba zajistit dobrý reprezentativní datový svazek pro dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="4667c-259">This is required to ensure a good representative data volume for batch processing.</span></span> <span data-ttu-id="4667c-260">Pro tento účel používáme kanál s názvem "PrepareSampleDataPipeline" v pracovním postupu Azure Data Factory ke generování za jeden rok simulované vehicle signály a diagnostiky datové sady.</span><span class="sxs-lookup"><span data-stu-id="4667c-260">For this purpose, we are using a pipeline named "PrepareSampleDataPipeline" in the Azure Data Factory workflow to generate one year's worth of simulated vehicle signals and diagnostic dataset.</span></span> <span data-ttu-id="4667c-261">Klikněte na tlačítko [vlastní aktivity služby Data Factory](http://go.microsoft.com/fwlink/?LinkId=717077) ke stažení pro vytváření dat vlastní DotNet aktivita řešení sady Visual Studio pro přizpůsobení podle požadavků.</span><span class="sxs-lookup"><span data-stu-id="4667c-261">Click [Data Factory custom activity](http://go.microsoft.com/fwlink/?LinkId=717077) to download the Data Factory custom DotNet activity Visual Studio solution for customizations based on your requirements.</span></span> 

![Příprava ukázková data pro dávkové zpracování pracovního postupu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

<span data-ttu-id="4667c-263">*Obrázek 7: Příprava ukázková data pro zpracování pracovní postup služby batch*</span><span class="sxs-lookup"><span data-stu-id="4667c-263">*Figure 7 - Prepare Sample data for batch processing workflow*</span></span>

<span data-ttu-id="4667c-264">Kanál se skládá z vlastní .net ADF aktivity, zobrazit tady:</span><span class="sxs-lookup"><span data-stu-id="4667c-264">The pipeline consists of a custom ADF .Net Activity, show here:</span></span>

![PrepareSampleDataPipeline aktivity](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

<span data-ttu-id="4667c-266">*Obrázek 8 - PrepareSampleDataPipeline*</span><span class="sxs-lookup"><span data-stu-id="4667c-266">*Figure 8 - PrepareSampleDataPipeline*</span></span>

<span data-ttu-id="4667c-267">Jakmile kanálu provede úspěšně a datovou sadu "RawCarEventsTable" je označen jako "Připravený", jeden rok vhodné simulované vehicle signálů a diagnostických dat vytváří.</span><span class="sxs-lookup"><span data-stu-id="4667c-267">Once the pipeline executes successfully and "RawCarEventsTable" dataset is marked "Ready", one-year worth of simulated vehicle signals and diagnostic data are produced.</span></span> <span data-ttu-id="4667c-268">Zobrazí následující složku a soubor vytvořený v účtu úložiště v kontejneru "connectedcar":</span><span class="sxs-lookup"><span data-stu-id="4667c-268">You see the following folder and file created in your storage account under the "connectedcar" container:</span></span>

![Výstup PrepareSampleDataPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

<span data-ttu-id="4667c-270">*Obrázek 9 - PrepareSampleDataPipeline výstup*</span><span class="sxs-lookup"><span data-stu-id="4667c-270">*Figure 9 - PrepareSampleDataPipeline Output*</span></span>

### <a name="references"></a><span data-ttu-id="4667c-271">Odkazy</span><span class="sxs-lookup"><span data-stu-id="4667c-271">References</span></span>
[<span data-ttu-id="4667c-272">Azure SDK centra událostí pro přijímání datového proudu</span><span class="sxs-lookup"><span data-stu-id="4667c-272">Azure Event Hub SDK for stream ingestion</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

<span data-ttu-id="4667c-273">[Možnosti přesun dat Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
[aktivita služby Azure Data Factory DotNet.](../data-factory/data-factory-use-custom-activities.md)</span><span class="sxs-lookup"><span data-stu-id="4667c-273">[Azure Data Factory data movement capabilities](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet Activity](../data-factory/data-factory-use-custom-activities.md)</span></span>

[<span data-ttu-id="4667c-274">Řešení sady visual studio Azure Data Factory DotNet aktivity pro přípravu ukázková data</span><span class="sxs-lookup"><span data-stu-id="4667c-274">Azure Data Factory DotNet activity visual studio solution for preparing sample data</span></span>](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-the-dataset"></a><span data-ttu-id="4667c-275">Oddíl datové sady</span><span class="sxs-lookup"><span data-stu-id="4667c-275">Partition the dataset</span></span>
<span data-ttu-id="4667c-276">Nezpracovaná částečně strukturovaných vehicle signály a diagnostiky datové sady jsou rozděleny do oddílů v rámci přípravy dat do formátu měsíc roku.</span><span class="sxs-lookup"><span data-stu-id="4667c-276">The raw semi-structured vehicle signals and diagnostic dataset are partitioned in the data preparation step into a YEAR/MONTH format.</span></span> <span data-ttu-id="4667c-277">Toto rozdělení do oddílů podporuje efektivnější dotazování a škálovatelné dlouhodobé úložiště povolíte odolnost převzetí z účtu jeden objekt blob na další jako první účet zaplní.</span><span class="sxs-lookup"><span data-stu-id="4667c-277">This partitioning promotes more efficient querying and scalable long-term storage by enabling fault-over from one blob account to the next as the first account fills up.</span></span> 

>[!NOTE] 
><span data-ttu-id="4667c-278">Tento krok v řešení se vztahuje pouze na dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="4667c-278">This step in the solution is applicable only to batch processing.</span></span>

<span data-ttu-id="4667c-279">Vstupní a výstupní data správy dat:</span><span class="sxs-lookup"><span data-stu-id="4667c-279">Input and output data data management:</span></span>

* <span data-ttu-id="4667c-280">**Výstupní data** (s názvem bez přípony *PartitionedCarEventsTable*) se musí na dlouhou dobu jako základní / "rawest" formu data v zákazníka "Data Lake".</span><span class="sxs-lookup"><span data-stu-id="4667c-280">The **output data** (labeled *PartitionedCarEventsTable*) is to be kept for a long period of time as the foundational/"rawest" form of data in the customer's "Data Lake".</span></span> 
* <span data-ttu-id="4667c-281">**Vstupní data** do tohoto kanálu by obvykle ignorována, protože výstupních dat má úplné věrnosti do vstupní – stačí uložené (oddíly) lépe pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="4667c-281">The **input data** to this pipeline would typically be discarded as the output data has full fidelity to the input - it's just stored (partitioned) better for subsequent use.</span></span>

![Pracovní postup události car oddílu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

<span data-ttu-id="4667c-283">*Obrázek 10: pracovní postup události Car oddílu*</span><span class="sxs-lookup"><span data-stu-id="4667c-283">*Figure 10 – Partition Car Events workflow*</span></span>

<span data-ttu-id="4667c-284">Nezpracovaná data je rozdělen na oddíly pomocí aktivitu Hive HDInsight v "PartitionCarEventsPipeline".</span><span class="sxs-lookup"><span data-stu-id="4667c-284">The raw data is partitioned using a Hive HDInsight activity in "PartitionCarEventsPipeline".</span></span> <span data-ttu-id="4667c-285">Ukázková data vygenerované v roce v kroku 1 je rozdělena na oddíly pomocí měsíc roku.</span><span class="sxs-lookup"><span data-stu-id="4667c-285">The sample data generated in step 1 for a year is partitioned by YEAR/MONTH.</span></span> <span data-ttu-id="4667c-286">Oddíly, které se používají ke generování vehicle signály a diagnostických dat pro každý měsíc (celkový počet 12 oddíly) v roce.</span><span class="sxs-lookup"><span data-stu-id="4667c-286">The partitions are used to generate vehicle signals and diagnostic data for each month (total 12 partitions) of a year.</span></span> 

![PartitionCarEventsPipeline aktivity](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

<span data-ttu-id="4667c-288">*Obrázek 11 – PartitionCarEventsPipeline*</span><span class="sxs-lookup"><span data-stu-id="4667c-288">*Figure 11 - PartitionCarEventsPipeline*</span></span>

<span data-ttu-id="4667c-289">***Skript PartitionConnectedCarEvents Hive***</span><span class="sxs-lookup"><span data-stu-id="4667c-289">***PartitionConnectedCarEvents Hive Script***</span></span>

<span data-ttu-id="4667c-290">Následující skript Hive, s názvem "partitioncarevents.hql", se používá pro vytváření oddílů a je umístěn ve složce "\demo\src\connectedcar\scripts" stažený zip.</span><span class="sxs-lookup"><span data-stu-id="4667c-290">The following Hive script, named "partitioncarevents.hql", is used for partitioning and is located in the "\demo\src\connectedcar\scripts" folder of the downloaded zip.</span></span> 
    
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

<span data-ttu-id="4667c-291">Jakmile kanálu proveden úspěšně, zobrazí se následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar".</span><span class="sxs-lookup"><span data-stu-id="4667c-291">Once the pipeline is executed successfully, you see the following partitions generated in your storage account under the "connectedcar" container.</span></span>

![Oddílů výstup](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

<span data-ttu-id="4667c-293">*Obrázek 12 - oddílů výstup*</span><span class="sxs-lookup"><span data-stu-id="4667c-293">*Figure 12 - Partitioned Output*</span></span>

<span data-ttu-id="4667c-294">Data je nyní optimalizovaná, správu a připraveno pro další zpracování a získáte přehled o bohaté batch.</span><span class="sxs-lookup"><span data-stu-id="4667c-294">The data is now optimized, is more manageable and ready for further processing to gain rich batch insights.</span></span> 

## <a name="data-analysis"></a><span data-ttu-id="4667c-295">Analýza dat</span><span class="sxs-lookup"><span data-stu-id="4667c-295">Data Analysis</span></span>
<span data-ttu-id="4667c-296">V této části najdete postup kombinace Azure Stream Analytics, Azure Machine Learning, Azure Data Factory a Azure HDInsight pro bohaté pokročilé analýzy na vehicle stavu a řídí zvyklosti.</span><span class="sxs-lookup"><span data-stu-id="4667c-296">In this section, you see how to combine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory, and Azure HDInsight for rich advanced analytics on vehicle health and driving habits.</span></span> <span data-ttu-id="4667c-297">Existují tři pododdíly tady:</span><span class="sxs-lookup"><span data-stu-id="4667c-297">There are three subsections here:</span></span>

1. <span data-ttu-id="4667c-298">**Strojového učení**: Tato část obsahuje informace o experimentu detekce anomálií, který jsme použili v tomto řešení k předvídání vozidel nutnosti obsluhy údržby a nutnosti navrácení kvůli problémům s zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4667c-298">**Machine Learning**: This subsection contains information on the anomaly detection experiment that we have used in this solution to predict vehicles requiring servicing maintenance and vehicles requiring recalls due to safety issues.</span></span>
2. <span data-ttu-id="4667c-299">**Analýzy v reálném čase**: Tato část obsahuje informace týkající se analýzy v reálném čase pomocí Stream Analytics Query Language a zprovozňování experimentu strojové učení v reálném čase pomocí vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4667c-299">**Real-time analysis**: This subsection contains information regarding the real-time analytics using the Stream Analytics Query Language and operationalizing the machine learning experiment in real time using a custom application.</span></span>
3. <span data-ttu-id="4667c-300">**Batch analysis**: Tato část obsahuje informace týkající se transformace a zpracování dávky dat pomocí Azure HDInsight nebo Azure Machine Learning operationalized službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4667c-300">**Batch analysis**: This subsection contains information regarding the transforming and processing of the batch data using Azure HDInsight and Azure Machine Learning operationalized by Azure Data Factory.</span></span>

### <a name="machine-learning"></a><span data-ttu-id="4667c-301">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4667c-301">Machine Learning</span></span>
<span data-ttu-id="4667c-302">Zde Naším cílem je k předvídání vozidel, které vyžadují údržby nebo odvolání na základě statistik určité stavu.</span><span class="sxs-lookup"><span data-stu-id="4667c-302">Our goal here is to predict the vehicles that require maintenance or recall based on certain heath statistics.</span></span> <span data-ttu-id="4667c-303">Provedeme následující předpoklady</span><span class="sxs-lookup"><span data-stu-id="4667c-303">We make the following assumptions</span></span>

* <span data-ttu-id="4667c-304">Pokud platí jedna z následujících tří podmínek, vyžadují vozidel **obsluhy údržby**:</span><span class="sxs-lookup"><span data-stu-id="4667c-304">If one of the following three conditions are true, the vehicles require **servicing maintenance**:</span></span>
  
  * <span data-ttu-id="4667c-305">Můžete zadat naléhavost je nízká</span><span class="sxs-lookup"><span data-stu-id="4667c-305">Tire pressure is low</span></span>
  * <span data-ttu-id="4667c-306">Modul těžba ropy úroveň je nízká</span><span class="sxs-lookup"><span data-stu-id="4667c-306">Engine oil level is low</span></span>
  * <span data-ttu-id="4667c-307">Modul teploty je vysoká.</span><span class="sxs-lookup"><span data-stu-id="4667c-307">Engine temperature is high</span></span>
* <span data-ttu-id="4667c-308">Pokud jeden z následujících podmínek jsou splněny, může mít vozidel **problém zabezpečení** a vyžadovat **odvolání**:</span><span class="sxs-lookup"><span data-stu-id="4667c-308">If one of the following conditions are true, the vehicles may have a **safety issue** and require **recall**:</span></span>
  
  * <span data-ttu-id="4667c-309">Modul teploty je vysoká, ale mimo teploty je nízká</span><span class="sxs-lookup"><span data-stu-id="4667c-309">Engine temperature is high but outside temperature is low</span></span>
  * <span data-ttu-id="4667c-310">Modul teploty je nízký, ale mimo teploty je vysoká.</span><span class="sxs-lookup"><span data-stu-id="4667c-310">Engine temperature is low but outside temperature is high</span></span>

<span data-ttu-id="4667c-311">V závislosti na požadavcích předchozí, jsme vytvořili dva samostatné modely k detekci anomálií, jeden pro zjišťování vehicle údržby a jeden pro zjišťování pro vyvolání vehicle.</span><span class="sxs-lookup"><span data-stu-id="4667c-311">Based on the previous requirements, we have created two separate models to detect anomalies, one for vehicle maintenance detection, and one for vehicle recall detection.</span></span> <span data-ttu-id="4667c-312">V obou těchto modelů integrované algoritmus hlavní součásti Analysis (PCA) slouží pro zjišťování anomálií.</span><span class="sxs-lookup"><span data-stu-id="4667c-312">In both these models, the built-in Principal Component Analysis (PCA) algorithm is used for anomaly detection.</span></span> 

<span data-ttu-id="4667c-313">**Model pro odhalování údržby**</span><span class="sxs-lookup"><span data-stu-id="4667c-313">**Maintenance detection model**</span></span>

<span data-ttu-id="4667c-314">Pokud jeden z tři indikátory - zatížení můžete zadat, těžba ropy modul nebo modul teploty - splňuje jeho odpovídající stav, model pro odhalování údržby sestavy anomálií.</span><span class="sxs-lookup"><span data-stu-id="4667c-314">If one of three indicators - tire pressure, engine oil, or engine temperature - satisfies its respective condition, the maintenance detection model reports an anomaly.</span></span> <span data-ttu-id="4667c-315">Výsledkem je musíme pouze vzít v úvahu tyto tři proměnné při vytváření modelu.</span><span class="sxs-lookup"><span data-stu-id="4667c-315">As a result, we only need to consider these three variables in building the model.</span></span> <span data-ttu-id="4667c-316">V našem experimentu v Azure Machine Learning, nejprve používáme **výběr sloupců v datové sadě** modulu k extrakci těchto tří proměnných.</span><span class="sxs-lookup"><span data-stu-id="4667c-316">In our experiment in Azure Machine Learning, we first use a **Select Columns in Dataset** module to extract these three variables.</span></span> <span data-ttu-id="4667c-317">Modul pro zjišťování anomálií založený na PCA vedle používáme k sestavení modelu detekce anomálií.</span><span class="sxs-lookup"><span data-stu-id="4667c-317">Next we use the PCA-based anomaly detection module to build the anomaly detection model.</span></span> 

<span data-ttu-id="4667c-318">Vytvořeným technika v machine learning, který lze použít k výběru funkcí, klasifikace a detekce anomálií je hlavní součásti analýza (PCA).</span><span class="sxs-lookup"><span data-stu-id="4667c-318">Principal Component Analysis (PCA) is an established technique in machine learning that can be applied to feature selection, classification, and anomaly detection.</span></span> <span data-ttu-id="4667c-319">PCA převede sadu případ obsahující pravděpodobně korelační proměnné, do sady hodnot nazývané jako hlavní komponenty.</span><span class="sxs-lookup"><span data-stu-id="4667c-319">PCA converts a set of case containing possibly correlated variables, into a set of values called principal components.</span></span> <span data-ttu-id="4667c-320">Klíče představu o na základě PCA modelování je projektu dat na nižší dimenzí místa, aby funkce a anomálií, lze snadno identifikovat.</span><span class="sxs-lookup"><span data-stu-id="4667c-320">The key idea of PCA-based modeling is to project data onto a lower-dimensional space so that features and anomalies can be more easily identified.</span></span>

<span data-ttu-id="4667c-321">Pro každý nový vstup model pro odhalování detekce anomálií nejprve vypočítá jeho projekce na eigenvectors a pak vypočítá chyba normalizovaný obnovu.</span><span class="sxs-lookup"><span data-stu-id="4667c-321">For each new input to  the detection model, the anomaly detector first computes its projection on the eigenvectors, and then computes the normalized reconstruction error.</span></span> <span data-ttu-id="4667c-322">Tato chyba normalizovaný je skóre anomálií.</span><span class="sxs-lookup"><span data-stu-id="4667c-322">This normalized error is the anomaly score.</span></span> <span data-ttu-id="4667c-323">Čím chyby, více neobvyklé instance je.</span><span class="sxs-lookup"><span data-stu-id="4667c-323">The higher the error, the more anomalous the instance is.</span></span> 

<span data-ttu-id="4667c-324">V údržby zjišťování problému každý záznam lze považovat za bod v prostorovém 3 prostoru definovaný zatížení můžete zadat, modul těžba ropy a teploty modul souřadnice.</span><span class="sxs-lookup"><span data-stu-id="4667c-324">In the maintenance detection problem, each record can be considered as a point in a 3-dimensional space defined by tire pressure, engine oil, and engine temperature coordinates.</span></span> <span data-ttu-id="4667c-325">Pokud chcete zaznamenat tyto anomálií, jsme můžete promítnout do 2 dimenzí prostoru pomocí PCA původní data v prostorovém 3 prostoru.</span><span class="sxs-lookup"><span data-stu-id="4667c-325">To capture these anomalies, we can project the original data in the 3-dimensional space onto a 2-dimensional space using PCA.</span></span> <span data-ttu-id="4667c-326">Proto jsme nastavit parametr počet komponenty pro použití v PCA být 2.</span><span class="sxs-lookup"><span data-stu-id="4667c-326">Thus, we set the parameter Number of components to use in PCA to be 2.</span></span> <span data-ttu-id="4667c-327">Tento parametr hraje důležitou roli při uplatňování detekce anomálií založený na PCA.</span><span class="sxs-lookup"><span data-stu-id="4667c-327">This parameter plays an important role in applying PCA-based anomaly detection.</span></span> <span data-ttu-id="4667c-328">Po plánování dat pomocí PCA abychom mohli snadno identifikovat tyto anomálií.</span><span class="sxs-lookup"><span data-stu-id="4667c-328">After projecting data using PCA, we can identify these anomalies more easily.</span></span>

<span data-ttu-id="4667c-329">**Model detekce anomálií odvolání** v modelu s detekce anomálií odvolání používáme výběr sloupců v datové sadě a anomálií založený na PCA moduly rozpoznávání podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="4667c-329">**Recall anomaly detection model** In the recall anomaly detection model, we use the Select Columns in Dataset and PCA-based anomaly detection modules in a similar way.</span></span> <span data-ttu-id="4667c-330">Konkrétně jsme nejprve extrahovat pomocí tří proměnných - mimo teploty a rychlost teploty modul - **výběr sloupců v datové sadě** modulu.</span><span class="sxs-lookup"><span data-stu-id="4667c-330">Specifically, we first extract three variables - engine temperature, outside temperature, and speed - using the **Select Columns in Dataset** module.</span></span> <span data-ttu-id="4667c-331">Můžeme také zahrnovat proměnnou rychlost vzhledem k tomu, že teplota modul obvykle je vztažen k rychlosti.</span><span class="sxs-lookup"><span data-stu-id="4667c-331">We also include the speed variable since the engine temperature typically is correlated to the speed.</span></span> <span data-ttu-id="4667c-332">Další používáme modulu detekce anomálií založený na PCA do projektu data z dimenzí 3 prostoru na 2 dimenzí místa.</span><span class="sxs-lookup"><span data-stu-id="4667c-332">Next we use PCA-based anomaly detection module to project the data from the 3-dimensional space onto a 2-dimensional space.</span></span> <span data-ttu-id="4667c-333">Jsou splněna kritéria pro vyvolání a proto nástroj při vysoce negativně korelační modul teploty a mimo teploty vyžaduje odvolání.</span><span class="sxs-lookup"><span data-stu-id="4667c-333">The recall criteria are satisfied and so the vehicle requires recall when engine temperature and outside temperature are highly negatively correlated.</span></span> <span data-ttu-id="4667c-334">Pomocí algoritmu detekce anomálií založený na PCA, jsme po provedení PCA zachytit anomálií.</span><span class="sxs-lookup"><span data-stu-id="4667c-334">Using PCA-based anomaly detection algorithm, we can capture the anomalies after performing PCA.</span></span> 

<span data-ttu-id="4667c-335">Při tréninku buď modelu, musíme normální data, která nevyžaduje údržby nebo odvolání jako vstupní data pro trénování modelu detekce anomálií založený na PCA použít.</span><span class="sxs-lookup"><span data-stu-id="4667c-335">When training either model, we need to use normal data, which does not require maintenance or recall as the input data to train the PCA-based anomaly detection model.</span></span> <span data-ttu-id="4667c-336">V rámci vyhodnocování experimentu používáme modelu detekce anomálií vyškolení ke zjištění, zda nástroj vyžaduje údržby nebo odvolání.</span><span class="sxs-lookup"><span data-stu-id="4667c-336">In the scoring experiment, we use the trained anomaly detection model to detect whether or not the vehicle requires maintenance or recall.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="4667c-337">Analýzy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="4667c-337">Real-time analysis</span></span>
<span data-ttu-id="4667c-338">Následující dotaz SQL Stream Analytics se používá k získání průměr všech parametrů důležité vehicle jako rychlosti, úroveň paliva, modul teploty, počítadlo kilometrů čtení, můžete zadat naléhavost, modul těžba ropy úroveň a dalších.</span><span class="sxs-lookup"><span data-stu-id="4667c-338">The following Stream Analytics SQL Query is used to get the average of all the important vehicle parameters like vehicle speed, fuel level, engine temperature, odometer reading, tire pressure, engine oil level, and others.</span></span> <span data-ttu-id="4667c-339">Průměry se používají k zjišťovat anomálie, vydání výstrahy a určení celkového stavu podmínky vozidel provozovat v určité oblasti a chybě korelace demografické údaje.</span><span class="sxs-lookup"><span data-stu-id="4667c-339">The averages are used to detect anomalies, issue alerts, and determine the overall health conditions of vehicles operated in specific region and then correlate it to demographics.</span></span> 

![Stream Analytics query pro zpracování v reálném čase](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

<span data-ttu-id="4667c-341">*Obrázek 13: dotaz služby Stream Analytics ke zpracování v reálném čase*</span><span class="sxs-lookup"><span data-stu-id="4667c-341">*Figure 13 – Stream Analytics query for real-time processing*</span></span>

<span data-ttu-id="4667c-342">Všechny průměry jsou vypočítávány přes TumblingWindow 3 sekundu.</span><span class="sxs-lookup"><span data-stu-id="4667c-342">All the averages are calculated over a 3-second TumblingWindow.</span></span> <span data-ttu-id="4667c-343">TubmlingWindow se používá v tomto případě vzhledem k tomu, že je nutné, aby se a souvislý časové intervaly.</span><span class="sxs-lookup"><span data-stu-id="4667c-343">We are using TubmlingWindow in this case since we require non-overlapping and contiguous time intervals.</span></span> 

<span data-ttu-id="4667c-344">Další informace o všech možnostech "Oddílová" v Azure Stream Analytics, klikněte na tlačítko [Oddílová (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span><span class="sxs-lookup"><span data-stu-id="4667c-344">To learn more about all the "Windowing" capabilities in Azure Stream Analytics, click [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span></span>

<span data-ttu-id="4667c-345">**V reálném čase předpovědi**</span><span class="sxs-lookup"><span data-stu-id="4667c-345">**Real-time prediction**</span></span>

<span data-ttu-id="4667c-346">Aplikace je součástí řešení, aby zprovoznit model machine learning v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="4667c-346">An application is included as part of the solution to operationalize the machine learning model in real time.</span></span> <span data-ttu-id="4667c-347">Tuto aplikaci s názvem "RealTimeDashboardApp" je vytvořen a nakonfigurován jako součást nasazení řešení.</span><span class="sxs-lookup"><span data-stu-id="4667c-347">This application called “RealTimeDashboardApp” is created and configured as part of the solution deployment.</span></span> <span data-ttu-id="4667c-348">Aplikace provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="4667c-348">The application performs the following:</span></span>

1. <span data-ttu-id="4667c-349">Naslouchá na instanci Centra událostí, kde Stream Analytics je publikování události ve tvaru nepřetržitě.</span><span class="sxs-lookup"><span data-stu-id="4667c-349">Listens to an Event Hub instance where Stream Analytics is publishing the events in a pattern continuously.</span></span> <span data-ttu-id="4667c-350">![Stream Analytics dotazu pro publikování dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *obrázek 14: Stream Analytics dotazu pro publikování dat do výstupu instance centra událostí*</span><span class="sxs-lookup"><span data-stu-id="4667c-350">![Stream Analytics query for publishing the data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *Figure 14 – Stream Analytics query for publishing the data to an output Event Hub instance*</span></span> 
2. <span data-ttu-id="4667c-351">Pro každou událost, která obdrží tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="4667c-351">For every event that this application receives:</span></span> 
   
   * <span data-ttu-id="4667c-352">Zpracuje data pomocí koncového bodu Machine Learning požadavků a odpovědí vyhodnocování (záznamy RR).</span><span class="sxs-lookup"><span data-stu-id="4667c-352">Processes the data using Machine Learning Request-Response Scoring (RRS) endpoint.</span></span> <span data-ttu-id="4667c-353">Záznamy o prostředku koncového bodu je automaticky publikován jako součást nasazení.</span><span class="sxs-lookup"><span data-stu-id="4667c-353">The RRS endpoint is automatically published as part of the deployment.</span></span>
   * <span data-ttu-id="4667c-354">Výstup RRS je publikovaný na datovou sadu Power BI pomocí nabízených rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4667c-354">The RRS output is published to a Power BI dataset using the push APIs.</span></span>

<span data-ttu-id="4667c-355">Tento vzor platí i pro scénáře, ve kterých chcete integrovat obchodní (LoB) aplikace s tokem analýzu v reálném čase, pro scénáře, jako je výstrahy, oznámení a zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="4667c-355">This pattern is also applicable to scenarios in which you want to integrate a Line of Business (LoB) application with the real-time analytics flow, for scenarios such as alerts, notifications, and messaging.</span></span>

<span data-ttu-id="4667c-356">Klikněte na tlačítko [RealtimeDashboardApp stažení](http://go.microsoft.com/fwlink/?LinkId=717078) ke stažení sady Visual Studio RealtimeDashboardApp řešení pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="4667c-356">Click [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078) to download the RealtimeDashboardApp Visual Studio solution for customizations.</span></span> 

<span data-ttu-id="4667c-357">**Spuštění aplikace řídicího panelu v reálném čase**</span><span class="sxs-lookup"><span data-stu-id="4667c-357">**To execute the Real-time Dashboard Application**</span></span>
1. <span data-ttu-id="4667c-358">Extrahování a uložit místně ![RealtimeDashboardApp složky](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *obrázek 16 – RealtimeDashboardApp složky*</span><span class="sxs-lookup"><span data-stu-id="4667c-358">Extract and save locally ![RealtimeDashboardApp folder](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *Figure 16 – RealtimeDashboardApp folder*</span></span>  
2. <span data-ttu-id="4667c-359">Spuštění aplikace RealtimeDashboardApp.exe</span><span class="sxs-lookup"><span data-stu-id="4667c-359">Execute the application RealtimeDashboardApp.exe</span></span>
3. <span data-ttu-id="4667c-360">Zadejte platné přihlašovací údaje Power BI, přihlášení a klikněte na tlačítko Přijmout</span><span class="sxs-lookup"><span data-stu-id="4667c-360">Provide valid Power BI credentials, sign in and click Accept</span></span> ![Přihlášení řídicího panelu v reálném čase aplikace k Power BI.](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Aplikace v reálném čase řídicího panelu dokončit přihlášení k Power BI.](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

<span data-ttu-id="4667c-363">*Obrázek 17 – RealtimeDashboardApp: Přihlaste se do Power BI*</span><span class="sxs-lookup"><span data-stu-id="4667c-363">*Figure 17 – RealtimeDashboardApp: Sign-in to Power BI*</span></span>

>[!NOTE] 
><span data-ttu-id="4667c-364">Pokud chcete vyprázdnit datové sady Power BI, spouštění RealtimeDashboardApp s parametrem "flushdata":</span><span class="sxs-lookup"><span data-stu-id="4667c-364">If you want to flush the Power BI dataset, execute the RealtimeDashboardApp with the "flushdata" parameter:</span></span> 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a><span data-ttu-id="4667c-365">Dávková analýza</span><span class="sxs-lookup"><span data-stu-id="4667c-365">Batch analysis</span></span>
<span data-ttu-id="4667c-366">Cílem je zobrazit, jak motory Contoso využívá funkce výpočtů Azure a budou využívat velké objemy dat a získáte přehled o bohaté na řízení vzorek, chování využití a stavu vehicle.</span><span class="sxs-lookup"><span data-stu-id="4667c-366">The goal here is to show how Contoso Motors utilizes the Azure compute capabilities to harness big data to gain rich insights on driving pattern, usage behavior, and vehicle health.</span></span> <span data-ttu-id="4667c-367">Díky tomu je možné:</span><span class="sxs-lookup"><span data-stu-id="4667c-367">This makes it possible to:</span></span>

* <span data-ttu-id="4667c-368">Zlepšovat prostředí zákazníků a nastavit jej jako levnější tím, že poskytuje přehled na řídí zvyklosti a efektivní řízení chování paliva</span><span class="sxs-lookup"><span data-stu-id="4667c-368">Improve the customer experience and make it cheaper by providing insights on driving habits and fuel efficient driving behaviors</span></span>
* <span data-ttu-id="4667c-369">Seznamte se aktivně zákazníků a jejich řízení patters řídit obchodní rozhodnutí a poskytovat nejlepší v třída produkty a služby</span><span class="sxs-lookup"><span data-stu-id="4667c-369">Learn proactively about customers and their driving patters to govern business decisions and provide the best in class products & services</span></span>

<span data-ttu-id="4667c-370">V tomto řešení jsme cílení následující metriky:</span><span class="sxs-lookup"><span data-stu-id="4667c-370">In this solution, we are targeting the following metrics:</span></span>

1. <span data-ttu-id="4667c-371">**Agresivní řídí chování**: identifikuje trend modely, umístění, podporovat jeho podmínky, a času v roce a získáte přehled o agresivní řízení vzory.</span><span class="sxs-lookup"><span data-stu-id="4667c-371">**Aggressive driving behavior**: Identifies the trend of the models, locations, driving conditions, and time of the year to gain insights on aggressive driving patterns.</span></span> <span data-ttu-id="4667c-372">Contoso motory pro můžete použít tyto přehledy marketingových kampaní, řídí přizpůsobené nové funkce a pojišťovnictví na základě využití.</span><span class="sxs-lookup"><span data-stu-id="4667c-372">Contoso Motors can use these insights for marketing campaigns, driving new personalized features and usage-based insurance.</span></span>
2. <span data-ttu-id="4667c-373">**Paliva efektivní řízení chování**: identifikuje trend modely, umístění, podporovat jeho podmínky, a času v roce a získáte přehled o paliva efektivní řízení vzory.</span><span class="sxs-lookup"><span data-stu-id="4667c-373">**Fuel efficient driving behavior**: Identifies the trend of the models, locations, driving conditions, and time of the year to gain insights on fuel efficient driving patterns.</span></span> <span data-ttu-id="4667c-374">Contoso motory pro můžete použít tyto přehledy marketingových kampaní, řídí nové funkce a proaktivní zprávy ovladače pro náklady platné a prostředí zvyklosti popisný řízení.</span><span class="sxs-lookup"><span data-stu-id="4667c-374">Contoso Motors can use these insights for marketing campaigns, driving new features and proactive reporting to the drivers for cost effective and environment friendly driving habits.</span></span> 
3. <span data-ttu-id="4667c-375">**Odvolat modely**: identifikuje modely nutnosti navrácení podle zprovozňování detekce anomálií strojového učení experimentu</span><span class="sxs-lookup"><span data-stu-id="4667c-375">**Recall models**: Identifies models requiring recalls by operationalizing the anomaly detection machine learning experiment</span></span>

<span data-ttu-id="4667c-376">Podívejme se na podrobné informace o jednotlivých tyto metriky</span><span class="sxs-lookup"><span data-stu-id="4667c-376">Let's look into the details of each of these metrics,</span></span>

<span data-ttu-id="4667c-377">**Agresivní řízení vzor**</span><span class="sxs-lookup"><span data-stu-id="4667c-377">**Aggressive driving pattern**</span></span>

<span data-ttu-id="4667c-378">Signály oddílů vehicle a diagnostických dat jsou zpracovány v kanál s názvem "AggresiveDrivingPatternPipeline" použití Hive k určení modely, umístění, vehicle, jízdních podmínek a další parametry, pro jehož agresivní řízení vzor.</span><span class="sxs-lookup"><span data-stu-id="4667c-378">The partitioned vehicle signals and diagnostic data are processed in the pipeline named "AggresiveDrivingPatternPipeline" using Hive to determine the models, location, vehicle, driving conditions, and other parameters that exhibits aggressive driving pattern.</span></span>

<span data-ttu-id="4667c-379">![Agresivní řízení pracovního postupu vzor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*obrázek 18 – agresivní řízení pracovního postupu vzor*</span><span class="sxs-lookup"><span data-stu-id="4667c-379">![Aggressive driving pattern workflow](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*Figure 18 – Aggressive driving pattern workflow*</span></span>


<span data-ttu-id="4667c-380">***Agresivní řízení dotaz Hive vzor***</span><span class="sxs-lookup"><span data-stu-id="4667c-380">***Aggressive driving pattern Hive query***</span></span>

<span data-ttu-id="4667c-381">S názvem "aggresivedriving.hql" použít pro analýzu agresivní řízení vzor podmínku skriptu Hive je umístěný ve složce "\demo\src\connectedcar\scripts" souboru zip staženého.</span><span class="sxs-lookup"><span data-stu-id="4667c-381">The Hive script named "aggresivedriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of the downloaded zip.</span></span> 

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


<span data-ttu-id="4667c-382">Kombinace ozubené kolečko pozici přenosu na vehicle, brzdy Pedálové stavu a rychlost používá ke zjišťování reckless/agresivní řízení chování v závislosti na brzdění vzor vysokou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="4667c-382">It uses the combination of vehicle's transmission gear position, brake pedal status, and speed to detect reckless/aggressive driving behavior based on braking pattern at high speed.</span></span> 

<span data-ttu-id="4667c-383">Jakmile kanálu proveden úspěšně, zobrazí se následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar".</span><span class="sxs-lookup"><span data-stu-id="4667c-383">Once the pipeline is executed successfully, you see the following partitions generated in your storage account under the "connectedcar" container.</span></span>

![Výstup AggressiveDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

<span data-ttu-id="4667c-385">*Obrázek 19 – AggressiveDrivingPatternPipeline výstup*</span><span class="sxs-lookup"><span data-stu-id="4667c-385">*Figure 19 – AggressiveDrivingPatternPipeline output*</span></span>

<span data-ttu-id="4667c-386">**Efektivní řízení vzor paliva**</span><span class="sxs-lookup"><span data-stu-id="4667c-386">**Fuel efficient driving pattern**</span></span>

<span data-ttu-id="4667c-387">Signály oddílů vehicle a diagnostických dat jsou zpracovány v kanál s názvem "FuelEfficientDrivingPatternPipeline".</span><span class="sxs-lookup"><span data-stu-id="4667c-387">The partitioned vehicle signals and diagnostic data are processed in the pipeline named "FuelEfficientDrivingPatternPipeline".</span></span> <span data-ttu-id="4667c-388">Hive slouží k určení modely, umístění, vehicle, jízdních podmínek a další vlastnosti, které vykazují paliva efektivní řízení vzorem.</span><span class="sxs-lookup"><span data-stu-id="4667c-388">Hive is used to determine the models, location, vehicle, driving conditions, and other properties that exhibit fuel efficient driving pattern.</span></span>

![Zvýšení řízení vzor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

<span data-ttu-id="4667c-390">*Obrázek 20 – zvýšení řízení pracovního postupu vzor*</span><span class="sxs-lookup"><span data-stu-id="4667c-390">*Figure 20 – Fuel-efficient driving pattern workflow*</span></span>

<span data-ttu-id="4667c-391">***Paliva efektivní řízení vzor dotaz Hive***</span><span class="sxs-lookup"><span data-stu-id="4667c-391">***Fuel efficient driving pattern Hive query***</span></span>

<span data-ttu-id="4667c-392">S názvem "fuelefficientdriving.hql" použít pro analýzu agresivní řízení vzor podmínku skriptu Hive je umístěný ve složce "\demo\src\connectedcar\scripts" souboru zip staženého.</span><span class="sxs-lookup"><span data-stu-id="4667c-392">The Hive script named "fuelefficientdriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of the downloaded zip.</span></span> 

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


<span data-ttu-id="4667c-393">Používá kombinaci vehicle na přenos ozubené kolečko pozice, brzdy Pedálové stav, rychlost a akcelerátoru Pedálové pozice ke zjištění paliva efektivní řízení chování v závislosti na akcelerace, brzdění a rychlost vzory.</span><span class="sxs-lookup"><span data-stu-id="4667c-393">It uses the combination of vehicle's transmission gear position, brake pedal status, speed, and accelerator pedal position to detect fuel efficient driving behavior based on acceleration, braking, and speed patterns.</span></span> 

<span data-ttu-id="4667c-394">Jakmile kanálu proveden úspěšně, zobrazí se následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar".</span><span class="sxs-lookup"><span data-stu-id="4667c-394">Once the pipeline is executed successfully, you see the following partitions generated in your storage account under the "connectedcar" container.</span></span>

![Výstup FuelEfficientDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

<span data-ttu-id="4667c-396">*Obrázek 21 – FuelEfficientDrivingPatternPipeline výstup*</span><span class="sxs-lookup"><span data-stu-id="4667c-396">*Figure 21 – FuelEfficientDrivingPatternPipeline output*</span></span>

<span data-ttu-id="4667c-397">**Odvolat předpovědi**</span><span class="sxs-lookup"><span data-stu-id="4667c-397">**Recall Predictions**</span></span>

<span data-ttu-id="4667c-398">Strojového učení experimentu je zřízený a publikovat jako webovou službu, jako součást nasazení řešení.</span><span class="sxs-lookup"><span data-stu-id="4667c-398">The machine learning experiment is provisioned and published as a web service as part of the solution deployment.</span></span> <span data-ttu-id="4667c-399">V tomto pracovním postupu, zaregistrován jako služba data factory propojené a operationalized pomocí data factory aktivita dávkového vyhodnocování se využívají záznamy dávkového vyhodnocování koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="4667c-399">The batch scoring end point is leveraged in this workflow, registered as a data factory linked service and operationalized using data factory batch scoring activity.</span></span>

![Koncový bod Machine Learning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

<span data-ttu-id="4667c-401">*Obrázek 22 – strojového učení koncový bod registrován jako propojené služby v datové továrně*</span><span class="sxs-lookup"><span data-stu-id="4667c-401">*Figure 22 – Machine learning endpoint registered as a linked service in data factory*</span></span>

<span data-ttu-id="4667c-402">Registrovanou propojenou službu se používá v DetectAnomalyPipeline skóre pro data s využitím modelu detekce anomálií.</span><span class="sxs-lookup"><span data-stu-id="4667c-402">The registered linked service is used in the DetectAnomalyPipeline to score the data using the anomaly detection model.</span></span> 

![Počítač Learning aktivita dávkového vyhodnocování v datové továrně](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

<span data-ttu-id="4667c-404">*Obrázek 23 – Azure Machine Learning dávkového vyhodnocování aktivity v objektu pro vytváření dat*</span><span class="sxs-lookup"><span data-stu-id="4667c-404">*Figure 23 – Azure Machine Learning Batch Scoring activity in data factory*</span></span> 

<span data-ttu-id="4667c-405">Je několik kroků provést u tohoto kanálu pro přípravu dat tak, aby může být operationalized s dávkového vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="4667c-405">There are few steps performed in this pipeline for data preparation so that it can be operationalized with the batch scoring web service.</span></span> 

![DetectAnomalyPipeline pro predikci vozidel nutnosti navrácení](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

<span data-ttu-id="4667c-407">*Obrázek 24 – DetectAnomalyPipeline pro predikci vozidel nutnosti navrácení*</span><span class="sxs-lookup"><span data-stu-id="4667c-407">*Figure 24 – DetectAnomalyPipeline for predicting vehicles requiring recalls*</span></span> 

<span data-ttu-id="4667c-408">***Dotaz Hive detekce anomálií***</span><span class="sxs-lookup"><span data-stu-id="4667c-408">***Anomaly detection Hive query***</span></span>

<span data-ttu-id="4667c-409">Po dokončení vyhodnocování aktivitu HDInsight se používá ke zpracování a agregovat data, která jsou zařazené do kategorií jako anomálií modelem se skóre pravděpodobnosti 0,60 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="4667c-409">Once the scoring is completed, an HDInsight activity is used to process and aggregate the data that are categorized as anomalies by the model with a probability score of 0.60 or higher.</span></span>

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


<span data-ttu-id="4667c-410">Jakmile kanálu proveden úspěšně, zobrazí se následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar".</span><span class="sxs-lookup"><span data-stu-id="4667c-410">Once the pipeline is executed successfully, you see the following partitions generated in your storage account under the "connectedcar" container.</span></span>

![Výstup DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

<span data-ttu-id="4667c-412">*Obrázek 25 – DetectAnomalyPipeline výstup*</span><span class="sxs-lookup"><span data-stu-id="4667c-412">*Figure 25 – DetectAnomalyPipeline output*</span></span>

## <a name="publish"></a><span data-ttu-id="4667c-413">Publikování</span><span class="sxs-lookup"><span data-stu-id="4667c-413">Publish</span></span>

### <a name="real-time-analysis"></a><span data-ttu-id="4667c-414">Analýzy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="4667c-414">Real-time analysis</span></span>
<span data-ttu-id="4667c-415">Některý z těchto dotazů v úloze Stream Analytics publikuje události do výstupu instance centra událostí.</span><span class="sxs-lookup"><span data-stu-id="4667c-415">One of the queries in the Stream Analytics job publishes the events to an output Event Hub instance.</span></span> 

![Úlohy Stream Analytics publikuje do výstupu instance centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

<span data-ttu-id="4667c-417">*Obrázek 26 – Stream Analytics úlohy publikuje do výstupu instance centra událostí*</span><span class="sxs-lookup"><span data-stu-id="4667c-417">*Figure 26 – Stream Analytics job publishes to an output Event Hub instance*</span></span>

![Dotaz analýzy datového proudu k publikování výstupu v instanci Centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

<span data-ttu-id="4667c-419">*Obrázek 27 – Stream Analytics dotaz pro publikování ve výstupu centra událostí instance*</span><span class="sxs-lookup"><span data-stu-id="4667c-419">*Figure 27 – Stream Analytics query to publish to the output Event Hub instance*</span></span>

<span data-ttu-id="4667c-420">Tento datový proud událostí, které se spotřebovávají RealTimeDashboardApp součástí řešení.</span><span class="sxs-lookup"><span data-stu-id="4667c-420">This stream of events is consumed by the RealTimeDashboardApp included in the solution.</span></span> <span data-ttu-id="4667c-421">Tato aplikace využívá webové služby Machine Learning požadavků a odpovědí pro vyhodnocování v reálném čase a publikuje Výsledná data do Power BI datové sady pro používání.</span><span class="sxs-lookup"><span data-stu-id="4667c-421">This application leverages the Machine Learning Request-Response web service for real-time scoring and publishes the resultant data to a Power BI dataset for consumption.</span></span> 

### <a name="batch-analysis"></a><span data-ttu-id="4667c-422">Dávková analýza</span><span class="sxs-lookup"><span data-stu-id="4667c-422">Batch analysis</span></span>
<span data-ttu-id="4667c-423">Výsledky batch a v reálném čase zpracování se publikují do tabulky Azure SQL Database pro používání.</span><span class="sxs-lookup"><span data-stu-id="4667c-423">The results of the batch and real-time processing are published to the Azure SQL Database tables for consumption.</span></span> <span data-ttu-id="4667c-424">Azure SQL Server, databáze a tabulky se vytvoří automaticky v rámci instalační skript.</span><span class="sxs-lookup"><span data-stu-id="4667c-424">The Azure SQL Server, Database, and the tables are created automatically as part of the setup script.</span></span> 

![Dávkové zpracování kopírovat výsledky do pracovního postupu datového tržiště](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

<span data-ttu-id="4667c-426">*Obrázek 28 – dávkového zpracování kopírovat výsledky do pracovního postupu datového tržiště*</span><span class="sxs-lookup"><span data-stu-id="4667c-426">*Figure 28 – Batch processing results copy to data mart workflow*</span></span>

![Úlohy Stream Analytics publikuje do datového tržiště](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

<span data-ttu-id="4667c-428">*Obrázek 29 – Stream Analytics úlohy publikuje do datového tržiště*</span><span class="sxs-lookup"><span data-stu-id="4667c-428">*Figure 29 – Stream Analytics job publishes to data mart*</span></span>

![Datové Tržiště nastavení v úloze Stream Analytics](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

<span data-ttu-id="4667c-430">*Obrázek 30 – datové Tržiště nastavení v úloze Stream Analytics*</span><span class="sxs-lookup"><span data-stu-id="4667c-430">*Figure 30 – Data mart setting in Stream Analytics job*</span></span>

## <a name="consume"></a><span data-ttu-id="4667c-431">Konzumace</span><span class="sxs-lookup"><span data-stu-id="4667c-431">Consume</span></span>
<span data-ttu-id="4667c-432">Power BI poskytuje toto řešení bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="4667c-432">Power BI gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="4667c-433">Kliknutím sem získáte podrobné pokyny k nastavení služby Power BI sestavy a řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="4667c-433">Click here for detailed instructions on setting up the Power BI reports and the dashboard.</span></span> <span data-ttu-id="4667c-434">Poslední řídicí panel vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4667c-434">The final dashboard looks like this:</span></span>

![Řídicí panel Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

<span data-ttu-id="4667c-436">*Obrázek 31 - řídicí panel Power BI*</span><span class="sxs-lookup"><span data-stu-id="4667c-436">*Figure 31 - Power BI Dashboard*</span></span>

## <a name="summary"></a><span data-ttu-id="4667c-437">Souhrn</span><span class="sxs-lookup"><span data-stu-id="4667c-437">Summary</span></span>
<span data-ttu-id="4667c-438">Tento dokument obsahuje podrobné procházení řešení Analytics Vehicle Telemetrie.</span><span class="sxs-lookup"><span data-stu-id="4667c-438">This document contains a detailed drill-down of the Vehicle Telemetry Analytics Solution.</span></span> <span data-ttu-id="4667c-439">To umožňující prezentovat vzor architektura lambda v reálném čase a dávky analytics s předpovědi a akce.</span><span class="sxs-lookup"><span data-stu-id="4667c-439">This showcases a lambda architecture pattern for real-time and batch analytics with predictions and actions.</span></span> <span data-ttu-id="4667c-440">Tento vzor platí pro širokou škálu případy použití, které vyžadují aktivní cesta (v reálném čase) a analýzy neaktivní trase (batch).</span><span class="sxs-lookup"><span data-stu-id="4667c-440">This pattern applies to a wide range of use cases that require hot path (real-time) and cold path (batch) analytics.</span></span> 

