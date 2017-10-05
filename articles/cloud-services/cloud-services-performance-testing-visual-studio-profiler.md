---
title: "Profilace cloudové služby místně v emulátoru služby výpočty v | Microsoft Docs"
services: cloud-services
description: "Prozkoumat problémy s výkonem v cloudové služby Visual Studio profileru"
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
tags: 
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 51c8192d8312dc5cf546b4c357aeecf6f19d56b8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a><span data-ttu-id="b4af0-103">Testování výkonu cloudové služby místně v emulátor výpočtů v Azure pomocí sady Visual Studio profileru</span><span class="sxs-lookup"><span data-stu-id="b4af0-103">Testing the Performance of a Cloud Service Locally in the Azure Compute Emulator Using the Visual Studio Profiler</span></span>
<span data-ttu-id="b4af0-104">K dispozici pro testování výkonu cloudové služby jsou různé nástroje a techniky.</span><span class="sxs-lookup"><span data-stu-id="b4af0-104">A variety of tools and techniques are available for testing the performance of cloud services.</span></span>
<span data-ttu-id="b4af0-105">Při publikování Cloudová služba Azure, můžete v aplikaci Visual Studio shromažďovat data profilování a potom analyzovat místně, jak je popsáno v [profilace aplikace Azure][1].</span><span class="sxs-lookup"><span data-stu-id="b4af0-105">When you publish a cloud service to Azure, you can have Visual Studio collect profiling data and then analyze it locally, as described in [Profiling an Azure Application][1].</span></span>
<span data-ttu-id="b4af0-106">Můžete také použít diagnostiky sledovat celou řadu čítače výkonu, jak je popsáno v [pomocí čítačů výkonu v Azure][2].</span><span class="sxs-lookup"><span data-stu-id="b4af0-106">You can also use diagnostics to track a variety of performance counters, as described in [Using performance counters in Azure][2].</span></span>
<span data-ttu-id="b4af0-107">Také můžete chtít profil aplikace místně v emulátoru služby výpočty v ještě před nasazením do cloudu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-107">You might also want to profile your application locally in the compute emulator before deploying it to the cloud.</span></span>

<span data-ttu-id="b4af0-108">Tento článek se věnuje metodě profilování pomocí vzorkování procesoru, která se dá dělat místně v emulátoru.</span><span class="sxs-lookup"><span data-stu-id="b4af0-108">This article covers the CPU Sampling method of profiling, which can be done locally in the emulator.</span></span> <span data-ttu-id="b4af0-109">Vzorkování procesoru je, že metoda profilace, který není velmi nežádoucí.</span><span class="sxs-lookup"><span data-stu-id="b4af0-109">CPU sampling is a method of profiling that is not very intrusive.</span></span> <span data-ttu-id="b4af0-110">V intervalu určené vzorkování profileru pořídí snímek zásobníku volání.</span><span class="sxs-lookup"><span data-stu-id="b4af0-110">At a designated sampling interval, the profiler takes a snapshot of the call stack.</span></span> <span data-ttu-id="b4af0-111">Data jsou shromažďovány prostřednictvím v časovém intervalu a zobrazí v sestavě.</span><span class="sxs-lookup"><span data-stu-id="b4af0-111">The data is collected over a period of time, and shown in a report.</span></span> <span data-ttu-id="b4af0-112">Tato metoda profilace se obvykle označuje, kde výpočetně náročné aplikace se právě provádí většinu práce procesoru.</span><span class="sxs-lookup"><span data-stu-id="b4af0-112">This method of profiling tends to indicate where in a computationally intensive application most of the CPU work is being done.</span></span>  <span data-ttu-id="b4af0-113">To vám dává příležitost a zaměřit se na "aktivní cesta" kde vaše aplikace je výdaje nejvíce času.</span><span class="sxs-lookup"><span data-stu-id="b4af0-113">This gives you the opportunity to focus on the "hot path" where your application is spending the most time.</span></span>

## <a name="1-configure-visual-studio-for-profiling"></a><span data-ttu-id="b4af0-114">1: konfigurace Visual Studia pro profilaci</span><span class="sxs-lookup"><span data-stu-id="b4af0-114">1: Configure Visual Studio for profiling</span></span>
<span data-ttu-id="b4af0-115">Existují nejprve pár možností konfigurace sady Visual Studio, které mohou být užitečné při vytváření profilu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-115">First, there are a few Visual Studio configuration options that might be helpful when profiling.</span></span> <span data-ttu-id="b4af0-116">Chcete-li smysl sestav profilování, budete potřebovat symboly (soubory PDB) pro vaše aplikace a také symboly pro knihovny systému.</span><span class="sxs-lookup"><span data-stu-id="b4af0-116">To make sense of the profiling reports, you'll need symbols (.pdb files) for your application and also symbols for system libraries.</span></span> <span data-ttu-id="b4af0-117">Budete chtít Ujistěte se, zda odkazujete servery k dispozici symbolů.</span><span class="sxs-lookup"><span data-stu-id="b4af0-117">You'll want to make sure that you reference the available symbol servers.</span></span> <span data-ttu-id="b4af0-118">To uděláte, na **nástroje** ve Visual Studiu zvolte v nabídce zvolte **možnosti**, zvolte **ladění**, pak **symboly**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-118">To do this, on the **Tools** menu in Visual Studio, choose **Options**, then choose **Debugging**, then **Symbols**.</span></span> <span data-ttu-id="b4af0-119">Ujistěte se, že servery symbolů Microsoft je uveden v části **symbolů umístění souborů (.pdb)**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-119">Make sure that Microsoft Symbol Servers is listed under **Symbol file (.pdb) locations**.</span></span>  <span data-ttu-id="b4af0-120">Můžete taky odkazovat http://referencesource.microsoft.com/symbols, což může mít další symbol soubory.</span><span class="sxs-lookup"><span data-stu-id="b4af0-120">You can also reference http://referencesource.microsoft.com/symbols, which might have additional symbol files.</span></span>

![Možnosti – symbol][4]

<span data-ttu-id="b4af0-122">V případě potřeby můžete zjednodušit sestavy, které generuje profileru nastavením pouze můj kód.</span><span class="sxs-lookup"><span data-stu-id="b4af0-122">If desired, you can simplify the reports that the profiler generates by setting Just My Code.</span></span> <span data-ttu-id="b4af0-123">S pouze můj kód povolena jsou zjednodušené zásobníky volání funkce tak, aby se ze sestav skrytá volání zcela interní knihovny a rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b4af0-123">With Just My Code enabled, function call stacks are simplified so that calls entirely internal to libraries and the .NET Framework are hidden from the reports.</span></span> <span data-ttu-id="b4af0-124">Na **nástroje** nabídky, zvolte **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-124">On the **Tools** menu, choose **Options**.</span></span> <span data-ttu-id="b4af0-125">Potom rozbalte **nástroje pro sledování výkonu** uzel a zvolte **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-125">Then expand the **Performance Tools** node, and choose **General**.</span></span> <span data-ttu-id="b4af0-126">Zaškrtněte políčko **povolit volbu pouze vlastní kód pro sestav profileru**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-126">Select the checkbox for **Enable Just My Code for profiler reports**.</span></span>

![Pouze můj kód možnosti][17]

<span data-ttu-id="b4af0-128">Tyto pokyny můžete použít existující projekt nebo s nový projekt.</span><span class="sxs-lookup"><span data-stu-id="b4af0-128">You can use these instructions with an existing project or with a new project.</span></span>  <span data-ttu-id="b4af0-129">Pokud vytvoříte nový projekt můžete vyzkoušet níže popsané postupy, zvolte C# **Azure Cloud Service** projektu a vyberte **webovou roli** a **Role pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-129">If you create a new project to try the techniques described below, choose a C# **Azure Cloud Service** project, and select a **Web Role** and a **Worker Role**.</span></span>

![Role projektu Azure Cloud Service][5]

<span data-ttu-id="b4af0-131">Pro účely, například přidat nějaký kód do projektu, který přebírá mnoho času a ukazuje některé zřejmé výkonu problém.</span><span class="sxs-lookup"><span data-stu-id="b4af0-131">For example purposes, add some code to your project that takes a lot of time and demonstrates some obvious performance problem.</span></span> <span data-ttu-id="b4af0-132">Například přidejte následující kód do projektu role pracovního procesu:</span><span class="sxs-lookup"><span data-stu-id="b4af0-132">For example, add the following code to a worker role project:</span></span>

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

<span data-ttu-id="b4af0-133">Tento kód volejte z metodě RunAsync v roli pracovního procesu RoleEntryPoint odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="b4af0-133">Call this code from the RunAsync method in the worker role's RoleEntryPoint-derived class.</span></span> <span data-ttu-id="b4af0-134">(Ignorovat upozornění o metodě synchronně spuštěná.)</span><span class="sxs-lookup"><span data-stu-id="b4af0-134">(Ignore the warning about the method running synchronously.)</span></span>

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

<span data-ttu-id="b4af0-135">Sestavení a spuštění cloudové služby místně bez ladění (Ctrl + F5), s konfigurací řešení nastavena na **verze**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-135">Build and run your cloud service locally without debugging (Ctrl+F5), with the solution configuration set to **Release**.</span></span> <span data-ttu-id="b4af0-136">To zajišťuje, že všechny soubory a složky jsou vytvořeny pro spuštění aplikace místně a zajistí, že jsou spuštěné všechny emulátorů.</span><span class="sxs-lookup"><span data-stu-id="b4af0-136">This ensures that all files and folders are created for running the application locally, and ensures that all the emulators are started.</span></span> <span data-ttu-id="b4af0-137">Ověřte, zda je spuštěna své role pracovního procesu začít uživatelské prostředí emulátoru výpočtů z hlavního panelu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-137">Start the Compute Emulator UI from the taskbar to verify that your worker role is running.</span></span>

## <a name="2-attach-to-a-process"></a><span data-ttu-id="b4af0-138">2: připojení k procesu</span><span class="sxs-lookup"><span data-stu-id="b4af0-138">2: Attach to a process</span></span>
<span data-ttu-id="b4af0-139">Místo profilace aplikace pomocí spuštění z prostředí Visual Studio 2010 IDE, musí připojení profileru k spuštěných procesů.</span><span class="sxs-lookup"><span data-stu-id="b4af0-139">Instead of profiling the application by starting it from the Visual Studio 2010 IDE, you must attach the profiler to a running process.</span></span> 

<span data-ttu-id="b4af0-140">K připojení profileru k procesu, na **analyzovat** nabídce zvolte **profileru** a **Attach/Detach**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-140">To attach the profiler to a process, on the **Analyze** menu, choose **Profiler** and **Attach/Detach**.</span></span>

![Připojte profile – možnost][6]

<span data-ttu-id="b4af0-142">Pro roli pracovního procesu najít WaWorkerHost.exe proces.</span><span class="sxs-lookup"><span data-stu-id="b4af0-142">For a worker role, find the WaWorkerHost.exe process.</span></span>

![Proces WaWorkerHost][7]

<span data-ttu-id="b4af0-144">Pokud složky projektu na síťové jednotce, požádá profileru zadat jiné umístění pro uložení sestav profilování.</span><span class="sxs-lookup"><span data-stu-id="b4af0-144">If your project folder is on a network drive, the profiler will ask you to provide another location to save the profiling reports.</span></span>

 <span data-ttu-id="b4af0-145">Můžete se také připojit k webové role připojením k WaIISHost.exe.</span><span class="sxs-lookup"><span data-stu-id="b4af0-145">You can also attach to a web role by attaching to WaIISHost.exe.</span></span>
<span data-ttu-id="b4af0-146">Pokud existují více procesů role v aplikaci, budete muset použít processID rozlište je.</span><span class="sxs-lookup"><span data-stu-id="b4af0-146">If there are multiple worker role processes in your application, you need to use the processID to distinguish them.</span></span> <span data-ttu-id="b4af0-147">Prostřednictvím kódu programu můžete dotazovat processID díky přístupu k objekt procesu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-147">You can query the processID programmatically by accessing the Process object.</span></span> <span data-ttu-id="b4af0-148">Například tento kód vložte do metoda Run RoleEntryPoint odvozené třídy v roli, můžete zobrazit v protokolu v uživatelské prostředí emulátoru výpočtů vědět, co proces připojení k.</span><span class="sxs-lookup"><span data-stu-id="b4af0-148">For example, if you add this code to the Run method of the RoleEntryPoint-derived class in a role, you can look at the log in the Compute Emulator UI to know what process to connect to.</span></span>

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

<span data-ttu-id="b4af0-149">Chcete-li zobrazit protokol, spustíte uživatelské prostředí emulátoru výpočtů.</span><span class="sxs-lookup"><span data-stu-id="b4af0-149">To view the log, start the Compute Emulator UI.</span></span>

![Spustit emulátoru služby výpočty uživatelského rozhraní][8]

<span data-ttu-id="b4af0-151">Otevřete v okně konzoly protokolu role pracovního procesu v uživatelské prostředí emulátoru výpočtů kliknutím na záhlaví okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="b4af0-151">Open the worker role log console window in the Compute Emulator UI by clicking on the console window's title bar.</span></span> <span data-ttu-id="b4af0-152">Zobrazí ID procesu v protokolu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-152">You can see the process ID in the log.</span></span>

![ID procesu zobrazení][9]

<span data-ttu-id="b4af0-154">Jeden, který jste připojili, proveďte kroky v uživatelském rozhraní aplikace (v případě potřeby) pro reprodukci scénáři.</span><span class="sxs-lookup"><span data-stu-id="b4af0-154">One you've attached, perform the steps in your application's UI (if needed) to reproduce the scenario.</span></span>

<span data-ttu-id="b4af0-155">Pokud chcete zastavit profilování, vyberte **zastavit profilování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="b4af0-155">When you want to stop profiling, choose the **Stop Profiling** link.</span></span>

![Zastavit profilování možnost][10]

## <a name="3-view-performance-reports"></a><span data-ttu-id="b4af0-157">3: Zobrazit sestavy pro zvýšení výkonu</span><span class="sxs-lookup"><span data-stu-id="b4af0-157">3: View performance reports</span></span>
<span data-ttu-id="b4af0-158">Zobrazí se sestava výkonu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b4af0-158">The performance report for your application is displayed.</span></span>

<span data-ttu-id="b4af0-159">V tomto okamžiku profileru zastaví provádění, uloží data v souboru .vsp a zobrazí sestavu, která obsahuje analýzu tato data.</span><span class="sxs-lookup"><span data-stu-id="b4af0-159">At this point, the profiler stops executing, saves data in a .vsp file, and displays a report that shows an analysis of this data.</span></span>

![Sestav profileru][11]

<span data-ttu-id="b4af0-161">Pokud se zobrazí String.wstrcpy v aktivní trase, klikněte na volbu pouze vlastní kód změníte zobrazení. Chcete-li zobrazit pouze uživatele kód.</span><span class="sxs-lookup"><span data-stu-id="b4af0-161">If you see String.wstrcpy in the Hot Path, click on Just My Code to change the view to show user code only.</span></span>  <span data-ttu-id="b4af0-162">Pokud se zobrazí String.Concat, zkuste kliknutím na tlačítko Zobrazit všechny kódu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-162">If you see String.Concat, try pressing the Show All Code button.</span></span>

<span data-ttu-id="b4af0-163">Měli byste vidět zřetězení metoda a String.Concat zabírají velkou část čas provádění.</span><span class="sxs-lookup"><span data-stu-id="b4af0-163">You should see the Concatenate method and String.Concat taking up a large portion of the execution time.</span></span>

![Analýzy zprávy][12]

<span data-ttu-id="b4af0-165">Pokud jste přidali kód zřetězení řetězec v tomto článku, měli byste vidět upozornění v seznamu úkolů pro tuto.</span><span class="sxs-lookup"><span data-stu-id="b4af0-165">If you added the string concatenation code in this article, you should see a warning in the Task List for this.</span></span> <span data-ttu-id="b4af0-166">Může se také zobrazit upozornění, že je nadměrné množství uvolňování paměti, která je kvůli velkému počtu řetězce, které jsou vytvořeny a zlikvidován.</span><span class="sxs-lookup"><span data-stu-id="b4af0-166">You may also see a warning that there is an excessive amount of garbage collection, which is due to the number of strings that are created and disposed.</span></span>

![Upozornění výkonu][14]

## <a name="4-make-changes-and-compare-performance"></a><span data-ttu-id="b4af0-168">4: proveďte změny a porovnání výkonu</span><span class="sxs-lookup"><span data-stu-id="b4af0-168">4: Make changes and compare performance</span></span>
<span data-ttu-id="b4af0-169">Také můžete porovnat výkon před a po změně kódu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-169">You can also compare the performance before and after a code change.</span></span>  <span data-ttu-id="b4af0-170">Zastavení spuštěných procesů a upravit kód, který nahradí operace zřetězení řetězců pomocí StringBuilder:</span><span class="sxs-lookup"><span data-stu-id="b4af0-170">Stop the running process, and edit the code to replace the string concatenation operation with the use of StringBuilder:</span></span>

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

<span data-ttu-id="b4af0-171">Nezadávejte jiné výkonu spustit a potom porovnejte výkon.</span><span class="sxs-lookup"><span data-stu-id="b4af0-171">Do another performance run, and then compare the performance.</span></span> <span data-ttu-id="b4af0-172">V Průzkumníku výkonu, pokud je spuštěn jsou ve stejné relaci, můžete právě vybrat obě tyto sestavy, otevřete místní nabídky a zvolte **porovnat sestavy pro zvýšení výkonu**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-172">In the Performance Explorer, if the runs are in the same session, you can just select both reports, open the shortcut menu, and choose **Compare Performance Reports**.</span></span> <span data-ttu-id="b4af0-173">Pokud chcete porovnat s spuštěný v jiné relaci výkonu, otevřete **analyzovat** nabídce a zvolte **porovnat sestavy pro zvýšení výkonu**.</span><span class="sxs-lookup"><span data-stu-id="b4af0-173">If you want to compare with a run in another performance session, open the **Analyze** menu, and choose **Compare Performance Reports**.</span></span> <span data-ttu-id="b4af0-174">Zadejte oba soubory v zobrazeném dialogu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-174">Specify both files in the dialog box that appears.</span></span>

![Porovnání výkonu sestavy možnost][15]

<span data-ttu-id="b4af0-176">Sestavy zvýrazněte rozdíly mezi dvěma spustí.</span><span class="sxs-lookup"><span data-stu-id="b4af0-176">The reports highlight differences between the two runs.</span></span>

![Sestavy porovnání][16]

<span data-ttu-id="b4af0-178">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="b4af0-178">Congratulations!</span></span> <span data-ttu-id="b4af0-179">Začnete profileru.</span><span class="sxs-lookup"><span data-stu-id="b4af0-179">You've gotten started with the profiler.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b4af0-180">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b4af0-180">Troubleshooting</span></span>
* <span data-ttu-id="b4af0-181">Zkontrolujte, zda jsou profilace sestavení pro vydání a spustit bez ladění.</span><span class="sxs-lookup"><span data-stu-id="b4af0-181">Make sure you are profiling a Release build and start without debugging.</span></span>
* <span data-ttu-id="b4af0-182">Pokud je možnost Attach/Detach není povoleno v nabídce profileru, spusťte Průvodce výkonu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-182">If the Attach/Detach option is not enabled on the Profiler menu, run the Performance Wizard.</span></span>
* <span data-ttu-id="b4af0-183">Uživatelské prostředí emulátoru výpočtů slouží k zobrazení stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4af0-183">Use the Compute Emulator UI to view the status of your application.</span></span> 
* <span data-ttu-id="b4af0-184">Pokud máte potíže se spuštěním aplikace v emulátoru, nebo se připojení profileru, vypnout dolů emulátoru služby výpočty v a restartujte ji.</span><span class="sxs-lookup"><span data-stu-id="b4af0-184">If you have problems starting applications in the emulator, or attaching the profiler, shut down the compute emulator and restart it.</span></span> <span data-ttu-id="b4af0-185">Pokud se problém nevyřeší, zkuste restartování.</span><span class="sxs-lookup"><span data-stu-id="b4af0-185">If that doesn't solve the problem, try rebooting.</span></span> <span data-ttu-id="b4af0-186">Tomuto problému může dojít, pokud používáte emulátor výpočetní pozastavení a odebrat spuštěné nasazení.</span><span class="sxs-lookup"><span data-stu-id="b4af0-186">This problem can occur if you use the Compute Emulator to suspend and remove running deployments.</span></span>
* <span data-ttu-id="b4af0-187">Pokud jste použili žádné příkazy profilování z příkazového řádku, zejména globální nastavení, ujistěte se, že byla volána /globaloff vsperfclrenv – a že VsPerfMon.exe byla vypnuta.</span><span class="sxs-lookup"><span data-stu-id="b4af0-187">If you have used any of the profiling commands from the command line, especially the global settings, make sure that VSPerfClrEnv /globaloff has been called and that VsPerfMon.exe has been shut down.</span></span>
* <span data-ttu-id="b4af0-188">Pokud při vzorkování, zobrazí se zpráva "PRF0025: Nebyla shromážděna žádná data," Zkontrolujte, jestli proces, který jste připojili k procesoru aktivitu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-188">If when sampling, you see the message "PRF0025: No data was collected," check that the process you attached to has CPU activity.</span></span> <span data-ttu-id="b4af0-189">Aplikace, které nejsou provádění veškerou práci, výpočetní nemusí vracet žádné data vzorkování.</span><span class="sxs-lookup"><span data-stu-id="b4af0-189">Applications that are not doing any computational work might not produce any sampling data.</span></span>  <span data-ttu-id="b4af0-190">Je také možné, že proces byl ukončen před provedením jakékoli vzorkování.</span><span class="sxs-lookup"><span data-stu-id="b4af0-190">It's also possible that the process exited before any sampling was done.</span></span> <span data-ttu-id="b4af0-191">Zkontrolujte, že metoda Run pro roli, která jsou profilace neskončí.</span><span class="sxs-lookup"><span data-stu-id="b4af0-191">Check to see that the Run method for a role that you are profiling does not terminate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4af0-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b4af0-192">Next Steps</span></span>
<span data-ttu-id="b4af0-193">Instrumentace Azure binárních souborů v emulátoru není podporována v sadě Visual Studio profiler, ale pokud chcete testovat přidělení paměti, můžete tuto možnost, při vytváření profilu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-193">Instrumenting Azure binaries in the emulator is not supported in the Visual Studio profiler, but if you want to test memory allocation, you can choose that option when profiling.</span></span> <span data-ttu-id="b4af0-194">Můžete také profilace souběžného zpracování, který vám pomůže určit, zda jsou vláken plýtvání časem neslučitelných pro zámky nebo profilace interakce vrstvy, které umožňuje sledovat problémy s výkonem při interakci mezi vrstvami aplikace nejčastěji mezi datovou vrstvou a roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="b4af0-194">You can also choose concurrency profiling, which helps you determine whether threads are wasting time competing for locks, or tier interaction profiling, which helps you track down performance problems when interacting between tiers of an application, most frequently between the data tier and a worker role.</span></span>  <span data-ttu-id="b4af0-195">Můžete zobrazit dotazy databáze, které vaše aplikace generuje a využívat data profilování ke zlepšování používání databáze.</span><span class="sxs-lookup"><span data-stu-id="b4af0-195">You can view the database queries that your app generates and use the profiling data to improve your use of the database.</span></span> <span data-ttu-id="b4af0-196">Informace o vytváření profilů interakce vrstvy, naleznete v příspěvku blogu [návod: použití profileru interakce vrstvy v sadě Visual Studio Team System 2010][3].</span><span class="sxs-lookup"><span data-stu-id="b4af0-196">For information about tier interaction profiling, see the blog post [Walkthrough: Using the Tier Interaction Profiler in Visual Studio Team System 2010][3].</span></span>

[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
