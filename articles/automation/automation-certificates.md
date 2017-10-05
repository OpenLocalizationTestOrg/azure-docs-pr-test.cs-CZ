---
title: "Certifikát prostředky ve službě Azure Automation | Microsoft Docs"
description: "Certifikáty můžete bezpečně uloženy ve službě Azure Automation, takže přístupná pomocí sady runbook nebo konfigurace DSC k ověřování na základě Azure a prostředky třetích stran.  Tento článek vysvětluje podrobnosti o certifikáty a postupy pro práci s nimi v textové a grafické vytváření."
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
ms.openlocfilehash: fd1737a420c132dace9307436bfea98a9bde94a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="certificate-assets-in-azure-automation"></a>Certifikát prostředky ve službě Azure Automation

Certifikáty můžete bezpečně uloženy ve službě Azure Automation, přístupná pomocí sady runbook nebo konfigurací DSC pomocí **Get-AzureRmAutomationRmCertificate** aktivity pro prostředky Azure Resource Manager. To vám umožní vytvořit runboocích a konfiguracích DSC, které používají certifikáty k ověřování nebo přidá je do Azure nebo třetích stran prostředky.

> [!NOTE] 
> Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné. Tyto prostředky jsou zašifrovány a uložené ve službě Azure Automation pomocí jedinečný klíč, který se vygeneruje pro každý účet automation. Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation. Před ukládání o zabezpečený prostředek, klíč pro účet služby automation jsou dešifrována pomocí hlavního certifikátu a pak se použije k zašifrování asset.
> 

## <a name="windows-powershell-cmdlets"></a>Rutiny prostředí Windows PowerShell

Rutiny v následující tabulce se používají k vytváření a správě certifikátu prostředky automation pomocí prostředí Windows PowerShell. Se dodávají jako součást [modul Azure PowerShell](../powershell-install-configure.md) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.

|Rutiny|Popis|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx)|Načte informace o certifikátu pro použití v runbooku nebo konfigurace DSC. Samotný certifikát můžete načíst jenom z Get-AutomationCertificate aktivity.|
|[Nové AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603604.aspx)|Vytvoří nový certifikát do Azure Automation.|
[Odebrat AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603529.aspx)|Odebere certifikát Azure Automation.|Vytvoří nový certifikát do Azure Automation.
|[Set-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603760.aspx)|Nastaví vlastnosti existujícího certifikátu včetně nahrání souboru certifikátu a nastavení hesla pro .pfx.|
|[Přidat AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Odešle certifikát služby pro zadané cloudové služby.|


## <a name="creating-a-new-certificate"></a>Vytvoření nového certifikátu

Když vytvoříte nový certifikát, odeslat soubor .cer nebo .pfx pro Azure Automation. Pokud označíte certifikátu jako exportovatelný, můžete ho přenést z úložiště certifikátů Azure Automation. Pokud není exportovatelný, pak je můžete jenom použije pro podepisování v rámci sady runbook nebo konfigurace DSC.


### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Chcete-li vytvořit nový certifikát pomocí portálu Azure

1. Z vašeho účtu Automation, klikněte **prostředky** dlaždici otevřete **prostředky** okno.
1. Klikněte **certifikáty** dlaždici otevřete **certifikáty** okno.
1. Klikněte na tlačítko **přidat certifikát** v horní části okna.
2. Zadejte název certifikátu **název** pole.
2. Klikněte na tlačítko **vyberte soubor** pod **nahrát soubor s certifikátem** a vyhledejte soubor .cer nebo .pfx.  Pokud jste vybrali soubor .pfx, zadejte heslo a jestli by měl být povolen export.
1. Klikněte na tlačítko **vytvořit** uložit nový prostředek certifikátu.


### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Chcete-li vytvořit nový certifikát v prostředí Windows PowerShell

Následující příklad ukazuje, jak vytvořit nový certifikát automatizace a označte ji jako exportovatelný. To Importuje existující soubor .pfx.

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a>Použití certifikátu

Je nutné použít **Get-AutomationCertificate** aktivity pro použití certifikátu. Nelze použít [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) rutiny vzhledem k tomu, že ji vrací informace o prostředek certifikátu, ale ne samotný certifikát.

### <a name="textual-runbook-sample"></a>Ukázkový textový

Následující vzorový kód ukazuje, jak přidat certifikát do cloudové služby v sadě runbook. V této ukázce je z proměnná šifrované automatizace načíst heslo.

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a>Ukázka grafický runbook

Přidání **Get-AutomationCertificate** na grafický runbook pravým tlačítkem na certifikát v podokně knihovna grafického editoru a výběrem **přidat na plátno**.

![Přidání certifikátu na plátno](media/automation-certificates/automation-certificate-add-to-canvas.png)

Následující obrázek ukazuje příklad použití certifikátu v grafický runbook.  Toto je stejný příkladu výše pro přidání certifikátu do cloudové služby z textové runbooku.

![Vytváření grafického obsahu příklad ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a>Další kroky

- Další informace o práci s odkazy na řízení logický tok vaše sada runbook je navržen pro provádění aktivit najdete v tématu [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow). 
