---
title: "aaaCertificate prostředky ve službě Azure Automation | Microsoft Docs"
description: "Certifikáty můžete bezpečně uloženy ve službě Azure Automation, takže přístupná pomocí sady runbook nebo tooauthenticate konfigurace DSC s Azure a prostředky třetích stran.  Tento článek vysvětluje podrobnosti hello certifikátů a jak toowork s nimi v textové a grafické vytváření."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: ac9c22ae-501f-42b9-9543-ac841cf2cc36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 2c25bee937890438ea9022669be2c24c77a110b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="certificate-assets-in-azure-automation"></a>Certifikát prostředky ve službě Azure Automation

Certifikáty můžete bezpečně uloženy ve službě Azure Automation, přístupná pomocí sady runbook nebo konfigurací DSC pomocí hello **Get-AzureRmAutomationRmCertificate** aktivity pro prostředky Azure Resource Manager. To vám umožní toocreate runboocích a konfiguracích DSC, které používají certifikáty k ověřování nebo je přidá prostředky tooAzure nebo třetích stran.

> [!NOTE] 
> Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné. Tyto prostředky jsou zašifrovány a uložené v Azure Automation jednotlivých účtů automation pomocí jedinečný klíč, který se vygeneruje hello. Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation. Před uložením o zabezpečený prostředek, hello klíč pro účet automation hello se dešifruje pomocí hlavního certifikátu hello a pak se použije tooencrypt hello asset.
> 

## <a name="windows-powershell-cmdlets"></a>Rutiny prostředí Windows PowerShell

Hello rutiny v následující tabulce hello jsou použité toocreate a spravovat prostředky certifikátu automation pomocí prostředí Windows PowerShell. Se dodávají jako součást hello [modul Azure PowerShell](../powershell-install-configure.md) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.

|Rutiny|Popis|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx)|Načte informace o certifikátu toouse v sady runbook nebo konfigurace DSC. Vlastní certifikát hello můžete načíst jenom z Get-AutomationCertificate aktivity.|
|[Nové AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603604.aspx)|Vytvoří nový certifikát do Azure Automation.|
[Odebrat AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603529.aspx)|Odebere certifikát Azure Automation.|Vytvoří nový certifikát do Azure Automation.
|[Set-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603760.aspx)|Nastaví vlastnosti hello existujícího certifikátu včetně nahrání souboru certifikátu hello a nastavení hello hesla pro .pfx.|
|[Přidat AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Odeslání certifikátu služby pro hello zadat cloudové služby.|


## <a name="creating-a-new-certificate"></a>Vytvoření nového certifikátu

Když vytvoříte nový certifikát, nahrajete .cer nebo .pfx souboru tooAzure automatizace. Pokud označíte hello certifikátu jako exportovatelný, můžete ho přenést z úložiště certifikátů hello Azure Automation. Pokud není exportovatelný, pak je můžete jenom použije pro podepisování v rámci sady runbook hello nebo konfigurace DSC.


### <a name="toocreate-a-new-certificate-with-hello-azure-portal"></a>toocreate nový certifikát se hello portálu Azure

1. Z vašeho účtu Automation, klikněte na tlačítko hello **prostředky** dlaždice tooopen hello **prostředky** okno.
1. Klikněte na tlačítko hello **certifikáty** dlaždice tooopen hello **certifikáty** okno.
1. Klikněte na tlačítko **přidat certifikát** hello horní části okna hello.
2. Zadejte název certifikátu hello hello **název** pole.
2. Klikněte na tlačítko **vyberte soubor** pod **nahrát soubor s certifikátem** toobrowse pro soubor .cer nebo .pfx.  Pokud jste vybrali soubor .pfx, zadejte heslo a zda se má být povoleno toobe exportovali.
1. Klikněte na tlačítko **vytvořit** toosave hello nového certifikátu prostředku.


### <a name="toocreate-a-new-certificate-with-windows-powershell"></a>toocreate nový certifikát v prostředí Windows PowerShell

Hello následující příklad ukazuje, jak toocreate nové automatizace certifikátů a označte ji jako exportovatelný. To Importuje existující soubor .pfx.

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a>Použití certifikátu

Je nutné použít hello **Get-AutomationCertificate** aktivity toouse certifikát. Nemůžete použít hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) rutiny vzhledem k tomu, že ji vrací informace o hello asset certifikátu, ale není hello vlastní certifikát.

### <a name="textual-runbook-sample"></a>Ukázkový textový

Hello následující vzorový kód ukazuje, jak tooadd tooa certifikát cloudové služby v sadě runbook. V této ukázce hello heslo se načítají proměnná šifrované automatizace.

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a>Ukázka grafický runbook

Přidání **Get-AutomationCertificate** tooa grafický runbook kliknutím pravým tlačítkem na certifikát hello v podokně knihovna hello grafický editor hello a výběrem **přidat toocanvas**.

![Přidat certifikát toohello plátno](media/automation-certificates/automation-certificate-add-to-canvas.png)

Hello následující obrázek ukazuje příklad použití certifikátu v grafický runbook.  Toto je hello stejné příkladu výše pro přidání certifikátu tooa cloudové služby z textové runbooku.

![Vytváření grafického obsahu příklad ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a>Další kroky

- informace o práci s odkazy toocontrol hello logický tok aktivit vaše sada runbook je toolearn určené tooperform najdete v tématu [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow). 
