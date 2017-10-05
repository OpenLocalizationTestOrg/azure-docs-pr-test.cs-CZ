---
title: "Simulace selhání v Azure mikroslužeb | Microsoft Docs"
description: "V tomto článku bude zmíněn testovatelnosti akce v Microsoft Azure Service Fabric nalezen."
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
ms.openlocfilehash: c8ddc7732999ae555323bebaef60aa34c8f2ec17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="testability-actions"></a><span data-ttu-id="13ed4-103">Testovatelnosti akce</span><span class="sxs-lookup"><span data-stu-id="13ed4-103">Testability actions</span></span>
<span data-ttu-id="13ed4-104">Chcete-li simulovat nespolehlivé infrastruktury, poskytuje Azure Service Fabric vás jako na vývojáři způsoby pro simulaci různé chyby reálného a přechodů mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="13ed4-104">In order to simulate an unreliable infrastructure, Azure Service Fabric provides you, the developer, with ways to simulate various real-world failures and state transitions.</span></span> <span data-ttu-id="13ed4-105">Tyto jsou zveřejněné jako testovatelnosti akce.</span><span class="sxs-lookup"><span data-stu-id="13ed4-105">These are exposed as testability actions.</span></span> <span data-ttu-id="13ed4-106">Akce jsou nízké úrovně rozhraní API, která způsobí ověření, přechod stavu nebo konkrétní chyby vkládání.</span><span class="sxs-lookup"><span data-stu-id="13ed4-106">The actions are the low-level APIs that cause a specific fault injection, state transition, or validation.</span></span> <span data-ttu-id="13ed4-107">Kombinací těchto akcí můžete napsat scénáře komplexní testování pro vaše služby.</span><span class="sxs-lookup"><span data-stu-id="13ed4-107">By combining these actions, you can write comprehensive test scenarios for your services.</span></span>

<span data-ttu-id="13ed4-108">Service Fabric najdete že některé běžné scénáře, test se skládá z těchto akcí.</span><span class="sxs-lookup"><span data-stu-id="13ed4-108">Service Fabric provides some common test scenarios composed of these actions.</span></span> <span data-ttu-id="13ed4-109">Důrazně doporučujeme, aby využíváte těchto předdefinovaných scénáře, které jsou pečlivě zvolili k testování běžné přechodů mezi stavy a selhání případech.</span><span class="sxs-lookup"><span data-stu-id="13ed4-109">We highly recommend that you utilize these built-in scenarios, which are carefully chosen to test common state transitions and failure cases.</span></span> <span data-ttu-id="13ed4-110">Však akce slouží k vytvoření scénáře vlastní testování, když chcete přidat pokrytí pro scénáře, které nejsou předmětem předdefinované scénáře ještě nebo které jsou vlastní, přizpůsobené pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="13ed4-110">However, actions can be used to create custom test scenarios when you want to add coverage for scenarios that are not covered by the built-in scenarios yet or that are custom tailored for your application.</span></span>

<span data-ttu-id="13ed4-111">C# implementace akce, které se nacházejí v sestavení System.Fabric.dll.</span><span class="sxs-lookup"><span data-stu-id="13ed4-111">C# implementations of the actions are found in the System.Fabric.dll assembly.</span></span> <span data-ttu-id="13ed4-112">V sestavení Microsoft.ServiceFabric.Powershell.dll nebyl nalezen modul prostředí PowerShell systému prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="13ed4-112">The System Fabric PowerShell module is found in the Microsoft.ServiceFabric.Powershell.dll assembly.</span></span> <span data-ttu-id="13ed4-113">Jako součást instalace modulu runtime je nainstalován modul ServiceFabric PowerShell umožňující snadné použití.</span><span class="sxs-lookup"><span data-stu-id="13ed4-113">As part of runtime installation, the ServiceFabric PowerShell module is installed to allow for ease of use.</span></span>

## <a name="graceful-vs-ungraceful-fault-actions"></a><span data-ttu-id="13ed4-114">Řádné oproti vynuceném selhání akce</span><span class="sxs-lookup"><span data-stu-id="13ed4-114">Graceful vs. ungraceful fault actions</span></span>
<span data-ttu-id="13ed4-115">Akce testovatelnosti se zařazují do dvou hlavních kbelíků:</span><span class="sxs-lookup"><span data-stu-id="13ed4-115">Testability actions are classified into two major buckets:</span></span>

* <span data-ttu-id="13ed4-116">Vynuceném chyb: Simulace selhání, jako je restartování počítače tyto chyby a zpracování dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="13ed4-116">Ungraceful faults: These faults simulate failures like machine restarts and process crashes.</span></span> <span data-ttu-id="13ed4-117">V takových případech chyb zastaví náhle kontextu spuštění procesu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-117">In such cases of failures, the execution context of process stops abruptly.</span></span> <span data-ttu-id="13ed4-118">To znamená, že žádné vyčištění stavu můžete spustit před spuštěním aplikace znovu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-118">This means no cleanup of the state can run before the application starts up again.</span></span>
* <span data-ttu-id="13ed4-119">Řádné chyb: tyto chyby simulace řádně akcí, jako jsou repliky přesune a vyřazuje aktivovány Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="13ed4-119">Graceful faults: These faults simulate graceful actions like replica moves and drops triggered by load balancing.</span></span> <span data-ttu-id="13ed4-120">V takových případech služba získá oznámení o ukončení a můžete vyčistit stav před ukončením.</span><span class="sxs-lookup"><span data-stu-id="13ed4-120">In such cases, the service gets a notification of the close and can clean up the state before exiting.</span></span>

<span data-ttu-id="13ed4-121">Pro lepší kvalitu ověření spouštění úloh služby a obchodní při vyvolat různé řádně a vynuceném chyb.</span><span class="sxs-lookup"><span data-stu-id="13ed4-121">For better quality validation, run the service and business workload while inducing various graceful and ungraceful faults.</span></span> <span data-ttu-id="13ed4-122">Při vynuceném chyb výkonu scénáře, kde proces služby náhle ukončí uprostřed některé pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-122">Ungraceful faults exercise scenarios where the service process abruptly exits in the middle of some workflow.</span></span> <span data-ttu-id="13ed4-123">To testy cesta obnovení ihned po repliky služby pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="13ed4-123">This tests  the recovery path once the service replica is restored by Service Fabric.</span></span> <span data-ttu-id="13ed4-124">To vám pomůže testování konzistenci dat a zda je stav služby udržovat správně po selhání.</span><span class="sxs-lookup"><span data-stu-id="13ed4-124">This will help test data consistency and whether the service state is maintained correctly after failures.</span></span> <span data-ttu-id="13ed4-125">Další sadu test selhání (řádně počet selhání), který služba správně reaguje na repliky přenášeny pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="13ed4-125">The other set of failures (the graceful failures) test that the service correctly reacts to replicas being moved around by Service Fabric.</span></span> <span data-ttu-id="13ed4-126">V metodě RunAsync to testy zpracování zrušení.</span><span class="sxs-lookup"><span data-stu-id="13ed4-126">This tests handling of cancellation in the RunAsync method.</span></span> <span data-ttu-id="13ed4-127">Služba potřebuje pro kontrolu se nastavit, správně uložit stav token zrušení a ukončete metodě RunAsync.</span><span class="sxs-lookup"><span data-stu-id="13ed4-127">The service needs to check for the cancellation token being set, correctly save its state, and exit the RunAsync method.</span></span>

## <a name="testability-actions-list"></a><span data-ttu-id="13ed4-128">Seznam akcí testovatelnosti</span><span class="sxs-lookup"><span data-stu-id="13ed4-128">Testability actions list</span></span>
| <span data-ttu-id="13ed4-129">Akce</span><span class="sxs-lookup"><span data-stu-id="13ed4-129">Action</span></span> | <span data-ttu-id="13ed4-130">Popis</span><span class="sxs-lookup"><span data-stu-id="13ed4-130">Description</span></span> | <span data-ttu-id="13ed4-131">Spravovaná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="13ed4-131">Managed API</span></span> | <span data-ttu-id="13ed4-132">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="13ed4-132">PowerShell cmdlet</span></span> | <span data-ttu-id="13ed4-133">Řádné/vynuceném chyb</span><span class="sxs-lookup"><span data-stu-id="13ed4-133">Graceful/ungraceful faults</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="13ed4-134">CleanTestState</span><span class="sxs-lookup"><span data-stu-id="13ed4-134">CleanTestState</span></span> |<span data-ttu-id="13ed4-135">Odebere všechny stav testu z clusteru v případě, test ovladače chybné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="13ed4-135">Removes all the test state from the cluster in case of a bad shutdown of the test driver.</span></span> |<span data-ttu-id="13ed4-136">CleanTestStateAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-136">CleanTestStateAsync</span></span> |<span data-ttu-id="13ed4-137">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="13ed4-137">Remove-ServiceFabricTestState</span></span> |<span data-ttu-id="13ed4-138">Neuvedeno</span><span class="sxs-lookup"><span data-stu-id="13ed4-138">Not applicable</span></span> |
| <span data-ttu-id="13ed4-139">InvokeDataLoss</span><span class="sxs-lookup"><span data-stu-id="13ed4-139">InvokeDataLoss</span></span> |<span data-ttu-id="13ed4-140">Indukuje ztráty dat do oddílu služby.</span><span class="sxs-lookup"><span data-stu-id="13ed4-140">Induces data loss into a service partition.</span></span> |<span data-ttu-id="13ed4-141">InvokeDataLossAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-141">InvokeDataLossAsync</span></span> |<span data-ttu-id="13ed4-142">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="13ed4-142">Invoke-ServiceFabricPartitionDataLoss</span></span> |<span data-ttu-id="13ed4-143">Řádné</span><span class="sxs-lookup"><span data-stu-id="13ed4-143">Graceful</span></span> |
| <span data-ttu-id="13ed4-144">InvokeQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="13ed4-144">InvokeQuorumLoss</span></span> |<span data-ttu-id="13ed4-145">Oddíl dané stavové služby umístí do ztrátě kvora.</span><span class="sxs-lookup"><span data-stu-id="13ed4-145">Puts a given stateful service partition into quorum loss.</span></span> |<span data-ttu-id="13ed4-146">InvokeQuorumLossAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-146">InvokeQuorumLossAsync</span></span> |<span data-ttu-id="13ed4-147">Vyvolání ServiceFabricQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="13ed4-147">Invoke-ServiceFabricQuorumLoss</span></span> |<span data-ttu-id="13ed4-148">Řádné</span><span class="sxs-lookup"><span data-stu-id="13ed4-148">Graceful</span></span> |
| <span data-ttu-id="13ed4-149">Přesunutí primární</span><span class="sxs-lookup"><span data-stu-id="13ed4-149">Move Primary</span></span> |<span data-ttu-id="13ed4-150">Přesune zadaný primární repliky stavové služby do určeného uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-150">Moves the specified primary replica of a stateful service to the specified cluster node.</span></span> |<span data-ttu-id="13ed4-151">MovePrimaryAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-151">MovePrimaryAsync</span></span> |<span data-ttu-id="13ed4-152">Přesunutí ServiceFabricPrimaryReplica</span><span class="sxs-lookup"><span data-stu-id="13ed4-152">Move-ServiceFabricPrimaryReplica</span></span> |<span data-ttu-id="13ed4-153">Řádné</span><span class="sxs-lookup"><span data-stu-id="13ed4-153">Graceful</span></span> |
| <span data-ttu-id="13ed4-154">Přesunout sekundární</span><span class="sxs-lookup"><span data-stu-id="13ed4-154">Move Secondary</span></span> |<span data-ttu-id="13ed4-155">Aktuální sekundární replika stavové služby se přesune do jiného uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-155">Moves the current secondary replica of a stateful service to a different cluster node.</span></span> |<span data-ttu-id="13ed4-156">MoveSecondaryAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-156">MoveSecondaryAsync</span></span> |<span data-ttu-id="13ed4-157">Přesunutí ServiceFabricSecondaryReplica</span><span class="sxs-lookup"><span data-stu-id="13ed4-157">Move-ServiceFabricSecondaryReplica</span></span> |<span data-ttu-id="13ed4-158">Řádné</span><span class="sxs-lookup"><span data-stu-id="13ed4-158">Graceful</span></span> |
| <span data-ttu-id="13ed4-159">RemoveReplica</span><span class="sxs-lookup"><span data-stu-id="13ed4-159">RemoveReplica</span></span> |<span data-ttu-id="13ed4-160">Repliky selhání simuluje odebráním repliku z clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-160">Simulates a replica failure by removing a replica from a cluster.</span></span> <span data-ttu-id="13ed4-161">To se zavře repliky a přejde do role 'None', všechny její stav odebrání z clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-161">This will close the replica and will transition it to role 'None', removing all of its state from the cluster.</span></span> |<span data-ttu-id="13ed4-162">RemoveReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-162">RemoveReplicaAsync</span></span> |<span data-ttu-id="13ed4-163">Odebrat ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="13ed4-163">Remove-ServiceFabricReplica</span></span> |<span data-ttu-id="13ed4-164">Řádné</span><span class="sxs-lookup"><span data-stu-id="13ed4-164">Graceful</span></span> |
| <span data-ttu-id="13ed4-165">RestartDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="13ed4-165">RestartDeployedCodePackage</span></span> |<span data-ttu-id="13ed4-166">Simuluje selhání procesu balíček kódu restartováním balíček kódu nasazené na uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-166">Simulates a code package process failure by restarting a code package deployed on a node in a cluster.</span></span> <span data-ttu-id="13ed4-167">To zruší proces balíčku kódu, který restartuje všechny uživatele repliky služby hostované v tomto procesu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-167">This aborts the code package process, which will restart all the user service replicas hosted in that process.</span></span> |<span data-ttu-id="13ed4-168">RestartDeployedCodePackageAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-168">RestartDeployedCodePackageAsync</span></span> |<span data-ttu-id="13ed4-169">Restartování ServiceFabricDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="13ed4-169">Restart-ServiceFabricDeployedCodePackage</span></span> |<span data-ttu-id="13ed4-170">Vynuceném</span><span class="sxs-lookup"><span data-stu-id="13ed4-170">Ungraceful</span></span> |
| <span data-ttu-id="13ed4-171">RestartNode</span><span class="sxs-lookup"><span data-stu-id="13ed4-171">RestartNode</span></span> |<span data-ttu-id="13ed4-172">Selhání uzlu clusteru Service Fabric simuluje restartováním uzlu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-172">Simulates a Service Fabric cluster node failure by restarting a node.</span></span> |<span data-ttu-id="13ed4-173">RestartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-173">RestartNodeAsync</span></span> |<span data-ttu-id="13ed4-174">Restartování ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="13ed4-174">Restart-ServiceFabricNode</span></span> |<span data-ttu-id="13ed4-175">Vynuceném</span><span class="sxs-lookup"><span data-stu-id="13ed4-175">Ungraceful</span></span> |
| <span data-ttu-id="13ed4-176">RestartPartition</span><span class="sxs-lookup"><span data-stu-id="13ed4-176">RestartPartition</span></span> |<span data-ttu-id="13ed4-177">Simuluje scénáři přerušení spojení přerušení spojení nebo cluster datacentra restartováním některé nebo všechny repliky oddílu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-177">Simulates a datacenter blackout or cluster blackout scenario by restarting some or all replicas of a partition.</span></span> |<span data-ttu-id="13ed4-178">RestartPartitionAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-178">RestartPartitionAsync</span></span> |<span data-ttu-id="13ed4-179">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="13ed4-179">Restart-ServiceFabricPartition</span></span> |<span data-ttu-id="13ed4-180">Řádné</span><span class="sxs-lookup"><span data-stu-id="13ed4-180">Graceful</span></span> |
| <span data-ttu-id="13ed4-181">RestartReplica</span><span class="sxs-lookup"><span data-stu-id="13ed4-181">RestartReplica</span></span> |<span data-ttu-id="13ed4-182">Repliky selhání simuluje restartováním trvalou repliky v clusteru, zavření repliky a pak ho znovu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-182">Simulates a replica failure by restarting a persisted replica in a cluster, closing the replica and then reopening it.</span></span> |<span data-ttu-id="13ed4-183">RestartReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-183">RestartReplicaAsync</span></span> |<span data-ttu-id="13ed4-184">Restartování ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="13ed4-184">Restart-ServiceFabricReplica</span></span> |<span data-ttu-id="13ed4-185">Řádné</span><span class="sxs-lookup"><span data-stu-id="13ed4-185">Graceful</span></span> |
| <span data-ttu-id="13ed4-186">Počáteční_uzel</span><span class="sxs-lookup"><span data-stu-id="13ed4-186">StartNode</span></span> |<span data-ttu-id="13ed4-187">Spustí uzlu v clusteru, který je již zastaveno.</span><span class="sxs-lookup"><span data-stu-id="13ed4-187">Starts a node in a cluster that is already stopped.</span></span> |<span data-ttu-id="13ed4-188">StartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-188">StartNodeAsync</span></span> |<span data-ttu-id="13ed4-189">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="13ed4-189">Start-ServiceFabricNode</span></span> |<span data-ttu-id="13ed4-190">Neuvedeno</span><span class="sxs-lookup"><span data-stu-id="13ed4-190">Not applicable</span></span> |
| <span data-ttu-id="13ed4-191">Příkaz StopNode</span><span class="sxs-lookup"><span data-stu-id="13ed4-191">StopNode</span></span> |<span data-ttu-id="13ed4-192">Selhání uzlu simuluje podle zastavení uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-192">Simulates a node failure by stopping a node in a cluster.</span></span> <span data-ttu-id="13ed4-193">Uzel zůstanou dolů, dokud se nazývá Počáteční_uzel.</span><span class="sxs-lookup"><span data-stu-id="13ed4-193">The node will stay down until StartNode is called.</span></span> |<span data-ttu-id="13ed4-194">StopNodeAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-194">StopNodeAsync</span></span> |<span data-ttu-id="13ed4-195">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="13ed4-195">Stop-ServiceFabricNode</span></span> |<span data-ttu-id="13ed4-196">Vynuceném</span><span class="sxs-lookup"><span data-stu-id="13ed4-196">Ungraceful</span></span> |
| <span data-ttu-id="13ed4-197">ValidateApplication</span><span class="sxs-lookup"><span data-stu-id="13ed4-197">ValidateApplication</span></span> |<span data-ttu-id="13ed4-198">Ověří dostupnost a stav všech služeb Service Fabric v rámci aplikace, obvykle po vyvolat některé chyby do systému.</span><span class="sxs-lookup"><span data-stu-id="13ed4-198">Validates the availability and health of all Service Fabric services within an application, usually after inducing some fault into the system.</span></span> |<span data-ttu-id="13ed4-199">ValidateApplicationAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-199">ValidateApplicationAsync</span></span> |<span data-ttu-id="13ed4-200">Test ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="13ed4-200">Test-ServiceFabricApplication</span></span> |<span data-ttu-id="13ed4-201">Neuvedeno</span><span class="sxs-lookup"><span data-stu-id="13ed4-201">Not applicable</span></span> |
| <span data-ttu-id="13ed4-202">ValidateService</span><span class="sxs-lookup"><span data-stu-id="13ed4-202">ValidateService</span></span> |<span data-ttu-id="13ed4-203">Ověří dostupnost a stav služby Service Fabric, obvykle po vyvolat některé chyby do systému.</span><span class="sxs-lookup"><span data-stu-id="13ed4-203">Validates the availability and health of a Service Fabric service, usually after inducing some fault into the system.</span></span> |<span data-ttu-id="13ed4-204">ValidateServiceAsync</span><span class="sxs-lookup"><span data-stu-id="13ed4-204">ValidateServiceAsync</span></span> |<span data-ttu-id="13ed4-205">Test ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="13ed4-205">Test-ServiceFabricService</span></span> |<span data-ttu-id="13ed4-206">Neuvedeno</span><span class="sxs-lookup"><span data-stu-id="13ed4-206">Not applicable</span></span> |

## <a name="running-a-testability-action-using-powershell"></a><span data-ttu-id="13ed4-207">Spuštění testovatelnosti akci pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="13ed4-207">Running a testability action using PowerShell</span></span>
<span data-ttu-id="13ed4-208">V tomto kurzu se dozvíte, jak spustit akci testovatelnosti pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="13ed4-208">This tutorial shows you how to run a testability action by using PowerShell.</span></span> <span data-ttu-id="13ed4-209">Se dozvíte, jak ke spouštění testovatelnosti akce na místní cluster (jedna box) nebo Azure clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-209">You will learn how to run a testability action against a local (one-box) cluster or an Azure cluster.</span></span> <span data-ttu-id="13ed4-210">Microsoft.Fabric.Powershell.dll – modul Service Fabric prostředí PowerShell – je automaticky nainstaluje při instalaci Microsoft Service Fabric MSI.</span><span class="sxs-lookup"><span data-stu-id="13ed4-210">Microsoft.Fabric.Powershell.dll--the Service Fabric PowerShell module--is installed automatically when you install the Microsoft Service Fabric MSI.</span></span> <span data-ttu-id="13ed4-211">Modul je načten automaticky při otevření příkazovém řádku prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="13ed4-211">The module is loaded automatically when you open a PowerShell prompt.</span></span>

<span data-ttu-id="13ed4-212">Kurz segmenty:</span><span class="sxs-lookup"><span data-stu-id="13ed4-212">Tutorial segments:</span></span>

* [<span data-ttu-id="13ed4-213">Spouštění akce clusteru s podporou jeden pole</span><span class="sxs-lookup"><span data-stu-id="13ed4-213">Run an action against a one-box cluster</span></span>](#run-an-action-against-a-one-box-cluster)
* [<span data-ttu-id="13ed4-214">Spouštění akce clusteru služby Azure</span><span class="sxs-lookup"><span data-stu-id="13ed4-214">Run an action against an Azure cluster</span></span>](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a><span data-ttu-id="13ed4-215">Spouštění akce clusteru s podporou jeden pole</span><span class="sxs-lookup"><span data-stu-id="13ed4-215">Run an action against a one-box cluster</span></span>
<span data-ttu-id="13ed4-216">Ke spouštění testovatelnosti akce na místní cluster, nejdřív připojit ke clusteru a otevřete příkazovém řádku prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="13ed4-216">To run a testability action against a local cluster, first connect to the cluster and open the PowerShell prompt in administrator mode.</span></span> <span data-ttu-id="13ed4-217">Dejte nám podívat **restartování ServiceFabricNode** akce.</span><span class="sxs-lookup"><span data-stu-id="13ed4-217">Let us look at the **Restart-ServiceFabricNode** action.</span></span>

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

<span data-ttu-id="13ed4-218">Zde akce **restartování ServiceFabricNode** je spuštěn na uzlu s názvem "Uzel1".</span><span class="sxs-lookup"><span data-stu-id="13ed4-218">Here the action **Restart-ServiceFabricNode** is being run on a node named "Node1".</span></span> <span data-ttu-id="13ed4-219">Režim dokončení Určuje, že by neměl ověřte, zda akce restartování uzlu ve skutečnosti úspěšně.</span><span class="sxs-lookup"><span data-stu-id="13ed4-219">The completion mode specifies that it should not verify whether the restart-node action actually succeeded.</span></span> <span data-ttu-id="13ed4-220">Režim dokončení, jak je "Ověřit" způsobí, že chcete ověřit, jestli akce restartování ve skutečnosti úspěšně.</span><span class="sxs-lookup"><span data-stu-id="13ed4-220">Specifying the completion mode as "Verify" will cause it to verify whether the restart action actually succeeded.</span></span> <span data-ttu-id="13ed4-221">Místo zadání přímo uzlu jeho název, můžete je zadat prostřednictvím klíč oddílu a druh repliky, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="13ed4-221">Instead of directly specifying the node by its name, you can specify it via a partition key and the kind of replica, as follows:</span></span>

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

<span data-ttu-id="13ed4-222">**Restartování ServiceFabricNode** se má použít k restartování Service Fabric uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-222">**Restart-ServiceFabricNode** should be used to restart a Service Fabric node in a cluster.</span></span> <span data-ttu-id="13ed4-223">To se zastaví Fabric.exe procesu, který restartuje všechny systému služby a uživatele služby repliky hostované v tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-223">This will stop the Fabric.exe process, which will restart all of the system service and user service replicas hosted on that node.</span></span> <span data-ttu-id="13ed4-224">Použití toto rozhraní API k testování služby pomáhá odkrýt chyby podél cesty převzetí služeb při selhání obnovení.</span><span class="sxs-lookup"><span data-stu-id="13ed4-224">Using this API to test your service helps uncover bugs along the failover recovery paths.</span></span> <span data-ttu-id="13ed4-225">Pomáhá simulace selhání uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-225">It helps simulate node failures in the cluster.</span></span>

<span data-ttu-id="13ed4-226">Následující snímek obrazovky ukazuje **restartování ServiceFabricNode** testovatelnosti příkaz v akci.</span><span class="sxs-lookup"><span data-stu-id="13ed4-226">The following screenshot shows the **Restart-ServiceFabricNode** testability command in action.</span></span>

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

<span data-ttu-id="13ed4-227">Výstup první **Get-ServiceFabricNode** (rutiny z modulu Service Fabric prostředí PowerShell) zobrazuje, že místní cluster s pěti uzly: Node.1 k Node.5.</span><span class="sxs-lookup"><span data-stu-id="13ed4-227">The output of the first **Get-ServiceFabricNode** (a cmdlet from the Service Fabric PowerShell module) shows that the local cluster has five nodes: Node.1 to Node.5.</span></span> <span data-ttu-id="13ed4-228">Po akci testovatelnosti (rutiny) **restartování ServiceFabricNode** se spustí na uzlu s názvem Node.4, vidíme, že provozu uzlu byl obnoven.</span><span class="sxs-lookup"><span data-stu-id="13ed4-228">After the testability action (cmdlet) **Restart-ServiceFabricNode** is executed on the node, named Node.4, we see that the node's uptime has been reset.</span></span>

### <a name="run-an-action-against-an-azure-cluster"></a><span data-ttu-id="13ed4-229">Spouštění akce clusteru služby Azure</span><span class="sxs-lookup"><span data-stu-id="13ed4-229">Run an action against an Azure cluster</span></span>
<span data-ttu-id="13ed4-230">Spuštění akci testovatelnosti (pomocí prostředí PowerShell) proti clusteru služby Azure je podobná spuštění akce proti místní cluster.</span><span class="sxs-lookup"><span data-stu-id="13ed4-230">Running a testability action (by using PowerShell) against an Azure cluster is similar to running the action against a local cluster.</span></span> <span data-ttu-id="13ed4-231">Jediným rozdílem je, před spuštěním akce, místo připojování k místnímu clusteru, je třeba nejprve připojit ke clusteru Azure.</span><span class="sxs-lookup"><span data-stu-id="13ed4-231">The only difference is that before you can run the action, instead of connecting to the local cluster, you need to connect to the Azure cluster first.</span></span>

## <a name="running-a-testability-action-using-c35"></a><span data-ttu-id="13ed4-232">Spuštění testovatelnosti akci pomocí C &#35;</span><span class="sxs-lookup"><span data-stu-id="13ed4-232">Running a testability action using C&#35;</span></span>
<span data-ttu-id="13ed4-233">Pokud chcete spustit testovatelnosti akce pomocí jazyka C#, musíte se připojit ke clusteru pomocí FabricClient.</span><span class="sxs-lookup"><span data-stu-id="13ed4-233">To run a testability action by using C#, first you need to connect to the cluster by using FabricClient.</span></span> <span data-ttu-id="13ed4-234">Potom získejte parametry potřebné ke spuštění akce.</span><span class="sxs-lookup"><span data-stu-id="13ed4-234">Then obtain the parameters needed to run the action.</span></span> <span data-ttu-id="13ed4-235">Různé parametry slouží ke spuštění stejné akce.</span><span class="sxs-lookup"><span data-stu-id="13ed4-235">Different parameters can be used to run the same action.</span></span>
<span data-ttu-id="13ed4-236">Prohlížení akce RestartServiceFabricNode, jeden způsob, jak spouštět je pomocí uzlu informace (název uzlu a ID instance uzlu) v clusteru.</span><span class="sxs-lookup"><span data-stu-id="13ed4-236">Looking at the RestartServiceFabricNode action, one way to run it is by using the node information (node name and node instance ID) in the cluster.</span></span>

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

<span data-ttu-id="13ed4-237">Vysvětlení parametr:</span><span class="sxs-lookup"><span data-stu-id="13ed4-237">Parameter explanation:</span></span>

* <span data-ttu-id="13ed4-238">**CompleteMode** Určuje, že režim nesmí ověřit, zda akce restartování ve skutečnosti úspěšně.</span><span class="sxs-lookup"><span data-stu-id="13ed4-238">**CompleteMode** specifies that the mode should not verify whether the restart action actually succeeded.</span></span> <span data-ttu-id="13ed4-239">Režim dokončení, jak je "Ověřit" způsobí, že chcete ověřit, jestli akce restartování ve skutečnosti úspěšně.</span><span class="sxs-lookup"><span data-stu-id="13ed4-239">Specifying the completion mode as "Verify" will cause it to verify whether the restart action actually succeeded.</span></span>  
* <span data-ttu-id="13ed4-240">**OperationTimeout** Nastaví množství času pro operace dokončeno předtím, než je vyvolána výjimka TimeoutException.</span><span class="sxs-lookup"><span data-stu-id="13ed4-240">**OperationTimeout** sets the amount of time for the operation to finish before a TimeoutException exception is thrown.</span></span>
* <span data-ttu-id="13ed4-241">**CancellationToken** umožňuje čekající volání ke zrušení.</span><span class="sxs-lookup"><span data-stu-id="13ed4-241">**CancellationToken** enables a pending call to be canceled.</span></span>

<span data-ttu-id="13ed4-242">Místo zadání přímo uzlu jeho název, můžete je zadat prostřednictvím klíč oddílu a druh repliky.</span><span class="sxs-lookup"><span data-stu-id="13ed4-242">Instead of directly specifying the node by its name, you can specify it via a partition key and the kind of replica.</span></span>

<span data-ttu-id="13ed4-243">Další informace najdete v tématu [partitionselector nejde a replicaselector nejde](#partition_replica_selector).</span><span class="sxs-lookup"><span data-stu-id="13ed4-243">For further information, see [PartitionSelector and ReplicaSelector](#partition_replica_selector).</span></span>

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
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
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
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

## <a name="partitionselector-and-replicaselector"></a><span data-ttu-id="13ed4-244">Partitionselector nejde a replicaselector nejde</span><span class="sxs-lookup"><span data-stu-id="13ed4-244">PartitionSelector and ReplicaSelector</span></span>
### <a name="partitionselector"></a><span data-ttu-id="13ed4-245">Partitionselector nejde</span><span class="sxs-lookup"><span data-stu-id="13ed4-245">PartitionSelector</span></span>
<span data-ttu-id="13ed4-246">Partitionselector nejde je pomocné rutiny v testovatelnosti a slouží k výběru konkrétního oddílu, na které se mají provádět operace testovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="13ed4-246">PartitionSelector is a helper exposed in testability and is used to select a specific partition on which to perform any of the testability actions.</span></span> <span data-ttu-id="13ed4-247">Slouží k výběru na konkrétní oddíl, pokud je předem znám ID oddílu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-247">It can be used to select a specific partition if the partition ID is known beforehand.</span></span> <span data-ttu-id="13ed4-248">Nebo můžete zadat klíč oddílu a operaci interně vyřešit ID oddílu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-248">Or, you can provide the partition key and the operation will resolve the partition ID internally.</span></span> <span data-ttu-id="13ed4-249">Máte také možnost výběru náhodného oddílu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-249">You also have the option of selecting a random partition.</span></span>

<span data-ttu-id="13ed4-250">Pokud chcete použít tohoto pomocníka, vytvořit objekt partitionselector nejde a vyberte oddíl, a to pomocí jedné z metod vyberte *.</span><span class="sxs-lookup"><span data-stu-id="13ed4-250">To use this helper, create the PartitionSelector object and select the partition by using one of the Select* methods.</span></span> <span data-ttu-id="13ed4-251">Pak předejte v objektu partitionselector nejde do rozhraní API, kterého se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="13ed4-251">Then pass in the PartitionSelector object to the API that requires it.</span></span> <span data-ttu-id="13ed4-252">Pokud je vybraná žádná možnost, bude výchozí náhodných oddílu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-252">If no option is selected, it defaults to a random partition.</span></span>

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

### <a name="replicaselector"></a><span data-ttu-id="13ed4-253">Replicaselector nejde</span><span class="sxs-lookup"><span data-stu-id="13ed4-253">ReplicaSelector</span></span>
<span data-ttu-id="13ed4-254">Replicaselector nejde je pomocné rutiny v testovatelnosti a slouží k zajištění vyberte repliku, na které se mají provádět operace testovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="13ed4-254">ReplicaSelector is a helper exposed in testability and is used to help select a replica on which to perform any of the testability actions.</span></span> <span data-ttu-id="13ed4-255">Může sloužit k vyberte konkrétní repliku, pokud je ID repliky předem znám.</span><span class="sxs-lookup"><span data-stu-id="13ed4-255">It can be used to select a specific replica if the replica ID is known beforehand.</span></span> <span data-ttu-id="13ed4-256">Kromě toho máte možnost výběru primární repliku nebo náhodné sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="13ed4-256">In addition, you have the option of selecting a primary replica or a random secondary.</span></span> <span data-ttu-id="13ed4-257">Replicaselector nejde je odvozena z partitionselector nejde, takže je nutné vybrat repliky a oddílu, na kterém chcete provést operaci testovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="13ed4-257">ReplicaSelector derives from PartitionSelector, so you need to select both the replica and the partition on which you wish to perform the testability operation.</span></span>

<span data-ttu-id="13ed4-258">Chcete-li použít tohoto pomocníka, vytvořit objekt replicaselector nejde a nastavte způsob, jakým chcete vybrat repliky a oddíl.</span><span class="sxs-lookup"><span data-stu-id="13ed4-258">To use this helper, create a ReplicaSelector object and set the way you want to select the replica and the partition.</span></span> <span data-ttu-id="13ed4-259">Pak předejte ji do rozhraní API, kterého se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="13ed4-259">You can then pass it into the API that requires it.</span></span> <span data-ttu-id="13ed4-260">Pokud je vybraná žádná možnost, bude výchozí náhodných repliky a náhodných oddílu.</span><span class="sxs-lookup"><span data-stu-id="13ed4-260">If no option is selected, it defaults to a random replica and random partition.</span></span>

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a><span data-ttu-id="13ed4-261">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13ed4-261">Next steps</span></span>
* [<span data-ttu-id="13ed4-262">Testovatelnosti scénáře</span><span class="sxs-lookup"><span data-stu-id="13ed4-262">Testability scenarios</span></span>](service-fabric-testability-scenarios.md)
* <span data-ttu-id="13ed4-263">Postup testování služby</span><span class="sxs-lookup"><span data-stu-id="13ed4-263">How to test your service</span></span>
  * [<span data-ttu-id="13ed4-264">Simulace selhání během úloh služeb</span><span class="sxs-lookup"><span data-stu-id="13ed4-264">Simulate failures during service workloads</span></span>](service-fabric-testability-workload-tests.md)
  * [<span data-ttu-id="13ed4-265">Selhání komunikace Service-to-service</span><span class="sxs-lookup"><span data-stu-id="13ed4-265">Service-to-service communication failures</span></span>](service-fabric-testability-scenarios-service-communication.md)

