---
title: "aaaProfiling cloudové služby místně v hello výpočetní emulátor | Microsoft Docs"
services: cloud-services
description: "Zkoumání problémů s výkonem v cloudové služby Visual Studio profiler hello"
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
ms.openlocfilehash: fc37c85dad4db4cc0310f73afad56fc0fe5f3963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="testing-hello-performance-of-a-cloud-service-locally-in-hello-azure-compute-emulator-using-hello-visual-studio-profiler"></a><span data-ttu-id="7da5d-103">Testování hello výkon cloudové služby místně v hello Azure výpočetní emulátor pomocí hello Visual Studio Profiler</span><span class="sxs-lookup"><span data-stu-id="7da5d-103">Testing hello Performance of a Cloud Service Locally in hello Azure Compute Emulator Using hello Visual Studio Profiler</span></span>
<span data-ttu-id="7da5d-104">Jsou k dispozici pro testování výkonu hello cloudových služeb různé nástroje a techniky.</span><span class="sxs-lookup"><span data-stu-id="7da5d-104">A variety of tools and techniques are available for testing hello performance of cloud services.</span></span>
<span data-ttu-id="7da5d-105">Když publikujete tooAzure cloudové služby, může mít Visual Studio shromažďovat data profilování a potom analyzovat místně, jak je popsáno v [profilace aplikace Azure][1].</span><span class="sxs-lookup"><span data-stu-id="7da5d-105">When you publish a cloud service tooAzure, you can have Visual Studio collect profiling data and then analyze it locally, as described in [Profiling an Azure Application][1].</span></span>
<span data-ttu-id="7da5d-106">Můžete také diagnostiky tootrack čítače celou řadu výkonu, jak je popsáno v [pomocí čítačů výkonu v Azure][2].</span><span class="sxs-lookup"><span data-stu-id="7da5d-106">You can also use diagnostics tootrack a variety of performance counters, as described in [Using performance counters in Azure][2].</span></span>
<span data-ttu-id="7da5d-107">Můžete také chtít tooprofile aplikace místně v emulátoru služby výpočty hello ještě před nasazením toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-107">You might also want tooprofile your application locally in hello compute emulator before deploying it toohello cloud.</span></span>

<span data-ttu-id="7da5d-108">Tento článek se zabývá hello procesoru vzorkování metoda profilování, což lze provést místně v emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-108">This article covers hello CPU Sampling method of profiling, which can be done locally in hello emulator.</span></span> <span data-ttu-id="7da5d-109">Vzorkování procesoru je, že metoda profilace, který není velmi nežádoucí.</span><span class="sxs-lookup"><span data-stu-id="7da5d-109">CPU sampling is a method of profiling that is not very intrusive.</span></span> <span data-ttu-id="7da5d-110">V intervalu určené vzorkování profileru hello pořídí snímek hello zásobníku volání.</span><span class="sxs-lookup"><span data-stu-id="7da5d-110">At a designated sampling interval, hello profiler takes a snapshot of hello call stack.</span></span> <span data-ttu-id="7da5d-111">Hello data se shromažďují v časovém intervalu a zobrazí v sestavě.</span><span class="sxs-lookup"><span data-stu-id="7da5d-111">hello data is collected over a period of time, and shown in a report.</span></span> <span data-ttu-id="7da5d-112">Tato metoda profilace obvykle tooindicate, kde výpočetně náročné aplikace se právě provádí většinu práce hello procesoru.</span><span class="sxs-lookup"><span data-stu-id="7da5d-112">This method of profiling tends tooindicate where in a computationally intensive application most of hello CPU work is being done.</span></span>  <span data-ttu-id="7da5d-113">Tato poskytuje hello možnost toofocus cestou hello"aktivní" kde vaše aplikace je výdaje hello většinu času.</span><span class="sxs-lookup"><span data-stu-id="7da5d-113">This gives you hello opportunity toofocus on hello "hot path" where your application is spending hello most time.</span></span>

## <a name="1-configure-visual-studio-for-profiling"></a><span data-ttu-id="7da5d-114">1: konfigurace Visual Studia pro profilaci</span><span class="sxs-lookup"><span data-stu-id="7da5d-114">1: Configure Visual Studio for profiling</span></span>
<span data-ttu-id="7da5d-115">Existují nejprve pár možností konfigurace sady Visual Studio, které mohou být užitečné při vytváření profilu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-115">First, there are a few Visual Studio configuration options that might be helpful when profiling.</span></span> <span data-ttu-id="7da5d-116">představu o tom, toomake hello sestav profilace, budete potřebovat symboly (soubory PDB) pro aplikaci a také symboly pro knihovny systému.</span><span class="sxs-lookup"><span data-stu-id="7da5d-116">toomake sense of hello profiling reports, you'll need symbols (.pdb files) for your application and also symbols for system libraries.</span></span> <span data-ttu-id="7da5d-117">Budete muset toomake se, zda odkazujete servery k dispozici symbolů hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-117">You'll want toomake sure that you reference hello available symbol servers.</span></span> <span data-ttu-id="7da5d-118">toodo to na hello **nástroje** ve Visual Studiu zvolte v nabídce zvolte **možnosti**, zvolte **ladění**, pak **symboly**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-118">toodo this, on hello **Tools** menu in Visual Studio, choose **Options**, then choose **Debugging**, then **Symbols**.</span></span> <span data-ttu-id="7da5d-119">Ujistěte se, že servery symbolů Microsoft je uveden v části **symbolů umístění souborů (.pdb)**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-119">Make sure that Microsoft Symbol Servers is listed under **Symbol file (.pdb) locations**.</span></span>  <span data-ttu-id="7da5d-120">Můžete taky odkazovat http://referencesource.microsoft.com/symbols, což může mít další symbol soubory.</span><span class="sxs-lookup"><span data-stu-id="7da5d-120">You can also reference http://referencesource.microsoft.com/symbols, which might have additional symbol files.</span></span>

![Možnosti – symbol][4]

<span data-ttu-id="7da5d-122">V případě potřeby můžete zjednodušit hello hlásí, že hello profileru generuje nastavením pouze můj kód.</span><span class="sxs-lookup"><span data-stu-id="7da5d-122">If desired, you can simplify hello reports that hello profiler generates by setting Just My Code.</span></span> <span data-ttu-id="7da5d-123">S pouze můj kód povolena jsou zjednodušené zásobníky volání funkce tak, aby zcela interní toolibraries, který volá a hello rozhraní .NET Framework jsou skryta hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="7da5d-123">With Just My Code enabled, function call stacks are simplified so that calls entirely internal toolibraries and hello .NET Framework are hidden from hello reports.</span></span> <span data-ttu-id="7da5d-124">Na hello **nástroje** nabídce zvolte **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-124">On hello **Tools** menu, choose **Options**.</span></span> <span data-ttu-id="7da5d-125">Potom rozbalte hello **nástroje pro sledování výkonu** uzel a zvolte **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-125">Then expand hello **Performance Tools** node, and choose **General**.</span></span> <span data-ttu-id="7da5d-126">Zaškrtněte políčko hello pro **povolit volbu pouze vlastní kód pro sestav profileru**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-126">Select hello checkbox for **Enable Just My Code for profiler reports**.</span></span>

![Pouze můj kód možnosti][17]

<span data-ttu-id="7da5d-128">Tyto pokyny můžete použít existující projekt nebo s nový projekt.</span><span class="sxs-lookup"><span data-stu-id="7da5d-128">You can use these instructions with an existing project or with a new project.</span></span>  <span data-ttu-id="7da5d-129">Pokud vytvoříte nový hello tootry projektu níže popsané postupy, zvolte C# **Azure Cloud Service** projektu a vyberte **webovou roli** a **Role pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-129">If you create a new project tootry hello techniques described below, choose a C# **Azure Cloud Service** project, and select a **Web Role** and a **Worker Role**.</span></span>

![Role projektu Azure Cloud Service][5]

<span data-ttu-id="7da5d-131">Pro účely, například přidat některé projekt tooyour kód, který přebírá mnoho času a ukazuje některé zřejmé výkonu problém.</span><span class="sxs-lookup"><span data-stu-id="7da5d-131">For example purposes, add some code tooyour project that takes a lot of time and demonstrates some obvious performance problem.</span></span> <span data-ttu-id="7da5d-132">Například přidejte hello následující projekt role pracovního procesu tooa kódu:</span><span class="sxs-lookup"><span data-stu-id="7da5d-132">For example, add hello following code tooa worker role project:</span></span>

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

<span data-ttu-id="7da5d-133">Tento kód volejte z hello metodě RunAsync v roli pracovního procesu hello RoleEntryPoint odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="7da5d-133">Call this code from hello RunAsync method in hello worker role's RoleEntryPoint-derived class.</span></span> <span data-ttu-id="7da5d-134">(Ignorovat upozornění hello o metodě hello systémem synchronně.)</span><span class="sxs-lookup"><span data-stu-id="7da5d-134">(Ignore hello warning about hello method running synchronously.)</span></span>

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace hello following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

<span data-ttu-id="7da5d-135">Sestavení a spuštění cloudové služby místně bez ladění (Ctrl + F5), s konfigurací řešení hello nastavit příliš**verze**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-135">Build and run your cloud service locally without debugging (Ctrl+F5), with hello solution configuration set too**Release**.</span></span> <span data-ttu-id="7da5d-136">To zajistí, že všechny soubory a složky jsou vytvořeny pro spuštění aplikace hello místně a zajistí, že jsou spuštěné všechny emulátorů hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-136">This ensures that all files and folders are created for running hello application locally, and ensures that all hello emulators are started.</span></span> <span data-ttu-id="7da5d-137">Spusťte hello uživatelské prostředí emulátoru výpočtů z tooverify hello panelu, spuštění své role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-137">Start hello Compute Emulator UI from hello taskbar tooverify that your worker role is running.</span></span>

## <a name="2-attach-tooa-process"></a><span data-ttu-id="7da5d-138">2: připojit tooa proces</span><span class="sxs-lookup"><span data-stu-id="7da5d-138">2: Attach tooa process</span></span>
<span data-ttu-id="7da5d-139">Místo profilace aplikace hello spuštěním z hello Visual Studio 2010 IDE, je nutné připojit tooa profileru hello spuštění procesu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-139">Instead of profiling hello application by starting it from hello Visual Studio 2010 IDE, you must attach hello profiler tooa running process.</span></span> 

<span data-ttu-id="7da5d-140">tooattach hello profileru tooa proces, na hello **analyzovat** nabídce zvolte **profileru** a **Attach/Detach**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-140">tooattach hello profiler tooa process, on hello **Analyze** menu, choose **Profiler** and **Attach/Detach**.</span></span>

![Připojte profile – možnost][6]

<span data-ttu-id="7da5d-142">Pro roli pracovního procesu najít hello WaWorkerHost.exe procesu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-142">For a worker role, find hello WaWorkerHost.exe process.</span></span>

![Proces WaWorkerHost][7]

<span data-ttu-id="7da5d-144">Pokud složky projektu na síťové jednotce, profileru hello se zeptá, tooprovide jiné umístění toosave hello profilace sestavy.</span><span class="sxs-lookup"><span data-stu-id="7da5d-144">If your project folder is on a network drive, hello profiler will ask you tooprovide another location toosave hello profiling reports.</span></span>

 <span data-ttu-id="7da5d-145">Můžete taky přiložit tooa webovou roli připojením tooWaIISHost.exe.</span><span class="sxs-lookup"><span data-stu-id="7da5d-145">You can also attach tooa web role by attaching tooWaIISHost.exe.</span></span>
<span data-ttu-id="7da5d-146">Pokud existují více procesů role v aplikaci, musíte toouse hello processID toodistinguish je.</span><span class="sxs-lookup"><span data-stu-id="7da5d-146">If there are multiple worker role processes in your application, you need toouse hello processID toodistinguish them.</span></span> <span data-ttu-id="7da5d-147">Hello processID můžete dotazovat prostřednictvím kódu programu díky přístupu k objektu proces hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-147">You can query hello processID programmatically by accessing hello Process object.</span></span> <span data-ttu-id="7da5d-148">Například pokud chcete přidat tato metoda kód toohello spustit hello RoleEntryPoint odvozené třídy v roli, můžete můžete prohlížet v protokolu v tooknow uživatelské prostředí emulátoru výpočtů hello co tooconnect proces pro.</span><span class="sxs-lookup"><span data-stu-id="7da5d-148">For example, if you add this code toohello Run method of hello RoleEntryPoint-derived class in a role, you can look at the log in hello Compute Emulator UI tooknow what process tooconnect to.</span></span>

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

<span data-ttu-id="7da5d-149">tooview hello protokolu spuštění hello uživatelské prostředí emulátoru výpočtů.</span><span class="sxs-lookup"><span data-stu-id="7da5d-149">tooview hello log, start hello Compute Emulator UI.</span></span>

![Spustit hello uživatelské prostředí emulátoru výpočtů][8]

<span data-ttu-id="7da5d-151">Otevřete okno konzoly protokolu role pracovního procesu hello v hello uživatelské prostředí emulátoru výpočtů kliknutím na záhlaví okna konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-151">Open hello worker role log console window in hello Compute Emulator UI by clicking on hello console window's title bar.</span></span> <span data-ttu-id="7da5d-152">Zobrazí ID procesu hello v protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-152">You can see hello process ID in hello log.</span></span>

![ID procesu zobrazení][9]

<span data-ttu-id="7da5d-154">Jeden, který jste připojili, proveďte kroky hello ve scénáři hello tooreproduce uživatelského rozhraní (v případě potřeby) vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7da5d-154">One you've attached, perform hello steps in your application's UI (if needed) tooreproduce hello scenario.</span></span>

<span data-ttu-id="7da5d-155">Když chcete toostop profilace, zvolte hello **zastavit profilování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="7da5d-155">When you want toostop profiling, choose hello **Stop Profiling** link.</span></span>

![Zastavit profilování možnost][10]

## <a name="3-view-performance-reports"></a><span data-ttu-id="7da5d-157">3: Zobrazit sestavy pro zvýšení výkonu</span><span class="sxs-lookup"><span data-stu-id="7da5d-157">3: View performance reports</span></span>
<span data-ttu-id="7da5d-158">Zobrazí se sestava Hello výkonu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7da5d-158">hello performance report for your application is displayed.</span></span>

<span data-ttu-id="7da5d-159">V tomto okamžiku hello profileru zastaví provádění, uloží data v souboru .vsp a zobrazí sestavu, která obsahuje analýzu tato data.</span><span class="sxs-lookup"><span data-stu-id="7da5d-159">At this point, hello profiler stops executing, saves data in a .vsp file, and displays a report that shows an analysis of this data.</span></span>

![Sestav profileru][11]

<span data-ttu-id="7da5d-161">Pokud se zobrazí String.wstrcpy v hello aktivní trase, klikněte na volbu pouze vlastní kód toochange hello zobrazit pouze tooshow uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-161">If you see String.wstrcpy in hello Hot Path, click on Just My Code toochange hello view tooshow user code only.</span></span>  <span data-ttu-id="7da5d-162">Pokud se zobrazí String.Concat, zkuste kliknutím na tlačítko Zobrazit všechny kód hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-162">If you see String.Concat, try pressing hello Show All Code button.</span></span>

<span data-ttu-id="7da5d-163">Měli byste vidět hello zřetězení metoda a String.Concat zabírají velkou část čas provádění hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-163">You should see hello Concatenate method and String.Concat taking up a large portion of hello execution time.</span></span>

![Analýzy zprávy][12]

<span data-ttu-id="7da5d-165">Pokud jste přidali hello řetězec zřetězení kód v tomto článku, měli byste vidět upozornění v hello seznam úkolů pro tuto.</span><span class="sxs-lookup"><span data-stu-id="7da5d-165">If you added hello string concatenation code in this article, you should see a warning in hello Task List for this.</span></span> <span data-ttu-id="7da5d-166">Může se také zobrazit upozornění, že je nadměrné množství uvolňování paměti, což je z důvodu toohello počet řetězce, které jsou vytvořeny a zlikvidován.</span><span class="sxs-lookup"><span data-stu-id="7da5d-166">You may also see a warning that there is an excessive amount of garbage collection, which is due toohello number of strings that are created and disposed.</span></span>

![Upozornění výkonu][14]

## <a name="4-make-changes-and-compare-performance"></a><span data-ttu-id="7da5d-168">4: proveďte změny a porovnání výkonu</span><span class="sxs-lookup"><span data-stu-id="7da5d-168">4: Make changes and compare performance</span></span>
<span data-ttu-id="7da5d-169">Také můžete porovnat výkon hello před a po změně kódu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-169">You can also compare hello performance before and after a code change.</span></span>  <span data-ttu-id="7da5d-170">Zastavení hello spuštění procesu a upravit hello tooreplace hello řetězec zřetězení operaci kódu s použitím hello StringBuilder:</span><span class="sxs-lookup"><span data-stu-id="7da5d-170">Stop hello running process, and edit hello code tooreplace hello string concatenation operation with hello use of StringBuilder:</span></span>

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

<span data-ttu-id="7da5d-171">Udělat jiné výkonu spustit a potom porovnejte výkon hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-171">Do another performance run, and then compare hello performance.</span></span> <span data-ttu-id="7da5d-172">V hello prohlížeč výkonu, pokud hello spustí jsou ve hello stejné relaci, můžete právě vyberte obě tyto sestavy, otevřete hello místní nabídky a zvolte **porovnat sestavy pro zvýšení výkonu**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-172">In hello Performance Explorer, if hello runs are in hello same session, you can just select both reports, open hello shortcut menu, and choose **Compare Performance Reports**.</span></span> <span data-ttu-id="7da5d-173">Pokud chcete, aby toocompare s spuštěný v jiné relaci výkonu, otevřete hello **analyzovat** nabídce a zvolte **porovnat sestavy pro zvýšení výkonu**.</span><span class="sxs-lookup"><span data-stu-id="7da5d-173">If you want toocompare with a run in another performance session, open hello **Analyze** menu, and choose **Compare Performance Reports**.</span></span> <span data-ttu-id="7da5d-174">Zadejte oba soubory v hello zobrazeném dialogu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-174">Specify both files in hello dialog box that appears.</span></span>

![Porovnání výkonu sestavy možnost][15]

<span data-ttu-id="7da5d-176">sestavy Hello zvýrazněte rozdíly mezi dvěma spustí hello.</span><span class="sxs-lookup"><span data-stu-id="7da5d-176">hello reports highlight differences between hello two runs.</span></span>

![Sestavy porovnání][16]

<span data-ttu-id="7da5d-178">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="7da5d-178">Congratulations!</span></span> <span data-ttu-id="7da5d-179">Začnete s hello profileru.</span><span class="sxs-lookup"><span data-stu-id="7da5d-179">You've gotten started with hello profiler.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7da5d-180">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="7da5d-180">Troubleshooting</span></span>
* <span data-ttu-id="7da5d-181">Zkontrolujte, zda jsou profilace sestavení pro vydání a spustit bez ladění.</span><span class="sxs-lookup"><span data-stu-id="7da5d-181">Make sure you are profiling a Release build and start without debugging.</span></span>
* <span data-ttu-id="7da5d-182">Pokud hello možnost Attach/Detach není povolen v nabídce profileru hello, spusťte hello Průvodce výkonu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-182">If hello Attach/Detach option is not enabled on hello Profiler menu, run hello Performance Wizard.</span></span>
* <span data-ttu-id="7da5d-183">Použití hello uživatelské prostředí emulátoru výpočtů tooview hello stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="7da5d-183">Use hello Compute Emulator UI tooview hello status of your application.</span></span> 
* <span data-ttu-id="7da5d-184">Pokud máte potíže s spuštění aplikace v emulátoru hello nebo připojení hello profileru, vypněte hello emulátoru služby výpočty a restartujte ji.</span><span class="sxs-lookup"><span data-stu-id="7da5d-184">If you have problems starting applications in hello emulator, or attaching hello profiler, shut down hello compute emulator and restart it.</span></span> <span data-ttu-id="7da5d-185">Pokud který hello problém nevyřeší, zkuste restartování.</span><span class="sxs-lookup"><span data-stu-id="7da5d-185">If that doesn't solve hello problem, try rebooting.</span></span> <span data-ttu-id="7da5d-186">Tomuto problému může dojít, pokud používáte hello výpočetní emulátor toosuspend a odebrat spuštěné nasazení.</span><span class="sxs-lookup"><span data-stu-id="7da5d-186">This problem can occur if you use hello Compute Emulator toosuspend and remove running deployments.</span></span>
* <span data-ttu-id="7da5d-187">Pokud jste používali některé z hello profilace příkazy z příkazového řádku, zejména hello globální nastavení, ujistěte se, že byla volána /globaloff vsperfclrenv – a že VsPerfMon.exe byla vypnuta.</span><span class="sxs-lookup"><span data-stu-id="7da5d-187">If you have used any of hello profiling commands from the command line, especially hello global settings, make sure that VSPerfClrEnv /globaloff has been called and that VsPerfMon.exe has been shut down.</span></span>
* <span data-ttu-id="7da5d-188">Pokud při vzorkování, se zobrazí zpráva hello "PRF0025: Nebyla shromážděna žádná data," Zkontrolujte, zda text hello proces připojené aktivity toohas procesoru.</span><span class="sxs-lookup"><span data-stu-id="7da5d-188">If when sampling, you see hello message "PRF0025: No data was collected," check that hello process you attached toohas CPU activity.</span></span> <span data-ttu-id="7da5d-189">Aplikace, které nejsou provádění veškerou práci, výpočetní nemusí vracet žádné data vzorkování.</span><span class="sxs-lookup"><span data-stu-id="7da5d-189">Applications that are not doing any computational work might not produce any sampling data.</span></span>  <span data-ttu-id="7da5d-190">Je také možné, že hello proces byl ukončen před provedením jakékoli vzorkování.</span><span class="sxs-lookup"><span data-stu-id="7da5d-190">It's also possible that hello process exited before any sampling was done.</span></span> <span data-ttu-id="7da5d-191">Zkontrolujte toosee, která metoda Run hello pro roli, která jsou profilace neskončí.</span><span class="sxs-lookup"><span data-stu-id="7da5d-191">Check toosee that hello Run method for a role that you are profiling does not terminate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7da5d-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7da5d-192">Next Steps</span></span>
<span data-ttu-id="7da5d-193">Instrumentace Azure binárních souborů v emulátoru hello není podporována v sadě Visual Studio profiler hello, ale pokud chcete tootest přidělení paměti, můžete tuto možnost, při vytváření profilu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-193">Instrumenting Azure binaries in hello emulator is not supported in hello Visual Studio profiler, but if you want tootest memory allocation, you can choose that option when profiling.</span></span> <span data-ttu-id="7da5d-194">Můžete také profilace souběžného zpracování, který vám pomůže určit, zda jsou vláken plýtvání čas neslučitelných pro zámky nebo vrstvy, interakce profilování, pomáhá sledovat problémy s výkonem při interakci mezi vrstvami aplikace, které nejvíce často mezi hello datové vrstvy a roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="7da5d-194">You can also choose concurrency profiling, which helps you determine whether threads are wasting time competing for locks, or tier interaction profiling, which helps you track down performance problems when interacting between tiers of an application, most frequently between hello data tier and a worker role.</span></span>  <span data-ttu-id="7da5d-195">Můžete zobrazit hello databázové dotazy, které vaše aplikace generuje a použít hello profilace data tooimprove používání hello databáze.</span><span class="sxs-lookup"><span data-stu-id="7da5d-195">You can view hello database queries that your app generates and use hello profiling data tooimprove your use of hello database.</span></span> <span data-ttu-id="7da5d-196">Informace o vytváření profilů interakce vrstvy, najdete v příspěvku blogu hello [návod: použití hello profileru interakce vrstvy v sadě Visual Studio Team System 2010][3].</span><span class="sxs-lookup"><span data-stu-id="7da5d-196">For information about tier interaction profiling, see hello blog post [Walkthrough: Using hello Tier Interaction Profiler in Visual Studio Team System 2010][3].</span></span>

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
