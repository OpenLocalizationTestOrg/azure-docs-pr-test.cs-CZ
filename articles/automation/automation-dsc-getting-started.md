---
title: "aaaGetting začít s Azure Automation DSC. | Microsoft Docs"
description: "Vysvětlení a příklady hello nejběžnější úlohy v Azure Automation požadovaného stavu konfigurace (DSC)"
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;eslesar
ms.openlocfilehash: 82910c96e928b9264c2e1b52a5c98dc47273dcc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a>Začínáme s Azure Automation DSC.
Toto téma vysvětluje, jak toodo hello běžné úkoly s Azure Automation požadovaného stavu konfigurace (DSC), jako je například vytváření, importování a kompilování konfigurace, registrace, které počítače příliš spravovat, a zobrazování sestav. Přehled co Azure Automation DSC je, najdete v části [přehled Azure Automation DSC](automation-dsc-overview.md). DSC dokumentaci najdete v tématu [Přehled konfigurace prostředí Windows PowerShell požadovaného stavu](https://msdn.microsoft.com/PowerShell/dsc/overview).

Toto téma obsahuje podrobný průvodce toousing Azure Automation DSC. Pokud chcete, aby ukázkové prostředí, které jsou již nastaveny bez hello kroků popsaných v tomto tématu, můžete použít [hello následující šablony ARM](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup). Tato šablona nastavuje dokončené prostředí Azure Automation DSC, včetně virtuálního počítače Azure, který je spravovaný nástrojem Azure Automation DSC.

## <a name="prerequisites"></a>Požadavky
toocomplete hello příklady v tomto tématu, hello vyžadují se následující věci:

* Účet Azure Automation. Pokyny k vytvoření účtu Azure Automation Spustit jako najdete v tématu [Účet Spustit jako pro Azure](automation-sec-configure-azure-runas-account.md).
* Virtuální počítač Azure Resource Manager (ne Classic) systémem Windows Server 2008 R2 nebo novější. Pokyny pro vytvoření virtuálního počítače, v tématu [vytvořit svůj první virtuální počítač Windows v hello portálu Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>Vytvoření konfigurace DSC
Vytvoříme jednoduchou [konfigurace DSC](https://msdn.microsoft.com/powershell/dsc/configurations) zajistí se tak hello přítomnosti nebo absenci hello **Webový Server** Windows funkce (IIS), v závislosti na tom, jak přiřadit uzly.

1. Spusťte Windows PowerShell ISE hello (nebo libovolného textového editoru).
2. Zadejte hello následující text:
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
   
            }
        }
   
        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
   
            }
        }
        }
    ```
3. Uložte soubor hello jako `TestConfig.ps1`.

Tato konfigurace volá jeden prostředek v každém uzlu bloku hello [WindowsFeature prostředků](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), který zajišťuje hello přítomnosti nebo absenci hello **Webový Server** funkce.

## <a name="importing-a-configuration-into-azure-automation"></a>Konfigurace importu do Azure Automation
V dalším kroku jsme budete importovat hello konfigurace do hello účet Automation.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **konfigurace DSC**.
4. Na hello **konfigurace DSC** okně klikněte na tlačítko **přidání konfigurace**.
5. Na hello **importovat konfiguraci** okno, procházet toohello `TestConfig.ps1` souboru ve svém počítači.
   
    ![Snímek obrazovky hello ** okno importu konfigurace **](./media/automation-dsc-getting-started/AddConfig.png)
6. Klikněte na **OK**.

## <a name="viewing-a-configuration-in-azure-automation"></a>Zobrazení konfigurace ve službě Azure Automation
Po dokončení importu konfigurace, můžete ji zobrazit v hello portálu Azure.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **konfigurace DSC**
4. Na hello **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (to je název hello hello konfigurace, které jste importovali v předchozím postupu hello).
5. Na hello **TestConfig konfigurace** okně klikněte na tlačítko **zdroj konfigurace zobrazení**.
   
    ![Snímek obrazovky okna konfigurace TestConfig hello](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    A **zdroj konfigurace TestConfig** otevře se okno zobrazení kódu hello prostředí PowerShell pro konfiguraci hello.

## <a name="compiling-a-configuration-in-azure-automation"></a>Kompilování konfigurace ve službě Azure Automation
Před použitím požadovaný stav uzlu tooa konfigurace DSC, definování tohoto stavu musí být zkompilovány do minimálně jedna konfigurace uzlu (MOF dokumentu) a umístit na hello serveru vyžádané replikace s DSC automatizace. Podrobnější popis kompilování konfigurace v Azure Automation DSC, najdete v části [kompilování konfigurace v Azure Automation DSC](automation-dsc-compile.md). Další informace o kompilování konfigurace najdete v tématu [konfigurace DSC](https://msdn.microsoft.com/PowerShell/DSC/configurations).

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **konfigurace DSC**
4. Na hello **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (hello název hello předtím naimportovali konfigurace).
5. Na hello **TestConfig konfigurace** okně klikněte na tlačítko **zkompilovat**a potom klikněte na **Ano**. Spustí úlohu kompilace.
   
    ![Snímek obrazovky okna konfigurace TestConfig hello zvýraznění tlačítko kompilace](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Při kompilaci konfigurace ve službě Azure Automation, automaticky nasadí všechny vytvořené uzlu konfigurační soubory MOF toohello vyžádání obsahu server.
> 
> 

## <a name="viewing-a-compilation-job"></a>Zobrazení úlohu kompilace
Po spuštění kompilace, můžete ji zobrazit v hello **úlohy kompilace** dlaždici v hello **konfigurace** okno. Hello **úlohy kompilace** dlaždice ukazuje aktuálně spuštěna, byla dokončena a neúspěšné úlohy. Otevřete okno úlohy kompilace, se zobrazí informace o této úloze, včetně žádné chyby nebo upozornění došlo, vstupní parametry v konfiguraci hello a kompilace použity protokoly.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **konfigurace DSC**.
4. Na hello **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (hello název hello předtím naimportovali konfigurace).
5. Na hello **úlohy kompilace** dlaždice hello **TestConfig konfigurace** okně klikněte na některé z uvedených úloh hello. A **úloha kompilace** otevře se okno s názvem bez přípony se datum hello hello úloha kompilace byla spuštěna.
   
    ![Snímek obrazovky okna hello úloha kompilace](./media/automation-dsc-getting-started/CompilationJob.png)
6. Kliknutím na libovolnou dlaždici v hello **úloha kompilace** okno toosee další podrobnosti o úloze hello.

## <a name="viewing-node-configurations"></a>Zobrazení konfigurace uzlu
Úspěšné dokončení úlohy kompilace vytvoří jeden nebo více nové konfigurace uzlu. Konfigurace uzlu je dokument MOF, který je nasazený toohello načítacího serveru a připravena toobe vyžádat a použít jeden nebo více uzly. Konfigurace uzlu hello si můžete prohlédnout v účtu Automation v hello **konfigurace uzlu DSC** okno. Konfigurace uzlu má název s hello formuláře *ConfigurationName*. *NodeName*.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **konfigurace uzlu DSC**.
   
    ![Snímek obrazovky okna konfigurace uzlu DSC hello](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Registrace virtuální počítač Azure pro správu s Azure Automation DSC.
Můžete použít Azure Automation DSC toomanage virtuálních počítačích Azure (Classic i Resource Manager), místní virtuální počítače, Linux počítačů, virtuálních počítačů AWS a místní fyzických počítačů. V tomto tématu nabídneme jak tooonboard jenom virtuální počítače Azure Resource Manager. Informace o registraci najdete v části Další typy počítačů, [registrace počítačů pro správu Azure Automation DSC](automation-dsc-onboarding.md).

### <a name="tooonboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>tooonboard virtuální počítač Azure Resource Manager pro správu Azure Automation DSC.
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.
4. V hello **uzly DSC** okně klikněte na tlačítko **přidat virtuální počítač Azure**.
   
    ![Snímek obrazovky okna uzly DSC hello zvýraznění hello tlačítko Přidat virtuální počítač Azure](./media/automation-dsc-getting-started/OnboardVM.png)
5. V hello **přidat virtuální počítače Azure** okně klikněte na tlačítko **vyberte virtuální počítače tooonboard**.
6. V hello **vyberte virtuální počítače** okně vyberte hello tooonboard a klikněte na virtuální počítač **OK**.
   
   > [!IMPORTANT]
   > Tato hodnota musí být virtuální počítač Azure Resource Manager systémem Windows Server 2008 R2 nebo novější.
   > 
   > 
7. V hello **přidat virtuální počítače Azure** okně klikněte na tlačítko **konfigurace registrační data**.
8. V hello **registrace** okno, zadejte název hello hello konfigurace uzlu, na které má tooapply toohello virtuálních počítačů v hello **název konfigurace uzlu** pole. Toto musí přesně shodovat hello název konfigurace uzlu v hello účet Automation. Poskytnutí názvu v tomto okamžiku je volitelné. Konfigurace uzlu hello přiřazené po registraci hello uzlu, můžete změnit.
   Zkontrolujte **restartovat uzel v případě potřeby**a potom klikněte na **OK**.
   
    ![Snímek obrazovky okna registrace hello](./media/automation-dsc-getting-started/RegisterVM.png)
   
    Konfigurace uzlu Hello jste zadali bude použité toohello virtuálních počítačů v intervalech určeného hello **frekvence režimu konfigurace**, a hello virtuálních počítačů bude kontrolovat aktualizace konfigurace uzlu toohello v intervalech určeného hello  **Obnovovací frekvence**. Další informace o tom, jak se používají tyto hodnoty, najdete v části [hello konfigurace správce místní konfigurace](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
9. V hello **přidat virtuální počítače Azure** okně klikněte na tlačítko **vytvořit**.

Azure začne hello proces registrace hello virtuálních počítačů. Po dokončení hello virtuálního počítače se zobrazí v hello **uzly DSC** okno v hello účet Automation.

## <a name="viewing-hello-list-of-dsc-nodes"></a>Zobrazení seznamu hello uzlů DSC
Můžete zobrazit seznam všech počítačů, které byly zařazený, nemá pro správu ve vašem účtu Automation v hello hello **uzly DSC** okno.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.

## <a name="viewing-reports-for-dsc-nodes"></a>Prohlížení sestav pro uzly DSC
Pokaždé, když Azure Automation DSC provede kontrolu konzistence na spravovaný uzel, uzel hello odešle serveru vyžádání back toohello sestav stavu. Tyto sestavy můžete zobrazit v okně hello pro tento uzel.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.
4. Na hello **sestavy** dlaždici, klikněte na libovolný hello sestavy v seznamu hello.
   
    ![Snímek obrazovky okna sestavy hello](./media/automation-dsc-getting-started/NodeReport.png)

V okně hello jednotlivé sestavy můžete zjistit hello následující informace o stavu odpovídající konzistenci hello zkontrolujte:

* Hello zprávy o stavu – hello uzel "Kompatibilní", konfigurace hello "Failed", zda je uzel hello je "Neodpovídá" (Pokud je uzel hello v **applyandmonitor** režim a hello počítač není ve stavu hello potřeby).
* Hello počáteční čas pro kontrolu konzistence hello.
* Zkontrolujte celkové doby běhu Hello konzistenci hello.
* Zkontrolujte typ Hello konzistence.
* Všechny chyby, včetně hello chyba kódu a chybové zprávy. 
* Veškeré prostředky DSC v hello konfigurace a stavu hello každého prostředku (zda uzel hello je ve stavu hello požadovaných pro tento prostředek) – kliknutím na každého prostředku tooget podrobnější informace pro tento prostředek.
* Hello název, IP adresa a režim konfigurace uzlu hello.

Můžete také kliknout na **zobrazit nezpracované sestavy** hello toosee skutečné se data, která hello uzlu, odešle toohello server. Další informace o používání dat najdete v tématu [pomocí serveru sestav DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).

Může trvat delší dobu, po uzel, který je zařazený nemá před hello první sestava je k dispozici. Bude pravděpodobně nutné toowait až too30 minut pro první sestavu hello po můžete zaváděním uzlu.

## <a name="reassigning-a-node-tooa-different-node-configuration"></a>Opětovné přiřazování konfigurace uzlu tooa jiného uzlu
Toouse uzlu můžete přiřadit konfiguraci jiného uzlu, než jeden, ke které původně přiřazený hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.
4. Na hello **uzly DSC** okně klikněte na název hello hello uzlu chcete tooreassign.
5. V okně hello pro tento uzel, klikněte na tlačítko **přiřazení uzlu**.
   
    ![Snímek obrazovky okna uzlu hello zvýraznění tlačítko přiřadit uzlu hello](./media/automation-dsc-getting-started/AssignNode.png)
6. Na hello **přiřadit konfigurace uzlu** okně, vyberte hello uzlu Konfigurace toowhich tooassign hello uzel a pak klikněte na **OK**.
   
    ![Snímek obrazovky okna hello přiřadit konfigurace uzlu](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Zrušení registrace uzlu
Pokud již nechcete uzlu toobe, spravuje Azure Automation DSC, můžete zrušení její registrace.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.
3. Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.
4. Na hello **uzly DSC** okně klikněte na název hello hello uzlu chcete toounregister.
5. V okně hello pro tento uzel, klikněte na tlačítko **Unregister**.
   
    ![Snímek obrazovky okna uzlu hello zvýraznění tlačítko Unregister hello](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Související články
* [Přehled služby Azure Automation DSC](automation-dsc-overview.md)
* [Registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md)
* [Přehled stavu konfigurace požadovaného prostředí Windows PowerShell](https://msdn.microsoft.com/powershell/dsc/overview)
* [Rutiny Azure Automation DSC](/powershell/module/azurerm.automation/#automation)
* [Ceny služby Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)

