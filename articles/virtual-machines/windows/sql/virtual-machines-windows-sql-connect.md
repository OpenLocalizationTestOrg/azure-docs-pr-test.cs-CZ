---
title: "Připojení k virtuálnímu počítači s SQL serverem (Resource Manager) | Microsoft Docs"
description: "Zjistěte, jak se připojit k serveru SQL, které jsou spuštěny na virtuálním počítači v Azure. Toto téma používá model nasazení classic. Scénáře se liší v závislosti na konfiguraci sítě a umístění klienta."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: 67ba43f9456bbeffbf602067586143c4c68af672
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a><span data-ttu-id="b4271-105">Připojení k virtuálnímu počítači s SQL Serverem v Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="b4271-105">Connect to a SQL Server Virtual Machine on Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b4271-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b4271-106">Resource Manager</span></span>](virtual-machines-windows-sql-connect.md)
> * [<span data-ttu-id="b4271-107">Classic</span><span class="sxs-lookup"><span data-stu-id="b4271-107">Classic</span></span>](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="b4271-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="b4271-108">Overview</span></span>

<span data-ttu-id="b4271-109">Toto téma popisuje, jak se připojit k instanci systému SQL Server spuštěna na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="b4271-109">This topic describes how to connect to your SQL Server instance running on an Azure virtual machine.</span></span> <span data-ttu-id="b4271-110">Některé pokrývá [obecné připojení scénáře](#connection-scenarios) a pak poskytuje [podrobné kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="b4271-110">It covers some [general connectivity scenarios](#connection-scenarios) and then provides [detailed steps for configuring SQL Server connectivity in an Azure VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="b4271-111">Pokud máte by místo úplné návodu zřizování a připojení, najdete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="b4271-111">If you would rather have a full walk-through of both provisioning and connectivity, see [Provisioning a SQL Server Virtual Machine on Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="connection-scenarios"></a><span data-ttu-id="b4271-112">Scénáře připojení</span><span class="sxs-lookup"><span data-stu-id="b4271-112">Connection scenarios</span></span>

<span data-ttu-id="b4271-113">Způsob, jakým se klient připojí k serveru SQL Server spuštěny na virtuálním počítači se liší v závislosti na umístění klienta a konfigurace sítí.</span><span class="sxs-lookup"><span data-stu-id="b4271-113">The way a client connects to SQL Server running on a Virtual Machine differs depending on the location of the client and the networking configuration.</span></span>

<span data-ttu-id="b4271-114">Pokud zřídíte virtuální počítač SQL Server na portálu Azure, máte možnost zadat typ **připojení SQL**.</span><span class="sxs-lookup"><span data-stu-id="b4271-114">If you provision a SQL Server VM in the Azure portal, you have the option of specifying the type of **SQL connectivity**.</span></span>

![Veřejné možnosti připojení SQL během zřizování](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

<span data-ttu-id="b4271-116">Možnosti připojení patří:</span><span class="sxs-lookup"><span data-stu-id="b4271-116">Your options for connectivity include:</span></span>

| <span data-ttu-id="b4271-117">Možnost</span><span class="sxs-lookup"><span data-stu-id="b4271-117">Option</span></span> | <span data-ttu-id="b4271-118">Popis</span><span class="sxs-lookup"><span data-stu-id="b4271-118">Description</span></span> |
|---|---|
| <span data-ttu-id="b4271-119">**Veřejné**</span><span class="sxs-lookup"><span data-stu-id="b4271-119">**Public**</span></span> | <span data-ttu-id="b4271-120">Připojení k systému SQL Server přes internet</span><span class="sxs-lookup"><span data-stu-id="b4271-120">Connect to SQL Server over the internet</span></span> |
| <span data-ttu-id="b4271-121">**Privátní**</span><span class="sxs-lookup"><span data-stu-id="b4271-121">**Private**</span></span> | <span data-ttu-id="b4271-122">Připojení k systému SQL Server ve stejné virtuální síti</span><span class="sxs-lookup"><span data-stu-id="b4271-122">Connect to SQL Server in the same virtual network</span></span> |
| <span data-ttu-id="b4271-123">**Místní**</span><span class="sxs-lookup"><span data-stu-id="b4271-123">**Local**</span></span> | <span data-ttu-id="b4271-124">Připojení k systému SQL Server lokálně na jednom virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="b4271-124">Connect to SQL Server locally on the same virtual machine</span></span> | 

<span data-ttu-id="b4271-125">Následující části popisují **veřejné** a **privátní** možnosti podrobněji.</span><span class="sxs-lookup"><span data-stu-id="b4271-125">The following sections explain the **Public** and **Private** options in more detail.</span></span>

## <a name="connect-to-sql-server-over-the-internet"></a><span data-ttu-id="b4271-126">Připojení k systému SQL Server přes Internet</span><span class="sxs-lookup"><span data-stu-id="b4271-126">Connect to SQL Server over the Internet</span></span>

<span data-ttu-id="b4271-127">Pokud se chcete připojit k vaší databázový stroj SQL Server z Internetu, vyberte **veřejné** pro **připojení SQL** typ na portálu během zřizování.</span><span class="sxs-lookup"><span data-stu-id="b4271-127">If you want to connect to your SQL Server database engine from the Internet, select **Public** for the **SQL connectivity** type in the portal during provisioning.</span></span> <span data-ttu-id="b4271-128">Na portálu automaticky provede následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b4271-128">The portal automatically does the following steps:</span></span>

* <span data-ttu-id="b4271-129">Umožňuje protokol TCP/IP pro SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b4271-129">Enables the TCP/IP protocol for SQL Server.</span></span>
* <span data-ttu-id="b4271-130">Nakonfiguruje pravidlo brány firewall pro otevření portu TCP systému SQL Server (standardně 1433).</span><span class="sxs-lookup"><span data-stu-id="b4271-130">Configures a firewall rule to open the SQL Server TCP port (default 1433).</span></span>
* <span data-ttu-id="b4271-131">Umožňuje ověřování systému SQL Server, vyžaduje se pro veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="b4271-131">Enables SQL Server Authentication, required for public access.</span></span>
* <span data-ttu-id="b4271-132">Nakonfiguruje skupinu zabezpečení sítě na virtuálním počítači pro všechny přenosy TCP na portu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b4271-132">Configures the network security group on the VM to all TCP traffic on the SQL Server port.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b4271-133">Bitové kopie virtuálních počítačů pro SQL Server Developer a edice Express automaticky nepovolujte protokol TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="b4271-133">The virtual machine images for the SQL Server Developer and Express editions do not automatically enable the TCP/IP protocol.</span></span> <span data-ttu-id="b4271-134">U edice Developer a Express, je nutné použít Správce konfigurace systému SQL Server na [ručně povolit protokol TCP/IP](#manualtcp) po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b4271-134">For Developer and Express editions, you must use SQL Server Configuration Manager to [manually enable the TCP/IP protocol](#manualtcp) after creating the VM.</span></span>

<span data-ttu-id="b4271-135">Libovolného klienta s přístupem k Internetu může připojit k instanci systému SQL Server zadáním buď veřejnou IP adresu virtuálního počítače nebo libovolný popisek DNS přiřazené tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b4271-135">Any client with internet access can connect to the SQL Server instance by specifying either the public IP address of the virtual machine or any DNS label assigned to that IP address.</span></span> <span data-ttu-id="b4271-136">Pokud port serveru SQL Server je 1433, není potřeba zadat v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="b4271-136">If the SQL Server port is 1433, you do not need to specify it in the connection string.</span></span> <span data-ttu-id="b4271-137">Následující připojovací řetězec připojuje k virtuálnímu počítači SQL s jmenovkou DNS `sqlvmlabel.eastus.cloudapp.azure.com` pomocí ověřování SQL (můžete také použít veřejné IP adresy).</span><span class="sxs-lookup"><span data-stu-id="b4271-137">The following connection string connects to a SQL VM with a DNS label of `sqlvmlabel.eastus.cloudapp.azure.com` using SQL Authentication (you could also use the public IP address).</span></span>

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

<span data-ttu-id="b4271-138">I když to umožňuje připojení klientů přes internet, to neznamená, že každý, kdo může připojit k systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b4271-138">Although this enables connectivity for clients over the internet, this does not imply that anyone can connect to your SQL Server.</span></span> <span data-ttu-id="b4271-139">Klienti nemusí mimo správné uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="b4271-139">Outside clients have to the correct username and password.</span></span> <span data-ttu-id="b4271-140">Pro dodatečné zabezpečení se však vyhnout dobře známého portu 1433.</span><span class="sxs-lookup"><span data-stu-id="b4271-140">However, for additional security, you can avoid the well-known port 1433.</span></span> <span data-ttu-id="b4271-141">Například pokud jste nakonfigurovali SQL serveru pro naslouchání na portu 1500 a zavedené správné brány firewall a pravidel skupiny zabezpečení sítě, může připojení připojením číslo portu na název serveru.</span><span class="sxs-lookup"><span data-stu-id="b4271-141">For example, if you configured SQL Server to listen on port 1500 and established proper firewall and network security group rules, you could connect by appending the port number to the Server name.</span></span> <span data-ttu-id="b4271-142">Následující příklad mění předchozímu přidáním vlastní číslo portu, **1 500**, název serveru:</span><span class="sxs-lookup"><span data-stu-id="b4271-142">The following example alters the previous one by adding a custom port number, **1500**, to the server name:</span></span>

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> <span data-ttu-id="b4271-143">Když dotazujete systému SQL Server ve virtuálním počítači přes internet, všechny odchozí data z datové centrum Azure podléhá normální [ceny na odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/).</span><span class="sxs-lookup"><span data-stu-id="b4271-143">When you query SQL Server in a VM over the internet, all outgoing data from the Azure datacenter is subject to normal [pricing on outbound data transfers](https://azure.microsoft.com/pricing/details/data-transfers/).</span></span>

## <a name="connect-to-sql-server-within-a-virtual-network"></a><span data-ttu-id="b4271-144">Připojení k systému SQL Server v rámci virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b4271-144">Connect to SQL Server within a virtual network</span></span>

<span data-ttu-id="b4271-145">Pokud vyberete **privátní** pro **připojení SQL** typ na portálu Azure nakonfiguruje většinu nastavení identické **veřejné**.</span><span class="sxs-lookup"><span data-stu-id="b4271-145">When you choose **Private** for the **SQL connectivity** type in the portal, Azure configures most of the settings identical to **Public**.</span></span> <span data-ttu-id="b4271-146">Jeden rozdíl je, že neexistuje žádná pravidla skupiny zabezpečení sítě umožňuje mimo přenosy na portu systému SQL Server (standardně 1433).</span><span class="sxs-lookup"><span data-stu-id="b4271-146">The one difference is that there is no network security group rule to allow outside traffic on the SQL Server port (default 1433).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b4271-147">Bitové kopie virtuálních počítačů pro SQL Server Developer a edice Express automaticky nepovolujte protokol TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="b4271-147">The virtual machine images for the SQL Server Developer and Express editions do not automatically enable the TCP/IP protocol.</span></span> <span data-ttu-id="b4271-148">U edice Developer a Express, je nutné použít Správce konfigurace systému SQL Server na [ručně povolit protokol TCP/IP](#manualtcp) po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b4271-148">For Developer and Express editions, you must use SQL Server Configuration Manager to [manually enable the TCP/IP protocol](#manualtcp) after creating the VM.</span></span>

<span data-ttu-id="b4271-149">Privátní připojení se často používá ve spojení s [virtuální sítě](../../../virtual-network/virtual-networks-overview.md), což umožňuje několik scénářů.</span><span class="sxs-lookup"><span data-stu-id="b4271-149">Private connectivity is often used in conjunction with [Virtual Network](../../../virtual-network/virtual-networks-overview.md), which enables several scenarios.</span></span> <span data-ttu-id="b4271-150">I když tyto virtuální počítače existovat v různých skupinách prostředků se můžete připojit virtuální počítače ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="b4271-150">You can connect VMs in the same virtual network, even if those VMs exist in different resource groups.</span></span> <span data-ttu-id="b4271-151">A s [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), můžete vytvořit hybridní architekturu, která připojí virtuální počítače s místními sítěmi a počítače.</span><span class="sxs-lookup"><span data-stu-id="b4271-151">And with a [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), you can create a hybrid architecture that connects VMs with on-premises networks and machines.</span></span>

<span data-ttu-id="b4271-152">Virtuální sítě umožňují připojit virtuální počítače Azure k doméně.</span><span class="sxs-lookup"><span data-stu-id="b4271-152">Virtual networks also enable     you to join your Azure VMs to a domain.</span></span> <span data-ttu-id="b4271-153">Toto je jediným způsobem, jak použít ověřování systému Windows na serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b4271-153">This is the only way to use Windows Authentication to SQL Server.</span></span> <span data-ttu-id="b4271-154">Jiné připojení scénáře vyžadují ověřování SQL pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="b4271-154">The other connection scenarios require SQL Authentication with user names and passwords.</span></span>

<span data-ttu-id="b4271-155">Za předpokladu, že nakonfigurujete DNS ve virtuální síti, můžete se připojit k instanci systému SQL Server zadáním názvu počítače virtuální počítač s SQL serverem v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="b4271-155">Assuming that you have configured DNS in your virtual network, you can connect to your SQL Server instance by specifying the SQL Server VM computer name in the connection string.</span></span> <span data-ttu-id="b4271-156">Následující příklad předpokládá také ověřování systému Windows také nakonfigurován a že uživatel byl udělen přístup k instanci systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b4271-156">The following example also assumes that Windows Authentication has also been configured and that the user has been granted access to the SQL Server instance.</span></span>

```
Server=mysqlvm;Integrated Security=true
```

## <span data-ttu-id="b4271-157"><a id="change"></a>Změňte nastavení připojení k SQL</span><span class="sxs-lookup"><span data-stu-id="b4271-157"><a id="change"></a> Change SQL connectivity settings</span></span>

<span data-ttu-id="b4271-158">Můžete změnit nastavení připojení pro virtuální počítač systému SQL Server na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b4271-158">You can change the connectivity settings for your SQL Server virtual machine in the Azure portal.</span></span>

1. <span data-ttu-id="b4271-159">Na portálu Azure vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="b4271-159">In the Azure portal, select **Virtual Machines**.</span></span>

2. <span data-ttu-id="b4271-160">Vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b4271-160">Select your SQL Server VM.</span></span>

3. <span data-ttu-id="b4271-161">V části **nastavení**, klikněte na tlačítko **konfigurace systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="b4271-161">Under **Settings**, click **SQL Server configuration**.</span></span>

4. <span data-ttu-id="b4271-162">Změna **úroveň připojení SQL** na požadované nastavení.</span><span class="sxs-lookup"><span data-stu-id="b4271-162">Change the **SQL connectivity level** to your required setting.</span></span> <span data-ttu-id="b4271-163">Volitelně můžete této oblasti změnit port serveru SQL Server nebo nastavení ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="b4271-163">You can optionally use this area to change the SQL Server port or the SQL Authentication settings.</span></span>

   ![Změnit připojení SQL](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. <span data-ttu-id="b4271-165">Počkejte několik minut na dokončení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b4271-165">Wait several minutes for the update to complete.</span></span>

   ![Virtuální počítač SQL aktualizace oznámení](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <span data-ttu-id="b4271-167"><a id="manualtcp"></a>Povolte protokol TCP/IP pro edice Developer a Express</span><span class="sxs-lookup"><span data-stu-id="b4271-167"><a id="manualtcp"></a> Enable TCP/IP for Developer and Express editions</span></span>

<span data-ttu-id="b4271-168">Při změně nastavení připojení k systému SQL Server, Azure automaticky neumožňuje protokol TCP/IP pro vývojáře serveru SQL a edice Express.</span><span class="sxs-lookup"><span data-stu-id="b4271-168">When changing SQL Server connectivity settings, Azure does not automatically enable the TCP/IP protocol for SQL Server Developer and Express editions.</span></span> <span data-ttu-id="b4271-169">Následující kroky popisují ruční povolení protokolu TCP/IP, abyste se mohli vzdáleně připojit pomocí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b4271-169">The steps below explain how to manually enable TCP/IP so that you can connect remotely by IP address.</span></span>

<span data-ttu-id="b4271-170">Nejdřív připojte k počítači serveru SQL Server pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="b4271-170">First, connect to the SQL Server machine with remote desktop.</span></span>

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

<span data-ttu-id="b4271-171">Dál povolte protokol TCP/IP s **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="b4271-171">Next, enable the TCP/IP protocol with **SQL Server Configuration Manager**.</span></span>

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a><span data-ttu-id="b4271-172">Připojení přes SSMS</span><span class="sxs-lookup"><span data-stu-id="b4271-172">Connect with SSMS</span></span>

<span data-ttu-id="b4271-173">Následující kroky ukazují, jak vytvořit popisek volitelné DNS pro virtuální počítač Azure a potom se připojte pomocí SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="b4271-173">The following steps show how to create an optional DNS Label for your Azure VM and then connect with SQL Server Management Studio (SSMS).</span></span>

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="b4271-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b4271-174">Next Steps</span></span>

<span data-ttu-id="b4271-175">Zřizování pokyny společně s postupem připojení najdete v tématu [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="b4271-175">To see provisioning instructions along with these connectivity steps, see [Provisioning a SQL Server Virtual Machine on Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

<span data-ttu-id="b4271-176">Další témata související se systémem SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b4271-176">For other topics related to running SQL Server in Azure VMs, see [SQL Server on Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>