---
title: "aaaDebugging Azure cloudové služby nebo virtuálního počítače v sadě Visual Studio | Microsoft Docs"
description: "Ladění cloudové služby nebo virtuálního počítače v sadě Visual Studio"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 945e06e0-2100-41af-b218-72347367ddab
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 32a326430021ba2ea9317a6a71fa005d4b87c273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Ladění služby Azure cloudové služby nebo virtuálního počítače v sadě Visual Studio
Visual Studio poskytuje různé možnosti pro ladění cloudových služeb Azure a virtuálních počítačů.

## <a name="debug-your-cloud-service-on-your-local-computer"></a>Ladění cloudové služby na místním počítači
Můžete uložit čas a peníze pomocí toodebug emulátor výpočtů Azure hello cloudové služby v místním počítači. Ladění služby místně, před nasazením, můžete zlepšit spolehlivost a výkon bez placení pro dobu výpočtů. Ale některé může dojít k chybám pouze při spuštění cloudové služby v Azure sám sebe. Tyto chyby můžete ladit, pokud povolíte vzdálené ladění při publikování služby a pak připojte instanci role tooa hello ladicí program.

emulátor Hello simuluje služby Azure Compute hello a běží ve vašem místním prostředí, aby mohli testování a ladění cloudové služby, před nasazením. obslužné rutiny emulátoru Hello hello životní cyklus instance role a poskytuje přístup toosimulated prostředky, například místní úložiště. Při ladění a spouštění služby ze sady Visual Studio, automaticky spustí hello emulátor jako pozadí aplikaci a poté nasadí emulátoru toohello vaší služby. Můžete použít tooview hello emulátor služby při spuštění v místním prostředí hello. Můžete spustit úplnou verzi hello nebo hello expresní verze emulátoru hello. (Počínaje Azure 2.3, hello expresní verze emulátoru hello je výchozí hello.) V tématu [pomocí emulátoru Express tooRun a ladění cloudové služby místně](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="toodebug-your-cloud-service-on-your-local-computer"></a>toodebug vaše cloudové služby na místním počítači
1. V řádku nabídek hello zvolte **ladění**, **spustit ladění** toorun projektu Azure cloud service. Jako alternativu můžete stisknutím klávesy F5. Uvidíte, že se spouští zprávu, která hello výpočetní emulátor. Když se spustí hello emulátoru, potvrdí hello ikonu na hlavním panelu ho.

    ![Azure emulátoru na panelu systému hello](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)
2. Zobrazení hello uživatelského rozhraní pro emulátoru služby výpočty hello otevřením hello místní nabídku pro hello Azure ikonu v oznamovací oblasti hello a potom vyberte **zobrazit uživatelské prostředí emulátoru výpočtů**.

    Hello levém podokně hello uživatelského rozhraní obsahuje hello služby, které jsou aktuálně nasazené toohello výpočetní emulátor a hello instance rolí, které běží jednotlivých služeb. V pravém podokně hello můžete životního cyklu toodisplay hello služby nebo role, protokolování a diagnostické informace. Pokud hello zaměřit na horním okrajem hello zahrnuté okna, rozšíří toofill hello pravém podokně.
3. Krok prostřednictvím aplikace hello výběrem příkazů na hello **ladění** nabídce a nastavení zarážek v kódu. Krocích aplikace hello v ladicím programu hello hello podokna aktualizují hello aktuální stav aplikace hello. Když zastavíte ladění, nasazení aplikace hello je odstraněn. Pokud vaše aplikace obsahuje webovou roli a nastavili jste hello spuštění akce vlastnost toostart hello webový prohlížeč, Visual Studio spustí webovou aplikaci v prohlížeči hello. Pokud změníte hello počet instancí role v konfiguraci služby hello, musíte zastavit cloudovou službu a poté znovu spusťte ladění tak, že ladíte tyto nové instance role hello.

    **Poznámka:** při zastavení, spuštění nebo ladění vaší služby, hello emulátoru služby výpočty místní a emulátoru úložiště nejsou byla zastavena. Explicitně je potřeba zastavit z oznamovací oblasti hello.

## <a name="debug-a-cloud-service-in-azure"></a>Ladění cloudové služby v Azure
toodebug Cloudová služba ze vzdáleného počítače, je nutné povolit tuto funkci explicitně při nasazení cloudové služby tak, aby požadované na virtuálních počítačích hello spuštěné instance role jsou nainstalovány služby (například msvsmon.exe). Pokud nebylo povolení vzdáleného ladění při publikování služby hello, máte toorepublish hello služby s povoleným laděním vzdálené.

Pokud povolíte vzdálené ladění pro cloudovou službu, ho nebude vykazovat snížení výkonu nebo toho vám být účtovány další poplatky. Neměli byste používat vzdáleného ladění na produkční služby, protože klienty, kteří používají službu hello může být nepříznivě ovlivněn.

> [!NOTE]
> Když publikujete Cloudová služba ze sady Visual Studio, můžete povolit **IntelliTrace** pro všechny role v tom, že služba hello této cílové rozhraní .NET Framework 4 nebo hello rozhraní .NET Framework 4.5. Pomocí **IntelliTrace**, můžete prozkoumat události, k nimž došlo v instanci role v posledních hello a reprodukujte hello kontext z této doby. V tématu [ladění publikované Cloudová služba se IntelliTrace a Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) a [IntelliTrace pomocí](https://msdn.microsoft.com/library/dd264915.aspx).
>
>

### <a name="tooenable-remote-debugging-for-a-cloud-service"></a>tooenable vzdáleného ladění pro cloudové služby
1. Otevřete hello místní nabídku pro hello projektu Azure a pak vyberte **publikovat**.
2. Vyberte hello **pracovní** prostředí a hello **ladění** konfigurace.

    Toto je pouze platí. Můžete se taky rozhodnout toorun vaše testovací prostředí v produkčním prostředí. Pokud povolíte vzdáleného ladění na hello provozním prostředí však může ovlivnit nepříznivě uživatele. Konfigurace verze hello můžete, ale hello ladění konfigurace umožňuje snazší ladění.

    ![Vyberte konfiguraci ladění hello](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)
3. Postupujte podle kroků obvyklé hello, ale vyberte hello **povolení vzdáleného ladicího programu u všech rolí** políčko hello **Upřesnit nastavení** kartě.

    ![Konfigurace ladění](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="tooattach-hello-debugger-tooa-cloud-service-in-azure"></a>tooattach hello ladicí program tooa cloudové služby v Azure
1. V Průzkumníku serveru rozbalte uzel hello vaší cloudové služby.
2. Otevřete hello místní nabídky pro hello roli nebo role instance toowhich chcete tooattach a potom vyberte **připojit ladicí program**.

    Pokud ladíte roli, připojí ladicí program Visual Studio hello tooeach instance dané role. ladicí program Hello dojde k přerušení na zarážky pro hello první role instanci, která spouští tohoto řádku kódu a splňuje všechny podmínky této zarážek. Při ladění instance, připojí ladicí program hello tooonly, instanci a zalomení na zarážku jenom v případě, že konkrétní instanci běží tohoto řádku kódu a splňuje hello na zarážek podmínky.

    ![Připojit ladicí program](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)
3. Po ladicího programu hello připojí tooan instance, ladění jako obvykle. ladicí program Hello automaticky připojí toohello odpovídající hostitelský proces pro vaši roli. V závislosti na tom, jaké hello role je toow3wp.exe připojí ladicí program hello, WaWorkerHost.exe nebo WaIISHost.exe. je připojen ladicí program tooverify hello proces toowhich hello, rozbalte uzel hello instance v Průzkumníku serveru. V tématu [architektura Role Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) Další informace o Azure procesy.

    ![Vyberte typ kód – dialogové okno](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
4. tooidentify hello procesy, které je připojen ladicí program hello toowhich, otevřete hello procesy dialogové okno, v nabídce hello panelu Výběr ladění, Windows, procesy. (Klávesové: Ctrl + Alt + Z) toodetach konkrétního procesu otevřete jeho místní nabídce a potom vyberte **odpojit proces**. Nebo, najít uzel hello instance v Průzkumníku serveru, najít hello proces, otevřete jeho místní nabídky a pak vyberte **odpojit proces**.

    ![Ladění procesů](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

> [!WARNING]
> Vyhněte se dlouho se zastavuje na zarážky při vzdáleném ladění. Azure zpracovává proces, který je zastavena po dobu delší než několik minut jako reagovat a zastaví odesílání přenosů toothat instance. Pokud zastavíte příliš dlouho, msvsmon.exe odpojí z procesu hello.
>
>

ladicí program hello toodetach ze všech procesů ve vaší instanci nebo role, otevřete hello místní nabídku pro hello role nebo instance, kterou ladíte a potom vyberte **odpojit ladicí program**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Omezení pro vzdálené ladění v Azure
Z Azure SDK 2.3 vzdálené ladění má následující omezení hello.

* S vzdálené ladění povoleno, nelze publikovat Cloudová služba, ve kterém žádné role, má víc než 25 instancí.
* ladicí program Hello používá 30400 too30424 porty, 31400 too31424 a 32400 too32424. Pokud toouse pokusíte některý z těchto portů, nebudete moct toopublish, služby a jeden z následujících chybových zpráv hello se zobrazí v protokolu aktivit hello pro Azure:

  * Chyba při ověřování hello souboru .cscfg proti souboru .csdef hello.
    Hello vyhrazený rozsah portů "rozsah" pro koncový bod, který Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector role role se překrývá s již definovaný port nebo rozsah.
  * Přidělení se nezdařilo. Opakujte akci později, zkuste zmenšit hello velikost virtuálního počítače nebo počet instancí role nebo vyzkoušejte nasazení tooa jiné oblasti.

## <a name="debugging-azure-virtual-machines"></a>Ladění virtuální počítače Azure
Můžete ladit programy, které běží na virtuálních počítačích Azure s použitím Průzkumníka serveru v sadě Visual Studio. Když povolíte vzdálené ladění na virtuální počítač Azure, Azure nainstaluje hello vzdáleného ladění rozšíření ve virtuálním počítači hello. Pak můžete připojit tooprocesses hello virtuálního počítače a ladění běžným způsobem.

> [!NOTE]
> Virtuální počítače vytvořené přes hello Azure resource manager protokolů můžete vzdáleně ladit pomocí Průzkumníku cloudu ve Visual Studiu 2015. Další informace najdete v tématu [Správa prostředků Azure pomocí Průzkumníku cloudu](http://go.microsoft.com/fwlink/?LinkId=623031).
>
>

### <a name="toodebug-an-azure-virtual-machine"></a>toodebug virtuální počítač Azure
1. V Průzkumníku serveru rozbalte uzel hello virtuální počítače a vyberte hello uzel hello virtuálního počítače, které chcete toodebug.
2. Otevřete hello kontextovou nabídku a vyberte **povolit ladění**. Když se zobrazí dotaz, pokud jste si jisti, zda chcete tooenable ladění na hello virtuálního počítače, vyberte **Ano**.

    Azure nainstaluje hello vzdáleného ladění rozšíření ladění tooenable hello virtuálního počítače.

    ![Virtuální počítač povolit ladění příkaz](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Protokol činnosti Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
3. Po dokončení instalace vzdáleného ladění rozšíření hello otevřete kontextu nabídku hello virtuálního počítače a vyberte možnost **připojit ladicí program...**

    Azure získá seznam procesů, hello hello virtuálního počítače a zobrazí je v dialogovém okně tooProcess hello připojit.

    ![Připojit ladicí program příkaz](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
4. V hello **připojit tooProcess** dialogové okno, vyberte **vyberte** toolimit hello výsledky seznamu tooshow pouze hello typy kódu chcete toodebug. Můžete ladit 32 - nebo 64bitový spravovaný kód, nativního kódu nebo obojí.

    ![Vyberte typ kód – dialogové okno](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
5. Vyberte hello procesy mají toodebug hello virtuálního počítače a potom vyberte **Attach**. Například můžete zvolit proces w3wp.exe hello, pokud jste chtěli toodebug webové aplikace na virtuálním počítači hello. V tématu [ladění jeden nebo více procesy v sadě Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) a [architektura Role Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) Další informace.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Vytvoření webového projektu a virtuální počítač pro ladění
Před publikováním projektu Azure, může pro vás užitečné tootest v prostředí s omezením podporující ladění a testování scénáře a kde můžete nainstalovat testování a monitorování programy. Jedním ze způsobů toodo jde tooremotely ladění aplikace s virtuálním počítačem.

Projekty Visual Studio ASP.NET nabízejí toocreate možnost užitečný virtuální počítač, který můžete použít pro testování aplikace. virtuální počítač Hello zahrnuje běžně potřeby koncových bodů například prostředí PowerShell, vzdálené plochy a WebDeploy.

### <a name="toocreate-a-web-project-and-a-virtual-machine-for-debugging"></a>toocreate webový projekt a virtuální počítač pro ladění
1. V sadě Visual Studio vytvořte novou webovou aplikaci ASP.NET.
2. V dialogovém okně Nový projekt ASP.NET hello, v hello Azure části, zvolte **virtuálního počítače** v rozevíracím seznamu hello. Nechte hello **vytvořit vzdálené prostředky** zaškrtnuté políčko. Vyberte **OK** tooproceed.

    Hello **vytvořit virtuální počítač na platformě Azure** zobrazí se dialogové okno.

    ![ASP.NET webového projektu dialogové okno vytvořit](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Poznámka:** budete dotázáni, toosign v tooyour účet Azure Pokud jste již přihlášeni.

1. Vyberte hello různá nastavení pro hello virtuální počítač a potom vyberte **OK**. V tématu [virtuální počítače](http://go.microsoft.com/fwlink/?LinkId=623033) Další informace.

    Název Hello, které zadáte pro název DNS bude hello název hello virtuálního počítače.

    ![Vytvoření virtuálního počítače na Azure dialogové okno](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure vytvoří hello virtuálního počítače a pak poskytuje a konfiguruje hello koncových bodů, jako je vzdálené plochy a nasazení webu
2. Po nakonfigurování hello virtuální počítač vyberte uzel hello virtuálního počítače v Průzkumníku serveru.
3. Otevřete hello kontextovou nabídku a vyberte **povolit ladění**. Když se zobrazí dotaz, pokud jste si jisti, zda chcete tooenable ladění na hello virtuálního počítače, vyberte **Ano**.

    Azure nainstaluje hello vzdálené ladění rozšíření toohello virtuální počítač tooenable ladění.

    ![Virtuální počítač povolit ladění příkaz](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Protokol činnosti Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
4. Publikování projektu, jak je uvedeno v [postupy: nasazení webového projektu pomocí publikování jedním kliknutím v sadě Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Protože chcete, aby toodebug na hello virtuálním počítači, na hello **nastavení** stránku hello **Publikovat Web** průvodci vyberte **ladění** jako hello konfigurace. Tím je zajištěno, že symboly kódu jsou k dispozici při ladění.

    ![Nastavení publikování](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)
5. V hello **možností publikování souboru**, vyberte **odebrat další soubory v cílovém umístění** Pokud hello projekt již byl nasazen na dřívější čas.
6. Když projekt hello publikuje v místní nabídce hello virtuálního počítače v Průzkumníku serveru, vyberte **připojit ladicí program...**

    Azure získá seznam procesů, hello hello virtuálního počítače a zobrazí je v dialogovém okně tooProcess hello připojit.

    ![Připojit ladicí program příkaz](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
7. V hello **připojit tooProcess** dialogové okno, vyberte **vyberte** toolimit hello výsledky seznamu tooshow pouze hello typy kódu chcete toodebug. Můžete ladit 32 - nebo 64bitový spravovaný kód, nativního kódu nebo obojí.

    ![Vyberte typ kód – dialogové okno](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
8. Vyberte hello procesy mají toodebug hello virtuálního počítače a potom vyberte **Attach**. Například můžete zvolit proces w3wp.exe hello, pokud jste chtěli toodebug webové aplikace na virtuálním počítači hello. V tématu [ladění jeden nebo více procesy v sadě Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) Další informace.

## <a name="next-steps"></a>Další kroky
* Použití **Intellitrace** toocollect protokolu volání a událostí ze serveru verze. V tématu [ladění publikované Cloudová služba se IntelliTrace a Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).
* Použití **Azure Diagnostics** toolog podrobné informace o spouštění kódu v rámci role, zda text hello role běží ve vývojovém prostředí hello nebo v Azure. V tématu [shromažďování dat protokolování pomocí Azure Diagnostics](http://go.microsoft.com/fwlink/p/?LinkId=400450).
