## <a name="set-up-your-development-environment"></a>Nastavení vývojového prostředí
Dále nastavte vývojového prostředí v sadě Visual Studio, takže jste připravené tootry hello příklady kódu v této příručce.

### <a name="create-a-windows-console-application-project"></a>Vytvoření projektu konzolové aplikace pro Windows
V sadě Visual Studio vytvořte novou konzolovou aplikaci pro Windows. Hello následující kroky vám ukážou, jak toocreate konzolovou aplikaci v sadě Visual Studio 2017, ale hello kroky jsou podobné v jiných verzích sady Visual Studio.

1. Vyberte **Soubor**  >  **Nový**  >  **Projekt**.
2. Vyberte **Instalováno** > **Šablony** > **Visual C#** > **Klasická plocha Windows**.
3. Vyberte **Aplikace konzoly (.NET Framework)**.
4. Zadejte název pro vaši aplikaci ve hello **název:** pole
5. Vyberte **OK**.

![Dialogové okno vytvoření projektu v sadě Visual Studio](./media/storage-development-environment-include/storage-development-environment-include-1.png)

Všechny příklady kódu v tomto kurzu lze přidat toohello `Main()` metoda konzolové aplikace `Program.cs` souboru.

Hello Klientská knihovna pro úložiště Azure můžete použít v libovolného typu aplikace .NET, včetně Azure cloud service nebo webovou aplikaci a stolní počítače a mobilní aplikace. V této příručce použijeme konzolovou aplikaci kvůli zjednodušení.

### <a name="use-nuget-tooinstall-hello-required-packages"></a>Použití NuGet tooinstall hello požadované balíčky
Existují dva balíčky, které potřebujete tooreference ve vašem projektu toocomplete v tomto kurzu:

* [Microsoft Azure Storage Client Library pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Tento balíček zajišťuje programový přístup toodata prostředky ve vašem účtu úložiště.
* [Microsoft Azure Configuration Manager library for .NET:](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) Tento balíček poskytuje třídu pro potřeby analýzy připojovacího řetězce v konfiguračním souboru bez ohledu na to, kde je aplikace spuštěná.

Můžete vytvořit NuGet tooobtain oba balíčky. Postupujte následovně:

1. Klikněte v **Průzkumníku řešení** pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet**.
2. Vyhledejte online text "WindowsAzure.Storage" a klikněte na tlačítko **nainstalovat** tooinstall hello Klientská knihovna pro úložiště a jeho závislosti.
3. Online hledání "WindowsAzure.ConfigurationManager" a klikněte na tlačítko **nainstalovat** tooinstall hello Azure Configuration Manager.

> [!NOTE]
> balíček knihovny klienta úložiště Hello je také součástí hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/). Doporučujeme však také nainstalovat hello Klientská knihovna pro úložiště z NuGet tooensure si vždy hello nejnovější verzi klientské knihovny hello.
> 
> závislosti ODataLib Hello v hello Klientská knihovna pro úložiště pro .NET jsou vyřešeny balíčky ODataLib hello k dispozici na NuGet, nikoli z datových služeb WCF. Hello knihovny ODataLib můžete stáhnout přímo nebo je odkazované ve vašem kódovém projektu prostřednictvím balíčku NuGet. konkrétní balíčky ODataLib Hello používané hello Klientská knihovna pro úložiště jsou [OData](http://nuget.org/packages/Microsoft.Data.OData/), [Edm](http://nuget.org/packages/Microsoft.Data.Edm/), a [Spatial](http://nuget.org/packages/System.Spatial/). Když tyto knihovny používají třídy Azure Table storage hello, představují požadované závislosti pro programování s hello Klientská knihovna pro úložiště.
> 
> 

### <a name="determine-your-target-environment"></a>Určení cílového prostředí
Máte dvě možnosti prostředí pro spuštění hello příklady v tomto průvodci:

* Svůj kód můžete spustit na účtu Azure Storage v cloudu hello. 
* Svůj kód můžete spustit emulátoru úložiště Azure hello. emulátor úložiště Hello je místní prostředí, které emuluje účet služby Azure Storage v cloudu hello. Hello emulátor je bezplatnou možností pro testování a ladění kódu během aplikace je ve vývoji. Hello emulátor používá známý účet a klíč. Další informace najdete v tématu [hello pomocí emulátoru úložiště Azure pro vývoj a testování](../articles/storage/common/storage-use-emulator.md)

Pokud cílíte na účet úložiště v cloudu hello, zkopírujte hello primární přístupový klíč pro účet úložiště z hello portálu Azure. Další informace najdete v článku [Zobrazení a kopírování přístupového klíče k úložišti](../articles/storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys).

> [!NOTE]
> Můžete určit cílovou tooavoid emulátor úložiště hello nákladům spojeným s Azure Storage. Ale pokud si zvolíte tootarget účet úložiště Azure v cloudu hello, náklady na provádění tohoto kurzu bude nepatrné.
> 
> 

### <a name="configure-your-storage-connection-string"></a>Konfigurace připojovacího řetězce úložiště
Hello Klientská knihovna pro úložiště Azure pro .NET podporuje použití úložiště připojovací řetězec tooconfigure koncových bodů a přihlašovací údaje pro přístup ke službám úložiště. nejlepší způsob, jak toomaintain Hello připojovací řetězec úložiště je v konfiguračním souboru. 

Další informace o připojovacích řetězcích najdete v tématu [konfigurace tooAzure připojovací řetězec úložiště](../articles/storage/common/storage-configure-connection-string.md).

> [!NOTE]
> Klíč účtu úložiště je podobný toohello kořenové heslo pro účet úložiště. Pečlivě tooprotect být vždy klíč účtu úložiště. Nedávejte ho tooother uživatelům, nezakódovávejte ho nebo ho uložit v souboru formátu prostého textu, který je přístupný tooothers. Znovu vygenerujte klíč pomocí hello portálu Azure, pokud se domníváte, že by může ohrožení.
> 
> 

tooconfigure připojovací řetězec, otevřete hello `app.config` soubor v Průzkumníku řešení v sadě Visual Studio. Přidat obsah hello hello `<appSettings>` níže uvedeného prvku. Nahraďte `account-name` hello název účtu úložiště a `account-key` vaším přístupovým klíčem účet:

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
    </appSettings>
</configuration>
```

Nastavení konfigurace může vypadat následovně:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=GMuzNHjlB3S9itqZJHHCnRkrokLkcSyW7yK9BRbGp0ENePunLPwBgpxV1Z/pVo9zpem/2xSHXkMqTHHLcx8XRA==" />
```

emulátor úložiště hello tootarget, můžete vytvořit zástupce, který mapuje název toohello známého účtu a klíč. V takovém případě bude nastavení připojovacího řetězce vypadat následovně:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
```

