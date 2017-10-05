---
title: "Povolení vzdáleného ladění s nastavené průběžné doručování | Microsoft Docs"
description: "Postup povolení vzdáleného ladění při použití nastavené průběžné doručování k nasazení do Azure"
services: cloud-services
documentationcenter: .net
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d423639-3b2f-4ca5-ac5a-9ac19a217c29
ms.service: cloud-services
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 7a8a853a93e3e9915f687a20c871444e6a0de50d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a><span data-ttu-id="8804d-103">Aktivace vzdáleného ladění při použití průběžného odesílání k publikování v Azure</span><span class="sxs-lookup"><span data-stu-id="8804d-103">Enable remote debugging when using continuous delivery to publish to Azure</span></span>
<span data-ttu-id="8804d-104">Můžete povolit vzdálené ladění v Azure, pro cloudové služby nebo virtuálního počítače, při použití [nastavené průběžné doručování](cloud-services-dotnet-continuous-delivery.md) publikovat do Azure pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="8804d-104">You can enable remote debugging in Azure, for cloud services or virtual machines, when you use [continuous delivery](cloud-services-dotnet-continuous-delivery.md) to publish to Azure by following these steps.</span></span>

## <a name="enabling-remote-debugging-for-cloud-services"></a><span data-ttu-id="8804d-105">Povolení vzdáleného ladění pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="8804d-105">Enabling remote debugging for cloud services</span></span>
1. <span data-ttu-id="8804d-106">V agentovi sestavení nastavení počáteční prostředí pro Azure jak je uvedeno v [sestavení příkazového řádku pro Azure](http://msdn.microsoft.com/library/hh535755.aspx).</span><span class="sxs-lookup"><span data-stu-id="8804d-106">On the build agent, set up the initial environment for Azure as outlined in [Command-Line Build for Azure](http://msdn.microsoft.com/library/hh535755.aspx).</span></span>
2. <span data-ttu-id="8804d-107">Protože je vyžadován pro balíček vzdáleného ladění runtime (msvsmon.exe), nainstalujte **nástrojů pro vzdálenou pro Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="8804d-107">Because the remote debug runtime (msvsmon.exe) is required for the package, install the **Remote Tools for Visual Studio**.</span></span>

    * [<span data-ttu-id="8804d-108">Nástroje pro vzdálenou pro Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8804d-108">Remote Tools for Visual Studio 2017</span></span>](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [<span data-ttu-id="8804d-109">Nástroje pro vzdálenou pro Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="8804d-109">Remote Tools for Visual Studio 2015</span></span>](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [<span data-ttu-id="8804d-110">Nástroje pro vzdálenou pro Visual Studio 2013 Update 5</span><span class="sxs-lookup"><span data-stu-id="8804d-110">Remote Tools for Visual Studio 2013 Update 5</span></span>](https://www.microsoft.com/download/details.aspx?id=48156)
    
    <span data-ttu-id="8804d-111">Jako alternativu můžete zkopírovat binární soubory pro vzdálené ladění ze systému, který má nainstalovanou sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8804d-111">As an alternative, you can copy the remote debug binaries from a system that has Visual Studio installed.</span></span>

3. <span data-ttu-id="8804d-112">Vytvoření certifikátu, jak je uvedeno v [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="8804d-112">Create a certificate as outlined in [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="8804d-113">Udržujte .pfx a kryptografický otisk certifikátu protokolu RDP a nahrání certifikátu ke cloudové službě cíl.</span><span class="sxs-lookup"><span data-stu-id="8804d-113">Keep the .pfx and RDP certificate thumbprint and upload the certificate to the target cloud service.</span></span>
4. <span data-ttu-id="8804d-114">Pomocí následujících možností v příkazovém řádku MSBuild sestavení a balíček s vzdáleného ladění povoleno.</span><span class="sxs-lookup"><span data-stu-id="8804d-114">Use the following options in the MSBuild command line to build and package with remote debug enabled.</span></span> <span data-ttu-id="8804d-115">(Nahraďte skutečné cesty k souborům systému a projektů pro položky v závorkách úhel.)</span><span class="sxs-lookup"><span data-stu-id="8804d-115">(Substitute actual paths to your system and project files for the angle-bracketed items.)</span></span>
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"
   
    <span data-ttu-id="8804d-116">`VSX64RemoteDebuggerPath`je cesta ke složce obsahující msvsmon.exe v nástrojích pro vzdálenou pro sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8804d-116">`VSX64RemoteDebuggerPath` is the path to the folder containing msvsmon.exe in the Remote Tools for Visual Studio.</span></span>
    <span data-ttu-id="8804d-117">`RemoteDebuggerConnectorVersion`je verze sady SDK Azure v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8804d-117">`RemoteDebuggerConnectorVersion` is the Azure SDK version in your cloud service.</span></span> <span data-ttu-id="8804d-118">Je také by měl odpovídat verzi nainstalované s Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8804d-118">It should also match the version installed with Visual Studio.</span></span>
5. <span data-ttu-id="8804d-119">Publikujte ke cloudové službě cíl s použitím souboru balíčku a .cscfg vygenerované v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="8804d-119">Publish to the target cloud service by using the package and .cscfg file generated in the previous step.</span></span>
6. <span data-ttu-id="8804d-120">Importujte certifikátu (soubor .pfx) do počítače, který má Visual Studio s Azure SDK pro .NET nainstalované.</span><span class="sxs-lookup"><span data-stu-id="8804d-120">Import the certificate (.pfx file) to the machine that has Visual Studio with Azure SDK for .NET installed.</span></span> <span data-ttu-id="8804d-121">Zajistěte, aby k importu do `CurrentUser\My` úložiště certifikátů, jinak se připojuje k v ladicím programu sady Visual Studio se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8804d-121">Make sure to import to the `CurrentUser\My` certificate store, otherwise attaching to the debugger in Visual Studio will fail.</span></span>

## <a name="enabling-remote-debugging-for-virtual-machines"></a><span data-ttu-id="8804d-122">Povolení vzdáleného ladění pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8804d-122">Enabling remote debugging for virtual machines</span></span>
1. <span data-ttu-id="8804d-123">Vytvořte virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="8804d-123">Create an Azure virtual machine.</span></span> <span data-ttu-id="8804d-124">V tématu [vytvoření virtuálního počítače se systémem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [vytvářet a spravovat virtuální počítače Azure v sadě Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8804d-124">See [Create a Virtual Machine Running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [Create and Manage Azure Virtual Machines in Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
2. <span data-ttu-id="8804d-125">Na [Azure classic stránky portálu](http://go.microsoft.com/fwlink/p/?LinkID=269851), zobrazení řídicího panelu virtuálního počítače zobrazíte virtuálního počítače **kryptografický OTISK certifikátu protokolu RDP**.</span><span class="sxs-lookup"><span data-stu-id="8804d-125">On the [Azure classic portal page](http://go.microsoft.com/fwlink/p/?LinkID=269851), view the virtual machine's dashboard to see the virtual machine’s **RDP CERTIFICATE THUMBPRINT**.</span></span> <span data-ttu-id="8804d-126">Tato hodnota se používá pro `ServerThumbprint` hodnota v konfiguraci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8804d-126">This value is used for the `ServerThumbprint` value in the extension configuration.</span></span>
3. <span data-ttu-id="8804d-127">Vytvoření certifikátu klienta, jak je uvedeno v [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md) (Uchovávejte .pfx a kryptografický otisk certifikátu protokolu RDP).</span><span class="sxs-lookup"><span data-stu-id="8804d-127">Create a client certificate as outlined in [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md) (keep the .pfx and RDP certificate thumbprint).</span></span>
4. <span data-ttu-id="8804d-128">Nainstalovat Azure Powershell (verze 0.7.4 nebo novější) jak je uvedeno v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8804d-128">Install Azure Powershell (version 0.7.4 or later) as outlined in [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
5. <span data-ttu-id="8804d-129">Spusťte následující skript, který chcete povolit rozšíření RemoteDebug.</span><span class="sxs-lookup"><span data-stu-id="8804d-129">Run the following script to enable the RemoteDebug extension.</span></span> <span data-ttu-id="8804d-130">Nahraďte cesty a osobní data vlastní, například název odběru, název služby a kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="8804d-130">Replace the paths and personal data with your own, such as your subscription name, service name, and thumbprint.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8804d-131">Tento skript je nakonfigurován pro Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="8804d-131">This script is configured for Visual Studio 2015.</span></span> <span data-ttu-id="8804d-132">Pokud používáte Visual Studio 2013 nebo Visual Studio 2017, upravte `$referenceName` a `$extensionName` přiřazení níže, aby `RemoteDebugVS2013` nebo `RemoteDebugVS2017`.</span><span class="sxs-lookup"><span data-stu-id="8804d-132">If you’re using Visual Studio 2013 or Visual Studio 2017, modify the `$referenceName` and `$extensionName` assignments below to `RemoteDebugVS2013` or `RemoteDebugVS2017`.</span></span>

    ```powershell   
    Add-AzureAccount

    Select-AzureSubscription "My Microsoft Subscription"

    $vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"

    $endpoints = @(
                    ,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
                    ,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
                )

    foreach($endpoint in $endpoints)
    {
        Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
    }

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
    $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
    $extensionName = "RemoteDebugVS2015"
    $version = "1.*"
    $publicConfiguration = "<PublicConfig><Connector.Enabled>true</Connector.Enabled><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension -ReferenceName $referenceName -Publisher $publisher -ExtensionName $extensionName -Version $version -PublicConfiguration $publicConfiguration

    foreach($extension in $vm.VM.ResourceExtensionReferences)
    {
        if(($extension.ReferenceName -eq $referenceName) `
        -and ($extension.Publisher -eq $publisher) `
        -and ($extension.Name -eq $extensionName) `
        -and ($extension.Version -eq $version))
        {
            $extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
            break
        }
    }

    $vm | Update-AzureVM
    ```

6. <span data-ttu-id="8804d-133">Importujte certifikátu (.pfx) do počítače, který má Visual Studio s Azure SDK pro .NET nainstalované.</span><span class="sxs-lookup"><span data-stu-id="8804d-133">Import the certificate (.pfx) to the machine that has Visual Studio with Azure SDK for .NET installed.</span></span>

