---
title: "aaaAutomating nasazení virtuálního počítače ve službě Amazon Web Services | Microsoft Docs"
description: "Tento článek ukazuje, jak toouse Azure Automation tooautomate vytvoření virtuálního počítače Amazon webové služby"
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 1d85c01a-d795-4523-8194-84fc15b53838
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/14/2017
ms.author: tiandert; bwren
ms.openlocfilehash: dd1f3af47d8700ced85a3ee5a406dde1c1311990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Automation scénář – zřizování se AWS virtuální počítač
V tomto článku jsme ukazují, jak můžete využít Azure Automation tooprovision virtuálního počítače ve vašem předplatném Amazon Web Service (AWS) a pojmenujte tohoto virtuálního počítače konkrétní – které AWS odkazuje tooas "označování" hello virtuálních počítačů.

## <a name="prerequisites"></a>Požadavky
Pro účely hello tohoto článku budete potřebovat toohave účet automatizace Azure a předplatné AWS. Další informace o vytvoření účtu Azure Automation a nastavit ho pomocí svých přihlašovacích údajů AWS předplatné, zkontrolujte [konfigurace ověřování pomocí Amazon Web Services](automation-configure-aws-account.md).  Tento účet by měl vytvořit ani aktualizovat pomocí svých přihlašovacích údajů AWS předplatné než budete pokračovat, jak jsme bude odkazovat na tento účet v následujících kroků hello.

## <a name="deploy-amazon-web-services-powershell-module"></a>Nasazení modulu Amazon Web Services prostředí PowerShell
Náš runbook zřizování virtuálních počítačů bude využívat toodo modulu prostředí PowerShell AWS hello svou práci. Proveďte následující kroky tooadd hello modulu tooyour automatizace účet, který je nakonfigurován pomocí svých přihlašovacích údajů AWS předplatné hello.  

1. Otevřete webový prohlížeč a přejděte toohello [Galerie prostředí PowerShell](http://www.powershellgallery.com/packages/AWSPowerShell/) a klikněte na hello **nasadit tooAzure automatizace tlačítko**.<br><br> ![Import modulu AWS PS](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Budete přesměrováni toohello Azure přihlašovací stránku a po ověření, budou směrované toohello portálu Azure a zobrazí se následující okno hello.<br><br> ![Import modulu okno](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Hello vyberte skupinu prostředků z hello **skupiny prostředků** rozevíracího seznamu a v okně hello parametry, zadejte hello následující informace:
   
   * Z hello **nový nebo existující účet služby Automation (string)** rozevíracího seznamu vyberte **existující**.  
   * V hello **název účtu Automation (string)** pole, zadejte přesný název hello hello účet Automation, který zahrnuje hello přihlašovací údaje pro vaše předplatné AWS.  Například, pokud jste vytvořili vyhrazený účet s názvem **AWSAutomation**, je typ v poli hello.
   * Vyberte hello odpovídající oblast z hello **umístění účtu Automation** rozevíracího seznamu.
4. Pokud jste vyplnili vstup hello požadované informace, klikněte na tlačítko **vytvořit**.
   
   > [!NOTE]
   > Při importu do Azure Automation modul prostředí PowerShell, ho je také extrahování hello rutiny a tyto aktivity se nezobrazí, dokud hello modulu úplně nedokončí import a extrahování hello rutiny. Tento proces může trvat několik minut.  
   > <br>
   > 
   > 
5. V hello portálu Azure otevřete účet Automation z kroku 3.
6. Klikněte na hello **prostředky** dlaždici a na hello **prostředky** okně, vyberte hello **moduly** dlaždici.
7. Na hello **moduly** okně uvidíte hello **AWSPowerShell** modulu v seznamu hello.

## <a name="create-aws-deploy-vm-runbook"></a>Vytvoření AWS nasazení virtuálních počítačů sady runbook
Jednou hello nasazení modulu PowerShell AWS, můžete nyní vytváříme runbook tooautomate, zřízení virtuálního počítače v AWS pomocí skriptu prostředí PowerShell. Hello následující postup popisuje, jak tooleverage nativní skript prostředí PowerShell ve službě Azure Automation.  

> [!NOTE]
> Pro další možnosti a informace týkající se tento skript, navštivte hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/New-AwsVM/DisplayScript).
> 

1. Stáhněte skript prostředí PowerShell hello New-AwsVM z hello Galerie prostředí PowerShell otevřením relace prostředí PowerShell a zadáním hello následující:<br>
   ```
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Z hello portálu Azure otevřete účet Automation a klikněte na tlačítko hello **Runbooky** dlaždici.  
3. Z hello **Runbooky** vyberte **přidat runbook**.
4. Na hello **přidat runbook** vyberte **rychle vytvořit** (vytvořit novou sadu runbook).
5. Na hello **Runbook** vlastnosti okno, zadejte název do hello název vaší sady runbook a z hello **typ Runbooku** rozevíracího seznamu vyberte **prostředí PowerShell**a pak klikněte na tlačítko  **Vytvoření**.<br><br> ![Import modulu okno](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. Jakmile se zobrazí okno Úprava Powershellového Runbooku hello, zkopírujte a vložte skript prostředí PowerShell hello do runbooku hello vytváření plátno.<br><br> ![Skript prostředí PowerShell sady Runbook](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Je třeba počítat hello následující při práci s hello příklad skriptu prostředí PowerShell:
    > 
    > * Hello runbook obsahuje počet výchozí hodnoty parametrů. Posuďte všechny výchozí hodnoty a aktualizovat v případě potřeby.
    > * Pokud máte uložené přihlašovací údaje AWS jako asset přihlašovacích údajů jiný název než **AWScred**, budete potřebovat tooupdate hello skriptu na řádku 57 toomatch odpovídajícím způsobem.  
    > * Při práci s hello rozhraní příkazového řádku AWS příkazy v prostředí PowerShell, zejména s Tento ukázkový runbook, je nutné zadat hello AWS oblast. Hello rutiny, jinak nebude úspěšná.  Zobrazení AWS tématu [zadejte oblast AWS](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) v hello AWS nástroje pro dokument prostředí PowerShell pro další podrobnosti.  
    >

7. tooretrieve seznam názvů bitové kopie ze svého předplatného AWS, spusťte prostředí PowerShell ISE a importovat hello AWS modul prostředí PowerShell.  Ověřování na základě AWS nahrazením **Get-AutomationPSCredential** ve vašem prostředí ISE s **AWScred = Get-Credential**.  To vás vyzve k zadání pověření a může poskytnout vaše **Access Key ID** pro uživatelské jméno hello a **tajný přístupový klíč** hello hesla.  Viz následující příklad hello:  

        #Sample tooget hello AWS VM available images
        #Please provide hello path where you have downloaded hello AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up hello environment tooaccess AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile

    Hello následující výstup se vrátí:<br><br>
   ![Získat AWS obrázků](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Zkopírujte a vložte hello mezi názvy hello obrázků v proměnné Automation za použití hello runbook jako **$InstanceType**. Vzhledem k tomu, že v tomto příkladu používáme hello vrstvené AWS bezplatné předplatné, použijeme **t2.micro** pro náš příklad sady runbook.  
9. Uložte hello runbook a poté klikněte na tlačítko **publikovat** toopublish hello runbook a potom **Ano** po zobrazení výzvy.

### <a name="testing-hello-aws-vm-runbook"></a>Testování hello runbook AWS virtuálních počítačů
Před pokračováním se sadou runbook testování hello, potřebujeme tooverify pár věcí. Zejména:  

* Prostředek pro ověřování proti AWS vytvořila volané **AWScred** nebo skriptu hello byl aktualizovaný tooreference hello název asset přihlašovacích údajů.    
* modul prostředí PowerShell AWS Hello importu v Azure Automation.  
* Byla vytvořena nová sada runbook a byl ověřen a podle potřeby je aktualizován hodnoty parametru  
* **Protokolování podrobných záznamů** a volitelně **protokolování záznamů o průběhu** v části nastavení sady runbook hello **protokolování a trasování** příliš nastavili**na**.<br><br> ![Protokolování sad Runbook a trasování](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. Jsme má toostart hello runbook, proto klikněte na **spustit** a pak klikněte na **OK** po otevření okna spuštění Runbooku hello.
2. V okně hello spustit sadu Runbook, zadejte **VMname**.  Přijměte hello výchozí hodnoty pro hello dalších parametrů, které dříve předkonfigurované ve skriptu hello.  Klikněte na tlačítko **OK** úlohy runbooku toostart hello.<br><br> ![Spuštění AwsVM nový runbook](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Podokno úlohy je otevřené hello úlohy runbooku, který jsme právě vytvořili. Zavřete toto podokno.
4. Jsme můžete zobrazit průběh úlohy a zobrazte výstup hello **datové proudy** výběrem hello **všechny protokoly** dlaždici v okně úlohy sady runbook hello.<br><br> ![Výstup datového proudu](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. tooconfirm hello, který je zřizovaný virtuální počítač, přihlaste se k hello konzoly pro správu AWS, pokud se neprotokolují aktuálně.<br><br> ![Konzoly AWS nasazení virtuálních počítačů](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Další kroky
* tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md)
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)
* tooknow Další informace o typech runbooků, jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md)
* Další informace o funkci podpory powershellových skriptů najdete v článku [Nativní podpora powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

