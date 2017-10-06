---
title: "role hello aaaConfigure Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak tooset až a konfigurovat role pro cloudové služby Azure pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: d3c62eb57040ebe987787e73b17b468bb82122bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a>Konfigurace role Azure cloud service pomocí sady Visual Studio
Cloudové služby Azure může mít jeden nebo více pracovních nebo webové role. Pro každou roli potřebovat toodefine nastavení dané role a taky nakonfigurovat, jak tato role běží. toolearn Další informace o rolí v cloudových služeb, najdete v části hello video [tooAzure Úvod cloudové služby](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). 

Hello informace pro cloudové služby jsou uloženy v hello následující soubory:

- **ServiceDefinition.csdef** -souboru definice služby hello definuje hello nastavení běhového prostředí pro cloudové služby, včetně toho, jaké role jsou požadovány, koncových bodů a velikost virtuálního počítače. Žádná z hello data uložená v `ServiceDefinition.csdef` jde změnit, pokud vaše role běží.
- **Souboru ServiceConfiguration.cscfg** – konfigurační soubor služby hello konfiguruje, jak velký počet instancí role spouštějí a hello hodnoty hello nastavení definované pro roli. Hello data uložená v `ServiceConfiguration.cscfg` lze změnit, když vaše role běží.

toostore různé hodnoty pro hello nastavení, které určují běh roli, můžete definovat více konfigurace služby. Konfigurace různé služby můžete použít pro jednotlivá prostředí nasazení. Můžete například nastavit vašeho úložiště účet připojovací řetězec toouse hello místní emulátoru úložiště Azure v konfiguraci místní služby a vytvořit další toouse konfigurace služby Azure storage v cloudu hello.

Při vytváření cloudové služby Azure v sadě Visual Studio, dvě konfigurace služby se automaticky vytvoří a přidány tooyour projektu Azure:

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a>Konfigurace Azure cloud service
Cloudové služby Azure v Průzkumníku řešení v sadě Visual Studio, můžete nakonfigurovat, jak je znázorněno v hello následující kroky:

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **vlastnosti**.
   
    ![Místní nabídku projektu Průzkumník řešení](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. Na stránce vlastností projektu hello vyberte hello **vývoj** kartě. 

    ![Stránka vlastností projektu - karta vývoj](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. V hello **konfigurace služby** seznamu, vyberte hello název hello konfigurace služby, které chcete tooedit. (Pokud chcete toomake změny konfigurace služby hello tooall pro tuto roli, vyberte **všechny konfigurace**.)
   
    > [!IMPORTANT]
    > Pokud si zvolíte specifické služby konfigurace, některé vlastnosti jsou zakázané, protože jejich lze nastavit pouze pro všechny konfigurace. tooedit tyto vlastnosti, je nutné vybrat **všechny konfigurace**.
    > 
    > 
   
    ![Seznam konfigurace služby pro cloudové služby Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-hello-number-of-role-instances"></a>Změnit hello počet instancí role
výkon hello tooimprove cloudové služby, můžete změnit hello počet instancí role, které běží na základě hello počtu uživatelů nebo hello zatížení pro konkrétní roli. Samostatný virtuální počítač se vytvoří pro každou instanci role, když hello Cloudová služba běží v Azure. Tato akce ovlivní hello fakturace pro hello nasazení pro tuto cloudovou službu. Další informace o fakturaci, viz [porozumět vaší faktuře pro Microsoft Azure](billing/billing-understand-your-bill.md).

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku**, rozbalte uzel projektu hello. V části hello **role** uzel, klikněte pravým tlačítkem na hello role chcete tooupdate a, hello místní nabídce vyberte **vlastnosti**.

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Vyberte hello **konfigurace** kartě.

    ![Na kartě Konfigurace](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. V hello **konfigurace služby** seznamu, vyberte hello konfigurace služby, které chcete tooupdate.
   
    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. V hello **Instance počet** textové pole, zadejte hello počet instancí, který má toostart pro tuto roli. Každá instance běží na samostatný virtuální počítač při publikování hello cloudové služby tooAzure.

    ![Aktualizace hello počet instancí](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. Z hello Visual Studio panelu nástrojů vyberte **Uložit**.

## <a name="manage-connection-strings-for-storage-accounts"></a>Spravovat připojovací řetězce pro účty úložiště
Můžete přidat, odebrat nebo upravit připojovací řetězce pro vaše konfigurace služby. Například můžete místní připojovací řetězec pro konfiguraci místní služby, který má hodnotu `UseDevelopmentStorage=true`. Také můžete chtít tooconfigure konfigurace služby cloud, který používá účet úložiště v Azure.

> [!WARNING]
> Když zadáte hello Azure klíčové informace o účtu úložiště pro připojovací řetězec, účet úložiště, tyto informace jsou uloženy místně v konfiguračním souboru služby hello. Tyto informace nejsou aktuálně uloženy jako šifrovaný text.
> 
> 

Pomocí jinou hodnotu pro každou konfiguraci služby nemáte mít toouse různých připojovací řetězce v rámci cloudové služby nebo upravit kód při publikování tooAzure vaše cloudové služby. Můžete použít stejný název pro hello připojovací řetězec v kódu a hello hodnota se liší podle konfigurace služby hello, kterou jste vybrali při sestavování cloudové služby, nebo pokud ji publikujete hello.

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku**, rozbalte uzel projektu hello. V části hello **role** uzel, klikněte pravým tlačítkem na hello role chcete tooupdate a, hello místní nabídce vyberte **vlastnosti**.

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Vyberte hello **nastavení** kartě.

    ![Karta nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. V hello **konfigurace služby** seznamu, vyberte hello konfigurace služby, které chcete tooupdate.

    ![Konfigurace služby](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Vyberte tooadd připojovací řetězec, **přidat nastavení**.

    ![Přidat připojovací řetězec](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Po přidání seznamu toohello nové nastavení hello aktualizujte hello řádek v seznamu hello hello potřebné informace.

    ![Nový připojovací řetězec](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Název** – zadejte název hello chcete toouse pro hello připojovací řetězec.
    - **Typ** – vyberte **připojovací řetězec** z rozevíracího seznamu hello.
    - **Hodnota** -hello připojovací řetězec můžete zadat přímo do hello **hodnotu** buňku nebo toowork třemi tečkami (...) vyberte hello v hello **vytvoření připojovacího řetězce úložiště** dialogové okno.  

1. V hello **vytvoření připojovacího řetězce úložiště** dialogovém okně, vyberte možnost pro **připojit pomocí**. Postupujte podle pokynů hello hello možnost, kterou vyberete:

    - **Emulátor úložiště Microsoft Azure** – Pokud vyberete tuto možnost, hello zbývající nastavení v dialogovém okně hello jsou zakázána, protože se vztahují pouze tooAzure. Vyberte **OK**.
    - **Vaše předplatné** – Pokud vyberte tuto možnost, použít hello rozevíracího seznamu tooeither vybrat a přihlášení do účtu Microsoft, nebo přidat účet Microsoft. Vyberte účet předplatného a úložiště Azure. Vyberte **OK**.
    - **Ručně zadali přihlašovací údaje** – zadejte název účtu úložiště hello a buď hello primární a druhý klíč. Vyberte možnost pro **připojení** (HTTPS se doporučuje pro většinu scénářů). Vyberte **OK**.

1. toodelete připojovací řetězec, vyberte hello připojovací řetězec a pak vyberte **odebrat nastavení**.

1. Z hello Visual Studio panelu nástrojů vyberte **Uložit**.

## <a name="programmatically-access-a-connection-string"></a>Programový přístup připojovací řetězec

Hello následující kroky ukazují, jak tooprogrammatically přístup připojovací řetězec pomocí jazyka C#.

1. Přidejte následující hello pomocí souboru tooa C# direktivy kam toouse hello nastavení:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Hello následující kód ukazuje příklad tooaccess připojovací řetězec. Nahraďte hello &lt;ConnectionStringName > zástupný symbol hello odpovídající hodnotu. 

    ```csharp
    // Setup hello connection tooAzure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-toouse-in-your-azure-cloud-service"></a>Přidat vlastní nastavení toouse ve službě Azure cloud
Vlastní nastavení v konfiguračním souboru služby hello umožňuje přidat název a hodnotu řetězce pro konfiguraci konkrétní služby. Můžete zvolit, toouse tato nastavení tooconfigure funkce v cloudové službě načtením hello hodnotu hello nastavení a použití této hodnoty toocontrol hello logiky v kódu. Tyto hodnoty konfigurace služby můžete změnit bez nutnosti toorebuild vašeho balíčku služby, nebo když cloudové služby běží. Kódu můžete zkontrolovat pro oznámení, když se změní nastavení. V tématu [RoleEnvironment.Changing událostí](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Můžete přidat, odebrat nebo upravit vlastní nastavení pro vaše konfigurace služby. Můžete chtít různé hodnoty pro tyto řetězce pro různé služby konfigurace.

Pomocí jinou hodnotu pro každou konfiguraci služby nemáte máte toouse jiné řetězce v rámci cloudové služby nebo upravit kód při publikování tooAzure vaše cloudové služby. Můžete použít stejný název pro hello řetězec v kódu a hello hodnota se liší podle konfigurace služby hello, kterou jste vybrali při sestavování cloudové služby, nebo pokud ji publikujete hello.

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku**, rozbalte uzel projektu hello. V části hello **role** uzel, klikněte pravým tlačítkem na hello role chcete tooupdate a, hello místní nabídce vyberte **vlastnosti**.

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Vyberte hello **nastavení** kartě.

    ![Karta nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. V hello **konfigurace služby** seznamu, vyberte hello konfigurace služby, které chcete tooupdate.

    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. tooadd vlastního nastavení, vyberte **přidat nastavení**.

    ![Přidat vlastní nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Po přidání seznamu toohello nové nastavení hello aktualizujte hello řádek v seznamu hello hello potřebné informace.

    ![Nové vlastní nastavení](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Název** – zadejte název hello hello nastavení.
    - **Typ** – vyberte **řetězec** z rozevíracího seznamu hello.
    - **Hodnota** – zadejte hodnotu hello hello nastavení. Můžete buď zadat hello hodnotu přímo do hello **hodnotu** buňky, nebo vyberte hello třemi tečkami (...) tooenter hello hodnota ve hello **Upravit řetězec** dialogové okno.  

1. Vyberte nastavení hello toodelete vlastního nastavení a pak vyberte **odebrat nastavení**.

1. Z hello Visual Studio panelu nástrojů vyberte **Uložit**.

## <a name="programmatically-access-a-custom-settings-value"></a>Programový přístup hodnoty vlastní nastavení
 
Hello následující kroky ukazují, jak tooprogrammatically přístup vlastního nastavení pomocí jazyka C#.

1. Přidejte následující hello pomocí souboru tooa C# direktivy kam toouse hello nastavení:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Hello následující kód ukazuje příklad tooaccess vlastního nastavení. Nahraďte hello &lt;SettingName > zástupný symbol hello odpovídající hodnotu. 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Spravovat místní úložiště pro každou instanci role
Můžete přidat úložiště systému místní soubor pro každou instanci role. Hello datům uloženým v úložiště není přístupné ostatní instance hello role, pro které hello data uložená nebo jiné role.  

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku**, rozbalte uzel projektu hello. V části hello **role** uzel, klikněte pravým tlačítkem na hello role chcete tooupdate a, hello místní nabídce vyberte **vlastnosti**.

    ![Řešení Explorer Azure role kontextové nabídky](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Vyberte hello **místní úložiště** kartě.

    ![Karta místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. V hello **konfigurace služby** seznamu, ujistěte se, že **všechny konfigurace** je vybrán jako nastavení místní úložiště hello použít tooall služby konfigurace. Jakákoli jiná hodnota výsledkem všechny hello vstupních polí na stránce hello zakázaná. 

    ![Konfigurace služby seznamu](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. Vyberte položku místní úložiště tooadd **přidat místní úložiště**.

    ![Přidejte místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. Po přidání seznamu toohello hello nové místní úložiště položku aktualizujte hello řádek v seznamu hello hello potřebné informace.

    ![Nová položka místní úložiště](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - **Název** – zadejte název hello má toouse pro hello nové místní úložiště.
    - **Velikost (MB)** – zadejte velikost hello v MB, kterou potřebujete pro hello nové místní úložiště.
    - **Čištění na role recyklaci** – vyberte tuto možnost tooremove hello data v nové místní úložiště hello po recyklaci hello virtuálního počítače pro roli hello.

1. Vyberte položku hello toodelete položku místní úložiště a potom vyberte **odebrat místní úložiště**.

1. Z hello Visual Studio panelu nástrojů vyberte **Uložit**.

## <a name="programmatically-accessing-local-storage"></a>Prostřednictvím kódu programu přístup k místnímu úložišti

Tato část ukazuje, jak tooprogrammatically přistupovat k místnímu úložišti pomocí jazyka C# vytvořením textového souboru testovací `MyLocalStorageTest.txt`.  

### <a name="write-a-text-file-toolocal-storage"></a>Zápis textu souboru toolocal úložiště

Hello následující kód ukazuje příklad jak toowrite textový toolocal úložiště souborů. Nahraďte hello &lt;LocalStorageName > zástupný symbol hello odpovídající hodnotu. 

    ```csharp
    // Retrieve an object that points toohello local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define hello file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-toolocal-storage"></a>Vyhledat soubor zapsána toolocal úložiště

soubor hello tooview vytvořené hello kód v předchozí části hello, postupujte takto:
    
1.  V oznamovací oblasti systému Windows hello, klikněte pravým tlačítkem na hello Azure – ikona a, hello místní nabídce vyberte **zobrazit uživatelské prostředí emulátoru výpočtů**. 

    ![Zobrazit emulátoru služby výpočty Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. Vyberte roli webový hello.

    ![Emulátoru služby výpočty Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. Na hello **Microsoft Azure výpočetní emulátor** nabídce vyberte možnost **nástroje** > **otevřete místní úložiště**.

    ![Položky nabídky otevřete místního úložiště.](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. Když se otevře okno Průzkumníka Windows hello, zadejte ' MyLocalStorageTest.txt'' do hello **vyhledávání** textového pole a vyberte **Enter** toostart hello vyhledávání. 

## <a name="next-steps"></a>Další kroky
Další informace o Azure projekty v sadě Visual Studio načtením [konfigurace projektu Azure](vs-azure-tools-configuring-an-azure-project.md). Další informace o hello cloudové služby schéma načtením [– odkaz schématu](https://msdn.microsoft.com/library/azure/dd179398).

