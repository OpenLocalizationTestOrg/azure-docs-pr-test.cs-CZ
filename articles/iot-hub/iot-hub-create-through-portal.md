---
title: "aaaUse hello Azure portálu toocreate služby IoT Hub | Microsoft Docs"
description: "Toocreate, jak spravovat a odstranit Azure IoT hubs prostřednictvím hello portálu Azure. Obsahuje informace o cenových úrovní, škálování, zabezpečení a zasílání zpráv konfigurace."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: 383968c90ee7ef3bff85a6c90efbf5f0e8fbb208
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-portal"></a><span data-ttu-id="dee39-104">Vytvoření služby IoT hub pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dee39-104">Create an IoT hub using hello Azure portal</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="dee39-105">Tento článek popisuje:</span><span class="sxs-lookup"><span data-stu-id="dee39-105">This article describes:</span></span>

* <span data-ttu-id="dee39-106">Jak toofind hello služby IoT Hub v portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-106">How toofind hello IoT Hub service in hello Azure portal.</span></span>
* <span data-ttu-id="dee39-107">Jak toocreate a spravovat centra IoT.</span><span class="sxs-lookup"><span data-stu-id="dee39-107">How toocreate and manage IoT hubs.</span></span>

## <a name="where-toofind-hello-iot-hub-service"></a><span data-ttu-id="dee39-108">Kde toofind hello služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="dee39-108">Where toofind hello IoT Hub service</span></span>

<span data-ttu-id="dee39-109">Hello služby IoT Hub naleznete v následujících umístění portálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="dee39-109">You can find hello IoT Hub service in hello following locations in hello portal:</span></span>

* <span data-ttu-id="dee39-110">Zvolte **+ nový**, zvolte **Internet věcí**.</span><span class="sxs-lookup"><span data-stu-id="dee39-110">Choose **+ New**, then choose **Internet of Things**.</span></span>
* <span data-ttu-id="dee39-111">V hello Marketplace, zvolte **Internet věcí**.</span><span class="sxs-lookup"><span data-stu-id="dee39-111">In hello Marketplace, choose **Internet of Things**.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="dee39-112">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="dee39-112">Create an IoT hub</span></span>

<span data-ttu-id="dee39-113">Můžete vytvořit služby IoT hub pomocí hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="dee39-113">You can create an IoT hub using hello following methods:</span></span>

* <span data-ttu-id="dee39-114">Hello **+ nový** možnost otevře okno hello ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-114">hello **+ New** option opens hello blade shown in hello following screen shot.</span></span> <span data-ttu-id="dee39-115">Hello kroky pro vytvoření služby IoT hub hello prostřednictvím této metody a prostřednictvím hello marketplace jsou identické.</span><span class="sxs-lookup"><span data-stu-id="dee39-115">hello steps for creating hello IoT hub through this method and through hello marketplace are identical.</span></span>
* <span data-ttu-id="dee39-116">V hello Marketplace, zvolte **vytvořit** okno hello tooopen ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-116">In hello Marketplace, choose **Create** tooopen hello blade shown in hello following screen shot.</span></span>

<span data-ttu-id="dee39-117">Hello následující části popisují hello několik kroků toocreate služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="dee39-117">hello following sections describe hello several steps toocreate an IoT hub:</span></span>

### <a name="choose-hello-name-of-hello-iot-hub"></a><span data-ttu-id="dee39-118">Zvolte název hello hello centra IoT</span><span class="sxs-lookup"><span data-stu-id="dee39-118">Choose hello name of hello IoT hub</span></span>

<span data-ttu-id="dee39-119">toocreate služby IoT hub, musí název hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dee39-119">toocreate an IoT hub, you must name hello IoT hub.</span></span> <span data-ttu-id="dee39-120">Tento název musí být jedinečný mezi všechny služby IoT hubs.</span><span class="sxs-lookup"><span data-stu-id="dee39-120">This name must be unique across all IoT hubs.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-hello-pricing-tier"></a><span data-ttu-id="dee39-121">Vyberte cenovou úroveň hello</span><span class="sxs-lookup"><span data-stu-id="dee39-121">Choose hello pricing tier</span></span>

<span data-ttu-id="dee39-122">Můžete si vybrat z čtyři úrovně: **volné**, **standardní 1** a **standardní 2**, a **úrovně Standard S3**.</span><span class="sxs-lookup"><span data-stu-id="dee39-122">You can choose from four tiers: **Free**, **Standard 1** and **Standard 2**, and **Standard S3**.</span></span> <span data-ttu-id="dee39-123">úroveň free Hello umožňuje pouze 500 toobe zařízení připojené toohello IoT hub a až too8 000 zpráv za den.</span><span class="sxs-lookup"><span data-stu-id="dee39-123">hello free tier allows only 500 devices toobe connected toohello IoT hub and up too8,000 messages per day.</span></span>

<span data-ttu-id="dee39-124">**Standard S1**: použití hello S1 edition pro počítače s řešeními IoT s velký počet zařízení, že každý generovat malé množství dat.</span><span class="sxs-lookup"><span data-stu-id="dee39-124">**Standard S1**: Use hello S1 edition for IoT solutions with a large number of devices that each generate small amounts of data.</span></span> <span data-ttu-id="dee39-125">Jednotlivé jednotky hello S1 edition umožňuje až too400 000 zpráv za den ve všech připojených zařízeních.</span><span class="sxs-lookup"><span data-stu-id="dee39-125">Each unit of hello S1 edition allows up too400,000 messages per day across all connected devices.</span></span>

<span data-ttu-id="dee39-126">**Standard S2**: použití hello S2 edition pro počítače s řešení IoT, ve kterých zařízení generovat velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="dee39-126">**Standard S2**: Use hello S2 edition for IoT solutions in which devices generate large amounts of data.</span></span> <span data-ttu-id="dee39-127">Jednotlivé jednotky hello S2 edition umožňuje až too6 mil. zpráv za den mezi všech připojených zařízeních.</span><span class="sxs-lookup"><span data-stu-id="dee39-127">Each unit of hello S2 edition allows up too6 million messages per day between all connected devices.</span></span>

<span data-ttu-id="dee39-128">**Úrovně Standard S3**: použití hello S3 edition pro počítače s řešení IoT, které generují velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="dee39-128">**Standard S3**: Use hello S3 edition for IoT solutions that generate large amounts of data.</span></span> <span data-ttu-id="dee39-129">Jednotlivé jednotky hello S3 edition umožňuje až too300 mil. zpráv za den mezi všech připojených zařízeních.</span><span class="sxs-lookup"><span data-stu-id="dee39-129">Each unit of hello S3 edition allows up too300 million messages per day between all connected devices.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="dee39-130">IoT Hub umožňuje jenom jednu bezplatnou rozbočovače za předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="dee39-130">IoT Hub only allows one free hub per Azure subscription.</span></span>

### <a name="iot-hub-units"></a><span data-ttu-id="dee39-131">Jednotky centra IoT hub</span><span class="sxs-lookup"><span data-stu-id="dee39-131">IoT hub units</span></span>

<span data-ttu-id="dee39-132">Hello počet zpráv povolených na jednotku za den závisí na vaše Centrum cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="dee39-132">hello number of messages allowed per unit per day depends on your hub's pricing tier.</span></span> <span data-ttu-id="dee39-133">Například pokud chcete hello IoT hub toosupport příchozího 700 000 zpráv, zvolte dvě jednotky vrstvy S1.</span><span class="sxs-lookup"><span data-stu-id="dee39-133">For example, if you want hello IoT hub toosupport ingress of 700,000 messages, you choose two S1 tier units.</span></span>

### <a name="device-toocloud-partitions-and-resource-group"></a><span data-ttu-id="dee39-134">Oddíly toocloud zařízení a skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="dee39-134">Device toocloud partitions and resource group</span></span>

<span data-ttu-id="dee39-135">Můžete změnit hello počet oddílů pro Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="dee39-135">You can change hello number of partitions for an IoT hub.</span></span> <span data-ttu-id="dee39-136">Hello výchozí počet oddílů je 4, můžete vybrat jiné číslo z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-136">hello default number of partitions is 4, you can choose a different number from hello drop-down list.</span></span>

<span data-ttu-id="dee39-137">Není nutné tooexplicitly vytvořte skupinu prostředků prázdný.</span><span class="sxs-lookup"><span data-stu-id="dee39-137">You do not need tooexplicitly create an empty resource group.</span></span> <span data-ttu-id="dee39-138">Když vytvoříte prostředek, můžete zvolit buď toocreate a nové nebo použít existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="dee39-138">When you create a resource, you can choose either toocreate a new, or use an existing resource group.</span></span>

![][5]

### <a name="choose-subscription"></a><span data-ttu-id="dee39-139">Zvolte předplatné</span><span class="sxs-lookup"><span data-stu-id="dee39-139">Choose subscription</span></span>

<span data-ttu-id="dee39-140">Azure IoT Hub automaticky seznamy hello předplatná Azure hello uživatelský účet je přiřazen.</span><span class="sxs-lookup"><span data-stu-id="dee39-140">Azure IoT Hub automatically lists hello Azure subscriptions hello user account is linked to.</span></span> <span data-ttu-id="dee39-141">Můžete použít Centrum IoT hello předplatného Azure tooassociate hello k.</span><span class="sxs-lookup"><span data-stu-id="dee39-141">You can choose hello Azure subscription tooassociate hello IoT hub to.</span></span>

### <a name="choose-hello-location"></a><span data-ttu-id="dee39-142">Vyberte umístění hello</span><span class="sxs-lookup"><span data-stu-id="dee39-142">Choose hello location</span></span>

<span data-ttu-id="dee39-143">možnosti umístění Hello poskytuje seznam hello oblastí, kde je k dispozici služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dee39-143">hello location option provides a list of hello regions where IoT Hub is available.</span></span>

### <a name="create-hello-iot-hub"></a><span data-ttu-id="dee39-144">Vytvoření centra IoT hello</span><span class="sxs-lookup"><span data-stu-id="dee39-144">Create hello IoT hub</span></span>

<span data-ttu-id="dee39-145">Po dokončení všech předchozích kroků můžete vytvořit hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dee39-145">When all previous steps are complete, you can create hello IoT hub.</span></span> <span data-ttu-id="dee39-146">Klikněte na tlačítko **vytvořit** toostart hello toocreate proces back-end a nasadit hello IoT hub pro hello možnosti, které jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="dee39-146">Click **Create** toostart hello back-end process toocreate and deploy hello IoT hub with hello options you chose.</span></span>

<span data-ttu-id="dee39-147">Může trvat několik minut toocreate hello IoT hub jako trvá dobu hello back-end nasazení toorun na serverech příslušné umístění hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-147">It can take a few minutes toocreate hello IoT hub as it takes time for hello back-end deployment toorun on hello appropriate location servers.</span></span>

## <a name="change-hello-settings-of-hello-iot-hub"></a><span data-ttu-id="dee39-148">Změňte nastavení hello hello IoT hub</span><span class="sxs-lookup"><span data-stu-id="dee39-148">Change hello settings of hello IoT hub</span></span>

<span data-ttu-id="dee39-149">Po vytvoření z hello okno centra IoT, můžete změnit nastavení hello stávající služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dee39-149">You can change hello settings of an existing IoT hub after it is created from hello IoT Hub blade.</span></span>

![][8]

<span data-ttu-id="dee39-150">**Sdílené zásady přístupu**: tyto zásady určují hello oprávnění pro zařízení a služeb tooconnect tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="dee39-150">**Shared access policies**: These policies define hello permissions for devices and services tooconnect tooIoT Hub.</span></span> <span data-ttu-id="dee39-151">Tyto zásady můžete přístupu kliknutím **zásady sdíleného přístupu** pod **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="dee39-151">You can access these policies by clicking **Shared access policies** under **General**.</span></span> <span data-ttu-id="dee39-152">V tomto okně můžete upravit existující zásady nebo přidání nové zásady.</span><span class="sxs-lookup"><span data-stu-id="dee39-152">In this blade, you can either modify existing policies or add a new policy.</span></span>

### <a name="create-a-policy"></a><span data-ttu-id="dee39-153">Vytvořit zásadu</span><span class="sxs-lookup"><span data-stu-id="dee39-153">Create a policy</span></span>

* <span data-ttu-id="dee39-154">Klikněte na tlačítko **přidat** tooopen okno.</span><span class="sxs-lookup"><span data-stu-id="dee39-154">Click **Add** tooopen a blade.</span></span> <span data-ttu-id="dee39-155">Zadejte název nové zásady hello a hello oprávnění, která má tooassociate s těmito zásadami, jak je znázorněno v následující hello obrázek:</span><span class="sxs-lookup"><span data-stu-id="dee39-155">Here you can enter hello new policy name and hello permissions that you want tooassociate with this policy, as shown in hello following figure:</span></span>

    <span data-ttu-id="dee39-156">Jsou k dispozici několik oprávnění, které může být spojeno s tyto sdílené zásady.</span><span class="sxs-lookup"><span data-stu-id="dee39-156">There are several permissions that can be associated with these shared policies.</span></span> <span data-ttu-id="dee39-157">Hello **registru číst** a **registru zápisu** zásady udělit ke čtení a registru identit toohello práva k zápisu.</span><span class="sxs-lookup"><span data-stu-id="dee39-157">hello **Registry read** and **Registry write** policies grant read and write access rights toohello identity registry.</span></span> <span data-ttu-id="dee39-158">Výběrem možnosti zápisu hello automaticky zvolí možnost čtení hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-158">Choosing hello write option automatically chooses hello read option.</span></span>

    <span data-ttu-id="dee39-159">Hello **služba připojit** zásad uděluje oprávnění tooaccess koncové body služby, jako **přijímat zařízení cloud**.</span><span class="sxs-lookup"><span data-stu-id="dee39-159">hello **Service connect** policy grants permission tooaccess service endpoints such as **Receive device-to-cloud**.</span></span> <span data-ttu-id="dee39-160">Hello **zařízení připojit** zásad uděluje oprávnění pro odesílání a přijímání zpráv pomocí koncových bodů hello straně zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dee39-160">hello **Device connect** policy grants permissions for sending and receiving messages using hello IoT Hub device-side endpoints.</span></span>

* <span data-ttu-id="dee39-161">Klikněte na tlačítko **vytvořit** tooadd tomto nově vytvořený zásad toohello existujícího seznamu.</span><span class="sxs-lookup"><span data-stu-id="dee39-161">Click **Create** tooadd this newly created policy toohello existing list.</span></span>

![][10]

## <a name="endpoints"></a><span data-ttu-id="dee39-162">Koncové body</span><span class="sxs-lookup"><span data-stu-id="dee39-162">Endpoints</span></span>

<span data-ttu-id="dee39-163">Klikněte na tlačítko **koncové body** toodisplay seznam koncových bodů pro hello IoT hub, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="dee39-163">Click **Endpoints** toodisplay a list of endpoints for hello IoT hub that you are modifying.</span></span> <span data-ttu-id="dee39-164">Existují dva typy koncových bodů: koncové body, které jsou součástí hello IoT hub a koncové body přidejte toohello IoT hub po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="dee39-164">There are two types of endpoints: endpoints that are built into hello IoT hub, and endpoints that you add toohello IoT hub after its creation.</span></span>

![][11]

### <a name="built-in-endpoints"></a><span data-ttu-id="dee39-165">Předdefinované koncové body</span><span class="sxs-lookup"><span data-stu-id="dee39-165">Built-in endpoints</span></span>

<span data-ttu-id="dee39-166">Existují dva předdefinované koncové body: **cloudu zpětné vazby toodevice** a **události**.</span><span class="sxs-lookup"><span data-stu-id="dee39-166">There are two built-in endpoints: **Cloud toodevice feedback** and **Events**.</span></span>

* <span data-ttu-id="dee39-167">**Cloud zpětné vazby toodevice** nastavení: Toto nastavení má dva subsettings: **cloudu tooDevice TTL** (time-to-live) a **dobu uchování** (v hodinách) pro zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-167">**Cloud toodevice feedback** settings: This setting has two subsettings: **Cloud tooDevice TTL** (time-to-live) and **Retention time** (in hours) for hello messages.</span></span> <span data-ttu-id="dee39-168">Při první vytvoření služby IoT hub, obě tato nastavení mají hello výchozí hodnotu 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="dee39-168">When your first create an IoT hub, both these settings have hello default value of one hour.</span></span> <span data-ttu-id="dee39-169">tooadjust tato nastavení použít posuvníky hello nebo zadat hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-169">tooadjust these settings, use hello sliders or type hello values.</span></span>
* <span data-ttu-id="dee39-170">**Události** nastavení: Toto nastavení má několik subsettings, z nichž některé jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="dee39-170">**Events** settings: This setting has several subsettings, some of which are read-only.</span></span> <span data-ttu-id="dee39-171">Hello následující seznam popisuje tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="dee39-171">hello following list describes these settings:</span></span>

  * <span data-ttu-id="dee39-172">**Oddíly**: výchozí hodnota je nastavena, když je vytvořen hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dee39-172">**Partitions**: A default value is set when hello IoT hub is created.</span></span> <span data-ttu-id="dee39-173">Hello počet oddílů pomocí tohoto nastavení můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="dee39-173">You can change hello number of partitions through this setting.</span></span>

  * <span data-ttu-id="dee39-174">**Název kompatibilní s centrem událostí a koncového bodu**: Po vytvoření služby IoT hub hello centra událostí se může potřebovat přistupovat toounder určitých okolností interně vytvoří.</span><span class="sxs-lookup"><span data-stu-id="dee39-174">**Event Hub-compatible name and endpoint**: When hello IoT hub is created, an Event Hub is created internally that you may need access toounder certain circumstances.</span></span> <span data-ttu-id="dee39-175">Nelze přizpůsobit název a koncový bod hodnoty hello kompatibilní s centrem událostí, ale můžete je zkopírovat kliknutím **kopie**.</span><span class="sxs-lookup"><span data-stu-id="dee39-175">You cannot customize hello Event Hub-compatible name and endpoint values but you can copy them by clicking **Copy**.</span></span>

  * <span data-ttu-id="dee39-176">**Doba uchování**: ve výchozím nastavení tooone den, ale můžete změnit pomocí rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-176">**Retention Time**: Set tooone day by default but you can change it using hello drop-down list.</span></span> <span data-ttu-id="dee39-177">Tato hodnota je ve dnech pro nastavení zařízení cloud hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-177">This value is in days for hello device-to-cloud setting.</span></span>

  * <span data-ttu-id="dee39-178">**Skupiny příjemců**: skupiny příjemců povolit více zpráv tooread čtečky nezávisle hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dee39-178">**Consumer Groups**: Consumer groups enable multiple readers tooread messages independently from hello IoT hub.</span></span> <span data-ttu-id="dee39-179">Každé centrum IoT je vytvořen s výchozí skupina příjemců.</span><span class="sxs-lookup"><span data-stu-id="dee39-179">Every IoT hub is created with a default consumer group.</span></span> <span data-ttu-id="dee39-180">Můžete ale přidat nebo odstranit příjemce skupiny tooyour IoT hubs pomocí tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="dee39-180">However, you can add or delete consumer groups tooyour IoT hubs using this setting.</span></span>

  > [!NOTE]
  > <span data-ttu-id="dee39-181">Výchozí skupina příjemců Hello nelze upravit ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="dee39-181">hello default consumer group cannot be edited or deleted.</span></span>

### <a name="custom-endpoints"></a><span data-ttu-id="dee39-182">Vlastní koncové body</span><span class="sxs-lookup"><span data-stu-id="dee39-182">Custom endpoints</span></span>

<span data-ttu-id="dee39-183">Ve službě IoT hub pomocí portálu hello můžete přidat vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="dee39-183">You can add custom endpoints on your IoT hub using hello portal.</span></span> <span data-ttu-id="dee39-184">Z hello **koncové body** okně klikněte na tlačítko **přidat** v hello nejvyšší tooopen hello **přidání koncového bodu** okno.</span><span class="sxs-lookup"><span data-stu-id="dee39-184">From hello **Endpoints** blade, click **Add** at hello top tooopen hello **Add endpoint** blade.</span></span> <span data-ttu-id="dee39-185">Zadejte hello požadované informace a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="dee39-185">Enter hello required information, then click **OK**.</span></span> <span data-ttu-id="dee39-186">Svůj vlastní koncový bod je nyní obsažena v hello hlavní **koncové body** okno.</span><span class="sxs-lookup"><span data-stu-id="dee39-186">Your custom endpoint is now listed in hello main **Endpoints** blade.</span></span>

![][13]

<span data-ttu-id="dee39-187">Další informace o vlastní koncové body v [odkaz – koncové body centra IoT][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="dee39-187">You can read more about custom endpoints in [Reference - IoT hub endpoints][lnk-devguide-endpoints].</span></span>

## <a name="routes"></a><span data-ttu-id="dee39-188">Trasy</span><span class="sxs-lookup"><span data-stu-id="dee39-188">Routes</span></span>

<span data-ttu-id="dee39-189">Klikněte na tlačítko **trasy** toomanage jak IoT Hub odešle zprávu vaše zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="dee39-189">Click **Routes** toomanage how IoT Hub dispatches your device-to-cloud messages.</span></span>

![][14]

<span data-ttu-id="dee39-190">Kliknutím můžete přidat trasy tooyour IoT hub **přidat** hello horní části hello **trasy*** okně zadáním hello požadované informace a kliknutím na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="dee39-190">You can add routes tooyour IoT hub by clicking **Add** at hello top of hello **Routes*** blade, entering hello required information, and clicking **OK**.</span></span> <span data-ttu-id="dee39-191">Vaše trasy, pak je uvedena ve hello hlavní **trasy** okno.</span><span class="sxs-lookup"><span data-stu-id="dee39-191">Your route is then listed in hello main **Routes** blade.</span></span> <span data-ttu-id="dee39-192">Trasa můžete upravit kliknutím na hello seznam tras.</span><span class="sxs-lookup"><span data-stu-id="dee39-192">You can edit a route by clicking it in hello list of routes.</span></span> <span data-ttu-id="dee39-193">tooenable trasu, klikněte na něj v seznamu hello tras a nastavit hello **povoleno** přepnutí příliš**vypnout**.</span><span class="sxs-lookup"><span data-stu-id="dee39-193">tooenable a route, click it in hello list of routes and set hello **Enabled** toggle too**Off**.</span></span> <span data-ttu-id="dee39-194">Změna hello toosave, klikněte na tlačítko **OK** v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-194">toosave hello change, click **OK** at hello bottom of hello blade.</span></span>

![][15]

## <a name="pricing-and-scale"></a><span data-ttu-id="dee39-195">Ceny a škálování</span><span class="sxs-lookup"><span data-stu-id="dee39-195">Pricing and scale</span></span>

<span data-ttu-id="dee39-196">Hello ceny z stávající služby IoT hub lze změnit pomocí hello **cenová** nastavení s hello následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="dee39-196">hello pricing of an existing IoT hub can be changed through hello **Pricing** settings, with hello following exceptions:</span></span>

* <span data-ttu-id="dee39-197">V aktuální implementace hello, nelze změnit centrum IoT se volné SKU tooone vrstev z hello placené SKU, nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="dee39-197">In hello current implementation, an IoT hub with a free SKU cannot change tiers tooone of hello paid SKUs, or vice versa.</span></span>
* <span data-ttu-id="dee39-198">Může existovat pouze jedna úroveň free IoT hub v hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="dee39-198">There can only be one free tier IoT hub in hello Azure subscription.</span></span>

![][12]

<span data-ttu-id="dee39-199">Můžete přesunout z vyšší úroveň toolower jenom v případě, že hello počet zpráv odeslaných daný den překročení hello kvóty pro nižší úroveň hello.</span><span class="sxs-lookup"><span data-stu-id="dee39-199">You can move from a higher toolower tier only when hello number of messages sent that day do exceed hello quota for hello lower tier.</span></span> <span data-ttu-id="dee39-200">Například pokud hello počet zpráv za den překračuje 400,000, pak hello vrstva pro hello IoT hub lze změnit.</span><span class="sxs-lookup"><span data-stu-id="dee39-200">For example, if hello number of messages per day exceeds 400,000, then hello tier for hello IoT hub can be changed.</span></span> <span data-ttu-id="dee39-201">Ale pokud změníte úroveň S1 toohello pak hello IoT hub je omezen pro daný den.</span><span class="sxs-lookup"><span data-stu-id="dee39-201">However, if you change toohello S1 tier then hello IoT hub is throttled for that day.</span></span>

## <a name="delete-hello-iot-hub"></a><span data-ttu-id="dee39-202">Odstranit centrum IoT hello</span><span class="sxs-lookup"><span data-stu-id="dee39-202">Delete hello IoT hub</span></span>

<span data-ttu-id="dee39-203">Můžete procházet toohello IoT hub chcete toodelete kliknutím **Procházet**, a pak vyberete hello toodelete odpovídající rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="dee39-203">You can browse toohello IoT hub you want toodelete by clicking **Browse**, and then choosing hello appropriate hub toodelete.</span></span> <span data-ttu-id="dee39-204">toodelete hello služby IoT hub, klikněte na tlačítko hello **odstranit** tlačítko pod hello název centra IoT.</span><span class="sxs-lookup"><span data-stu-id="dee39-204">toodelete hello IoT hub, click hello **Delete** button below hello IoT hub name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dee39-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dee39-205">Next steps</span></span>

<span data-ttu-id="dee39-206">Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="dee39-206">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="dee39-207">[Hromadné spravovat zařízení IoT][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="dee39-207">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="dee39-208">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="dee39-208">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="dee39-209">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="dee39-209">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="dee39-210">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="dee39-210">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="dee39-211">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="dee39-211">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="dee39-212">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="dee39-212">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="dee39-213">[Zabezpečení řešení IoT z hello pozadí][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="dee39-213">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
