---
title: "selhání aaaSimulate v Azure mikroslužeb | Microsoft Docs"
description: "V tomto článku bude zmíněn hello testovatelnosti akce v Microsoft Azure Service Fabric nalezen."
services: service-fabric
documentationcenter: .net
author: motanv
manager: timlt
editor: toddabel
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv;heeldin
ms.openlocfilehash: 5bdda1c0c5a40b243ab956c4791afd52e11c4089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="testability-actions"></a><span data-ttu-id="547d8-103">Testovatelnosti akce</span><span class="sxs-lookup"><span data-stu-id="547d8-103">Testability actions</span></span>
<span data-ttu-id="547d8-104">V pořadí toosimulate nespolehlivé infrastruktury poskytuje Azure Service Fabric vás hello vývojáři a s způsoby toosimulate různé reálného selhání a přechodů mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="547d8-104">In order toosimulate an unreliable infrastructure, Azure Service Fabric provides you, hello developer, with ways toosimulate various real-world failures and state transitions.</span></span> <span data-ttu-id="547d8-105">Tyto jsou zveřejněné jako testovatelnosti akce.</span><span class="sxs-lookup"><span data-stu-id="547d8-105">These are exposed as testability actions.</span></span> <span data-ttu-id="547d8-106">Hello akce jsou hello nízké úrovně rozhraní API, která způsobí ověření, přechod stavu nebo konkrétní chyby vkládání.</span><span class="sxs-lookup"><span data-stu-id="547d8-106">hello actions are hello low-level APIs that cause a specific fault injection, state transition, or validation.</span></span> <span data-ttu-id="547d8-107">Kombinací těchto akcí můžete napsat scénáře komplexní testování pro vaše služby.</span><span class="sxs-lookup"><span data-stu-id="547d8-107">By combining these actions, you can write comprehensive test scenarios for your services.</span></span>

<span data-ttu-id="547d8-108">Service Fabric najdete že některé běžné scénáře, test se skládá z těchto akcí.</span><span class="sxs-lookup"><span data-stu-id="547d8-108">Service Fabric provides some common test scenarios composed of these actions.</span></span> <span data-ttu-id="547d8-109">Důrazně doporučujeme, aby využíváte těchto předdefinovaných scénáře, které jsou pečlivě zvolili tootest běžné přechodů mezi stavy a selhání případy.</span><span class="sxs-lookup"><span data-stu-id="547d8-109">We highly recommend that you utilize these built-in scenarios, which are carefully chosen tootest common state transitions and failure cases.</span></span> <span data-ttu-id="547d8-110">Akce však může být použité toocreate scénáře vlastní testování, pokud chcete tooadd pokrytí pro scénáře, která nespadají pod předdefinované scénáře hello ještě nebo které jsou vlastní, přizpůsobené pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="547d8-110">However, actions can be used toocreate custom test scenarios when you want tooadd coverage for scenarios that are not covered by hello built-in scenarios yet or that are custom tailored for your application.</span></span>

<span data-ttu-id="547d8-111">C# implementace hello akce se nacházejí v hello System.Fabric.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="547d8-111">C# implementations of hello actions are found in hello System.Fabric.dll assembly.</span></span> <span data-ttu-id="547d8-112">modul prostředí PowerShell systému prostředků infrastruktury Hello v hello Microsoft.ServiceFabric.Powershell.dll sestavení nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="547d8-112">hello System Fabric PowerShell module is found in hello Microsoft.ServiceFabric.Powershell.dll assembly.</span></span> <span data-ttu-id="547d8-113">Jako součást instalace modulu runtime hello modul ServiceFabric PowerShell je nainstalovaný tooallow pro snadné použití.</span><span class="sxs-lookup"><span data-stu-id="547d8-113">As part of runtime installation, hello ServiceFabric PowerShell module is installed tooallow for ease of use.</span></span>

## <a name="graceful-vs-ungraceful-fault-actions"></a><span data-ttu-id="547d8-114">Řádné oproti vynuceném selhání akce</span><span class="sxs-lookup"><span data-stu-id="547d8-114">Graceful vs. ungraceful fault actions</span></span>
<span data-ttu-id="547d8-115">Akce testovatelnosti se zařazují do dvou hlavních kbelíků:</span><span class="sxs-lookup"><span data-stu-id="547d8-115">Testability actions are classified into two major buckets:</span></span>

* <span data-ttu-id="547d8-116">Vynuceném chyb: Simulace selhání, jako je restartování počítače tyto chyby a zpracování dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="547d8-116">Ungraceful faults: These faults simulate failures like machine restarts and process crashes.</span></span> <span data-ttu-id="547d8-117">V takových případech chyb zastaví náhle hello kontext spuštění procesu.</span><span class="sxs-lookup"><span data-stu-id="547d8-117">In such cases of failures, hello execution context of process stops abruptly.</span></span> <span data-ttu-id="547d8-118">To znamená, že žádné čištění hello stavu mohou spouštět před spuštěním aplikace hello znovu.</span><span class="sxs-lookup"><span data-stu-id="547d8-118">This means no cleanup of hello state can run before hello application starts up again.</span></span>
* <span data-ttu-id="547d8-119">Řádné chyb: tyto chyby simulace řádně akcí, jako jsou repliky přesune a vyřazuje aktivovány Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="547d8-119">Graceful faults: These faults simulate graceful actions like replica moves and drops triggered by load balancing.</span></span> <span data-ttu-id="547d8-120">V takových případech hello služba získá upozornění hello zavřít a můžete vyčistit hello stavu před ukončením.</span><span class="sxs-lookup"><span data-stu-id="547d8-120">In such cases, hello service gets a notification of hello close and can clean up hello state before exiting.</span></span>

<span data-ttu-id="547d8-121">Pro lepší kvalitu ověření spuštění služby hello a obchodní zatížení při vyvolat různé řádně a vynuceném chyb.</span><span class="sxs-lookup"><span data-stu-id="547d8-121">For better quality validation, run hello service and business workload while inducing various graceful and ungraceful faults.</span></span> <span data-ttu-id="547d8-122">Při vynuceném chyb výkonu scénáře, kde hello služby neočekávaného ukončení procesu uprostřed hello některé pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="547d8-122">Ungraceful faults exercise scenarios where hello service process abruptly exits in hello middle of some workflow.</span></span> <span data-ttu-id="547d8-123">Tato testy hello obnovení cesta ihned po hello služby repliky pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="547d8-123">This tests  hello recovery path once hello service replica is restored by Service Fabric.</span></span> <span data-ttu-id="547d8-124">To vám pomůže testování konzistenci dat a zda je stav služby hello udržovat správně po selhání.</span><span class="sxs-lookup"><span data-stu-id="547d8-124">This will help test data consistency and whether hello service state is maintained correctly after failures.</span></span> <span data-ttu-id="547d8-125">Hello další sadu chyb (hello řádně selhání) otestovat jestli reakce hello služby správně tooreplicas přenášeny pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="547d8-125">hello other set of failures (hello graceful failures) test that hello service correctly reacts tooreplicas being moved around by Service Fabric.</span></span> <span data-ttu-id="547d8-126">Toto zpracování zrušení testů v metodě RunAsync hello.</span><span class="sxs-lookup"><span data-stu-id="547d8-126">This tests handling of cancellation in hello RunAsync method.</span></span> <span data-ttu-id="547d8-127">Služba Hello musí toocheck pro hello zrušení tokenu se nastavit správně uložit stav a ukončete metodě RunAsync hello.</span><span class="sxs-lookup"><span data-stu-id="547d8-127">hello service needs toocheck for hello cancellation token being set, correctly save its state, and exit hello RunAsync method.</span></span>

## <a name="testability-actions-list"></a><span data-ttu-id="547d8-128">Seznam akcí testovatelnosti</span><span class="sxs-lookup"><span data-stu-id="547d8-128">Testability actions list</span></span>
| <span data-ttu-id="547d8-129">Akce</span><span class="sxs-lookup"><span data-stu-id="547d8-129">Action</span></span> | <span data-ttu-id="547d8-130">Popis</span><span class="sxs-lookup"><span data-stu-id="547d8-130">Description</span></span> | <span data-ttu-id="547d8-131">Spravovaná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="547d8-131">Managed API</span></span> | <span data-ttu-id="547d8-132">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="547d8-132">PowerShell cmdlet</span></span> | <span data-ttu-id="547d8-133">Řádné/vynuceném chyb</span><span class="sxs-lookup"><span data-stu-id="547d8-133">Graceful/ungraceful faults</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="547d8-134">CleanTestState</span><span class="sxs-lookup"><span data-stu-id="547d8-134">CleanTestState</span></span> |<span data-ttu-id="547d8-135">Odebere všechny stavy testovací hello hello clusteru v případě chybný vypnutí hello testovací ovladače.</span><span class="sxs-lookup"><span data-stu-id="547d8-135">Removes all hello test state from hello cluster in case of a bad shutdown of hello test driver.</span></span> |<span data-ttu-id="547d8-136">CleanTestStateAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-136">CleanTestStateAsync</span></span> |<span data-ttu-id="547d8-137">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="547d8-137">Remove-ServiceFabricTestState</span></span> |<span data-ttu-id="547d8-138">Neuvedeno</span><span class="sxs-lookup"><span data-stu-id="547d8-138">Not applicable</span></span> |
| <span data-ttu-id="547d8-139">InvokeDataLoss</span><span class="sxs-lookup"><span data-stu-id="547d8-139">InvokeDataLoss</span></span> |<span data-ttu-id="547d8-140">Indukuje ztráty dat do oddílu služby.</span><span class="sxs-lookup"><span data-stu-id="547d8-140">Induces data loss into a service partition.</span></span> |<span data-ttu-id="547d8-141">InvokeDataLossAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-141">InvokeDataLossAsync</span></span> |<span data-ttu-id="547d8-142">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="547d8-142">Invoke-ServiceFabricPartitionDataLoss</span></span> |<span data-ttu-id="547d8-143">Řádné</span><span class="sxs-lookup"><span data-stu-id="547d8-143">Graceful</span></span> |
| <span data-ttu-id="547d8-144">InvokeQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="547d8-144">InvokeQuorumLoss</span></span> |<span data-ttu-id="547d8-145">Oddíl dané stavové služby umístí do ztrátě kvora.</span><span class="sxs-lookup"><span data-stu-id="547d8-145">Puts a given stateful service partition into quorum loss.</span></span> |<span data-ttu-id="547d8-146">InvokeQuorumLossAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-146">InvokeQuorumLossAsync</span></span> |<span data-ttu-id="547d8-147">Vyvolání ServiceFabricQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="547d8-147">Invoke-ServiceFabricQuorumLoss</span></span> |<span data-ttu-id="547d8-148">Řádné</span><span class="sxs-lookup"><span data-stu-id="547d8-148">Graceful</span></span> |
| <span data-ttu-id="547d8-149">Přesunutí primární</span><span class="sxs-lookup"><span data-stu-id="547d8-149">Move Primary</span></span> |<span data-ttu-id="547d8-150">Přesune hello zadat primární repliky stavové služby toohello zadaný uzel clusteru.</span><span class="sxs-lookup"><span data-stu-id="547d8-150">Moves hello specified primary replica of a stateful service toohello specified cluster node.</span></span> |<span data-ttu-id="547d8-151">MovePrimaryAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-151">MovePrimaryAsync</span></span> |<span data-ttu-id="547d8-152">Přesunutí ServiceFabricPrimaryReplica</span><span class="sxs-lookup"><span data-stu-id="547d8-152">Move-ServiceFabricPrimaryReplica</span></span> |<span data-ttu-id="547d8-153">Řádné</span><span class="sxs-lookup"><span data-stu-id="547d8-153">Graceful</span></span> |
| <span data-ttu-id="547d8-154">Přesunout sekundární</span><span class="sxs-lookup"><span data-stu-id="547d8-154">Move Secondary</span></span> |<span data-ttu-id="547d8-155">Přesune hello aktuální sekundární repliky do stavové služby tooa jiného uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="547d8-155">Moves hello current secondary replica of a stateful service tooa different cluster node.</span></span> |<span data-ttu-id="547d8-156">MoveSecondaryAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-156">MoveSecondaryAsync</span></span> |<span data-ttu-id="547d8-157">Přesunutí ServiceFabricSecondaryReplica</span><span class="sxs-lookup"><span data-stu-id="547d8-157">Move-ServiceFabricSecondaryReplica</span></span> |<span data-ttu-id="547d8-158">Řádné</span><span class="sxs-lookup"><span data-stu-id="547d8-158">Graceful</span></span> |
| <span data-ttu-id="547d8-159">RemoveReplica</span><span class="sxs-lookup"><span data-stu-id="547d8-159">RemoveReplica</span></span> |<span data-ttu-id="547d8-160">Repliky selhání simuluje odebráním repliku z clusteru.</span><span class="sxs-lookup"><span data-stu-id="547d8-160">Simulates a replica failure by removing a replica from a cluster.</span></span> <span data-ttu-id="547d8-161">To se zavře hello repliky a přejde se toorole 'None', všechny její stav odebrání z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="547d8-161">This will close hello replica and will transition it toorole 'None', removing all of its state from hello cluster.</span></span> |<span data-ttu-id="547d8-162">RemoveReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-162">RemoveReplicaAsync</span></span> |<span data-ttu-id="547d8-163">Odebrat ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="547d8-163">Remove-ServiceFabricReplica</span></span> |<span data-ttu-id="547d8-164">Řádné</span><span class="sxs-lookup"><span data-stu-id="547d8-164">Graceful</span></span> |
| <span data-ttu-id="547d8-165">RestartDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="547d8-165">RestartDeployedCodePackage</span></span> |<span data-ttu-id="547d8-166">Simuluje selhání procesu balíček kódu restartováním balíček kódu nasazené na uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="547d8-166">Simulates a code package process failure by restarting a code package deployed on a node in a cluster.</span></span> <span data-ttu-id="547d8-167">To zruší hello kód balíček procesu, který restartuje všechny hello uživatele služby repliky hostované v tomto procesu.</span><span class="sxs-lookup"><span data-stu-id="547d8-167">This aborts hello code package process, which will restart all hello user service replicas hosted in that process.</span></span> |<span data-ttu-id="547d8-168">RestartDeployedCodePackageAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-168">RestartDeployedCodePackageAsync</span></span> |<span data-ttu-id="547d8-169">Restartování ServiceFabricDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="547d8-169">Restart-ServiceFabricDeployedCodePackage</span></span> |<span data-ttu-id="547d8-170">Vynuceném</span><span class="sxs-lookup"><span data-stu-id="547d8-170">Ungraceful</span></span> |
| <span data-ttu-id="547d8-171">RestartNode</span><span class="sxs-lookup"><span data-stu-id="547d8-171">RestartNode</span></span> |<span data-ttu-id="547d8-172">Selhání uzlu clusteru Service Fabric simuluje restartováním uzlu.</span><span class="sxs-lookup"><span data-stu-id="547d8-172">Simulates a Service Fabric cluster node failure by restarting a node.</span></span> |<span data-ttu-id="547d8-173">RestartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-173">RestartNodeAsync</span></span> |<span data-ttu-id="547d8-174">Restartování ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="547d8-174">Restart-ServiceFabricNode</span></span> |<span data-ttu-id="547d8-175">Vynuceném</span><span class="sxs-lookup"><span data-stu-id="547d8-175">Ungraceful</span></span> |
| <span data-ttu-id="547d8-176">RestartPartition</span><span class="sxs-lookup"><span data-stu-id="547d8-176">RestartPartition</span></span> |<span data-ttu-id="547d8-177">Simuluje scénáři přerušení spojení přerušení spojení nebo cluster datacentra restartováním některé nebo všechny repliky oddílu.</span><span class="sxs-lookup"><span data-stu-id="547d8-177">Simulates a datacenter blackout or cluster blackout scenario by restarting some or all replicas of a partition.</span></span> |<span data-ttu-id="547d8-178">RestartPartitionAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-178">RestartPartitionAsync</span></span> |<span data-ttu-id="547d8-179">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="547d8-179">Restart-ServiceFabricPartition</span></span> |<span data-ttu-id="547d8-180">Řádné</span><span class="sxs-lookup"><span data-stu-id="547d8-180">Graceful</span></span> |
| <span data-ttu-id="547d8-181">RestartReplica</span><span class="sxs-lookup"><span data-stu-id="547d8-181">RestartReplica</span></span> |<span data-ttu-id="547d8-182">Repliky selhání simuluje restartování trvalou repliky v clusteru, zavřením hello repliky a pak ho znovu.</span><span class="sxs-lookup"><span data-stu-id="547d8-182">Simulates a replica failure by restarting a persisted replica in a cluster, closing hello replica and then reopening it.</span></span> |<span data-ttu-id="547d8-183">RestartReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-183">RestartReplicaAsync</span></span> |<span data-ttu-id="547d8-184">Restartování ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="547d8-184">Restart-ServiceFabricReplica</span></span> |<span data-ttu-id="547d8-185">Řádné</span><span class="sxs-lookup"><span data-stu-id="547d8-185">Graceful</span></span> |
| <span data-ttu-id="547d8-186">Počáteční_uzel</span><span class="sxs-lookup"><span data-stu-id="547d8-186">StartNode</span></span> |<span data-ttu-id="547d8-187">Spustí uzlu v clusteru, který je již zastaveno.</span><span class="sxs-lookup"><span data-stu-id="547d8-187">Starts a node in a cluster that is already stopped.</span></span> |<span data-ttu-id="547d8-188">StartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-188">StartNodeAsync</span></span> |<span data-ttu-id="547d8-189">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="547d8-189">Start-ServiceFabricNode</span></span> |<span data-ttu-id="547d8-190">Neuvedeno</span><span class="sxs-lookup"><span data-stu-id="547d8-190">Not applicable</span></span> |
| <span data-ttu-id="547d8-191">Příkaz StopNode</span><span class="sxs-lookup"><span data-stu-id="547d8-191">StopNode</span></span> |<span data-ttu-id="547d8-192">Selhání uzlu simuluje podle zastavení uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="547d8-192">Simulates a node failure by stopping a node in a cluster.</span></span> <span data-ttu-id="547d8-193">Hello uzlu zůstanou dolů, dokud se nazývá Počáteční_uzel.</span><span class="sxs-lookup"><span data-stu-id="547d8-193">hello node will stay down until StartNode is called.</span></span> |<span data-ttu-id="547d8-194">StopNodeAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-194">StopNodeAsync</span></span> |<span data-ttu-id="547d8-195">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="547d8-195">Stop-ServiceFabricNode</span></span> |<span data-ttu-id="547d8-196">Vynuceném</span><span class="sxs-lookup"><span data-stu-id="547d8-196">Ungraceful</span></span> |
| <span data-ttu-id="547d8-197">ValidateApplication</span><span class="sxs-lookup"><span data-stu-id="547d8-197">ValidateApplication</span></span> |<span data-ttu-id="547d8-198">Ověří dostupnost hello a stav všech služeb Service Fabric v rámci aplikace, obvykle po vyvolat některé chyby do systému hello.</span><span class="sxs-lookup"><span data-stu-id="547d8-198">Validates hello availability and health of all Service Fabric services within an application, usually after inducing some fault into hello system.</span></span> |<span data-ttu-id="547d8-199">ValidateApplicationAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-199">ValidateApplicationAsync</span></span> |<span data-ttu-id="547d8-200">Test ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="547d8-200">Test-ServiceFabricApplication</span></span> |<span data-ttu-id="547d8-201">Neuvedeno</span><span class="sxs-lookup"><span data-stu-id="547d8-201">Not applicable</span></span> |
| <span data-ttu-id="547d8-202">ValidateService</span><span class="sxs-lookup"><span data-stu-id="547d8-202">ValidateService</span></span> |<span data-ttu-id="547d8-203">Ověří dostupnost hello a stav služby Service Fabric, obvykle po vyvolat některé chyby do systému hello.</span><span class="sxs-lookup"><span data-stu-id="547d8-203">Validates hello availability and health of a Service Fabric service, usually after inducing some fault into hello system.</span></span> |<span data-ttu-id="547d8-204">ValidateServiceAsync</span><span class="sxs-lookup"><span data-stu-id="547d8-204">ValidateServiceAsync</span></span> |<span data-ttu-id="547d8-205">Test ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="547d8-205">Test-ServiceFabricService</span></span> |<span data-ttu-id="547d8-206">Neuvedeno</span><span class="sxs-lookup"><span data-stu-id="547d8-206">Not applicable</span></span> |

## <a name="running-a-testability-action-using-powershell"></a><span data-ttu-id="547d8-207">Spuštění testovatelnosti akci pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="547d8-207">Running a testability action using PowerShell</span></span>
<span data-ttu-id="547d8-208">Tento kurz ukazuje, jak toorun testovatelnosti akce pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="547d8-208">This tutorial shows you how toorun a testability action by using PowerShell.</span></span> <span data-ttu-id="547d8-209">Se dozvíte, jak toorun testovatelnosti akce proti místní cluster (jedna box) nebo Azure clusteru.</span><span class="sxs-lookup"><span data-stu-id="547d8-209">You will learn how toorun a testability action against a local (one-box) cluster or an Azure cluster.</span></span> <span data-ttu-id="547d8-210">Microsoft.Fabric.Powershell.dll – hello modul Service Fabric prostředí PowerShell – se nainstaluje automaticky při instalaci hello Microsoft Service Fabric MSI.</span><span class="sxs-lookup"><span data-stu-id="547d8-210">Microsoft.Fabric.Powershell.dll--hello Service Fabric PowerShell module--is installed automatically when you install hello Microsoft Service Fabric MSI.</span></span> <span data-ttu-id="547d8-211">modul Hello je načten automaticky při otevření příkazovém řádku prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="547d8-211">hello module is loaded automatically when you open a PowerShell prompt.</span></span>

<span data-ttu-id="547d8-212">Kurz segmenty:</span><span class="sxs-lookup"><span data-stu-id="547d8-212">Tutorial segments:</span></span>

* [<span data-ttu-id="547d8-213">Spouštění akce clusteru s podporou jeden pole</span><span class="sxs-lookup"><span data-stu-id="547d8-213">Run an action against a one-box cluster</span></span>](#run-an-action-against-a-one-box-cluster)
* [<span data-ttu-id="547d8-214">Spouštění akce clusteru služby Azure</span><span class="sxs-lookup"><span data-stu-id="547d8-214">Run an action against an Azure cluster</span></span>](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a><span data-ttu-id="547d8-215">Spouštění akce clusteru s podporou jeden pole</span><span class="sxs-lookup"><span data-stu-id="547d8-215">Run an action against a one-box cluster</span></span>
<span data-ttu-id="547d8-216">toorun testovatelnosti opatření proti místní cluster, nejdřív připojit toohello cluster a otevřete hello příkazovém řádku prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="547d8-216">toorun a testability action against a local cluster, first connect toohello cluster and open hello PowerShell prompt in administrator mode.</span></span> <span data-ttu-id="547d8-217">Dejte nám podívejte se na hello **restartování ServiceFabricNode** akce.</span><span class="sxs-lookup"><span data-stu-id="547d8-217">Let us look at hello **Restart-ServiceFabricNode** action.</span></span>

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

<span data-ttu-id="547d8-218">Zde hello akce **restartování ServiceFabricNode** je spuštěn na uzlu s názvem "Uzel1".</span><span class="sxs-lookup"><span data-stu-id="547d8-218">Here hello action **Restart-ServiceFabricNode** is being run on a node named "Node1".</span></span> <span data-ttu-id="547d8-219">režim dokončení Hello Určuje, by neměl ověřit, zda text hello restartování uzlu akce ve skutečnosti úspěšně.</span><span class="sxs-lookup"><span data-stu-id="547d8-219">hello completion mode specifies that it should not verify whether hello restart-node action actually succeeded.</span></span> <span data-ttu-id="547d8-220">Zadání režim dokončení hello jako "Ověřit" způsobí, že tooverify jestli akce restartování hello ve skutečnosti bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="547d8-220">Specifying hello completion mode as "Verify" will cause it tooverify whether hello restart action actually succeeded.</span></span> <span data-ttu-id="547d8-221">Namísto uzlu hello přímo zadat jeho název, můžete zadat pomocí druh oddílu klíč a hello repliky, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="547d8-221">Instead of directly specifying hello node by its name, you can specify it via a partition key and hello kind of replica, as follows:</span></span>

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

<span data-ttu-id="547d8-222">**Restartování ServiceFabricNode** by měla být použité toorestart Service Fabric uzel v clusteru.</span><span class="sxs-lookup"><span data-stu-id="547d8-222">**Restart-ServiceFabricNode** should be used toorestart a Service Fabric node in a cluster.</span></span> <span data-ttu-id="547d8-223">To se zastaví hello Fabric.exe procesu, který restartuje všechny hello systému služby a uživatele služby repliky hostované v tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="547d8-223">This will stop hello Fabric.exe process, which will restart all of hello system service and user service replicas hosted on that node.</span></span> <span data-ttu-id="547d8-224">Pomocí této tootest rozhraní API služby pomáhá odkrýt chyby podél hello převzetí služeb při selhání obnovení cesty.</span><span class="sxs-lookup"><span data-stu-id="547d8-224">Using this API tootest your service helps uncover bugs along hello failover recovery paths.</span></span> <span data-ttu-id="547d8-225">Pomáhá simulace selhání uzlu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="547d8-225">It helps simulate node failures in hello cluster.</span></span>

<span data-ttu-id="547d8-226">Hello následující snímek obrazovky ukazuje hello **restartování ServiceFabricNode** testovatelnosti příkaz v akci.</span><span class="sxs-lookup"><span data-stu-id="547d8-226">hello following screenshot shows hello **Restart-ServiceFabricNode** testability command in action.</span></span>

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

<span data-ttu-id="547d8-227">Hello výstup hello nejprve **Get-ServiceFabricNode** (použití rutiny z modulu Service Fabric prostředí PowerShell hello) zobrazuje, že hello místní cluster s pěti uzly: Node.1 tooNode.5.</span><span class="sxs-lookup"><span data-stu-id="547d8-227">hello output of hello first **Get-ServiceFabricNode** (a cmdlet from hello Service Fabric PowerShell module) shows that hello local cluster has five nodes: Node.1 tooNode.5.</span></span> <span data-ttu-id="547d8-228">Po akci testovatelnosti hello (rutiny) **restartování ServiceFabricNode** se spouští na hello uzel s názvem Node.4, vidíme, tento uzel hello provozu byl obnoven.</span><span class="sxs-lookup"><span data-stu-id="547d8-228">After hello testability action (cmdlet) **Restart-ServiceFabricNode** is executed on hello node, named Node.4, we see that hello node's uptime has been reset.</span></span>

### <a name="run-an-action-against-an-azure-cluster"></a><span data-ttu-id="547d8-229">Spouštění akce clusteru služby Azure</span><span class="sxs-lookup"><span data-stu-id="547d8-229">Run an action against an Azure cluster</span></span>
<span data-ttu-id="547d8-230">Spuštění akci testovatelnosti (pomocí prostředí PowerShell) proti clusteru služby Azure je podobné akce hello toorunning proti místní cluster.</span><span class="sxs-lookup"><span data-stu-id="547d8-230">Running a testability action (by using PowerShell) against an Azure cluster is similar toorunning hello action against a local cluster.</span></span> <span data-ttu-id="547d8-231">Hello jen rozdílem je, že před spuštěním akce hello místo připojování toohello místní cluster, je nutné tooconnect toohello Azure nejprve clusteru.</span><span class="sxs-lookup"><span data-stu-id="547d8-231">hello only difference is that before you can run hello action, instead of connecting toohello local cluster, you need tooconnect toohello Azure cluster first.</span></span>

## <a name="running-a-testability-action-using-c35"></a><span data-ttu-id="547d8-232">Spuštění testovatelnosti akci pomocí C &#35;</span><span class="sxs-lookup"><span data-stu-id="547d8-232">Running a testability action using C&#35;</span></span>
<span data-ttu-id="547d8-233">toorun testovatelnosti akce pomocí jazyka C#, je třeba nejprve tooconnect toohello clusteru pomocí FabricClient.</span><span class="sxs-lookup"><span data-stu-id="547d8-233">toorun a testability action by using C#, first you need tooconnect toohello cluster by using FabricClient.</span></span> <span data-ttu-id="547d8-234">Poté získejte hello potřebné parametry toorun hello akce.</span><span class="sxs-lookup"><span data-stu-id="547d8-234">Then obtain hello parameters needed toorun hello action.</span></span> <span data-ttu-id="547d8-235">Můžete použít jiné parametry toorun hello stejnou akci.</span><span class="sxs-lookup"><span data-stu-id="547d8-235">Different parameters can be used toorun hello same action.</span></span>
<span data-ttu-id="547d8-236">Prohlížení hello RestartServiceFabricNode akce, jedním ze způsobů toorun, je pomocí hello uzlu informace (název uzlu a ID instance uzlu) v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="547d8-236">Looking at hello RestartServiceFabricNode action, one way toorun it is by using hello node information (node name and node instance ID) in hello cluster.</span></span>

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

<span data-ttu-id="547d8-237">Vysvětlení parametr:</span><span class="sxs-lookup"><span data-stu-id="547d8-237">Parameter explanation:</span></span>

* <span data-ttu-id="547d8-238">**CompleteMode** Určuje, že hello režimu by neměl ověřit, zda akce restartování hello ve skutečnosti úspěšně.</span><span class="sxs-lookup"><span data-stu-id="547d8-238">**CompleteMode** specifies that hello mode should not verify whether hello restart action actually succeeded.</span></span> <span data-ttu-id="547d8-239">Zadání režim dokončení hello jako "Ověřit" způsobí, že tooverify jestli akce restartování hello ve skutečnosti bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="547d8-239">Specifying hello completion mode as "Verify" will cause it tooverify whether hello restart action actually succeeded.</span></span>  
* <span data-ttu-id="547d8-240">**OperationTimeout** nastaví hello doba hello operaci toofinish předtím, než je vyvolána výjimka TimeoutException.</span><span class="sxs-lookup"><span data-stu-id="547d8-240">**OperationTimeout** sets hello amount of time for hello operation toofinish before a TimeoutException exception is thrown.</span></span>
* <span data-ttu-id="547d8-241">**CancellationToken** umožňuje čekající volání toobe, došlo ke zrušení.</span><span class="sxs-lookup"><span data-stu-id="547d8-241">**CancellationToken** enables a pending call toobe canceled.</span></span>

<span data-ttu-id="547d8-242">Namísto uzlu hello přímo zadat jeho název, můžete zadat pomocí druh oddílu klíč a hello repliky.</span><span class="sxs-lookup"><span data-stu-id="547d8-242">Instead of directly specifying hello node by its name, you can specify it via a partition key and hello kind of replica.</span></span>

<span data-ttu-id="547d8-243">Další informace najdete v tématu [partitionselector nejde a replicaselector nejde](#partition_replica_selector).</span><span class="sxs-lookup"><span data-stu-id="547d8-243">For further information, see [PartitionSelector and ReplicaSelector](#partition_replica_selector).</span></span>

```csharp
// Add a reference tooSystem.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart hello node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way toorestart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a><span data-ttu-id="547d8-244">Partitionselector nejde a replicaselector nejde</span><span class="sxs-lookup"><span data-stu-id="547d8-244">PartitionSelector and ReplicaSelector</span></span>
### <a name="partitionselector"></a><span data-ttu-id="547d8-245">Partitionselector nejde</span><span class="sxs-lookup"><span data-stu-id="547d8-245">PartitionSelector</span></span>
<span data-ttu-id="547d8-246">Partitionselector nejde je pomocné rutiny v testovatelnosti a je použité tooselect konkrétní rozdělit na oddíly na které tooperform jednu z akcí testovatelnosti hello.</span><span class="sxs-lookup"><span data-stu-id="547d8-246">PartitionSelector is a helper exposed in testability and is used tooselect a specific partition on which tooperform any of hello testability actions.</span></span> <span data-ttu-id="547d8-247">Pokud je ID oddílu hello znám předem, může být použité tooselect na konkrétní oddíl.</span><span class="sxs-lookup"><span data-stu-id="547d8-247">It can be used tooselect a specific partition if hello partition ID is known beforehand.</span></span> <span data-ttu-id="547d8-248">Můžete zadat klíč oddílu hello a hello operace bude vyřešen ID oddílu hello interně.</span><span class="sxs-lookup"><span data-stu-id="547d8-248">Or, you can provide hello partition key and hello operation will resolve hello partition ID internally.</span></span> <span data-ttu-id="547d8-249">Máte také možnost hello výběru náhodného oddílu.</span><span class="sxs-lookup"><span data-stu-id="547d8-249">You also have hello option of selecting a random partition.</span></span>

<span data-ttu-id="547d8-250">toouse tohoto pomocníka vytvořit objekt partitionselector nejde hello a vyberte oddíl hello pomocí jedné z hello vyberte * metody.</span><span class="sxs-lookup"><span data-stu-id="547d8-250">toouse this helper, create hello PartitionSelector object and select hello partition by using one of hello Select* methods.</span></span> <span data-ttu-id="547d8-251">Pak předejte v hello partitionselector nejde objekt toohello rozhraní API, které vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="547d8-251">Then pass in hello PartitionSelector object toohello API that requires it.</span></span> <span data-ttu-id="547d8-252">Pokud je vybraná žádná možnost, bude výchozí tooa náhodných oddílu.</span><span class="sxs-lookup"><span data-stu-id="547d8-252">If no option is selected, it defaults tooa random partition.</span></span>

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a><span data-ttu-id="547d8-253">Replicaselector nejde</span><span class="sxs-lookup"><span data-stu-id="547d8-253">ReplicaSelector</span></span>
<span data-ttu-id="547d8-254">Replicaselector nejde je pomocné rutiny v testovatelnosti a je použité toohelp vyberte repliku, na které tooperform některé hello testovatelnosti akcí.</span><span class="sxs-lookup"><span data-stu-id="547d8-254">ReplicaSelector is a helper exposed in testability and is used toohelp select a replica on which tooperform any of hello testability actions.</span></span> <span data-ttu-id="547d8-255">Pokud je ID repliky hello znám předem, může být použité tooselect konkrétní repliky.</span><span class="sxs-lookup"><span data-stu-id="547d8-255">It can be used tooselect a specific replica if hello replica ID is known beforehand.</span></span> <span data-ttu-id="547d8-256">Kromě toho máte možnost hello výběru primární repliku nebo náhodné sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="547d8-256">In addition, you have hello option of selecting a primary replica or a random secondary.</span></span> <span data-ttu-id="547d8-257">Replicaselector nejde je odvozena z partitionselector nejde, proto musíte tooselect obě hello repliky a hello oddílu, na kterém chcete tooperform hello testovatelnosti operaci.</span><span class="sxs-lookup"><span data-stu-id="547d8-257">ReplicaSelector derives from PartitionSelector, so you need tooselect both hello replica and hello partition on which you wish tooperform hello testability operation.</span></span>

<span data-ttu-id="547d8-258">toouse tohoto pomocníka vytvořit objekt replicaselector nejde a nastavte požadovaným způsobem hello tooselect hello repliky a hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="547d8-258">toouse this helper, create a ReplicaSelector object and set hello way you want tooselect hello replica and hello partition.</span></span> <span data-ttu-id="547d8-259">Pak předejte ji do hello rozhraní API, které vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="547d8-259">You can then pass it into hello API that requires it.</span></span> <span data-ttu-id="547d8-260">Pokud je vybraná žádná možnost, bude výchozí tooa náhodných repliky a náhodných oddílu.</span><span class="sxs-lookup"><span data-stu-id="547d8-260">If no option is selected, it defaults tooa random replica and random partition.</span></span>

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select hello primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select hello replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a><span data-ttu-id="547d8-261">Další kroky</span><span class="sxs-lookup"><span data-stu-id="547d8-261">Next steps</span></span>
* [<span data-ttu-id="547d8-262">Testovatelnosti scénáře</span><span class="sxs-lookup"><span data-stu-id="547d8-262">Testability scenarios</span></span>](service-fabric-testability-scenarios.md)
* <span data-ttu-id="547d8-263">Jak tootest služby</span><span class="sxs-lookup"><span data-stu-id="547d8-263">How tootest your service</span></span>
  * [<span data-ttu-id="547d8-264">Simulace selhání během úloh služeb</span><span class="sxs-lookup"><span data-stu-id="547d8-264">Simulate failures during service workloads</span></span>](service-fabric-testability-workload-tests.md)
  * [<span data-ttu-id="547d8-265">Selhání komunikace Service-to-service</span><span class="sxs-lookup"><span data-stu-id="547d8-265">Service-to-service communication failures</span></span>](service-fabric-testability-scenarios-service-communication.md)

