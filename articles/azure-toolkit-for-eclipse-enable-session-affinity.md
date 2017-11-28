---
title: "použití spřažení relace aaaEnable hello nástrojů Azure pro Eclipse"
description: "Zjistěte, jak pomocí spřažení relace tooenable hello nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 523e728c58bda95e7af4b242e831694eb6d75cb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-session-affinity"></a><span data-ttu-id="c0786-103">Povolit spřažení relace</span><span class="sxs-lookup"><span data-stu-id="c0786-103">Enable Session Affinity</span></span>
<span data-ttu-id="c0786-104">V rámci hello nástrojů Azure pro prostředí Eclipse můžete povolit spřažení relace HTTP, nebo "trvalé relace", pro své role.</span><span class="sxs-lookup"><span data-stu-id="c0786-104">Within hello Azure Toolkit for Eclipse, you can enable HTTP session affinity, or "sticky sessions", for your roles.</span></span> <span data-ttu-id="c0786-105">Hello následující obrázek ukazuje hello **Vyrovnávání zatížení** funkce spřažení relace hello tooenable pro dialogové okno Vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c0786-105">hello following image shows hello **Load Balancing** properties dialog used tooenable hello session affinity feature:</span></span>

![][ic719492]

## <a name="tooenable-session-affinity-for-your-role"></a><span data-ttu-id="c0786-106">spřažení relace tooenable pro vaši roli</span><span class="sxs-lookup"><span data-stu-id="c0786-106">tooenable session affinity for your role</span></span>
1. <span data-ttu-id="c0786-107">Klikněte pravým tlačítkem na roli hello v prohlížeči projektu Eclipse společnosti, klikněte na tlačítko **Azure**a potom klikněte na **Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="c0786-107">Right-click hello role in Eclipse's Project Explorer, click **Azure**, and then click **Load Balancing**.</span></span>

2. <span data-ttu-id="c0786-108">V hello **vlastnosti pro vyrovnávání zatížení WorkerRole1** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="c0786-108">In hello **Properties for WorkerRole1 Load Balancing** dialog:</span></span>

   <span data-ttu-id="c0786-109">a.</span><span class="sxs-lookup"><span data-stu-id="c0786-109">a.</span></span> <span data-ttu-id="c0786-110">Zkontrolujte **spřažení relace povolit HTTP (trvalé relace) pro tuto roli.**</span><span class="sxs-lookup"><span data-stu-id="c0786-110">Check **Enable HTTP session affinity (sticky sessions) for this role.**</span></span>

   <span data-ttu-id="c0786-111">b.</span><span class="sxs-lookup"><span data-stu-id="c0786-111">b.</span></span> <span data-ttu-id="c0786-112">Pro **vstupní koncový bod toouse**, vyberte toouse vstupní koncový bod, například **http (veřejné: 80, privátní: 8080)**.</span><span class="sxs-lookup"><span data-stu-id="c0786-112">For **Input endpoint toouse**, select an input endpoint toouse, for example, **http (public:80, private:8080)**.</span></span> <span data-ttu-id="c0786-113">Aplikace musí používat tento koncový bod jako svůj koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0786-113">Your application must use this endpoint as its HTTP endpoint.</span></span> <span data-ttu-id="c0786-114">Víc koncových bodů můžete povolit pro vaši roli, ale pouze jeden z nich můžete vybrat toosupport trvalé relace.</span><span class="sxs-lookup"><span data-stu-id="c0786-114">You can enable multiple endpoints for your role, but you can select only one of them toosupport sticky sessions.</span></span>

   <span data-ttu-id="c0786-115">c.</span><span class="sxs-lookup"><span data-stu-id="c0786-115">c.</span></span> <span data-ttu-id="c0786-116">Sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0786-116">Rebuild your application.</span></span>

<span data-ttu-id="c0786-117">Jakmile bude povoleno, pokud máte více než jednu instanci role, požadavky HTTP z konkrétního klienta bude pokračovat zpracovanou hello stejné role instance.</span><span class="sxs-lookup"><span data-stu-id="c0786-117">Once enabled, if you have more than one role instance, HTTP requests coming from a particular client will continue being handled by hello same role instance.</span></span>

<span data-ttu-id="c0786-118">Hello nástrojů Eclipse umožňuje to nainstalováním speciální IIS modul s názvem směrování (žádostí na aplikace) do každé instance role.</span><span class="sxs-lookup"><span data-stu-id="c0786-118">hello Eclipse Toolkit enables this by installing a special IIS module called Application Request Routing (ARR) into each of your role instances.</span></span> <span data-ttu-id="c0786-119">Směrování žádostí na aplikace bude přesměrována instanci příslušné role toohello požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0786-119">ARR reroutes HTTP requests toohello appropriate role instance.</span></span> <span data-ttu-id="c0786-120">Hello toolkit automaticky změní konfiguraci koncového bodu hello vybrané tak, aby příchozí přenos HTTP hello je první software směrované toohello směrování žádostí na aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0786-120">hello toolkit automatically reconfigures hello selected endpoint so that hello incoming HTTP traffic is first routed toohello ARR software.</span></span> <span data-ttu-id="c0786-121">Sada nástrojů Hello také vytvoří nový vnitřní koncový bod, který je nakonfigurovaný toolisten na váš server Java.</span><span class="sxs-lookup"><span data-stu-id="c0786-121">hello toolkit also creates a new internal endpoint that your Java server is configured toolisten to.</span></span> <span data-ttu-id="c0786-122">To je koncový bod hello používá směrování žádostí na aplikace tooreroute hello HTTP provoz toohello příslušné role instance.</span><span class="sxs-lookup"><span data-stu-id="c0786-122">That is hello endpoint used by ARR tooreroute hello HTTP traffic toohello appropriate role instance.</span></span> <span data-ttu-id="c0786-123">Tímto způsobem, každá instance role ve vašem nasazení s více instancemi slouží jako reverzní proxy server pro všechny hello ostatní instance povolení trvalé relace.</span><span class="sxs-lookup"><span data-stu-id="c0786-123">This way, each role instance in your multi-instance deployment serves as a reverse proxy for all hello other instances, enabling sticky sessions.</span></span>

## <a name="notes-about-session-affinity"></a><span data-ttu-id="c0786-124">Poznámky o spřažení relace</span><span class="sxs-lookup"><span data-stu-id="c0786-124">Notes about session affinity</span></span>
* <span data-ttu-id="c0786-125">Spřažení relace v emulátoru služby výpočty hello nefunguje.</span><span class="sxs-lookup"><span data-stu-id="c0786-125">Session affinity does not work in hello compute emulator.</span></span> <span data-ttu-id="c0786-126">nastavení Hello lze použít v emulátoru služby výpočty hello bez zasahování vašeho procesu sestavení nebo výpočetní emulátor spuštění, ale hello samotné funkce nefunguje v emulátoru služby výpočty hello.</span><span class="sxs-lookup"><span data-stu-id="c0786-126">hello settings can be applied in hello compute emulator without interfering with your build process or compute emulator execution, but hello feature itself does not function within hello compute emulator.</span></span>

* <span data-ttu-id="c0786-127">Povolení spřažení relace, bude mít za následek zvýšení hello množství místa na disku zabírá nasazení v Azure, jako další software bude možné stáhnout a nainstalovat do instance role při spuštění služby v hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0786-127">Enabling session affinity will result in an increase in hello amount of disk space taken up by your deployment in Azure, as additional software will be downloaded and installed into your role instances when your service is started in hello Azure cloud.</span></span>

* <span data-ttu-id="c0786-128">Hello čas tooinitialize každou roli bude trvat déle.</span><span class="sxs-lookup"><span data-stu-id="c0786-128">hello time tooinitialize each role will take longer.</span></span>

* <span data-ttu-id="c0786-129">Vnitřní koncový bod, toofunction jako provoz rerouter, jak je uvedeno nahoře, bude přidána.</span><span class="sxs-lookup"><span data-stu-id="c0786-129">An internal endpoint, toofunction as a traffic rerouter as mentioned above, will be added.</span></span>


## <a name="see-also"></a><span data-ttu-id="c0786-130">Viz také</span><span class="sxs-lookup"><span data-stu-id="c0786-130">See Also</span></span>
<span data-ttu-id="c0786-131">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="c0786-131">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="c0786-132">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="c0786-132">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="c0786-133">[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="c0786-133">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="c0786-134">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="c0786-134">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How tooMaintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
