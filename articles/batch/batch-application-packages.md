---
title: "aaaInstall balíčky aplikací na výpočetní uzly - Azure Batch | Microsoft Docs"
description: "Použití hello aplikace balíčky funkcí Azure Batch tooeasily spravovat více aplikací a verze pro instalaci na Batch výpočetních uzlů."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 683be7b7f1bd5db7835332016f6dccb72f45c3b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-applications-toocompute-nodes-with-batch-application-packages"></a>Nasaďte uzly toocompute aplikací pomocí balíčků aplikací Batch

Funkce balíčky aplikace Hello služby Azure Batch poskytuje snadnou správu úloh aplikací a jejich nasazení toohello výpočetní uzly ve fondu. Pomocí balíčků aplikací můžete odeslat a spravovat více verzí hello aplikací, které vaše úkoly spouštět, včetně jejich podpůrné soubory. Pak můžete automaticky nasadit jednu nebo více z těchto aplikací toohello výpočetní uzly ve fondu.

V tomto článku se dozvíte, jak tooupload a správě balíčků aplikací v nástroji hello portálu Azure. Potom se dozvíte, jak tooinstall je ve fondu na výpočetní uzly s hello [Batch .NET] [ api_net] knihovny.

> [!NOTE]
> 
> Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017. Že jsou podporované v fondy Batch vytvořit až 10. března 2016 5 2017 července pouze v případě, že hello fondu byla vytvořena pomocí konfigurace cloudové služby. Fondy batch vytvořen předchozí too10. března 2016 nepodporují balíčky aplikací.
>
> Hello rozhraní API pro vytváření a správě balíčků aplikací jsou součástí hello [rozhraní Batch Management .NET] [[api_net_mgmt]] knihovny. Hello rozhraní API pro instalaci balíčků aplikací na výpočetním uzlu jsou součástí hello [Batch .NET] [ api_net] knihovny.  
>
> Funkce balíčky aplikace Hello zde popsané nahrazuje funkce aplikace Batch hello k dispozici v předchozích verzích služby hello.
> 
> 

## <a name="application-package-requirements"></a>Požadavky na balíček aplikace
balíčky aplikací toouse, budete potřebovat příliš[propojení účtu Azure Storage](#link-a-storage-account) tooyour účtu Batch.

Tato funkce byla zavedena v [Batch REST API] [ api_rest] verze 2015-12-01.2.2 a odpovídající hello [Batch .NET] [ api_net] knihovní verze 3.1.0. Doporučujeme vám, že vždy používáte nejnovější verzi rozhraní API hello při práci se službou Batch.

> [!NOTE]
> Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017. Že jsou podporované v fondy Batch vytvořit až 10. března 2016 5 2017 července pouze v případě, že hello fondu byla vytvořena pomocí konfigurace cloudové služby. Fondy batch vytvořen předchozí too10. března 2016 nepodporují balíčky aplikací.
>
>

## <a name="about-applications-and-application-packages"></a>O aplikacích a balíčky aplikací
V rámci Azure Batch *aplikace* odkazuje tooa sadu verzí binární soubory, které se dají automaticky stažené toohello výpočetních uzlů ve fondu. *Balíčku aplikace* odkazuje tooa *konkrétní sadu* těchto binárních souborů a představuje danou *verze* aplikace hello.

![Vysokoúrovňový diagram aplikace a balíčky aplikací][1]

### <a name="applications"></a>Aplikace
Aplikace ve službě Batch obsahuje jeden nebo více aplikací, balíčků a určuje možnosti konfigurace pro aplikaci hello. Aplikace můžete například zadat hello výchozí aplikace balíčku verze tooinstall na výpočetní uzly a zda může být jeho balíčky aktualizovat ani odstranit.

### <a name="application-packages"></a>Balíčky aplikací
Balíček aplikace je soubor .zip, který obsahuje binární soubory aplikace hello a podpůrné soubory, které jsou požadovány pro vaše aplikace hello toorun úlohy. Každý balíček aplikace představuje určitou verzi aplikace hello.

Můžete zadat balíčky aplikací na úrovních hello fondu a úloh. Nejméně jeden z těchto balíčků a (volitelně) verzi můžete zadat při vytváření fondu nebo úloh.

* **Fond aplikací balíčky** nasazených příliš*každých* uzlu ve fondu hello. Jestliže se uzel připojí fondu, a, pokud je restartovat nebo obnovit z Image se aplikace nasadí.
  
    Balíčky fondu aplikací jsou vhodné, když všechny uzly v rámci fondu spuštění úlohy. Jeden nebo více balíčků aplikací můžete určit, když vytvoříte fond, a můžete přidat nebo aktualizovat existující fond balíčky. Pokud aktualizujete balíčky existující fond aplikací, je nutné restartovat nový balíček jeho uzly tooinstall hello.
* **Úloha balíčky aplikací** nasazených jen tooa výpočetním uzlu naplánované toorun úlohy, právě před spuštěním příkazového řádku úkolu hello. Pokud hello zadaný balíček aplikace a verze, který již je na uzlu hello, není znovu nasazena a slouží hello existující balíček.
  
    Balíčky aplikací úloh jsou užitečné v sdílený fond prostředí, kde různé úlohy se spouštějí na jeden fond, a hello fondu se neodstraní po dokončení úlohy. Pokud vaše úlohy má méně úloh než uzly ve fondu hello, balíčky aplikací úloh Minimalizovat přenos dat vzhledem k tomu, že je vaše aplikace nasazené toohello pouze uzly, které využívají úlohy.
  
    Další scénáře, které můžete využít balíčky aplikací úloh jsou úlohy, které spouštějí rozsáhlé aplikace, ale pouze několik úkolů. Předběžné zpracování fáze nebo sloučení úlohy, kde je aplikace hello předzpracováním nebo sloučená těžký, může například těžit z pomocí balíčků aplikací úloh.

> [!IMPORTANT]
> Existují omezení počtu hello aplikace a balíčky aplikací v rámci účtu Batch a na velikost balíčku maximální aplikace hello. V tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md) podrobnosti o těchto omezeních.
> 
> 

### <a name="benefits-of-application-packages"></a>Výhody balíčky aplikací
Balíčky aplikací můžete zjednodušit kód hello v řešení Batch a nižší hello režijní požadované toomanage hello aplikace, které běží vaše úkoly.

Pomocí balíčků aplikací váš fond spouštěcí úkol nemá toospecify dlouhý seznam tooinstall soubory jednotlivých prostředků na uzlech hello. Nemáte toomanually spravovat více verzí soubory aplikace v Azure Storage nebo na uzly. A nepotřebujete tooworry o generování [adresy URL SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide přístup k souborům toohello ve vašem účtu úložiště. Batch funguje hello pozadí pomocí balíčků aplikací Azure Storage toostore a nasadit je toocompute uzlů.

> [!NOTE] 
> Hello celková velikost spouštěcí úkol musí být menší než nebo rovna too32768 znaků, včetně zdrojových souborů a proměnných prostředí. Pokud spouštěcí úkol překračuje tento limit, pak pomocí balíčků aplikací je jinou možnost. Můžete také vytvořit komprimované archivu obsahující soubory prostředků, nahrajte ho jako objekt blob tooAzure úložiště a pak ho rozbalte z hello příkazový řádek spouštěcího úkolu. 
>
>

## <a name="upload-and-manage-applications"></a>Odesílat a spravovat aplikace
Můžete použít hello [portál Azure] [ portal] nebo hello [rozhraní Batch Management .NET](batch-management-dotnet.md) balíčky aplikací hello knihovně toomanage v účtu Batch. Hello v další části několik, nejprve ukazuje, jak toolink účtu úložiště, pak popisují přidáním aplikací a balíčků a jejich s správě hello portálu.

### <a name="link-a-storage-account"></a>Odkaz účet úložiště
toouse balíčky aplikací, je nutné nejprve propojit tooyour účet Azure Storage účtu Batch. Pokud ještě nemáte nakonfigurovaný účet úložiště, hello portál Azure zobrazí upozornění hello poprvé, klikněte na tlačítko hello **aplikace** dlaždici v hello **účet Batch** okno.

> [!IMPORTANT]
> Batch aktuálně podporuje *pouze* hello **pro obecné účely** typ účtu úložiště, jak je popsáno v kroku 5, [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account)v [o Azure účty úložiště](../storage/common/storage-create-storage-account.md). Při propojení tooyour účtu Azure Storage účtu Batch, propojte *pouze* **pro obecné účely** účet úložiště.
> 
> 

![Upozornění: nakonfigurován žádný účet úložiště, na portálu Azure][9]

Hello hello používá služba Batch související toostore účet úložiště balíčky aplikací. Po propojili jste hello dva účty Batch můžete automaticky nasadit balíčky hello uložené v hello propojené úložiště účet tooyour výpočetních uzlů. Klikněte na tlačítko toolink tooyour účet úložiště účtu Batch, **nastavení účtu úložiště** na hello **upozornění** okna a potom klikněte na **účet úložiště** na hello **Účet úložiště** okno.

![Zvolte okně účtu úložiště na portálu Azure][10]

Doporučujeme vytvořit účet úložiště *konkrétně* pro použití s vaším účtem Batch a vyberte ho sem. Podrobnosti o toocreate účtu úložiště, najdete v části "Vytvoření účtu úložiště" v [účty Azure Storage](../storage/common/storage-create-storage-account.md). Po vytvoření účtu úložiště, pak můžete propojit se účtu Batch tooyour pomocí hello **účet úložiště** okno.

> [!WARNING]
> Hello služba Batch používá Azure Storage toostore balíčky aplikací jako objekty BLOB bloku. Jste [účtován jako normální] [ storage_pricing] pro data objektů blob bloku hello. Zda tooconsider hello velikost a počet balíčky aplikací a pravidelně odstraňuje zastaralá balíčky toominimize náklady.
> 
> 

### <a name="view-current-applications"></a>Zobrazit aktuální aplikace
tooview hello aplikace v účtu Batch, klikněte na tlačítko hello **aplikace** položka nabídky v levé nabídce hello při zobrazení hello **účet Batch** okno.

![Dlaždice aplikace][2]

Výběrem této možnosti nabídky otevře hello **aplikace** okno:

![Seznam aplikací][3]

Hello **aplikace** zobrazí okno hello ID každé aplikace ve vašem účtu a hello následující vlastnosti:

* **Balíčky**: hello číslo verze přidružené k této aplikaci.
* **Výchozí verze**: verze aplikace hello nainstalovat, pokud neuvedete na verzi, když zadáte hello aplikací pro fond. Toto nastavení je volitelné.
* **Povolit aktualizace**: hello hodnotu, která určuje, zda balíček aktualizace, odstranění a přidání jsou povoleny. Pokud je toto nastaveno příliš**ne**, jsou pro aplikaci hello zakázány balíček aktualizace a odstranění. Můžete přidat pouze nové verze balíčku aplikace. Výchozí hodnota Hello je **Ano**.

### <a name="view-application-details"></a>Zobrazení podrobností o aplikaci
okno hello tooopen, která zahrnuje hello podrobnosti pro aplikace, vyberte hello aplikaci v hello **aplikace** okno.

![Podrobnosti o aplikaci][4]

V okně podrobností aplikace hello můžete nakonfigurovat následující nastavení pro vaše aplikace hello.

* **Povolit aktualizace**: Určete, zda jeho balíčky aplikací můžete aktualizovat nebo odstranit. Později v tomto článku najdete v části "Aktualizace nebo odstranění balíčku aplikace".
* **Výchozí verze**: určit výchozí aplikace balíčku toodeploy toocompute uzlů.
* **Zobrazovaný název**: Zadejte popisný název, který dávku řešení můžete použít při zobrazuje informace o aplikaci hello, například v hello uživatelského rozhraní služby, které poskytujete tooyour zákazníkům prostřednictvím Batch.

### <a name="add-a-new-application"></a>Přidejte novou aplikaci
toocreate novou aplikaci, přidejte balíček aplikace a zadejte ID aplikace nové, jedinečné. Hello první balíček aplikace, které přidáte s novým ID aplikace hello také vytvoří novou aplikaci hello.

Klikněte na tlačítko **přidat** na hello **aplikace** okno tooopen hello **novou aplikaci** okno.

![Nové okno aplikace na portálu Azure][5]

Hello **novou aplikaci** okno poskytuje následující hello polí toospecify hello nastavení nové aplikace a balíček aplikace.

**Id aplikace**

Toto pole určuje hello ID novou aplikaci, která je subjektu toohello standardní ID dávky Azure ověřovacích pravidel. Hello pravidla pro zajištění ID aplikací jsou následující:

* Na uzlech Windows hello ID může obsahovat libovolnou kombinaci alfanumerických znaků, pomlčky a podtržítka. Na uzlech Linux jsou povoleny pouze alfanumerické znaky a podtržítka.
* Nesmí obsahovat víc než 64 znaků.
* Musí být jedinečný v rámci hello účtu Batch.
* Je zachována a velká a malá písmena.

**Verze**

Toto pole určuje hello verzi balíčku aplikace hello, který ukládáte. Verze řetězce jsou subjektu toohello následující pravidla ověřování:

* Na uzlech Windows hello řetězec verze může obsahovat libovolnou kombinaci alfanumerických znaků, pomlčky, podtržítka a tečky. Na uzlech Linux řetězec verze hello může obsahovat pouze alfanumerické znaky a podtržítka.
* Nesmí obsahovat víc než 64 znaků.
* Musí být jedinečný v rámci aplikace hello.
* Jsou zachována a velká a malá písmena.

**Balíček aplikace**

Toto pole určuje hello soubor .zip, který obsahuje binární soubory aplikace hello a podpůrné soubory, které jsou požadované tooexecute hello aplikace. Klikněte na tlačítko hello **vyberte soubor** pole nebo hello tooand toobrowse ikonu složky, vyberte soubor .zip, který obsahuje soubory aplikace.

Jakmile vyberete soubor, klikněte na tlačítko **OK** toobegin hello nahrávání tooAzure úložiště. Po dokončení operace nahrávání hello hello portál zobrazí oznámení a zavře okno hello. V závislosti na velikosti hello hello souboru se odesílání a hello rychlost síťového připojení může tato operace chvíli trvat.

> [!WARNING]
> Nezavírejte stránku hello **novou aplikaci** okno před dokončením operace nahrávání hello. Díky tomu se zastaví proces odesílání hello.
> 
> 

### <a name="add-a-new-application-package"></a>Přidat nový balíček aplikace
tooadd novou verzi balíčku aplikace pro existující aplikace, vyberte aplikaci v hello **aplikace** okně klikněte na tlačítko **balíčky**, pak klikněte na tlačítko **přidat** tooopen Hello **přidat balíček** okno.

![Přidejte balíček okna aplikací na portálu Azure][8]

Jak vidíte, hello pole shodují s těmi hello **novou aplikaci** okno, ale hello **id aplikace** pole je zakázána. Stejně jako u nové aplikace hello zadejte hello **verze** pro nový balíček, procházet tooyour **balíčku aplikace** .zip souboru a pak klikněte na **OK** tooupload hello balíček.

### <a name="update-or-delete-an-application-package"></a>Aktualizace nebo odstranění balíčku aplikace
tooupdate nebo odstranit, klikněte na existující balíček aplikace, otevřete hello okno Podrobnosti pro aplikace hello **balíčky** tooopen hello **balíčky** okně klikněte na tlačítko hello **třemi tečkami**v řádku hello balíčku aplikace hello má toomodify a vyberte hello akce, které chcete tooperform.

![Aktualizace nebo odstranění balíčku na portálu Azure][7]

**Aktualizace**

Když kliknete na tlačítko **aktualizace**, hello *balíček aktualizace* zobrazí se okno. Toto okno je podobné toohello *nový balíček aplikace* okno, ale pouze pole výběr balíčku hello je povoleno, což vám toospecify nové tooupload souboru ZIP.

![Okno balíček aktualizace na portálu Azure][11]

**Odstranění**

Když kliknete na tlačítko **odstranit**, se zobrazí výzva tooconfirm hello odstranění verze balíčku hello a Batch odstraní hello balíček z úložiště Azure. Pokud odstraníte hello výchozí verze aplikace, hello **výchozí verze** je odebrat nastavení pro aplikace hello.

![Odstranit aplikaci][12]

## <a name="install-applications-on-compute-nodes"></a>Instalace aplikací na výpočetní uzly
Teď, když jste se naučili, jak balíčky toomanage aplikace s hello portálu Azure, můžete probereme jak toodeploy je toocompute uzlů a jejich spuštění s úkoly služby Batch.

### <a name="install-pool-application-packages"></a>Instalovat balíčky fondu aplikací
tooinstall balíček aplikace na všech výpočetních uzlů ve fondu, zadejte balíček aplikace jeden nebo více *odkazy* hello fondu. Hello balíčky aplikací, které zadáte pro fond jsou nainstalované na každém výpočetním uzlu, pokud takový uzel připojí hello fondu a pokud je uzel hello restartovat nebo obnovit z Image.

V Batch .NET, zadejte jednu nebo více [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] při vytvoření nového fondu, nebo pro existující fond. Hello [ApplicationPackageReference] [ net_pkgref] třída určuje ID aplikace a verze tooinstall fondu výpočetních uzlů.

```csharp
// Create hello unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify hello application and version tooinstall on hello compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit hello pool so that it's created in hello Batch service. As hello nodes join
// hello pool, hello specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> Pokud z nějakého důvodu selže nasazení balíček aplikace, značky služby Batch hello hello uzlu [nepoužitelná][net_nodestate], a žádné úlohy jsou naplánovány pro spuštění v tomto uzlu. V takovém případě byste měli **restartujte** hello uzlu tooreinitiate hello balíčku nasazení. Restartování uzlu hello taky umožňuje znovu plánování úkolů na uzlu hello.
> 
> 

### <a name="install-task-application-packages"></a>Instalovat balíčky aplikace úkolů
Podobně jako tooa fondu, zadejte balíček aplikace *odkazy* pro úlohu. Když je úkol naplánované toorun na uzlu, balíček hello stažena a extrahována těsně před hello úloh příkazový řádek se spustí. Pokud zadaného balíčku a verze je již nainstalován v uzlu hello, nebude stažen hello balíček a se používá existující balíček hello.

tooinstall balíček aplikace úlohy, konfigurovat úlohy hello [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] vlastnost:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-hello-installed-applications"></a>Spouštění aplikací hello nainstalován
Hello balíčky, které jste určili pro fond nebo úloh jsou stažené a rozbalené tooa s názvem adresáře v rámci hello `AZ_BATCH_ROOT_DIR` hello uzlu. Batch vytvoří také proměnná prostředí, který obsahuje toohello hello cestu s názvem adresáře. Vaše příkazové řádky úkolu pomocí této proměnné prostředí při odkazování na aplikace hello na uzlu hello. 

V uzlů Windows hello proměnné se v hello následující formát:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

Na uzlech Linux formátu hello se mírně liší. Tečky (.) a pomlčky (-) a tyto znaky (#) jsou plochou toounderscores v proměnné prostředí hello. Například:

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

`APPLICATIONID`a `version` jsou hodnoty, které odpovídají toohello aplikace a verze balíčku jste určili pro nasazení. Například pokud jste zadali, že verze 2.7 aplikace *digestoru* by měly být nainstalovány na systému Windows se vaše příkazové řádky úkolu využije tento tooaccess proměnné prostředí jeho soubory:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Na Linuxových uzlů zadejte proměnnou prostředí hello v tomto formátu:

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

Při nahrávání balíčku aplikace, můžete zadat, výchozí verze toodeploy tooyour výpočetních uzlů. Pokud jste zadali výchozí verze pro aplikaci, můžete vynechat hello verze příponu když odkazujete aplikace hello. Verze aplikace hello výchozí můžete zadat v hello portál Azure, v okně aplikace hello, jak je znázorněno v [odesílat a spravovat aplikace](#upload-and-manage-applications).

Například pokud nastavíte jako hello výchozí verze pro aplikace "2.7" *digestoru*a vaše úkoly odkazovat hello následující proměnné prostředí, pak uzlů Windows, budou spuštěny verze 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Hello následující fragment kódu ukazuje příklad úloh příkazový řádek, který spouští hello výchozí verzi hello *digestoru* aplikace:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> V tématu [nastavení prostředí pro úkoly](batch-api-basics.md#environment-settings-for-tasks) v hello [přehled funkcí Batch](batch-api-basics.md) Další informace o nastavení prostředí výpočetního uzlu.
> 
> 

## <a name="update-a-pools-application-packages"></a>Aktualizace balíčků aplikací fondu
Pokud se balíček aplikace již byla nakonfigurována existujícího fondu, můžete zadat nový balíček pro fond hello. Pokud zadáte nový odkaz na balíček pro fond, platí následující hello:

* Hello služba Batch nainstaluje hello nově zadaný balíček na všechny nové uzly, které připojí hello fondu a na všechny existující uzlu, který je restartovat nebo obnovit z Image.
* Výpočetní uzly, které jsou už ve fondu hello při aktualizaci hello balíček odkazuje neinstalujte automaticky hello nový balíček aplikace. Tyto výpočetní uzly, je nutné restartovat nebo přeinstalovanou tooreceive hello nový balíček.
* Po nasazení nového balíčku projeví hello vytvoření proměnné prostředí balíček odkazuje nové aplikace hello.

V tomto příkladu má existující fond hello 2.7 verzi hello *digestoru* aplikace konfigurovaná jako jeden z jeho [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref]. uzly fondu hello tooupdate s verzí 2.76b, zadejte nový [ApplicationPackageReference] [ net_pkgref] s novou verzí hello a potvrzení změn hello.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Teď, když hello novou verzi byl nakonfigurován, hello služba Batch nainstaluje verzi 2.76b tooany *nové* uzlu, který připojí hello fondu. tooinstall 2.76b na hello uzlech, jež jsou *již* ve fondu hello restartovat nebo obnovit z Image je. Všimněte si, že restartovaný uzly zachovat hello soubory z předchozích nasazení balíčku.

## <a name="list-hello-applications-in-a-batch-account"></a>Seznam hello aplikace v účtu Batch
Můžete vytvořit seznam hello aplikace a jejich balíčky v účtu Batch pomocí hello [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] metoda.

```csharp
// List hello applications and their application packages in hello Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Zabalení
Pomocí balíčků aplikací můžete pomoct vašim zákazníkům vyberte hello aplikací pro svou práci a zadejte hello přesnou verzi toouse při zpracování úlohy se služby podporují službu Batch. Také můžete zadat hello možnost pro vaše zákazníky tooupload a sledovat své vlastní aplikace ve službě.

## <a name="next-steps"></a>Další kroky
* Hello [Batch REST API] [ api_rest] taky poskytuje podporu toowork pomocí balíčků aplikací. Například v tématu hello [applicationPackageReferences] [ rest_add_pool_with_packages] element v [přidat účet fondu tooan] [ rest_add_pool] informace o toospecify balíčky tooinstall pomocí hello REST API. V tématu [aplikace] [ rest_applications] podrobnosti o tom, jak informace o aplikaci tooobtain pomocí hello Batch REST API.
* Zjistěte, jak tooprogrammatically [spravovat účty Azure Batch a kvóty pomocí rozhraní Batch Management .NET](batch-management-dotnet.md). Hello [rozhraní Batch Management .NET][api_net_mgmt] knihovny můžete povolit funkce vytváření a odstraňování účtu Batch aplikace nebo služby.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/en-us/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Vysokoúrovňový diagram balíčky aplikací"
[2]: ./media/batch-application-packages/app_pkg_02.png "Dlaždice aplikace na portálu Azure"
[3]: ./media/batch-application-packages/app_pkg_03.png "Okno aplikace na portálu Azure"
[4]: ./media/batch-application-packages/app_pkg_04.png "Okno podrobností aplikace v portálu Azure"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nové okno aplikace na portálu Azure"
[7]: ./media/batch-application-packages/app_pkg_07.png "Aktualizace nebo odstranění balíčků rozevírací seznam v portálu Azure"
[8]: ./media/batch-application-packages/app_pkg_08.png "Okno nový balíček aplikace na portálu Azure"
[9]: ./media/batch-application-packages/app_pkg_09.png "Žádná propojené výstraha účtu úložiště"
[10]: ./media/batch-application-packages/app_pkg_10.png "Zvolte okně účtu úložiště na portálu Azure"
[11]: ./media/batch-application-packages/app_pkg_11.png "Okno balíček aktualizace na portálu Azure"
[12]: ./media/batch-application-packages/app_pkg_12.png "Odstranit balíček dialogové okno potvrzení na portálu Azure"
