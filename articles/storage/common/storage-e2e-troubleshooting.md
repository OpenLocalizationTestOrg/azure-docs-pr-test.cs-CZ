---
title: "aaaTroubleshooting Azure Storage pomocí diagnostiky & Message Analyzer | Microsoft Docs"
description: "Kurz demonstraci začátku do konce řešení potíží s Azure Storage Analytics, AzCopy a Microsoft Message Analyzer"
services: storage
documentationcenter: dotnet
author: robinsh
manager: timlt
ms.assetid: 643372a3-1c07-4e88-b4ef-042512a43086
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/15/2017
ms.author: robinsh
ms.openlocfilehash: 2d7a2a14b2e8da01b566ac3dec19996f0ef88cc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Poradce při potížích začátku do konce pomocí metrik Azure Storage a protokolování, AzCopy a Message Analyzer
[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../../includes/storage-selector-portal-e2e-troubleshooting.md)]

Diagnostika a řešení potíží se klíče dovednosti pro vytváření a podpora klientských aplikací s Microsoft Azure Storage. Z důvodu toohello distribuované povaha aplikaci Azure může být složitější než v tradiční prostředích diagnostice a řešení potíží s chybami a problémy s výkonem.

V tomto kurzu ukážeme, jak tooidentify určitým chybám, které mohou ovlivnit výkon a řešení těchto chyb od začátku do konce pomocí nástroje poskytované společností Microsoft a Azure Storage, v pořadí toooptimize hello klientské aplikace.

V tomto kurzu poskytuje praktické zkoumání scénáře řešení potíží začátku do konce. Podrobný průvodce koncepční tootroubleshooting úložiště Azure aplikace, najdete v části [monitorování, Diagnostika a řešení Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Nástroje pro řešení potíží s aplikacemi Azure Storage
tootroubleshoot klientským aplikacím pomocí služby Microsoft Azure Storage, můžete použít kombinaci nástroje toodetermine, kdy došlo problému a může být co hello příčinu problému hello. Mezi tyto nástroje patří:

* **Azure Storage Analytics**. [Azure Storage Analytics](/rest/api/storageservices/Storage-Analytics) poskytuje metriky a protokolování pro Azure Storage.
  
  * **Metriky úložiště** sleduje transakce metriky a metriky kapacity pro váš účet úložiště. Pomocí metriky, můžete určit, jak vaše aplikace pracuje podle tooa řadu různých opatření. V tématu [schématu tabulky metriky Analytics úložiště](/rest/api/storageservices/Storage-Analytics-Metrics-Table-Schema) pro další informace o typech hello metrik sledován pomocí funkce Storage Analytics.
  * **Protokolování úložiště** protokoly protokol každé žádosti toohello tooa straně serveru služby Azure Storage. Podrobná data sleduje Hello protokolu pro každý požadavek, včetně hello operaci provést, hello stav operace hello a informace o latenci. V tématu [úložiště analýzy protokolů formátu](/rest/api/storageservices/Storage-Analytics-Log-Format) Další informace o datech hello žádosti a odpovědi, která jsou zapsána toohello protokoly analytika úložiště.

> [!NOTE]
> Účty úložiště s typem replikace z Zónově redundantní úložiště (ZRS) nemají hello metriky nebo možnosti protokolování v tuto chvíli povolena. 
> 
> 

* **Portál Azure**. Můžete nakonfigurovat protokolování a metriky pro svůj účet úložiště hello [portál Azure](https://portal.azure.com). Můžete také zobrazit grafy, které ukazují, jak se vaše aplikace provádí v čase a nakonfigurovat toonotify výstrahy, které pokud aplikace provede jinak než měla příslušné metriky.
  
    V tématu [monitorovat účet úložiště na portálu Azure hello](storage-monitor-storage-account.md) informace o konfiguraci, monitorování v hello portálu Azure.
* **AzCopy**. Protokoly serveru pro Azure Storage jsou uloženy jako objekty BLOB, abyste je mohli používat AzCopy toocopy hello objekty BLOB tooa místní adresář protokolu pro analýzu pomocí Microsoft Message Analyzer. V tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md) Další informace o AzCopy.
* **Microsoft Message Analyzer**. Message Analyzer je nástroj, který využívá soubory protokolu a zobrazí data protokolu ve visual formátu, který umožňuje snadno toofilter, vyhledávání a skupiny protokolovat data do užitečné sad, které můžete tooanalyze chyb a problémů s výkonem. V tématu [Microsoft zpráva analyzátor operační průvodce](http://technet.microsoft.com/library/jj649776.aspx) Další informace o Message Analyzer.

## <a name="about-hello-sample-scenario"></a>O hello vzorový scénář
V tomto kurzu podíváme na scénář, kde metrik Azure Storage udává míra nízkou procenta úspěšnosti pro aplikaci, která volá úložiště Azure. Hello nízkou procenta úspěšnosti míra metrika (zobrazené jako **PercentSuccess** v hello [portál Azure](https://portal.azure.com) a v tabulkách metriky hello) sleduje operace, která úspěšné, ale stavový kód HTTP, která je větší, které vracejí než 299. V souborech protokolů hello úložiště na straně serveru, se zaznamenávají tyto operace se stavem transakce **ClientOtherErrors**. Další podrobnosti o hello nízkou procenta úspěšnosti metrika najdete v tématu [metriky ukazují nízkou PercentSuccess nebo položky protokolu analýzy mít operací s stav transakce ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Operace úložiště v Azure může vrátit stavové kódy HTTP větší než 299 v rámci své normální fungování. Ale tyto chyby v některých případech indikovat, že bude mít toooptimize klientské aplikace pro zvýšení výkonu.

V tomto scénáři popíšeme toobe rychlost nízkou procenta úspěšnosti nic nižší než 100 %. Můžete vybrat jiné metriky úrovni, ale podle potřeby tooyour. Doporučujeme vám, že během testování vaší aplikace, vytvoříte tolerance směrného plánu pro vaše klíčové metriky výkonu. Například lze určit, na základě testování, že aplikace má mít míra konzistentní procenta úspěšnosti 90 % nebo 85 %. Pokud vaše data metriky ukazuje, že je aplikace hello odchylují od toto číslo, můžete prozkoumat, co může být příčinou zvýšení hello.

Pro naše vzorový scénář když jsme jste vytvořili hello procenta úspěšnosti míra metrika je menší než 100 %, jsme zkontrolujte hello protokoly toofind hello chyb, které korelovat toohello metriky a využít toofigure se co způsobuje hello nižší procenta úspěšnosti. Podíváme konkrétně chyby v rozsahu 400 hello. Potom jsme budete prozkoumat přesněji chyb 404 (není nalezena).

### <a name="some-causes-of-400-range-errors"></a>Některé příčiny chyb 400 rozsahu
Příklady Hello níže jsou uvedeny vzorky chybami 400 rozsah pro požadavky na Azure Blob Storage a jejich možné příčiny. Některé z těchto chyb, jakož i chyby v rozsahu hello 300 a 500 rozsah hello, můžete přispívat tooa nízkou procenta úspěšnosti.

Všimněte si, že následující seznamy hello daleko od dokončení. V tématu [stavu a kódy chyb](http://msdn.microsoft.com/library/azure/dd179382.aspx) na webu MSDN podrobnosti o obecných chyb úložiště Azure a o konkrétní tooeach chyby služby úložiště hello.

**Příklady stavový kód 404 (není nalezena)**

Nastane, když operace čtení pro kontejner nebo objekt blob se nezdaří, protože nebyl nalezen objekt blob hello nebo kontejneru.

* Nastane, pokud jiný klient před tento požadavek byl odstraněn z kontejneru nebo objektů blob.
* Nastane, pokud používáte volání rozhraní API, která vytvoří hello kontejner nebo objekt blob po zjišťování, zda existuje. Ujistěte se, HEAD, volání první toocheck pro existenci hello hello kontejner nebo objekt blob; Hello CreateIfNotExists rozhraní API Pokud neexistuje, bude vrácena chyba 404, a pak Přišla žádost o druhou PUT toowrite hello kontejner nebo objekt blob.

**Stav 409 (konflikt) příklady kódu**

* Nastane, pokud používáte vytvoření rozhraní API toocreate nový kontejner nebo objekt blob, bez nejprve zjištění existence a kontejner nebo objekt blob s tímto názvem již existuje.
* Nastane, pokud kontejner se odstraňuje a pokusíte se toocreate nový kontejner s hello stejný název před dokončením operace odstranění hello.
* V případě zadejte zapůjčení na kontejner nebo objekt blob a je už existuje zapůjčení.

**Stavový kód 412 (předběžnou se nezdařilo) příklady**

* Nastane, když hello podmínka uvedená v hlavičku podmíněného nebyly splněny.
* Nastane, když je zadané ID zapůjčení hello neodpovídá ID hello zapůjčení na hello kontejner nebo objekt blob.

## <a name="generate-log-files-for-analysis"></a>Generovat soubory protokolů pro analýzu
V tomto kurzu použijeme Message Analyzer toowork s tři různé typy souborů protokolu, i když může zvolit toowork pomocí některého z těchto:

* Hello **serveru protokolu**, který je vytvořen, když povolíte protokolování Azure Storage. protokol server Hello obsahuje data o každé operace volána pro jeden z hello služby Azure Storage – objekt blob, fronty, tabulky a soubor. Hello serveru protokolu označuje, kterou operaci byla volána a jaký kód stavu je vrácený, stejně jako další podrobnosti o hello žádostí a odpovědí.
* Hello **protokolu klientů .NET**, který je vytvořen, když povolíte protokolování na straně klienta z v rámci aplikace .NET. protokol klienta Hello zahrnuje podrobné informace o tom, jak hello klienta připraví hello žádosti a přijímá a zpracovává hello odpovědi.
* Hello **protokolu trasování sítě HTTP**, který shromažďuje data na data požadavku a odpovědi protokolu HTTP nebo HTTPS, včetně pro operace Azure Storage. V tomto kurzu budete se vygeneruje hello trasování sítě prostřednictvím Message Analyzer.

### <a name="configure-server-side-logging-and-metrics"></a>Konfigurace protokolování na straně serveru a metriky
Nejdřív potřebujeme tooconfigure protokolování Azure Storage a metriky, tak, aby jsme data z tooanalyze aplikace hello klienta. Protokolování a metriky můžete nakonfigurovat různými způsoby – prostřednictvím hello [portál Azure](https://portal.azure.com), pomocí prostředí PowerShell, nebo prostřednictvím kódu programu. V tématu [povolení metriky úložiště a zobrazení dat metrik](http://msdn.microsoft.com/library/azure/dn782843.aspx) a [povolení protokolování úložiště a přístup k datům protokolu](http://msdn.microsoft.com/library/azure/dn782840.aspx) na webu MSDN pro podrobné informace o konfiguraci protokolování a metriky.

**Prostřednictvím hello portálu Azure**

tooconfigure protokolování a metriky pro váš účet úložiště pomocí hello [portál Azure](https://portal.azure.com), postupujte podle pokynů hello [monitorovat účet úložiště na portálu Azure hello](storage-monitor-storage-account.md).

> [!NOTE]
> Není možné tooset minutu metriky pomocí hello portálu Azure. Doporučujeme však, že je nastavena pro účely tohoto kurzu hello a příčin problémů s výkonem s vaší aplikací. Můžete nastavit minutu metriky pomocí prostředí PowerShell, jak je uvedeno níže, nebo programově pomocí klientské knihovny pro úložiště hello.
> 
> Všimněte si, že hello portál Azure je nemůže zobrazit minutu metriky, pouze hodinové metriky.
> 
> 

**Pomocí prostředí PowerShell**

tooget spuštění pomocí prostředí PowerShell pro Azure, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

1. Použití hello [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0) tooadd rutiny Azure uživatelů účet toohello okno prostředí PowerShell:
   
    ```powershell
    Add-AzureAccount
    ```

2. V hello **přihlášení tooMicrosoft Azure** oken, typ hello e-mailovou adresu a heslo spojené s vaším účtem. Azure ověřuje a uloží hello přihlašovací údaje a potom zavře okno hello.
3. Nastavte hello výchozí účet toohello úložiště účet úložiště, který používáte pro kurz hello spuštěním těchto příkazů v okně PowerShell hello:
   
    ```powershell
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Povolení protokolování pro hello služby objektů Blob storage:
   
    ```powershell
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```

5. Povolit úložiště metriky pro hello služba objektů Blob, která se tooset **- MetricsType** příliš`Minute`:
   
    ```powershell
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>Konfigurovat protokolování klienta rozhraní .NET
tooconfigure protokolování pro aplikace .NET na straně klienta zapněte diagnostiku .NET v konfiguračním souboru aplikace hello (soubor web.config nebo app.config). V tématu [klienta protokolování s hello Klientská knihovna pro .NET úložiště](http://msdn.microsoft.com/library/azure/dn782839.aspx) a [klienta protokolování s hello Microsoft Azure SDK úložiště pro jazyk Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) na webu MSDN podrobnosti.

Hello klienta protokolu obsahuje podrobné informace o tom, jak hello klienta připraví hello žádosti a přijímá a zpracovává hello odpovědi.

Hello Klientská knihovna pro úložiště ukládá data na straně klienta protokolu v umístění hello zadaný v konfiguračním souboru aplikace hello (soubor web.config nebo app.config).

### <a name="collect-a-network-trace"></a>Shromažďovat trasování sítě
Message Analyzer toocollect při trasování sítě protokolu HTTP nebo HTTPS můžete použít, když běží klientské aplikace. Message Analyzer používá [Fiddler](http://www.telerik.com/fiddler) na hello back-end. Než budete shromažďovat trasování sítě hello, doporučujeme nakonfigurovat komunikaci přes protokol HTTPS Fiddler toorecord bez šifrování:

1. Nainstalujte [Fiddler](http://www.telerik.com/download/fiddler).
2. Spusťte aplikaci Fiddler.
3. Vyberte **nástroje | Možnosti Fiddler**.
4. V dialogovém okně Možnosti hello, zajistěte, aby **zaznamenat umožňuje připojení HTTPS** a **dešifrovat provoz HTTPS** jsou obě vybrané, jak je uvedeno níže.

![Konfigurace možností Fiddler](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Hello kurzu shromažďovat a uložit trasování sítě nejprve v Message Analyzer, pak vytvořit při analysis relace tooanalyze hello trasování a hello protokoly. toocollect trasování sítě v Message Analyzer:

1. V Message Analyzer, vyberte **souboru | Rychlé trasování | Bez šifrování HTTPS**.
2. Hello trasování bude zahájeno okamžitě. Vyberte **Zastavit** toostop hello trasování, takže jsme ji můžete nakonfigurovat pouze provoz typu tootrace úložiště.
3. Vyberte **upravit** tooedit hello trasování relace.
4. Vyberte hello **konfigurace** odkaz toohello napravo od hello **Microsoft-Pef-WebProxy** zprostředkovatele trasování událostí pro Windows.
5. V hello **Upřesnit nastavení** dialogové okno, klikněte na tlačítko hello **zprostředkovatele** kartě.
6. V hello **filtru Hostname** pole, zadejte vaše koncové body úložiště, oddělené mezerami. Například můžete zadat koncové body takto: Změnit `storagesample` toohello název účtu úložiště:

    ```   
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Ukončete hello dialogové okno a klikněte na tlačítko **restartujte** toobegin shromažďování hello trasování pomocí filtru název hostitele hello na místě, aby pouze provoz sítě úložiště Azure je součástí trasování hello.

> [!NOTE]
> Po dokončení shromažďování síťové trasování, důrazně doporučujeme hello nastavení, které můžete měnit v komunikaci přes protokol HTTPS toodecrypt Fiddler vrátit zpět. V dialogovém okně Možnosti Fiddler hello, zrušte výběr hello **zaznamenat umožňuje připojení HTTPS** a **dešifrovat provoz HTTPS** zaškrtávací políčka.
> 
> 

V tématu [pomocí funkce trasování sítě hello](http://technet.microsoft.com/library/jj674819.aspx) na webu Technet podrobnosti.

## <a name="review-metrics-data-in-hello-azure-portal"></a>Zkontrolujte data metriky v hello portálu Azure
Po spuštění vaší aplikace pro určitou dobu, můžete zkontrolovat grafy hello metriky, které se zobrazují v hello [portál Azure](https://portal.azure.com) tooobserve, jaký má byl výkon služby.

Nejprve přejděte tooyour účet úložiště v hello portálu Azure. Ve výchozím nastavení, monitorování grafu s hello **procento úspěšnosti** metrika se zobrazí v okně účtu hello. Pokud jste dříve upravit hello grafu toodisplay jiné metriky, přidejte hello **procento úspěšnosti** metriku.

Nyní uvidíte **procento úspěšnosti** v hello monitorování grafu, společně s další metriky jste mohli přidat. V hello scénáři, který jsme vám další šetření tak, že analýza protokolů hello v Message Analyzer procenta úspěšnosti hello je trochu nižší než 100 %.

Další informace o přidání a přizpůsobení metriky grafy najdete v tématu [přizpůsobení metriky grafy](storage-monitor-storage-account.md#customize-metrics-charts).

> [!NOTE]
> Po povolení metrik úložiště může trvat nějakou dobu, než vaše data tooappear metriky v hello portálu Azure. To je proto hodinové metriky pro hello předchozí hodiny nejsou v hello portál Azure nezobrazí, dokud hello uplynul aktuální hodiny. Navíc minutu metriky se momentálně nezobrazují v hello portálu Azure. V závislosti na tom, když povolíte metriky, může trvat až tootwo hodin toosee metriky data.
> 
> 

## <a name="use-azcopy-toocopy-server-logs-tooa-local-directory"></a>Použít AzCopy toocopy server protokoly tooa místní adresář
Úložiště Azure zapíše serveru tooblobs data protokolu, zatímco metriky se zapisují tootables. Objekty BLOB protokolu jsou k dispozici v hello dobře známé `$logs` kontejner pro váš účet úložiště. Objekty BLOB protokolu jsou pojmenované hierarchicky podle rok, měsíc, den a hodina, aby mohli snadno najít hello rozsah doba, kterou chcete tooinvestigate. Například v hello `storagesample` účet, hello kontejner pro objekty BLOB protokolu hello pro 01/02/2015, z 8-9: 00, je `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Hello jednotlivé objekty BLOB v tomto kontejneru jsou pojmenované postupně, počínaje `000000.log`.

Tyto serverové protokolu soubory tooa umístění podle vaší volby toodownload nástroj příkazového řádku AzCopy hello můžete použít na místním počítači. Například můžete použít následující příkaz toodownload hello protokolu soubory pro operace objektů blob, které trvalo umístit na hello 2 leden 2015 toohello složky `C:\Temp\Logs\Server`; nahradit `<storageaccountname>` hello název účtu úložiště a `<storageaccountkey>` s vaší přístupový klíč účtu:

```azcopy
AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V
```
AzCopy je k dispozici ke stažení na hello [Azure stáhne](https://azure.microsoft.com/downloads/) stránky. Podrobnosti o použití nástroje AzCopy najdete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).

Další informace o stahování protokolů na straně serveru najdete v tématu [stáhnout protokolování úložiště dat protokolu](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-tooanalyze-log-data"></a>Použít data protokolu tooanalyze Microsoft Message Analyzer
Microsoft Message Analyzer je nástroj pro zachytávání, zobrazení a analýza protokolu zasílání zpráv provoz, události a další scénáře řešení potíží a diagnostické zprávy systému nebo aplikace. Message Analyzer také vám umožní tooload agregační a analyzovat data z protokolu a uložit trasovací soubory. Další informace o Message Analyzer najdete v tématu [Microsoft zpráva analyzátor operační průvodce](http://technet.microsoft.com/library/jj649776.aspx).

Message Analyzer obsahuje prostředky pro úložiště Azure, které vám pomohou tooanalyze serveru, klienta a protokoly sítě. V této části probereme, jak toouse tyto nástroje tooaddress hello problém nízkou procenta úspěšnosti v hello protokol úložiště.

### <a name="download-and-install-message-analyzer-and-hello-azure-storage-assets"></a>Stáhněte a nainstalujte Message Analyzer nebo hello prostředky úložiště Azure
1. Stáhněte si [Message Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) z hello Microsoft Download Center a spusťte instalační program hello.
2. Spusťte Message Analyzer.
3. Z hello **nástroje** nabídce vyberte možnost **správce inventáře**. V hello **správce inventáře** dialogovém okně, vyberte **stáhne**, vyfiltrujte v **Azure Storage**. Hello prostředků úložiště Azure, uvidíte, jak ukazuje následující obrázek hello.
4. Klikněte na tlačítko **všechny zobrazené položky synchronizace** tooinstall hello prostředky úložiště Azure. k dispozici prostředky Hello patří:
   * **Pravidla ve službě Azure Storage barev:** pravidel barvy úložiště Azure umožňují toodefine speciální filtry, které používají barvu textu, a styly písma toohighlight zprávy, které obsahují konkrétní informace trasování.
   * **Grafy úložiště Azure:** grafy úložiště Azure jsou předdefinované grafy graf data protokolu serveru. Všimněte si, že toouse Azure Storage grafy v tuto chvíli může pouze zatížení hello serveru protokolu do hello Analysis mřížky.
   * **Azure Storage analyzátory:** hello Azure Storage analyzátory analýzy hello Azure Storage klienta, serveru a HTTP přihlásí v pořadí toodisplay je hello Analysis mřížky.
   * **Filtry úložiště Azure:** úložiště Azure filtry jsou předdefinované kritéria používané tooquery vaše data v hello Analysis mřížky.
   * **Rozložení zobrazení Azure Storage:** zobrazení rozložení úložiště Azure jsou předdefinovaného zobrazení sloupců a seskupení v hello Analysis mřížky.
5. Po instalaci hello prostředky, restartujte Message Analyzer.

![Správce Asset analyzátor zpráv](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [!NOTE]
> Nainstalujte všechny prostředky Azure Storage hello zobrazený pro účely tohoto kurzu hello.
> 
> 

### <a name="import-your-log-files-into-message-analyzer"></a>Import souborů protokolu do Message Analyzer
Můžete importovat všechny uložené soubory protokolů (na straně serveru, klienta a sítě) do jedné relace v Microsoft Message Analyzer pro analýzu.

1. Na hello **soubor** nabídky v Microsoft Message Analyzer, klikněte na tlačítko **novou relaci**a potom klikněte na **prázdné relace**. V hello **novou relaci** dialogové okno, zadejte název pro relaci analýzy. V hello **relace podrobnosti** panelu a potom klikněte na hello **soubory** tlačítko.
2. tooload hello síťové trasování data generována Message Analyzer, klikněte na **přidat soubory**, vyhledejte toohello umístění pro uložení souboru .matp z webové relace trasování, vyberte hello .matp souboru, a klikněte na tlačítko **otevřete**.
3. data protokolu na straně serveru hello tooload, kliknutím na tlačítko **přidat soubory**, procházet toohello umístění, kam jste stáhli svoje protokoly na straně serveru, vyberte soubory protokolu hello hello časové rozmezí tooanalyze a klikněte na **otevřete**. Potom v hello **relace podrobnosti** panelu, sada hello **konfigurace protokolů Text** rozevíracího seznamu pro každý soubor protokolu na straně serveru příliš**AzureStorageLog** tooensure které vám společnost Microsoft Message Analyzer můžete analyzovat soubor protokolu hello správně.
4. data protokolu na straně klienta hello tooload, kliknutím na tlačítko **přidat soubory**, procházet toohello umístění, kam jste uložili svoje protokoly na straně klienta, vyberte soubory protokolu hello tooanalyze a klikněte na **otevřete**. Potom v hello **relace podrobnosti** panelu, sada hello **konfigurace protokolů Text** rozevíracího seznamu pro každý soubor protokolu na straně klienta příliš**AzureStorageClientDotNetV4** tooensure, Microsoft Message Analyzer můžete analyzovat soubor protokolu hello správně.
5. Klikněte na tlačítko **spustit** v hello **novou relaci** data protokolu hello dialogové okno pro tooload a analýzy. data protokolu Hello se zobrazí v hello zpráva analyzátor Analysis mřížky.

Následující obrázek Hello ukazuje relaci příklad nakonfigurované serveru, klienta a soubory protokolu trasování sítě.

![Konfigurace relace analyzátor zpráv](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Všimněte si, že Message Analyzer načte soubory protokolů do paměti. Pokud máte velké sady dat protokolu, budete chtít toofilter v pořadí tooget hello nejlepší výkon ze Message Analyzer.

Nejprve určit hello časovém rámci, která vás zajímá kontrola a udržovat tento časový rámec co nejmenší. V řadě případů budete chtít tooreview a dobu minut nebo hodin maximálně. Importovat hello nejmenší sadu protokoly, které bude vyhovovat vašim potřebám.

Pokud máte velké množství dat protokolu, pak můžete toospecify toofilter filtr relace data protokolu jej před načtením. V hello **filtru relace** pole, vyberte hello **knihovny** tlačítko toochoose předdefinovaný filtr; například zvolit **globální doba filtru I** z hello filtry Azure Storage toofilter v časovém intervalu. Potom můžete upravit hello filtru kritéria toospecify hello spuštění a ukončení časové razítko pro hello interval chcete toosee. Můžete také filtrovat podle kódu stavu; Například můžete pouze položky protokolu tooload kde je hello stavový kód 404.

Další informace o importování dat protokolu do Microsoft Message Analyzer najdete v tématu [Data načítání zpráv](http://technet.microsoft.com/library/dn772437.aspx) na webu TechNet.

### <a name="use-hello-client-request-id-toocorrelate-log-file-data"></a>Pomocí hello klienta požadavek ID toocorrelate protokolu soubor dat
Hello Klientská knihovna pro úložiště Azure automaticky vygeneruje ID žádosti klienta jedinečný pro každý požadavek. Tato hodnota se zapíše protokol klienta toohello, hello serveru protokolu a trasování sítě hello, můžete ji použít toocorrelate dat napříč všechny tři protokoly v rámci Message Analyzer. V tématu [ID žádosti klienta](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) pro další informace o klientovi hello vyžadovat ID.

Hello části níže popisují, jak toouse předem nakonfigurované a vlastní rozložení zobrazení dat toocorrelate a skupiny založené na požadavku klienta hello.

### <a name="select-a-view-layout-toodisplay-in-hello-analysis-grid"></a>Vyberte toodisplay rozložení zobrazení v hello Analysis mřížky
Hello prostředků úložiště pro Message Analyzer zahrnují zobrazení rozložení úložiště Azure, které jsou předem nakonfigurovaná zobrazení používané toodisplay data s užitečné seskupení a sloupců pro různé scénáře. Můžete také vytvořit vlastní zobrazení rozložení a uložit pro opakované použití.

Hello obrázek níže ukazuje hello **rozložení zobrazení** nabídce výběrem **rozložení zobrazení** pásu karet hello panelu nástrojů. Hello zobrazení rozložení pro Azure Storage jsou seskupené v rámci hello **Azure Storage** uzlu v nabídce hello. Můžete vyhledat `Azure Storage` v toofilter pole hello hledání v úložišti Azure zobrazit pouze rozložení. Můžete také vybrat hello hvězdičkami další tooa zobrazení rozložení toomake it a Oblíbené položky a zobrazit ji hello horní části nabídky hello.

![Nabídka Zobrazit rozložení](./media/storage-e2e-troubleshooting/view-layout-menu.png)

toobegin s vyberte **seskupené podle ClientRequestID a modul**. Toto zobrazení rozložení skupiny protokolovat data ze všech tří protokolů nejprve podle ID žádosti klienta a pak podle zdrojového souboru protokolu (nebo **modulu** v Message Analyzer). K tomuto zobrazení můžete rozbalit ID žádosti klienta konkrétní a vidět data z všechny tři soubory protokolů pro tento požadavek klienta ID.

Následující obrázek Hello ukazuje tento rozložení zobrazení použít toohello ukázková data protokolu, s podmnožinu sloupců zobrazí. Uvidíte, že pro ID žádosti konkrétního klienta, zobrazuje hello mřížky analýzy dat z protokolu hello klienta, serveru protokolu a trasování sítě.

![Zobrazení rozložení úložiště Azure](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

> [!NOTE]
> Různých protokolových souborech mají různé sloupce, takže když v hello Analysis mřížky se zobrazí data z více souborů protokolu, některé sloupce nesmí obsahovat žádná data pro daný řádek. Například hello obrázku výše, řádky protokolu klienta nezobrazovat žádná data pro hello **časové razítko**, **TimeElapsed**, **zdroj**, a **cílové** sloupců, protože tyto sloupce nejsou k dispozici v hello klienta protokolu, ale existují v hello síťové trasování. Podobně hello **časové razítko** sloupec zobrazuje časové razítko data z hello serveru protokolu, ale nezobrazí se žádná data pro hello **TimeElapsed**, **zdroj**, a  **Cílový** sloupce, které nejsou součástí hello serveru protokolu.
> 
> 

Kromě toho toousing hello Azure Storage zobrazení rozložení, můžete také definovat a uložit vlastní rozložení zobrazení. Můžete vybrat další požadované pole pro seskupování dat a uložit hello seskupení jako součást vaší vlastní rozložení.

### <a name="apply-color-rules-toohello-analysis-grid"></a>Použít toohello pravidla barev Analysis mřížky
Prostředky úložiště Hello také obsahovat pravidla barvy, která nabízí že vizuál znamená tooidentify různých typů chyb v hello Analysis mřížky. Hello pravidel předdefinované barvy použít tooHTTP chyby, aby se zobrazovaly pouze pro server hello protokolu a síťové trasování.

Vyberte tooapply pravidel barvy, **pravidel barvy** z pásu karet hello panelu nástrojů. Uvidíte pravidel barvy hello Azure Storage v nabídce hello. Hello kurzu vyberte **chyby klienta (StatusCode mezi 400 a 499)**, jak ukazuje následující obrázek hello.

![Zobrazení rozložení úložiště Azure](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Kromě toho toousing hello Azure Storage barvu, která pravidla, můžete také definovat a uložit vlastní pravidla barvy.

### <a name="group-and-filter-log-data-toofind-400-range-errors"></a>Skupiny a filtrování dat toofind 400-range chyby v protokolu
V dalším kroku jsme budete skupiny a filtrovat hello protokolu data toofind všechny chyby v rozsahu hello 400.

1. Vyhledejte hello **StatusCode** sloupec v hello Analysis mřížky, klikněte pravým tlačítkem na hello sloupec nadpis a vyberte **skupiny**.
2. V dalším kroku skupina na hello **ClientRequestId** sloupce. Dozvíte se, že hello data v hello Analysis mřížky je nyní uspořádané podle stavu kódu a požadavkem klienta ID.
3. Zobrazí okno nástroje filtr zobrazení hello, pokud již není zobrazen. Na pásu karet nástrojů hello, vyberte **nástroj Windows**, pak **filtr zobrazení**.
4. toofilter hello data toodisplay pouze 400-range chyby v protokolu, přidejte následující toohello kritéria filtru hello **filtr zobrazení** a klikněte na **použít**:

    ```   
    (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)
    ```

Následující obrázek Hello ukazuje výsledky hello toto seskupení a filtru. Se zvětšující hello **ClientRequestID** pole pod hello seskupení pro kód stavu 409, například zobrazí operaci, která tento kód stavu.

![Zobrazení rozložení úložiště Azure](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Po použití tento filtr, uvidíte, že řádky z hello klienta protokolu jsou vyloučeny, jako hello nezahrnuje protokol klienta **StatusCode** sloupce. toobegin s přečtěte hello serveru a chyb 404 protokoly toolocate trasování pro sítě a potom vrátíme toohello klienta protokolu tooexamine hello operací klienta, které vedly toothem.

> [!NOTE]
> Můžete filtrovat podle hello **StatusCode** sloupce a stále zobrazí data ze všech tří protokolů, včetně hello protokolu klienta, pokud přidáte filtr toohello výraz, který obsahuje položky protokolu, kde má hodnotu null hello stavový kód. tooconstruct tento výraz filtru, použijte:
> 
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
> 
> Tento filtr vrátí všechny řádky z klienta hello protokolu a pouze řádky z hello serveru protokolu a protokolu HTTP kde je větší než 400 hello stavový kód. Pokud ji použijete rozložení zobrazení toohello seskupené podle ID žádosti klienta a modul, můžete hledat nebo procházení hello protokolu položky toofind ty, které kde zastoupení všech tří protokolů.   
> 
> 

### <a name="filter-log-data-toofind-404-errors"></a>Filtrování chyb 404 toofind data protokolu
Hello úložiště prostředky zahrnují předdefinované filtry, které můžete použít toonarrow protokolovat data toofind hello chyby nebo trendů, které hledáte. V dalším kroku jsme budete používat dvě předdefinované filtry: jeden, který filtruje hello serveru a síťové protokoly trasování pro chyb 404 a ten, který filtruje hello data v zadaném časovém rozmezí.

1. Zobrazí okno nástroje filtr zobrazení hello, pokud již není zobrazen. Na pásu karet nástrojů hello, vyberte **nástroj Windows**, pak **filtr zobrazení**.
2. V okně filtru zobrazení hello vyberte **knihovny**a vyhledejte na `Azure Storage` toofind hello filtry Azure Storage. Vyberte hello filtr **404 (Nenalezeno) zpráv ve všech protokolech**.
3. Zobrazení hello **knihovny** nabídky znovu a vyhledejte a vyberte hello **globální filtr času**.
4. Časová razítka hello zobrazuje v rozsahu toohello hello filtru, že tooview chcete upravte. To vám pomůže toonarrow hello řadu data tooanalyze.
5. Filtr měla vypadat podobně jako následující příklad toohello. Klikněte na tlačítko **použít** tooapply hello filtru toohello Analysis mřížky.

    ```   
    ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)
    ```

    ![Zobrazení rozložení úložiště Azure](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Analyzovat data protokolu
Teď, když máte seskupené a filtrovat data, můžete zkontrolovat hello podrobnosti o jednotlivých požadavků, které generuje chyby 404. V zobrazení rozložení aktuální hello je hello data seskupené podle ID žádosti klienta, pak zdrojem protokolu. Vzhledem k tomu, že jsme jsou filtrování požadavků kde pole hello StatusCode obsahuje 404, uvidíme pouze hello serveru a data trasování sítě, není hello data protokolu klienta.

Následující obrázek Hello ukazuje konkrétního požadavku, kde operace získání objektu Blob poskytuje 404, protože objekt blob hello složka neexistuje. Všimněte si, že některé sloupce byly odebrány z hello standardní zobrazení v pořadí toodisplay hello relevantní data.

![Filtrované serveru a protokoly trasování sítě](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

V dalším kroku jsme budete korelovat toto ID žádosti klienta s hello klienta protokolu data toosee klienta hello jaké akce trvala při došlo k chybě hello. Můžete zobrazit nové zobrazení mřížky analýzy pro tuto relaci tooview hello klienta protokolu data, která se otevře v druhé kartě:

1. Nejdřív zkopírujte hodnotu hello hello **ClientRequestId** pole toohello schránky. To provedete tak, že vyberete buď řádek vyhledání hello **ClientRequestId** pole, pravým tlačítkem myši na hodnotu data hello a zvolením **kopie 'ClientRequestId'**.
2. Na pásu karet nástrojů hello, vyberte **nový prohlížeč**, pak vyberte **Analysis mřížky** tooopen nové kartě hello novou kartu zobrazí všechna data v souboru protokolu, bez seskupení, filtrování nebo pravidel barvy.
3. Na pásu karet nástrojů hello, vyberte **rozložení zobrazení**, pak vyberte **všechny sloupce klienta rozhraní .NET** pod hello **Azure Storage** části. Toto rozložení zobrazení zobrazuje data z hello klienta protokolu, jakož i hello protokoly trasování serveru a sítě. Ve výchozím nastavení je seřazen v hello **MessageNumber** sloupce.
4. V dalším kroku hello klienta protokolu vyhledejte ID hello klienta požadavku. Na pásu karet nástrojů hello, vyberte **najít zprávy**, pak zadání vlastního filtru na hello ID žádosti klienta v hello **najít** pole. Pro filtr hello, zadání vlastní ID žádosti klienta použijte následující syntaxi:

    ```
    *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"
    ```

Message Analyzer vyhledá a vybere první položka protokolu hello kde kritéria vyhledávání hello odpovídá hello klienta žádosti. V protokolu klienta hello, existuje několik záznamů pro každý požadavek ID klienta, tak může být vhodné toogroup je na hello **ClientRequestId** toomake pole je snazší toosee je všechny společně. Hello obrázek níže znázorňuje všechny zprávy hello v klientovi hello protokolu hello zadané ID klienta požadavku.

![Zobrazuje klienta protokolu chyb 404](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Pomocí hello data zobrazená v rozložení zobrazení hello v tyto dvě karty, můžete analyzovat toodetermine data požadavku hello co způsobilo chybu hello. Můžete také prohlédnout požadavků, které před tento jeden toosee Pokud předchozí události může vedly toohello chybu 404. Můžete například zkontrolovat položky protokolu klienta hello předcházející tento toodetermine ID žádosti klienta, zda hello blob mohla být odstraněna, nebo pokud byla chyba hello kvůli toohello klientská aplikace volání rozhraní API CreateIfNotExists na kontejner nebo objekt blob. V protokolu hello klienta, můžete najít v hello hello blob adresu **popis** pole; v serveru hello a protokoly trasování sítě, tyto informace se zobrazí v hello **Souhrn** pole.

Jakmile znáte hello adresu hello objekt blob, který poskytuje chyba hello 404, můžete dále prozkoumat. Pokud hledáte položky protokolu hello další zprávy týkající se operací na hello stejný objekt blob, můžete zkontrolovat, zda klient hello dříve odstraněny hello entity.

## <a name="analyze-other-types-of-storage-errors"></a>Analýza jiné typy chyb úložiště
Teď, když jste obeznámeni s používáním Message Analyzer tooanalyze data protokolu, můžete analyzovat jiné typy chyb pomocí zobrazení rozložení, pravidel barvy a hledání nebo filtrování. Hello tabulky níže seznamy některé problémy, může dojít k a hello kritéria filtru, která můžete použít toolocate je. Další informace o vytváření filtrů a hello Message Analyzer filtrování jazyka, najdete v části [filtrování Data zpráv](http://technet.microsoft.com/library/jj819365.aspx).

| tooInvestigate... | Použijte výraz filtru... | Výraz platí tooLog (klient, Server, sítě, všechny) |
| --- | --- | --- |
| Neočekávané zpoždění při doručování zpráv ve frontě |AzureStorageClientDotNetV4.Description obsahuje "Bude opakován operace se nezdařila." |Klient |
| Zvýšení HTTP PercentThrottlingError |HTTP. Response.StatusCode == 500 &#124; &#124; HTTP. Response.StatusCode == 503 |Síť |
| Nárůst PercentTimeoutError |HTTP. Response.StatusCode == 500 |Síť |
| Nárůst PercentTimeoutError (všechny) |* StatusCode == 500 |Všechny |
| Nárůst PercentNetworkError |AzureStorageClientDotNetV4.EventLogEntry.Level < 2 |Klient |
| HTTP 403 (zakázáno) zprávy |HTTP. Response.StatusCode == 403 |Síť |
| HTTP 404 (Nenalezeno) zprávy |HTTP. Response.StatusCode == 404 |Síť |
| 404 (všechny) |* StatusCode == 404 |Všechny |
| Sdíleného přístupového podpisu (SAS) autorizace problém |AzureStorageLog.RequestStatus == "SASAuthorizationError" |Síť |
| HTTP 409 (konflikt) zprávy |HTTP. Response.StatusCode == 409 |Síť |
| 409 (všechny) |* StatusCode == 409 |Všechny |
| Nízká PercentSuccess nebo analytics položky protokolu mít operací s stav transakce ClientOtherErrors |AzureStorageLog.RequestStatus == "ClientOtherError" |Server |
| Nagle upozornění |((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) a (AzureStorageLog.RequestPacketSize < 1460) a (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200) |Server |
| Doba v protokolech serveru a sítě |#Timestamp > = 2014-10-20T16:36:38 a #Timestamp < = 2014-10-20T16:36:39 |Server sítě |
| Rozsah čas v protokolech serveru |AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 a AzureStorageLog.Timestamp < = 2014-10-20T16:36:39 |Server |

## <a name="next-steps"></a>Další kroky
Další informace o odstraňování potíží začátku do konce scénáře ve službě Azure Storage naleznete v následujících zdrojích:

* [Monitorování, Diagnostika a řešení potíží s Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md)
* [Storage Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx)
* [Monitorování účtu úložiště v hello portálu Azure](storage-monitor-storage-account.md)
* [Přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md)
* [Microsoft Message Analyzer operační Průvodce](http://technet.microsoft.com/library/jj649776.aspx)
