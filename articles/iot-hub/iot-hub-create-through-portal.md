---
title: "Vytvoření služby IoT Hub pomocí portálu Azure | Microsoft Docs"
description: "Postup vytvoření, správě a odstranění služby Azure IoT hubs prostřednictvím portálu Azure. Obsahuje informace o cenových úrovní, škálování, zabezpečení a zasílání zpráv konfigurace."
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
ms.openlocfilehash: bca7eea5f44bbed3b784b56edaac235161b43e5e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a><span data-ttu-id="27f2a-104">Vytvoření služby IoT hub pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="27f2a-104">Create an IoT hub using the Azure portal</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="27f2a-105">Tento článek popisuje:</span><span class="sxs-lookup"><span data-stu-id="27f2a-105">This article describes:</span></span>

* <span data-ttu-id="27f2a-106">Jak najít službu IoT Hub v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="27f2a-106">How to find the IoT Hub service in the Azure portal.</span></span>
* <span data-ttu-id="27f2a-107">Jak vytvořit a spravovat centra IoT.</span><span class="sxs-lookup"><span data-stu-id="27f2a-107">How to create and manage IoT hubs.</span></span>

## <a name="where-to-find-the-iot-hub-service"></a><span data-ttu-id="27f2a-108">Kde najít službu IoT Hub</span><span class="sxs-lookup"><span data-stu-id="27f2a-108">Where to find the IoT Hub service</span></span>

<span data-ttu-id="27f2a-109">Služba IoT Hub můžete najít v následujících umístěních na portálu:</span><span class="sxs-lookup"><span data-stu-id="27f2a-109">You can find the IoT Hub service in the following locations in the portal:</span></span>

* <span data-ttu-id="27f2a-110">Zvolte **+ nový**, zvolte **Internet věcí**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-110">Choose **+ New**, then choose **Internet of Things**.</span></span>
* <span data-ttu-id="27f2a-111">Na webu Marketplace, zvolte **Internet věcí**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-111">In the Marketplace, choose **Internet of Things**.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="27f2a-112">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="27f2a-112">Create an IoT hub</span></span>

<span data-ttu-id="27f2a-113">Můžete vytvořit služby IoT hub pomocí následujících metod:</span><span class="sxs-lookup"><span data-stu-id="27f2a-113">You can create an IoT hub using the following methods:</span></span>

* <span data-ttu-id="27f2a-114">**+ Nový** možnost otevře okno znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="27f2a-114">The **+ New** option opens the blade shown in the following screen shot.</span></span> <span data-ttu-id="27f2a-115">Kroky pro vytvoření služby IoT hub prostřednictvím této metody a přes marketplace jsou identické.</span><span class="sxs-lookup"><span data-stu-id="27f2a-115">The steps for creating the IoT hub through this method and through the marketplace are identical.</span></span>
* <span data-ttu-id="27f2a-116">Na webu Marketplace, zvolte **vytvořit** otevřete okno znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="27f2a-116">In the Marketplace, choose **Create** to open the blade shown in the following screen shot.</span></span>

<span data-ttu-id="27f2a-117">Následující části popisují několik kroků pro vytvoření služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="27f2a-117">The following sections describe the several steps to create an IoT hub:</span></span>

### <a name="choose-the-name-of-the-iot-hub"></a><span data-ttu-id="27f2a-118">Vyberte název centra IoT</span><span class="sxs-lookup"><span data-stu-id="27f2a-118">Choose the name of the IoT hub</span></span>

<span data-ttu-id="27f2a-119">Vytvoření služby IoT hub, název služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27f2a-119">To create an IoT hub, you must name the IoT hub.</span></span> <span data-ttu-id="27f2a-120">Tento název musí být jedinečný mezi všechny služby IoT hubs.</span><span class="sxs-lookup"><span data-stu-id="27f2a-120">This name must be unique across all IoT hubs.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a><span data-ttu-id="27f2a-121">Vyberte cenovou úroveň</span><span class="sxs-lookup"><span data-stu-id="27f2a-121">Choose the pricing tier</span></span>

<span data-ttu-id="27f2a-122">Můžete si vybrat z čtyři úrovně: **volné**, **standardní 1** a **standardní 2**, a **úrovně Standard S3**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-122">You can choose from four tiers: **Free**, **Standard 1** and **Standard 2**, and **Standard S3**.</span></span> <span data-ttu-id="27f2a-123">Úroveň free umožňuje pouze 500 zařízení k připojení ke službě IoT hub a maximálně 8 000 jednotek zpráv za den.</span><span class="sxs-lookup"><span data-stu-id="27f2a-123">The free tier allows only 500 devices to be connected to the IoT hub and up to 8,000 messages per day.</span></span>

<span data-ttu-id="27f2a-124">**Standard S1**: pomocí edice S1 pro řešení IoT velký počet zařízení, že každý generovat malé množství dat.</span><span class="sxs-lookup"><span data-stu-id="27f2a-124">**Standard S1**: Use the S1 edition for IoT solutions with a large number of devices that each generate small amounts of data.</span></span> <span data-ttu-id="27f2a-125">Každá jednotka edice S1 umožňuje přenášet v rámci všech připojených zařízení až 400 000 zpráv denně.</span><span class="sxs-lookup"><span data-stu-id="27f2a-125">Each unit of the S1 edition allows up to 400,000 messages per day across all connected devices.</span></span>

<span data-ttu-id="27f2a-126">**Standard S2**: pro řešení IoT, ve kterých zařízení generovat velké objemy dat použít edici S2.</span><span class="sxs-lookup"><span data-stu-id="27f2a-126">**Standard S2**: Use the S2 edition for IoT solutions in which devices generate large amounts of data.</span></span> <span data-ttu-id="27f2a-127">Jednotlivé jednotky S2 edice umožňuje až 6 milionu zpráv za den mezi všech připojených zařízeních.</span><span class="sxs-lookup"><span data-stu-id="27f2a-127">Each unit of the S2 edition allows up to 6 million messages per day between all connected devices.</span></span>

<span data-ttu-id="27f2a-128">**Úrovně Standard S3**: pro řešení IoT, které generují velké objemy dat použít edici S3.</span><span class="sxs-lookup"><span data-stu-id="27f2a-128">**Standard S3**: Use the S3 edition for IoT solutions that generate large amounts of data.</span></span> <span data-ttu-id="27f2a-129">Jednotlivé jednotky S3 edice umožňuje až 300 milionů zpráv za den mezi všech připojených zařízeních.</span><span class="sxs-lookup"><span data-stu-id="27f2a-129">Each unit of the S3 edition allows up to 300 million messages per day between all connected devices.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="27f2a-130">IoT Hub umožňuje jenom jednu bezplatnou rozbočovače za předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="27f2a-130">IoT Hub only allows one free hub per Azure subscription.</span></span>

### <a name="iot-hub-units"></a><span data-ttu-id="27f2a-131">Jednotky centra IoT hub</span><span class="sxs-lookup"><span data-stu-id="27f2a-131">IoT hub units</span></span>

<span data-ttu-id="27f2a-132">Počet zpráv povolených na jednotku za den závisí na vaše Centrum cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="27f2a-132">The number of messages allowed per unit per day depends on your hub's pricing tier.</span></span> <span data-ttu-id="27f2a-133">Například pokud chcete službu IoT hub pro podporu příchozího 700 000 zpráv, zvolte dvě jednotky vrstvy S1.</span><span class="sxs-lookup"><span data-stu-id="27f2a-133">For example, if you want the IoT hub to support ingress of 700,000 messages, you choose two S1 tier units.</span></span>

### <a name="device-to-cloud-partitions-and-resource-group"></a><span data-ttu-id="27f2a-134">Zařízení do cloudu oddíly a skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="27f2a-134">Device to cloud partitions and resource group</span></span>

<span data-ttu-id="27f2a-135">Můžete změnit počet oddílů pro Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="27f2a-135">You can change the number of partitions for an IoT hub.</span></span> <span data-ttu-id="27f2a-136">Výchozí počet oddílů je 4, můžete v rozevíracím seznamu jiné číslo.</span><span class="sxs-lookup"><span data-stu-id="27f2a-136">The default number of partitions is 4, you can choose a different number from the drop-down list.</span></span>

<span data-ttu-id="27f2a-137">Není nutné explicitně vytvořit skupinu prostředků prázdný.</span><span class="sxs-lookup"><span data-stu-id="27f2a-137">You do not need to explicitly create an empty resource group.</span></span> <span data-ttu-id="27f2a-138">Když vytvoříte prostředek, můžete zvolit buď vytvořit novou nebo použít existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="27f2a-138">When you create a resource, you can choose either to create a new, or use an existing resource group.</span></span>

![][5]

### <a name="choose-subscription"></a><span data-ttu-id="27f2a-139">Zvolte předplatné</span><span class="sxs-lookup"><span data-stu-id="27f2a-139">Choose subscription</span></span>

<span data-ttu-id="27f2a-140">Azure IoT Hub automaticky uvádí uživatelský účet je spojena se předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="27f2a-140">Azure IoT Hub automatically lists the Azure subscriptions the user account is linked to.</span></span> <span data-ttu-id="27f2a-141">Můžete přidružit IoT hub, která předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="27f2a-141">You can choose the Azure subscription to associate the IoT hub to.</span></span>

### <a name="choose-the-location"></a><span data-ttu-id="27f2a-142">Vyberte umístění</span><span class="sxs-lookup"><span data-stu-id="27f2a-142">Choose the location</span></span>

<span data-ttu-id="27f2a-143">Možnosti umístění obsahuje seznam oblastí, kde je k dispozici služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="27f2a-143">The location option provides a list of the regions where IoT Hub is available.</span></span>

### <a name="create-the-iot-hub"></a><span data-ttu-id="27f2a-144">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="27f2a-144">Create the IoT hub</span></span>

<span data-ttu-id="27f2a-145">Po dokončení všech předchozích kroků můžete vytvořit službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27f2a-145">When all previous steps are complete, you can create the IoT hub.</span></span> <span data-ttu-id="27f2a-146">Klikněte na tlačítko **vytvořit** spuštění procesu back-end pro vytvoření a nasazení služby IoT hub s možnostmi, které jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="27f2a-146">Click **Create** to start the back-end process to create and deploy the IoT hub with the options you chose.</span></span>

<span data-ttu-id="27f2a-147">To může trvat několik minut pro vytvoření služby IoT hub jako trvá dobu pro spuštění na serverech příslušné umístění nasazení back-end.</span><span class="sxs-lookup"><span data-stu-id="27f2a-147">It can take a few minutes to create the IoT hub as it takes time for the back-end deployment to run on the appropriate location servers.</span></span>

## <a name="change-the-settings-of-the-iot-hub"></a><span data-ttu-id="27f2a-148">Změňte nastavení centra IoT</span><span class="sxs-lookup"><span data-stu-id="27f2a-148">Change the settings of the IoT hub</span></span>

<span data-ttu-id="27f2a-149">Nastavení stávající služby IoT hub můžete změnit po vytvoření v okně služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="27f2a-149">You can change the settings of an existing IoT hub after it is created from the IoT Hub blade.</span></span>

![][8]

<span data-ttu-id="27f2a-150">**Sdílené zásady přístupu**: tyto zásady určují oprávnění pro zařízení a služeb pro připojení ke službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="27f2a-150">**Shared access policies**: These policies define the permissions for devices and services to connect to IoT Hub.</span></span> <span data-ttu-id="27f2a-151">Tyto zásady můžete přístupu kliknutím **zásady sdíleného přístupu** pod **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-151">You can access these policies by clicking **Shared access policies** under **General**.</span></span> <span data-ttu-id="27f2a-152">V tomto okně můžete upravit existující zásady nebo přidání nové zásady.</span><span class="sxs-lookup"><span data-stu-id="27f2a-152">In this blade, you can either modify existing policies or add a new policy.</span></span>

### <a name="create-a-policy"></a><span data-ttu-id="27f2a-153">Vytvořit zásadu</span><span class="sxs-lookup"><span data-stu-id="27f2a-153">Create a policy</span></span>

* <span data-ttu-id="27f2a-154">Klikněte na tlačítko **přidat** a otevřete okno.</span><span class="sxs-lookup"><span data-stu-id="27f2a-154">Click **Add** to open a blade.</span></span> <span data-ttu-id="27f2a-155">Zde můžete zadat nový název zásady a oprávnění, které chcete přidružit k této zásadě, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="27f2a-155">Here you can enter the new policy name and the permissions that you want to associate with this policy, as shown in the following figure:</span></span>

    <span data-ttu-id="27f2a-156">Jsou k dispozici několik oprávnění, které může být spojeno s tyto sdílené zásady.</span><span class="sxs-lookup"><span data-stu-id="27f2a-156">There are several permissions that can be associated with these shared policies.</span></span> <span data-ttu-id="27f2a-157">**Registru číst** a **registru zápisu** zásady udělit oprávnění ke čtení a zápis do registru identit.</span><span class="sxs-lookup"><span data-stu-id="27f2a-157">The **Registry read** and **Registry write** policies grant read and write access rights to the identity registry.</span></span> <span data-ttu-id="27f2a-158">Výběrem možnosti zápisu automaticky zvolí možnost pro čtení.</span><span class="sxs-lookup"><span data-stu-id="27f2a-158">Choosing the write option automatically chooses the read option.</span></span>

    <span data-ttu-id="27f2a-159">**Služba připojit** zásad uděluje oprávnění k přístupu koncové body služby, jako například **přijímat zařízení cloud**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-159">The **Service connect** policy grants permission to access service endpoints such as **Receive device-to-cloud**.</span></span> <span data-ttu-id="27f2a-160">**Zařízení připojit** zásad uděluje oprávnění pro odesílání a přijímání zpráv pomocí koncových bodů na straně zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="27f2a-160">The **Device connect** policy grants permissions for sending and receiving messages using the IoT Hub device-side endpoints.</span></span>

* <span data-ttu-id="27f2a-161">Klikněte na tlačítko **vytvořit** to nově vytvořený zásad do existujícího seznamu přidat.</span><span class="sxs-lookup"><span data-stu-id="27f2a-161">Click **Create** to add this newly created policy to the existing list.</span></span>

![][10]

## <a name="endpoints"></a><span data-ttu-id="27f2a-162">Koncové body</span><span class="sxs-lookup"><span data-stu-id="27f2a-162">Endpoints</span></span>

<span data-ttu-id="27f2a-163">Klikněte na tlačítko **koncové body** zobrazíte seznam koncových bodů pro službu IoT hub, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="27f2a-163">Click **Endpoints** to display a list of endpoints for the IoT hub that you are modifying.</span></span> <span data-ttu-id="27f2a-164">Existují dva typy koncových bodů: koncové body, které jsou součástí služby IoT hub a koncové body, které přidáte do služby IoT hub po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="27f2a-164">There are two types of endpoints: endpoints that are built into the IoT hub, and endpoints that you add to the IoT hub after its creation.</span></span>

![][11]

### <a name="built-in-endpoints"></a><span data-ttu-id="27f2a-165">Předdefinované koncové body</span><span class="sxs-lookup"><span data-stu-id="27f2a-165">Built-in endpoints</span></span>

<span data-ttu-id="27f2a-166">Existují dva předdefinované koncové body: **cloudu na zařízení připomínky** a **události**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-166">There are two built-in endpoints: **Cloud to device feedback** and **Events**.</span></span>

* <span data-ttu-id="27f2a-167">**Cloudu na zařízení připomínky** nastavení: Toto nastavení má dva subsettings: **cloudu do zařízení TTL** (time-to-live) a **dobu uchování** (v hodinách) pro zprávy.</span><span class="sxs-lookup"><span data-stu-id="27f2a-167">**Cloud to device feedback** settings: This setting has two subsettings: **Cloud to Device TTL** (time-to-live) and **Retention time** (in hours) for the messages.</span></span> <span data-ttu-id="27f2a-168">Při první vytvoření služby IoT hub, obě tato nastavení mají výchozí hodnotu 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="27f2a-168">When your first create an IoT hub, both these settings have the default value of one hour.</span></span> <span data-ttu-id="27f2a-169">Chcete-li tato nastavení upravit, pomocí posuvníků nebo zadejte hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27f2a-169">To adjust these settings, use the sliders or type the values.</span></span>
* <span data-ttu-id="27f2a-170">**Události** nastavení: Toto nastavení má několik subsettings, z nichž některé jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="27f2a-170">**Events** settings: This setting has several subsettings, some of which are read-only.</span></span> <span data-ttu-id="27f2a-171">Následující seznam popisuje tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="27f2a-171">The following list describes these settings:</span></span>

  * <span data-ttu-id="27f2a-172">**Oddíly**: výchozí hodnota je nastavena při vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27f2a-172">**Partitions**: A default value is set when the IoT hub is created.</span></span> <span data-ttu-id="27f2a-173">Počet oddílů pomocí tohoto nastavení můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="27f2a-173">You can change the number of partitions through this setting.</span></span>

  * <span data-ttu-id="27f2a-174">**Název kompatibilní s centrem událostí a koncového bodu**: centra událostí je vytvořili interně, že potřebujete přístup k za určitých okolností, vzniká, když IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27f2a-174">**Event Hub-compatible name and endpoint**: When the IoT hub is created, an Event Hub is created internally that you may need access to under certain circumstances.</span></span> <span data-ttu-id="27f2a-175">Nelze upravit hodnoty název a koncový bod kompatibilní s centrem událostí, ale můžete je zkopírovat kliknutím **kopie**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-175">You cannot customize the Event Hub-compatible name and endpoint values but you can copy them by clicking **Copy**.</span></span>

  * <span data-ttu-id="27f2a-176">**Doba uchování**: ve výchozím nastavení má jeden den, ale můžete změnit pomocí rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="27f2a-176">**Retention Time**: Set to one day by default but you can change it using the drop-down list.</span></span> <span data-ttu-id="27f2a-177">Tato hodnota je ve dnech pro nastavení zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="27f2a-177">This value is in days for the device-to-cloud setting.</span></span>

  * <span data-ttu-id="27f2a-178">**Skupiny příjemců**: skupiny příjemců povolit více uživatelům nezávisle číst zprávy z centra IoT.</span><span class="sxs-lookup"><span data-stu-id="27f2a-178">**Consumer Groups**: Consumer groups enable multiple readers to read messages independently from the IoT hub.</span></span> <span data-ttu-id="27f2a-179">Každé centrum IoT je vytvořen s výchozí skupina příjemců.</span><span class="sxs-lookup"><span data-stu-id="27f2a-179">Every IoT hub is created with a default consumer group.</span></span> <span data-ttu-id="27f2a-180">Můžete ale přidat nebo odstranit skupiny uživatelů do vašeho centra IoT použití tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="27f2a-180">However, you can add or delete consumer groups to your IoT hubs using this setting.</span></span>

  > [!NOTE]
  > <span data-ttu-id="27f2a-181">Výchozí skupina příjemců nelze upravit ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="27f2a-181">The default consumer group cannot be edited or deleted.</span></span>

### <a name="custom-endpoints"></a><span data-ttu-id="27f2a-182">Vlastní koncové body</span><span class="sxs-lookup"><span data-stu-id="27f2a-182">Custom endpoints</span></span>

<span data-ttu-id="27f2a-183">Ve službě IoT hub pomocí portálu můžete přidat vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="27f2a-183">You can add custom endpoints on your IoT hub using the portal.</span></span> <span data-ttu-id="27f2a-184">Z **koncové body** okně klikněte na tlačítko **přidat** v horní části otevřete **přidání koncového bodu** okno.</span><span class="sxs-lookup"><span data-stu-id="27f2a-184">From the **Endpoints** blade, click **Add** at the top to open the **Add endpoint** blade.</span></span> <span data-ttu-id="27f2a-185">Zadejte požadované informace a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-185">Enter the required information, then click **OK**.</span></span> <span data-ttu-id="27f2a-186">Svůj vlastní koncový bod je nyní obsažena v hlavní **koncové body** okno.</span><span class="sxs-lookup"><span data-stu-id="27f2a-186">Your custom endpoint is now listed in the main **Endpoints** blade.</span></span>

![][13]

<span data-ttu-id="27f2a-187">Další informace o vlastní koncové body v [odkaz – koncové body centra IoT][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="27f2a-187">You can read more about custom endpoints in [Reference - IoT hub endpoints][lnk-devguide-endpoints].</span></span>

## <a name="routes"></a><span data-ttu-id="27f2a-188">Trasy</span><span class="sxs-lookup"><span data-stu-id="27f2a-188">Routes</span></span>

<span data-ttu-id="27f2a-189">Klikněte na tlačítko **trasy** ke správě, jak IoT Hub odešle zprávu vaše zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="27f2a-189">Click **Routes** to manage how IoT Hub dispatches your device-to-cloud messages.</span></span>

![][14]

<span data-ttu-id="27f2a-190">Kliknutím můžete přidat trasy do služby IoT hub **přidat** v horní části **trasy*** okno, zadáte požadované informace a kliknutím na **OK**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-190">You can add routes to your IoT hub by clicking **Add** at the top of the **Routes*** blade, entering the required information, and clicking **OK**.</span></span> <span data-ttu-id="27f2a-191">Vaše trasy je pak uveden v hlavní **trasy** okno.</span><span class="sxs-lookup"><span data-stu-id="27f2a-191">Your route is then listed in the main **Routes** blade.</span></span> <span data-ttu-id="27f2a-192">Trasa můžete upravit kliknutím na seznam tras.</span><span class="sxs-lookup"><span data-stu-id="27f2a-192">You can edit a route by clicking it in the list of routes.</span></span> <span data-ttu-id="27f2a-193">Chcete-li povolit trasu, klikněte na něj v seznamu tras a nastavte **povoleno** přepnutím **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="27f2a-193">To enable a route, click it in the list of routes and set the **Enabled** toggle to **Off**.</span></span> <span data-ttu-id="27f2a-194">Chcete-li uložit změny, klikněte na tlačítko **OK** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="27f2a-194">To save the change, click **OK** at the bottom of the blade.</span></span>

![][15]

## <a name="pricing-and-scale"></a><span data-ttu-id="27f2a-195">Ceny a škálování</span><span class="sxs-lookup"><span data-stu-id="27f2a-195">Pricing and scale</span></span>

<span data-ttu-id="27f2a-196">Ceny stávající služby IoT hub lze změnit pomocí **cenová** nastavení s těmito výjimkami:</span><span class="sxs-lookup"><span data-stu-id="27f2a-196">The pricing of an existing IoT hub can be changed through the **Pricing** settings, with the following exceptions:</span></span>

* <span data-ttu-id="27f2a-197">V aktuální implementace služby IoT hub s bezplatnou SKU nelze změnit vrstvy do jednoho z placené SKU, nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="27f2a-197">In the current implementation, an IoT hub with a free SKU cannot change tiers to one of the paid SKUs, or vice versa.</span></span>
* <span data-ttu-id="27f2a-198">Může existovat pouze jedna úroveň free IoT hub v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="27f2a-198">There can only be one free tier IoT hub in the Azure subscription.</span></span>

![][12]

<span data-ttu-id="27f2a-199">Můžete přesunout z vyššího nižší úrovně jenom v případě, že počet zpráv odeslaných daný den překročili kvótu pro nižší úroveň.</span><span class="sxs-lookup"><span data-stu-id="27f2a-199">You can move from a higher to lower tier only when the number of messages sent that day do exceed the quota for the lower tier.</span></span> <span data-ttu-id="27f2a-200">Například pokud počet zpráv za den překračuje 400,000, pak vrstvy pro službu IoT hub lze změnit.</span><span class="sxs-lookup"><span data-stu-id="27f2a-200">For example, if the number of messages per day exceeds 400,000, then the tier for the IoT hub can be changed.</span></span> <span data-ttu-id="27f2a-201">Ale pokud změníte k vrstvě S1 pak službu IoT hub je omezen pro daný den.</span><span class="sxs-lookup"><span data-stu-id="27f2a-201">However, if you change to the S1 tier then the IoT hub is throttled for that day.</span></span>

## <a name="delete-the-iot-hub"></a><span data-ttu-id="27f2a-202">Odstranit službu IoT hub</span><span class="sxs-lookup"><span data-stu-id="27f2a-202">Delete the IoT hub</span></span>

<span data-ttu-id="27f2a-203">Můžete procházet ke službě IoT hub, který chcete odstranit kliknutím **Procházet**a pak vyberete příslušné rozbočovače k odstranění.</span><span class="sxs-lookup"><span data-stu-id="27f2a-203">You can browse to the IoT hub you want to delete by clicking **Browse**, and then choosing the appropriate hub to delete.</span></span> <span data-ttu-id="27f2a-204">Pokud chcete odstranit centrum IoT, klikněte na tlačítko **odstranit** tlačítko pod název centra IoT.</span><span class="sxs-lookup"><span data-stu-id="27f2a-204">To delete the IoT hub, click the **Delete** button below the IoT hub name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27f2a-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27f2a-205">Next steps</span></span>

<span data-ttu-id="27f2a-206">Další informace o správě Azure IoT Hub na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="27f2a-206">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="27f2a-207">[Hromadné spravovat zařízení IoT][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="27f2a-207">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="27f2a-208">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="27f2a-208">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="27f2a-209">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="27f2a-209">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="27f2a-210">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="27f2a-210">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="27f2a-211">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="27f2a-211">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="27f2a-212">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="27f2a-212">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="27f2a-213">[Zabezpečení řešení IoT od základů nahoru][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="27f2a-213">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

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
