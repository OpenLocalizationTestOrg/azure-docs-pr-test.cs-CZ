---
title: "sestavování řešení Batch s použitím šablony projektů Visual Studio – Azure aaaStart | Microsoft Docs"
description: "Zjistěte, jak pomocí šablony projektů Visual Studio můžete implementovat a spouštět úlohy náročné na Azure Batch."
services: batch
documentationcenter: .net
author: fayora
manager: timlt
editor: 
ms.assetid: 5e041ae2-25af-4882-a79e-3aa63c4bfb20
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a61c480ddc4dffd66c01220a137a3e852e39c338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-project-templates-toojump-start-batch-solutions"></a>Pomocí řešení Batch toojump-start šablony projektů sady Visual Studio

Hello **Správce úloh** a **šablony sady Visual Studio úloh procesoru** pro dávku zadejte kód toohelp jste tooimplement a spusťte úlohy náročné na dávce s hello alespoň úsilí. Tento dokument popisuje tyto šablony a poskytuje pokyny, jak toouse je.

> [!IMPORTANT]
> Tento článek popisuje pouze informace toothese použít dvě šablony a předpokládá, že jste obeznámeni s služba Batch hello a související tooit klíčové koncepty: fondy, výpočetní uzly, úlohy a úlohy, úkolech Správce úloh, proměnné prostředí a další důležité informace. Můžete najít další informace v [základy Azure Batch](batch-technical-overview.md), [přehled funkcí Batch pro vývojáře](batch-api-basics.md), a [Začínáme s knihovnou Azure Batch hello pro .NET](batch-dotnet-get-started.md).
> 
> 

## <a name="high-level-overview"></a>Podrobný přehled
Hello Správce úloh a úloh procesoru šablony může být použité toocreate dvě užitečné součásti:

* Úkolu Správce úloh, který implementuje rozdělovače úlohy, který můžete rozdělit úlohu na více úkolů, které můžete spustit samostatně, paralelně.
* Procesor úloh, které lze použít tooperform předběžné zpracování a po zpracování kolem příkazového řádku s aplikací.

Například v případě vykreslování film by hello úlohy rozdělovače zapněte úlohu jeden film do stovkami nebo tisíci samostatné úkoly, které by jednotlivé snímky zpracovávají odděleně. Odpovídajícím způsobem procesoru úloh hello by vyvolání hello vykreslování aplikace a všechny závislé procesy, které jsou potřebné toorender každý snímek, jakož i provádět žádné další akce (například kopírování hello vykresluje rámce tooa umístění úložiště).

> [!NOTE]
> Hello Správce úloh a úloh procesoru šablony jsou nezávisle na sobě navzájem, abyste si mohli vybrat toouse oba, nebo jenom jeden z nich, v závislosti na požadavcích hello výpočetní úlohy a vaše předvolby.
> 
> 

Jak je znázorněno v následujícím diagramu hello, bude výpočetní úlohu, která používá tyto šablony projít tří fází:

1. Kód klienta Hello (např. aplikace, webové služby atd.) odešle toohello úlohy služby Batch v Azure, a jako jeho úlohy správce úloh hello úlohy správce program.
2. Služba Batch Hello spouští hello úkolu Správce úloh na výpočetním uzlu a hello hello spustí úlohu rozdělovače zadaný počet úloh procesoru úlohy, na jako mnoho výpočetních uzlech podle potřeby, na základě specifikací v hello úlohy rozdělovače kódu a parametry hello.
3. Hello úloh procesoru úlohy spustit samostatně, paralelní, tooprocess hello vstupní data a vygenerovat výstupní data hello.

![Diagram znázorňující, jak kód klienta komunikuje s hello služby Batch][diagram01]

## <a name="prerequisites"></a>Požadavky
toouse hello Batch šablony, budete potřebovat následující hello:

* Počítač s nainstalovaná sada Visual Studio 2015. Šablony batch jsou aktuálně podporovány pouze pro Visual Studio 2015.
* Hello Batch šablony, které jsou k dispozici na hello [Galerie sady Visual Studio] [ vs_gallery] jako rozšíření Visual Studia. Existují dva způsoby tooget hello šablony:
  
  * Instalace šablony hello používá hello **rozšíření a aktualizace** dialogové okno v sadě Visual Studio (Další informace najdete v tématu [hledání a používání rozšíření Visual Studia][vs_find_use_ext]). V hello **rozšíření a aktualizace** dialogové okno, hledání a stahování hello následující dvě rozšíření:
    
    * Správce úloh Azure Batch pomocí rozdělovače úlohy
    * Procesor úlohy Azure Batch
  * Stahování šablon hello z online galerie hello pro sadu Visual Studio: [projektu šablony aplikace Microsoft Azure Batch][vs_gallery_templates]
* Pokud máte v plánu toouse hello [balíčky aplikací](batch-application-packages.md) Správce úloh hello toodeploy funkce a toohello procesoru úlohy Batch výpočetní uzly, musíte toolink tooyour účet úložiště účtu Batch.

## <a name="preparation"></a>Příprava
Doporučujeme vytvářet řešení, které může obsahovat váš správce úloh a také vaše úloha procesoru, protože to může být snazší kód tooshare mezi Správce úloh a úloh procesoru programy. toocreate toto řešení, postupujte takto:

1. Otevřete Visual Studio a vyberte **soubor** > **nový** > **projektu**.
2. V části **šablony**, rozbalte položku **jiné typy projektů**, klikněte na tlačítko **řešení sady Visual Studio**a potom vyberte **prázdného řešení**.
3. Zadejte název, který popisuje účel vaší aplikace a hello tohoto řešení (například "LitwareBatchTaskPrograms").
4. toocreate hello nové řešení, klikněte na tlačítko **OK**.

## <a name="job-manager-template"></a>Správce úloh šablony
Správce úloh šablony Hello pomáhá tooimplement úkolu Správce úloh, které můžete provádět následující akce hello:

* Úlohu rozdělte do několika úloh.
* Odešlete toorun tyto úlohy v dávce.

> [!NOTE]
> Další informace o úkolech Správce úloh najdete v tématu [přehled funkcí Batch pro vývojáře](batch-api-basics.md#job-manager-task).
> 
> 

### <a name="create-a-job-manager-using-hello-template"></a>Vytvoření správce úloh, pomocí šablony hello
tooadd řešení toohello Správce úloh, které jste vytvořili dříve, postupujte takto:

1. Otevřete existující řešení v sadě Visual Studio.
2. V Průzkumníku řešení klikněte pravým tlačítkem na řešení hello, klikněte na tlačítko **přidat** > **nový projekt**.
3. V části **Visual C#**, klikněte na tlačítko **cloudu**a potom klikněte na **Správce úloh Azure Batch pomocí úlohy rozdělovače**.
4. Zadejte název, který popisuje vaší aplikace a identifikuje tohoto projektu jako správce úloh hello (např.) "LitwareJobManager").
5. toocreate hello projektu, klikněte na tlačítko **OK**.
6. Nakonec odkazovaná sestavení hello projektu tooforce Visual Studio tooload všech balíčků NuGet a tooverify, který hello projektu je platná, než začnete, provádění úprav.

### <a name="job-manager-template-files-and-their-purpose"></a>Soubory šablon Správce úloh a jejich účel
Při vytváření projektu pomocí šablony Správce úloh hello generuje tři skupiny soubory kódu:

* hlavní programový soubor Hello (Program.cs). Tato položka obsahuje hello program Vstupní bod a nejvyšší úrovně výjimek. Neměli byste potřebovat normálně toomodify to.
* adresář Framework Hello. Tato položka obsahuje soubory hello zodpovědná za pracovní, často používaný text"hello provádí program Správce úloh hello – vybalení parametry, přidávání úlohy toohello dávkovou úlohu, atd. Neměli byste potřebovat normálně toomodify tyto soubory.
* Hello úlohy rozdělovače soubor (JobSplitter.cs). Toto je, kam jste umístili specifické pro aplikaci logiky k rozdělení úlohu do úlohy.

Samozřejmě můžete přidat další soubory jako požadované toosupport kódu rozdělovače úlohy, v závislosti na složitosti hello hello úlohu, rozdělování logiku.

Šablona Hello generuje také standardní soubory rozhraní .NET projektu, například souboru .csproj, app.config, souboru packages.config, atd.

Hello zbývající část tohoto oddílu popisuje hello různých souborů a jejich struktura kódu a vysvětluje, jaké jsou jednotlivé třídy.

![Visual Studio Průzkumník řešení zobrazující hello Správce úloh šablony řešení][solution_explorer01]

**Soubory Framework**

* `Configuration.cs`: Zapouzdří hello načítání úlohy konfiguračních dat, jako je například Podrobnosti účtu Batch, přihlašovací údaje účtu propojené úložiště, úlohy a úkolů informace a parametry úlohy. Také poskytuje přístup tooBatch definované proměnné prostředí (viz nastavení prostředí pro úkoly v dokumentaci Batch hello) prostřednictvím třídy Configuration.EnvironmentVariable hello.
* `IConfiguration.cs`: Přehledů hello implementace hello třída konfigurace, aby bylo možné jednotkové testování vaší rozdělovače úlohy pomocí falešných nebo imitované konfiguraci objektu.
* `JobManager.cs`: Orchestruje hello součásti programu Správce úloh hello. Zodpovídá za inicializaci hello úlohy rozdělovače, vyvolání hello úlohy rozdělovače, hello a odeslání úlohy hello vrácený odesílatel úlohy toohello rozdělovače úlohy hello.
* `JobManagerException.cs`: Představuje chybu, která vyžaduje hello úlohy správce tooterminate. JobManagerException je použité toowrap 'očekávání, chyby, kde lze zadat konkrétní diagnostické informace jako součást ukončení.
* `TaskSubmitter.cs`: Tato třída je odpovědný tooadding úlohy vrácený hello úlohy rozdělovače toohello dávkovou úlohu. Třída JobManager Hello agreguje hello pořadí úloh do dávek pro efektivní, ale včas přidání toohello úlohu a potom volá TaskSubmitter.SubmitTasks vlákna na pozadí pro každou dávku.

**Úloha rozdělovače**

`JobSplitter.cs`: Tato třída obsahuje konkrétní aplikace logiky k rozdělení hello úlohy do úlohy. Hello framework vyvolá hello JobSplitter.Split metoda tooobtain sekvenci úlohy, které přidá vrátí tyto adresy úlohy toohello jako metodu hello. Toto je třída hello, kde bude vložit hello logiku úlohy. Implementujte hello rozdělení metoda tooreturn posloupnost instancí CloudTask představující hello úlohy, do kterých chcete, aby úloha toopartition hello.

**Standardní soubory projektu příkazového řádku .NET**

* `App.config`: Standardní soubor konfigurace aplikace .NET.
* `Packages.config`: Standardní soubor závislost balíčku NuGet.
* `Program.cs`: Obsahuje hello program Vstupní bod a nejvyšší úrovně výjimek.

### <a name="implementing-hello-job-splitter"></a>Implementace rozdělovače úlohy hello
Když otevřete projekt šablony hello Správce úloh, bude mít hello projektu hello JobSplitter.cs soubor otevřete ve výchozím nastavení. Můžete implementovat hello rozdělení logiku pro úlohy hello v vaše úlohy pomocí hello Split() metoda zobrazit níže:

```csharp
/// <summary>
/// Gets hello tasks into which toosplit hello job. This is where you inject
/// your application-specific logic for decomposing hello job into tasks.
///
/// hello job manager framework invokes hello Split method for you; you need
/// only tooimplement it, not toocall it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and hello "yield return" statement; this allows tasks toobe added
/// and toostart running while splitting is still in progress.
/// </summary>
/// <returns>hello tasks toobe added toohello job. Tasks are added automatically
/// by hello job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for hello split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

> [!NOTE]
> Hello opatřeny poznámkami v hello části `Split()` metoda je hello pouze část hello kód šablony Správce úloh, který je určený pro toomodify můžete přidáním hello logiku toosplit úloh do různých úloh. Pokud chcete toomodify různé části hello šablony, zkontrolujte, zda jsou familiarized s fungování Batch a vyzkoušejte několik hello [ukázky kódu služby Batch][github_samples].
> 
> 

Implementace Split() má přístup k:

* Parametry úlohy prostřednictvím hello Hello `_parameters` pole.
* objekt představující úlohu hello prostřednictvím hello Hello CloudJob `_job` pole.
* objekt představující hello úkolu Správce úloh, prostřednictvím hello Hello CloudTask `_jobManagerTask` pole.

Vaše `Split()` implementace nemusí tooadd úlohy toohello úlohy přímo. Místo toho kódu by měla vrátit pořadí objektů CloudTask a tyto úlohy toohello automaticky přidá hello framework třídy, které vyvolají rozdělovače úlohy hello. Je běžné toouse C# na iterator (`yield return`) funkci příčky tooimplement úlohy, jako to umožňuje toostart hello úlohy s co možná nejdříve než čekání na všechny úlohy toobe vypočítat.

**Selhání úlohy rozdělovače**

Pokud vaše úlohy rozdělovače dojde k chybě, měli buď:

* Ukončit hello pořadí pomocí hello C# `yield break` příkaz, ve kterém bude Správce úloh případu hello jednal jako úspěšné; nebo
* Způsobí výjimku, ve kterém případu hello Správce úloh bude považován za se nezdařilo a může v závislosti na klienta hello konfiguraci ji opakovat).

V obou případech všechny úlohy již vrácený rozdělovače hello úlohy a přidané toohello dávkové úlohy bude vhodné toorun. Pokud nechcete, aby tento toohappen, pak vám může:

* Ukončit úlohu hello před návratem od hello úlohy rozdělovače
* Formulovali hello celý úkol kolekcí před jejím vrácením (tedy vrátit `ICollection<CloudTask>` nebo `IList<CloudTask>` místo implementace rozdělovače vaše úlohy pomocí iterator C#)
* Použít toomake závislosti úkolů, všechny úlohy jsou závislé na úspěšné dokončení hello hello Správce úloh

**Opakování úlohy správce**

Správce úloh hello selže, může být opakovat hello služba Batch v závislosti na nastavení opakování hello klienta. Obecně platí to je bezpečné, protože při hello framework přidá úlohy toohello úlohy, je ignorováno jakékoli úlohy, které již existují. Ale pokud výpočet úlohy je náročná, nemusí chcete tooincur hello náklady přepočítání úlohy, které již byly přidány toohello úlohy; Naopak pokud hello spusťte znovu není zaručenou toogenerate hello stejné ID úloh pak nebude nové chování 'ignorovat duplicity' hello. V těchto případech měli navrhnout vaše úlohy rozdělovače toodetect hello práci, kterou již bylo provedeno a neopakuje, například provedením CloudJob.ListTasks před zahájením tooyield úlohy.

### <a name="exit-codes-and-exceptions-in-hello-job-manager-template"></a>Ukončete kódy a výjimky v šabloně hello Správce úloh
Kódy ukončení a výjimky zadejte mechanismus toodetermine hello výsledek spuštění programu, a můžou pomoct tooidentify případné potíže s hello spuštění programu hello. Správce úloh šablony Hello implementuje hello ukončovacích kódů a výjimek popsaných v této části.

Úkolu Správce úloh, která je implementována pomocí šablony Správce úloh hello vrátit tři možné ukončovací kód:

| Kód | Popis |
| --- | --- |
| 0 |Správce úloh Hello byla úspěšně dokončena. Kód rozdělovače úloha byla spuštěna toocompletion a všechny úlohy byly přidány toohello úlohy. |
| 1 |Hello úkolu Správce úloh se nezdařilo s výjimkou v 'očekávané' součást programu hello. Výjimka Hello nebyla přeložený tooa JobManagerException s diagnostické informace a, kde je to možné, návrhy na řešení hello selhání. |
| 2 |Hello úkolu Správce úloh se nezdařilo s výjimkou 'neočekávané'. Hello výjimka zaznamenané toostandard výstup, ale Správce úloh hello byl nelze tooadd jakékoli další diagnostiky nebo nápravnou informace. |

V případě hello úlohy správce úloh selhání některé úlohy mohou stále byly přidány toohello služby před hello došlo k chybě. Tyto úlohy se spustí normálně. Najdete v části "Selhání úlohy rozdělovače" nad diskuzi o tuto cestu kódu.

Všechny informace hello vrácený výjimky jsou zapsány do stdout.txt a stderr.txt souborů. Další informace najdete v tématu [zpracování chyb](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Důležité informace o klientech
Tato část popisuje některé požadavky na implementaci klienta při vyvolání Správce úloh, na základě této šablony. V tématu [jak toopass parametry a proměnné prostředí z hello kód klienta](#pass-environment-settings) podrobnosti o předání parametry a nastavení prostředí.

**Povinné přihlašovací údaje**

V úloze pořadí tooadd úlohy toohello Azure Batch hello úkolu Správce úloh vyžaduje adresa URL účtu Azure Batch a klíč. Je nutné předat v proměnné prostředí s názvem YOUR_BATCH_URL a YOUR_BATCH_KEY. Můžete nastavit v hello Správce úloh nastavení prostředí úloh. Například v C# klienta:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Přihlašovací údaje úložiště**

Obvykle hello klient nemusí tooprovide hello propojené úložiště účet pověření toohello úkolu Správce úloh, protože správců (a) většinu úloh nepotřebují přístup tooexplicitly, hello propojený účet úložiště a (b) hello propojený účet úložiště se často poskytuje úlohy tooall jako běžné nastavení prostředí pro úlohu hello. Pokud nejsou poskytování hello propojený účet úložiště prostřednictvím hello běžných nastavení prostředí a hello Správce úloh vyžaduje přístup k úložišti toolinked, pak by měl zadat přihlašovací údaje storage hello propojené následujícím způsobem:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Nastavení úloh Správce úloh**

Hello klienta musí nastavit správce úloh hello *killJobOnCompletion* příznak příliš**false**.

Je obvykle bezpečné pro přístup z hello klienta tooset *runExclusive* příliš**false**.

Hello klient musí použít hello *resourceFiles* nebo *applicationPackageReferences* Správce úloh hello kolekce toohave je spustitelný soubor (a její požadované knihovny DLL) nasazené toohello výpočetním uzlu.

Ve výchozím nastavení nebude Správce úloh hello zopakován, pokud se nezdaří. V závislosti na logice manager úlohy hello klienta může chtít tooenable opakování prostřednictvím *omezení*/*maxTaskRetryCount*.

**Nastavení úloh**

Pokud hello úlohy rozdělovače vysílá úlohy se závislostmi, musíte nastavit hello klienta tootrue usesTaskDependencies hello úlohy.

V modelu rozdělovače úlohy hello neobvyklé, že klienti toowish tooadd úlohy toojobs nad rámec vytvoří jaké úlohu rozdělovače hello. Hello klienta by měl proto obvykle nastavit hello úlohy *onAllTasksComplete* příliš**terminatejob**.

## <a name="task-processor-template"></a>Šablony úloh procesoru
Šablony úloh procesoru vám pomůže tooimplement procesor úloh, které můžete provádět následující akce hello:

* Nastavení informací hello vyžaduje každý toorun úlohy Batch.
* Spusťte všechny akce, které vyžaduje každý úkol Batch.
* Uložte výstupy úlohy toopersistent úložiště.

I když procesor úloh není požadovaná toorun úlohy na Batch, hello klíče výhodou použití procesor úloh je, že poskytuje obálku tooimplement všechny akce provádění úkolů na jednom místě. Například pokud potřebujete toorun více aplikacemi v kontextu hello každý úkol, nebo pokud potřebujete úložiště toopersistent toocopy dat po dokončení každé úlohy.

Hello akce prováděné procesorem hello úloha může být jako jednoduché nebo komplexní a jako mnoho nebo jako několika podle požadavků vaší zatížení. Kromě toho implementací všechny akce úkolů do jednoho procesoru úloh můžete snadno aktualizovat nebo přidejte akce na základě požadavků změny tooapplications nebo zatížení. Ale v některých případech procesor úloh nemusí být hello optimální řešení týkající se vaší implementace jako ji můžete přidat nepotřebné složitost, například při spuštění úlohy, které lze rychle spustit z příkazového řádku jednoduché.

### <a name="create-a-task-processor-using-hello-template"></a>Vytvoření úloh procesor pomocí šablony hello
tooadd řešení toohello procesoru úloh, které jste vytvořili dříve, postupujte takto:

1. Otevřete existující řešení v sadě Visual Studio.
2. V Průzkumníku řešení klikněte pravým tlačítkem na řešení hello, klikněte na tlačítko **přidat**a potom klikněte na **nový projekt**.
3. V části **Visual C#**, klikněte na tlačítko **cloudu**a potom klikněte na **Azure Batch úloh procesoru**.
4. Zadejte název, který popisuje vaší aplikace a identifikuje tohoto projektu jako hello úloh procesoru (např.) "LitwareTaskProcessor").
5. toocreate hello projektu, klikněte na tlačítko **OK**.
6. Nakonec odkazovaná sestavení hello projektu tooforce Visual Studio tooload všech balíčků NuGet a tooverify, který hello projektu je platná, než začnete, provádění úprav.

### <a name="task-processor-template-files-and-their-purpose"></a>Soubory šablon procesoru úloh a jejich účel
Při vytváření projektu pomocí šablony procesoru úloh hello generuje tři skupiny soubory kódu:

* hlavní programový soubor Hello (Program.cs). Tato položka obsahuje hello program Vstupní bod a nejvyšší úrovně výjimek. Neměli byste potřebovat normálně toomodify to.
* adresář Framework Hello. Tato položka obsahuje soubory hello zodpovědná za pracovní, často používaný text"hello provádí program Správce úloh hello – vybalení parametry, přidávání úlohy toohello dávkovou úlohu, atd. Neměli byste potřebovat normálně toomodify tyto soubory.
* procesor soubor úkolů Hello (TaskProcessor.cs). Toto je, kam jste umístili logika specifické pro aplikaci pro provádění úlohy (obvykle při volání na existující spustitelný soubor tooan). Před a po zpracování kódu, třeba stahování další data nebo nahrávání souborů výsledek také zde bude.

Samozřejmě můžete přidat další soubory jako požadované toosupport kódu procesoru úloh, v závislosti na složitosti hello hello úlohu, rozdělování logiku.

Šablona Hello generuje také standardní soubory rozhraní .NET projektu, například souboru .csproj, app.config, souboru packages.config, atd.

Hello zbývající část tohoto oddílu popisuje hello různých souborů a jejich struktura kódu a vysvětluje, jaké jsou jednotlivé třídy.

![Visual Studio Průzkumníku řešení zobrazující řešení šablony úloh procesoru hello][solution_explorer02]

**Soubory Framework**

* `Configuration.cs`: Zapouzdří hello načítání úlohy konfiguračních dat, jako je například Podrobnosti účtu Batch, přihlašovací údaje účtu propojené úložiště, úlohy a úkolů informace a parametry úlohy. Také poskytuje přístup tooBatch definované proměnné prostředí (viz nastavení prostředí pro úkoly v dokumentaci Batch hello) prostřednictvím třídy Configuration.EnvironmentVariable hello.
* `IConfiguration.cs`: Přehledů hello implementace hello třída konfigurace, aby bylo možné jednotkové testování vaší rozdělovače úlohy pomocí falešných nebo imitované konfiguraci objektu.
* `TaskProcessorException.cs`: Představuje chybu, která vyžaduje hello úlohy správce tooterminate. TaskProcessorException je použité toowrap 'očekávání, chyby, kde lze zadat konkrétní diagnostické informace jako součást ukončení.

**Úloha procesoru**

* `TaskProcessor.cs`: Spustí úlohu hello. Hello framework vyvolá metodu TaskProcessor.Run hello. Toto je třída hello, kde bude vložit hello specifické pro aplikaci logiky vaše úlohy. Implementujte metodu spustit hello:
  * Analýza a ověření žádné parametry úlohy
  * Napište hello příkazového řádku pro jakýkoli externí program chcete tooinvoke
  * Přihlaste se další diagnostické informace, které můžete potřebovat pro účely ladění
  * Spustit proces, který používá tento příkazový řádek
  * Počkejte tooexit proces hello
  * Zaznamenat hello ukončovací kód procesu toodetermine hello, pokud ji byla úspěšná nebo neúspěšná
  * Výstupní soubory, které chcete uložit tookeep toopersistent úložiště

**Standardní soubory projektu příkazového řádku .NET**

* `App.config`: Standardní soubor konfigurace aplikace .NET.
* `Packages.config`: Standardní soubor závislost balíčku NuGet.
* `Program.cs`: Obsahuje hello program Vstupní bod a nejvyšší úrovně výjimek.

## <a name="implementing-hello-task-processor"></a>Implementace hello úloh procesoru
Když otevřete projekt šablony úloh procesoru hello, bude mít hello projektu hello TaskProcessor.cs soubor otevřete ve výchozím nastavení. Můžete implementovat logiku hello spuštění pro úlohy hello v vaše úlohy pomocí metody Run() hello vidíte níže:

```csharp
/// <summary>
/// Runs hello task processing logic. This is where you inject
/// your application-specific logic for decomposing hello job into tasks.
///
/// hello task processor framework invokes hello Run method for you; you need
/// only tooimplement it, not toocall it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check hello exit code of that program and
/// save output files toopersistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for hello task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
> [!NOTE]
> Hello poznámkou část v hello metodu Run() se hello pouze část hello procesoru úloh šablony kód, který je určený pro toomodify můžete přidáním hello spustit logiku pro úlohy hello vaše úlohy. Pokud chcete toomodify různé části hello šablony, prosím nejdřív Seznamte se s fungování Batch přečtení dokumentace hello Batch a vyzkoušení několik ukázek kódu služby Batch hello.
> 
> 

Hello metodu Run() je zodpovědná za spuštění hello příkazového řádku, od jednoho nebo více procesů, čekání na všechny toocomplete procesu, ukládání hello výsledky a nakonec vrácení s ukončovacím kódem. Hello metodu Run() je, kde budete implementovat hello zpracování logiku pro vaše úkoly. Hello úloh procesoru framework vyvolá metodu Run() hello vám; není nutné toocall ho sami.

Implementace Run() má přístup k:

* Parametry úlohy prostřednictvím hello Hello `_parameters` pole.
* ID úlohy a úkolů, prostřednictvím hello Hello `_jobId` a `_taskId` pole.
* konfigurace úlohy Hello, prostřednictvím hello `_configuration` pole.

**Selhání úlohy**

V případě selhání můžete k ukončení metodu Run() hello došlo k výjimce, ale zůstane obslužná rutina výjimky nejvyšší úrovně hello kontrolu nad hello úloh ukončovací kód. Pokud potřebujete toocontrol hello ukončovací kód, aby mohl rozlišit různé typy selhání, například k diagnostickým účelům nebo protože některé selhání režimy musí ukončit hello úlohy a další neměli a pak musí ukončit metodu Run() hello vrácením nenulový ukončovací kód. To se stane hello úloh ukončovací kód.

### <a name="exit-codes-and-exceptions-in-hello-task-processor-template"></a>Ukončete kódy a výjimky v šabloně úlohy procesoru hello
Kódy ukončení a výjimky zadejte mechanismus toodetermine hello výsledek spuštění programu, a můžou pomoct zjistit případné problémy s hello spuštění programu hello. šablony úloh procesoru Hello implementuje hello ukončovacích kódů a výjimek popsaných v této části.

Úloha procesoru úloh, která je implementována pomocí šablony úloh procesoru hello vrátit tři možné ukončovací kód:

| Kód | Popis |
| --- | --- |
| [Process.ExitCode][process_exitcode] |procesor úloh Hello spustili toocompletion. Všimněte si, že to neznamená, že tento program hello, vyvolaná proběhla úspěšně – tento hello úloh procesor úspěšně volat a provést jakékoli po zpracování bez výjimek. Hello význam hello ukončovací kód závisí na programu hello vyvolat – obvykle ukončovací kód 0 znamená programu hello úspěšná a znamená jiných ukončovací kód programu hello se nezdařilo. |
| 1 |Hello úloh procesoru se nezdařilo s výjimkou v 'očekávané' součást programu hello. Hello se výjimka přeložený tooa `TaskProcessorException` s diagnostické informace a, kde je to možné, návrhy na řešení hello selhání. |
| 2 |Hello úloh procesoru se nezdařilo s 'neočekávané' výjimky. Hello výjimka zaznamenané toostandard výstup, ale hello úloh procesor byl nelze tooadd jakékoli další diagnostiky nebo nápravnou informace. |

> [!NOTE]
> Pokud program hello vyvolání používá ukončovací kódy 1 a 2 tooindicate konkrétní chyby režimy, pak pomocí ukončovací kód 1 a 2 pro procesoru chybám úloh je nejednoznačný. Tyto úlohy procesoru kódy toodistinctive ukončovací kódy chyb můžete změnit úpravou hello případů výjimka v souboru Program.cs hello.
> 
> 

Všechny informace hello vrácený výjimky jsou zapsány do stdout.txt a stderr.txt souborů. Další informace najdete v tématu zpracování chyb, v hello Batch dokumentaci.

### <a name="client-considerations"></a>Důležité informace o klientech
**Přihlašovací údaje úložiště**

Pokud procesor úloh používá výstupy toopersist úložiště objektů blob v Azure, například pomocí hello souboru konvence pomocné knihovny, pak potřebuje přístup příliš*buď* hello přihlašovacích údajů účtu úložiště cloudu *nebo* adresu URL kontejneru objektů blob, která zahrnuje sdílený přístupový podpis (SAS). Šablona Hello zahrnuje podporu poskytování pověření prostřednictvím společné proměnné prostředí. Váš klient může předat hello přihlašovacích údajů úložiště následujícím způsobem:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

účet úložiště Hello je pak k dispozici v hello TaskProcessor třída prostřednictvím hello `_configuration.StorageAccount` vlastnost.

Pokud dáváte přednost toouse URL adresa kontejneru s tokenem SAS, můžete to taky předat prostřednictvím nastavení prostředí běžné úlohy, ale hello úloh procesoru šablona neobsahuje aktuálně integrovanou podporu pro tento.

**Instalační program úložiště**

Doporučuje se, že hello klienta nebo úloha úkol Správce úloh vytvořit žádné kontejnery požadované úlohami před přidáním hello úlohy toohello úlohy. Toto je povinné, že pokud použijete adresu URL kontejneru s tokenem SAS, jako například adresu URL nezahrnuje oprávnění toocreate hello kontejneru. I v případě, že předáváte přihlašovacích údajů účtu úložiště, doporučujeme jako ukládá každý úkol s toocall CloudBlobContainer.CreateIfNotExistsAsync na kontejneru hello.

## <a name="pass-parameters-and-environment-variables"></a>Předávání parametrů a proměnných prostředí
### <a name="pass-environment-settings"></a>Předat nastavení prostředí
Klienta můžete předat informace toohello úkolu Správce úloh v podobě hello nastavení prostředí. Tyto informace pak může být použit hello úkol správce při generování hello úloh procesoru úlohy, které se spustí jako součást hello výpočetní úlohy. Příklady hello informace, které můžete předat jako nastavení prostředí:

* Klíče pro účet a název účtu úložiště
* Adresa URL účtu batch
* Klíč účtu batch

Hello služba Batch má jednoduché mechanismus toopass prostředí nastavení tooa úkol správce pomocí hello `EnvironmentSettings` vlastnost [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Například tooget hello `BatchClient` instance pro účet Batch můžete předat jako proměnné prostředí z klienta hello kód hello adresy URL a sdíleného klíče přihlašovací údaje pro účet Batch hello. Podobně tooaccess hello úložiště účtu, který je propojený účet Batch toohello, můžete předat hello název účtu úložiště a klíč účtu úložiště hello jako proměnné prostředí.

### <a name="pass-parameters-toohello-job-manager-template"></a>Předávání parametrů toohello Správce úloh šablony
V mnoha případech je užitečné toopass na úlohu parametry toohello úkolu Správce úloh, buď úlohu hello toocontrol rozdělení procesu nebo hello tooconfigure hello úlohy pro úlohu. To provedete tím, že nahrajete soubor JSON s názvem parameters.JSON tímto kódem jako soubor prostředků pro hello úkolu Správce úloh. Parametry Hello se pak můžou stát k dispozici v hello `JobSplitter._parameters` pole v šabloně hello Správce úloh.

> [!NOTE]
> Obslužná rutina předdefinované parametru Hello podporuje pouze slovník řetězec řetězec. Pokud chcete toopass komplexní JSON hodnoty jako hodnoty parametrů, bude nutné toopass jako řetězce a analyzovat je ve hello úlohy rozdělovače ani upravovat hello framework `Configuration.GetJobParameters` metoda.
> 
> 

### <a name="pass-parameters-toohello-task-processor-template"></a>Předávání parametrů toohello procesoru úloh šablony
Můžete také předat parametry úlohy tooindividual implementovaná pomocí šablony úloh procesoru hello. Stejně jako s hello šablonu Správce úloh, hello úloh procesoru šablona hledá soubor prostředků s názvem

Parameters.JSON tímto kódem a pokud zjistila, že ho načte jako slovník parametrů hello. Existuje několik možností pro jak toopass parametry toohello procesoru úkoly úloh:

* Znovu použít parametry úlohy hello JSON. Tento postup funguje dobře v případě, že jsou jenom parametry hello ty úlohy celou (pro příklad, vykreslení výška a šířka). toodo, při vytváření CloudTask v hello úlohy rozdělovače, přidání objektu odkaz toohello Parameters.JSON tímto kódem prostředků souboru z hello úkolu Správce úloh je ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) kolekce ResourceFiles toohello CloudTask.
* Generování a nahrávání dokumentu Parameters.JSON tímto kódem specifických úkolů při provádění úlohy rozdělovače a odkazovat na tento objekt blob v kolekci souborů prostředků hello úloh. To je nezbytné, pokud jiné úlohy s odlišnými parametry. Příkladem může být 3D vykreslování scénář, kde je index rámečku hello předán toohello úloh jako parametr.

> [!NOTE]
> Obslužná rutina předdefinované parametru Hello podporuje pouze slovník řetězec řetězec. Pokud chcete toopass komplexní JSON hodnoty jako hodnoty parametrů, bude nutné toopass jako řetězce a analyzovat je procesor úloh hello ani upravovat hello framework `Configuration.GetTaskParameters` metoda.
> 
> 

## <a name="next-steps"></a>Další kroky
### <a name="persist-job-and-task-output-tooazure-storage"></a>Zachovat úlohy a úkolů tooAzure výstup úložiště
Další užitečné nástroje při vývoji řešení Batch je [Azure Batch souboru konvence][nuget_package]. Pomocí této knihovně tříd rozhraní .NET (momentálně ve verzi preview) v úložišti tooeasily aplikací Batch .NET a načíst tooand výstupy úlohy ze služby Azure Storage. [Zachovat výstup úlohy a úlohy Azure Batch](batch-task-output.md) obsahuje úplnou diskusi hello knihovně a jeho použití.

### <a name="batch-forum"></a>Fórum batch
Hello [fóru služby Azure Batch] [ forum] na webu MSDN je skvělá umístit toodiscuss Batch a klást otázky týkající se služby hello. HEAD na přes pro užitečné "rychlé" příspěvky a při jejich vzniku při sestavování řešení Batch zveřejněte svoje otázky.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
