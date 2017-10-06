---
title: "aaaConfiguring diagnostiku pro Azure Cloud Services a virtuálních počítačů | Microsoft Docs"
description: "Popisuje, jak tooconfigure diagnostické informace pro ladění Azure cloude služeb a virtuálních počítačů (VM) v sadě Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: e70cd7b4-6298-43aa-adea-6fd618414c26
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 74cdf49413d6d89a84195070dd9d817da2463373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Konfiguraci diagnostiky pro cloudové služby Azure a virtuální počítače
Pokud budete potřebovat tootroubleshoot Azure cloudové služby nebo virtuálního počítače Azure, můžete nakonfigurovat Azure diagnostics snadněji pomocí sady Visual Studio. Azure diagnostics zaznamená data systému a data protokolování v hello virtuálních počítačů a instancí virtuálního počítače se systémem cloudové služby a přenosy dat do účtu úložiště podle vašeho výběru. V tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md) Další informace o protokolování v Azure diagnostics.

Toto téma ukazuje, jak tooenable a nakonfigurovat Azure diagnostics v sadě Visual Studio, před i po nasazení, a také na virtuálních počítačích Azure. Také vám ukazuje jak tooselect hello typy toocollect diagnostické informace a jak se shromáždí informace o hello tooview za ním.

Azure Diagnostics můžete nakonfigurovat v hello následující způsoby:

* Můžete změnit nastavení konfigurace diagnostiky prostřednictvím hello **konfigurace diagnostiky** dialogové okno v sadě Visual Studio. Hello nastavení se ukládají do souboru s názvem diagnostics.wadcfgx (diagnostics.wadcfg v Azure SDK 2.4 nebo starší). Alternativně můžete přímo upravit hello konfigurační soubor. Pokud ručně aktualizovat soubor hello, budou se změny konfigurace hello se vliv hello příštím nasazení hello cloudové služby tooAzure nebo spuštění hello služba v emulátoru hello.
* Použití **Průzkumník cloudu** nebo **Průzkumníka serveru** v sadě Visual Studio toochange hello nastavení diagnostiky pro spuštěné cloudové služby nebo virtuálního počítače.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnostics změny
Pro Azure SDK 2.6 projekty v sadě Visual Studio byly provedeny následující změny hello. (Tyto změny také použita toolater verze sady SDK pro Azure.)

* místní emulátoru Hello nyní podporuje diagnostiku. To znamená, že můžete shromažďovat diagnostická data a ujistěte se, že aplikace je vytvoření hello správné trasování při jste vývoj a testování v sadě Visual Studio. Hello připojovací řetězec `UseDevelopmentStorage=true` umožňuje shromažďování dat diagnostiky, když spouštíte projekt cloudové služby v sadě Visual Studio pomocí emulátoru úložiště Azure hello. Všechny diagnostická data se shromažďují v účtu úložiště hello (vývoj pro úložiště).
* Hello diagnostiky úložiště připojovací řetězce k účtu (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) je znovu uložené v souboru definice hello služby (.csdef). V Azure SDK 2.5 nebyl zadán účet úložiště diagnostiky hello v souboru diagnostics.wadcfgx hello.

Existují některé významné rozdíly mezi jak šlo hello připojovací řetězec v Azure SDK 2.4 a starší a jak to funguje v Azure SDK 2.6 nebo novější.

* Azure SDK 2.4 a starších verzích používal hello připojovací řetězec jako modul runtime hello diagnostiky modul plug-in tooget hello informace o účtu úložiště pro přenos diagnostické protokoly.
* Ve verzi 2.6 Azure SDK a později je rozšíření diagnostiky hello tooconfigure sady Visual Studio s informace o účtu úložiště odpovídající hello během publikování používají hello diagnostiky připojovací řetězec. připojovací řetězec Hello umožňuje definovat jiným účtům úložiště pro různé služby konfigurace, které Visual Studio budou používat při publikování. Ale vzhledem k tomu, že modul plug-in diagnostiky hello již není k dispozici (po Azure SDK 2.5), nelze povolit souboru .cscfg hello samostatně hello rozšíření diagnostiky. Máte tooenable hello rozšíření samostatně prostřednictvím nástrojů, jako je Visual Studio nebo prostředí PowerShell.
* toosimplify hello proces konfigurace rozšíření diagnostiky hello v prostředí PowerShell, výstup hello balíčku ze sady Visual Studio také obsahuje hello veřejné konfigurace XML pro hello rozšíření diagnostiky pro každou roli. Visual Studio použije hello diagnostiky připojovací řetězec toopopulate hello informace o účtu úložiště v konfiguraci veřejného hello. Hello veřejné konfigurační soubory jsou vytvořeny ve složce rozšíření hello a postupujte podle vzoru hello PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. Všechna nasazení na základě prostředí PowerShell můžete použít tento vzor toomap každý tooa konfigurace Role.
* Hello připojovací řetězec v souboru .cscfg hello používá také hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) tooaccess hello diagnostická data, může se objevit v hello **monitorování** kartě hello připojovací řetězec je potřeba. tooshow služby tooconfigure v hello podrobné dat portálu hello monitorování.

## <a name="migrating-projects-tooazure-sdk-26-and-later"></a>Migrace projekty tooAzure SDK 2.6 a novější
Při migraci z Azure SDK 2.5 tooAzure SDK 2.6 nebo novější, pokud jste měli účet úložiště diagnostiky zadané v souboru .wadcfgx hello, pak zůstane existuje. tootake výhod hello flexibilitu pomocí jiného úložiště účtů pro jiné úložiště, budete mít toomanually přidat hello připojovací řetězec tooyour projekt. Pokud jste migraci projektu z Azure SDK 2.4 nebo starší tooAzure SDK 2.6, pak hello diagnostiky, které se zachovají připojovací řetězce. Upozorňujeme však hello změny v tom, jak jsou připojovací řetězce považovány ve verzi 2.6 Azure SDK uvedeného v předchozí části hello.

### <a name="how-visual-studio-determines-hello-diagnostics-storage-account"></a>Určuje, jak Visual Studio účet úložiště diagnostiky hello
* Pokud připojovací řetězec diagnostiky je zadané v souboru .cscfg hello, Visual Studio použije ji rozšíření diagnostiky hello tooconfigure při publikování a při generování souborů xml hello veřejné konfigurace během balení.
* Pokud žádný diagnostiky připojovací řetězec je zadané v souboru .cscfg hello, pak Visual Studio spadne zpět toousing hello úložiště účet zadaný v hello .wadcfgx tooconfigure hello diagnostiky příponu souboru při publikování a generování veřejného hello soubory xml konfigurace při balení.
* Hello diagnostiky připojovací řetězec v souboru .cscfg hello má přednost před hello účet úložiště v souboru .wadcfgx hello. Pokud diagnostiky připojovací řetězec zadaný v souboru .cscfg hello, pak Visual Studio použije tento a ignoruje hello účet úložiště v .wadcfgx.

### <a name="what-does-hello-update-development-storage-connection-strings-checkbox-do"></a>Co hello "Aktualizovat připojovací řetězce s vývoj úložiště..." zaškrtávací políčko dělat?
Hello zaškrtávací políčko pro **aktualizace vývoj úložiště připojovací řetězce pro diagnostiku a ukládání do mezipaměti pomocí přihlašovacích údajů účtu úložiště Microsoft Azure při publikování tooMicrosoft Azure** nabízí pohodlný způsob tooupdate všechny vývoj úložiště připojovací řetězce k účtu s účtem úložiště Azure hello zadaný během publikování.

Předpokládejme například, zaškrtněte toto políčko a hello diagnostiky připojovací řetězec Určuje `UseDevelopmentStorage=true`. Při publikování tooAzure hello projektu sady Visual Studio automaticky aktualizovat hello diagnostiky připojovací řetězec s hello účet úložiště, který jste zadali v Průvodci publikovat hello. Pokud účet úložiště skutečné byl zadán jako hello diagnostiky připojovací řetězec, pak tento účet se ale používá místo.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostické funkce rozdíly mezi Azure SDK 2.4 a starší a Azure SDK, 2.5 nebo novější
Pokud upgradujete projekt z Azure SDK 2.4 tooAzure SDK, 2.5 nebo novější, byste měli mít na paměti hello následující rozdíly funkce diagnostiky.

* **Rozhraní API pro konfiguraci jsou zastaralé** – Programová konfigurace diagnostiky je k dispozici v Azure SDK 2.4 nebo starší verze, ale je zastaralý v Azure SDK 2.5 nebo novější. Pokud vaše konfigurace diagnostiky je aktuálně definována v kódu, budete potřebovat tooreconfigure tato nastavení ručně v hello migrované projektu, pro práci tookeep diagnostiky. Hello diagnostiky konfiguračního souboru pro Azure SDK 2.4 je diagnostics.wadcfg a diagnostics.wadcfgx pro Azure SDK 2.5 nebo novější.
* **Diagnostika pro cloudové aplikace, služby se dá nakonfigurovat jenom na úrovni role hello, nikoliv na úrovni instance hello.**
* **Pokaždé, když nasadíte aplikaci, konfiguraci diagnostiky hello je aktualizovat** – to může způsobit problémy parita, pokud změníte konfiguraci diagnostiky z Průzkumníka serveru a pak znovu nasadit aplikace.
* **V Azure SDK 2.5 nebo novější, jsou nakonfigurované výpisy stavu systému v souboru konfigurace diagnostiky hello, není v kódu** – Pokud máte nakonfigurované v kódu výpisy stavu systému, budete mít toomanually přenos hello konfigurace z kódu toohello konfiguračního souboru protože výpisů stavu systému hello nejsou přenesených během migrace tooAzure hello SDK 2.6.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Povolte diagnostiku v projekty cloudových služeb před jejich nasazením
V sadě Visual Studio můžete toocollect diagnostická data pro role, které běží v Azure, když spustíte hello služba v emulátoru hello ještě před nasazením. Všechna nastavení toodiagnostics změny v sadě Visual Studio jsou uložena v souboru konfigurace diagnostics.wadcfgx hello. Tato nastavení konfigurace zadejte účet úložiště hello uložení dat diagnostiky při nasazení cloudové služby.

[!INCLUDE [cloud-services-wad-warning](../includes/cloud-services-wad-warning.md)]

### <a name="tooenable-diagnostics-in-visual-studio-before-deployment"></a>Diagnostika tooenable v sadě Visual Studio před nasazením
1. V místní nabídce hello hello role, které vás zajímají, zvolte **vlastnosti**a potom zvolte hello **konfigurace** kartě v roli hello **vlastnosti** okno.
2. V hello **diagnostiky** část, ujistěte se, že hello **povolení diagnostiky** je zaškrtnuté políčko.
   
    ![Přístup k hello možnost povolení diagnostiky](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)
3. Zvolte hello třemi tečkami (...) tlačítko toospecify hello úložiště účet místo hello diagnostiky dat toobe uložené. Hello účet úložiště, který zvolíte, bude hello umístění, kde jsou uložené diagnostická data.
   
    ![Zadejte toouse účet úložiště hello](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)
4. V hello **vytvoření připojovacího řetězce úložiště** dialogové okno pole, určete, zda chcete tooconnect pomocí hello emulátoru úložiště Azure, předplatné Azure, nebo ručně zadali přihlašovací údaje.
   
    ![Dialogové okno účtu úložiště](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)
   
   * Pokud zvolíte možnost hello emulátor úložiště Microsoft Azure, je nastaven hello připojovací řetězec tooUseDevelopmentStorage = true.
   * Pokud vyberete volbu hello vaše předplatné, můžete vybrat hello předplatné Azure, které chcete toouse a hello název účtu. Je možné, že správa účtů hello tlačítko toomanage předplatné Azure.
   * Pokud si zvolíte možnost hello ručně zadali přihlašovací údaje, můžete se výzvami tooenter hello název a klíč hello Azure účet, který má toouse.
5. Zvolte hello **konfigurace** tlačítko tooview hello **konfigurace diagnostiky** dialogové okno. Každé kartě (s výjimkou **Obecné** a **adresáře protokolu**) představuje zdroj diagnostických dat, který můžete shromáždit. Karta výchozí Hello, **Obecné**, nabízí hello následující možnosti shromažďování dat diagnostiky: **pouze chyby**, **všechny informace o**, a **vlastní plán** . Hello výchozí možnost **pouze chyby**, trvá hello nejmenší velikost úložiště, protože nepřenese upozornění nebo trasování zpráv. Hello všechny přenosy možnost informace hello většinu informací a je, tedy hello nejnákladnější možnost z hlediska úložiště.
   
    ![Povolení a konfigurace Azure Diagnostika](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
6. V tomto příkladu vyberte hello **vlastní plán** možnost, můžete přizpůsobit hello data shromažďují.
7. Hello **kvóty disku v MB** pole určuje, kolik místa, které mají být tooallocate v účtu úložiště pro data diagnostiky. Pokud chcete, můžete změnit hello výchozí hodnotu.
8. Na každé kartě diagnostická data chcete toocollect, vyberte jeho **povolit přenos z <log type>**  zaškrtávací políčko. Například pokud chcete toocollect protokoly aplikací, vyberte hello **povolit přenos protokoly aplikací** políčko hello **protokoly aplikací** kartě. Zadejte také všechny ostatní informace, které vyžaduje každý typ dat diagnostiky. Části hello **konfigurace zdroje dat diagnostiky** dál v tomto tématu pro informace o konfiguraci na každé kartě.
9. Až poprvé povolíte shromažďování všech dat diagnostiky hello chcete, zvolte hello **OK** tlačítko.
10. Spusťte projekt Azure cloud service v sadě Visual Studio jako obvykle. Při používání aplikace se uložily informace protokolu hello, které jste povolili toohello účtu úložiště Azure, které zadáte.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Povolte diagnostiku ve virtuálních počítačích Azure
V sadě Visual Studio můžete toocollect dat diagnostiky pro virtuální počítače Azure.

### <a name="tooenable-diagnostics-in-azure-virtual-machines"></a>Diagnostika tooenable ve virtuálních počítačích Azure
1. V **Průzkumníka serveru**, vyberte hello Azure uzel a potom se připojte tooyour předplatné Azure, pokud jste již připojeni.
2. Rozbalte hello **virtuální počítače** uzlu. Můžete vytvořit nový virtuální počítač nebo vyberte ten, který již existuje.
3. V místní nabídce hello hello virtuálního počítače, které vás zajímají, zvolte **konfigurace**. Zobrazí dialogové okno Konfigurace virtuálního počítače hello.
   
    ![Konfigurace virtuálního počítače Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)
4. Pokud ještě není nainstalovaná, přidejte rozšíření diagnostiky agenta monitorování Microsoft hello. Toto rozšíření umožňuje shromažďování dat diagnostiky pro virtuální počítač Azure hello. V seznamu hello nainstalovaná rozšíření zvolte hello vyberte rozšíření k dispozici rozevírací nabídky a pak zvolte Microsoft Monitoring Agent Diagnostics.
   
    ![Instalace rozšíření virtuálního počítače Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)
   
   > [!NOTE]
   > Další rozšíření diagnostiky jsou k dispozici pro virtuální počítače. Další informace najdete v tématu rozšíření virtuálního počítače Azure a funkce.
   > 
   > 
5. Zvolte hello **přidat** tlačítko tooadd hello rozšíření a zobrazte její **konfigurace diagnostiky** dialogové okno.
6. Zvolte hello **konfigurace** tlačítko toospecify účet úložiště a potom zvolte hello **OK** tlačítko.
   
    Každé kartě (s výjimkou **Obecné** a **adresáře protokolu**) představuje zdroj diagnostických dat, který můžete shromáždit.
   
    ![Povolení a konfigurace Azure Diagnostika](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
   
    Karta výchozí Hello, **Obecné**, nabízí hello následující možnosti shromažďování dat diagnostiky: **pouze chyby**, **všechny informace o**, a **vlastní plán** . Hello výchozí možnost **pouze chyby**, trvá hello nejmenší velikost úložiště, protože nepřenese upozornění nebo trasování zpráv. Hello **všechny informace o** možnost přenosy hello většinu informací a je proto hello nejnákladnější možnost z hlediska úložiště.
7. V tomto příkladu vyberte hello **vlastní plán** možnost, můžete přizpůsobit hello data shromažďují.
8. Hello **kvóty disku v MB** pole určuje, kolik místa, které mají být tooallocate v účtu úložiště pro data diagnostiky. Pokud chcete, můžete změnit hello výchozí hodnotu.
9. Na každé kartě diagnostická data chcete toocollect, vyberte jeho **povolit přenos z <log type>**  zaškrtávací políčko.
   
    Například pokud chcete toocollect protokoly aplikací, vyberte hello **povolit přenos protokoly aplikací** políčko hello **protokoly aplikací** kartě. Zadejte také všechny ostatní informace, které vyžaduje každý typ dat diagnostiky. Části hello **konfigurace zdroje dat diagnostiky** dál v tomto tématu pro informace o konfiguraci na každé kartě.
10. Až poprvé povolíte shromažďování všech dat diagnostiky hello chcete, zvolte hello **OK** tlačítko.
11. Uložte projekt hello aktualizovat.
    
     Se zobrazí zpráva v hello **protokoly aktivit Microsoft Azure** okno, které hello virtuálního počítače se aktualizovala.

## <a name="configure-diagnostics-data-sources"></a>Konfigurace zdroje dat diagnostiky
Jakmile povolíte shromažďování dat diagnostiky, můžete přesně jaké zdroje dat mají toocollect a jaké informace se shromažďují. Vítejte v následujícím seznamu jsou karet v hello **konfigurace diagnostiky** dialogové okno a znamená, co jednotlivé možnosti konfigurace.

### <a name="application-logs"></a>Protokoly aplikací
**Protokoly aplikací** obsahovat diagnostické informace vyprodukované webové aplikace. Pokud chcete toocapture protokoly aplikací, vyberte hello **povolit přenos protokoly aplikací** zaškrtávací políčko. Můžete zvýšit nebo snížit hello počet minut doby přenosu protokolů aplikace hello účet úložiště tooyour změnou hello **přenosu období (min.)** hodnotu. Můžete také změnit hello množství informací zachycenou v protokolu hello nastavení hodnota úrovně hello protokolu. Například můžete **podrobné** tooget Další informace, nebo zvolte **kritický** toocapture pouze kritické chyby. Pokud máte diagnostiky specifické pro zprostředkovatele, který vysílá protokoly aplikací, můžete je zaznamenat přidáním hello zprostředkovatele GUID toohello **identifikátor GUID** pole.

  ![Protokoly aplikací](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  V tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md) Další informace o protokolech aplikací.

### <a name="windows-event-logs"></a>Protokoly událostí systému Windows
Pokud chcete toocapture protokoly událostí systému Windows, vyberte hello **povolit přenos protokoly událostí systému Windows** zaškrtávací políčko. Můžete zvýšit nebo snížit hello počet minut doby přenosu protokolů událostí hello účet úložiště tooyour změnou hello **přenosu období (min.)** hodnotu. Zaškrtněte políčka hello hello typu událostí, které chcete tootrack.

  ![Protokoly událostí](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Pokud používáte Azure SDK 2.6 nebo novější a chcete toospecify vlastní zdroj dat, zadejte ho v hello  **<Data source name>**  textové pole a zvolte hello **přidat** tooit tlačítko Další. zdroj dat Hello je přidaný toohello diagnostics.cfcfg souboru.

Pokud používáte Azure SDK 2.5 a chcete toospecify vlastní zdroj dat, můžete ho přidat toohello `WindowsEventLog` části hello diagnostics.wadcfgx souboru, například hello následující ukázka.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Čítače výkonu
Informace o čítači výkonu můžete vyhledat problémová místa systému a optimalizovat výkon systému a aplikací. V tématu [vytvořit a použít čítače výkonu v aplikaci Azure](https://msdn.microsoft.com/library/azure/hh411542.aspx) Další informace. Pokud chcete čítače výkonu toocapture, vyberte hello **povolit přenos čítače výkonu** zaškrtávací políčko. Můžete zvýšit nebo snížit hello počet minut doby přenosu protokolů událostí hello účet úložiště tooyour změnou hello **přenosu období (min.)** hodnotu. Vyberte hello zaškrtnutí políček pro hello čítače výkonu, které chcete tootrack.

  ![Čítače výkonu](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

tootrack čítače výkonu, který není uveden, zadejte ho pomocí hello navrhované syntaxe a potom zvolte hello **přidat** tlačítko. Hello operační systém na virtuálním počítači hello Určuje, které můžete sledovat čítače výkonu. Další informace o syntaxi najdete v tématu [zadání cesty čítače](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Protokoly infrastruktury
Pokud chcete toocapture infrastruktury protokoly, které obsahují informace o hello diagnostiky infrastrukturu Azure, hello RemoteAccess modulu a RemoteForwarder modulu hello, vyberte možnost hello **povolit přenos infrastruktury protokoly**zaškrtávací políčko. Můžete zvýšit nebo snížit hello počet minut doby přenosu protokolů hello tooyour účet úložiště tak, že změníte hodnotu hello přenosu období (min.).

  ![Diagnostické protokoly infrastruktury](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  V tématu [shromažďovat protokolování dat pomocí Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx) Další informace.

### <a name="log-directories"></a>Protokol adresáře
Pokud chcete toocapture protokolu adresáře, které obsahují data shromážděná z adresáře protokolu pro požadavky na Internetové informační služby (IIS), neúspěšné požadavky nebo složky, které zvolíte, vyberte hello **povolit přenos protokolu adresáře**zaškrtávací políčko. Můžete zvýšit nebo snížit hello počet minut doby přenosu protokolů hello účet úložiště tooyour změnou hello **přenosu období (min.)** hodnotu.

Můžete vybrat pole hello hello protokolů chcete toocollect, například **protokoly služby IIS** a **se nezdařilo požadavku** protokoly. Výchozí názvy kontejnerů úložiště jsou k dispozici, ale pokud chcete, můžete změnit názvy hello.

Navíc můžete zaznamenat protokoly z libovolné složky. Stačí zadat cestu hello v hello **protokolu z adresáře absolutní** tématu a potom zvolte hello **přidat adresář** tlačítko. Hello protokoly bude možné zaznamenat toohello zadaný kontejnery.

  ![Protokol adresáře](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>Protokoly trasování událostí pro Windows
Pokud používáte [trasování událostí pro Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803\(v=vs.85\).aspx) (ETW) a chcete toocapture protokoly trasování událostí pro Windows, vyberte hello **povolit přenos protokolů trasování událostí pro Windows** zaškrtávací políčko. Můžete zvýšit nebo snížit hello počet minut doby přenosu protokolů hello účet úložiště tooyour změnou hello **přenosu období (min.)** hodnotu.

zaznamenání Hello události ze zdroje událostí a manifestů událostí, které zadáte. toospecify zdroje událostí, zadejte název v hello **zdroje událostí** tématu a potom zvolte hello **přidat zdroj události** tlačítko. Podobně můžete zadat manifest událostí v hello **událostí manifesty** tématu a potom zvolte hello **přidat Manifest událostí** tlačítko.

  ![Protokoly trasování událostí pro Windows](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  framework trasování událostí pro Windows Hello je podporované v technologii ASP.NET pomocí třídy v hello [System.Diagnostics.aspx] (obor názvů https://msdn.microsoft.com/library/system.diagnostics (v=vs.110). Hello Microsoft.WindowsAzure.Diagnostics názvů, který dědí z a rozšiřuje standard [System.Diagnostics.aspx] (třídy https://msdn.microsoft.com/library/system.diagnostics (v=vs.110), umožňuje použití hello ([System.Diagnostics.aspx] https://msdn.microsoft.com/library/System.Diagnostics (v=vs.110) jako rozhraní protokolování v hello prostředí Azure. Další informace najdete v tématu [trvat ovládací prvek z protokolování a trasování v Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) a [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Výpisy stavu systému
Pokud chcete toocapture informace o když dojde k chybě instanci role, vyberte hello **povolit přenos havárií výpisy** zaškrtávací políčko. (Protože ASP.NET zpracovává většina výjimky, to je obecně užitečné pouze pro role pracovního procesu.) Můžete zvýšit nebo snížit hello podíl úložiště místo odeberte výpisy stavu systému toohello změnou hello **Directory kvóty (%)** hodnotu. Můžete změnit hello kontejneru úložiště, kde jsou uloženy výpisy stavu systému hello a můžete určit, zda chcete toocapture **úplné** nebo **malé** výpis.

procesy Hello aktuálně sledované jsou uvedené. Zaškrtněte políčka hello hello procesy, které chcete toocapture. tooadd jiného procesu toohello seznamu, zadejte název procesu hello a zvolte hello **přidat proces** tlačítko.

  ![Výpisy stavu systému](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  V tématu [trvat ovládací prvek z protokolování a trasování v Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) a [Microsoft Azure Diagnostics část 4: vlastní protokolování součásti a Azure Diagnostics 1.3 změny](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) Další informace.

## <a name="view-hello-diagnostics-data"></a>Zobrazení dat diagnostiky hello
Poté, co jste shromažďovaných dat diagnostiky hello cloudové služby nebo virtuálního počítače, můžete ji zobrazit.

### <a name="tooview-cloud-service-diagnostics-data"></a>tooview cloudové služby diagnostická data
1. Cloudové služby jako obvyklé nasadit a potom ho spusťte.
2. Hello diagnostická data můžete zobrazit sestavu, která generuje Visual Studio nebo tabulek ve vašem účtu úložiště. Otevřete tooview hello data v sestavě, **Průzkumník cloudu** nebo **Průzkumníka serveru**, otevřete hello místní nabídce uzlu hello hello role, které vás zajímají a potom zvolte **zobrazení diagnostických dat** .
   
    ![Zobrazení dat diagnostiky](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)
   
    Zobrazí se sestavu, která zobrazuje data k dispozici hello.
   
    ![Microsoft Azure Diagnostics sestavy v sadě Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)
   
    Pokud se nezobrazí hello nejnovější data, můžete mít toowait pro období tooelapse hello přenosu.
   
    Zvolte hello **aktualizovat** propojení tooimmediately aktualizace hello dat, nebo zvolte intervalu v hello **automatického obnovení** rozevírací seznam pole toohave hello dat automaticky aktualizován. tooexport hello Chyba dat, zvolte hello **exportovat tooCSV** tlačítko toocreate soubor hodnot oddělených čárkami, můžete otevřít v tabulce.
   
    V **Průzkumník cloudu** nebo **Průzkumníka serveru**, otevřete hello účet úložiště, který je spojen s hello nasazení.
3. Otevřete hello diagnostiky tabulky v prohlížeči hello tabulky a poté zkontrolovat shromážděných dat hello. Pro vlastní protokoly a protokoly služby IIS můžete otevřít kontejner objektů blob. Kontrolou hello následující tabulky můžete najít kontejner hello tabulky nebo objekt blob, který obsahuje hello data, která vás zajímá. Kromě toho toohello data pro tento soubor protokolu, položky tabulky hello obsahovat EventTickCount, DeploymentId, Role a RoleInstance toohelp je určit, jaké virtuální počítač a role generované hello data a kdy. 
   
   | Diagnostických dat | Popis | Umístění |
   | --- | --- | --- |
   | Protokoly aplikací |Protokoly, které generuje kód voláním metod hello System.Diagnostics.Trace – třída |WADLogsTable |
   | Protokoly událostí |Tato data jsou z protokolů událostí systému Windows hello hello virtuálními počítači. Systém Windows ukládá informace v těchto protokolech, ale aplikace a služby také používat tooreport chyby nebo protokolování informací. |WADWindowsEventLogsTable |
   | Čítače výkonu |Můžete shromáždit data pro všechny čítače výkonu, který je k dispozici na virtuálním počítači hello. Hello operačního systému poskytuje čítače výkonu, které zahrnují mnoho statistické údaje, třeba čas procesoru a využití paměti. |WADPerformanceCountersTable |
   | Protokoly infrastruktury |Tyto protokoly jsou generovány z infrastruktury hello diagnostiky, sám sebe. |WADDiagnosticInfrastructureLogsTable |
   | Protokoly služby IIS |Tyto protokoly záznam webových požadavků. Cloudové služby získá významný objem provozu, tyto protokoly mohou být velmi náročná, tak, aby měli shromažďovat a uložení dat této pouze v případě potřeby ho. |Můžete najít, že se nezdařilo žádost o protokolech v kontejneru objektů blob hello v rámci služby iis failedreqlogs wad v cestě pro tohoto nasazení, role a instance. Můžete najít v části wad. Služba iis logfiles dokončení protokoly. V tabulce WADDirectories hello jsou vytvořeny položky pro každý soubor. |
   | Výpisy stavu systému |Tyto informace obsahuje binární bitové kopie Cloudová služba proces (obvykle role pracovního procesu). |kontejner objektů blob wad. deformační výpisy |
   | Vlastní protokol soubory |Protokoluje data, která jste předdefinované. |Zadávat lze v umístění hello kód vlastní soubory protokolu ve vašem účtu úložiště. Můžete například zadat kontejner objektů blob vlastní. |
4. Pokud se zkrátí dat jakéhokoli typu, můžete zkusit roste hello vyrovnávací paměti pro tento datový typ nebo zkrácením hello interval mezi přenosy dat z účtu úložiště tooyour hello virtuálního počítače.
5. (volitelné) Mazání dat z úložiště hello účtu příležitostně tooreduce celkové náklady na úložiště.
6. Po provedení úplné nasazení hello diagnostics.cscfg soubor (.wadcfgx pro Azure SDK 2.5) je aktualizovat v Azure a cloudové služby převezme konfigurace diagnostiky tooyour žádné změny. Pokud, místo toho aktualizaci stávajícího nasazení, není soubor .cscfg hello aktualizovat v Azure. Můžete je stále změnit nastavení diagnostiky, ale postupem hello v další části hello. Další informace o provádění úplné nasazení a aktualizaci stávajícího nasazení najdete v tématu [Průvodci publikováním aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="tooview-virtual-machine-diagnostics-data"></a>data diagnostiky tooview virtuálního počítače
1. V místní nabídce hello hello virtuálního počítače, zvolte **zobrazení diagnostická Data**.
   
    ![Zobrazení dat diagnostiky ve virtuálním počítači Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)
   
    Tím se otevře hello **diagnostiky Souhrn** okno.
   
    ![Virtuální počítač Azure diagnostics souhrn](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  
   
    Pokud se nezobrazí hello nejnovější data, můžete mít toowait pro období tooelapse hello přenosu.
   
    Zvolte hello **aktualizovat** propojení tooimmediately aktualizace hello dat, nebo zvolte intervalu v hello **automatického obnovení** rozevírací seznam pole toohave hello dat automaticky aktualizován. tooexport hello Chyba dat, zvolte hello **exportovat tooCSV** tlačítko toocreate soubor hodnot oddělených čárkami, můžete otevřít v tabulce.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Po nasazení nakonfigurovat diagnostiku cloudové služby
Pokud jste příčin problém s cloudovou službou, že již byla spuštěna, můžete chtít toocollect dat, které jste neurčili před původně nasazené hello role. V takovém případě může spuštění toocollect dat pomocí nastavení hello v Průzkumníku serveru. Můžete nakonfigurovat diagnostiku pro jednu instanci nebo všechny instance hello v roli, v závislosti na tom, jestli můžete otevřít dialogové okno Konfigurace diagnostiky hello z místní nabídky hello hello instance nebo hello role. Pokud nakonfigurujete hello role uzlu, změny se použijí tooall instance. Pokud nakonfigurujete hello instance uzlu, změny se použijí jenom toothat instance.

### <a name="tooconfigure-diagnostics-for-a-running-cloud-service"></a>tooconfigure diagnostiku pro spuštěné cloudové služby
1. V Průzkumníku serveru rozbalte hello **cloudové služby** uzel a poté rozbalte uzly toolocate hello role nebo instance, který má tooinvestigate nebo obojí.
   
    ![Konfiguraci diagnostiky](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)
2. V nabídce hello zástupce pro uzel instanci nebo uzel role zvolte **nastavení diagnostiky aktualizace**a potom zvolte hello nastavení diagnostiky, které chcete toocollect.
   
    Informace o nastavení konfigurace hello najdete v tématu **konfigurace zdroje dat diagnostiky** v tomto tématu. Informace o tom, jak tooview hello diagnostická data najdete v tématu **zobrazit data diagnostiky hello** v tomto tématu.
   
    Pokud změníte shromažďování dat v **Průzkumníka serveru**, tyto změny zůstávají v platnosti, dokud nebude plně znovu nasadit cloudové služby. Pokud používáte hello výchozí nastavení publikování, změny hello nejsou přepsána, protože publikování hello výchozí nastavení je tooupdate hello stávajícího nasazení, nikoli proveďte úplné opětovné nasazení. zda hello nastavení toomake vymazat v době nasazení, přejděte toohello **Upřesnit nastavení** kartě v Průvodci publikovat hello a vymazat hello **aktualizaci nasazení** zaškrtávací políčko. Při opětovném s toto zaškrtávací políčko zaškrtnuto, nastavení hello vrátit toothose v souboru hello .wadcfgx (nebo .wadcfg) jako sada prostřednictvím editoru hello vlastnosti pro roli hello. Pokud aktualizujete vaše nasazení, Azure udržuje staré nastavení hello.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Řešení problémů služby Azure cloud
Pokud máte problémy s vaše projekty cloudových služeb, jako je například role, kterou vázne "zaneprázdněn" stav, opakovaně recykluje nebo vyvolá o vnitřní chybu serveru, jsou nástroje a techniky, které můžete použít toodiagnose a tyto problémy opravit. Konkrétní příklady běžné problémy a řešení, a také přehled hello koncepty a nástrojů používá toodiagnose a opravte tyto chyby, najdete v části [Azure PaaS výpočetní diagnostická Data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Dotazy a odpovědi
**Jaká je velikost vyrovnávací paměti hello a jak velké by to být?**

Na každou instanci virtuálního počítače kvóty omezit, kolik diagnostických dat může být uložený v hello místního systému souborů. Kromě toho můžete zadat velikost vyrovnávací paměti pro každý typ diagnostických dat, která je k dispozici. Tato velikost vyrovnávací paměti se chová jako individuální kvótu pro daný typ dat. Kontrolou hello dolní části dialogového hello můžete určit hello celkové kvóty a hello množství paměti, která zůstává. Pokud zadáte větší vyrovnávací paměti nebo další typy dat, budete postupovat hello celkové kvóty. Můžete změnit hello celkové kvóty změnou hello diagnostics.wadcfg/.wadcfgx konfigurační soubor. data se ukládají na diagnostiky Hello hello stejný systém souborů jako data aplikace, pokud vaše aplikace používá velké množství místa na disku, by neměl zvýšit hello celkové kvóty diagnostiky.

**Co je hello přenos období a jak dlouho má se?**

doba přenosu Hello je hello množství času, který uplyne mezi dat zaznamená. Po každé období přenos dat se přesune ze hello místního systému souborů na virtuální počítač tootables ve vašem účtu úložiště. Pokud hello množství dat, které jsou shromážděny překročí kvótu hello hello konce dobu přenosu, budou zahozeny starší data. Pokud jste ke ztrátě dat, protože data je větší než velikost vyrovnávací paměti hello nebo hello celkové kvóty můžete toodecrease hello přenos období.

**Jaké časové pásmo jsou hello časová razítka v?**

časová razítka Hello jsou v místním časovém pásmu hello hello datového centra, který je hostitelem cloudové služby. Hello následující tři sloupce časového razítka v tabulkách protokolu hello se používají.

* **PreciseTimeStamp** je časové razítko trasování událostí pro Windows hello hello události. To znamená hello čas hello událost je protokolována z klienta hello.
* **Časové razítko** se PreciseTimeStamp zaokrouhlí směrem dolů toohello nahrávání frekvence hranic. Ano Pokud vaše nahrávání četnost je 5 minut a událostí hello čas 00:17:12, časové razítko bude 00:15:00.
* **Časové razítko** je hello časové razítko, na které hello byla vytvořena entita v hello tabulky Azure.

**Jak lze spravovat náklady, při shromažďování diagnostických informací?**

Hello výchozí nastavení (**úrovně protokolování** nastavit příliš**chyba** a **přenos období** nastavit příliš**1 minuta**) jsou navržené toominimize náklady. Náklady na výpočetní zvýší, pokud shromažďování více diagnostických dat nebo snížit dobu přenosu hello. Nemáte shromažďovat více dat, než je třeba a nezapomeňte toodisable shromažďování dat, pokud ji už nepotřebujete. Můžete vždy ho znovu povolit, i v době běhu, jak je uvedeno v předchozí části hello.

**Jak můžu shromáždit protokoly se nezdařilo žádost ze služby IIS?**

Ve výchozím nastavení služba IIS není shromažďovat protokoly žádosti se nezdařilo. Můžete nakonfigurovat službu IIS toocollect je při úpravě souboru web.config pro vaši webovou roli hello.

**Nezobrazují se informace o trasování z metod RoleEntryPoint jako OnStart. Co je?**

Hello metody RoleEntryPoint se nazývají v kontextu hello WAIISHost.exe, ne služba IIS. Informace o konfiguraci v souboru web.config, který obvykle umožňuje trasování netýká proto hello. tooresolve tento problém, přidejte projekt .config souboru tooyour webové role a název hello souboru toomatch hello výstup sestavení, které obsahuje kód RoleEntryPoint hello. V projektu role hello výchozí web bude hello název souboru .config hello WAIISHost.exe.config. Pak přidejte následující řádky toothis soubor hello:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Nyní v hello **vlastnosti** okno, sada hello **zkopírujte tooOutput Directory** vlastnost příliš**vždy Kopírovat**.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o diagnostiku protokolování v Azure, najdete v části [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services/cloud-services-dotnet-diagnostics.md) a [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md).

