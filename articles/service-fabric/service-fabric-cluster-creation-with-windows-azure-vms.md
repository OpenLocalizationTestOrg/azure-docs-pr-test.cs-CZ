---
title: "aaaCreate samostatný cluster s virtuálními počítači Azure s Windows | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat cluster služby Azure Service Fabric na virtuálních počítačích Azure s Windows serverem."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 7eeb40d2-fb22-4a77-80ca-f1b46b22edbc
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: ryanwi;chackdan
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: 8900204fe69887a7a0ca54b06e0d32534421bcfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Vytvoření clusteru Service Fabric samostatné tři uzly s virtuálními počítači Azure s Windows serverem
Tento článek popisuje, jak pomocí toocreate a clusteru v systému Windows Azure virtuální počítače (VM), hello samostatný instalační program Service Fabric systému pro Windows Server. scénář Hello se zvláštním případem [vytvořit a spravovat cluster se systémem Windows Server](service-fabric-cluster-creation-for-windows-server.md) kde hello virtuální počítače jsou [virtuální počítače Azure se systémem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), ale nejsou vytváření [Azure cloudový cluster Service Fabric](service-fabric-cluster-creation-via-portal.md). Hello rozdíl v následujících tento vzor je tento cluster Service Fabric hello samostatné vytvořené hello následující kroky zcela spravovaná, zatímco hello Azure Service Fabric cloudu clustery jsou spravovaná a upgradovat přes hello Service Fabric Zprostředkovatel prostředků.

## <a name="steps-toocreate-hello-standalone-cluster"></a>Kroky toocreate hello samostatné clusteru
1. Přihlaste toohello portál Azure a vytvoření nového systému Windows Server 2012 R2 Datacenter nebo virtuálního počítače Windows serveru 2016 Datacenter ve skupině prostředků. Přečíst článek hello [vytvoření virtuálního počítače s Windows v portálu Azure hello](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) další podrobnosti.
2. Přidat několik více virtuálních počítačů toohello stejnou skupinu prostředků. Ujistěte se, že každý hello virtuálních počítačů má hello stejné uživatelské jméno správce a heslo při vytvoření. Po vytvoření, měli byste vidět všechny tři virtuální počítače v hello stejné virtuální síti.
3. Připojit tooeach hello virtuální počítače a vypnout nastavení hello brány Windows Firewall pomocí hello [správce serveru, řídicí panel místní Server](https://technet.microsoft.com/library/jj134147.aspx). Tím se zajistí, že může komunikovat hello síťový provoz mezi počítači hello. Při počítače připojené tooeach získat IP adresu hello otevřením příkazového řádku a zadáním `ipconfig`. Případně můžete zobrazit hello IP adresu každého počítače, na portálu hello výběrem zdroje hello virtuální sítě pro skupinu prostředků hello a kontrola hello síťová rozhraní pro každé z těchto počítačů.
4. Připojte tooone hello virtuální počítače a test, který může odeslat příkaz ping hello další dva virtuální počítače úspěšně.
5. Připojit tooone hello virtuální počítače a [Stáhnout balíček Service Fabric hello samostatné pro systém Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) do nové složky na hello počítač a rozbalte balíček hello.
6. Otevřete hello *ClusterConfig.Unsecure.MultiMachine.json* souboru v programu Poznámkový blok a upravit každý uzel s IP adresami hello tři hello počítačů. Změňte název clusteru hello hello horní a uložte soubor hello.  Částečné příklad hello manifestu clusteru jsou uvedené dole.
   
    ```
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "01-2017",
        "nodes": [
        {
            "nodeName": "standalonenode0",
            "iPAddress": "10.1.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "standalonenode1",
            "iPAddress": "10.1.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "standalonenode2",
            "iPAddress": "10.1.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
        ],
    ```
7. Pokud máte v úmyslu tento toobe zabezpečení clusteru, rozhodněte, hello bezpečnostní opatření chcete vytvořit toouse a postupujte podle kroků hello v hello přidružené odkaz: [X509 certifikátu](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md). Pokud nastavujete hello clusteru pomocí zabezpečení systému Windows, budete potřebovat tooset nahoru toomanage řadič domény služby Active Directory. Všimněte si, že používání počítačů řadiče domény jako Service Fabric uzel není podporován.
8. Otevřete [okno prostředí PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Přejděte toohello složku, kde jste extrahovali hello samostatné stažený instalační balíček a uložili hello clusteru konfigurační soubor. Spusťte hello následující clusteru hello toodeploy příkaz prostředí PowerShell:
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

skript Hello se vzdáleně konfigurovat cluster Service Fabric hello a musí hlásit průběh jako prostřednictvím zobrazí souhrn nasazení.

9. Po o minuty, můžete zkontrolovat, zda je hello clusteru provozní připojením toohello Service Fabric Explorer pomocí jedné z počítače hello IP adres, například pomocí `http://10.1.0.5:19080/Explorer/index.html`. 

## <a name="next-steps"></a>Další kroky
* [Vytvoření samostatných clusterů Service Fabric ve Windows Serveru nebo Linuxu](service-fabric-deploy-anywhere.md)
* [Přidat nebo odebrat cluster Service Fabric samostatné tooa uzly](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Nastavení konfigurace pro samostatný cluster Windows](service-fabric-cluster-manifest.md)
* [Zabezpečení clusteru s podporou samostatné do systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)
* [Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty](service-fabric-windows-cluster-x509-security.md)

