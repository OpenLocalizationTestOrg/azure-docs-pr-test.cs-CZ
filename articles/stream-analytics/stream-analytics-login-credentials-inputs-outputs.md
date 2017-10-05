---
title: "Stream Analytics: Otáčení protokolu pověření pro vstupy a výstupy | Microsoft Docs"
description: "Zjistěte, jak aktualizovat přihlašovací údaje pro Stream Analytics vstupy a výstupy."
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
ms.openlocfilehash: 2cb995a3969a8cb025f371ed0ab160cd04b0454d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a><span data-ttu-id="99fde-104">Otočit přihlašovací údaje pro vstupy a výstupy úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="99fde-104">Rotate login credentials for inputs and outputs in Stream Analytics Jobs</span></span>
## <a name="abstract"></a><span data-ttu-id="99fde-105">Abstraktní</span><span class="sxs-lookup"><span data-stu-id="99fde-105">Abstract</span></span>
<span data-ttu-id="99fde-106">Azure Stream Analytics dnes neumožňuje nahrazení přihlašovacích údajů na vstupu a výstupu při běhu úlohy.</span><span class="sxs-lookup"><span data-stu-id="99fde-106">Azure Stream Analytics today doesn’t allow replacing the credentials on an input/output while the job is running.</span></span>

<span data-ttu-id="99fde-107">Zatímco Azure Stream Analytics podporuje obnovení úlohy z posledního výstupu, jsme chtěli sdílet celý proces pro minimalizaci prodleva mezi zastavení a spuštění úlohy a otáčení přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="99fde-107">While Azure Stream Analytics does support resuming a job from last output, we wanted to share the entire process for minimizing the lag between the stopping and starting of the job and rotating the login credentials.</span></span>

## <a name="part-1---prepare-the-new-set-of-credentials"></a><span data-ttu-id="99fde-108">Část 1 – Příprava novou sadu přihlašovacích údajů:</span><span class="sxs-lookup"><span data-stu-id="99fde-108">Part 1 - Prepare the new set of credentials:</span></span>
<span data-ttu-id="99fde-109">Tato část se vztahuje na následující vstupy/výstupy:</span><span class="sxs-lookup"><span data-stu-id="99fde-109">This part is applicable to the following inputs/outputs:</span></span>

* <span data-ttu-id="99fde-110">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="99fde-110">Blob Storage</span></span>
* <span data-ttu-id="99fde-111">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="99fde-111">Event Hubs</span></span>
* <span data-ttu-id="99fde-112">SQL Database</span><span class="sxs-lookup"><span data-stu-id="99fde-112">SQL Database</span></span>
* <span data-ttu-id="99fde-113">Table Storage</span><span class="sxs-lookup"><span data-stu-id="99fde-113">Table Storage</span></span>

<span data-ttu-id="99fde-114">Pro ostatní vstupy/výstupy pokračujte část 2.</span><span class="sxs-lookup"><span data-stu-id="99fde-114">For other inputs/outputs, proceed with Part 2.</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="99fde-115">Úložiště objektů BLOB úložiště/tabulky</span><span class="sxs-lookup"><span data-stu-id="99fde-115">Blob storage/Table storage</span></span>
1. <span data-ttu-id="99fde-116">Přejdete na rozšíření úložiště na portálu pro správu Azure:</span><span class="sxs-lookup"><span data-stu-id="99fde-116">Go to the Storage extention on the Azure Management portal:</span></span>  
   ![graphic1][graphic1]
2. <span data-ttu-id="99fde-118">Vyhledejte úložiště používá vaše úloha a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="99fde-118">Locate the storage used by your job and go into it:</span></span>  
   ![graphic2][graphic2]
3. <span data-ttu-id="99fde-120">Klikněte na příkaz Spravovat přístupové klíče:</span><span class="sxs-lookup"><span data-stu-id="99fde-120">Click the Manage Access Keys command:</span></span>  
   ![graphic3][graphic3]
4. <span data-ttu-id="99fde-122">Mezi primární přístupový klíč a sekundární přístupový klíč **vyberte ten není používá vaše úloha**.</span><span class="sxs-lookup"><span data-stu-id="99fde-122">Between the Primary Access Key and the Secondary Access Key, **pick the one not used by your job**.</span></span>
5. <span data-ttu-id="99fde-123">Stiskněte tlačítko znovu vygenerovat:</span><span class="sxs-lookup"><span data-stu-id="99fde-123">Hit regenerate:</span></span>  
   ![graphic4][graphic4]
6. <span data-ttu-id="99fde-125">Zkopírujte nově vygenerovaný klíč:</span><span class="sxs-lookup"><span data-stu-id="99fde-125">Copy the newly generated key:</span></span>  
   ![graphic5][graphic5]
7. <span data-ttu-id="99fde-127">Pokračujte částí 2.</span><span class="sxs-lookup"><span data-stu-id="99fde-127">Continue to Part 2.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="99fde-128">Služba Event hubs</span><span class="sxs-lookup"><span data-stu-id="99fde-128">Event hubs</span></span>
1. <span data-ttu-id="99fde-129">Přejdete na Service Bus rozšíření na portálu pro správu Azure:</span><span class="sxs-lookup"><span data-stu-id="99fde-129">Go to the Service Bus extension on the Azure Management portal:</span></span>  
   ![graphic6][graphic6]
2. <span data-ttu-id="99fde-131">Vyhledejte Namespace Service Bus, používá vaše úloha a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="99fde-131">Locate the Service Bus Namespace used by your job and go into it:</span></span>  
   ![graphic7][graphic7]
3. <span data-ttu-id="99fde-133">Pokud vaše úlohy používá zásady sdíleného přístupu na Namespace sběrnice služby, přejít na krok 6</span><span class="sxs-lookup"><span data-stu-id="99fde-133">If your job uses a shared access policy on the Service Bus Namespace, jump to step 6</span></span>  
4. <span data-ttu-id="99fde-134">Přejděte na kartu centra událostí:</span><span class="sxs-lookup"><span data-stu-id="99fde-134">Go to the Event Hubs tab:</span></span>  
   ![graphic8][graphic8]
5. <span data-ttu-id="99fde-136">Vyhledejte centra událostí, které používá vaše úloha a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="99fde-136">Locate the Event Hub used by your job and go into it:</span></span>  
   ![graphic9][graphic9]
6. <span data-ttu-id="99fde-138">Přejděte na kartu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="99fde-138">Go to the Configure Tab:</span></span>  
   ![graphic10][graphic10]
7. <span data-ttu-id="99fde-140">Na název zásady rozevíracího seznamu vyhledejte zásady sdíleného přístupu používá vaše úloha:</span><span class="sxs-lookup"><span data-stu-id="99fde-140">On the Policy Name drop-down, locate the shared access policy used by your job:</span></span>  
   ![graphic11][graphic11]
8. <span data-ttu-id="99fde-142">Mezi primární klíč a sekundární klíč **vyberte ten není používá vaše úloha**.</span><span class="sxs-lookup"><span data-stu-id="99fde-142">Between the Primary Key and the Secondary Key, **pick the one not used by your job**.</span></span>  
9. <span data-ttu-id="99fde-143">Stiskněte tlačítko znovu vygenerovat:</span><span class="sxs-lookup"><span data-stu-id="99fde-143">Hit regenerate:</span></span>  
   ![graphic12][graphic12]
10. <span data-ttu-id="99fde-145">Zkopírujte nově vygenerovaný klíč:</span><span class="sxs-lookup"><span data-stu-id="99fde-145">Copy the newly generated key:</span></span>  
   ![graphic13][graphic13]
11. <span data-ttu-id="99fde-147">Pokračujte částí 2.</span><span class="sxs-lookup"><span data-stu-id="99fde-147">Continue to Part 2.</span></span>  

### <a name="sql-database"></a><span data-ttu-id="99fde-148">SQL Database</span><span class="sxs-lookup"><span data-stu-id="99fde-148">SQL Database</span></span>
> [!NOTE]
> <span data-ttu-id="99fde-149">Poznámka: budete muset připojit ke službě SQL Database.</span><span class="sxs-lookup"><span data-stu-id="99fde-149">Note: you will need to connect to the SQL Database Service.</span></span> <span data-ttu-id="99fde-150">Přidáme ukazují, jak to provést pomocí prostředí pro správu na portálu pro správu Azure, ale můžete použít některé klientské nástroje, jako je SQL Server Management Studio také.</span><span class="sxs-lookup"><span data-stu-id="99fde-150">We are going to show how to do this using the management experience on the Azure Management portal but you may choose to use some client-side tool such as SQL Server Management Studio as well.</span></span>
>
> 

1. <span data-ttu-id="99fde-151">Přejděte do databáze SQL rozšíření na portálu pro správu Azure:</span><span class="sxs-lookup"><span data-stu-id="99fde-151">Go to the SQL Databases extension on the Azure Management portal:</span></span>  
   ![graphic14][graphic14]
2. <span data-ttu-id="99fde-153">Vyhledejte databázi SQL používanou nástrojem úlohu a **klikněte na server** odkaz na stejném řádku:</span><span class="sxs-lookup"><span data-stu-id="99fde-153">Locate the SQL Database used by your job and **click on the server** link on the same line:</span></span>  
   <span data-ttu-id="99fde-154">![graphic15][graphic15]</span><span class="sxs-lookup"><span data-stu-id="99fde-154">![graphic15][graphic15]</span></span>
3. <span data-ttu-id="99fde-155">Klikněte na příkaz Spravovat:</span><span class="sxs-lookup"><span data-stu-id="99fde-155">Click the Manage command:</span></span>  
   ![graphic16][graphic16]
4. <span data-ttu-id="99fde-157">Hlavní databáze typu:</span><span class="sxs-lookup"><span data-stu-id="99fde-157">Type Database Master:</span></span>  
   ![graphic17][graphic17]
5. <span data-ttu-id="99fde-159">Zadejte uživatelské jméno, heslo a klikněte na protokolu:</span><span class="sxs-lookup"><span data-stu-id="99fde-159">Type in your User Name, Password, and click Log on:</span></span>  
   ![graphic18][graphic18]
6. <span data-ttu-id="99fde-161">Klikněte na nový dotaz:</span><span class="sxs-lookup"><span data-stu-id="99fde-161">Click New Query:</span></span>  
   ![graphic19][graphic19]
7. <span data-ttu-id="99fde-163">Typ v následujícím dotazu nahraďte < login_name > vaše uživatelské jméno a nahraďte <enterStrongPasswordHere> pomocí nového hesla:</span><span class="sxs-lookup"><span data-stu-id="99fde-163">Type in the following query replacing <login_name> with your User Name and replacing <enterStrongPasswordHere> with your new password:</span></span>  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. <span data-ttu-id="99fde-164">Klikněte na tlačítko spustit:</span><span class="sxs-lookup"><span data-stu-id="99fde-164">Click Run:</span></span>  
   ![graphic20][graphic20]
9. <span data-ttu-id="99fde-166">Vraťte se ke kroku 2 a tuto dobu, klepněte na databázi:</span><span class="sxs-lookup"><span data-stu-id="99fde-166">Go back to step 2 and this time click the database:</span></span>  
   ![graphic21][graphic21]
10. <span data-ttu-id="99fde-168">Klikněte na příkaz Spravovat:</span><span class="sxs-lookup"><span data-stu-id="99fde-168">Click the Manage command:</span></span>  
   ![graphic22][graphic22]
11. <span data-ttu-id="99fde-170">Zadejte uživatelské jméno, heslo a klikněte na možnost přihlášení:</span><span class="sxs-lookup"><span data-stu-id="99fde-170">type in your User Name, Password, and click Logon:</span></span>  
   ![graphic23][graphic23]
12. <span data-ttu-id="99fde-172">Klikněte na nový dotaz:</span><span class="sxs-lookup"><span data-stu-id="99fde-172">Click New Query:</span></span>  
   ![graphic24][graphic24]
13. <span data-ttu-id="99fde-174">Zadejte následující dotaz nahrazování < uživatelské_jméno > s názvem, podle kterého chcete identifikovat toto přihlášení v kontextu této databáze (můžete zadat stejnou hodnotu, který jste zadali pro < login_name >, třeba) a nahraďte < login_name > nové uživatelské jméno:</span><span class="sxs-lookup"><span data-stu-id="99fde-174">Type in the following query replacing <user_name> with a name by which you want to identify this login in the context of this database (you can provide the same value you gave for <login_name>, for example) and replacing <login_name> with your new user name:</span></span>  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. <span data-ttu-id="99fde-175">Klikněte na tlačítko spustit:</span><span class="sxs-lookup"><span data-stu-id="99fde-175">Click Run:</span></span>  
   ![graphic25][graphic25]
15. <span data-ttu-id="99fde-177">Nový uživatel by měl poskytovat teď se stejnou rolí a oprávnění měl původního uživatele.</span><span class="sxs-lookup"><span data-stu-id="99fde-177">You should now provide your new user with the same roles and privileges your original user had.</span></span>
16. <span data-ttu-id="99fde-178">Pokračujte částí 2.</span><span class="sxs-lookup"><span data-stu-id="99fde-178">Continue to Part 2.</span></span>

## <a name="part-2-stopping-the-stream-analytics-job"></a><span data-ttu-id="99fde-179">Část 2: Ukončení úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="99fde-179">Part 2: Stopping the Stream Analytics Job</span></span>
1. <span data-ttu-id="99fde-180">Přejdete na rozšíření Stream Analytics na portálu pro správu Azure:</span><span class="sxs-lookup"><span data-stu-id="99fde-180">Go to the Stream Analytics extension on the Azure Management portal:</span></span>  
   ![graphic26][graphic26]
2. <span data-ttu-id="99fde-182">Najděte úlohu a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="99fde-182">Locate your job and go into it:</span></span>  
   ![graphic27][graphic27]
3. <span data-ttu-id="99fde-184">Přejděte na kartu vstupů nebo výstupů založené na tom, jestli jsou otáčení přihlašovací údaje na vstup nebo výstup.</span><span class="sxs-lookup"><span data-stu-id="99fde-184">Go to the Inputs tab or the Outputs tab based on whether you are rotating the credentials on an Input or on an Output.</span></span>  
   ![graphic28][graphic28]
4. <span data-ttu-id="99fde-186">Klikněte na příkaz k ukončení a potvrďte, že úloha byla zastavena:</span><span class="sxs-lookup"><span data-stu-id="99fde-186">Click the Stop command and confirm the job has stopped:</span></span>  
   <span data-ttu-id="99fde-187">![graphic29][graphic29] počkejte na zastavení úlohy.</span><span class="sxs-lookup"><span data-stu-id="99fde-187">![graphic29][graphic29] Wait for the job to stop.</span></span>
5. <span data-ttu-id="99fde-188">Vyhledejte vstupu a výstupu, který chcete otočit na přihlašovací údaje a přejděte do ní:</span><span class="sxs-lookup"><span data-stu-id="99fde-188">Locate the input/output you want to rotate credentials on and go into it:</span></span>  
   ![graphic30][graphic30]
6. <span data-ttu-id="99fde-190">Přejděte k části 3.</span><span class="sxs-lookup"><span data-stu-id="99fde-190">Proceed to Part 3.</span></span>

## <a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a><span data-ttu-id="99fde-191">Část 3: Úprava přihlašovací údaje u úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="99fde-191">Part 3: Editing the credentials on the Stream Analytics Job</span></span>
### <a name="blob-storagetable-storage"></a><span data-ttu-id="99fde-192">Úložiště objektů BLOB úložiště/tabulky</span><span class="sxs-lookup"><span data-stu-id="99fde-192">Blob storage/Table storage</span></span>
1. <span data-ttu-id="99fde-193">Najít pole klíč účtu úložiště a vložte do něj nově vygenerovaný klíč:</span><span class="sxs-lookup"><span data-stu-id="99fde-193">Find the Storage Account Key field and paste your newly generated key into it:</span></span>  
   ![graphic31][graphic31]
2. <span data-ttu-id="99fde-195">Klikněte na příkaz Uložit a potvrďte uložení změn:</span><span class="sxs-lookup"><span data-stu-id="99fde-195">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic32][graphic32]
3. <span data-ttu-id="99fde-197">Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že je úspěšně byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="99fde-197">A connection test will automatically start when you save your changes, make sure that is has successfully passed.</span></span>
4. <span data-ttu-id="99fde-198">Přejděte k části 4.</span><span class="sxs-lookup"><span data-stu-id="99fde-198">Proceed to Part 4.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="99fde-199">Služba Event hubs</span><span class="sxs-lookup"><span data-stu-id="99fde-199">Event hubs</span></span>
1. <span data-ttu-id="99fde-200">Najít pole klíče události rozbočovače zásady a vložit vaše nově vygenerovaný klíč do ní:</span><span class="sxs-lookup"><span data-stu-id="99fde-200">Find the Event Hub Policy Key field and paste your newly generated key into it:</span></span>  
   ![graphic33][graphic33]
2. <span data-ttu-id="99fde-202">Klikněte na příkaz Uložit a potvrďte uložení změn:</span><span class="sxs-lookup"><span data-stu-id="99fde-202">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic34][graphic34]
3. <span data-ttu-id="99fde-204">Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že má úspěšně úspěšně.</span><span class="sxs-lookup"><span data-stu-id="99fde-204">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>
4. <span data-ttu-id="99fde-205">Přejděte k části 4.</span><span class="sxs-lookup"><span data-stu-id="99fde-205">Proceed to Part 4.</span></span>

### <a name="power-bi"></a><span data-ttu-id="99fde-206">Power BI</span><span class="sxs-lookup"><span data-stu-id="99fde-206">Power BI</span></span>
1. <span data-ttu-id="99fde-207">Klikněte na tlačítko Obnovit autorizace:</span><span class="sxs-lookup"><span data-stu-id="99fde-207">Click the Renew authorization:</span></span>  

   ![graphic35][graphic35]
2. <span data-ttu-id="99fde-209">Zobrazí se po potvrzení:</span><span class="sxs-lookup"><span data-stu-id="99fde-209">You will get the following confirmation:</span></span>  

   ![graphic36][graphic36]
3. <span data-ttu-id="99fde-211">Klikněte na příkaz Uložit a potvrďte uložení změn:</span><span class="sxs-lookup"><span data-stu-id="99fde-211">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic37][graphic37]
4. <span data-ttu-id="99fde-213">Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že úspěšně úspěšně prošel.</span><span class="sxs-lookup"><span data-stu-id="99fde-213">A connection test will automatically start when you save your changes, make sure it has successfully passed.</span></span>
5. <span data-ttu-id="99fde-214">Přejděte k části 4.</span><span class="sxs-lookup"><span data-stu-id="99fde-214">Proceed to Part 4.</span></span>

### <a name="sql-database"></a><span data-ttu-id="99fde-215">SQL Database</span><span class="sxs-lookup"><span data-stu-id="99fde-215">SQL Database</span></span>
1. <span data-ttu-id="99fde-216">Najít pole uživatelské jméno a heslo a vkládání do nich vaše nově vytvořenou sadu přihlašovacích údajů:</span><span class="sxs-lookup"><span data-stu-id="99fde-216">Find the User Name and Password fields and paste your newly created set of credentials into them:</span></span>  
   ![graphic38][graphic38]
2. <span data-ttu-id="99fde-218">Klikněte na příkaz Uložit a potvrďte uložení změn:</span><span class="sxs-lookup"><span data-stu-id="99fde-218">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic39][graphic39]
3. <span data-ttu-id="99fde-220">Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že má úspěšně úspěšně.</span><span class="sxs-lookup"><span data-stu-id="99fde-220">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>  
4. <span data-ttu-id="99fde-221">Přejděte k části 4.</span><span class="sxs-lookup"><span data-stu-id="99fde-221">Proceed to Part 4.</span></span>

## <a name="part-4-starting-your-job-from-last-stopped-time"></a><span data-ttu-id="99fde-222">Část 4: Od poslední zastaven úlohu</span><span class="sxs-lookup"><span data-stu-id="99fde-222">Part 4: Starting your job from last stopped time</span></span>
1. <span data-ttu-id="99fde-223">Opustit vstupu a výstupu:</span><span class="sxs-lookup"><span data-stu-id="99fde-223">Navigate away from the Input/Output:</span></span>  
   ![graphic40][graphic40]
2. <span data-ttu-id="99fde-225">Klikněte na příkaz ke spuštění:</span><span class="sxs-lookup"><span data-stu-id="99fde-225">Click the Start command:</span></span>  
   ![graphic41][graphic41]
3. <span data-ttu-id="99fde-227">Vyberte naposledy zastaveno a klikněte na tlačítko OK:</span><span class="sxs-lookup"><span data-stu-id="99fde-227">Pick the Last Stopped Time and click OK:</span></span>  
   ![graphic42][graphic42]
4. <span data-ttu-id="99fde-229">Přejděte k části 5.</span><span class="sxs-lookup"><span data-stu-id="99fde-229">Proceed to Part 5.</span></span>  

## <a name="part-5-removing-the-old-set-of-credentials"></a><span data-ttu-id="99fde-230">Část 5: Odebrání původní sadu přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="99fde-230">Part 5: Removing the old set of credentials</span></span>
<span data-ttu-id="99fde-231">Tato část se vztahuje na následující vstupy/výstupy:</span><span class="sxs-lookup"><span data-stu-id="99fde-231">This part is applicable to the following inputs/outputs:</span></span>

* <span data-ttu-id="99fde-232">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="99fde-232">Blob Storage</span></span>
* <span data-ttu-id="99fde-233">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="99fde-233">Event Hubs</span></span>
* <span data-ttu-id="99fde-234">SQL Database</span><span class="sxs-lookup"><span data-stu-id="99fde-234">SQL Database</span></span>
* <span data-ttu-id="99fde-235">Table Storage</span><span class="sxs-lookup"><span data-stu-id="99fde-235">Table Storage</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="99fde-236">Úložiště objektů BLOB úložiště/tabulky</span><span class="sxs-lookup"><span data-stu-id="99fde-236">Blob storage/Table storage</span></span>
<span data-ttu-id="99fde-237">Část 1 opakujte pro přístupový klíč, který byl dříve používán vaše úloha obnovení nyní nepoužívané přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="99fde-237">Repeat Part 1 for the Access Key that was previously used by your job to renew the now unused Access Key.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="99fde-238">Služba Event hubs</span><span class="sxs-lookup"><span data-stu-id="99fde-238">Event hubs</span></span>
<span data-ttu-id="99fde-239">Část 1 opakujte pro klíč, který byl dříve používán vaše úloha obnovíte klíč, teď nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="99fde-239">Repeat Part 1 for the Key that was previously used by your job to renew the now unused Key.</span></span>

### <a name="sql-database"></a><span data-ttu-id="99fde-240">SQL Database</span><span class="sxs-lookup"><span data-stu-id="99fde-240">SQL Database</span></span>
1. <span data-ttu-id="99fde-241">Přejděte zpět do okna dotazu z část 1 kroku 7 a zadejte následující dotaz, nahraďte < previous_login_name > uživatelské jméno, který byl dříve používán vaše úlohy:</span><span class="sxs-lookup"><span data-stu-id="99fde-241">Go back to the query window from Part 1 Step 7 and type in the following query, replacing <previous_login_name> with the User Name that was previously used by your job:</span></span>  
   `DROP LOGIN <previous_login_name>`  
2. <span data-ttu-id="99fde-242">Klikněte na tlačítko spustit:</span><span class="sxs-lookup"><span data-stu-id="99fde-242">Click Run:</span></span>  
   ![graphic43][graphic43]  

<span data-ttu-id="99fde-244">Měli byste obdržet následující potvrzení:</span><span class="sxs-lookup"><span data-stu-id="99fde-244">You should get the following confirmation:</span></span> 

    Command(s) completed successfully.

## <a name="get-help"></a><span data-ttu-id="99fde-245">Podpora</span><span class="sxs-lookup"><span data-stu-id="99fde-245">Get help</span></span>
<span data-ttu-id="99fde-246">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="99fde-246">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="99fde-247">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99fde-247">Next steps</span></span>
* [<span data-ttu-id="99fde-248">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="99fde-248">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="99fde-249">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="99fde-249">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="99fde-250">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="99fde-250">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="99fde-251">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="99fde-251">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="99fde-252">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="99fde-252">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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

