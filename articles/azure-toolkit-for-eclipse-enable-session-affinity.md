---
title: "Povolit spřažení relace pomocí sady nástrojů pro Azure pro Eclipse"
description: "Zjistěte, jak chcete povolit spřažení relace pomocí sady nástrojů pro Azure pro Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 88c595ec-7d85-40bd-9078-8d6be7b3f0fa
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: ab8623d6f9751ed6d71d9a5b1c0d5e939c442862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-session-affinity"></a><span data-ttu-id="d1670-103">Povolit spřažení relace</span><span class="sxs-lookup"><span data-stu-id="d1670-103">Enable Session Affinity</span></span>
<span data-ttu-id="d1670-104">V rámci sady nástrojů Azure pro prostředí Eclipse můžete povolit spřažení relace HTTP, nebo "trvalé relace", pro své role.</span><span class="sxs-lookup"><span data-stu-id="d1670-104">Within the Azure Toolkit for Eclipse, you can enable HTTP session affinity, or "sticky sessions", for your roles.</span></span> <span data-ttu-id="d1670-105">Na následujícím obrázku **Vyrovnávání zatížení** dialogové okno Vlastnosti použít k povolení této funkce spřažení relace:</span><span class="sxs-lookup"><span data-stu-id="d1670-105">The following image shows the **Load Balancing** properties dialog used to enable the session affinity feature:</span></span>

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a><span data-ttu-id="d1670-106">Chcete-li povolit spřažení relace pro vaši roli</span><span class="sxs-lookup"><span data-stu-id="d1670-106">To enable session affinity for your role</span></span>
1. <span data-ttu-id="d1670-107">Klikněte pravým tlačítkem na roli v prohlížeči projektu Eclipse společnosti, klikněte na tlačítko **Azure**a potom klikněte na **Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="d1670-107">Right-click the role in Eclipse's Project Explorer, click **Azure**, and then click **Load Balancing**.</span></span>

2. <span data-ttu-id="d1670-108">V **vlastnosti pro vyrovnávání zatížení WorkerRole1** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="d1670-108">In the **Properties for WorkerRole1 Load Balancing** dialog:</span></span>

   <span data-ttu-id="d1670-109">a.</span><span class="sxs-lookup"><span data-stu-id="d1670-109">a.</span></span> <span data-ttu-id="d1670-110">Zkontrolujte **spřažení relace povolit HTTP (trvalé relace) pro tuto roli.**</span><span class="sxs-lookup"><span data-stu-id="d1670-110">Check **Enable HTTP session affinity (sticky sessions) for this role.**</span></span>

   <span data-ttu-id="d1670-111">b.</span><span class="sxs-lookup"><span data-stu-id="d1670-111">b.</span></span> <span data-ttu-id="d1670-112">Pro **vstupní koncový bod používat**, vyberte vstupní koncový bod chcete použít, například **http (veřejné: 80, privátní: 8080)**.</span><span class="sxs-lookup"><span data-stu-id="d1670-112">For **Input endpoint to use**, select an input endpoint to use, for example, **http (public:80, private:8080)**.</span></span> <span data-ttu-id="d1670-113">Aplikace musí používat tento koncový bod jako svůj koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="d1670-113">Your application must use this endpoint as its HTTP endpoint.</span></span> <span data-ttu-id="d1670-114">Víc koncových bodů můžete povolit pro vaši roli, ale můžete vybrat jen jeden z nich pro podporu trvalé relace.</span><span class="sxs-lookup"><span data-stu-id="d1670-114">You can enable multiple endpoints for your role, but you can select only one of them to support sticky sessions.</span></span>

   <span data-ttu-id="d1670-115">c.</span><span class="sxs-lookup"><span data-stu-id="d1670-115">c.</span></span> <span data-ttu-id="d1670-116">Sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1670-116">Rebuild your application.</span></span>

<span data-ttu-id="d1670-117">Jakmile bude povoleno, pokud máte více než jednu instanci role, bude požadavky HTTP z konkrétního klienta zpracovanou stejnou instanci role.</span><span class="sxs-lookup"><span data-stu-id="d1670-117">Once enabled, if you have more than one role instance, HTTP requests coming from a particular client will continue being handled by the same role instance.</span></span>

<span data-ttu-id="d1670-118">Sada nástrojů Eclipse umožňuje to nainstalováním speciální IIS modul s názvem směrování (žádostí na aplikace) do každé instance role.</span><span class="sxs-lookup"><span data-stu-id="d1670-118">The Eclipse Toolkit enables this by installing a special IIS module called Application Request Routing (ARR) into each of your role instances.</span></span> <span data-ttu-id="d1670-119">Směrování žádostí na aplikace přesměrovává požadavky HTTP na instanci příslušné role.</span><span class="sxs-lookup"><span data-stu-id="d1670-119">ARR reroutes HTTP requests to the appropriate role instance.</span></span> <span data-ttu-id="d1670-120">Sada nástrojů lokalita automaticky překonfiguruje vybraný koncový bod, aby příchozí provoz protokolu HTTP, je nejprve směrovaly softwaru směrování žádostí na aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1670-120">The toolkit automatically reconfigures the selected endpoint so that the incoming HTTP traffic is first routed to the ARR software.</span></span> <span data-ttu-id="d1670-121">Sada nástrojů taky vytvoří novou vnitřní koncový bod, Java server je nakonfigurován pro naslouchání na.</span><span class="sxs-lookup"><span data-stu-id="d1670-121">The toolkit also creates a new internal endpoint that your Java server is configured to listen to.</span></span> <span data-ttu-id="d1670-122">To je s koncovým bodem používaným směrování žádostí na aplikace přesměrovat provoz protokolu HTTP k instanci příslušné role.</span><span class="sxs-lookup"><span data-stu-id="d1670-122">That is the endpoint used by ARR to reroute the HTTP traffic to the appropriate role instance.</span></span> <span data-ttu-id="d1670-123">Tímto způsobem, každá instance role ve vašem nasazení s více instancemi slouží jako reverzní proxy server pro všechny ostatní instance povolení trvalé relace.</span><span class="sxs-lookup"><span data-stu-id="d1670-123">This way, each role instance in your multi-instance deployment serves as a reverse proxy for all the other instances, enabling sticky sessions.</span></span>

## <a name="notes-about-session-affinity"></a><span data-ttu-id="d1670-124">Poznámky o spřažení relace</span><span class="sxs-lookup"><span data-stu-id="d1670-124">Notes about session affinity</span></span>
* <span data-ttu-id="d1670-125">Spřažení relace v emulátoru služby výpočty v nefunguje.</span><span class="sxs-lookup"><span data-stu-id="d1670-125">Session affinity does not work in the compute emulator.</span></span> <span data-ttu-id="d1670-126">Nastavení můžete použít v emulátoru služby výpočty v bez zasahování vašeho procesu sestavení nebo výpočetní emulátor spuštění, ale funkce nefunguje v emulátoru služby výpočty v.</span><span class="sxs-lookup"><span data-stu-id="d1670-126">The settings can be applied in the compute emulator without interfering with your build process or compute emulator execution, but the feature itself does not function within the compute emulator.</span></span>

* <span data-ttu-id="d1670-127">Povolení spřažení relace, bude mít za následek zvýšení množství místa na disku zabírá nasazení v Azure, jako další software bude stažen a nainstalován do instance role při spuštění služby v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="d1670-127">Enabling session affinity will result in an increase in the amount of disk space taken up by your deployment in Azure, as additional software will be downloaded and installed into your role instances when your service is started in the Azure cloud.</span></span>

* <span data-ttu-id="d1670-128">Čas potřebný k inicializaci každou roli bude trvat déle.</span><span class="sxs-lookup"><span data-stu-id="d1670-128">The time to initialize each role will take longer.</span></span>

* <span data-ttu-id="d1670-129">Vnitřní koncový bod, aby fungoval jako rerouter provozu jak je uvedeno nahoře, bude přidána.</span><span class="sxs-lookup"><span data-stu-id="d1670-129">An internal endpoint, to function as a traffic rerouter as mentioned above, will be added.</span></span>


## <a name="see-also"></a><span data-ttu-id="d1670-130">Viz také</span><span class="sxs-lookup"><span data-stu-id="d1670-130">See Also</span></span>
<span data-ttu-id="d1670-131">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d1670-131">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="d1670-132">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d1670-132">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="d1670-133">[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d1670-133">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="d1670-134">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="d1670-134">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
