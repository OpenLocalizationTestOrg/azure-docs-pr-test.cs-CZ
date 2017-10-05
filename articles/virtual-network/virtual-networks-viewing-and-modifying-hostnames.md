---
title: "Zobrazení a úprava názvy hostitelů | Microsoft Docs"
description: "Jak zobrazit a změnit názvy hostitelů pro virtuální počítače Azure, webové a rolí pracovního procesu pro překlad"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 9a3a1e1b58dcb828e2d2d09c18f1aab6d46051aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="viewing-and-modifying-hostnames"></a><span data-ttu-id="3afcc-103">Zobrazení a úprava názvy hostitelů</span><span class="sxs-lookup"><span data-stu-id="3afcc-103">Viewing and modifying hostnames</span></span>
<span data-ttu-id="3afcc-104">Pokud chcete povolit instance role bude odkazovat podle názvu hostitele, je nutné nastavit hodnotu pro název hostitele v souboru konfigurace služby pro každou roli.</span><span class="sxs-lookup"><span data-stu-id="3afcc-104">To allow your role instances to be referenced by host name, you must set the value for the host name in the service configuration file for each role.</span></span> <span data-ttu-id="3afcc-105">To uděláte tak, že přidáte název požadovaného hostitele má **vmName** atribut **Role** elementu.</span><span class="sxs-lookup"><span data-stu-id="3afcc-105">You do that by adding the desired host name to the **vmName** attribute of the **Role** element.</span></span> <span data-ttu-id="3afcc-106">Hodnota **vmName** atribut slouží jako základ pro název hostitele každá instance role.</span><span class="sxs-lookup"><span data-stu-id="3afcc-106">The value of the **vmName** attribute is used as a base for the host name of each role instance.</span></span> <span data-ttu-id="3afcc-107">Například pokud **vmName** je *webrole* a jsou tři instance dané role, bude názvy hostitelů instancí *webrole0*, *webrole1*, a *webrole2*.</span><span class="sxs-lookup"><span data-stu-id="3afcc-107">For example, if **vmName** is *webrole* and there are three instances of that role, the host names of the instances will be *webrole0*, *webrole1*, and *webrole2*.</span></span> <span data-ttu-id="3afcc-108">Není potřeba zadat název hostitele pro virtuální počítače v konfiguračním souboru, protože název hostitele pro virtuální počítač je vyplněný, na základě názvu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3afcc-108">You do not need to specify a host name for virtual machines in the configuration file, because the host name for a virtual machine is populated based on the virtual machine name.</span></span> <span data-ttu-id="3afcc-109">Další informace o konfiguraci služby Microsoft Azure najdete v tématu [schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx)</span><span class="sxs-lookup"><span data-stu-id="3afcc-109">For more information about configuring a Microsoft Azure service, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx)</span></span>

## <a name="viewing-hostnames"></a><span data-ttu-id="3afcc-110">Zobrazení názvů hostitelů</span><span class="sxs-lookup"><span data-stu-id="3afcc-110">Viewing hostnames</span></span>
<span data-ttu-id="3afcc-111">Názvy hostitelů virtuálních počítačů a instancí rolí můžete zobrazit v cloudové službě pomocí některé z nástrojů uvedených dole.</span><span class="sxs-lookup"><span data-stu-id="3afcc-111">You can view the host names of virtual machines and role instances in a cloud service by using any of the tools below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="3afcc-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3afcc-112">Azure Portal</span></span>
<span data-ttu-id="3afcc-113">Můžete použít [portál Azure](http://portal.azure.com) zobrazíte názvy hostitelů pro virtuální počítače v okně Přehled pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3afcc-113">You can use the [Azure portal](http://portal.azure.com) to view the host names for virtual machines on the overview blade for a virtual machine.</span></span> <span data-ttu-id="3afcc-114">Mějte na paměti, která okna zobrazuje hodnotu pro **název** a **název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="3afcc-114">Keep in mind that the blade shows a value for **Name** and **Host Name**.</span></span> <span data-ttu-id="3afcc-115">I když se původně shodují, nebude změna názvu hostitele změňte název virtuálního počítače nebo role instance.</span><span class="sxs-lookup"><span data-stu-id="3afcc-115">Although they are initially the same, changing the host name will not change the name of the virtual machine or role instance.</span></span>

<span data-ttu-id="3afcc-116">Instance rolí lze také zobrazit na portálu Azure, ale když můžete seznam instancí v cloudové službě, se nezobrazí, název hostitele.</span><span class="sxs-lookup"><span data-stu-id="3afcc-116">Role instances can also be viewed in the Azure portal, but when you list the instances in a cloud service, the host name is not displayed.</span></span> <span data-ttu-id="3afcc-117">Zobrazí se název pro každou instanci, ale tento název nepředstavuje název hostitele.</span><span class="sxs-lookup"><span data-stu-id="3afcc-117">You will see a name for each instance, but that name does not represent the host name.</span></span>

### <a name="service-configuration-file"></a><span data-ttu-id="3afcc-118">Konfigurační soubor služby</span><span class="sxs-lookup"><span data-stu-id="3afcc-118">Service configuration file</span></span>
<span data-ttu-id="3afcc-119">Soubor konfigurace služby pro službu nasazenou z můžete stáhnout **konfigurace** okno služby v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3afcc-119">You can download the service configuration file for a deployed service from the **Configure** blade of the service in the Azure portal.</span></span> <span data-ttu-id="3afcc-120">Potom můžete vyhledat **vmName** atribut pro **název Role** element, který zobrazí název hostitele.</span><span class="sxs-lookup"><span data-stu-id="3afcc-120">You can then look for the **vmName** attribute for the **Role name** element to see the host name.</span></span> <span data-ttu-id="3afcc-121">Uvědomte si, že tento název hostitele slouží jako základ pro název hostitele každá instance role.</span><span class="sxs-lookup"><span data-stu-id="3afcc-121">Keep in mind that this host name is used as a base for the host name of each role instance.</span></span> <span data-ttu-id="3afcc-122">Například pokud **vmName** je *webrole* a jsou tři instance dané role, bude názvy hostitelů instancí *webrole0*, *webrole1*, a *webrole2*.</span><span class="sxs-lookup"><span data-stu-id="3afcc-122">For example, if **vmName** is *webrole* and there are three instances of that role, the host names of the instances will be *webrole0*, *webrole1*, and *webrole2*.</span></span>

### <a name="remote-desktop"></a><span data-ttu-id="3afcc-123">Vzdálená plocha</span><span class="sxs-lookup"><span data-stu-id="3afcc-123">Remote Desktop</span></span>
<span data-ttu-id="3afcc-124">Když povolíte vzdálená plocha (Windows), vzdálené komunikace Windows Powershellu (Windows) nebo připojení SSH (Linux a Windows) pro virtuální počítače nebo instance rolí, můžete zobrazit název hostitele z aktivního připojení vzdálené plochy různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="3afcc-124">After you enable Remote Desktop (Windows), Windows PowerShell remoting (Windows), or SSH (Linux and Windows) connections to your virtual machines or role instances, you can view the host name from an active Remote Desktop connection in various ways:</span></span>

* <span data-ttu-id="3afcc-125">Název hostitele napište na příkazový řádek nebo SSH terminálu.</span><span class="sxs-lookup"><span data-stu-id="3afcc-125">Type hostname at the command prompt or SSH terminal.</span></span>
* <span data-ttu-id="3afcc-126">Zadáním ipconfig/všechny do příkazového řádku (jenom Windows).</span><span class="sxs-lookup"><span data-stu-id="3afcc-126">Type ipconfig /all at the command prompt (Windows only).</span></span>
* <span data-ttu-id="3afcc-127">Zobrazte název počítače v nastavení systému (jenom Windows).</span><span class="sxs-lookup"><span data-stu-id="3afcc-127">View the computer name in the system settings (Windows only).</span></span>

### <a name="azure-service-management-rest-api"></a><span data-ttu-id="3afcc-128">Rozhraní REST API pro správu služby Azure</span><span class="sxs-lookup"><span data-stu-id="3afcc-128">Azure Service Management REST API</span></span>
<span data-ttu-id="3afcc-129">Z klienta REST postupujte podle těchto pokynů:</span><span class="sxs-lookup"><span data-stu-id="3afcc-129">From a REST client, follow these instructions:</span></span>

1. <span data-ttu-id="3afcc-130">Ujistěte se, že máte certifikát klienta pro připojení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3afcc-130">Ensure that you have a client certificate to connect to the Azure portal.</span></span> <span data-ttu-id="3afcc-131">Pokud chcete získat certifikát klienta, postupujte podle kroků v [postupy: stahování a Import nastavení publikování a informace o předplatném](https://msdn.microsoft.com/library/dn385850.aspx).</span><span class="sxs-lookup"><span data-stu-id="3afcc-131">To obtain a client certificate, follow the steps presented in [How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850.aspx).</span></span> 
2. <span data-ttu-id="3afcc-132">Nastavte položku záhlaví s názvem x-ms-version s hodnotou 2013-11-01.</span><span class="sxs-lookup"><span data-stu-id="3afcc-132">Set a header entry named x-ms-version with a value of 2013-11-01.</span></span>
3. <span data-ttu-id="3afcc-133">Poslat žádost o v následujícím formátu: https://management.core.windows.net/\<subscrition id\>/services/hostedservices/\<název služby\>? vložení detail = true</span><span class="sxs-lookup"><span data-stu-id="3afcc-133">Send a request in the following format: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<service-name\>?embed-detail=true</span></span>
4. <span data-ttu-id="3afcc-134">Vyhledejte **HostName** element pro každou **RoleInstance** element.</span><span class="sxs-lookup"><span data-stu-id="3afcc-134">Look for the **HostName** element for each **RoleInstance** element.</span></span>

> [!WARNING]
> <span data-ttu-id="3afcc-135">Můžete také zobrazit přípona interní domény pro cloudové služby z odpovědi volání REST kontrolou **InternalDnsSuffix** element, nebo spuštěním ipconfig/všechny z příkazového řádku v relaci vzdálené plochy (Windows) nebo po spuštění cat /etc/resolv.conf z terminálu SSH (Linux).</span><span class="sxs-lookup"><span data-stu-id="3afcc-135">You can also view the internal domain suffix for your cloud service from the REST call response by checking the **InternalDnsSuffix** element, or by running ipconfig /all from a command prompt in a Remote Desktop session (Windows), or by running cat /etc/resolv.conf from an SSH terminal (Linux).</span></span>
> 
> 

## <a name="modifying-a-hostname"></a><span data-ttu-id="3afcc-136">Úprava název hostitele</span><span class="sxs-lookup"><span data-stu-id="3afcc-136">Modifying a hostname</span></span>
<span data-ttu-id="3afcc-137">Název hostitele pro všechny virtuální počítače nebo role instance můžete upravit tím, že nahrajete konfigurační soubor změny služby, nebo přejmenování počítače z relace vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="3afcc-137">You can modify the host name for any virtual machine or role instance by uploading a modified service configuration file, or by renaming the computer from a Remote Desktop session.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3afcc-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3afcc-138">Next steps</span></span>
[<span data-ttu-id="3afcc-139">Překlad adres (DNS)</span><span class="sxs-lookup"><span data-stu-id="3afcc-139">Name Resolution (DNS)</span></span>](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[<span data-ttu-id="3afcc-140">Schéma konfigurace služby Azure (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="3afcc-140">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[<span data-ttu-id="3afcc-141">Schéma konfigurace virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="3afcc-141">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="3afcc-142">Zadejte nastavení DNS pomocí konfiguračních souborů síť</span><span class="sxs-lookup"><span data-stu-id="3afcc-142">Specify DNS settings using network configuration files</span></span>](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

