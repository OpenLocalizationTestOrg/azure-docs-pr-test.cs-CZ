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
# <a name="testing-hello-performance-of-a-cloud-service-locally-in-hello-azure-compute-emulator-using-hello-visual-studio-profiler"></a>Testování hello výkon cloudové služby místně v hello Azure výpočetní emulátor pomocí hello Visual Studio Profiler
Jsou k dispozici pro testování výkonu hello cloudových služeb různé nástroje a techniky.
Když publikujete tooAzure cloudové služby, může mít Visual Studio shromažďovat data profilování a potom analyzovat místně, jak je popsáno v [profilace aplikace Azure][1].
Můžete také diagnostiky tootrack čítače celou řadu výkonu, jak je popsáno v [pomocí čítačů výkonu v Azure][2].
Můžete také chtít tooprofile aplikace místně v emulátoru služby výpočty hello ještě před nasazením toohello cloudu.

Tento článek se zabývá hello procesoru vzorkování metoda profilování, což lze provést místně v emulátoru hello. Vzorkování procesoru je, že metoda profilace, který není velmi nežádoucí. V intervalu určené vzorkování profileru hello pořídí snímek hello zásobníku volání. Hello data se shromažďují v časovém intervalu a zobrazí v sestavě. Tato metoda profilace obvykle tooindicate, kde výpočetně náročné aplikace se právě provádí většinu práce hello procesoru.  Tato poskytuje hello možnost toofocus cestou hello"aktivní" kde vaše aplikace je výdaje hello většinu času.

## <a name="1-configure-visual-studio-for-profiling"></a>1: konfigurace Visual Studia pro profilaci
Existují nejprve pár možností konfigurace sady Visual Studio, které mohou být užitečné při vytváření profilu. představu o tom, toomake hello sestav profilace, budete potřebovat symboly (soubory PDB) pro aplikaci a také symboly pro knihovny systému. Budete muset toomake se, zda odkazujete servery k dispozici symbolů hello. toodo to na hello **nástroje** ve Visual Studiu zvolte v nabídce zvolte **možnosti**, zvolte **ladění**, pak **symboly**. Ujistěte se, že servery symbolů Microsoft je uveden v části **symbolů umístění souborů (.pdb)**.  Můžete taky odkazovat http://referencesource.microsoft.com/symbols, což může mít další symbol soubory.

![Možnosti – symbol][4]

V případě potřeby můžete zjednodušit hello hlásí, že hello profileru generuje nastavením pouze můj kód. S pouze můj kód povolena jsou zjednodušené zásobníky volání funkce tak, aby zcela interní toolibraries, který volá a hello rozhraní .NET Framework jsou skryta hello sestavy. Na hello **nástroje** nabídce zvolte **možnosti**. Potom rozbalte hello **nástroje pro sledování výkonu** uzel a zvolte **Obecné**. Zaškrtněte políčko hello pro **povolit volbu pouze vlastní kód pro sestav profileru**.

![Pouze můj kód možnosti][17]

Tyto pokyny můžete použít existující projekt nebo s nový projekt.  Pokud vytvoříte nový hello tootry projektu níže popsané postupy, zvolte C# **Azure Cloud Service** projektu a vyberte **webovou roli** a **Role pracovního procesu**.

![Role projektu Azure Cloud Service][5]

Pro účely, například přidat některé projekt tooyour kód, který přebírá mnoho času a ukazuje některé zřejmé výkonu problém. Například přidejte hello následující projekt role pracovního procesu tooa kódu:

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

Tento kód volejte z hello metodě RunAsync v roli pracovního procesu hello RoleEntryPoint odvozené třídy. (Ignorovat upozornění hello o metodě hello systémem synchronně.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace hello following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Sestavení a spuštění cloudové služby místně bez ladění (Ctrl + F5), s konfigurací řešení hello nastavit příliš**verze**. To zajistí, že všechny soubory a složky jsou vytvořeny pro spuštění aplikace hello místně a zajistí, že jsou spuštěné všechny emulátorů hello. Spusťte hello uživatelské prostředí emulátoru výpočtů z tooverify hello panelu, spuštění své role pracovního procesu.

## <a name="2-attach-tooa-process"></a>2: připojit tooa proces
Místo profilace aplikace hello spuštěním z hello Visual Studio 2010 IDE, je nutné připojit tooa profileru hello spuštění procesu. 

tooattach hello profileru tooa proces, na hello **analyzovat** nabídce zvolte **profileru** a **Attach/Detach**.

![Připojte profile – možnost][6]

Pro roli pracovního procesu najít hello WaWorkerHost.exe procesu.

![Proces WaWorkerHost][7]

Pokud složky projektu na síťové jednotce, profileru hello se zeptá, tooprovide jiné umístění toosave hello profilace sestavy.

 Můžete taky přiložit tooa webovou roli připojením tooWaIISHost.exe.
Pokud existují více procesů role v aplikaci, musíte toouse hello processID toodistinguish je. Hello processID můžete dotazovat prostřednictvím kódu programu díky přístupu k objektu proces hello. Například pokud chcete přidat tato metoda kód toohello spustit hello RoleEntryPoint odvozené třídy v roli, můžete můžete prohlížet v protokolu v tooknow uživatelské prostředí emulátoru výpočtů hello co tooconnect proces pro.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

tooview hello protokolu spuštění hello uživatelské prostředí emulátoru výpočtů.

![Spustit hello uživatelské prostředí emulátoru výpočtů][8]

Otevřete okno konzoly protokolu role pracovního procesu hello v hello uživatelské prostředí emulátoru výpočtů kliknutím na záhlaví okna konzoly hello. Zobrazí ID procesu hello v protokolu hello.

![ID procesu zobrazení][9]

Jeden, který jste připojili, proveďte kroky hello ve scénáři hello tooreproduce uživatelského rozhraní (v případě potřeby) vaší aplikace.

Když chcete toostop profilace, zvolte hello **zastavit profilování** odkaz.

![Zastavit profilování možnost][10]

## <a name="3-view-performance-reports"></a>3: Zobrazit sestavy pro zvýšení výkonu
Zobrazí se sestava Hello výkonu pro vaši aplikaci.

V tomto okamžiku hello profileru zastaví provádění, uloží data v souboru .vsp a zobrazí sestavu, která obsahuje analýzu tato data.

![Sestav profileru][11]

Pokud se zobrazí String.wstrcpy v hello aktivní trase, klikněte na volbu pouze vlastní kód toochange hello zobrazit pouze tooshow uživatelského kódu.  Pokud se zobrazí String.Concat, zkuste kliknutím na tlačítko Zobrazit všechny kód hello.

Měli byste vidět hello zřetězení metoda a String.Concat zabírají velkou část čas provádění hello.

![Analýzy zprávy][12]

Pokud jste přidali hello řetězec zřetězení kód v tomto článku, měli byste vidět upozornění v hello seznam úkolů pro tuto. Může se také zobrazit upozornění, že je nadměrné množství uvolňování paměti, což je z důvodu toohello počet řetězce, které jsou vytvořeny a zlikvidován.

![Upozornění výkonu][14]

## <a name="4-make-changes-and-compare-performance"></a>4: proveďte změny a porovnání výkonu
Také můžete porovnat výkon hello před a po změně kódu.  Zastavení hello spuštění procesu a upravit hello tooreplace hello řetězec zřetězení operaci kódu s použitím hello StringBuilder:

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

Udělat jiné výkonu spustit a potom porovnejte výkon hello. V hello prohlížeč výkonu, pokud hello spustí jsou ve hello stejné relaci, můžete právě vyberte obě tyto sestavy, otevřete hello místní nabídky a zvolte **porovnat sestavy pro zvýšení výkonu**. Pokud chcete, aby toocompare s spuštěný v jiné relaci výkonu, otevřete hello **analyzovat** nabídce a zvolte **porovnat sestavy pro zvýšení výkonu**. Zadejte oba soubory v hello zobrazeném dialogu.

![Porovnání výkonu sestavy možnost][15]

sestavy Hello zvýrazněte rozdíly mezi dvěma spustí hello.

![Sestavy porovnání][16]

Blahopřejeme! Začnete s hello profileru.

## <a name="troubleshooting"></a>Řešení potíží
* Zkontrolujte, zda jsou profilace sestavení pro vydání a spustit bez ladění.
* Pokud hello možnost Attach/Detach není povolen v nabídce profileru hello, spusťte hello Průvodce výkonu.
* Použití hello uživatelské prostředí emulátoru výpočtů tooview hello stavu aplikace. 
* Pokud máte potíže s spuštění aplikace v emulátoru hello nebo připojení hello profileru, vypněte hello emulátoru služby výpočty a restartujte ji. Pokud který hello problém nevyřeší, zkuste restartování. Tomuto problému může dojít, pokud používáte hello výpočetní emulátor toosuspend a odebrat spuštěné nasazení.
* Pokud jste používali některé z hello profilace příkazy z příkazového řádku, zejména hello globální nastavení, ujistěte se, že byla volána /globaloff vsperfclrenv – a že VsPerfMon.exe byla vypnuta.
* Pokud při vzorkování, se zobrazí zpráva hello "PRF0025: Nebyla shromážděna žádná data," Zkontrolujte, zda text hello proces připojené aktivity toohas procesoru. Aplikace, které nejsou provádění veškerou práci, výpočetní nemusí vracet žádné data vzorkování.  Je také možné, že hello proces byl ukončen před provedením jakékoli vzorkování. Zkontrolujte toosee, která metoda Run hello pro roli, která jsou profilace neskončí.

## <a name="next-steps"></a>Další kroky
Instrumentace Azure binárních souborů v emulátoru hello není podporována v sadě Visual Studio profiler hello, ale pokud chcete tootest přidělení paměti, můžete tuto možnost, při vytváření profilu. Můžete také profilace souběžného zpracování, který vám pomůže určit, zda jsou vláken plýtvání čas neslučitelných pro zámky nebo vrstvy, interakce profilování, pomáhá sledovat problémy s výkonem při interakci mezi vrstvami aplikace, které nejvíce často mezi hello datové vrstvy a roli pracovního procesu.  Můžete zobrazit hello databázové dotazy, které vaše aplikace generuje a použít hello profilace data tooimprove používání hello databáze. Informace o vytváření profilů interakce vrstvy, najdete v příspěvku blogu hello [návod: použití hello profileru interakce vrstvy v sadě Visual Studio Team System 2010][3].

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
