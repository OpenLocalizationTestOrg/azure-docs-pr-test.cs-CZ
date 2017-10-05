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
# <a name="certificate-assets-in-azure-automation"></a><span data-ttu-id="63081-104">Certifikát prostředky ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="63081-104">Certificate assets in Azure Automation</span></span>

<span data-ttu-id="63081-105">Certifikáty můžete bezpečně uloženy ve službě Azure Automation, přístupná pomocí sady runbook nebo konfigurací DSC pomocí **Get-AzureRmAutomationRmCertificate** aktivity pro prostředky Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="63081-105">Certificates can be stored securely in Azure Automation so they can be accessed by runbooks or DSC configurations using the **Get-AzureRmAutomationRmCertificate** activity for Azure Resource Manager resources.</span></span> <span data-ttu-id="63081-106">To vám umožní vytvořit runboocích a konfiguracích DSC, které používají certifikáty k ověřování nebo přidá je do Azure nebo třetích stran prostředky.</span><span class="sxs-lookup"><span data-stu-id="63081-106">This allows you to create runbooks and DSC configurations that use certificates for authentication or adds them to Azure or third party resources.</span></span>

> [!NOTE] 
> <span data-ttu-id="63081-107">Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné.</span><span class="sxs-lookup"><span data-stu-id="63081-107">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="63081-108">Tyto prostředky jsou zašifrovány a uložené ve službě Azure Automation pomocí jedinečný klíč, který se vygeneruje pro každý účet automation.</span><span class="sxs-lookup"><span data-stu-id="63081-108">These assets are encrypted and stored in the Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="63081-109">Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="63081-109">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="63081-110">Před ukládání o zabezpečený prostředek, klíč pro účet služby automation jsou dešifrována pomocí hlavního certifikátu a pak se použije k zašifrování asset.</span><span class="sxs-lookup"><span data-stu-id="63081-110">Before storing a secure asset, the key for the automation account is decrypted using the master certificate and then used to encrypt the asset.</span></span>
> 

## <a name="windows-powershell-cmdlets"></a><span data-ttu-id="63081-111">Rutiny prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="63081-111">Windows PowerShell Cmdlets</span></span>

<span data-ttu-id="63081-112">Rutiny v následující tabulce se používají k vytváření a správě certifikátu prostředky automation pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="63081-112">The cmdlets in the following table are used to create and manage automation certificate assets with Windows PowerShell.</span></span> <span data-ttu-id="63081-113">Se dodávají jako součást [modul Azure PowerShell](../powershell-install-configure.md) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="63081-113">They ship as part of the [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configurations.</span></span>

|<span data-ttu-id="63081-114">Rutiny</span><span class="sxs-lookup"><span data-stu-id="63081-114">Cmdlets</span></span>|<span data-ttu-id="63081-115">Popis</span><span class="sxs-lookup"><span data-stu-id="63081-115">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="63081-116">Get-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="63081-116">Get-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603765.aspx)|<span data-ttu-id="63081-117">Načte informace o certifikátu pro použití v runbooku nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="63081-117">Retrieves information about a certificate to use in a runbook or DSC configuration.</span></span> <span data-ttu-id="63081-118">Samotný certifikát můžete načíst jenom z Get-AutomationCertificate aktivity.</span><span class="sxs-lookup"><span data-stu-id="63081-118">You can only retrieve the certificate itself from Get-AutomationCertificate activity.</span></span>|
|[<span data-ttu-id="63081-119">Nové AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="63081-119">New-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603604.aspx)|<span data-ttu-id="63081-120">Vytvoří nový certifikát do Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="63081-120">Creates a new certificate into Azure Automation.</span></span>|
[<span data-ttu-id="63081-121">Odebrat AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="63081-121">Remove-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603529.aspx)|<span data-ttu-id="63081-122">Odebere certifikát Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="63081-122">Removes a certificate from Azure Automation.</span></span>|<span data-ttu-id="63081-123">Vytvoří nový certifikát do Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="63081-123">Creates a new certificate into Azure Automation.</span></span>
|[<span data-ttu-id="63081-124">Set-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="63081-124">Set-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603760.aspx)|<span data-ttu-id="63081-125">Nastaví vlastnosti existujícího certifikátu včetně nahrání souboru certifikátu a nastavení hesla pro .pfx.</span><span class="sxs-lookup"><span data-stu-id="63081-125">Sets the properties for an existing certificate including uploading the certificate file and setting the password for a .pfx.</span></span>|
|[<span data-ttu-id="63081-126">Přidat AzureCertificate</span><span class="sxs-lookup"><span data-stu-id="63081-126">Add-AzureCertificate</span></span>](https://msdn.microsoft.com/library/azure/dn495214.aspx)|<span data-ttu-id="63081-127">Odešle certifikát služby pro zadané cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="63081-127">Uploads a service certificate for the specified cloud service.</span></span>|


## <a name="creating-a-new-certificate"></a><span data-ttu-id="63081-128">Vytvoření nového certifikátu</span><span class="sxs-lookup"><span data-stu-id="63081-128">Creating a new certificate</span></span>

<span data-ttu-id="63081-129">Když vytvoříte nový certifikát, odeslat soubor .cer nebo .pfx pro Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="63081-129">When you create a new certificate, you upload a .cer or .pfx file to Azure Automation.</span></span> <span data-ttu-id="63081-130">Pokud označíte certifikátu jako exportovatelný, můžete ho přenést z úložiště certifikátů Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="63081-130">If you mark the certificate as exportable, then you can transfer it out of the Azure Automation certificate store.</span></span> <span data-ttu-id="63081-131">Pokud není exportovatelný, pak je můžete jenom použije pro podepisování v rámci sady runbook nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="63081-131">If it is not exportable, then it can only be used for signing within the runbook or DSC configuration.</span></span>


### <a name="to-create-a-new-certificate-with-the-azure-portal"></a><span data-ttu-id="63081-132">Chcete-li vytvořit nový certifikát pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="63081-132">To create a new certificate with the Azure portal</span></span>

1. <span data-ttu-id="63081-133">Z vašeho účtu Automation, klikněte **prostředky** dlaždici otevřete **prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="63081-133">From your Automation account, click the **Assets** tile to open the **Assets** blade.</span></span>
1. <span data-ttu-id="63081-134">Klikněte **certifikáty** dlaždici otevřete **certifikáty** okno.</span><span class="sxs-lookup"><span data-stu-id="63081-134">Click the **Certificates** tile to open the **Certificates** blade.</span></span>
1. <span data-ttu-id="63081-135">Klikněte na tlačítko **přidat certifikát** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="63081-135">Click **Add a certificate** at the top of the blade.</span></span>
2. <span data-ttu-id="63081-136">Zadejte název certifikátu **název** pole.</span><span class="sxs-lookup"><span data-stu-id="63081-136">Type a name for the certificate in the **Name** box.</span></span>
2. <span data-ttu-id="63081-137">Klikněte na tlačítko **vyberte soubor** pod **nahrát soubor s certifikátem** a vyhledejte soubor .cer nebo .pfx.</span><span class="sxs-lookup"><span data-stu-id="63081-137">Click **Select a file** under **Upload a certificate file** to browse for a .cer or .pfx file.</span></span>  <span data-ttu-id="63081-138">Pokud jste vybrali soubor .pfx, zadejte heslo a jestli by měl být povolen export.</span><span class="sxs-lookup"><span data-stu-id="63081-138">If you select a .pfx file, specify a password and whether it should be allowed to be exported.</span></span>
1. <span data-ttu-id="63081-139">Klikněte na tlačítko **vytvořit** uložit nový prostředek certifikátu.</span><span class="sxs-lookup"><span data-stu-id="63081-139">Click **Create** to save the new certificate asset.</span></span>


### <a name="to-create-a-new-certificate-with-windows-powershell"></a><span data-ttu-id="63081-140">Chcete-li vytvořit nový certifikát v prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="63081-140">To create a new certificate with Windows PowerShell</span></span>

<span data-ttu-id="63081-141">Následující příklad ukazuje, jak vytvořit nový certifikát automatizace a označte ji jako exportovatelný.</span><span class="sxs-lookup"><span data-stu-id="63081-141">The following example demonstrates how to create a new Automation certificate and mark it exportable.</span></span> <span data-ttu-id="63081-142">To Importuje existující soubor .pfx.</span><span class="sxs-lookup"><span data-stu-id="63081-142">This imports an existing .pfx file.</span></span>

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a><span data-ttu-id="63081-143">Použití certifikátu</span><span class="sxs-lookup"><span data-stu-id="63081-143">Using a certificate</span></span>

<span data-ttu-id="63081-144">Je nutné použít **Get-AutomationCertificate** aktivity pro použití certifikátu.</span><span class="sxs-lookup"><span data-stu-id="63081-144">You must use the **Get-AutomationCertificate** activity to use a certificate.</span></span> <span data-ttu-id="63081-145">Nelze použít [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) rutiny vzhledem k tomu, že ji vrací informace o prostředek certifikátu, ale ne samotný certifikát.</span><span class="sxs-lookup"><span data-stu-id="63081-145">You cannot use the [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet since it returns information about the certificate asset but not the certificate itself.</span></span>

### <a name="textual-runbook-sample"></a><span data-ttu-id="63081-146">Ukázkový textový</span><span class="sxs-lookup"><span data-stu-id="63081-146">Textual runbook sample</span></span>

<span data-ttu-id="63081-147">Následující vzorový kód ukazuje, jak přidat certifikát do cloudové služby v sadě runbook.</span><span class="sxs-lookup"><span data-stu-id="63081-147">The following sample code shows how to add a certificate to a cloud service in a runbook.</span></span> <span data-ttu-id="63081-148">V této ukázce je z proměnná šifrované automatizace načíst heslo.</span><span class="sxs-lookup"><span data-stu-id="63081-148">In this sample, the password is retrieved from an encrypted automation variable.</span></span>

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a><span data-ttu-id="63081-149">Ukázka grafický runbook</span><span class="sxs-lookup"><span data-stu-id="63081-149">Graphical runbook sample</span></span>

<span data-ttu-id="63081-150">Přidání **Get-AutomationCertificate** na grafický runbook pravým tlačítkem na certifikát v podokně knihovna grafického editoru a výběrem **přidat na plátno**.</span><span class="sxs-lookup"><span data-stu-id="63081-150">You add a **Get-AutomationCertificate** to a graphical runbook by right-clicking on the certificate in the Library pane of the graphical editor and selecting **Add to canvas**.</span></span>

![Přidání certifikátu na plátno](media/automation-certificates/automation-certificate-add-to-canvas.png)

<span data-ttu-id="63081-152">Následující obrázek ukazuje příklad použití certifikátu v grafický runbook.</span><span class="sxs-lookup"><span data-stu-id="63081-152">The following image shows an example of using a certificate in a graphical runbook.</span></span>  <span data-ttu-id="63081-153">Toto je stejný příkladu výše pro přidání certifikátu do cloudové služby z textové runbooku.</span><span class="sxs-lookup"><span data-stu-id="63081-153">This is the same example shown above for adding a certificate to a cloud service from a textual runbook.</span></span>

![<span data-ttu-id="63081-154">Vytváření grafického obsahu příklad</span><span class="sxs-lookup"><span data-stu-id="63081-154">Example Graphical Authoring</span></span> ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a><span data-ttu-id="63081-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63081-155">Next steps</span></span>

- <span data-ttu-id="63081-156">Další informace o práci s odkazy na řízení logický tok vaše sada runbook je navržen pro provádění aktivit najdete v tématu [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="63081-156">To learn more about working with links to control the logical flow of activities your runbook is designed to perform, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow).</span></span> 
