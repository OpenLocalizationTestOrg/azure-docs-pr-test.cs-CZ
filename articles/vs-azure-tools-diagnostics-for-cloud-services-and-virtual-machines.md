---
title: "Konfigurace diagnostiky pro cloudové služby Azure a virtuální počítače | Microsoft Docs"
description: "Popisuje postup konfigurace diagnostické informace pro ladění Azure cloude služeb a virtuálních počítačů (VM) v sadě Visual Studio."
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
ms.openlocfilehash: 2516c0eb8ce470577731db9b844d5b9038465477
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Konfiguraci diagnostiky pro cloudové služby Azure a virtuální počítače
Pokud potřebujete řešení služby Azure cloudové služby nebo virtuálního počítače Azure, můžete nakonfigurovat Azure diagnostics snadněji pomocí sady Visual Studio. Azure diagnostics zaznamená data systému a protokolování dat na virtuálních počítačů a instancí virtuálního počítače se systémem cloudové služby a přenosy dat do účtu úložiště podle vašeho výběru. V tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md) Další informace o protokolování v Azure diagnostics.

Toto téma ukazuje, jak povolit a nakonfigurovat Azure diagnostics v sadě Visual Studio, před i po nasazení, a také na virtuálních počítačích Azure. Také vám ukazuje postup vyberte typy shromažďovat diagnostické informace a postup zobrazení informace po shromáždění zpracovat.

Azure Diagnostics můžete nakonfigurovat následujícím způsobem:

* Můžete změnit nastavení konfigurace diagnostiky prostřednictvím **konfigurace diagnostiky** dialogové okno v sadě Visual Studio. Nastavení se ukládají v souboru s názvem diagnostics.wadcfgx (diagnostics.wadcfg v Azure SDK 2.4 nebo starší). Alternativně můžete přímo upravit konfigurační soubor. Pokud soubor ručně aktualizujete, změny konfigurace se projeví na další čas nasazení cloudu služby Azure nebo službu spustit v emulátoru.
* Použití **Průzkumník cloudu** nebo **Průzkumníka serveru** v sadě Visual Studio, chcete-li změnit nastavení diagnostiky pro spuštěné cloudové služby nebo virtuálního počítače.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnostics změny
Pro Azure SDK 2.6 projekty v sadě Visual Studio byly provedeny následující změny. (Tyto změny také použita na novější verzi sady Azure SDK.)

* Místní emulátoru nyní podporuje diagnostiku. To znamená, že můžete shromažďovat diagnostická data a ujistěte se, že aplikace je vytváření správné trasování, když jste vývoj a testování v sadě Visual Studio. Připojovací řetězec `UseDevelopmentStorage=true` umožňuje shromažďování dat diagnostiky, když spouštíte projekt cloudové služby v sadě Visual Studio pomocí emulátoru úložiště Azure. Všechny diagnostická data se shromažďují v účtu úložiště (vývoj pro úložiště).
* Řetězec připojení účtu úložiště diagnostiky (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) je znovu uložené v souboru (.csdef) služby. V souboru diagnostics.wadcfgx bylo zadáno v Azure SDK 2.5 účet úložiště diagnostiky.

Existují některé významné rozdíly mezi jak šlo připojovací řetězec v Azure SDK 2.4 a starší a jak to funguje v Azure SDK 2.6 nebo novější.

* Azure SDK 2.4 a starší připojovací řetězec jako byl použit modulu runtime modulem plug-in diagnostiky získat informace o účtu úložiště pro přenos diagnostické protokoly.
* V Azure SDK 2.6 nebo novější diagnostiky připojovací řetězec se používá Visual Studio ke konfiguraci rozšíření diagnostiky s informace o účtu příslušné úložiště během publikování. Připojovací řetězec umožňuje definovat jiným účtům úložiště pro různé služby konfigurace, které Visual Studio budou používat při publikování. Ale vzhledem k tomu, že modul plug-in diagnostiky již není k dispozici (po Azure SDK 2.5), soubor .cscfg samostatně nelze povolit rozšíření diagnostiky. Je nutné povolit rozšíření, samostatně prostřednictvím nástrojů, jako je Visual Studio nebo prostředí PowerShell.
* Ke zjednodušení procesu konfigurace rozšíření diagnostiky pomocí prostředí PowerShell, balíček výstup ze sady Visual Studio také obsahuje veřejné konfigurace XML pro rozšíření diagnostiky pro každou roli. Visual Studio použije diagnostiky připojovací řetězec k naplnění ve veřejné konfigurace informace o účtu úložiště. Veřejné konfigurační soubory jsou vytvořeny ve složce rozšíření a postupujte podle vzoru PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. Všechna nasazení na základě prostředí PowerShell můžete použít tento vzor pro mapování každé konfiguraci k roli.
* Připojovací řetězec v souboru .cscfg je také používán [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) k přístupu k datům diagnostiky, může se objevit v **monitorování** kartě. Připojovací řetězec je potřeba ke konfiguraci službu, kterou chcete zobrazit podrobné údaje z monitorování na portálu.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migrace projekty k Azure SDK 2.6 a novějším
Při migraci z Azure SDK 2.5 Azure SDK 2.6 nebo novější, pokud jste měli účet úložiště diagnostiky, který je zadaný v souboru .wadcfgx, pak zůstane existuje. Abyste mohli využívat flexibilitu používání jiným účtům úložiště pro jiné úložiště, budete muset ručně přidat připojovací řetězec do projektu. Pokud projekt jste migraci z Azure SDK 2.4 nebo starší na Azure SDK 2.6, se zachovají diagnostiky připojovací řetězce. Upozorňujeme ale, změny v tom, jak se připojovací řetězce považují ve verzi 2.6 SDK Azure uvedeného v předchozí části.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Určuje, jak Visual Studio účet úložiště diagnostiky
* Pokud diagnostiky připojovací řetězec je zadané v souboru .cscfg, Visual Studio se používá ke konfiguraci rozšíření diagnostiky při publikování a při generování souborů xml veřejné konfigurace během balení.
* Pokud žádný diagnostiky připojovací řetězec je zadané v souboru .cscfg, pak Visual Studio se vrátí k používání účtu úložiště, zadaný v souboru .wadcfgx ke konfiguraci rozšíření diagnostiky při publikování a generování veřejného konfigurační soubory xml při balení.
* Diagnostika připojovací řetězec v souboru .cscfg má přednost před účet úložiště v souboru .wadcfgx. Pokud diagnostiky připojovací řetězec je zadané v souboru .cscfg, Visual Studio použije tento a ignoruje .wadcfgx účet úložiště.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Jaké jsou "aktualizace vývoj úložiště připojovací řetězce..." zaškrtávací políčko dělat?
Zaškrtávací políčko pro **aktualizace vývoj úložiště připojovací řetězce pro diagnostiku a ukládání do mezipaměti pomocí přihlašovacích údajů účtu úložiště Microsoft Azure při publikování do služby Microsoft Azure** nabízí pohodlný způsob aktualizace jakékoli vývoj úložiště připojovací řetězce k účtu pomocí účtu úložiště Azure během publikování zadány.

Předpokládejme například, zaškrtněte toto políčko a diagnostiky připojovací řetězec Určuje `UseDevelopmentStorage=true`. Při publikování tohoto projektu v Azure, Visual Studio automaticky aktualizovat připojovací řetězec diagnostiky s účtem úložiště, který jste zadali v Průvodci publikovat. Pokud účet úložiště skutečné byl zadán jako připojovací řetězec diagnostiky, pak tento účet se ale používá místo.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostické funkce rozdíly mezi Azure SDK 2.4 a starší a Azure SDK, 2.5 nebo novější
Pokud upgradujete projekt z Azure SDK 2.4 na Azure SDK 2.5 nebo novější, jste měli mít na paměti následující rozdíly funkce diagnostiky.

* **Rozhraní API pro konfiguraci jsou zastaralé** – Programová konfigurace diagnostiky je k dispozici v Azure SDK 2.4 nebo starší verze, ale je zastaralý v Azure SDK 2.5 nebo novější. Pokud vaše konfigurace diagnostiky je aktuálně definována v kódu, budete muset znovu nakonfigurujte tato nastavení ručně v migrovaných projektu, pro diagnostiku chcete pokračovat v práci. Diagnostika konfiguračního souboru pro Azure SDK 2.4 je diagnostics.wadcfg a diagnostics.wadcfgx pro Azure SDK 2.5 nebo novější.
* **Diagnostika pro cloudové aplikace, služby se dá nakonfigurovat jenom na úrovni role, ne na úrovni instance.**
* **Pokaždé, když nasadíte aplikaci, konfiguraci diagnostiky se aktualizuje** – to může způsobit problémy parita, pokud změníte konfiguraci diagnostiky z Průzkumníka serveru a pak znovu nasadit aplikace.
* **V Azure SDK 2.5 nebo novější, výpisy stavu systému jsou nakonfigurované v konfiguračním souboru diagnostics není v kódu** – Pokud máte nakonfigurované v kódu výpisy stavu systému, budete muset ručně přenos konfigurace z kódu do konfiguračního souboru, protože výpisy stavu systému nejsou přenést během migrace na Azure SDK 2.6.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Povolte diagnostiku v projekty cloudových služeb před jejich nasazením
V sadě Visual Studio můžete shromažďovat diagnostická data pro role, které běží v Azure, když spustíte službu v emulátoru ještě před nasazením. Všechny změny nastavení diagnostiky v sadě Visual Studio jsou uloženy v konfiguračním souboru diagnostics.wadcfgx. Tato nastavení konfigurace zadejte účet úložiště, uložení dat diagnostiky při nasazení cloudové služby.

[!INCLUDE [cloud-services-wad-warning](../includes/cloud-services-wad-warning.md)]

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Povolí se Diagnostika v sadě Visual Studio před nasazením
1. V místní nabídce pro roli, která vás zajímá, zvolte **vlastnosti**a potom zvolte **konfigurace** ve role **vlastnosti** okno.
2. V **diagnostiky** část, ujistěte se, že **povolení diagnostiky** je zaškrtnuté políčko.
   
    ![Přístup k možnost povolení diagnostiky](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)
3. Zvolte tlačítko se třemi tečkami (...) zadejte účet úložiště, kam chcete data diagnostiky k uložení. Účet úložiště, který zvolíte, bude místo, kam se ukládají diagnostická data.
   
    ![Zadejte účet úložiště, který chcete použít](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)
4. V **vytvoření připojovacího řetězce úložiště** dialogové okno pole, určete, zda se chcete připojit pomocí emulátoru úložiště Azure, předplatné Azure, nebo ručně zadali přihlašovací údaje.
   
    ![Dialogové okno účtu úložiště](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)
   
   * Pokud se rozhodnete Microsoft Azure emulátor úložiště možnost, připojovací řetězec je nastavena na UseDevelopmentStorage = true.
   * Pokud se rozhodnete vaše předplatné možnost, můžete zvolit předplatné Azure, kterou chcete použít a název účtu. Můžete spravovat účty pro tlačítko ke správě vašich předplatných Azure.
   * Pokud zvolíte možnost ručně zadat přihlašovací údaje, se zobrazí výzva k zadání názvu a klíče účtu Azure, kterou chcete použít.
5. Vyberte **konfigurace** tlačítko Zobrazit **konfigurace diagnostiky** dialogové okno. Každé kartě (s výjimkou **Obecné** a **adresáře protokolu**) představuje zdroj diagnostických dat, který můžete shromáždit. Na kartě Výchozí **Obecné**, nabízí následující možnosti shromažďování dat diagnostiky: **pouze chyby**, **všechny informace o**, a **vlastní plán**. Výchozí možnost **pouze chyby**, využívá nejmenší velikost úložiště, protože nepřenese upozornění nebo trasování zpráv. Všechny informace možnost přenáší většinu informací a je proto nejnákladnější možnost z hlediska úložiště.
   
    ![Povolení a konfigurace Azure Diagnostika](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
6. V tomto příkladu vyberte **vlastní plán** shromážděných možnost tak data můžete přizpůsobit.
7. **Kvóty disku v MB** pole určuje, kolik místa, které chcete přidělit ve vašem účtu úložiště pro data diagnostiky. Pokud chcete, můžete změnit výchozí hodnotu.
8. Na každé kartě chcete shromažďovat diagnostická data, vyberte jeho **povolit přenos z <log type>**  zaškrtávací políčko. Například pokud chcete shromažďovat protokoly aplikací, vyberte **povolit přenos protokoly aplikací** v zaškrtávací políčko **protokoly aplikací** kartě. Zadejte také všechny ostatní informace, které vyžaduje každý typ dat diagnostiky. Najdete v části **konfigurace zdroje dat diagnostiky** dál v tomto tématu pro informace o konfiguraci na každé kartě.
9. Až poprvé povolíte shromažďování dat diagnostiky chcete, vyberte **OK** tlačítko.
10. Spusťte projekt Azure cloud service v sadě Visual Studio jako obvykle. Při používání aplikace informace protokolu, které jste povolili se uloží do účtu úložiště Azure, které zadáte.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Povolte diagnostiku ve virtuálních počítačích Azure
V sadě Visual Studio můžete ke shromažďování dat diagnostiky pro virtuální počítače Azure.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Povolí se Diagnostika na virtuálních počítačích Azure
1. V **Průzkumníka serveru**, zvolte uzlu Azure a potom se připojte k předplatnému Azure, pokud jste již připojeni.
2. Rozbalte **virtuální počítače** uzlu. Můžete vytvořit nový virtuální počítač nebo vyberte ten, který již existuje.
3. V místní nabídce virtuálního počítače, které vás zajímají, zvolte **konfigurace**. Zobrazí dialogové okno Konfigurace virtuálního počítače.
   
    ![Konfigurace virtuálního počítače Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)
4. Pokud ještě není nainstalovaná, přidejte rozšíření diagnostiky agenta monitorování společnosti Microsoft. Toto rozšíření umožňuje shromažďování dat diagnostiky pro virtuální počítač Azure. V seznamu nainstalovaná rozšíření zvolte Select rozšíření k dispozici rozevírací nabídky a pak zvolte Microsoft Monitoring Agent Diagnostics.
   
    ![Instalace rozšíření virtuálního počítače Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)
   
   > [!NOTE]
   > Další rozšíření diagnostiky jsou k dispozici pro virtuální počítače. Další informace najdete v tématu rozšíření virtuálního počítače Azure a funkce.
   > 
   > 
5. Vyberte **přidat** tlačítko Přidat rozšíření a zobrazit jeho **konfigurace diagnostiky** dialogové okno.
6. Vyberte **konfigurace** tlačítko a zadejte účet úložiště a pak zvolte **OK** tlačítko.
   
    Každé kartě (s výjimkou **Obecné** a **adresáře protokolu**) představuje zdroj diagnostických dat, který můžete shromáždit.
   
    ![Povolení a konfigurace Azure Diagnostika](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
   
    Na kartě Výchozí **Obecné**, nabízí následující možnosti shromažďování dat diagnostiky: **pouze chyby**, **všechny informace o**, a **vlastní plán**. Výchozí možnost **pouze chyby**, využívá nejmenší velikost úložiště, protože nepřenese upozornění nebo trasování zpráv. **Všechny informace o** možnost přenáší informace na maximum a je proto nejnákladnější možnost z hlediska úložiště.
7. V tomto příkladu vyberte **vlastní plán** shromážděných možnost tak data můžete přizpůsobit.
8. **Kvóty disku v MB** pole určuje, kolik místa, které chcete přidělit ve vašem účtu úložiště pro data diagnostiky. Pokud chcete, můžete změnit výchozí hodnotu.
9. Na každé kartě chcete shromažďovat diagnostická data, vyberte jeho **povolit přenos z <log type>**  zaškrtávací políčko.
   
    Například pokud chcete shromažďovat protokoly aplikací, vyberte **povolit přenos protokoly aplikací** v zaškrtávací políčko **protokoly aplikací** kartě. Zadejte také všechny ostatní informace, které vyžaduje každý typ dat diagnostiky. Najdete v části **konfigurace zdroje dat diagnostiky** dál v tomto tématu pro informace o konfiguraci na každé kartě.
10. Až poprvé povolíte shromažďování dat diagnostiky chcete, vyberte **OK** tlačítko.
11. Uložte aktualizovaný projekt.
    
     Se zobrazí zpráva v **protokoly aktivit Microsoft Azure** okno aktualizovala virtuálního počítače.

## <a name="configure-diagnostics-data-sources"></a>Konfigurace zdroje dat diagnostiky
Jakmile povolíte shromažďování dat diagnostiky, můžete zvolit přesně jaké zdroje dat, které chcete shromažďovat a shromažďovaných informací. Následuje seznam karty v **konfigurace diagnostiky** dialogové okno a znamená, co jednotlivé možnosti konfigurace.

### <a name="application-logs"></a>Protokoly aplikací
**Protokoly aplikací** obsahovat diagnostické informace vyprodukované webové aplikace. Pokud chcete zaznamenat protokoly aplikací, vyberte **povolit přenos protokoly aplikací** zaškrtávací políčko. Můžete zvýšit nebo snížit počet minut doby přenosu v protokolech aplikací k vašemu účtu úložiště tak, že změníte **přenosu období (min.)** hodnotu. Můžete také změnit množství informací o nastavení hodnoty úroveň protokolu zachycenou v protokolu. Například můžete **podrobné** získat další informace nebo zvolte **kritický** k zachycení pouze kritické chyby. Pokud máte diagnostiky specifické pro zprostředkovatele, který vysílá protokoly aplikací, můžete je zaznamenat přidáním GUID poskytovatele do **identifikátor GUID** pole.

  ![Protokoly aplikací](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  V tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md) Další informace o protokolech aplikací.

### <a name="windows-event-logs"></a>Protokoly událostí systému Windows
Pokud chcete zaznamenat protokol událostí systému Windows, vyberte **povolit přenos protokoly událostí systému Windows** zaškrtávací políčko. Můžete zvýšit nebo snížit počet minut doby přenosu protokolů událostí na váš účet úložiště tak, že změníte **přenosu období (min.)** hodnotu. Zaškrtněte políčka pro typy událostí, které chcete sledovat.

  ![Protokoly událostí](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Pokud používáte Azure SDK 2.6 nebo novější a chcete zadat vlastní zdroj dat, zadejte ho v  **<Data source name>**  textové pole a zvolte **přidat** vedle sebe tlačítko. Zdroj dat je přidaný do souboru diagnostics.cfcfg.

Pokud používáte Azure SDK 2.5 a chcete zadat vlastní zdroj dat, můžete přidat jej do `WindowsEventLog` části diagnostics.wadcfgx souboru, například jako v následujícím příkladu.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Čítače výkonu
Informace o čítači výkonu můžete vyhledat problémová místa systému a optimalizovat výkon systému a aplikací. V tématu [vytvořit a použít čítače výkonu v aplikaci Azure](https://msdn.microsoft.com/library/azure/hh411542.aspx) Další informace. Pokud chcete zaznamenat čítače výkonu, vyberte **povolit přenos čítače výkonu** zaškrtávací políčko. Můžete zvýšit nebo snížit počet minut doby přenosu protokolů událostí na váš účet úložiště tak, že změníte **přenosu období (min.)** hodnotu. Zaškrtněte políčka pro čítače výkonu, které chcete sledovat.

  ![Čítače výkonu](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Chcete-li sledovat čítače výkonu, který není uveden, zadejte ho pomocí navrhované syntaxe a poté zvolte **přidat** tlačítko. Operační systém na virtuálním počítači určuje, které můžete sledovat čítače výkonu. Další informace o syntaxi najdete v tématu [zadání cesty čítače](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Protokoly infrastruktury
Pokud chcete zaznamenat infrastruktury protokoly, které obsahují informace o infrastrukturu Azure diagnostiky, vyberte modul RemoteAccess a RemoteForwarder modul, **povolit přenos infrastruktury protokoly** zaškrtávací políčko. Můžete zvýšit nebo snížit počet minut doby přenosu protokolů k vašemu účtu úložiště změnou hodnoty období přenosu (min.).

  ![Diagnostické protokoly infrastruktury](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  V tématu [shromažďovat protokolování dat pomocí Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx) Další informace.

### <a name="log-directories"></a>Protokol adresáře
Pokud chcete zaznamenat adresáře protokolu, které obsahují data shromážděná z adresáře protokolu pro požadavky na Internetové informační služby (IIS), neúspěšné požadavky nebo složky, které zvolíte, vyberte **povolit přenos protokolu adresáře** zaškrtávací políčko. Můžete zvýšit nebo snížit počet minut doby přenosu protokolů k vašemu účtu úložiště tak, že změníte **přenosu období (min.)** hodnotu.

Můžete vybrat pole protokolů, které chcete shromažďovat, jako například **protokoly služby IIS** a **se nezdařilo požadavku** protokoly. Názvy kontejnerů výchozí úložiště jsou k dispozici, ale pokud chcete, můžete změnit názvy.

Navíc můžete zaznamenat protokoly z libovolné složky. Stačí zadat cestu ve **protokolu z adresáře absolutní** tématu a potom zvolte **přidat adresář** tlačítko. Protokoly se zaznamená do zadaného kontejneru.

  ![Protokol adresáře](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>Protokoly trasování událostí pro Windows
Pokud používáte [trasování událostí pro Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803\(v=vs.85\).aspx) (ETW) a chcete zaznamenat protokoly trasování událostí pro Windows, vyberte **povolit přenos protokolů trasování událostí pro Windows** zaškrtávací políčko. Můžete zvýšit nebo snížit počet minut doby přenosu protokolů k vašemu účtu úložiště tak, že změníte **přenosu období (min.)** hodnotu.

Zaznamenání události ze zdroje událostí a manifestů událostí, které zadáte. Zdroje událostí, zadejte název do **zdroje událostí** tématu a potom zvolte **přidat zdroj události** tlačítko. Podobně můžete zadat manifest událostí v **událostí manifesty** tématu a potom zvolte **přidat Manifest událostí** tlačítko.

  ![Protokoly trasování událostí pro Windows](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  Rozhraní framework trasování událostí pro Windows je podporována v technologii ASP.NET pomocí třídy v [System.Diagnostics.aspx] (obor názvů https://msdn.microsoft.com/library/system.diagnostics (v=vs.110). Obor názvů Microsoft.WindowsAzure.Diagnostics, který dědí z a rozšiřuje standard [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) třídy, umožňuje použití [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) jako rozhraní protokolování v prostředí Azure. Další informace najdete v tématu [trvat ovládací prvek z protokolování a trasování v Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) a [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Výpisy stavu systému
Pokud chcete k zaznamenání informací o při instanci role dojde k chybě, vyberte **povolit přenos havárií výpisy** zaškrtávací políčko. (Protože ASP.NET zpracovává většina výjimky, to je obecně užitečné pouze pro role pracovního procesu.) Můžete zvýšit nebo snížit podíl úložiště věnované výpisy stavu systému změnou **Directory kvóty (%)** hodnotu. Kontejner úložiště, kde jsou uloženy výpisy stavu systému, a můžete určit, zda chcete zaznamenat, můžete změnit **úplné** nebo **malé** výpis.

Procesů, které jsou aktuálně sledované jsou uvedené. Zaškrtněte políčka u procesů, které chcete zaznamenat. Chcete-li přidat do seznamu jiný proces, zadejte název procesu a poté zvolte **přidat proces** tlačítko.

  ![Výpisy stavu systému](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  V tématu [trvat ovládací prvek z protokolování a trasování v Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) a [Microsoft Azure Diagnostics část 4: vlastní protokolování součásti a Azure Diagnostics 1.3 změny](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) Další informace.

## <a name="view-the-diagnostics-data"></a>Zobrazení dat diagnostiky
Poté, co jste shromažďovaných dat diagnostiky pro cloudové služby nebo virtuálního počítače, můžete ji zobrazit.

### <a name="to-view-cloud-service-diagnostics-data"></a>Chcete-li zobrazit data diagnostiky cloudové služby
1. Cloudové služby jako obvyklé nasadit a potom ho spusťte.
2. Diagnostická data můžete zobrazit sestavu, která generuje Visual Studio nebo tabulek ve vašem účtu úložiště. Pokud chcete zobrazit data v sestavě, otevřete **Průzkumník cloudu** nebo **Průzkumníka serveru**, otevřete místní nabídce uzlu pro roli, které vás zajímají a potom zvolte **zobrazení diagnostických dat**.
   
    ![Zobrazení dat diagnostiky](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)
   
    Zobrazí se sestavu, která obsahuje všechna dostupná data.
   
    ![Microsoft Azure Diagnostics sestavy v sadě Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)
   
    Pokud se nezobrazí nejnovější data, můžete chtít čekat na v období uplynout.
   
    Vyberte **aktualizovat** propojit okamžitě aktualizovat data, nebo zvolte interval v **automatického obnovení** rozevírací pole se seznamem dat automaticky aktualizován. Export dat chyby, zvolte **exportovat do souboru CSV** tlačítko vytvořit v tabulce můžete otevřít soubor hodnot oddělených čárkami.
   
    V **Průzkumník cloudu** nebo **Průzkumníka serveru**, otevřete účet úložiště, který je spojen s nasazením.
3. Otevřete v prohlížeči tabulky tabulky diagnostiky a poté zkontrolovat data, která jste shromáždili. Pro vlastní protokoly a protokoly služby IIS můžete otevřít kontejner objektů blob. Kontrola v následující tabulce, můžete najít kontejner tabulky nebo objekt blob, který obsahuje data, která vás zajímá. Kromě dat pro tento soubor protokolu, položek tabulky obsahují EventTickCount, DeploymentId, Role a RoleInstance vám pomůže určit, jaké virtuální počítač a role generuje data a kdy. 
   
   | Diagnostických dat | Popis | Umístění |
   | --- | --- | --- |
   | Protokoly aplikací |Protokoly, které generuje kód voláním metod třídy System.Diagnostics.Trace. |WADLogsTable |
   | Protokoly událostí |Tato data jsou z protokolů událostí systému Windows na virtuálních počítačích. Systém Windows ukládá informace v těchto protokolech, ale aplikace a služby také použít k hlášení chyb nebo protokolování informací. |WADWindowsEventLogsTable |
   | Čítače výkonu |Můžete shromáždit data pro všechny čítače výkonu, který je k dispozici na virtuálním počítači. Operační systém poskytuje čítače výkonu, které zahrnují mnoho statistické údaje, třeba čas procesoru a využití paměti. |WADPerformanceCountersTable |
   | Protokoly infrastruktury |Tyto protokoly jsou generovány z infrastruktury diagnostiky, sám sebe. |WADDiagnosticInfrastructureLogsTable |
   | Protokoly služby IIS |Tyto protokoly záznam webových požadavků. Cloudové služby získá významný objem provozu, tyto protokoly mohou být velmi náročná, tak, aby měli shromažďovat a uložení dat této pouze v případě potřeby ho. |Můžete najít, že se nezdařilo žádost o protokolech v kontejneru objektů blob v rámci služby iis failedreqlogs wad v cestě pro tohoto nasazení, role a instance. Můžete najít v části wad. Služba iis logfiles dokončení protokoly. V tabulce WADDirectories jsou vytvořeny položky pro každý soubor. |
   | Výpisy stavu systému |Tyto informace obsahuje binární bitové kopie Cloudová služba proces (obvykle role pracovního procesu). |kontejner objektů blob wad. deformační výpisy |
   | Vlastní protokol soubory |Protokoluje data, která jste předdefinované. |Zadávat lze v kódu umístění vlastního protokolu souborů ve vašem účtu úložiště. Můžete například zadat kontejner objektů blob vlastní. |
4. Pokud se zkrátí dat jakéhokoli typu, můžete zkusit zvýšit vyrovnávací paměti pro data typu nebo zkrátit interval mezi přenosy dat z virtuálního počítače do svého účtu úložiště.
5. (volitelné) Vyprázdnit data z účtu úložiště příležitostně ke snížení celkové náklady na úložiště.
6. Po provedení úplné nasazení aktualizaci souboru diagnostics.cscfg (.wadcfgx pro Azure SDK 2.5) v Azure a cloudové služby převezme žádné změny konfigurace diagnostiky. Pokud, místo toho aktualizaci stávajícího nasazení, se neaktualizuje soubor .cscfg v Azure. Můžete je stále změnit nastavení diagnostiky, ale podle kroků v další části. Další informace o provádění úplné nasazení a aktualizaci stávajícího nasazení najdete v tématu [Průvodci publikováním aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Chcete-li zobrazit data diagnostiky virtuálního počítače
1. V místní nabídce virtuálního počítače, zvolte **zobrazení diagnostická Data**.
   
    ![Zobrazení dat diagnostiky ve virtuálním počítači Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)
   
    Tím se otevře **diagnostiky Souhrn** okno.
   
    ![Virtuální počítač Azure diagnostics souhrn](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  
   
    Pokud se nezobrazí nejnovější data, můžete chtít čekat na v období uplynout.
   
    Vyberte **aktualizovat** propojit okamžitě aktualizovat data, nebo zvolte interval v **automatického obnovení** rozevírací pole se seznamem dat automaticky aktualizován. Export dat chyby, zvolte **exportovat do souboru CSV** tlačítko vytvořit v tabulce můžete otevřít soubor hodnot oddělených čárkami.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Po nasazení nakonfigurovat diagnostiku cloudové služby
Pokud jste příčin problému s cloudem služby, že již spuštěn, můžete chtít shromažďování dat, které jste neurčili předtím, než jste původně nasadili roli. V takovém případě můžete spustit ke shromažďování dat pomocí nastavení v Průzkumníku serveru. V roli, v závislosti na tom, jestli otevřete dialogové okno Konfigurace diagnostiky z místní nabídky pro instanci nebo roli můžete nakonfigurovat diagnostiku pro jednu instanci nebo všechny instance. Pokud nakonfigurujete uzlu role, všechny změny se vztahují na všechny instance. Pokud nakonfigurujete uzlu instance, změny se použijí jenom tuto instanci.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Ke konfiguraci diagnostiky spuštěné cloudové služby
1. V Průzkumníku serveru rozbalte **cloudové služby** uzel a poté rozbalte uzly najít, role nebo instance, který chcete prozkoumat nebo obojí.
   
    ![Konfiguraci diagnostiky](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)
2. V místní nabídce pro uzel instanci nebo uzel role, zvolte **nastavení diagnostiky aktualizace**a potom vyberte nastavení diagnostiky, které chcete shromažďovat.
   
    Informace týkající se nastavení konfigurace najdete v tématu **konfigurace zdroje dat diagnostiky** v tomto tématu. Informace o tom, jak zobrazit data diagnostiky najdete v tématu **zobrazit data diagnostiky** v tomto tématu.
   
    Pokud změníte shromažďování dat v **Průzkumníka serveru**, tyto změny zůstávají v platnosti, dokud nebude plně znovu nasadit cloudové služby. Pokud použijete výchozí nastavení publikování, změny se nepřepíšou, protože publikování výchozí nastavení je k aktualizaci stávajícího nasazení, nikoli provést úplné opětovné nasazení. Chcete-li mít jistotu, vymazat nastavení v době nasazení, přejděte na **Upřesnit nastavení** kartě v Průvodci publikovat a zrušte zaškrtnutí **nasazení aktualizace** zaškrtávací políčko. Při opětovném s toto zaškrtávací políčko zaškrtnuto, nastavení vrátit zpět na hodnoty v souboru .wadcfgx (nebo .wadcfg) jako sada prostřednictvím editoru vlastnosti pro roli. Pokud aktualizujete vaše nasazení, Azure udržuje staré nastavení.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Řešení problémů služby Azure cloud
Pokud máte problémy s vaše projekty cloudových služeb, jako je například role, kterou vázne "zaneprázdněn" stav, opakovaně recykluje nebo vyvolá o vnitřní chybu serveru, jsou nástroje a techniky, které můžete použít k diagnostikování a odstranění těchto problémů. Pro konkrétní příklady běžné problémy a řešení, jakož i Přehled konceptů a nástroje pro diagnostiku a opravte tyto chyby, najdete v části [Azure PaaS výpočetní diagnostická Data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Dotazy a odpovědi
**Jaká je velikost vyrovnávací paměti a jak velké by to být?**

Na každou instanci virtuálního počítače kvóty omezit, kolik diagnostických dat může být uložený v místním systému souborů. Kromě toho můžete zadat velikost vyrovnávací paměti pro každý typ diagnostických dat, která je k dispozici. Tato velikost vyrovnávací paměti se chová jako individuální kvótu pro daný typ dat. Kontrolou spodní části dialogových oken, můžete určit celkové kvóty a množství paměti, která zůstává. Pokud zadáte větší vyrovnávací paměti nebo další typy dat, budete postupovat celkové kvóty. Celkové kvóty, můžete změnit úpravou konfiguračního souboru diagnostics.wadcfg/.wadcfgx. Data diagnostiky se ukládají na stejný systém souborů jako data aplikace, takže pokud vaše aplikace používá velké množství místa na disku, by neměl zvýšení celkové kvóty diagnostiky.

**Co je v období a jak dlouho by měl být?**

V období je množství času, který uplyne mezi dat zaznamená. Po každé období přenos dat se přesune ze místního systému souborů na virtuálním počítači tabulky ve vašem účtu úložiště. Pokud množství dat, které jsou shromážděny překročí kvótu před koncem období přenosu, budou zahozeny starší data. Můžete chtít snížit dobu přenosu, pokud jste ztráty dat, protože data je větší než velikost vyrovnávací paměti nebo celkové kvóty.

**Jaké časové pásmo je časová razítka v?**

Časová razítka jsou v místním časovém pásmu datového centra, který je hostitelem cloudové služby. Následující tři sloupce časového razítka v tabulkách protokolu se používají.

* **PreciseTimeStamp** je časové razítko události trasování událostí pro Windows. To znamená, čas událost je protokolována z klienta.
* **Časové razítko** PreciseTimeStamp zaokrouhleno dolů na hranici frekvence odesílání. Ano Pokud vaše nahrávání četnost je 5 minut a události čas 00:17:12, časové razítko bude 00:15:00.
* **Časové razítko** je časové razítko, kdy byla vytvořena entita v tabulce Azure.

**Jak lze spravovat náklady, při shromažďování diagnostických informací?**

Výchozí nastavení (**úrovně protokolování** nastavena na **chyba** a **přenos období** nastavena na **1 minuta**) jsou navržená k minimalizaci náklady. Náklady na výpočetní se zvyšuje, je-li shromažďování více diagnostických dat nebo snižte periodu přenos. Nemáte shromažďovat více dat, než je třeba a nezapomeňte zakázat shromažďování dat, když už nepotřebujete. Můžete vždy ho znovu povolit, i v době běhu, jak je uvedeno v předchozí části.

**Jak můžu shromáždit protokoly se nezdařilo žádost ze služby IIS?**

Ve výchozím nastavení služba IIS není shromažďovat protokoly žádosti se nezdařilo. Můžete nakonfigurovat službu IIS ke shromažďování, je-li upravit soubor web.config pro vaši webovou roli.

**Nezobrazují se informace o trasování z metod RoleEntryPoint jako OnStart. Co je?**

Metody RoleEntryPoint se nazývají v kontextu WAIISHost.exe, ne služba IIS. Proto informace o konfiguraci v souboru web.config, který obvykle netýká umožňuje trasování. Chcete-li vyřešit tento problém, přidejte souboru .config do projektu webové role a název souboru tak, aby odpovídaly výstup sestavení, které obsahuje kód RoleEntryPoint. Ve výchozím projektu webové role bude název souboru .config WAIISHost.exe.config. Pak přidejte následující řádky do tohoto souboru:

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

Nyní v **vlastnosti** nastavte **kopírovat do výstupního adresáře** vlastnost **vždy Kopírovat**.

## <a name="next-steps"></a>Další kroky
Další informace o diagnostiku protokolování v Azure najdete v tématu [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services/cloud-services-dotnet-diagnostics.md) a [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md).

