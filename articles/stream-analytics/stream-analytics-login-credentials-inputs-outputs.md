---
title: "Stream Analytics: Otáčení protokolu pověření pro vstupy a výstupy | Microsoft Docs"
description: "Zjistěte, jak tooupdate hello přihlašovací údaje pro Stream Analytics vstupy a výstupy."
keywords: "přihlašovací údaje"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: ac2374c539012b66ab390656c5750024e02f6bdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a><span data-ttu-id="4494b-104">Otočit přihlašovací údaje pro vstupy a výstupy úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4494b-104">Rotate login credentials for inputs and outputs in Stream Analytics Jobs</span></span>
## <a name="abstract"></a><span data-ttu-id="4494b-105">Abstraktní</span><span class="sxs-lookup"><span data-stu-id="4494b-105">Abstract</span></span>
<span data-ttu-id="4494b-106">Azure Stream Analytics dnes neumožňuje nahrazení hello přihlašovacích údajů na vstupu a výstupu, když je spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="4494b-106">Azure Stream Analytics today doesn’t allow replacing hello credentials on an input/output while hello job is running.</span></span>

<span data-ttu-id="4494b-107">Zatímco Azure Stream Analytics podporuje obnovení úlohy z posledního výstupu, jsme chtěli tooshare hello celý proces pro minimalizaci hello prodleva mezi hello zastavení a spuštění úlohy hello a otáčení hello přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="4494b-107">While Azure Stream Analytics does support resuming a job from last output, we wanted tooshare hello entire process for minimizing hello lag between hello stopping and starting of hello job and rotating hello login credentials.</span></span>

## <a name="part-1---prepare-hello-new-set-of-credentials"></a><span data-ttu-id="4494b-108">Část 1 – Příprava hello novou sadu přihlašovacích údajů:</span><span class="sxs-lookup"><span data-stu-id="4494b-108">Part 1 - Prepare hello new set of credentials:</span></span>
<span data-ttu-id="4494b-109">Tato část se vztahuje toohello následující vstupy/výstupy:</span><span class="sxs-lookup"><span data-stu-id="4494b-109">This part is applicable toohello following inputs/outputs:</span></span>

* <span data-ttu-id="4494b-110">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="4494b-110">Blob Storage</span></span>
* <span data-ttu-id="4494b-111">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="4494b-111">Event Hubs</span></span>
* <span data-ttu-id="4494b-112">SQL Database</span><span class="sxs-lookup"><span data-stu-id="4494b-112">SQL Database</span></span>
* <span data-ttu-id="4494b-113">Table Storage</span><span class="sxs-lookup"><span data-stu-id="4494b-113">Table Storage</span></span>

<span data-ttu-id="4494b-114">Pro ostatní vstupy/výstupy pokračujte část 2.</span><span class="sxs-lookup"><span data-stu-id="4494b-114">For other inputs/outputs, proceed with Part 2.</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="4494b-115">Úložiště objektů BLOB úložiště/tabulky</span><span class="sxs-lookup"><span data-stu-id="4494b-115">Blob storage/Table storage</span></span>
1. <span data-ttu-id="4494b-116">Přejdete na portál pro správu Azure hello toohello úložiště rozšíření:</span><span class="sxs-lookup"><span data-stu-id="4494b-116">Go toohello Storage extention on hello Azure Management portal:</span></span>  
   ![graphic1][graphic1]
2. <span data-ttu-id="4494b-118">Vyhledejte hello úložiště, které používá vaše úloha a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="4494b-118">Locate hello storage used by your job and go into it:</span></span>  
   ![graphic2][graphic2]
3. <span data-ttu-id="4494b-120">Klikněte na příkaz Spravovat přístupové klíče hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-120">Click hello Manage Access Keys command:</span></span>  
   ![graphic3][graphic3]
4. <span data-ttu-id="4494b-122">Mezi hello primární přístupový klíč a hello sekundární přístupový klíč **vyberte hello jeden není používá vaše úloha**.</span><span class="sxs-lookup"><span data-stu-id="4494b-122">Between hello Primary Access Key and hello Secondary Access Key, **pick hello one not used by your job**.</span></span>
5. <span data-ttu-id="4494b-123">Stiskněte tlačítko znovu vygenerovat:</span><span class="sxs-lookup"><span data-stu-id="4494b-123">Hit regenerate:</span></span>  
   ![graphic4][graphic4]
6. <span data-ttu-id="4494b-125">Zkopírujte klíč hello nově vygenerované:</span><span class="sxs-lookup"><span data-stu-id="4494b-125">Copy hello newly generated key:</span></span>  
   ![graphic5][graphic5]
7. <span data-ttu-id="4494b-127">Pokračujte tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="4494b-127">Continue tooPart 2.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="4494b-128">Služba Event hubs</span><span class="sxs-lookup"><span data-stu-id="4494b-128">Event hubs</span></span>
1. <span data-ttu-id="4494b-129">Rozšíření Service Bus toohello přejdete na portál pro správu Azure hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-129">Go toohello Service Bus extension on hello Azure Management portal:</span></span>  
   ![graphic6][graphic6]
2. <span data-ttu-id="4494b-131">Vyhledejte hello Namespace Service Bus používá vaše úloha a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="4494b-131">Locate hello Service Bus Namespace used by your job and go into it:</span></span>  
   ![graphic7][graphic7]
3. <span data-ttu-id="4494b-133">Pokud vaše úlohy používá zásady sdíleného přístupu v hello Namespace Service Bus, přeskočit toostep 6</span><span class="sxs-lookup"><span data-stu-id="4494b-133">If your job uses a shared access policy on hello Service Bus Namespace, jump toostep 6</span></span>  
4. <span data-ttu-id="4494b-134">Přejdete toohello Event Hubs karty:</span><span class="sxs-lookup"><span data-stu-id="4494b-134">Go toohello Event Hubs tab:</span></span>  
   ![graphic8][graphic8]
5. <span data-ttu-id="4494b-136">Vyhledejte hello centra událostí, které používá vaše úloha a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="4494b-136">Locate hello Event Hub used by your job and go into it:</span></span>  
   ![graphic9][graphic9]
6. <span data-ttu-id="4494b-138">Přejdete toohello karta konfigurace:</span><span class="sxs-lookup"><span data-stu-id="4494b-138">Go toohello Configure Tab:</span></span>  
   ![graphic10][graphic10]
7. <span data-ttu-id="4494b-140">V hello rozevíracího seznamu název zásady najděte hello sdílené zásady přístupu používá vaše úloha:</span><span class="sxs-lookup"><span data-stu-id="4494b-140">On hello Policy Name drop-down, locate hello shared access policy used by your job:</span></span>  
   ![graphic11][graphic11]
8. <span data-ttu-id="4494b-142">Mezi hello primární klíč a sekundární klíč hello **vyberte hello jeden není používá vaše úloha**.</span><span class="sxs-lookup"><span data-stu-id="4494b-142">Between hello Primary Key and hello Secondary Key, **pick hello one not used by your job**.</span></span>  
9. <span data-ttu-id="4494b-143">Stiskněte tlačítko znovu vygenerovat:</span><span class="sxs-lookup"><span data-stu-id="4494b-143">Hit regenerate:</span></span>  
   ![graphic12][graphic12]
10. <span data-ttu-id="4494b-145">Zkopírujte klíč hello nově vygenerované:</span><span class="sxs-lookup"><span data-stu-id="4494b-145">Copy hello newly generated key:</span></span>  
   ![graphic13][graphic13]
11. <span data-ttu-id="4494b-147">Pokračujte tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="4494b-147">Continue tooPart 2.</span></span>  

### <a name="sql-database"></a><span data-ttu-id="4494b-148">SQL Database</span><span class="sxs-lookup"><span data-stu-id="4494b-148">SQL Database</span></span>
> [!NOTE]
> <span data-ttu-id="4494b-149">Poznámka: budete potřebovat tooconnect toohello služby databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="4494b-149">Note: you will need tooconnect toohello SQL Database Service.</span></span> <span data-ttu-id="4494b-150">Přidáme tooshow jak toodo této konfigurace pomocí prostředí správy hello na hello portál pro správu Azure, ale můžete rozhodnout toouse některé klientské nástroje, jako je SQL Server Management Studio také.</span><span class="sxs-lookup"><span data-stu-id="4494b-150">We are going tooshow how toodo this using hello management experience on hello Azure Management portal but you may choose toouse some client-side tool such as SQL Server Management Studio as well.</span></span>
>
> 

1. <span data-ttu-id="4494b-151">Rozšíření toohello databází SQL, přejdete na portál pro správu Azure hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-151">Go toohello SQL Databases extension on hello Azure Management portal:</span></span>  
   ![graphic14][graphic14]
2. <span data-ttu-id="4494b-153">Vyhledejte hello databázi SQL používanou nástrojem úlohu a **klikněte na hello server** odkaz na hello stejný řádek:</span><span class="sxs-lookup"><span data-stu-id="4494b-153">Locate hello SQL Database used by your job and **click on hello server** link on hello same line:</span></span>  
   <span data-ttu-id="4494b-154">![graphic15][graphic15]</span><span class="sxs-lookup"><span data-stu-id="4494b-154">![graphic15][graphic15]</span></span>
3. <span data-ttu-id="4494b-155">Klikněte na příkaz Spravovat hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-155">Click hello Manage command:</span></span>  
   ![graphic16][graphic16]
4. <span data-ttu-id="4494b-157">Hlavní databáze typu:</span><span class="sxs-lookup"><span data-stu-id="4494b-157">Type Database Master:</span></span>  
   ![graphic17][graphic17]
5. <span data-ttu-id="4494b-159">Zadejte uživatelské jméno, heslo a klikněte na protokolu:</span><span class="sxs-lookup"><span data-stu-id="4494b-159">Type in your User Name, Password, and click Log on:</span></span>  
   ![graphic18][graphic18]
6. <span data-ttu-id="4494b-161">Klikněte na nový dotaz:</span><span class="sxs-lookup"><span data-stu-id="4494b-161">Click New Query:</span></span>  
   ![graphic19][graphic19]
7. <span data-ttu-id="4494b-163">Typ v hello následující dotaz nahrazení < login_name > svým uživatelským jménem a nahrazení <enterStrongPasswordHere> pomocí nového hesla:</span><span class="sxs-lookup"><span data-stu-id="4494b-163">Type in hello following query replacing <login_name> with your User Name and replacing <enterStrongPasswordHere> with your new password:</span></span>  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. <span data-ttu-id="4494b-164">Klikněte na tlačítko spustit:</span><span class="sxs-lookup"><span data-stu-id="4494b-164">Click Run:</span></span>  
   ![graphic20][graphic20]
9. <span data-ttu-id="4494b-166">Vraťte toostep 2 a Tentokrát klikněte na databázi hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-166">Go back toostep 2 and this time click hello database:</span></span>  
   ![graphic21][graphic21]
10. <span data-ttu-id="4494b-168">Klikněte na příkaz Spravovat hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-168">Click hello Manage command:</span></span>  
   ![graphic22][graphic22]
11. <span data-ttu-id="4494b-170">Zadejte uživatelské jméno, heslo a klikněte na možnost přihlášení:</span><span class="sxs-lookup"><span data-stu-id="4494b-170">type in your User Name, Password, and click Logon:</span></span>  
   ![graphic23][graphic23]
12. <span data-ttu-id="4494b-172">Klikněte na nový dotaz:</span><span class="sxs-lookup"><span data-stu-id="4494b-172">Click New Query:</span></span>  
   ![graphic24][graphic24]
13. <span data-ttu-id="4494b-174">Zadejte následující dotaz nahrazení < uživatelské_jméno > s názvem, podle kterého chcete tooidentify hello toto přihlášení v kontextu hello této databáze (můžete zadat stejnou hodnotu, která jste zadali pro < login_name >, například hello) a nahraďte < login_name > nové uživatelské jméno:</span><span class="sxs-lookup"><span data-stu-id="4494b-174">Type in hello following query replacing <user_name> with a name by which you want tooidentify this login in hello context of this database (you can provide hello same value you gave for <login_name>, for example) and replacing <login_name> with your new user name:</span></span>  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. <span data-ttu-id="4494b-175">Klikněte na tlačítko spustit:</span><span class="sxs-lookup"><span data-stu-id="4494b-175">Click Run:</span></span>  
   ![graphic25][graphic25]
15. <span data-ttu-id="4494b-177">Nový uživatel by měl nyní poskytovat s hello stejné role a oprávnění měl původního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4494b-177">You should now provide your new user with hello same roles and privileges your original user had.</span></span>
16. <span data-ttu-id="4494b-178">Pokračujte tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="4494b-178">Continue tooPart 2.</span></span>

## <a name="part-2-stopping-hello-stream-analytics-job"></a><span data-ttu-id="4494b-179">Část 2: Zastavení hello úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4494b-179">Part 2: Stopping hello Stream Analytics Job</span></span>
1. <span data-ttu-id="4494b-180">Přejdete na portál pro správu Azure hello toohello Stream Analytics rozšíření:</span><span class="sxs-lookup"><span data-stu-id="4494b-180">Go toohello Stream Analytics extension on hello Azure Management portal:</span></span>  
   ![graphic26][graphic26]
2. <span data-ttu-id="4494b-182">Najděte úlohu a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="4494b-182">Locate your job and go into it:</span></span>  
   ![graphic27][graphic27]
3. <span data-ttu-id="4494b-184">Přejděte toohello vstupy kartě nebo hello výstupů založené na tom, jestli jsou otáčení hello pověření na vstup nebo výstup.</span><span class="sxs-lookup"><span data-stu-id="4494b-184">Go toohello Inputs tab or hello Outputs tab based on whether you are rotating hello credentials on an Input or on an Output.</span></span>  
   ![graphic28][graphic28]
4. <span data-ttu-id="4494b-186">Klikněte na příkaz Stop hello a potvrďte, že hello úloha byla zastavena:</span><span class="sxs-lookup"><span data-stu-id="4494b-186">Click hello Stop command and confirm hello job has stopped:</span></span>  
   <span data-ttu-id="4494b-187">![graphic29][graphic29] počkejte toostop úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="4494b-187">![graphic29][graphic29] Wait for hello job toostop.</span></span>
5. <span data-ttu-id="4494b-188">Vyhledejte hello vstupu a výstupu chcete toorotate pověření na a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="4494b-188">Locate hello input/output you want toorotate credentials on and go into it:</span></span>  
   ![graphic30][graphic30]
6. <span data-ttu-id="4494b-190">Pokračujte tooPart 3.</span><span class="sxs-lookup"><span data-stu-id="4494b-190">Proceed tooPart 3.</span></span>

## <a name="part-3-editing-hello-credentials-on-hello-stream-analytics-job"></a><span data-ttu-id="4494b-191">Část 3: Úprava hello přihlašovací údaje u hello úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4494b-191">Part 3: Editing hello credentials on hello Stream Analytics Job</span></span>
### <a name="blob-storagetable-storage"></a><span data-ttu-id="4494b-192">Úložiště objektů BLOB úložiště/tabulky</span><span class="sxs-lookup"><span data-stu-id="4494b-192">Blob storage/Table storage</span></span>
1. <span data-ttu-id="4494b-193">Najít pole hello klíč účtu úložiště a vložte do něj nově vygenerovaný klíč:</span><span class="sxs-lookup"><span data-stu-id="4494b-193">Find hello Storage Account Key field and paste your newly generated key into it:</span></span>  
   ![graphic31][graphic31]
2. <span data-ttu-id="4494b-195">Klikněte na příkaz Uložit hello a potvrďte uložení změn:</span><span class="sxs-lookup"><span data-stu-id="4494b-195">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic32][graphic32]
3. <span data-ttu-id="4494b-197">Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že je úspěšně byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="4494b-197">A connection test will automatically start when you save your changes, make sure that is has successfully passed.</span></span>
4. <span data-ttu-id="4494b-198">Pokračujte tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="4494b-198">Proceed tooPart 4.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="4494b-199">Služba Event hubs</span><span class="sxs-lookup"><span data-stu-id="4494b-199">Event hubs</span></span>
1. <span data-ttu-id="4494b-200">Najít pole hello klíč události rozbočovače zásady a vložit vaše nově vygenerovaný klíč do ní:</span><span class="sxs-lookup"><span data-stu-id="4494b-200">Find hello Event Hub Policy Key field and paste your newly generated key into it:</span></span>  
   ![graphic33][graphic33]
2. <span data-ttu-id="4494b-202">Klikněte na příkaz Uložit hello a potvrďte uložení změn:</span><span class="sxs-lookup"><span data-stu-id="4494b-202">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic34][graphic34]
3. <span data-ttu-id="4494b-204">Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že má úspěšně úspěšně.</span><span class="sxs-lookup"><span data-stu-id="4494b-204">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>
4. <span data-ttu-id="4494b-205">Pokračujte tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="4494b-205">Proceed tooPart 4.</span></span>

### <a name="power-bi"></a><span data-ttu-id="4494b-206">Power BI</span><span class="sxs-lookup"><span data-stu-id="4494b-206">Power BI</span></span>
1. <span data-ttu-id="4494b-207">Klikněte na tlačítko Obnovit autorizace hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-207">Click hello Renew authorization:</span></span>  

   ![graphic35][graphic35]
2. <span data-ttu-id="4494b-209">Zobrazí se následující potvrzení hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-209">You will get hello following confirmation:</span></span>  

   ![graphic36][graphic36]
3. <span data-ttu-id="4494b-211">Klikněte na příkaz Uložit hello a potvrďte uložení změn:</span><span class="sxs-lookup"><span data-stu-id="4494b-211">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic37][graphic37]
4. <span data-ttu-id="4494b-213">Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že úspěšně úspěšně prošel.</span><span class="sxs-lookup"><span data-stu-id="4494b-213">A connection test will automatically start when you save your changes, make sure it has successfully passed.</span></span>
5. <span data-ttu-id="4494b-214">Pokračujte tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="4494b-214">Proceed tooPart 4.</span></span>

### <a name="sql-database"></a><span data-ttu-id="4494b-215">SQL Database</span><span class="sxs-lookup"><span data-stu-id="4494b-215">SQL Database</span></span>
1. <span data-ttu-id="4494b-216">Najde hello pole uživatelské jméno a heslo a vkládání do nich vaše nově vytvořenou sadu přihlašovacích údajů:</span><span class="sxs-lookup"><span data-stu-id="4494b-216">Find hello User Name and Password fields and paste your newly created set of credentials into them:</span></span>  
   ![graphic38][graphic38]
2. <span data-ttu-id="4494b-218">Klikněte na příkaz Uložit hello a potvrďte uložení změn:</span><span class="sxs-lookup"><span data-stu-id="4494b-218">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic39][graphic39]
3. <span data-ttu-id="4494b-220">Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že má úspěšně úspěšně.</span><span class="sxs-lookup"><span data-stu-id="4494b-220">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>  
4. <span data-ttu-id="4494b-221">Pokračujte tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="4494b-221">Proceed tooPart 4.</span></span>

## <a name="part-4-starting-your-job-from-last-stopped-time"></a><span data-ttu-id="4494b-222">Část 4: Od poslední zastaven úlohu</span><span class="sxs-lookup"><span data-stu-id="4494b-222">Part 4: Starting your job from last stopped time</span></span>
1. <span data-ttu-id="4494b-223">Opustit hello vstupu a výstupu:</span><span class="sxs-lookup"><span data-stu-id="4494b-223">Navigate away from hello Input/Output:</span></span>  
   ![graphic40][graphic40]
2. <span data-ttu-id="4494b-225">Klikněte na příkaz spuštění hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-225">Click hello Start command:</span></span>  
   ![graphic41][graphic41]
3. <span data-ttu-id="4494b-227">Vyberte hello naposledy zastaveno a klikněte na tlačítko OK:</span><span class="sxs-lookup"><span data-stu-id="4494b-227">Pick hello Last Stopped Time and click OK:</span></span>  
   ![graphic42][graphic42]
4. <span data-ttu-id="4494b-229">Pokračujte tooPart 5.</span><span class="sxs-lookup"><span data-stu-id="4494b-229">Proceed tooPart 5.</span></span>  

## <a name="part-5-removing-hello-old-set-of-credentials"></a><span data-ttu-id="4494b-230">Část 5: Odebrání hello původní sadu přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="4494b-230">Part 5: Removing hello old set of credentials</span></span>
<span data-ttu-id="4494b-231">Tato část se vztahuje toohello následující vstupy/výstupy:</span><span class="sxs-lookup"><span data-stu-id="4494b-231">This part is applicable toohello following inputs/outputs:</span></span>

* <span data-ttu-id="4494b-232">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="4494b-232">Blob Storage</span></span>
* <span data-ttu-id="4494b-233">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="4494b-233">Event Hubs</span></span>
* <span data-ttu-id="4494b-234">SQL Database</span><span class="sxs-lookup"><span data-stu-id="4494b-234">SQL Database</span></span>
* <span data-ttu-id="4494b-235">Table Storage</span><span class="sxs-lookup"><span data-stu-id="4494b-235">Table Storage</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="4494b-236">Úložiště objektů BLOB úložiště/tabulky</span><span class="sxs-lookup"><span data-stu-id="4494b-236">Blob storage/Table storage</span></span>
<span data-ttu-id="4494b-237">Opakujte část 1 pro hello přístupový klíč, který byl dříve používán vaše úloha toorenew hello teď nepoužívá přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="4494b-237">Repeat Part 1 for hello Access Key that was previously used by your job toorenew hello now unused Access Key.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="4494b-238">Služba Event hubs</span><span class="sxs-lookup"><span data-stu-id="4494b-238">Event hubs</span></span>
<span data-ttu-id="4494b-239">Opakujte část 1 pro hello klíč, který byl dříve používán vaše úloha toorenew hello teď nepoužívá klíč.</span><span class="sxs-lookup"><span data-stu-id="4494b-239">Repeat Part 1 for hello Key that was previously used by your job toorenew hello now unused Key.</span></span>

### <a name="sql-database"></a><span data-ttu-id="4494b-240">SQL Database</span><span class="sxs-lookup"><span data-stu-id="4494b-240">SQL Database</span></span>
1. <span data-ttu-id="4494b-241">Přejděte zpět okno dotazu toohello z část 1 kroku 7 a zadejte následující dotaz, nahraďte < previous_login_name > hello uživatelské jméno, který byl dříve používán vaše úloha hello:</span><span class="sxs-lookup"><span data-stu-id="4494b-241">Go back toohello query window from Part 1 Step 7 and type in hello following query, replacing <previous_login_name> with hello User Name that was previously used by your job:</span></span>  
   `DROP LOGIN <previous_login_name>`  
2. <span data-ttu-id="4494b-242">Klikněte na tlačítko spustit:</span><span class="sxs-lookup"><span data-stu-id="4494b-242">Click Run:</span></span>  
   ![graphic43][graphic43]  

<span data-ttu-id="4494b-244">Měli byste obdržet hello následující potvrzení:</span><span class="sxs-lookup"><span data-stu-id="4494b-244">You should get hello following confirmation:</span></span> 

    Command(s) completed successfully.

## <a name="get-help"></a><span data-ttu-id="4494b-245">Podpora</span><span class="sxs-lookup"><span data-stu-id="4494b-245">Get help</span></span>
<span data-ttu-id="4494b-246">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="4494b-246">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4494b-247">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4494b-247">Next steps</span></span>
* [<span data-ttu-id="4494b-248">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4494b-248">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="4494b-249">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4494b-249">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="4494b-250">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4494b-250">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="4494b-251">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="4494b-251">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="4494b-252">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4494b-252">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

