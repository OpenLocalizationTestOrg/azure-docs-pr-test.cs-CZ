---
title: "filtry připojení aaaAzure IoT Hub IP | Microsoft Docs"
description: "Jak toouse IP filtrování tooblock připojení z konkrétní IP adresy pro tooyour Azure IoT hub. Můžete blokovat připojení z jednotlivých nebo rozsahy IP adres."
services: iot-hub
documentationcenter: 
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: f833eac3-5b5f-46a7-a47b-f4f6fc927f3f
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: boltean
ms.openlocfilehash: 45e5906a494561b6108895d86d6a04fc3b52b8fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-ip-filters"></a><span data-ttu-id="182bb-104">Použití filtrů IP</span><span class="sxs-lookup"><span data-stu-id="182bb-104">Use IP filters</span></span>

<span data-ttu-id="182bb-105">Zabezpečení je důležitým aspektem jakékoli řešení IoT podle Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="182bb-105">Security is an important aspect of any IoT solution based on Azure IoT Hub.</span></span> <span data-ttu-id="182bb-106">Někdy můžete potřebovat tooexplicitly zadejte hello IP adresy, ze kterých se zařízení můžou připojit jako součást konfigurace zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="182bb-106">Sometimes you need tooexplicitly specify hello IP addresses from which devices can connect as part of your security configuration.</span></span> <span data-ttu-id="182bb-107">Hello _filtr IP_ funkce vám umožní tooconfigure pravidla odmítat nebo přijímat přenosy z konkrétní adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="182bb-107">hello _IP filter_ feature enables you tooconfigure rules for rejecting or accepting traffic from specific IPv4 addresses.</span></span>

## <a name="when-toouse"></a><span data-ttu-id="182bb-108">Když toouse</span><span class="sxs-lookup"><span data-stu-id="182bb-108">When toouse</span></span>

<span data-ttu-id="182bb-109">Existují dvě určité případy použití po koncové body centra IoT hello užitečné tooblock pro určité IP adresy:</span><span class="sxs-lookup"><span data-stu-id="182bb-109">There are two specific use-cases when it is useful tooblock hello IoT Hub endpoints for certain IP addresses:</span></span>

- <span data-ttu-id="182bb-110">Služby IoT hub by měl přijímat přenosy pouze ze zadaného rozsahu IP adres a všem ostatním odmítnout.</span><span class="sxs-lookup"><span data-stu-id="182bb-110">Your IoT hub should receive traffic only from a specified range of IP addresses and reject everything else.</span></span> <span data-ttu-id="182bb-111">Například používáte služby IoT hub s [Azure Express trasy] toocreate privátní připojení mezi služby IoT hub a v místní infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="182bb-111">For example, you are using your IoT hub with [Azure Express Route] toocreate private connections between an IoT hub and your on-premises infrastructure.</span></span>
- <span data-ttu-id="182bb-112">Je nutné tooreject provoz z IP adresy, které byly identifikovány jako podezřelou správcem hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="182bb-112">You need tooreject traffic from IP addresses that have been identified as suspicious by hello IoT hub administrator.</span></span>

## <a name="how-filter-rules-are-applied"></a><span data-ttu-id="182bb-113">Používání pravidel filtru</span><span class="sxs-lookup"><span data-stu-id="182bb-113">How filter rules are applied</span></span>

<span data-ttu-id="182bb-114">pravidla filtru IP Hello se používají v hello úrovně služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="182bb-114">hello IP filter rules are applied at hello IoT Hub service level.</span></span> <span data-ttu-id="182bb-115">Proto platí pravidla filtru IP hello tooall připojení ze zařízení a back-end aplikace používající libovolný protokol pro podporované.</span><span class="sxs-lookup"><span data-stu-id="182bb-115">Therefore hello IP filter rules apply tooall connections from devices and back-end apps using any supported protocol.</span></span>

<span data-ttu-id="182bb-116">Všechny pokus o připojení z IP adresy, který odpovídá pravidlu předávaní IP ve službě IoT hub přijímá neoprávněným stavový kód 401 a popis.</span><span class="sxs-lookup"><span data-stu-id="182bb-116">Any connection attempt from an IP address that matches a rejecting IP rule in your IoT hub receives an unauthorized 401 status code and description.</span></span> <span data-ttu-id="182bb-117">zpráva odpovědi Hello není zmiňuje hello IP pravidlo.</span><span class="sxs-lookup"><span data-stu-id="182bb-117">hello response message does not mention hello IP rule.</span></span>

## <a name="default-setting"></a><span data-ttu-id="182bb-118">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="182bb-118">Default setting</span></span>

<span data-ttu-id="182bb-119">Ve výchozím nastavení, hello **filtr IP** mřížky hello portálu služby IoT hub je prázdný.</span><span class="sxs-lookup"><span data-stu-id="182bb-119">By default, hello **IP Filter** grid in hello portal for an IoT hub is empty.</span></span> <span data-ttu-id="182bb-120">Toto výchozí nastavení znamená, že vaše Centrum přijímá připojení jakékoli IP adresy.</span><span class="sxs-lookup"><span data-stu-id="182bb-120">This default setting means that your hub accepts connections any IP address.</span></span> <span data-ttu-id="182bb-121">Toto výchozí nastavení je ekvivalentní tooa pravidlo, které přijímá rozsah IP adres hello 0.0.0.0/0.</span><span class="sxs-lookup"><span data-stu-id="182bb-121">This default setting is equivalent tooa rule that accepts hello 0.0.0.0/0 IP address range.</span></span>

![Nastavení filtru IP výchozí služby IoT Hub][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a><span data-ttu-id="182bb-123">Přidat nebo upravit pravidlo filtru IP</span><span class="sxs-lookup"><span data-stu-id="182bb-123">Add or edit an IP filter rule</span></span>

<span data-ttu-id="182bb-124">Když přidáte pravidlo filtru IP, budete vyzváni k hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="182bb-124">When you add an IP filter rule, you are prompted for hello following values:</span></span>

- <span data-ttu-id="182bb-125">**Název pravidla filtru IP** , musí být jedinečný, velká a malá písmena, alfanumerické řetězec až too128 znaků.</span><span class="sxs-lookup"><span data-stu-id="182bb-125">An **IP filter rule name** that must be a unique, case-insensitive, alphanumeric string up too128 characters long.</span></span> <span data-ttu-id="182bb-126">Pouze plus alfanumerických znaků ASCII 7bitového hello `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` jsou podmínky přijaty.</span><span class="sxs-lookup"><span data-stu-id="182bb-126">Only hello ASCII 7-bit alphanumeric characters plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` are accepted.</span></span>
- <span data-ttu-id="182bb-127">Vyberte **odmítnout** nebo **přijmout** jako hello **akce** pravidla filtru IP hello.</span><span class="sxs-lookup"><span data-stu-id="182bb-127">Select a **reject** or **accept** as hello **action** for hello IP filter rule.</span></span>
- <span data-ttu-id="182bb-128">Zadejte adresu samostatného protokolu IPv4, nebo blok IP adres v notaci CIDR.</span><span class="sxs-lookup"><span data-stu-id="182bb-128">Provide a single IPv4 address or a block of IP addresses in CIDR notation.</span></span> <span data-ttu-id="182bb-129">V CIDR zápis 192.168.100.0/22 představuje hello 1024 IPv4 adresy z 192.168.100.0 too192.168.103.255.</span><span class="sxs-lookup"><span data-stu-id="182bb-129">For example, in CIDR notation 192.168.100.0/22 represents hello 1024 IPv4 addresses from 192.168.100.0 too192.168.103.255.</span></span>

![Přidat Centrum IoT tooan pravidlo filtru IP][img-ip-filter-add-rule]

<span data-ttu-id="182bb-131">Po uložení hello pravidlo se zobrazí výstraha upozorněním, že je daná aktualizace hello v průběhu.</span><span class="sxs-lookup"><span data-stu-id="182bb-131">After you save hello rule, you see an alert notifying you that hello update is in progress.</span></span>

![Oznámení o ukládání pravidlo filtru IP][img-ip-filter-save-new-rule]

<span data-ttu-id="182bb-133">Hello **přidat** možnost je vypnuta, když se dostanete hello maximálně 10 pravidla filtru IP.</span><span class="sxs-lookup"><span data-stu-id="182bb-133">hello **Add** option is disabled when you reach hello maximum of 10 IP filter rules.</span></span>

<span data-ttu-id="182bb-134">Dvojitým kliknutím na soubor hello řádek, který obsahuje pravidlo hello můžete upravit existující pravidlo.</span><span class="sxs-lookup"><span data-stu-id="182bb-134">You can edit an existing rule by double-clicking hello row that contains hello rule.</span></span>

> [!NOTE]
> <span data-ttu-id="182bb-135">Odmítat IP adresy můžete zabránit jiné služby Azure (například Azure Stream Analytics, virtuální počítače Azure nebo hello zařízení Explorer hello portálu) v interakci s centrem IoT hello.</span><span class="sxs-lookup"><span data-stu-id="182bb-135">Rejecting IP addresses can prevent other Azure Services (such as Azure Stream Analytics, Azure Virtual Machines, or hello Device Explorer in hello portal) from interacting with hello IoT hub.</span></span>

> [!WARNING]
> <span data-ttu-id="182bb-136">Pokud používáte Azure Stream Analytics (ASA) tooread zpráv ze služby IoT hub s povoleným filtrováním IP, použijte název hello kompatibilní s centrem událostí a koncový bod služby IoT Hub v hello ASA připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="182bb-136">If you use Azure Stream Analytics (ASA) tooread messages from an IoT hub with IP filtering enabled, use hello Event Hub-compatible name and endpoint of your IoT Hub in hello ASA connection string.</span></span>

## <a name="delete-an-ip-filter-rule"></a><span data-ttu-id="182bb-137">Odstranění pravidla filtru IP</span><span class="sxs-lookup"><span data-stu-id="182bb-137">Delete an IP filter rule</span></span>

<span data-ttu-id="182bb-138">toodelete pravidlo filtru IP vyberte jeden nebo více pravidel v mřížce hello a klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="182bb-138">toodelete an IP filter rule, select one or more rules in hello grid and click **Delete**.</span></span>

![Odstranění pravidla filtru IP centra IoT][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a><span data-ttu-id="182bb-140">Vyhodnocení pravidla filtru IP</span><span class="sxs-lookup"><span data-stu-id="182bb-140">IP filter rule evaluation</span></span>

<span data-ttu-id="182bb-141">Pravidla filtru IP jsou použity v zadaném pořadí a že odpovídá hello IP adresa je rozhodující pro hello první pravidlo hello přijmout nebo odmítnout akce.</span><span class="sxs-lookup"><span data-stu-id="182bb-141">IP filter rules are applied in order and hello first rule that matches hello IP address determines hello accept or reject action.</span></span>

<span data-ttu-id="182bb-142">Například pokud chcete tooaccept adresy v rozsahu 192.168.100.0/22 hello a všem ostatním odmítnout, hello prvního pravidla v mřížce hello musí přijmout 192.168.100.0/22 rozsah adres hello.</span><span class="sxs-lookup"><span data-stu-id="182bb-142">For example, if you want tooaccept addresses in hello range 192.168.100.0/22 and reject everything else, hello first rule in hello grid should accept hello address range 192.168.100.0/22.</span></span> <span data-ttu-id="182bb-143">Další pravidla Hello by měl zamítnout všechny adresy pomocí 0.0.0.0/0 rozsah hello.</span><span class="sxs-lookup"><span data-stu-id="182bb-143">hello next rule should reject all addresses by using hello range 0.0.0.0/0.</span></span>

<span data-ttu-id="182bb-144">Můžete změnit pořadí hello pravidel filtru IP v mřížce hello klepnutím hello tři tečky vertikálně při spuštění hello řádku a pomocí přetažení a vyřadit.</span><span class="sxs-lookup"><span data-stu-id="182bb-144">You can change hello order of your IP filter rules in hello grid by clicking hello three vertical dots at hello start of a row and using drag and drop.</span></span>

<span data-ttu-id="182bb-145">filtrovat nových IP toosave pravidlo pořadí, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="182bb-145">toosave your new IP filter rule order, click **Save**.</span></span>

![Změňte pořadí hello pravidel filtru IP centra IoT][img-ip-filter-rule-order]

## <a name="next-steps"></a><span data-ttu-id="182bb-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="182bb-147">Next steps</span></span>

<span data-ttu-id="182bb-148">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="182bb-148">toofurther explore hello capabilities of IoT Hub, see:</span></span>

- <span data-ttu-id="182bb-149">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="182bb-149">[Operations monitoring][lnk-monitor]</span></span>
- <span data-ttu-id="182bb-150">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="182bb-150">[IoT Hub metrics][lnk-metrics]</span></span>

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express trasy]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md