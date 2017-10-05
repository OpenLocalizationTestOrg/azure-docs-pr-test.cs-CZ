---
title: "Vytvoření clusteru s podporou samostatné s virtuálními počítači Azure s Windows | Microsoft Docs"
description: "Naučte se vytvářet a spravovat cluster služby Azure Service Fabric na virtuálních počítačích Azure s Windows serverem."
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
ms.openlocfilehash: f8a0305a22c00f9bdbdb1bdb06dc299cccee23dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Vytvoření clusteru Service Fabric samostatné tři uzly s virtuálními počítači Azure s Windows serverem
Tento článek popisuje postup vytvoření clusteru v systému Windows Azure virtuální počítače (VM), pomocí Service Fabric samostatný instalační program systému Windows Server. Tento scénář je zvláštní případ [vytvořit a spravovat cluster se systémem Windows Server](service-fabric-cluster-creation-for-windows-server.md) virtuální počítače, kde jsou [virtuální počítače Azure se systémem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), ale nejsou vytváření [Azure cluster Service Fabric cloudové](service-fabric-cluster-creation-via-portal.md). Rozdíl v následujících tento vzor je, že cluster Service Fabric samostatné vytvořené pomocí následujících kroků je zcela spravován, zatímco Azure Service Fabric clustery založené na cloudu jsou spravovaná a upgradovat poskytovatelem prostředků Service Fabric.

## <a name="steps-to-create-the-standalone-cluster"></a>Postup vytvoření clusteru samostatné
1. Přihlaste se k portálu Azure a vytvoření nového systému Windows Server 2012 R2 Datacenter nebo virtuálního počítače Windows serveru 2016 Datacenter ve skupině prostředků. Přečtěte si článek [vytvoření virtuálního počítače s Windows v portálu Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) další podrobnosti.
2. Přidejte několik více virtuálních počítačů do stejné skupiny prostředků. Zajistěte, aby všech virtuálních počítačích stejné uživatelské jméno správce a heslo při vytvoření. Po vytvoření, měli byste vidět všechny tři virtuální počítače ve stejné virtuální síti.
3. Připojit k jednotlivým virtuální počítače a vypnout bránu Windows Firewall pomocí [správce serveru, řídicí panel místní Server](https://technet.microsoft.com/library/jj134147.aspx). Tím se zajistí, že síťový provoz můžete komunikaci mezi počítači. Při připojení k každý počítač, získat IP adresu otevřením příkazového řádku a zadáním `ipconfig`. Případně se zobrazí IP adresu každého počítače, na portálu, výběrem zdroje virtuální sítě pro skupinu prostředků a kontrola rozhraní sítě vytvořené pro každou z těchto počítačů.
4. Připojit k virtuální počítače a otestovat, zda dostupný příkazem ping dva virtuální počítače úspěšně.
5. Připojení k jednomu z virtuálních počítačů a [Stáhnout balíček Service Fabric samostatné pro systém Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) do nové složky v počítači a extrahování balíčku.
6. Otevřete *ClusterConfig.Unsecure.MultiMachine.json* souboru v programu Poznámkový blok a upravit každý uzel s tři IP adresy počítačů. Změňte název clusteru v horní části a uložte soubor.  Částečné příklad manifestu clusteru jsou uvedené dole.
   
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
7. Pokud chcete jako cluster s podporou zabezpečení, rozhodněte, bezpečnostní opatření, které chcete použít a postupujte podle kroků v přidružené odkaz: [X509 certifikátu](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md). Pokud nastavení cluster pomocí zabezpečení systému Windows, musíte nastavit řadič domény ke správě služby Active Directory. Všimněte si, že používání počítačů řadiče domény jako Service Fabric uzel není podporován.
8. Otevřete [okno prostředí PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Přejděte do složky, kde jste extrahovali balíček stažené samostatný instalační program a uložili konfiguračního souboru clusteru. Spusťte následující příkaz prostředí PowerShell pro nasazení clusteru:
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

Skript se vzdáleně konfigurovat cluster Service Fabric a musí hlásit průběh jako prostřednictvím zobrazí souhrn nasazení.

9. Po o několik minut, můžete zkontrolovat, pokud je cluster funkční propojíte pomocí jedné z tohoto počítače IP adresy, například pomocí Service Fabric Explorer `http://10.1.0.5:19080/Explorer/index.html`. 

## <a name="next-steps"></a>Další kroky
* [Vytvoření samostatných clusterů Service Fabric ve Windows Serveru nebo Linuxu](service-fabric-deploy-anywhere.md)
* [Přidávat nebo odebírat uzly do clusteru s podporou samostatné Service Fabric](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Nastavení konfigurace pro samostatný cluster Windows](service-fabric-cluster-manifest.md)
* [Zabezpečení clusteru s podporou samostatné do systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)
* [Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty](service-fabric-windows-cluster-x509-security.md)

