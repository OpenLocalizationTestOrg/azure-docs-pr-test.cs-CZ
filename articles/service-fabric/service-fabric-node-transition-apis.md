---
title: "aaaStart a zastavení Clusterové uzly tootest Azure mikroslužeb | Microsoft Docs"
description: "Zjistěte, jak toouse poruch vkládání tootest aplikace Service Fabric pomocí spuštění a zastavení uzly clusteru."
services: service-fabric
documentationcenter: .net
author: LMWF
manager: rsinha
editor: 
ms.assetid: f4e70f6f-cad9-4a3e-9655-009b4db09c6d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/12/2017
ms.author: lemai
ms.openlocfilehash: 7d3f5147328e6233a67533fbfb2a525aa5fc060e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replacing-hello-start-node-and-stop-node-apis-with-hello-node-transition-api"></a>Nahraďte hello uzlu spuštění a zastavení uzlu rozhraní API hello rozhraní API pro přechod k uzlu

## <a name="what-do-hello-stop-node-and-start-node-apis-do"></a>Co hello zastavení uzlu a použít rozhraní API pro spuštění uzlu?

Hello zastavení uzlu rozhraní API (spravované: [StopNodeAsync()][stopnode], prostředí PowerShell: [Stop-ServiceFabricNode][stopnodeps]) zastaví uzlem Service Fabric.  Uzel Service Fabric je proces, není virtuálního počítače nebo počítače – hello virtuálního počítače nebo počítače bude stále spuštěna.  Pro hello zbytek dokumentu hello "uzel" znamená Service Fabric uzlu.  Zastavení uzlu vloží je do *zastavena* stavu, kde není členem clusteru hello a nemůže být hostitelem služby, proto simulaci *dolů* uzlu.  To je užitečné pro vložení chyb do hello systému tootest vaší aplikace.  Hello spuštění uzlu rozhraní API (spravované: [StartNodeAsync()][startnode], prostředí PowerShell: [Start-ServiceFabricNode][startnodeps]]) zpětná hello zastavit rozhraní API uzlu  která přináší hello uzel back tooa normálním stavu.

## <a name="why-are-we-replacing-these"></a>Proč to jsme nahradit?

Jak je popsáno výše, *zastavena* Service Fabric uzlu je uzel záměrně cílené pomocí hello zastavení uzlu rozhraní API.  A *dolů* uzel je uzel, který je mimo provoz z jiného důvodu (například hello virtuálního počítače nebo počítač je vypnutý).  S hello zastavení uzlu rozhraní API systému hello nevystavuje toodifferentiate informace mezi *zastavena* uzly a *dolů* uzlů.

Kromě toho nejsou jako popisný také možné, některé chyby vrácené systémem tato rozhraní API.  Například vyvolání hello zastavit API uzlu na element již *zastavena* uzlu vrátí chybu hello *InvalidAddress*.  Toto prostředí lze zlepšit.

Doba trvání hello uzlu je zastaveny pro je také "nekonečné" dokud hello při vyvolání spuštění uzlu rozhraní API.  Jsme zjistili to může způsobit problémy a mohou být náchylné k chybě.  Například jste viděli problémy, kde může uživatel vyvolat hello zastavit API uzel v uzlu a pak zapomněli jste o něm.  Později, bylo jasné, pokud byl uzel hello *dolů* nebo *zastavena*.


## <a name="introducing-hello-node-transition-apis"></a>Představení hello rozhraní API pro přechod k uzlu

Zaměřili jsme se tyto problémy výše v novou sadu rozhraní API.  Hello nové rozhraní API přechod uzlu (spravované: [StartNodeTransitionAsync()][snt]) může být použité tootransition tooa uzel Service Fabric *zastavena* stavu nebo tootransition ho z *zastavena* stavu tooa stavem Normální.  Upozorňujeme, že tento hello "Start" hello název hello API neodkazuje toostarting uzlu.  Odkazuje toobeginning asynchronní operaci, bude systém hello spouštění tootransition hello uzlu tooeither *zastavena* nebo spuštění stavu.

**Využití**

Pokud hello uzlu přechod API nevyvolá výjimku při vyvolání, pak hello systému přijal hello asynchronní operaci a spustí ho.  Úspěšné volání neznamená, že ještě dokončení operace hello.  tooget informace o aktuálním stavu hello hello operace, hello volání rozhraní API průběh přechod uzlu (spravované: [GetNodeTransitionProgressAsync()][gntp]) s identifikátorem guid hello používá při vyvolání uzlu Přechod rozhraní API pro tuto operaci.  Hello uzlu přechod průběh API vrátí objekt NodeTransitionProgress.  Vlastnost stav tohoto objektu určuje aktuální stav hello hello operace.  Pokud stav hello je "systém" hello operace provádí.  Pokud je dokončena, hello operace dokončena bez chyby.  Pokud je s chybou, došlo k potížím, provádění operace hello.  byla výjimka vlastnost výsledek Hello vlastnost označí, jaké hello vydání.  V tématu https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate Další informace o stavu vlastnost hello a části "Využití vzorků" hello níže příklady kódu.


**Rozdíl mezi zastaven uzel a uzel dolů** Pokud uzel je *zastavena* pomocí rozhraní API přechod uzlu, výstup hello uzel dotazu hello (spravované: [GetNodeListAsync()] [ nodequery], Prostředí PowerShell: [Get-ServiceFabricNode][nodequeryps]) se zobrazí, zda má tento uzel *IsStopped* vlastnost hodnotu true.  Poznámka: Tento soubor se liší od hodnoty hello hello *NodeStatus* vlastnosti, která se dozvíte *dolů*.  Pokud hello *NodeStatus* vlastnost má hodnotu *dolů*, ale *IsStopped* je nastavena hodnota false, pak hello uzlu nebyla zastavena, pomocí rozhraní API pro přechod k uzlu hello a  *Dolů* kvůli z jiného důvodu.  Pokud hello *IsStopped* vlastnost má hodnotu true a hello *NodeStatus* vlastnost je *dolů*, pak byla zastavena, pomocí hello rozhraní API pro přechod k uzlu.

Spuštění *zastavena* uzlu pomocí hello uzlu přechod API vrátí ho toofunction jako normální člena clusteru hello znovu.  Hello výstup hello uzel dotazu API bude zobrazovat *IsStopped* jako hodnotu false, a *NodeStatus* jako něco, co není dolů (například nahoru).


**Omezené trvání** při použití hello uzlu přechod API toostop uzlu, jeden z hello požadované parametry, *stopNodeDurationInSeconds*, představuje hello dobu v sekundách tookeep hello uzlu  *Zastavit*.  Tato hodnota musí být v hello povolený rozsah, který má minimálně 600 a maximálně 14400.  Po vypršení této doby hello uzlu se sám do stavem automaticky restartuje.  Příklad použití naleznete tooSample 1 níže.

> [!WARNING]
> Vyhněte se kombinování rozhraní API pro přechod k uzlu a hello API uzlu spuštění a zastavení uzlu.  Hello doporučuje se příliš používat pouze hello rozhraní API pro přechod k uzlu.  > Pokud uzel byl již byla zastavena, pomocí hello zastavení uzlu rozhraní API, ho by měl být spuštěn pomocí hello spustit API uzlu nejprve před použitím hello > rozhraní API pro přechod k uzlu.

> [!WARNING]
> Hello několik rozhraní API uzlu přechodu, které volání nelze provádět na stejném uzlu paralelně.  V takovém případě bude hello rozhraní API pro přechod k uzlu > throw FabricException má hodnotu vlastnosti ErrorCode NodeTransitionInProgress.  Po přechodu uzlu v konkrétním uzlu má > byla spuštěna, měli počkat, až operace hello dosáhne stavu terminálu (dokončeno, Faulted nebo ForceCancelled) před spuštěním > nový přechod na hello stejný uzel.  Paralelní uzlu volání přechod na jiné uzly jsou povoleny.


#### <a name="sample-usage"></a>Využití vzorků


**Příklad 1** -hello hello následující ukázka používá rozhraní API pro přechod k uzlu toostop uzlu.

```csharp
        // Helper function tooget information about a node
        static Node GetNodeInfo(FabricClient fc, string node)
        {
            NodeList n = null;
            while (n == null)
            {
                n = fc.QueryManager.GetNodeListAsync(node).GetAwaiter().GetResult();
                Task.Delay(TimeSpan.FromSeconds(1)).GetAwaiter();
            };

            return n.FirstOrDefault();
        }

        static async Task WaitForStateAsync(FabricClient fc, Guid operationId, TestCommandProgressState targetState)
        {
            NodeTransitionProgress progress = null;

            do
            {
                bool exceptionObserved = false;
                try
                {
                    progress = await fc.TestManager.GetNodeTransitionProgressAsync(operationId, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                    exceptionObserved = true;
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                    exceptionObserved = true;
                }

                if (!exceptionObserved)
                {
                    Console.WriteLine("Current state of operation '{0}': {1}", operationId, progress.State);

                    if (progress.State == TestCommandProgressState.Faulted)
                    {
                        // Inspect hello progress object's Result.Exception.HResult tooget hello error code.
                        Console.WriteLine("'{0}' failed with: {1}, HResult: {2}", operationId, progress.Result.Exception, progress.Result.Exception.HResult);

                        // ...additional logic as required
                    }

                    if (progress.State == targetState)
                    {
                        Console.WriteLine("Target state '{0}' has been reached", targetState);
                        break;
                    }
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (true);
        }

        static async Task StopNodeAsync(FabricClient fc, string nodeName, int durationInSeconds)
        {
            // Uses hello GetNodeListAsync() API tooget information about hello target node
            Node n = GetNodeInfo(fc, nodeName);

            // Create a Guid
            Guid guid = Guid.NewGuid();

            // Create a NodeStopDescription object, which will be used as a parameter into StartNodeTransition
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, durationInSeconds);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStopDescription from above, which will stop hello target node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    // This is retryable
                }
                catch (FabricTransientException fte)
                {
                    // This is retryable
                }

                // Backoff
                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Příklad 2** – hello následující ukázka spustí *zastavena* uzlu.  Některé metody helper od první vzorku hello používá.

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses hello GetNodeListAsync() API tooget information about hello target node
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = n.NodeInstanceId;

            // Create a NodeStartDescription object, which will be used as a parameter into StartNodeTransition
            NodeStartDescription description = new NodeStartDescription(guid, n.NodeName, nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStartDescription from above, which will start hello target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Ukázka 3** – hello následující příklad ukazuje nesprávné použití.  Toto použití je nesprávný protože hello *stopDurationInSeconds* poskytuje je větší než hello povolený rozsah.  Vzhledem k tomu, že StartNodeTransitionAsync() se nezdaří s závažné chybě, hello operaci nebyl přijat a rozhraní API průběh hello by neměl být volán.  Tato ukázka používá některé metody helper od první vzorku hello.

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds toodemonstrate error
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, 99999);

            try
            {
                await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
            }

            catch (FabricException e)
            {
                Console.WriteLine("Caught {0}", e);
                Console.WriteLine("ErrorCode {0}", e.ErrorCode);
                // Output:
                // Caught System.Fabric.FabricException: System.Runtime.InteropServices.COMException (-2147017629)
                // StopDurationInSeconds is out of range ---> System.Runtime.InteropServices.COMException: Exception from HRESULT: 0x80071C63
                // << Parts of exception omitted>>
                //
                // ErrorCode InvalidDuration
            }
        }
```

**Ukázka 4** – hello níže uvedená ukázka uvádí informace o chybě hello, který bude vrácen z hello uzlu přechod průběh rozhraní API při operaci hello iniciovaná hello rozhraní API pro přechod k uzlu byla přijata, ale později při provádění se nezdaří.  V případě hello se nezdaří, protože hello API přechod uzel pokusí toostart uzel, který neexistuje.  Tato ukázka používá některé metody helper od první vzorku hello.

```csharp
        static async Task StartNodeWithNonexistentNodeAsync(FabricClient fc)
        {
            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = 12345;

            // Intentionally use a nonexistent node
            NodeStartDescription description = new NodeStartDescription(guid, "NonexistentNode", nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStartDescription from above, which will start hello target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hello desired state is reached.  In this case, it will end up in hello Faulted state since hello node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect hello progress object's Result.Exception.HResult tooget hello error code.
            // In this case, it will be NodeNotFound.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Faulted).ConfigureAwait(false);
        }
```

[stopnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StopNodeAsync_System_String_System_Numerics_BigInteger_System_Fabric_CompletionMode_
[stopnodeps]: https://msdn.microsoft.com/library/mt125982.aspx
[startnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StartNodeAsync_System_String_System_Numerics_BigInteger_System_String_System_Int32_System_Fabric_CompletionMode_System_Threading_CancellationToken_
[startnodeps]: https://msdn.microsoft.com/library/mt163520.aspx
[nodequery]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient#System_Fabric_FabricClient_QueryClient_GetNodeListAsync_System_String_
[nodequeryps]: https://docs.microsoft.com/powershell/servicefabric/vlatest/Get-ServiceFabricNode?redirectedfrom=msdn
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_StartNodeTransitionAsync_System_Fabric_Description_NodeTransitionDescription_System_TimeSpan_System_Threading_CancellationToken_
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_GetNodeTransitionProgressAsync_System_Guid_System_TimeSpan_System_Threading_CancellationToken_
