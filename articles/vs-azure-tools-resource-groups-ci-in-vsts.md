---
title: "integrace aaaContinuous ve Visual Studio Team Services pomocí projekty skupiny prostředků Azure | Microsoft Docs"
description: "Popisuje, jak tooset až průběžnou integraci ve Visual Studio Team Services pomocí nasazení skupiny prostředků Azure projekty v sadě Visual Studio."
services: visual-studio-online
documentationcenter: na
author: mlearned
manager: erickson-doug
editor: 
ms.assetid: b81c172a-be87-4adc-861e-d20b94be9e38
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: mlearned
ms.openlocfilehash: 0fe4a4b8989ee323e8ef2206fa4ebed503025670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Průběžnou integraci ve Visual Studio Team Services pomocí projekty nasazení skupiny prostředků Azure
toodeploy šablony Azure, můžete provádět úlohy v různých fázích: vytvořit, testovací, kopie tooAzure (také nazývané "Přípravy") a nasadit šablonu. Existují dva různé způsoby toodeploy šablony tooVisual Studio Team Services (Visual Studio Team Services). Obě metody zadejte stejné výsledky hello, takže zvolte hello ten, který nejlépe vyhovuje pracovního postupu.

1. Přidejte definici sestavení tooyour jeden krok, který spouští skript prostředí PowerShell text hello, který je součástí hello projekt nasazení skupiny prostředků Azure (nasadit-AzureResourceGroup.ps1). skript Hello zkopíruje artefaktů a poté nasadí hello šablony.
2. Přidáte že více Visual Studio Team Services kroky sestavení, každé z nich provedení úlohy fáze.

Tento článek ukazuje obě možnosti. první možnost Hello má hello výhod používání hello používá stejný skript vývojáři v sadě Visual Studio a zajištění konzistence v průběhu cyklu hello. Druhá možnost Hello nabízí pohodlný alternativní toohello předdefinované skript. Obě postupech se předpokládá, že již máte nasazení projektu sady Visual Studio zaškrtnutí do Visual Studio Team Services.

## <a name="copy-artifacts-tooazure"></a>Zkopírování artefaktů tooAzure
Bez ohledu na scénáři hello máte artefakty, které jsou potřebné pro nasazení šablony, je třeba zadat toothem přístupu Azure Resource Manager. Tyto artefakty mohou zahrnovat například soubory:

* Vnořené šablony
* Konfiguračních skriptů a skripty, DSC
* Binární soubory aplikace

### <a name="nested-templates-and-configuration-scripts"></a>Vnořené šablony a konfigurační skripty
Při použití šablony hello poskytované sadě Visual Studio (nebo vytvořené s nástroji Visual Studio fragmenty) hello skript prostředí PowerShell nejen zpracuje hello artefakty, je také parameterizes hello identifikátor URI pro hello prostředky pro jiné nasazení. skript Hello pak zkopíruje hello artefakty tooa zabezpečeného kontejneru v Azure, vytvoří SaS token pro tento kontejner a pak předá tyto informace o nasazení šablony toohello. V tématu [vytvořit šablonu nasazení](https://msdn.microsoft.com/library/azure/dn790564.aspx) toolearn Další informace o vnořené šablony.  Při použití úlohy v Visual Studio Team Services, musíte vybrat hello příslušné úlohy pro šablonu nasazení a v případě potřeby předat hodnoty parametrů z hello pracovní krok toohello šablonu nasazení.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Nastavit průběžné nasazování ve Visual Studio Team Services
skript prostředí PowerShell toocall hello ve Visual Studio Team Services, budete potřebovat tooupdate vaší definice sestavení. Stručně řečeno hello kroky jsou: 

1. Upravte definici sestavení hello.
2. Nastavení Azure autorizace ve Visual Studio Team Services.
3. Přidejte krok sestavení Azure PowerShell, který odkazuje na skript prostředí PowerShell hello v hello projekt nasazení skupiny prostředků Azure.
4. Nastavte hodnotu hello hello *- ArtifactsStagingDirectory* toowork parametr s projektem součástí Visual Studio Team Services.

### <a name="detailed-walkthrough-for-option-1"></a>Podrobný návod pro možnost 1
Hello následující postupy vás provede procesem hello kroky nezbytné tooconfigure průběžné nasazování ve Visual Studio Team Services pomocí jednu úlohu, která se spouští skript prostředí PowerShell hello ve vašem projektu. 

1. Upravit definici sestavení vaší Visual Studio Team Services a přidejte krok sestavení prostředí Azure PowerShell. Zvolte definici sestavení hello pod hello **sestavení definice** kategorie a potom zvolte hello **upravit** odkaz.
   
   ![Upravit definici sestavení][0]
2. Přidejte nový **prostředí Azure PowerShell** sestavení definici sestavení toohello krok a pak zvolte hello **krok sestavení přidat...** .
   
   ![Přidejte krok sestavení][1]
3. Zvolte hello **úloh nasadit** kategorie, vyberte hello **prostředí Azure PowerShell** úkolů a potom vyberte jeho **přidat** tlačítko.
   
   ![Přidání úkolů][2]
4. Zvolte hello **prostředí Azure PowerShell** kroku sestavení a pak zadejte jeho hodnoty.
   
   1. Pokud již máte koncový bod služby Azure přidali tooVS Team Services, zvolte předplatné hello v hello **předplatné Azure** rozevíracího seznamu a pak přeskočit toohello další části. 
      
      Pokud nemáte koncový bod služby Azure ve Visual Studio Team Services, je třeba tooadd jeden. Tato část vás provede procesem hello. Pokud účtu Azure používá účet Microsoft (například Hotmail), je třeba provést následující kroky tooget hello ověřování instanční objekt.
   2. Zvolte hello **spravovat** propojit další toohello **předplatné Azure** rozevíracího seznamu.
      
      ![Spravovat předplatná Azure][3]
   3. Zvolte **Azure** v hello **nový koncový bod služby** rozevíracího seznamu.
      
      ![Nový koncový bod služby][4]
   4. V hello **přidat předplatné Azure** dialogové okno, vyberte hello **instanční objekt** možnost.
      
      ![Hlavní možnosti služby][5]
   5. Přidat toohello informace vašeho předplatného Azure **přidat předplatné Azure** dialogové okno. Je třeba tooprovide hello následující položky:
      
      * Id předplatného
      * Název odběru
      * Id objektu zabezpečení
      * Klíč objektu služby
      * Id klienta
   6. Přidejte název podle vašeho výběru toohello **předplatné** pole název. Tato hodnota se zobrazí později v hello **předplatné Azure** rozevíracího seznamu ve Visual Studio Team Services. 
   7. Pokud si nejste jisti svoje ID předplatného Azure, můžete použít jednu z následujících příkazů tooretrieve hello ho.
      
      Pro skripty prostředí PowerShell použijte:
      
      `Get-AzureRmSubscription`
      
      Pokud používáte Azure CLI, použijte:
      
      `azure account show`
   8. tooget ID objektu služby, klíč objektu služby a ID klienta, použijte postup hello v [vytvoření aplikace Active Directory a objektu zabezpečení pomocí portálu](resource-group-create-service-principal-portal.md) nebo [ověřování objekt služby s Azure Resource Manager](resource-group-authenticate-service-principal.md).
   9. Přidat hello ID objektu služby, klíč objektu služby a ID klienta hodnoty toohello **přidat předplatné Azure** dialogové okno pole a zvolte hello **OK** tlačítko.
      
      Nyní máte platný objekt služby toouse toorun hello skript prostředí Azure PowerShell.
5. Upravit definici sestavení hello a zvolte hello **prostředí Azure PowerShell** kroku sestavení. Vyberte předplatné hello v hello **předplatné Azure** rozevíracího seznamu. (Pokud se nezobrazí hello předplatného, zvolte hello **aktualizovat** tlačítko Další hello **spravovat** odkaz.) 
   
   ![Konfigurace prostředí Azure PowerShell úlohu sestavení][8]
6. Zadejte cestu toohello skript prostředí PowerShell AzureResourceGroup.ps1 nasadit. toodo, zvolte hello třemi tečkami (...) tlačítko Další toohello **cestu ke skriptu** přejděte skript prostředí PowerShell nasadit AzureResourceGroup.ps1 toohello v hello **skripty** složku projekt, vyberte ho, a potom zvolte hello **OK** tlačítko.    
   
   ![Vyberte cestu tooscript][9]
7. Po výběru hello skriptu, aktualizujte hello cesta toohello skript tak, aby je spuštěn z hello Build.StagingDirectory (hello stejný adresář, *ArtifactsLocation* je nastaven na). To provedete tak, že přidáte "$(Build.StagingDirectory)/" toohello začátek hello cestu ke skriptu.
   
    ![Upravit tooscript cesta][10]
8. V hello **argumenty skriptu** zadejte hello následující parametry (v jednom řádku). Pokud spustíte skript hello v sadě Visual Studio, můžete zobrazit, jak používá VS hello parametry v hello **výstup** okno. Můžete to použít jako výchozí bod pro nastavování hodnot parametrů hello v kroku vaše sestavení.
   
   | Parametr | Popis |
   | --- | --- |
   | -ResourceGroupLocation |Hello hodnotu geografického umístění, kde je umístěna, například skupina prostředků hello **eastus** nebo **, východní USA,**. (Pokud je mezera v názvu text hello, přidejte jednoduchých uvozovkách.) V tématu [oblasti Azure](https://azure.microsoft.com/en-us/regions/) Další informace. |
   | -ResourceGroupName |Hello název skupiny prostředků hello použít pro toto nasazení. |
   | -UploadArtifacts |Tento parametr, pokud jsou k dispozici, určuje, zda artefaktů, které je třeba toobe nahrán tooAzure z místního systému hello. Potřebujete jenom tooset tento přepínač. Pokud vaše šablony nasazení vyžaduje další artefaktů, které chcete toostage pomocí skriptu prostředí PowerShell hello (například konfigurační skripty nebo vnořené šablony). |
   | -StorageAccountName |Hello název účtu úložiště hello používá toostage artefaktů pro toto nasazení. Tento parametr se používá pouze pokud jsou pracovní artefaktů pro nasazení. Pokud je tento parametr zadaný, se vytvoří nový účet úložiště, pokud hello skript nebyl vytvořili během předchozí nasazení. Pokud je zadán parametr hello, již musí existovat účet úložiště hello. |
   | -StorageAccountResourceGroupName |Hello název skupiny prostředků hello přidružené k účtu úložiště hello. Tento parametr je povinný, jenom v případě, že je zadat hodnotu pro parametr StorageAccountName hello. |
   | -TemplateFile |Hello cesta toohello souboru šablony v hello projekt nasazení skupiny prostředků Azure. Flexibilita tooenhance, použijte cestu pro tento parametr, který je relativní toohello umístění hello skript prostředí PowerShell místo absolutní cesta. |
   | -TemplateParametersFile |Hello cesta toohello soubor parametrů v hello projekt nasazení skupiny prostředků Azure. Flexibilita tooenhance, použijte cestu pro tento parametr, který je relativní toohello umístění hello skript prostředí PowerShell místo absolutní cesta. |
   | -ArtifactStagingDirectory |Tento parametr umožňuje skript prostředí PowerShell hello vědět hello složku, ze kterých by se měl zkopírovat binární soubory hello projektu. Tato hodnota přepíše hello výchozí hodnota používaná metodou hello skript prostředí PowerShell. Pro Visual Studio Team Services používat, nastavte hodnotu hello: - ArtifactStagingDirectory $(Build.StagingDirectory) |
   
   Tady je příklad skriptu argumenty (řádku přerušený čitelnější):
   
   ```    
   -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
   -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
   –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)'    
   ```
   
   Jakmile budete hotovi, hello **argumenty skriptu** pole by měla vypadat přibližně hello následující seznamu:
   
   ![Argumenty skriptu][11]
9. Po přidání všech hello požadované položky toohello kroku sestavení Azure PowerShell, zvolte hello **fronty** tlačítko toobuild hello projekt sestavit. Hello **sestavení** obrazovka ukazuje výstup hello hello skript prostředí PowerShell.

### <a name="detailed-walkthrough-for-option-2"></a>Podrobný návod pro možnost 2
Hello následující postupy vás provede procesem hello kroky nezbytné tooconfigure průběžné nasazování ve Visual Studio Team Services pomocí předdefinované úlohy hello.

1. Upravte vaše Visual Studio Team Services definice tooadd dva nové sestavení kroky sestavení. Zvolte definici sestavení hello pod hello **sestavení definice** kategorie a potom zvolte hello **upravit** odkaz.
   
   ![Upravit definice sestavení][12]
2. Přidat hello nové kroky sestavení, toohello definici sestavení pomocí hello **krok sestavení přidat...** .
   
   ![Přidejte krok sestavení][13]
3. Zvolte hello **nasadit** kategorii úloh, vyberte hello **Azure File Copy** úkolů a potom vyberte jeho **přidat** tlačítko.
   
   ![Přidat úloha Azure File Copy][14]
4. Zvolte hello **nasazení skupiny prostředků Azure** úkolů, a potom vyberte jeho **přidat** tlačítko a potom **Zavřít** hello **úloha katalogu**.
   
   ![Přidat úloha nasazení skupiny prostředků Azure][15]
5. Zvolte hello **Azure File Copy** úloh a zadejte jeho hodnoty.
   
   Pokud již máte koncový bod služby Azure přidali tooVS Team Services, zvolte předplatné hello v hello **předplatné Azure** rozevíracího seznamu. Pokud nemáte předplatné, přečtěte si téma [možnost 1](#detailed-walkthrough-for-option-1) pokyny k nastavení jeden ve Visual Studio Team Services.
   
   * Zdroj – zadejte **$(Build.StagingDirectory)**
   * Azure typu připojení – vyberte **Azure Resource Manager**
   * Předplatné Azure RM - vyberte hello předplatné pro účet úložiště hello chcete toouse v hello **předplatné Azure** rozevíracího seznamu. Pokud se nezobrazí hello předplatného, zvolte hello **aktualizovat** tlačítko Další hello **spravovat** odkaz.
   * Cílový typ - vyberte **objektů Blob v Azure**
   * RM účet úložiště – vyberte účet úložiště hello byste chtěli toouse pro přípravu artefaktů
   * Název kontejneru – zadejte název hello hello kontejneru byste chtěli toouse pro pracovní; může být jakýkoli název kontejneru platný, ale používat jeden vyhrazený toothis definici sestavení
   
   Pro hodnoty výstup hello:
   
   * Zadejte kontejner úložiště URI - **artifactsLocation**
   * Token SAS pro kontejner úložiště – zadejte **artifactsLocationSasToken**
   
   ![Konfigurace úloze Azure File Copy][16]
6. Zvolte hello **nasazení skupiny prostředků Azure** kroku sestavení a pak zadejte jeho hodnoty.
   
   * Azure typu připojení – vyberte **Azure Resource Manager**
   * Předplatné Azure RM - vyberte hello předplatné pro nasazení v hello **předplatné Azure** rozevíracího seznamu. To bude obvykle hello používá stejného předplatného v předchozím kroku hello
   * Akce – vyberte **vytvořit nebo aktualizovat skupinu prostředků**
   * Skupiny prostředků – vyberte skupinu prostředků nebo zadejte název hello novou skupinu prostředků pro nasazení hello
   * Umístění – vyberte hello umístění pro skupinu prostředků hello
   * Šablona – zadejte hello cesta a název hello šablony toobe nasazené předřazení **$(Build.StagingDirectory)**, například: **$(Build.StagingDirectory/DSC-CI/azuredeploy.json)**
   * Zadejte parametry šablony - hello cesta a název toobe parametry hello používá, předřazení **$(Build.StagingDirectory)**, například: **$(Build.StagingDirectory/DSC-CI/azuredeploy.parameters.json)**
   * Přepsání parametry šablony – zadejte nebo zkopírujte a vložte následující kód hello:
     
     ```    
     -_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken (ConvertTo-SecureString -String "$(artifactsLocationSasToken)" -AsPlainText -Force)
     ```
     ![Konfigurace úlohy nasazení skupiny prostředků Azure][17]
7. Po přidání všech hello požadované položky, Uložit definici sestavení hello a zvolte **fronty nového sestavení** v horní části hello.

## <a name="next-steps"></a>Další kroky
Čtení [přehled Azure Resource Manageru](azure-resource-manager/resource-group-overview.md) toolearn Další informace o Azure Resource Manageru a skupin prostředků Azure.

[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
[12]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough13.png
[13]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough14.png
[14]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough15.png
[15]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough16.png
[16]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough17.png
[17]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough18.png
