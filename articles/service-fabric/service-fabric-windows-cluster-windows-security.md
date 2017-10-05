---
title: "Zabezpečení clusteru se systémem Windows pomocí zabezpečení systému Windows | Microsoft Docs"
description: "Naučte se konfigurovat – uzly a klientský uzel zabezpečení v samostatné clusteru se systémem Windows pomocí zabezpečení systému Windows."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: e093a631b0cf81195981a8e3d345504ebce02723
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a><span data-ttu-id="63c20-103">Zabezpečení samostatné clusteru v systému Windows pomocí zabezpečení systému Windows</span><span class="sxs-lookup"><span data-stu-id="63c20-103">Secure a standalone cluster on Windows by using Windows security</span></span>
<span data-ttu-id="63c20-104">K zabránění neoprávněného přístupu k cluster Service Fabric, je nutné zabezpečit clusteru.</span><span class="sxs-lookup"><span data-stu-id="63c20-104">To prevent unauthorized access to a Service Fabric cluster, you must secure the cluster.</span></span> <span data-ttu-id="63c20-105">Zabezpečení je zvlášť důležité při spuštění úlohy v produkčním prostředí clusteru.</span><span class="sxs-lookup"><span data-stu-id="63c20-105">Security is especially important when the cluster runs production workloads.</span></span> <span data-ttu-id="63c20-106">Tento článek popisuje, jak nakonfigurovat zabezpečení – uzly a uzlu klientů pomocí zabezpečení systému Windows v *souboru ClusterConfig.JSON* souboru.</span><span class="sxs-lookup"><span data-stu-id="63c20-106">This article describes how to configure node-to-node and client-to-node security by using Windows security in the *ClusterConfig.JSON* file.</span></span>  <span data-ttu-id="63c20-107">Proces odpovídá krok konfigurace zabezpečení [vytvořit samostatnou clusteru se systémem Windows](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="63c20-107">The process corresponds to the configure security step of [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span> <span data-ttu-id="63c20-108">Další informace o tom, jak Service Fabric používá zabezpečení systému Windows najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="63c20-108">For more information about how Service Fabric uses Windows security, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

> [!NOTE]
> <span data-ttu-id="63c20-109">Výběr mezi uzly zabezpečení byste měli zvážit pečlivě vzhledem k tomu, že neexistuje žádná upgrade clusteru z výběru jeden zabezpečení do jiného.</span><span class="sxs-lookup"><span data-stu-id="63c20-109">You should consider the selection of node-to-node security carefully because there is no cluster upgrade from one security choice to another.</span></span> <span data-ttu-id="63c20-110">Chcete-li změnit výběr zabezpečení, budete muset znovu sestavit úplnou clusteru.</span><span class="sxs-lookup"><span data-stu-id="63c20-110">To change the security selection, you have to rebuild the full cluster.</span></span>
>
>

## <a name="configure-windows-security-using-gmsa"></a><span data-ttu-id="63c20-111">Konfigurace zabezpečení systému Windows pomocí gMSA</span><span class="sxs-lookup"><span data-stu-id="63c20-111">Configure Windows security using gMSA</span></span>  
<span data-ttu-id="63c20-112">Ukázka *ClusterConfig.gMSA.Windows.MultiMachine.JSON* konfigurační soubor stažené společně se sadou [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) samostatný cluster balíček obsahuje šablony pro konfiguraci zabezpečení systému Windows pomocí [skupinový účet spravované služby (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span><span class="sxs-lookup"><span data-stu-id="63c20-112">The sample *ClusterConfig.gMSA.Windows.MultiMachine.JSON* configuration file downloaded with the [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security using [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span></span>  

```  
"security": {  
            "WindowsIdentities": {  
                "ClustergMSAIdentity": "accountname@fqdn"  
                "ClusterSPN": "fqdn"  
                "ClientIdentities": [  
                    {  
                        "Identity": "domain\\username",  
                        "IsAdmin": true  
                    }  
                ]  
            }  
        }  
```  
  
| <span data-ttu-id="63c20-113">**Nastavení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="63c20-113">**Configuration Setting**</span></span> | <span data-ttu-id="63c20-114">**Popis**</span><span class="sxs-lookup"><span data-stu-id="63c20-114">**Description**</span></span> |  
| --- | --- |  
| <span data-ttu-id="63c20-115">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="63c20-115">WindowsIdentities</span></span> |<span data-ttu-id="63c20-116">Obsahuje identity clusteru a klientů.</span><span class="sxs-lookup"><span data-stu-id="63c20-116">Contains the cluster and client identities.</span></span> |  
| <span data-ttu-id="63c20-117">ClustergMSAIdentity</span><span class="sxs-lookup"><span data-stu-id="63c20-117">ClustergMSAIdentity</span></span> |<span data-ttu-id="63c20-118">Nakonfiguruje zabezpečení mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="63c20-118">Configures node-to-node security.</span></span> <span data-ttu-id="63c20-119">Skupinu účet spravované služby.</span><span class="sxs-lookup"><span data-stu-id="63c20-119">A group managed service account.</span></span> |  
| <span data-ttu-id="63c20-120">ClusterSPN</span><span class="sxs-lookup"><span data-stu-id="63c20-120">ClusterSPN</span></span> |<span data-ttu-id="63c20-121">Plně kvalifikované domény hlavního názvu služby pro účet gMSA</span><span class="sxs-lookup"><span data-stu-id="63c20-121">Fully qualified domain SPN for gMSA account</span></span>|  
| <span data-ttu-id="63c20-122">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="63c20-122">ClientIdentities</span></span> |<span data-ttu-id="63c20-123">Nakonfiguruje klienta uzel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="63c20-123">Configures client-to-node security.</span></span> <span data-ttu-id="63c20-124">Pole klienta uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="63c20-124">An array of client user accounts.</span></span> |  
| <span data-ttu-id="63c20-125">Identita</span><span class="sxs-lookup"><span data-stu-id="63c20-125">Identity</span></span> |<span data-ttu-id="63c20-126">Je identita klienta, uživatel domény.</span><span class="sxs-lookup"><span data-stu-id="63c20-126">The client identity, a domain user.</span></span> |  
| <span data-ttu-id="63c20-127">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="63c20-127">IsAdmin</span></span> |<span data-ttu-id="63c20-128">Hodnota TRUE určí, že uživatele domény má přístup správce klienta, hodnotu false pro klientský přístup uživatele.</span><span class="sxs-lookup"><span data-stu-id="63c20-128">True specifies that the domain user has administrator client access, false for user client access.</span></span> |  
  
<span data-ttu-id="63c20-129">[Uzel zabezpečení uzlu](service-fabric-cluster-security.md#node-to-node-security) je nakonfigurované nastavením **ClustergMSAIdentity** když service fabric potřebuje pro spuštění pod gMSA.</span><span class="sxs-lookup"><span data-stu-id="63c20-129">[Node to node security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting **ClustergMSAIdentity** when service fabric needs to run under gMSA.</span></span> <span data-ttu-id="63c20-130">Chcete-li vytvořit vztahy důvěryhodnosti mezi uzly, se musí být provedeny vědět navzájem.</span><span class="sxs-lookup"><span data-stu-id="63c20-130">In order to build trust relationships between nodes, they must be made aware of each other.</span></span> <span data-ttu-id="63c20-131">Můžete to provést dvěma způsoby: Skupinový účet spravované služby zahrnující všechny uzly v clusteru nebo zadejte skupinu domény počítače, který obsahuje všechny uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="63c20-131">This can be accomplished in two different ways: Specify the Group Managed Service Account that includes all nodes in the cluster or Specify the domain machine group that includes all nodes in the cluster.</span></span> <span data-ttu-id="63c20-132">Důrazně doporučujeme pomocí [skupinový účet spravované služby (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) přístup, zejména pro větší clustery (víc než 10 uzlů) nebo pro clustery, které by mohly zvětšení nebo zmenšení.</span><span class="sxs-lookup"><span data-stu-id="63c20-132">We strongly recommend using the [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) approach, particularly for larger clusters (more than 10 nodes) or for clusters that are likely to grow or shrink.</span></span>  
<span data-ttu-id="63c20-133">Tento přístup nevyžaduje vytváření skupiny domény, na který byla udělena Správce clusterů přístupová práva k přidání a odebrání členů.</span><span class="sxs-lookup"><span data-stu-id="63c20-133">This approach does not require the creation of a domain group for which cluster administrators have been granted access rights to add and remove members.</span></span> <span data-ttu-id="63c20-134">Tyto účty jsou také užitečná pro automatickou správu hesel.</span><span class="sxs-lookup"><span data-stu-id="63c20-134">These accounts are also useful for automatic password management.</span></span> <span data-ttu-id="63c20-135">Další informace najdete v tématu [Začínáme s skupinové účty spravované služby](http://technet.microsoft.com/library/jj128431.aspx).</span><span class="sxs-lookup"><span data-stu-id="63c20-135">For more information, see [Getting Started with Group Managed Service Accounts](http://technet.microsoft.com/library/jj128431.aspx).</span></span>  
 
<span data-ttu-id="63c20-136">[Klient uzel zabezpečení](service-fabric-cluster-security.md#client-to-node-security) je konfigurován pomocí **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="63c20-136">[Client to node security](service-fabric-cluster-security.md#client-to-node-security) is configured using **ClientIdentities**.</span></span> <span data-ttu-id="63c20-137">Chcete-li vytvořit vztah důvěryhodnosti mezi klientem a cluster, je nutné nakonfigurovat clusteru potřebujete vědět, který klient identity, které můžete důvěřovat.</span><span class="sxs-lookup"><span data-stu-id="63c20-137">In order to establish trust between a client and the cluster, you must configure the cluster to know which client identities that it can trust.</span></span> <span data-ttu-id="63c20-138">To můžete provést dvěma způsoby: Zadejte skupinu uživatele domény, které můžete připojit nebo zadat uzlu uživatele domény, které se můžou připojit.</span><span class="sxs-lookup"><span data-stu-id="63c20-138">This can be done in two different ways: Specify the domain group users that can connect or specify the domain node users that can connect.</span></span> <span data-ttu-id="63c20-139">Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, kteří jsou připojené ke clusteru Service Fabric: správce a uživatele.</span><span class="sxs-lookup"><span data-stu-id="63c20-139">Service Fabric supports two different access control types for clients that are connected to a Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="63c20-140">Řízení přístupu poskytuje možnost pro správce clusteru k omezení přístupu k určitým typům operace clusteru pro různé skupiny uživatelů, lepší zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="63c20-140">Access control provides the ability for the cluster administrator to limit access to certain types of cluster operations for different groups of users, making the cluster more secure.</span></span>  <span data-ttu-id="63c20-141">Správci mají plný přístup k funkcím správy (včetně možnosti pro čtení i zápis).</span><span class="sxs-lookup"><span data-stu-id="63c20-141">Administrators have full access to management capabilities (including read/write capabilities).</span></span> <span data-ttu-id="63c20-142">Uživatelé, ve výchozím nastavení, mají pouze pro čtení přístup k možnosti správy (například možnosti dotazu) a možnost řešení aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="63c20-142">Users, by default, have only read access to management capabilities (for example, query capabilities), and the ability to resolve applications and services.</span></span> <span data-ttu-id="63c20-143">Další informace o řízení přístupu najdete v tématu [řízení přístupu podle rolí pro Service Fabric klienty](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="63c20-143">For more information on access controls, see [Role based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>  
 
<span data-ttu-id="63c20-144">Následující příklad **zabezpečení** části nakonfiguruje zabezpečení systému Windows pomocí gMSA a určuje, že počítače v *ServiceFabric.clusterA.contoso.com* gMSA jsou součástí clusteru a že *CONTOSO\usera* má klient přístup správce:</span><span class="sxs-lookup"><span data-stu-id="63c20-144">The following example **security** section configures Windows security using gMSA and specifies that the machines in *ServiceFabric.clusterA.contoso.com* gMSA are part of the cluster and that *CONTOSO\usera* has admin client access:</span></span>  
  
```  
"security": {  
    "WindowsIdentities": {  
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",  
        "ClusterSPN" : "clusterA.contoso.com",  
        "ClientIdentities": [{  
            "Identity": "CONTOSO\\usera",  
            "IsAdmin": true  
        }]  
    }  
}  
```  
  
## <a name="configure-windows-security-using-a-machine-group"></a><span data-ttu-id="63c20-145">Konfigurace zabezpečení systému Windows ve skupině počítač</span><span class="sxs-lookup"><span data-stu-id="63c20-145">Configure Windows security using a machine group</span></span>  
<span data-ttu-id="63c20-146">Ukázka *ClusterConfig.Windows.MultiMachine.JSON* konfigurační soubor stažené společně se sadou [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) samostatný cluster balíček obsahuje šablony pro konfiguraci zabezpečení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="63c20-146">The sample *ClusterConfig.Windows.MultiMachine.JSON* configuration file downloaded with the [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security.</span></span>  <span data-ttu-id="63c20-147">Zabezpečení systému Windows je nakonfigurovaný v **vlastnosti** části:</span><span class="sxs-lookup"><span data-stu-id="63c20-147">Windows security is configured in the **Properties** section:</span></span> 

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
                "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

| <span data-ttu-id="63c20-148">**Nastavení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="63c20-148">**Configuration setting**</span></span> | <span data-ttu-id="63c20-149">**Popis**</span><span class="sxs-lookup"><span data-stu-id="63c20-149">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="63c20-150">ClusterCredentialType</span><span class="sxs-lookup"><span data-stu-id="63c20-150">ClusterCredentialType</span></span> |<span data-ttu-id="63c20-151">**ClusterCredentialType** je nastaven na *Windows* Pokud ClusterIdentity Určuje název skupiny počítače Active Directory.</span><span class="sxs-lookup"><span data-stu-id="63c20-151">**ClusterCredentialType** is set to *Windows* if ClusterIdentity specifies an Active Directory Machine Group Name.</span></span> |  
| <span data-ttu-id="63c20-152">ServerCredentialType</span><span class="sxs-lookup"><span data-stu-id="63c20-152">ServerCredentialType</span></span> |<span data-ttu-id="63c20-153">Nastavte na *Windows* povolení zabezpečení systému Windows pro klienty.</span><span class="sxs-lookup"><span data-stu-id="63c20-153">Set to *Windows* to enable Windows security for clients.</span></span><br /><br /><span data-ttu-id="63c20-154">To znamená, že klienti clusteru a samotného clusteru běží v rámci domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="63c20-154">This indicates that the clients of the cluster and the cluster itself are running within an Active Directory domain.</span></span> |  
| <span data-ttu-id="63c20-155">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="63c20-155">WindowsIdentities</span></span> |<span data-ttu-id="63c20-156">Obsahuje identity clusteru a klientů.</span><span class="sxs-lookup"><span data-stu-id="63c20-156">Contains the cluster and client identities.</span></span> |  
| <span data-ttu-id="63c20-157">ClusterIdentity</span><span class="sxs-lookup"><span data-stu-id="63c20-157">ClusterIdentity</span></span> |<span data-ttu-id="63c20-158">Název skupiny počítače, domain\machinegroup, slouží ke konfiguraci zabezpečení mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="63c20-158">Use a machine group name, domain\machinegroup, to configure node-to-node security.</span></span> |  
| <span data-ttu-id="63c20-159">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="63c20-159">ClientIdentities</span></span> |<span data-ttu-id="63c20-160">Nakonfiguruje klienta uzel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="63c20-160">Configures client-to-node security.</span></span> <span data-ttu-id="63c20-161">Pole klienta uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="63c20-161">An array of client user accounts.</span></span> |  
| <span data-ttu-id="63c20-162">Identita</span><span class="sxs-lookup"><span data-stu-id="63c20-162">Identity</span></span> |<span data-ttu-id="63c20-163">Přidejte uživatele domény, doména\uživatelské jméno pro identitu klienta.</span><span class="sxs-lookup"><span data-stu-id="63c20-163">Add the domain user, domain\username, for the client identity.</span></span> |  
| <span data-ttu-id="63c20-164">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="63c20-164">IsAdmin</span></span> |<span data-ttu-id="63c20-165">Nastavte na hodnotu true, chcete-li určit, zda má uživatel domény přístup správce klienta, nebo hodnotu NEPRAVDA pro klientský přístup uživatele.</span><span class="sxs-lookup"><span data-stu-id="63c20-165">Set to true to specify that the domain user has administrator client access or false for user client access.</span></span> |  

<span data-ttu-id="63c20-166">[Uzel zabezpečení uzlu](service-fabric-cluster-security.md#node-to-node-security) se konfiguruje pomocí nastavení **ClusterIdentity** Pokud chcete použít skupinu počítače v doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="63c20-166">[Node to node security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting using **ClusterIdentity** if you want to use a machine group within an Active Directory Domain.</span></span> <span data-ttu-id="63c20-167">Další informace najdete v tématu [vytvořit skupinu počítače ve službě Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span><span class="sxs-lookup"><span data-stu-id="63c20-167">For more information, see [Create a Machine Group in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span></span>

<span data-ttu-id="63c20-168">[Uzel Klient zabezpečení](service-fabric-cluster-security.md#client-to-node-security) se konfiguruje pomocí **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="63c20-168">[Client-to-node security](service-fabric-cluster-security.md#client-to-node-security) is configured by using **ClientIdentities**.</span></span> <span data-ttu-id="63c20-169">K vybudování důvěry mezi klientem a cluster, je nutné nakonfigurovat clusteru potřebujete vědět, klient identity, které můžete důvěřovat clusteru.</span><span class="sxs-lookup"><span data-stu-id="63c20-169">To establish trust between a client and the cluster, you must configure the cluster to know the client identities that the cluster can trust.</span></span> <span data-ttu-id="63c20-170">Můžete vytvořit vztah důvěryhodnosti v dvěma různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="63c20-170">You can establish trust in two different ways:</span></span>

- <span data-ttu-id="63c20-171">Zadejte skupinu uživatele domény, které se můžou připojit.</span><span class="sxs-lookup"><span data-stu-id="63c20-171">Specify the domain group users that can connect.</span></span>
- <span data-ttu-id="63c20-172">Zadejte uživatele uzlu domény, které se můžou připojit.</span><span class="sxs-lookup"><span data-stu-id="63c20-172">Specify the domain node users that can connect.</span></span>

<span data-ttu-id="63c20-173">Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, kteří jsou připojené ke clusteru Service Fabric: správce a uživatele.</span><span class="sxs-lookup"><span data-stu-id="63c20-173">Service Fabric supports two different access control types for clients that are connected to a Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="63c20-174">Řízení přístupu umožňuje omezit přístup pro určité typy operací clusteru pro různé skupiny uživatelů, takže je bezpečnější clusteru pomocí Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="63c20-174">Access control enables the cluster administrator to limit access to certain types of cluster operations for different groups of users, which makes the cluster more secure.</span></span>  <span data-ttu-id="63c20-175">Správci mají plný přístup k funkcím správy (včetně možnosti pro čtení i zápis).</span><span class="sxs-lookup"><span data-stu-id="63c20-175">Administrators have full access to management capabilities (including read/write capabilities).</span></span> <span data-ttu-id="63c20-176">Uživatelé, ve výchozím nastavení, mají pouze pro čtení přístup k možnosti správy (například možnosti dotazu) a možnost řešení aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="63c20-176">Users, by default, have only read access to management capabilities (for example, query capabilities), and the ability to resolve applications and services.</span></span>  

<span data-ttu-id="63c20-177">Následující příklad **zabezpečení** části nakonfiguruje zabezpečení systému Windows, určuje, že počítače v *ServiceFabric/clusterA.contoso.com* jsou součástí clusteru a určuje, že *CONTOSO\usera* má klient přístup správce:</span><span class="sxs-lookup"><span data-stu-id="63c20-177">The following example **security** section configures Windows security, specifies that the machines in *ServiceFabric/clusterA.contoso.com* are part of the cluster, and specifies that *CONTOSO\usera* has admin client access:</span></span>

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> <span data-ttu-id="63c20-178">Service Fabric nesmí být nasazen na řadiči domény.</span><span class="sxs-lookup"><span data-stu-id="63c20-178">Service Fabric should not be deployed on a domain controller.</span></span> <span data-ttu-id="63c20-179">Ujistěte se, že souboru ClusterConfig.json při použití skupiny počítačů obsahující IP adresu řadiče domény nebo skupinový účet spravované služby (gMSA).</span><span class="sxs-lookup"><span data-stu-id="63c20-179">Make sure that ClusterConfig.json does not include the IP address of the domain controller when using a machine group or group Managed Service Account (gMSA).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="63c20-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63c20-180">Next steps</span></span>
<span data-ttu-id="63c20-181">Po dokončení konfigurace zabezpečení systému Windows v *souboru ClusterConfig.JSON* souboru, pokračovat v procesu vytváření clusteru [vytvořit samostatnou clusteru se systémem Windows](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="63c20-181">After configuring Windows security in the *ClusterConfig.JSON* file, resume the cluster creation process in [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span>

<span data-ttu-id="63c20-182">Další informace o tom, jak-uzly zabezpečení, klientský uzel zabezpečení a řízení přístupu na základě rolí, najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="63c20-182">For more information about how node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

<span data-ttu-id="63c20-183">V tématu [připojení ke clusteru zabezpečené](service-fabric-connect-to-secure-cluster.md) příklady připojení pomocí prostředí PowerShell nebo FabricClient.</span><span class="sxs-lookup"><span data-stu-id="63c20-183">See [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) for examples of connecting by using PowerShell or FabricClient.</span></span>
