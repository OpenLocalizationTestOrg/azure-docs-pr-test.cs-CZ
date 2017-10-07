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
# <a name="enable-remote-debugging-when-using-continuous-delivery-toopublish-tooazure"></a>Povolení vzdáleného ladění při použití tooAzure toopublish nastavené průběžné doručování
Můžete povolit vzdálené ladění v Azure, pro cloudové služby nebo virtuálního počítače, při použití [nastavené průběžné doručování](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure pomocí následujících kroků.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Povolení vzdáleného ladění pro cloudové služby
1. V agentovi hello sestavení, nastavení hello počáteční prostředí pro Azure jak je uvedeno v [sestavení příkazového řádku pro Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Protože je vyžadován pro balíček hello hello vzdáleného ladění runtime (msvsmon.exe), nainstalujte hello **nástrojů pro vzdálenou pro Visual Studio**.

    * [Nástroje pro vzdálenou pro Visual Studio 2017](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [Nástroje pro vzdálenou pro Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [Nástroje pro vzdálenou pro Visual Studio 2013 Update 5](https://www.microsoft.com/download/details.aspx?id=48156)
    
    Jako alternativu můžete zkopírovat binární soubory pro vzdálené ladění hello ze systému, který má nainstalovanou sadu Visual Studio.

3. Vytvoření certifikátu, jak je uvedeno v [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md). Zachovat hello .pfx a kryptografický otisk certifikátu protokolu RDP a nahrajte hello certifikát toohello cíl cloudové služby.
4. Použijte hello následující možnosti v toobuild příkazového řádku nástroje MSBuild hello a balíčku s vzdáleného ladění povoleno. (Nahraďte soubory systému a projekt tooyour skutečné cesty pro položky v závorkách úhel hello.)
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of hello certificate added toohello cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path tooyour VS solution file>"
   
    `VSX64RemoteDebuggerPath`je hello cesta toohello složku obsahující msvsmon.exe v nástrojích pro vzdálenou hello pro sadu Visual Studio.
    `RemoteDebuggerConnectorVersion`je hello Azure SDK verze v rámci cloudové služby. Je také by měl odpovídat hello verze nainstalované s Visual Studio.
5. Publikujte toohello cílovou cloudovou službu pomocí balíčku a .cscfg souboru hello vygenerovaného v předchozím kroku hello.
6. Importovat hello certifikát (soubor .pfx) toohello počítač, který má Visual Studio s Azure SDK pro .NET nainstalované. Ujistěte se, že toohello tooimport `CurrentUser\My` úložiště certifikátů, jinak připojení ladicí program toohello v sadě Visual Studio se nezdaří.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Povolení vzdáleného ladění pro virtuální počítače
1. Vytvořte virtuální počítač Azure. V tématu [vytvoření virtuálního počítače se systémem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [vytvářet a spravovat virtuální počítače Azure v sadě Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
2. Na hello [Azure stránky portálu classic](http://go.microsoft.com/fwlink/p/?LinkID=269851), zobrazit hello virtuálního počítače řídicí panel toosee hello virtuálního počítače **kryptografický OTISK certifikátu protokolu RDP**. Tato hodnota se používá pro hello `ServerThumbprint` hodnota v konfiguraci rozšíření hello.
3. Vytvoření certifikátu klienta, jak je uvedeno v [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md) (Uchovávejte hello .pfx a kryptografický otisk certifikátu protokolu RDP).
4. Nainstalovat Azure Powershell (verze 0.7.4 nebo novější) jak je uvedeno v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
5. Spusťte následující skript tooenable hello RemoteDebug rozšíření hello. Nahraďte hello cesty a osobní data vlastní, například název odběru, název služby a kryptografický otisk.
   
   > [!NOTE]
   > Tento skript je nakonfigurován pro Visual Studio 2015. Pokud používáte Visual Studio 2013 nebo Visual Studio 2017, upravte hello `$referenceName` a `$extensionName` následující přiřazení příliš`RemoteDebugVS2013` nebo `RemoteDebugVS2017`.

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

6. Import hello certifikátu (.pfx) toohello počítač, který má Visual Studio s Azure SDK pro .NET nainstalované.

