---
title: "vzdálené ladění pomocí nastavené průběžné doručování aaaEnable | Microsoft Docs"
description: "Zjistěte, jak tooenable vzdálené ladění při použití tooAzure toodeploy nastavené průběžné doručování"
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
ms.openlocfilehash: d9d9d1cfe5304c9526586a9164f172746a448e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-debugging-when-using-continuous-delivery-toopublish-tooazure"></a><span data-ttu-id="c30a3-103">Povolení vzdáleného ladění při použití tooAzure toopublish nastavené průběžné doručování</span><span class="sxs-lookup"><span data-stu-id="c30a3-103">Enable remote debugging when using continuous delivery toopublish tooAzure</span></span>
<span data-ttu-id="c30a3-104">Můžete povolit vzdálené ladění v Azure, pro cloudové služby nebo virtuálního počítače, při použití [nastavené průběžné doručování](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="c30a3-104">You can enable remote debugging in Azure, for cloud services or virtual machines, when you use [continuous delivery](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure by following these steps.</span></span>

## <a name="enabling-remote-debugging-for-cloud-services"></a><span data-ttu-id="c30a3-105">Povolení vzdáleného ladění pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="c30a3-105">Enabling remote debugging for cloud services</span></span>
1. <span data-ttu-id="c30a3-106">V agentovi hello sestavení, nastavení hello počáteční prostředí pro Azure jak je uvedeno v [sestavení příkazového řádku pro Azure](http://msdn.microsoft.com/library/hh535755.aspx).</span><span class="sxs-lookup"><span data-stu-id="c30a3-106">On hello build agent, set up hello initial environment for Azure as outlined in [Command-Line Build for Azure](http://msdn.microsoft.com/library/hh535755.aspx).</span></span>
2. <span data-ttu-id="c30a3-107">Protože je vyžadován pro balíček hello hello vzdáleného ladění runtime (msvsmon.exe), nainstalujte hello **nástrojů pro vzdálenou pro Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="c30a3-107">Because hello remote debug runtime (msvsmon.exe) is required for hello package, install hello **Remote Tools for Visual Studio**.</span></span>

    * [<span data-ttu-id="c30a3-108">Nástroje pro vzdálenou pro Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c30a3-108">Remote Tools for Visual Studio 2017</span></span>](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [<span data-ttu-id="c30a3-109">Nástroje pro vzdálenou pro Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="c30a3-109">Remote Tools for Visual Studio 2015</span></span>](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [<span data-ttu-id="c30a3-110">Nástroje pro vzdálenou pro Visual Studio 2013 Update 5</span><span class="sxs-lookup"><span data-stu-id="c30a3-110">Remote Tools for Visual Studio 2013 Update 5</span></span>](https://www.microsoft.com/download/details.aspx?id=48156)
    
    <span data-ttu-id="c30a3-111">Jako alternativu můžete zkopírovat binární soubory pro vzdálené ladění hello ze systému, který má nainstalovanou sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c30a3-111">As an alternative, you can copy hello remote debug binaries from a system that has Visual Studio installed.</span></span>

3. <span data-ttu-id="c30a3-112">Vytvoření certifikátu, jak je uvedeno v [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="c30a3-112">Create a certificate as outlined in [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="c30a3-113">Zachovat hello .pfx a kryptografický otisk certifikátu protokolu RDP a nahrajte hello certifikát toohello cíl cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c30a3-113">Keep hello .pfx and RDP certificate thumbprint and upload hello certificate toohello target cloud service.</span></span>
4. <span data-ttu-id="c30a3-114">Použijte hello následující možnosti v toobuild příkazového řádku nástroje MSBuild hello a balíčku s vzdáleného ladění povoleno.</span><span class="sxs-lookup"><span data-stu-id="c30a3-114">Use hello following options in hello MSBuild command line toobuild and package with remote debug enabled.</span></span> <span data-ttu-id="c30a3-115">(Nahraďte soubory systému a projekt tooyour skutečné cesty pro položky v závorkách úhel hello.)</span><span class="sxs-lookup"><span data-stu-id="c30a3-115">(Substitute actual paths tooyour system and project files for hello angle-bracketed items.)</span></span>
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of hello certificate added toohello cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path tooyour VS solution file>"
   
    <span data-ttu-id="c30a3-116">`VSX64RemoteDebuggerPath`je hello cesta toohello složku obsahující msvsmon.exe v nástrojích pro vzdálenou hello pro sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c30a3-116">`VSX64RemoteDebuggerPath` is hello path toohello folder containing msvsmon.exe in hello Remote Tools for Visual Studio.</span></span>
    <span data-ttu-id="c30a3-117">`RemoteDebuggerConnectorVersion`je hello Azure SDK verze v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c30a3-117">`RemoteDebuggerConnectorVersion` is hello Azure SDK version in your cloud service.</span></span> <span data-ttu-id="c30a3-118">Je také by měl odpovídat hello verze nainstalované s Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c30a3-118">It should also match hello version installed with Visual Studio.</span></span>
5. <span data-ttu-id="c30a3-119">Publikujte toohello cílovou cloudovou službu pomocí balíčku a .cscfg souboru hello vygenerovaného v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="c30a3-119">Publish toohello target cloud service by using hello package and .cscfg file generated in hello previous step.</span></span>
6. <span data-ttu-id="c30a3-120">Importovat hello certifikát (soubor .pfx) toohello počítač, který má Visual Studio s Azure SDK pro .NET nainstalované.</span><span class="sxs-lookup"><span data-stu-id="c30a3-120">Import hello certificate (.pfx file) toohello machine that has Visual Studio with Azure SDK for .NET installed.</span></span> <span data-ttu-id="c30a3-121">Ujistěte se, že toohello tooimport `CurrentUser\My` úložiště certifikátů, jinak připojení ladicí program toohello v sadě Visual Studio se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="c30a3-121">Make sure tooimport toohello `CurrentUser\My` certificate store, otherwise attaching toohello debugger in Visual Studio will fail.</span></span>

## <a name="enabling-remote-debugging-for-virtual-machines"></a><span data-ttu-id="c30a3-122">Povolení vzdáleného ladění pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c30a3-122">Enabling remote debugging for virtual machines</span></span>
1. <span data-ttu-id="c30a3-123">Vytvořte virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="c30a3-123">Create an Azure virtual machine.</span></span> <span data-ttu-id="c30a3-124">V tématu [vytvoření virtuálního počítače se systémem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [vytvářet a spravovat virtuální počítače Azure v sadě Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c30a3-124">See [Create a Virtual Machine Running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [Create and Manage Azure Virtual Machines in Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
2. <span data-ttu-id="c30a3-125">Na hello [Azure stránky portálu classic](http://go.microsoft.com/fwlink/p/?LinkID=269851), zobrazit hello virtuálního počítače řídicí panel toosee hello virtuálního počítače **kryptografický OTISK certifikátu protokolu RDP**.</span><span class="sxs-lookup"><span data-stu-id="c30a3-125">On hello [Azure classic portal page](http://go.microsoft.com/fwlink/p/?LinkID=269851), view hello virtual machine's dashboard toosee hello virtual machine’s **RDP CERTIFICATE THUMBPRINT**.</span></span> <span data-ttu-id="c30a3-126">Tato hodnota se používá pro hello `ServerThumbprint` hodnota v konfiguraci rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="c30a3-126">This value is used for hello `ServerThumbprint` value in hello extension configuration.</span></span>
3. <span data-ttu-id="c30a3-127">Vytvoření certifikátu klienta, jak je uvedeno v [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md) (Uchovávejte hello .pfx a kryptografický otisk certifikátu protokolu RDP).</span><span class="sxs-lookup"><span data-stu-id="c30a3-127">Create a client certificate as outlined in [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md) (keep hello .pfx and RDP certificate thumbprint).</span></span>
4. <span data-ttu-id="c30a3-128">Nainstalovat Azure Powershell (verze 0.7.4 nebo novější) jak je uvedeno v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c30a3-128">Install Azure Powershell (version 0.7.4 or later) as outlined in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
5. <span data-ttu-id="c30a3-129">Spusťte následující skript tooenable hello RemoteDebug rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="c30a3-129">Run hello following script tooenable hello RemoteDebug extension.</span></span> <span data-ttu-id="c30a3-130">Nahraďte hello cesty a osobní data vlastní, například název odběru, název služby a kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="c30a3-130">Replace hello paths and personal data with your own, such as your subscription name, service name, and thumbprint.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c30a3-131">Tento skript je nakonfigurován pro Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="c30a3-131">This script is configured for Visual Studio 2015.</span></span> <span data-ttu-id="c30a3-132">Pokud používáte Visual Studio 2013 nebo Visual Studio 2017, upravte hello `$referenceName` a `$extensionName` následující přiřazení příliš`RemoteDebugVS2013` nebo `RemoteDebugVS2017`.</span><span class="sxs-lookup"><span data-stu-id="c30a3-132">If you’re using Visual Studio 2013 or Visual Studio 2017, modify hello `$referenceName` and `$extensionName` assignments below too`RemoteDebugVS2013` or `RemoteDebugVS2017`.</span></span>

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

6. <span data-ttu-id="c30a3-133">Import hello certifikátu (.pfx) toohello počítač, který má Visual Studio s Azure SDK pro .NET nainstalované.</span><span class="sxs-lookup"><span data-stu-id="c30a3-133">Import hello certificate (.pfx) toohello machine that has Visual Studio with Azure SDK for .NET installed.</span></span>

