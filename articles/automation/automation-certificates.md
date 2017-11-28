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
# <a name="certificate-assets-in-azure-automation"></a><span data-ttu-id="5e724-104">Certifikát prostředky ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="5e724-104">Certificate assets in Azure Automation</span></span>

<span data-ttu-id="5e724-105">Certifikáty můžete bezpečně uloženy ve službě Azure Automation, přístupná pomocí sady runbook nebo konfigurací DSC pomocí hello **Get-AzureRmAutomationRmCertificate** aktivity pro prostředky Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5e724-105">Certificates can be stored securely in Azure Automation so they can be accessed by runbooks or DSC configurations using hello **Get-AzureRmAutomationRmCertificate** activity for Azure Resource Manager resources.</span></span> <span data-ttu-id="5e724-106">To vám umožní toocreate runboocích a konfiguracích DSC, které používají certifikáty k ověřování nebo je přidá prostředky tooAzure nebo třetích stran.</span><span class="sxs-lookup"><span data-stu-id="5e724-106">This allows you toocreate runbooks and DSC configurations that use certificates for authentication or adds them tooAzure or third party resources.</span></span>

> [!NOTE] 
> <span data-ttu-id="5e724-107">Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné.</span><span class="sxs-lookup"><span data-stu-id="5e724-107">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="5e724-108">Tyto prostředky jsou zašifrovány a uložené v Azure Automation jednotlivých účtů automation pomocí jedinečný klíč, který se vygeneruje hello.</span><span class="sxs-lookup"><span data-stu-id="5e724-108">These assets are encrypted and stored in hello Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="5e724-109">Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5e724-109">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="5e724-110">Před uložením o zabezpečený prostředek, hello klíč pro účet automation hello se dešifruje pomocí hlavního certifikátu hello a pak se použije tooencrypt hello asset.</span><span class="sxs-lookup"><span data-stu-id="5e724-110">Before storing a secure asset, hello key for hello automation account is decrypted using hello master certificate and then used tooencrypt hello asset.</span></span>
> 

## <a name="windows-powershell-cmdlets"></a><span data-ttu-id="5e724-111">Rutiny prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e724-111">Windows PowerShell Cmdlets</span></span>

<span data-ttu-id="5e724-112">Hello rutiny v následující tabulce hello jsou použité toocreate a spravovat prostředky certifikátu automation pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5e724-112">hello cmdlets in hello following table are used toocreate and manage automation certificate assets with Windows PowerShell.</span></span> <span data-ttu-id="5e724-113">Se dodávají jako součást hello [modul Azure PowerShell](../powershell-install-configure.md) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="5e724-113">They ship as part of hello [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configurations.</span></span>

|<span data-ttu-id="5e724-114">Rutiny</span><span class="sxs-lookup"><span data-stu-id="5e724-114">Cmdlets</span></span>|<span data-ttu-id="5e724-115">Popis</span><span class="sxs-lookup"><span data-stu-id="5e724-115">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="5e724-116">Get-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="5e724-116">Get-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603765.aspx)|<span data-ttu-id="5e724-117">Načte informace o certifikátu toouse v sady runbook nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="5e724-117">Retrieves information about a certificate toouse in a runbook or DSC configuration.</span></span> <span data-ttu-id="5e724-118">Vlastní certifikát hello můžete načíst jenom z Get-AutomationCertificate aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e724-118">You can only retrieve hello certificate itself from Get-AutomationCertificate activity.</span></span>|
|[<span data-ttu-id="5e724-119">Nové AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="5e724-119">New-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603604.aspx)|<span data-ttu-id="5e724-120">Vytvoří nový certifikát do Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5e724-120">Creates a new certificate into Azure Automation.</span></span>|
[<span data-ttu-id="5e724-121">Odebrat AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="5e724-121">Remove-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603529.aspx)|<span data-ttu-id="5e724-122">Odebere certifikát Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5e724-122">Removes a certificate from Azure Automation.</span></span>|<span data-ttu-id="5e724-123">Vytvoří nový certifikát do Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5e724-123">Creates a new certificate into Azure Automation.</span></span>
|[<span data-ttu-id="5e724-124">Set-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="5e724-124">Set-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603760.aspx)|<span data-ttu-id="5e724-125">Nastaví vlastnosti hello existujícího certifikátu včetně nahrání souboru certifikátu hello a nastavení hello hesla pro .pfx.</span><span class="sxs-lookup"><span data-stu-id="5e724-125">Sets hello properties for an existing certificate including uploading hello certificate file and setting hello password for a .pfx.</span></span>|
|[<span data-ttu-id="5e724-126">Přidat AzureCertificate</span><span class="sxs-lookup"><span data-stu-id="5e724-126">Add-AzureCertificate</span></span>](https://msdn.microsoft.com/library/azure/dn495214.aspx)|<span data-ttu-id="5e724-127">Odeslání certifikátu služby pro hello zadat cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="5e724-127">Uploads a service certificate for hello specified cloud service.</span></span>|


## <a name="creating-a-new-certificate"></a><span data-ttu-id="5e724-128">Vytvoření nového certifikátu</span><span class="sxs-lookup"><span data-stu-id="5e724-128">Creating a new certificate</span></span>

<span data-ttu-id="5e724-129">Když vytvoříte nový certifikát, nahrajete .cer nebo .pfx souboru tooAzure automatizace.</span><span class="sxs-lookup"><span data-stu-id="5e724-129">When you create a new certificate, you upload a .cer or .pfx file tooAzure Automation.</span></span> <span data-ttu-id="5e724-130">Pokud označíte hello certifikátu jako exportovatelný, můžete ho přenést z úložiště certifikátů hello Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5e724-130">If you mark hello certificate as exportable, then you can transfer it out of hello Azure Automation certificate store.</span></span> <span data-ttu-id="5e724-131">Pokud není exportovatelný, pak je můžete jenom použije pro podepisování v rámci sady runbook hello nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="5e724-131">If it is not exportable, then it can only be used for signing within hello runbook or DSC configuration.</span></span>


### <a name="toocreate-a-new-certificate-with-hello-azure-portal"></a><span data-ttu-id="5e724-132">toocreate nový certifikát se hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5e724-132">toocreate a new certificate with hello Azure portal</span></span>

1. <span data-ttu-id="5e724-133">Z vašeho účtu Automation, klikněte na tlačítko hello **prostředky** dlaždice tooopen hello **prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="5e724-133">From your Automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
1. <span data-ttu-id="5e724-134">Klikněte na tlačítko hello **certifikáty** dlaždice tooopen hello **certifikáty** okno.</span><span class="sxs-lookup"><span data-stu-id="5e724-134">Click hello **Certificates** tile tooopen hello **Certificates** blade.</span></span>
1. <span data-ttu-id="5e724-135">Klikněte na tlačítko **přidat certifikát** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="5e724-135">Click **Add a certificate** at hello top of hello blade.</span></span>
2. <span data-ttu-id="5e724-136">Zadejte název certifikátu hello hello **název** pole.</span><span class="sxs-lookup"><span data-stu-id="5e724-136">Type a name for hello certificate in hello **Name** box.</span></span>
2. <span data-ttu-id="5e724-137">Klikněte na tlačítko **vyberte soubor** pod **nahrát soubor s certifikátem** toobrowse pro soubor .cer nebo .pfx.</span><span class="sxs-lookup"><span data-stu-id="5e724-137">Click **Select a file** under **Upload a certificate file** toobrowse for a .cer or .pfx file.</span></span>  <span data-ttu-id="5e724-138">Pokud jste vybrali soubor .pfx, zadejte heslo a zda se má být povoleno toobe exportovali.</span><span class="sxs-lookup"><span data-stu-id="5e724-138">If you select a .pfx file, specify a password and whether it should be allowed toobe exported.</span></span>
1. <span data-ttu-id="5e724-139">Klikněte na tlačítko **vytvořit** toosave hello nového certifikátu prostředku.</span><span class="sxs-lookup"><span data-stu-id="5e724-139">Click **Create** toosave hello new certificate asset.</span></span>


### <a name="toocreate-a-new-certificate-with-windows-powershell"></a><span data-ttu-id="5e724-140">toocreate nový certifikát v prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e724-140">toocreate a new certificate with Windows PowerShell</span></span>

<span data-ttu-id="5e724-141">Hello následující příklad ukazuje, jak toocreate nové automatizace certifikátů a označte ji jako exportovatelný.</span><span class="sxs-lookup"><span data-stu-id="5e724-141">hello following example demonstrates how toocreate a new Automation certificate and mark it exportable.</span></span> <span data-ttu-id="5e724-142">To Importuje existující soubor .pfx.</span><span class="sxs-lookup"><span data-stu-id="5e724-142">This imports an existing .pfx file.</span></span>

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a><span data-ttu-id="5e724-143">Použití certifikátu</span><span class="sxs-lookup"><span data-stu-id="5e724-143">Using a certificate</span></span>

<span data-ttu-id="5e724-144">Je nutné použít hello **Get-AutomationCertificate** aktivity toouse certifikát.</span><span class="sxs-lookup"><span data-stu-id="5e724-144">You must use hello **Get-AutomationCertificate** activity toouse a certificate.</span></span> <span data-ttu-id="5e724-145">Nemůžete použít hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) rutiny vzhledem k tomu, že ji vrací informace o hello asset certifikátu, ale není hello vlastní certifikát.</span><span class="sxs-lookup"><span data-stu-id="5e724-145">You cannot use hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet since it returns information about hello certificate asset but not hello certificate itself.</span></span>

### <a name="textual-runbook-sample"></a><span data-ttu-id="5e724-146">Ukázkový textový</span><span class="sxs-lookup"><span data-stu-id="5e724-146">Textual runbook sample</span></span>

<span data-ttu-id="5e724-147">Hello následující vzorový kód ukazuje, jak tooadd tooa certifikát cloudové služby v sadě runbook.</span><span class="sxs-lookup"><span data-stu-id="5e724-147">hello following sample code shows how tooadd a certificate tooa cloud service in a runbook.</span></span> <span data-ttu-id="5e724-148">V této ukázce hello heslo se načítají proměnná šifrované automatizace.</span><span class="sxs-lookup"><span data-stu-id="5e724-148">In this sample, hello password is retrieved from an encrypted automation variable.</span></span>

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a><span data-ttu-id="5e724-149">Ukázka grafický runbook</span><span class="sxs-lookup"><span data-stu-id="5e724-149">Graphical runbook sample</span></span>

<span data-ttu-id="5e724-150">Přidání **Get-AutomationCertificate** tooa grafický runbook kliknutím pravým tlačítkem na certifikát hello v podokně knihovna hello grafický editor hello a výběrem **přidat toocanvas**.</span><span class="sxs-lookup"><span data-stu-id="5e724-150">You add a **Get-AutomationCertificate** tooa graphical runbook by right-clicking on hello certificate in hello Library pane of hello graphical editor and selecting **Add toocanvas**.</span></span>

![Přidat certifikát toohello plátno](media/automation-certificates/automation-certificate-add-to-canvas.png)

<span data-ttu-id="5e724-152">Hello následující obrázek ukazuje příklad použití certifikátu v grafický runbook.</span><span class="sxs-lookup"><span data-stu-id="5e724-152">hello following image shows an example of using a certificate in a graphical runbook.</span></span>  <span data-ttu-id="5e724-153">Toto je hello stejné příkladu výše pro přidání certifikátu tooa cloudové služby z textové runbooku.</span><span class="sxs-lookup"><span data-stu-id="5e724-153">This is hello same example shown above for adding a certificate tooa cloud service from a textual runbook.</span></span>

![<span data-ttu-id="5e724-154">Vytváření grafického obsahu příklad</span><span class="sxs-lookup"><span data-stu-id="5e724-154">Example Graphical Authoring</span></span> ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a><span data-ttu-id="5e724-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e724-155">Next steps</span></span>

- <span data-ttu-id="5e724-156">informace o práci s odkazy toocontrol hello logický tok aktivit vaše sada runbook je toolearn určené tooperform najdete v tématu [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="5e724-156">toolearn more about working with links toocontrol hello logical flow of activities your runbook is designed tooperform, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow).</span></span> 
