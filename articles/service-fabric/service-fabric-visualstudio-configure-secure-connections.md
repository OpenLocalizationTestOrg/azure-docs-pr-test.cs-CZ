---
title: "Konfigurace zabezpečeného připojení clusteru Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat zabezpečené připojení, které jsou podporovány v clusteru Azure Service Fabric pomocí sady Visual Studio."
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: dc07b2f38d6fd2de941ebbe99303f6e63cbf122d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a><span data-ttu-id="c2741-103">Konfigurace zabezpečeného připojení ke clusteru Service Fabric ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c2741-103">Configure secure connections to a Service Fabric cluster from Visual Studio</span></span>
<span data-ttu-id="c2741-104">Další informace o použití sady Visual Studio bezpečný přístup k clusteru služby Azure Service Fabric s nakonfigurované zásady řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="c2741-104">Learn how to use Visual Studio to securely access an Azure Service Fabric cluster with access control policies configured.</span></span>

## <a name="cluster-connection-types"></a><span data-ttu-id="c2741-105">Typy připojení clusteru</span><span class="sxs-lookup"><span data-stu-id="c2741-105">Cluster connection types</span></span>
<span data-ttu-id="c2741-106">Cluster Azure Service Fabric podporuje dva typy připojení: **nezabezpečené** připojení a **x509 založené na certifikátech** zabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="c2741-106">Two types of connections are supported by the Azure Service Fabric cluster: **non-secure** connections and **x509 certificate-based** secure connections.</span></span> <span data-ttu-id="c2741-107">(Pro clustery infrastruktury služby hostované na místních počítačích **Windows** a **dSTS** ověřování jsou také podporovány.) Je nutné konfigurovat typ připojení clusteru při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c2741-107">(For Service Fabric clusters hosted on-premises, **Windows** and **dSTS** authentications are also supported.) You have to configure the cluster connection type when the cluster is being created.</span></span> <span data-ttu-id="c2741-108">Jakmile je vytvořen, nelze změnit typ připojení.</span><span class="sxs-lookup"><span data-stu-id="c2741-108">Once it's created, the connection type can’t be changed.</span></span>

<span data-ttu-id="c2741-109">Nástroje sady Visual Studio Service Fabric podporovat všechny typy ověřování pro připojení ke clusteru s podporou pro publikování.</span><span class="sxs-lookup"><span data-stu-id="c2741-109">The Visual Studio Service Fabric tools support all authentication types for connecting to a cluster for publishing.</span></span> <span data-ttu-id="c2741-110">V tématu [nastavení cluster Service Fabric na portálu Azure](service-fabric-cluster-creation-via-portal.md) pokyny, jak nastavit zabezpečení clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c2741-110">See [Setting up a Service Fabric cluster from the Azure portal](service-fabric-cluster-creation-via-portal.md) for instructions on how to set up a secure Service Fabric cluster.</span></span>

## <a name="configure-cluster-connections-in-publish-profiles"></a><span data-ttu-id="c2741-111">Konfigurace připojení clusteru v publikační profily</span><span class="sxs-lookup"><span data-stu-id="c2741-111">Configure cluster connections in publish profiles</span></span>
<span data-ttu-id="c2741-112">Pokud publikujete projektu Service Fabric v sadě Visual Studio, použijte **publikovat aplikace Service Fabric** dialogové okno Vybrat clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c2741-112">If you publish a Service Fabric project from Visual Studio, use the **Publish Service Fabric Application** dialog box to choose an Azure Service Fabric cluster.</span></span> <span data-ttu-id="c2741-113">V části **koncového bodu připojení**, vyberte existující cluster v rámci svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="c2741-113">Under **Connection endpoint**, select an existing cluster under your subscription.</span></span>

![** Publikování Service Fabric aplikace ** dialogové okno slouží ke konfiguraci připojení Service Fabric.][publishdialog]

<span data-ttu-id="c2741-115">**Publikovat aplikace Service Fabric** dialogové okno automaticky ověří připojení clusteru.</span><span class="sxs-lookup"><span data-stu-id="c2741-115">The **Publish Service Fabric Application** dialog box automatically validates the cluster connection.</span></span> <span data-ttu-id="c2741-116">Pokud budete vyzváni, přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="c2741-116">If prompted, sign in to your Azure account.</span></span> <span data-ttu-id="c2741-117">V případě úspěšného ověření, znamená to, že má váš systém správné certifikáty pro připojení ke clusteru bezpečně nebo clusteru je nezabezpečené.</span><span class="sxs-lookup"><span data-stu-id="c2741-117">If validation passes, it means that your system has the correct certificates installed to connect to the cluster securely, or your cluster is non-secure.</span></span> <span data-ttu-id="c2741-118">Selhání ověření může být způsobeno problémy se síťovým nebo tak, že nejsou správně konfigurovány pro připojení ke clusteru s podporou zabezpečení systému.</span><span class="sxs-lookup"><span data-stu-id="c2741-118">Validation failures can be caused by network issues or by not having your system correctly configured to connect to a secure cluster.</span></span>

![** Publikování Service Fabric aplikace ** dialogové okno ověří stávající správně nakonfigurované připojení clusteru Service Fabric.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a><span data-ttu-id="c2741-120">Připojit k zabezpečení clusteru</span><span class="sxs-lookup"><span data-stu-id="c2741-120">To connect to a secure cluster</span></span>
1. <span data-ttu-id="c2741-121">Ujistěte se, že vám přístup mezi klientské certifikáty, které důvěřuje cílový cluster.</span><span class="sxs-lookup"><span data-stu-id="c2741-121">Make sure you can access one of the client certificates that the destination cluster trusts.</span></span> <span data-ttu-id="c2741-122">Certifikát se obvykle sdílí jako soubor Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="c2741-122">The certificate is usually shared as a Personal Information Exchange (.pfx) file.</span></span> <span data-ttu-id="c2741-123">V tématu [nastavení cluster Service Fabric na portálu Azure](service-fabric-cluster-creation-via-portal.md) pro konfiguraci serveru k udělení přístupu ke klientovi.</span><span class="sxs-lookup"><span data-stu-id="c2741-123">See [Setting up a Service Fabric cluster from the Azure portal](service-fabric-cluster-creation-via-portal.md) for how to configure the server to grant access to a client.</span></span>
2. <span data-ttu-id="c2741-124">Nainstalujte důvěryhodný certifikát.</span><span class="sxs-lookup"><span data-stu-id="c2741-124">Install the trusted certificate.</span></span> <span data-ttu-id="c2741-125">Chcete-li to provést, poklikejte na soubor .pfx nebo pomocí skriptu prostředí PowerShell Import PfxCertificate pro import certifikátů.</span><span class="sxs-lookup"><span data-stu-id="c2741-125">To do this, double-click the .pfx file, or use the PowerShell script Import-PfxCertificate to import the certificates.</span></span> <span data-ttu-id="c2741-126">Nainstalujte certifikát, který chcete **Cert: \LocalMachine\My**.</span><span class="sxs-lookup"><span data-stu-id="c2741-126">Install the certificate to **Cert:\LocalMachine\My**.</span></span> <span data-ttu-id="c2741-127">Je OK přijměte všechna výchozí nastavení při importu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c2741-127">It's OK to accept all default settings while importing the certificate.</span></span>
3. <span data-ttu-id="c2741-128">Vyberte **publikování...**  příkaz v místní nabídce projektu otevřete **publikování aplikaci Azure** dialogové okno a potom vyberte cílový cluster.</span><span class="sxs-lookup"><span data-stu-id="c2741-128">Choose the **Publish...** command on the shortcut menu of the project to open the **Publish Azure Application** dialog box and then select the target cluster.</span></span> <span data-ttu-id="c2741-129">Nástroj automaticky vyřeší připojení a uloží parametry zabezpečené připojení v profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="c2741-129">The tool automatically resolves the connection and saves the secure connection parameters in the publish profile.</span></span>
4. <span data-ttu-id="c2741-130">Volitelné: Můžete upravit profilu publikování k určení připojení zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="c2741-130">Optional: You can edit the publish profile to specify a secure cluster connection.</span></span>
   
   <span data-ttu-id="c2741-131">Vzhledem k tomu, že jste ruční úpravy souboru XML profilu publikování k určení informací o certifikátu, poznamenejte si název úložiště certifikátu, uložení, umístění a kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c2741-131">Since you're manually editing the Publish Profile XML file to specify the certificate information, be sure to note the certificate store name, store location, and certificate thumbprint.</span></span> <span data-ttu-id="c2741-132">Budete muset zadat tyto hodnoty pro úložiště certifikátu, název a umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="c2741-132">You'll need to provide these values for the certificate's store name and store location.</span></span> <span data-ttu-id="c2741-133">V tématu [postupy: načtení kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="c2741-133">See [How to: Retrieve the Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) for more information.</span></span>
   
   <span data-ttu-id="c2741-134">Můžete použít *ClusterConnectionParameters* parametry zadat parametry prostředí PowerShell, které chcete použít při připojení ke clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c2741-134">You can use the *ClusterConnectionParameters* parameters to specify the PowerShell parameters to use when connecting to the Service Fabric cluster.</span></span> <span data-ttu-id="c2741-135">Platné parametry jsou všechny, které přijímají Connect-ServiceFabricCluster rutinou.</span><span class="sxs-lookup"><span data-stu-id="c2741-135">Valid parameters are any that are accepted by the Connect-ServiceFabricCluster cmdlet.</span></span> <span data-ttu-id="c2741-136">V tématu [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) seznam dostupných parametrů.</span><span class="sxs-lookup"><span data-stu-id="c2741-136">See [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) for a list of available parameters.</span></span>
   
   <span data-ttu-id="c2741-137">Pokud publikujete na vzdálený cluster, je třeba zadat příslušné parametry pro tento konkrétní cluster.</span><span class="sxs-lookup"><span data-stu-id="c2741-137">If you’re publishing to a remote cluster, you need to specify the appropriate parameters for that specific cluster.</span></span> <span data-ttu-id="c2741-138">Následuje příklad připojování ke clusteru nezabezpečené:</span><span class="sxs-lookup"><span data-stu-id="c2741-138">The following is an example of connecting to a non-secure cluster:</span></span>
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   <span data-ttu-id="c2741-139">Tady je příklad pro připojení k x509 na základě certifikátu zabezpečeného clusteru:</span><span class="sxs-lookup"><span data-stu-id="c2741-139">Here’s an example for connecting to an x509 certificate-based secure cluster:</span></span>
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. <span data-ttu-id="c2741-140">Upravit další potřebné nastavení, například upgradu parametry a umístění souboru aplikace parametr a pak publikujte aplikaci z **publikovat aplikace Service Fabric** dialogové okno v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2741-140">Edit any other necessary settings, such as upgrade parameters and Application Parameter file location, and then publish your application from the **Publish Service Fabric Application** dialog box in Visual Studio.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c2741-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2741-141">Next steps</span></span>
<span data-ttu-id="c2741-142">Další informace o přístupu k clusterů Service Fabric najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="c2741-142">For more information about accessing Service Fabric clusters, see [Visualizing your cluster by using Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
