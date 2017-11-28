---
title: "aaaConfigure zabezpečení Azure Service Fabric připojení clusteru | Microsoft Docs"
description: "Zjistěte, jak toouse Visual Studio tooconfigure zabezpečené připojení, která podporují Azure Service Fabric clusterem hello."
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
ms.openlocfilehash: ed3e5043264cf026f74e24ca09b40ccc70086cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-connections-tooa-service-fabric-cluster-from-visual-studio"></a><span data-ttu-id="346cb-103">Konfigurace clusteru Service Fabric tooa zabezpečené připojení ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="346cb-103">Configure secure connections tooa Service Fabric cluster from Visual Studio</span></span>
<span data-ttu-id="346cb-104">Zjistěte, jak toouse Visual Studio toosecurely přistupovat k clusteru služby Azure Service Fabric s nakonfigurované zásady řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="346cb-104">Learn how toouse Visual Studio toosecurely access an Azure Service Fabric cluster with access control policies configured.</span></span>

## <a name="cluster-connection-types"></a><span data-ttu-id="346cb-105">Typy připojení clusteru</span><span class="sxs-lookup"><span data-stu-id="346cb-105">Cluster connection types</span></span>
<span data-ttu-id="346cb-106">Cluster Azure Service Fabric hello podporuje dva typy připojení: **nezabezpečené** připojení a **x509 založené na certifikátech** zabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="346cb-106">Two types of connections are supported by hello Azure Service Fabric cluster: **non-secure** connections and **x509 certificate-based** secure connections.</span></span> <span data-ttu-id="346cb-107">(Pro clustery infrastruktury služby hostované na místních počítačích **Windows** a **dSTS** ověřování jsou také podporovány.) Typ připojení clusteru hello tooconfigure máte, když vytváří se hello cluster.</span><span class="sxs-lookup"><span data-stu-id="346cb-107">(For Service Fabric clusters hosted on-premises, **Windows** and **dSTS** authentications are also supported.) You have tooconfigure hello cluster connection type when hello cluster is being created.</span></span> <span data-ttu-id="346cb-108">Jakmile je vytvořen, nelze změnit typ připojení hello.</span><span class="sxs-lookup"><span data-stu-id="346cb-108">Once it's created, hello connection type can’t be changed.</span></span>

<span data-ttu-id="346cb-109">Nástroje sady Visual Studio Service Fabric Hello podporovat všechny typy ověřování pro připojování tooa cluster pro publikování.</span><span class="sxs-lookup"><span data-stu-id="346cb-109">hello Visual Studio Service Fabric tools support all authentication types for connecting tooa cluster for publishing.</span></span> <span data-ttu-id="346cb-110">Najdete v části [nastavení cluster Service Fabric z portálu Azure hello](service-fabric-cluster-creation-via-portal.md) pokyny tooset zabezpečení clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="346cb-110">See [Setting up a Service Fabric cluster from hello Azure portal](service-fabric-cluster-creation-via-portal.md) for instructions on how tooset up a secure Service Fabric cluster.</span></span>

## <a name="configure-cluster-connections-in-publish-profiles"></a><span data-ttu-id="346cb-111">Konfigurace připojení clusteru v publikační profily</span><span class="sxs-lookup"><span data-stu-id="346cb-111">Configure cluster connections in publish profiles</span></span>
<span data-ttu-id="346cb-112">Pokud publikujete projektu Service Fabric v sadě Visual Studio, použijte hello **publikovat aplikace Service Fabric** dialogové okno pole toochoose clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="346cb-112">If you publish a Service Fabric project from Visual Studio, use hello **Publish Service Fabric Application** dialog box toochoose an Azure Service Fabric cluster.</span></span> <span data-ttu-id="346cb-113">V části **koncového bodu připojení**, vyberte existující cluster v rámci svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="346cb-113">Under **Connection endpoint**, select an existing cluster under your subscription.</span></span>

![Hello ** publikování Service Fabric aplikace ** dialogové okno je použité tooconfigure Service Fabric připojení.][publishdialog]

<span data-ttu-id="346cb-115">Hello **publikovat aplikace Service Fabric** dialogové okno automaticky ověří připojení clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="346cb-115">hello **Publish Service Fabric Application** dialog box automatically validates hello cluster connection.</span></span> <span data-ttu-id="346cb-116">Pokud budete vyzváni, přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="346cb-116">If prompted, sign in tooyour Azure account.</span></span> <span data-ttu-id="346cb-117">V případě úspěšného ověření, znamená to, že má váš systém správné nainstalované certifikáty tooconnect toohello clusteru bezpečně nebo clusteru je nezabezpečené hello.</span><span class="sxs-lookup"><span data-stu-id="346cb-117">If validation passes, it means that your system has hello correct certificates installed tooconnect toohello cluster securely, or your cluster is non-secure.</span></span> <span data-ttu-id="346cb-118">Selhání ověření můžete způsobeno problémy se síťovým nebo nemá clusteru zabezpečené tooa tooconnect systému správně nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="346cb-118">Validation failures can be caused by network issues or by not having your system correctly configured tooconnect tooa secure cluster.</span></span>

![Hello ** publikování Service Fabric aplikace ** dialogové okno ověří stávající správně nakonfigurované připojení clusteru Service Fabric.][selectsfcluster]

### <a name="tooconnect-tooa-secure-cluster"></a><span data-ttu-id="346cb-120">tooconnect tooa zabezpečení clusteru</span><span class="sxs-lookup"><span data-stu-id="346cb-120">tooconnect tooa secure cluster</span></span>
1. <span data-ttu-id="346cb-121">Ujistěte se, že dostanete mezi hello klientské certifikáty, které hello vztahy důvěryhodnosti cílového clusteru.</span><span class="sxs-lookup"><span data-stu-id="346cb-121">Make sure you can access one of hello client certificates that hello destination cluster trusts.</span></span> <span data-ttu-id="346cb-122">certifikát Hello se obvykle sdílí jako soubor Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="346cb-122">hello certificate is usually shared as a Personal Information Exchange (.pfx) file.</span></span> <span data-ttu-id="346cb-123">V tématu [nastavení cluster Service Fabric z portálu Azure hello](service-fabric-cluster-creation-via-portal.md) pro přístupu tooconfigure hello server toogrant tooa klienta.</span><span class="sxs-lookup"><span data-stu-id="346cb-123">See [Setting up a Service Fabric cluster from hello Azure portal](service-fabric-cluster-creation-via-portal.md) for how tooconfigure hello server toogrant access tooa client.</span></span>
2. <span data-ttu-id="346cb-124">Nainstalujte certifikát důvěryhodné hello.</span><span class="sxs-lookup"><span data-stu-id="346cb-124">Install hello trusted certificate.</span></span> <span data-ttu-id="346cb-125">toodo, poklikejte na soubor .pfx hello, nebo použijte hello prostředí PowerShell skriptu importu PfxCertificate tooimport hello certifikáty.</span><span class="sxs-lookup"><span data-stu-id="346cb-125">toodo this, double-click hello .pfx file, or use hello PowerShell script Import-PfxCertificate tooimport hello certificates.</span></span> <span data-ttu-id="346cb-126">Nainstalujte certifikát hello příliš**Cert: \LocalMachine\My**.</span><span class="sxs-lookup"><span data-stu-id="346cb-126">Install hello certificate too**Cert:\LocalMachine\My**.</span></span> <span data-ttu-id="346cb-127">Je OK tooaccept všechna výchozí nastavení při importu certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="346cb-127">It's OK tooaccept all default settings while importing hello certificate.</span></span>
3. <span data-ttu-id="346cb-128">Zvolte hello **publikování...**  příkaz v místní nabídce hello hello projektu tooopen hello **publikování aplikaci Azure** dialogové okno a potom vyberte hello cílový cluster.</span><span class="sxs-lookup"><span data-stu-id="346cb-128">Choose hello **Publish...** command on hello shortcut menu of hello project tooopen hello **Publish Azure Application** dialog box and then select hello target cluster.</span></span> <span data-ttu-id="346cb-129">Nástroj Hello automaticky vyřeší hello připojení a uloží hello zabezpečené připojení, které parametry v hello Publikovat profil.</span><span class="sxs-lookup"><span data-stu-id="346cb-129">hello tool automatically resolves hello connection and saves hello secure connection parameters in hello publish profile.</span></span>
4. <span data-ttu-id="346cb-130">Volitelné: Můžete upravit hello publikování toospecify profil připojení zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="346cb-130">Optional: You can edit hello publish profile toospecify a secure cluster connection.</span></span>
   
   <span data-ttu-id="346cb-131">Vzhledem k tomu, že ručně upravujete hello Publikovat profil XML soubor toospecify hello informací o certifikátu, že název úložiště certifikátu toonote hello, uložení umístění a kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="346cb-131">Since you're manually editing hello Publish Profile XML file toospecify hello certificate information, be sure toonote hello certificate store name, store location, and certificate thumbprint.</span></span> <span data-ttu-id="346cb-132">Budete potřebovat tooprovide tyto hodnoty pro úložiště certifikátů hello název a umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="346cb-132">You'll need tooprovide these values for hello certificate's store name and store location.</span></span> <span data-ttu-id="346cb-133">V tématu [postupy: načtení hello kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="346cb-133">See [How to: Retrieve hello Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) for more information.</span></span>
   
   <span data-ttu-id="346cb-134">Můžete použít hello *ClusterConnectionParameters* parametry toospecify hello prostředí PowerShell parametry toouse při připojování toohello cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="346cb-134">You can use hello *ClusterConnectionParameters* parameters toospecify hello PowerShell parameters toouse when connecting toohello Service Fabric cluster.</span></span> <span data-ttu-id="346cb-135">Platné parametry jsou všechny, které přijímají hello Connect-ServiceFabricCluster rutinou.</span><span class="sxs-lookup"><span data-stu-id="346cb-135">Valid parameters are any that are accepted by hello Connect-ServiceFabricCluster cmdlet.</span></span> <span data-ttu-id="346cb-136">V tématu [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) seznam dostupných parametrů.</span><span class="sxs-lookup"><span data-stu-id="346cb-136">See [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) for a list of available parameters.</span></span>
   
   <span data-ttu-id="346cb-137">Pokud publikujete tooa vzdálený cluster, musíte pro tento konkrétní cluster toospecify hello příslušné parametry.</span><span class="sxs-lookup"><span data-stu-id="346cb-137">If you’re publishing tooa remote cluster, you need toospecify hello appropriate parameters for that specific cluster.</span></span> <span data-ttu-id="346cb-138">Hello tady je příklad připojování tooa nezabezpečené clusteru:</span><span class="sxs-lookup"><span data-stu-id="346cb-138">hello following is an example of connecting tooa non-secure cluster:</span></span>
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   <span data-ttu-id="346cb-139">Tady je příklad pro připojování tooan x509, na základě certifikátu zabezpečeného clusteru:</span><span class="sxs-lookup"><span data-stu-id="346cb-139">Here’s an example for connecting tooan x509 certificate-based secure cluster:</span></span>
   
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
5. <span data-ttu-id="346cb-140">Upravit další potřebné nastavení, například upgradu parametry a umístění souboru aplikace parametr a pak publikujte aplikaci z hello **publikovat aplikace Service Fabric** dialogové okno v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="346cb-140">Edit any other necessary settings, such as upgrade parameters and Application Parameter file location, and then publish your application from hello **Publish Service Fabric Application** dialog box in Visual Studio.</span></span>

## <a name="next-steps"></a><span data-ttu-id="346cb-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="346cb-141">Next steps</span></span>
<span data-ttu-id="346cb-142">Další informace o přístupu k clusterů Service Fabric najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="346cb-142">For more information about accessing Service Fabric clusters, see [Visualizing your cluster by using Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
