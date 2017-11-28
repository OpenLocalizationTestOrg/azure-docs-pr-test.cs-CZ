---
title: "aaaSecure a cluster se systémem Windows pomocí zabezpečení systému Windows | Microsoft Docs"
description: "Zjistěte, jak tooconfigure-uzly a klientský uzel zabezpečení v samostatné cluster se systémem Windows pomocí zabezpečení systému Windows."
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
ms.openlocfilehash: 44f3011eb630357f342052a48d6c852b17dccec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a><span data-ttu-id="f777d-103">Zabezpečení samostatné clusteru v systému Windows pomocí zabezpečení systému Windows</span><span class="sxs-lookup"><span data-stu-id="f777d-103">Secure a standalone cluster on Windows by using Windows security</span></span>
<span data-ttu-id="f777d-104">tooprevent Neautorizováno cluster Service Fabric tooa přístup, je nutné zabezpečit hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f777d-104">tooprevent unauthorized access tooa Service Fabric cluster, you must secure hello cluster.</span></span> <span data-ttu-id="f777d-105">Zabezpečení je zvlášť důležité při hello cluster spouští úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f777d-105">Security is especially important when hello cluster runs production workloads.</span></span> <span data-ttu-id="f777d-106">Tento článek popisuje, jak tooconfigure-uzly a klientský uzel zabezpečení pomocí zabezpečení systému Windows v hello *souboru ClusterConfig.JSON* souboru.</span><span class="sxs-lookup"><span data-stu-id="f777d-106">This article describes how tooconfigure node-to-node and client-to-node security by using Windows security in hello *ClusterConfig.JSON* file.</span></span>  <span data-ttu-id="f777d-107">Hello procesu odpovídá toohello konfigurace zabezpečení krok [vytvořit samostatnou clusteru se systémem Windows](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="f777d-107">hello process corresponds toohello configure security step of [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span> <span data-ttu-id="f777d-108">Další informace o tom, jak Service Fabric používá zabezpečení systému Windows najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="f777d-108">For more information about how Service Fabric uses Windows security, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f777d-109">Výběr hello – uzly zabezpečení byste měli zvážit pečlivě protože neexistuje žádný upgrade clusteru z jednoho tooanother volbu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f777d-109">You should consider hello selection of node-to-node security carefully because there is no cluster upgrade from one security choice tooanother.</span></span> <span data-ttu-id="f777d-110">Výběr zabezpečení hello toochange, máte toorebuild hello úplné clusteru.</span><span class="sxs-lookup"><span data-stu-id="f777d-110">toochange hello security selection, you have toorebuild hello full cluster.</span></span>
>
>

## <a name="configure-windows-security-using-gmsa"></a><span data-ttu-id="f777d-111">Konfigurace zabezpečení systému Windows pomocí gMSA</span><span class="sxs-lookup"><span data-stu-id="f777d-111">Configure Windows security using gMSA</span></span>  
<span data-ttu-id="f777d-112">Ukázka Hello *ClusterConfig.gMSA.Windows.MultiMachine.JSON* konfigurační soubor stažené společně se sadou hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) samostatný cluster balíček obsahuje šablony pro konfiguraci zabezpečení systému Windows pomocí [skupinový účet spravované služby (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span><span class="sxs-lookup"><span data-stu-id="f777d-112">hello sample *ClusterConfig.gMSA.Windows.MultiMachine.JSON* configuration file downloaded with hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security using [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span></span>  

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
  
| <span data-ttu-id="f777d-113">**Nastavení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="f777d-113">**Configuration Setting**</span></span> | <span data-ttu-id="f777d-114">**Popis**</span><span class="sxs-lookup"><span data-stu-id="f777d-114">**Description**</span></span> |  
| --- | --- |  
| <span data-ttu-id="f777d-115">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="f777d-115">WindowsIdentities</span></span> |<span data-ttu-id="f777d-116">Obsahuje identity hello clusteru a klientů.</span><span class="sxs-lookup"><span data-stu-id="f777d-116">Contains hello cluster and client identities.</span></span> |  
| <span data-ttu-id="f777d-117">ClustergMSAIdentity</span><span class="sxs-lookup"><span data-stu-id="f777d-117">ClustergMSAIdentity</span></span> |<span data-ttu-id="f777d-118">Nakonfiguruje zabezpečení mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="f777d-118">Configures node-to-node security.</span></span> <span data-ttu-id="f777d-119">Skupinu účet spravované služby.</span><span class="sxs-lookup"><span data-stu-id="f777d-119">A group managed service account.</span></span> |  
| <span data-ttu-id="f777d-120">ClusterSPN</span><span class="sxs-lookup"><span data-stu-id="f777d-120">ClusterSPN</span></span> |<span data-ttu-id="f777d-121">Plně kvalifikované domény hlavního názvu služby pro účet gMSA</span><span class="sxs-lookup"><span data-stu-id="f777d-121">Fully qualified domain SPN for gMSA account</span></span>|  
| <span data-ttu-id="f777d-122">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="f777d-122">ClientIdentities</span></span> |<span data-ttu-id="f777d-123">Nakonfiguruje klienta uzel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f777d-123">Configures client-to-node security.</span></span> <span data-ttu-id="f777d-124">Pole klienta uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f777d-124">An array of client user accounts.</span></span> |  
| <span data-ttu-id="f777d-125">Identita</span><span class="sxs-lookup"><span data-stu-id="f777d-125">Identity</span></span> |<span data-ttu-id="f777d-126">identita klienta Hello, uživatel domény.</span><span class="sxs-lookup"><span data-stu-id="f777d-126">hello client identity, a domain user.</span></span> |  
| <span data-ttu-id="f777d-127">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="f777d-127">IsAdmin</span></span> |<span data-ttu-id="f777d-128">Hodnota TRUE Určuje, že tento uživatel domény hello nemá přístup správce klienta, hodnotu false pro klientský přístup uživatele.</span><span class="sxs-lookup"><span data-stu-id="f777d-128">True specifies that hello domain user has administrator client access, false for user client access.</span></span> |  
  
<span data-ttu-id="f777d-129">[Uzel toonode zabezpečení](service-fabric-cluster-security.md#node-to-node-security) je nakonfigurované nastavením **ClustergMSAIdentity** když service fabric potřebuje toorun pod gMSA.</span><span class="sxs-lookup"><span data-stu-id="f777d-129">[Node toonode security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting **ClustergMSAIdentity** when service fabric needs toorun under gMSA.</span></span> <span data-ttu-id="f777d-130">V pořadí toobuild vztahy důvěryhodnosti mezi uzly se musí být provedeny vědět navzájem.</span><span class="sxs-lookup"><span data-stu-id="f777d-130">In order toobuild trust relationships between nodes, they must be made aware of each other.</span></span> <span data-ttu-id="f777d-131">Můžete to provést dvěma způsoby: hello skupinový účet spravované služby zahrnující všechny uzly v clusteru hello nebo zadejte skupinu hello domény počítače, který obsahuje všechny uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f777d-131">This can be accomplished in two different ways: Specify hello Group Managed Service Account that includes all nodes in hello cluster or Specify hello domain machine group that includes all nodes in hello cluster.</span></span> <span data-ttu-id="f777d-132">Důrazně doporučujeme používat hello [skupinový účet spravované služby (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) přístup, zejména pro větší clustery (víc než 10 uzlů) nebo pro clustery, které jsou pravděpodobně toogrow nebo zmenšit.</span><span class="sxs-lookup"><span data-stu-id="f777d-132">We strongly recommend using hello [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) approach, particularly for larger clusters (more than 10 nodes) or for clusters that are likely toogrow or shrink.</span></span>  
<span data-ttu-id="f777d-133">Tento přístup nevyžaduje hello vytvoření skupiny domény, pro který byla udělena práva tooadd přístup Správce clusterů a odeberte členy.</span><span class="sxs-lookup"><span data-stu-id="f777d-133">This approach does not require hello creation of a domain group for which cluster administrators have been granted access rights tooadd and remove members.</span></span> <span data-ttu-id="f777d-134">Tyto účty jsou také užitečná pro automatickou správu hesel.</span><span class="sxs-lookup"><span data-stu-id="f777d-134">These accounts are also useful for automatic password management.</span></span> <span data-ttu-id="f777d-135">Další informace najdete v tématu [Začínáme s skupinové účty spravované služby](http://technet.microsoft.com/library/jj128431.aspx).</span><span class="sxs-lookup"><span data-stu-id="f777d-135">For more information, see [Getting Started with Group Managed Service Accounts](http://technet.microsoft.com/library/jj128431.aspx).</span></span>  
 
<span data-ttu-id="f777d-136">[Zabezpečení klienta toonode](service-fabric-cluster-security.md#client-to-node-security) je konfigurován pomocí **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="f777d-136">[Client toonode security](service-fabric-cluster-security.md#client-to-node-security) is configured using **ClientIdentities**.</span></span> <span data-ttu-id="f777d-137">Pořadí tooestablish vztah důvěryhodnosti mezi klientem a hello clusteru musíte nakonfigurovat hello clusteru tooknow které identity klienta, které můžete důvěřovat.</span><span class="sxs-lookup"><span data-stu-id="f777d-137">In order tooestablish trust between a client and hello cluster, you must configure hello cluster tooknow which client identities that it can trust.</span></span> <span data-ttu-id="f777d-138">To můžete provést dvěma způsoby: Zadejte hello skupinu uživatele domény, které můžete připojit nebo zadat hello uzlu uživatele domény, které se můžou připojit.</span><span class="sxs-lookup"><span data-stu-id="f777d-138">This can be done in two different ways: Specify hello domain group users that can connect or specify hello domain node users that can connect.</span></span> <span data-ttu-id="f777d-139">Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené tooa cluster Service Fabric: správce a uživatele.</span><span class="sxs-lookup"><span data-stu-id="f777d-139">Service Fabric supports two different access control types for clients that are connected tooa Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="f777d-140">Řízení přístupu poskytuje schopnost hello hello toolimit Správce clusteru přístup toocertain typy operací clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f777d-140">Access control provides hello ability for hello cluster administrator toolimit access toocertain types of cluster operations for different groups of users, making hello cluster more secure.</span></span>  <span data-ttu-id="f777d-141">Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis).</span><span class="sxs-lookup"><span data-stu-id="f777d-141">Administrators have full access toomanagement capabilities (including read/write capabilities).</span></span> <span data-ttu-id="f777d-142">Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="f777d-142">Users, by default, have only read access toomanagement capabilities (for example, query capabilities), and hello ability tooresolve applications and services.</span></span> <span data-ttu-id="f777d-143">Další informace o řízení přístupu najdete v tématu [řízení přístupu podle rolí pro Service Fabric klienty](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="f777d-143">For more information on access controls, see [Role based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>  
 
<span data-ttu-id="f777d-144">v následujícím příkladu Hello **zabezpečení** části nakonfiguruje zabezpečení systému Windows pomocí gMSA a určuje, že hello počítačů v *ServiceFabric.clusterA.contoso.com* gMSA jsou součástí clusteru hello a že *CONTOSO\usera* má klient přístup správce:</span><span class="sxs-lookup"><span data-stu-id="f777d-144">hello following example **security** section configures Windows security using gMSA and specifies that hello machines in *ServiceFabric.clusterA.contoso.com* gMSA are part of hello cluster and that *CONTOSO\usera* has admin client access:</span></span>  
  
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
  
## <a name="configure-windows-security-using-a-machine-group"></a><span data-ttu-id="f777d-145">Konfigurace zabezpečení systému Windows ve skupině počítač</span><span class="sxs-lookup"><span data-stu-id="f777d-145">Configure Windows security using a machine group</span></span>  
<span data-ttu-id="f777d-146">Ukázka Hello *ClusterConfig.Windows.MultiMachine.JSON* konfigurační soubor stažené společně se sadou hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) samostatný cluster balíček obsahuje šablony pro konfiguraci zabezpečení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f777d-146">hello sample *ClusterConfig.Windows.MultiMachine.JSON* configuration file downloaded with hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security.</span></span>  <span data-ttu-id="f777d-147">Zabezpečení systému Windows je nakonfigurovaný v hello **vlastnosti** části:</span><span class="sxs-lookup"><span data-stu-id="f777d-147">Windows security is configured in hello **Properties** section:</span></span> 

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

| <span data-ttu-id="f777d-148">**Nastavení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="f777d-148">**Configuration setting**</span></span> | <span data-ttu-id="f777d-149">**Popis**</span><span class="sxs-lookup"><span data-stu-id="f777d-149">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="f777d-150">ClusterCredentialType</span><span class="sxs-lookup"><span data-stu-id="f777d-150">ClusterCredentialType</span></span> |<span data-ttu-id="f777d-151">**ClusterCredentialType** je nastaven příliš*Windows* Pokud ClusterIdentity Určuje název skupiny počítače Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f777d-151">**ClusterCredentialType** is set too*Windows* if ClusterIdentity specifies an Active Directory Machine Group Name.</span></span> |  
| <span data-ttu-id="f777d-152">ServerCredentialType</span><span class="sxs-lookup"><span data-stu-id="f777d-152">ServerCredentialType</span></span> |<span data-ttu-id="f777d-153">Nastavení příliš*Windows* tooenable zabezpečení systému Windows pro klienty.</span><span class="sxs-lookup"><span data-stu-id="f777d-153">Set too*Windows* tooenable Windows security for clients.</span></span><br /><br /><span data-ttu-id="f777d-154">To znamená, že klienti hello hello clusteru a samotného clusteru hello běží v rámci domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f777d-154">This indicates that hello clients of hello cluster and hello cluster itself are running within an Active Directory domain.</span></span> |  
| <span data-ttu-id="f777d-155">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="f777d-155">WindowsIdentities</span></span> |<span data-ttu-id="f777d-156">Obsahuje identity hello clusteru a klientů.</span><span class="sxs-lookup"><span data-stu-id="f777d-156">Contains hello cluster and client identities.</span></span> |  
| <span data-ttu-id="f777d-157">ClusterIdentity</span><span class="sxs-lookup"><span data-stu-id="f777d-157">ClusterIdentity</span></span> |<span data-ttu-id="f777d-158">Použijte název skupiny počítače, domain\machinegroup, tooconfigure-uzly zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f777d-158">Use a machine group name, domain\machinegroup, tooconfigure node-to-node security.</span></span> |  
| <span data-ttu-id="f777d-159">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="f777d-159">ClientIdentities</span></span> |<span data-ttu-id="f777d-160">Nakonfiguruje klienta uzel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f777d-160">Configures client-to-node security.</span></span> <span data-ttu-id="f777d-161">Pole klienta uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f777d-161">An array of client user accounts.</span></span> |  
| <span data-ttu-id="f777d-162">Identita</span><span class="sxs-lookup"><span data-stu-id="f777d-162">Identity</span></span> |<span data-ttu-id="f777d-163">Přidejte uživatele domény hello, doména\uživatelské jméno pro identitu klienta hello.</span><span class="sxs-lookup"><span data-stu-id="f777d-163">Add hello domain user, domain\username, for hello client identity.</span></span> |  
| <span data-ttu-id="f777d-164">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="f777d-164">IsAdmin</span></span> |<span data-ttu-id="f777d-165">Toospecify tootrue sadu, která hello uživatele domény nemá přístup správce klienta, nebo hodnotu NEPRAVDA pro klientský přístup uživatele.</span><span class="sxs-lookup"><span data-stu-id="f777d-165">Set tootrue toospecify that hello domain user has administrator client access or false for user client access.</span></span> |  

<span data-ttu-id="f777d-166">[Uzel toonode zabezpečení](service-fabric-cluster-security.md#node-to-node-security) se konfiguruje pomocí nastavení **ClusterIdentity** Pokud chcete, aby toouse skupinu počítače v doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f777d-166">[Node toonode security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting using **ClusterIdentity** if you want toouse a machine group within an Active Directory Domain.</span></span> <span data-ttu-id="f777d-167">Další informace najdete v tématu [vytvořit skupinu počítače ve službě Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span><span class="sxs-lookup"><span data-stu-id="f777d-167">For more information, see [Create a Machine Group in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span></span>

<span data-ttu-id="f777d-168">[Uzel Klient zabezpečení](service-fabric-cluster-security.md#client-to-node-security) se konfiguruje pomocí **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="f777d-168">[Client-to-node security](service-fabric-cluster-security.md#client-to-node-security) is configured by using **ClientIdentities**.</span></span> <span data-ttu-id="f777d-169">tooestablish vztah důvěryhodnosti mezi klientem a hello clusteru, je nutné nakonfigurovat hello clusteru tooknow hello klienta, které můžete důvěřovat identity, které hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f777d-169">tooestablish trust between a client and hello cluster, you must configure hello cluster tooknow hello client identities that hello cluster can trust.</span></span> <span data-ttu-id="f777d-170">Můžete vytvořit vztah důvěryhodnosti v dvěma různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="f777d-170">You can establish trust in two different ways:</span></span>

- <span data-ttu-id="f777d-171">Zadejte hello skupinu uživatele domény, které se můžou připojit.</span><span class="sxs-lookup"><span data-stu-id="f777d-171">Specify hello domain group users that can connect.</span></span>
- <span data-ttu-id="f777d-172">Zadejte hello uzlu uživatele domény, které se můžou připojit.</span><span class="sxs-lookup"><span data-stu-id="f777d-172">Specify hello domain node users that can connect.</span></span>

<span data-ttu-id="f777d-173">Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené tooa cluster Service Fabric: správce a uživatele.</span><span class="sxs-lookup"><span data-stu-id="f777d-173">Service Fabric supports two different access control types for clients that are connected tooa Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="f777d-174">Řízení přístupu umožňuje hello clusteru správce toolimit přístup toocertain typy operací clusteru pro různé skupiny uživatelů, takže hello clusteru bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="f777d-174">Access control enables hello cluster administrator toolimit access toocertain types of cluster operations for different groups of users, which makes hello cluster more secure.</span></span>  <span data-ttu-id="f777d-175">Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis).</span><span class="sxs-lookup"><span data-stu-id="f777d-175">Administrators have full access toomanagement capabilities (including read/write capabilities).</span></span> <span data-ttu-id="f777d-176">Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="f777d-176">Users, by default, have only read access toomanagement capabilities (for example, query capabilities), and hello ability tooresolve applications and services.</span></span>  

<span data-ttu-id="f777d-177">Následující příklad Hello **zabezpečení** části nakonfiguruje zabezpečení systému Windows, určuje, že hello počítačů v *ServiceFabric/clusterA.contoso.com* jsou součástí clusteru hello a určuje, že  *CONTOSO\usera* má klient přístup správce:</span><span class="sxs-lookup"><span data-stu-id="f777d-177">hello following example **security** section configures Windows security, specifies that hello machines in *ServiceFabric/clusterA.contoso.com* are part of hello cluster, and specifies that *CONTOSO\usera* has admin client access:</span></span>

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
> <span data-ttu-id="f777d-178">Service Fabric nesmí být nasazen na řadiči domény.</span><span class="sxs-lookup"><span data-stu-id="f777d-178">Service Fabric should not be deployed on a domain controller.</span></span> <span data-ttu-id="f777d-179">Ujistěte se, že souboru ClusterConfig.json zahrnovat hello IP adresu řadiče domény hello při použití skupiny počítačů nebo skupinový účet spravované služby (gMSA).</span><span class="sxs-lookup"><span data-stu-id="f777d-179">Make sure that ClusterConfig.json does not include hello IP address of hello domain controller when using a machine group or group Managed Service Account (gMSA).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="f777d-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f777d-180">Next steps</span></span>
<span data-ttu-id="f777d-181">Po dokončení konfigurace zabezpečení systému Windows v hello *souboru ClusterConfig.JSON* souboru, pokračovat v procesu vytváření clusteru hello v [vytvořit samostatnou clusteru se systémem Windows](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="f777d-181">After configuring Windows security in hello *ClusterConfig.JSON* file, resume hello cluster creation process in [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span>

<span data-ttu-id="f777d-182">Další informace o tom, jak-uzly zabezpečení, klientský uzel zabezpečení a řízení přístupu na základě rolí, najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="f777d-182">For more information about how node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

<span data-ttu-id="f777d-183">V tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md) příklady připojení pomocí prostředí PowerShell nebo FabricClient.</span><span class="sxs-lookup"><span data-stu-id="f777d-183">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for examples of connecting by using PowerShell or FabricClient.</span></span>
