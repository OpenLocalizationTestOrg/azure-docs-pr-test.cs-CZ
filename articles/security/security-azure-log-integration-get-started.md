---
title: "aaaGet začít s Azure protokolu integrace | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello Azure protokolu integrační služby a integraci protokoly z úložiště Azure, Azure Security Center výstrahy a protokolování auditu Azure."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 07/26/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 26c19070d76ff73b1bdbd32ba77fb04978af387e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Integrace Azure protokolu s protokolování diagnostiky Azure a předávání událostí systému Windows
Protokol integrace se službou Azure (AzLog) umožňuje toointegrate nezpracovaná protokolů z vašich prostředků Azure do místní informace o zabezpečení a událostí správy (SIEM) systémy. Tato integrace umožňuje možné toohave řídicí panel jednotná zabezpečení pro všechny prostředky, místně nebo v cloudu hello, takže můžete agregovat, korelovat, analyzovat a výstrahy pro události zabezpečení související s vašimi aplikacemi.
>[!NOTE]
Další informace o protokolu integrace se službou Azure, můžete zkontrolovat hello [Přehled integrace protokolu Azure](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview).

Tento článek vám pomůže začít pracovat s protokolu integrace se službou Azure podle zaměření na instalaci hello hello Azlog služby a integrace služby hello s Azure Diagnostics. Hello Azure protokolu integrační služba bude mít toocollect informace protokolu událostí systému Windows z hello kanál událostí zabezpečení systému Windows z virtuálních počítačů nasazených v Azure IaaS. To je velmi podobné příliš "Předávání událostí" může použitá na místě.

>[!NOTE]
>Hello možnost toobring hello výstup integrace Azure protokolu v toohello SIEM je poskytovaný hello SIEM sám sebe. Prosím najdete v článku hello [integrace protokolu integrace se službou Azure s vašeho systému SIEM místní](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) Další informace.

toobe velmi jasně - hello Azure protokolu integrační služba běží na fyzický nebo virtuální počítač, který používá hello Windows Server 2008 R2 nebo novější operační systém (Windows Server 2012 R2 nebo Windows Server 2016 jsou upřednostňované).

fyzický počítač Hello můžete spustit místně (nebo na lokalitu hostitel). Pokud si zvolíte toorun hello Azure protokolu integrační služba na virtuálním počítači, aby virtuální počítač může být nacházejí na místních nebo ve veřejném cloudu, jako je Microsoft Azure.

Hello fyzické nebo virtuální počítač spuštěna služba integrace se službou Azure protokolu hello vyžaduje síťové připojení toohello veřejného cloudu Azure. Kroky v tomto článku poskytují podrobnosti o konfiguraci hello.

## <a name="prerequisites"></a>Požadavky
Minimálně vyžaduje instalaci hello AzLog hello následující položky:
* **Předplatné**. Pokud žádné nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/)
* A **účet úložiště** který lze použít pro protokolování diagnostiky systému Windows Azure (můžete použít účet předem nakonfigurovaná úložiště nebo vytvořte novou – bude ukážeme, jak tooconfigure hello účet úložiště později v tomto článku)
  >[!NOTE]
  V závislosti na vašem scénáři nemusí být vyžaduje účet úložiště. Pro hello Azure diagnostics scénář popsaná v tomto článku, které je zapotřebí.
* **Dvěma systémy**: počítač, který se spustí služba integrace protokolu Azure hello a počítači, ve kterém se bude monitorovat a mít jeho protokolování informace odesílané toohello Azlog služby počítače.
   * Na počítači, který chcete toomonitor – to je virtuální počítač spuštěn jako [virtuální počítač Azure](../virtual-machines/virtual-machines-windows-overview.md)
   * Počítač, který se spustí služba integrace Azure protokolu hello; Tento počítač bude shromažďovat všechny informace o hello protokolu, který bude později importovat do vašeho systému SIEM.
    * Tento systém může být místní nebo v Microsoft Azure.  
    * Je nutné toobe systémem x64 verzi systému Windows server 2008 R2 SP1 nebo vyšší a mít technologie .NET 4.5.1 nainstalována. Můžete určit hello verze rozhraní .NET nainstalované následující článek hello s názvem [postupy: určení rozhraní .NET Framework verze jsou nainstalované](https://msdn.microsoft.com/library/hh925568)  
    Musí mít účet úložiště Azure toohello připojení použitý pro Azure protokolování diagnostiky. Poskytujeme pokyny později v tomto článku na tom, jak si můžete ověřit tyto možnosti připojení

Pro rychlé ukázka hello proces vytváření virtuálního počítače pomocí portálu Azure hello podívejte se na video níže hello.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]



## <a name="deployment-considerations"></a>Aspekty nasazování
Během testování protokolu integrace se službou Azure, můžete použít libovolný systém, který splňuje požadavky na minimální verzi operačního systému hello. Pro hello produkční prostředí však zatížení může vyžadovat tooplan pro Škálováním.

Můžete spustit víc instancí služby Azure protokolu integrace hello (jedna instance na fyzický nebo virtuální počítač), pokud je svazek událostí vysoké. Kromě toho můžete načíst zůstatek, který účty úložiště Azure Diagnostics pro Windows (WAD) a hello počet instancí toohello tooprovide odběry by měla být založena na vaše kapacita.
>[!NOTE]
V tuto chvíli nemáme konkrétní doporučení pro při tooscale out instancí Azure protokolu integrace počítače (tj, počítače, které běží služba integrace Azure protokolu hello), nebo pro účty úložiště nebo předplatných. Škálování rozhodnutí, která by měla být založena na vaše připomínky výkonu v každé z těchto oblastí.

Máte také hello možnost tooscale až toohelp Služba integrace se službou Azure protokolu hello zlepšit výkon. v tématu Nastavení velikosti hello počítače, abyste zvolili toorun hello protokolů Azure integrační služba vám můžou pomoct Hello následující metriky výkonu:
* Na počítači 8 procesor (základní) může jedna instance Azlog integrátor zpracovat přibližně 24 milión událostí za den (~1M/hour).

* Na počítači 4procesoru (základní) může jedna instance Azlog integrátor zpracovat asi 1,5 milionu událostí za den (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Nainstalujte integrační protokolů Azure
tooinstall protokolu integrace se službou Azure, budete potřebovat toodownload hello [integrace Azure protokolu](https://www.microsoft.com/download/details.aspx?id=53324) instalační soubor. Spusťte pomocí rutiny hello instalační program a rozhodnout, pokud tooMicrosoft informace tooprovide telemetrie.  

![Instalace obrazovky se zaškrtnutým políčkem telemetrie](./media/security-azure-log-integration-get-started/telemetry.png)

*
> [!NOTE]
> Doporučujeme vám, že povolíte Microsoft toocollect telemetrická data. Zrušením zaškrtnutí políčka tuto možnost můžete vypnout kolekce telemetrická data.
>


Hello Azure protokolu integrační služba shromažďuje telemetrická data z hello počítače, na kterém je nainstalovaný.  

Mezi shromažďovaná telemetrická data je:

* Výjimky, ke kterým došlo během provádění integrace protokolů Azure
* Metriky o počtu hello dotazy a zpracování událostí
* Statistiky, o které Azlog.exe jsou používány možnosti příkazového řádku

proces instalace Hello je popsaná v hello video níže.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]



## <a name="post-installation-and-validation-steps"></a>POST kroky instalace a ověření
Po dokončení rutiny hello základní nastavení, jste kroky instalace a ověření tooperform post připravené kroku:
1. Otevřete okno prostředí PowerShell se zvýšenými oprávněními a přejděte příliš**c:\Program Files\Microsoft Azure protokolu integrace**
2. prvním krokem Hello potřebujete tootake je hello tooget, AzLog rutiny importovány. Můžete to udělat spuštěním skriptu hello **LoadAzlogModule.ps1** (Všimněte si hello ". \" v hello následující příkaz). Typ **.\LoadAzlogModule.ps1** a stiskněte klávesu **ENTER**.  
Měli byste vidět něco podobného jako co se zobrazí v hello obrázek. </br></br>
![Instalace obrazovky se zaškrtnutým políčkem telemetrie](./media/security-azure-log-integration-get-started/loaded-modules.png) </br></br>
3. Teď musíte tooconfigure AzLog toouse konkrétní prostředí Azure. Je "Prostředí Azure" hello "typ" cloudu Azure datové centrum, které chcete toowork s. Existuje několik prostředí Azure v tuto chvíli, hello aktuálně příslušné možnosti jsou buď **AzureCloud** nebo **AzureUSGovernment**.   Ujistěte se, abyste byli v v prostředí PowerShell zvýšenými **c:\program files\Microsoft Azure protokolu integrace\** </br></br>
    Jakmile existuje, spusťte příkaz hello: </br>
    ``Set-AzlogAzureEnvironment -Name AzureCloud``(pro Azure komerční)

      >[!NOTE]
      Pokud je příkaz hello úspěšná, neobdržíte žádné zpětnou vazbu.  Pokud chcete toouse hello US Government Azure cloud, byste použili **AzureUSGovernment** (pro hello – název proměnné) pro hello cloud vlády USA. V tuto chvíli nepodporuje ostatních cloudů Azure.  
4. Aby bylo možné sledovat systému budete potřebovat hello název účtu úložiště hello používá Azure Diagnostics.  V hello portálu Azure přejděte příliš**virtuální počítače** a vyhledejte hello virtuální počítač, který budete sledovat. V hello **vlastnosti** zvolte **nastavení pro diagnostiku**.  Klikněte na **agenta** a poznamenejte si název účtu úložiště hello zadán. Je nutné tento název účtu pro později.
![Nastavení diagnostiky Azure](./media/security-azure-log-integration-get-started/storage-account-large.png) </br></br>

      ![Nastavení diagnostiky Azure](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)
      >[!NOTE]
      Pokud monitorování není povoleno při vytvoření virtuálního počítače budete mít možnost tooenable hello ho jako v příkladu nahoře.
5. Nyní jsme budete přepnout zpět toohello naše pozornost počítač integrace Azure protokolů. Potřebujeme tooverify mít toohello připojení k účtu úložiště ze systému hello nainstalovanou protokolu integrace se službou Azure. Hello fyzický počítač nebo virtuální počítač se systémem hello protokolu integrace se službou Azure služba potřebuje přístup toohello tooretrieve informace o účtu úložiště v Azure Diagnostics protokolu jako nastaveny na všech hello sledovat systémy.  
  1. Můžete si stáhnout Azure Storage Explorer [zde](http://storageexplorer.com/).
  2. Spusťte pomocí rutiny instalace hello
  3. Po dokončení klikněte na tlačítko hello instalace **Další** a nechte políčko hello **spusťte Microsoft Azure Storage Explorer** zaškrtnutí.  
  4. Přihlaste se tooAzure.
  5. Ověřte, zda se zobrazí hello účet úložiště, který jste nakonfigurovali pro Azure Diagnostics.  
![Účty úložiště](./media/security-azure-log-integration-get-started/storage-account.jpg) </br></br>
   6. Všimněte si, že nejsou k dispozici několik možností v části účty úložiště. Jeden z nich je **tabulky**. V části **tabulky** byste měli vidět jednu s názvem **WADWindowsEventLogsTable**. </br></br>
   ![Účty úložiště](./media/security-azure-log-integration-get-started/storage-explorer.png) </br>

## <a name="integrate-azure-diagnostic-logging"></a>Integrovat protokolování diagnostiky Azure
V tomto kroku nakonfigurujete hello počítač se systémem hello protokolu integrace se službou Azure service tooconnect toohello účet úložiště, který obsahuje soubory protokolu hello.
toocomplete tento krok je nutné zadat předem pár věcí.  
* **FriendlyNameForSource:** Toto je popisný název, které můžete použít v účtu úložiště toohello, že jste nakonfigurovali informace toostore hello virtuální počítač z Azure Diagnostics
* **StorageAccountName:** Toto je název účtu úložiště hello, kterou jste zadali při konfiguraci Azure diagnotics hello.  
* **Klíč úložiště:** je hello úložiště klíč pro účet úložiště hello se uloží hello Azure diagnostické informace pro tento virtuální počítač.  

Proveďte následující kroky tooobtain hello úložiště klíč hello:
 1. Procházet toohello [portál Azure](http://portal.azure.com).
 2. V navigačním podokně hello hello Azure konzole, posuňte se dolů toohello a klikněte na tlačítko **další služby.**

 ![Další služby](./media/security-azure-log-integration-get-started/more-services.png)
 3. Zadejte **úložiště** v hello **filtru** textové pole. Klikněte na tlačítko **účty úložiště** (to se zobrazí po zadání **úložiště**)

   ![pole filtru](./media/security-azure-log-integration-get-started/filter.png)
 4. Zobrazí se seznam účtů úložiště, dvakrát klikněte na hello účet, který jste přiřadili tooLog úložiště.

   ![seznam účtů úložiště](./media/security-azure-log-integration-get-started/storage-accounts.png)
 5. Klikněte na **přístupové klíče** v hello **nastavení** části.

  ![Přístupové klávesy](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 6. Kopírování **key1** a umístí jej na bezpečném místě, který je k dispozici pro další krok hello.

   ![dva přístupové klíče](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 7. Na serveru hello nainstalovanou integrace se službou Azure protokolu otevřete příkazový řádek se zvýšenými oprávněními (Všimněte si, že okno příkazového řádku se zvýšenými oprávněními používáme v tomto poli, není prostředí PowerShell konzolu se zvýšenými oprávněními).
 8. Přejděte příliš**c:\Program Files\Microsoft Azure protokolu integrace**
 9. Spusťte ``Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey> ``. </br> Například ``Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`` budete-li tooshow ID předplatného hello až v případě hello XML, hello předplatné ID toohello popisný název připojení: ``Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`` nebo například``Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``

>[!NOTE]  
Too60 minut počkat a potom zobrazit hello události, které jsou vyžádány z účtu úložiště hello. tooview, otevřete **Prohlížeč událostí > protokoly systému Windows > předávaných událostí ty** na hello Azlog integrátor.

Zde vidíte, že video nad hello kroky popsané výše.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="what-if-data-is-not-showing-up-in-hello-forwarded-events-folder"></a>Co v případě dat se nezobrazí ve složce hello předávaných událostí?
Pokud po hodině data se nezobrazí v hello **předávaných událostí ty** složku, pak:

1. Zkontrolujte hello počítač spuštěné hello Azure protokolu integrační služby a ověřte, že má přístup k Azure. tootest připojení, zkuste tooopen hello [portál Azure](http://portal.azure.com) z prohlížeče hello.
2. Ujistěte se, že uživatelský účet hello **Azlog** má oprávnění k zápisu pro složku hello **users\Azlog**.
  <ol type="a">
   <li>Otevřete **Průzkumníka Windows** </li>
  <li> Přejděte příliš**c:\users** </li>
  <li> Klikněte pravým tlačítkem na **c:\users\Azlog** </li>
  <li> Klikněte na **zabezpečení**  </li>
  <li> Klikněte na **NT Service\Azlog** a zkontrolujte hello oprávnění pro účet hello. Pokud účet hello chybí na této kartě nebo pokud příslušná oprávnění hello nejsou aktuálně ukazuje můžete udělit práva účtu hello na této kartě.</li>
  </ol>
3.Ujistěte se, že účet úložiště hello přidán v příkazu hello **přidat zdroj Azlog** je uveden při spuštění příkazu hello **Azlog zdrojového seznamu**.
4. Přejděte příliš**Prohlížeč událostí > protokoly systému Windows > aplikace** toosee, pokud nejsou žádné chyby nahlásila integrace hello protokolů Azure.


Pokud narazíte na potíže během hello instalace a konfigurace, otevřete prosím [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md), vyberte **integrace protokolu** jako hello služby, pro kterou jsou žádosti o podporu.

Další možností podpory je hello [fórum MSDN integrace protokolu Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). Zde hello komunity může podporovat vzájemně otázky, odpovědi, tipy a triky na tom, jak tooget hello nejvíce mimo protokolu integrace se službou Azure. Kromě toho hello protokolu integrace se službou Azure tým monitoruje Toto fórum a bude vždy, když můžeme.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o integraci Azure protokolu, najdete v části hello následující dokumenty:

* [Microsoft Azure protokolu integrace pro Azure protokoly](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center podrobnosti, požadavky na systém a instalovat pokyny týkající se integrace protokolů Azure.
* [Integrace protokolu tooAzure ÚVOD](security-azure-log-integration-overview.md) – Toto téma představuje integrace protokolu tooAzure, jejích klíčových funkcích a jak to funguje.
* [Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – tento příspěvek blogu ukazuje, jak tooconfigure Azure protokolu toowork integrace s partnerských řešení Splunk, HP ArcSight a IBM QRadar. Toto je naše aktuální pokyny jak tooconfigure hello součásti systému SIEM. Zkontrolujte u dodavatele SIEM nejprve další podrobnosti.
* [Protokolů Azure integrace nejčastější dotazy (FAQ)](security-azure-log-integration-faq.md) – nejčastější dotazy týkající se tento odpovědi dotazy týkající se integrace protokolů Azure.
* [Integrace Security Center výstrahy s Azure protokolu integrace](../security-center/security-center-integrating-alerts-with-log-integration.md) – tento dokument ukazuje, jak toosync Security Center výstrahy, společně s shromážděných pomocí diagnostiky Azure a Azure protokoly aktivity, s vaší analýzy protokolů událostí zabezpečení virtuálního počítače nebo řešení SIEM.
* [Nové funkce pro protokoly auditu Azure a Azure diagnostics](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – tento příspěvek blogu vás seznámí tooAzure protokoly auditu a další funkce, které vám pomůžou proniknout do operací hello vašich prostředků Azure.
