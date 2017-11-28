---
title: "aaaConnect tooa virtuálního počítače systému SQL Server (Resource Manager) | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooSQL serveru spuštěny na virtuálním počítači v Azure. Toto téma používá model nasazení classic hello. scénáře Hello se liší v závislosti na konfiguraci sítí hello a umístění hello hello klienta."
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
ms.openlocfilehash: 7b127c14c37b9a72c19ed17f8b1dad61c7bc2d38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-resource-manager"></a><span data-ttu-id="37488-105">Připojit tooa virtuálního počítače systému SQL Server na platformě Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="37488-105">Connect tooa SQL Server Virtual Machine on Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="37488-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="37488-106">Resource Manager</span></span>](virtual-machines-windows-sql-connect.md)
> * [<span data-ttu-id="37488-107">Classic</span><span class="sxs-lookup"><span data-stu-id="37488-107">Classic</span></span>](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="37488-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="37488-108">Overview</span></span>

<span data-ttu-id="37488-109">Toto téma popisuje, jak tooconnect tooyour systému SQL Server instance spuštěna na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="37488-109">This topic describes how tooconnect tooyour SQL Server instance running on an Azure virtual machine.</span></span> <span data-ttu-id="37488-110">Některé pokrývá [obecné připojení scénáře](#connection-scenarios) a pak poskytuje [podrobné kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="37488-110">It covers some [general connectivity scenarios](#connection-scenarios) and then provides [detailed steps for configuring SQL Server connectivity in an Azure VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="37488-111">Pokud máte by místo úplné návodu zřizování a připojení, najdete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="37488-111">If you would rather have a full walk-through of both provisioning and connectivity, see [Provisioning a SQL Server Virtual Machine on Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="connection-scenarios"></a><span data-ttu-id="37488-112">Scénáře připojení</span><span class="sxs-lookup"><span data-stu-id="37488-112">Connection scenarios</span></span>

<span data-ttu-id="37488-113">způsob Hello klient připojí tooSQL Server běžící na virtuálním počítači, se může lišit v závislosti na umístění hello hello klienta a síťové konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="37488-113">hello way a client connects tooSQL Server running on a Virtual Machine differs depending on hello location of hello client and hello networking configuration.</span></span>

<span data-ttu-id="37488-114">Pokud zřizujete virtuální počítač SQL Server v hello portálu Azure, máte možnost hello určující typ hello **připojení SQL**.</span><span class="sxs-lookup"><span data-stu-id="37488-114">If you provision a SQL Server VM in hello Azure portal, you have hello option of specifying hello type of **SQL connectivity**.</span></span>

![Veřejné možnosti připojení SQL během zřizování](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

<span data-ttu-id="37488-116">Možnosti připojení patří:</span><span class="sxs-lookup"><span data-stu-id="37488-116">Your options for connectivity include:</span></span>

| <span data-ttu-id="37488-117">Možnost</span><span class="sxs-lookup"><span data-stu-id="37488-117">Option</span></span> | <span data-ttu-id="37488-118">Popis</span><span class="sxs-lookup"><span data-stu-id="37488-118">Description</span></span> |
|---|---|
| <span data-ttu-id="37488-119">**Veřejné**</span><span class="sxs-lookup"><span data-stu-id="37488-119">**Public**</span></span> | <span data-ttu-id="37488-120">Připojit tooSQL serveru přes hello Internetu</span><span class="sxs-lookup"><span data-stu-id="37488-120">Connect tooSQL Server over hello internet</span></span> |
| <span data-ttu-id="37488-121">**Privátní**</span><span class="sxs-lookup"><span data-stu-id="37488-121">**Private**</span></span> | <span data-ttu-id="37488-122">Připojit tooSQL serveru v hello stejné virtuální síti</span><span class="sxs-lookup"><span data-stu-id="37488-122">Connect tooSQL Server in hello same virtual network</span></span> |
| <span data-ttu-id="37488-123">**Místní**</span><span class="sxs-lookup"><span data-stu-id="37488-123">**Local**</span></span> | <span data-ttu-id="37488-124">Připojit tooSQL serveru místně na hello stejného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="37488-124">Connect tooSQL Server locally on hello same virtual machine</span></span> | 

<span data-ttu-id="37488-125">Hello následující části popisují hello **veřejné** a **privátní** možnosti podrobněji.</span><span class="sxs-lookup"><span data-stu-id="37488-125">hello following sections explain hello **Public** and **Private** options in more detail.</span></span>

## <a name="connect-toosql-server-over-hello-internet"></a><span data-ttu-id="37488-126">Připojit tooSQL serveru přes Internet hello</span><span class="sxs-lookup"><span data-stu-id="37488-126">Connect tooSQL Server over hello Internet</span></span>

<span data-ttu-id="37488-127">Pokud chcete, aby databázový stroj SQL Server tooconnect tooyour z hello Internet, vyberte **veřejné** pro hello **připojení SQL** typ portálu hello během zřizování.</span><span class="sxs-lookup"><span data-stu-id="37488-127">If you want tooconnect tooyour SQL Server database engine from hello Internet, select **Public** for hello **SQL connectivity** type in hello portal during provisioning.</span></span> <span data-ttu-id="37488-128">Hello portálu automaticky hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="37488-128">hello portal automatically does hello following steps:</span></span>

* <span data-ttu-id="37488-129">Umožňuje hello protokol TCP/IP pro SQL Server.</span><span class="sxs-lookup"><span data-stu-id="37488-129">Enables hello TCP/IP protocol for SQL Server.</span></span>
* <span data-ttu-id="37488-130">Nakonfiguruje hello tooopen pravidlo brány firewall port TCP systému SQL Server (standardně 1433).</span><span class="sxs-lookup"><span data-stu-id="37488-130">Configures a firewall rule tooopen hello SQL Server TCP port (default 1433).</span></span>
* <span data-ttu-id="37488-131">Umožňuje ověřování systému SQL Server, vyžaduje se pro veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="37488-131">Enables SQL Server Authentication, required for public access.</span></span>
* <span data-ttu-id="37488-132">Nakonfiguruje skupinu zabezpečení sítě hello na hello přenosy TCP tooall virtuálních počítačů na hello port serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="37488-132">Configures hello network security group on hello VM tooall TCP traffic on hello SQL Server port.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37488-133">Hello bitové kopie virtuálních počítačů pro hello vývojáře serveru SQL a edice Express automaticky nepovolujte protokol hello protokolu TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="37488-133">hello virtual machine images for hello SQL Server Developer and Express editions do not automatically enable hello TCP/IP protocol.</span></span> <span data-ttu-id="37488-134">Pro edice Developer a Express, musíte použít SQL Server Configuration Manager příliš[ručně povolte protokol hello TCP/IP](#manualtcp) po vytvoření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="37488-134">For Developer and Express editions, you must use SQL Server Configuration Manager too[manually enable hello TCP/IP protocol](#manualtcp) after creating hello VM.</span></span>

<span data-ttu-id="37488-135">Libovolného klienta s přístupem k Internetu, můžete připojit toohello instance systému SQL Server zadáním hello veřejnou IP adresu hello virtuálního počítače nebo libovolný popisek DNS přiřazenou toothat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="37488-135">Any client with internet access can connect toohello SQL Server instance by specifying either hello public IP address of hello virtual machine or any DNS label assigned toothat IP address.</span></span> <span data-ttu-id="37488-136">Pokud hello port serveru SQL Server je 1433, není nutné toospecify v hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="37488-136">If hello SQL Server port is 1433, you do not need toospecify it in hello connection string.</span></span> <span data-ttu-id="37488-137">Hello následující připojovací řetězec připojí tooa virtuální počítač SQL s popiskem DNS z `sqlvmlabel.eastus.cloudapp.azure.com` pomocí ověřování SQL (můžete také použít hello veřejné IP adresy).</span><span class="sxs-lookup"><span data-stu-id="37488-137">hello following connection string connects tooa SQL VM with a DNS label of `sqlvmlabel.eastus.cloudapp.azure.com` using SQL Authentication (you could also use hello public IP address).</span></span>

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

<span data-ttu-id="37488-138">I když to umožňuje připojení klientů přes internet Dobrý den, to neznamená, že každý, kdo se může připojit tooyour systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="37488-138">Although this enables connectivity for clients over hello internet, this does not imply that anyone can connect tooyour SQL Server.</span></span> <span data-ttu-id="37488-139">Mimo klienti mají toohello správné uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="37488-139">Outside clients have toohello correct username and password.</span></span> <span data-ttu-id="37488-140">Pro dodatečné zabezpečení se však vyhnout hello dobře známého portu 1433.</span><span class="sxs-lookup"><span data-stu-id="37488-140">However, for additional security, you can avoid hello well-known port 1433.</span></span> <span data-ttu-id="37488-141">Například pokud jste nakonfigurovali toolisten systému SQL Server na portu 1500 a zavedené správné brány firewall a pravidel skupiny zabezpečení sítě, může připojení připojením hello port číslo toohello serveru název.</span><span class="sxs-lookup"><span data-stu-id="37488-141">For example, if you configured SQL Server toolisten on port 1500 and established proper firewall and network security group rules, you could connect by appending hello port number toohello Server name.</span></span> <span data-ttu-id="37488-142">Hello následující příklad mění hello předchozí přidáním vlastní číslo portu, **1 500**, toohello název serveru:</span><span class="sxs-lookup"><span data-stu-id="37488-142">hello following example alters hello previous one by adding a custom port number, **1500**, toohello server name:</span></span>

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> <span data-ttu-id="37488-143">Při dotazování systému SQL Server ve virtuálním počítači přes internet, všechna odchozí data hello z hello Azure datacenter je subjektu toonormal [ceny na odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/).</span><span class="sxs-lookup"><span data-stu-id="37488-143">When you query SQL Server in a VM over hello internet, all outgoing data from hello Azure datacenter is subject toonormal [pricing on outbound data transfers](https://azure.microsoft.com/pricing/details/data-transfers/).</span></span>

## <a name="connect-toosql-server-within-a-virtual-network"></a><span data-ttu-id="37488-144">Připojit tooSQL serveru v rámci virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="37488-144">Connect tooSQL Server within a virtual network</span></span>

<span data-ttu-id="37488-145">Pokud vyberete **privátní** pro hello **připojení SQL** typ hello portálu Azure nakonfiguruje většinu nastavení hello identické příliš**veřejné**.</span><span class="sxs-lookup"><span data-stu-id="37488-145">When you choose **Private** for hello **SQL connectivity** type in hello portal, Azure configures most of hello settings identical too**Public**.</span></span> <span data-ttu-id="37488-146">Hello jeden rozdílem je, že neexistuje žádná síť zabezpečení skupiny pravidlo tooallow mimo přenosy na portu hello systému SQL Server (standardně 1433).</span><span class="sxs-lookup"><span data-stu-id="37488-146">hello one difference is that there is no network security group rule tooallow outside traffic on hello SQL Server port (default 1433).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37488-147">Hello bitové kopie virtuálních počítačů pro hello vývojáře serveru SQL a edice Express automaticky nepovolujte protokol hello protokolu TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="37488-147">hello virtual machine images for hello SQL Server Developer and Express editions do not automatically enable hello TCP/IP protocol.</span></span> <span data-ttu-id="37488-148">Pro edice Developer a Express, musíte použít SQL Server Configuration Manager příliš[ručně povolte protokol hello TCP/IP](#manualtcp) po vytvoření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="37488-148">For Developer and Express editions, you must use SQL Server Configuration Manager too[manually enable hello TCP/IP protocol](#manualtcp) after creating hello VM.</span></span>

<span data-ttu-id="37488-149">Privátní připojení se často používá ve spojení s [virtuální sítě](../../../virtual-network/virtual-networks-overview.md), což umožňuje několik scénářů.</span><span class="sxs-lookup"><span data-stu-id="37488-149">Private connectivity is often used in conjunction with [Virtual Network](../../../virtual-network/virtual-networks-overview.md), which enables several scenarios.</span></span> <span data-ttu-id="37488-150">Můžete se připojit virtuální počítače ve stejné virtuální síti, i když tyto virtuální počítače existovat v různých skupinách prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="37488-150">You can connect VMs in hello same virtual network, even if those VMs exist in different resource groups.</span></span> <span data-ttu-id="37488-151">A s [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), můžete vytvořit hybridní architekturu, která připojí virtuální počítače s místními sítěmi a počítače.</span><span class="sxs-lookup"><span data-stu-id="37488-151">And with a [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), you can create a hybrid architecture that connects VMs with on-premises networks and machines.</span></span>

<span data-ttu-id="37488-152">Virtuální sítě také povolit toojoin jste doménu tooa virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="37488-152">Virtual networks also enable     you toojoin your Azure VMs tooa domain.</span></span> <span data-ttu-id="37488-153">Toto je hello pouze způsob toouse ověřování systému Windows tooSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="37488-153">This is hello only way toouse Windows Authentication tooSQL Server.</span></span> <span data-ttu-id="37488-154">Hello jiné připojení scénáře vyžadují ověřování SQL pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="37488-154">hello other connection scenarios require SQL Authentication with user names and passwords.</span></span>

<span data-ttu-id="37488-155">Za předpokladu, že nakonfigurujete DNS ve virtuální síti, můžete připojit tooyour instance systému SQL Server zadáním názvu počítače hello virtuální počítač s SQL serverem v hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="37488-155">Assuming that you have configured DNS in your virtual network, you can connect tooyour SQL Server instance by specifying hello SQL Server VM computer name in hello connection string.</span></span> <span data-ttu-id="37488-156">Hello také následující příklad předpokládá, že ověřování systému Windows je také nakonfigurovaný a tento uživatel hello byla udělena instance systému SQL Server toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="37488-156">hello following example also assumes that Windows Authentication has also been configured and that hello user has been granted access toohello SQL Server instance.</span></span>

```
Server=mysqlvm;Integrated Security=true
```

## <span data-ttu-id="37488-157"><a id="change"></a>Změňte nastavení připojení k SQL</span><span class="sxs-lookup"><span data-stu-id="37488-157"><a id="change"></a> Change SQL connectivity settings</span></span>

<span data-ttu-id="37488-158">Pro virtuální počítač systému SQL Server v hello portálu Azure, můžete změnit nastavení připojení k hello.</span><span class="sxs-lookup"><span data-stu-id="37488-158">You can change hello connectivity settings for your SQL Server virtual machine in hello Azure portal.</span></span>

1. <span data-ttu-id="37488-159">V hello portálu Azure, vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="37488-159">In hello Azure portal, select **Virtual Machines**.</span></span>

2. <span data-ttu-id="37488-160">Vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="37488-160">Select your SQL Server VM.</span></span>

3. <span data-ttu-id="37488-161">V části **nastavení**, klikněte na tlačítko **konfigurace systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="37488-161">Under **Settings**, click **SQL Server configuration**.</span></span>

4. <span data-ttu-id="37488-162">Změna hello **úroveň připojení SQL** tooyour požadované nastavení.</span><span class="sxs-lookup"><span data-stu-id="37488-162">Change hello **SQL connectivity level** tooyour required setting.</span></span> <span data-ttu-id="37488-163">Volitelně můžete této oblasti toochange hello port serveru SQL Server nebo hello nastavení ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="37488-163">You can optionally use this area toochange hello SQL Server port or hello SQL Authentication settings.</span></span>

   ![Změnit připojení SQL](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. <span data-ttu-id="37488-165">Počkejte několik minut, než toocomplete aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="37488-165">Wait several minutes for hello update toocomplete.</span></span>

   ![Virtuální počítač SQL aktualizace oznámení](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <span data-ttu-id="37488-167"><a id="manualtcp"></a>Povolte protokol TCP/IP pro edice Developer a Express</span><span class="sxs-lookup"><span data-stu-id="37488-167"><a id="manualtcp"></a> Enable TCP/IP for Developer and Express editions</span></span>

<span data-ttu-id="37488-168">Při změně nastavení připojení k systému SQL Server, Azure automaticky neumožňuje protokol hello TCP/IP pro vývojáře serveru SQL a edice Express.</span><span class="sxs-lookup"><span data-stu-id="37488-168">When changing SQL Server connectivity settings, Azure does not automatically enable hello TCP/IP protocol for SQL Server Developer and Express editions.</span></span> <span data-ttu-id="37488-169">Hello kroky vysvětlují, jak toomanually povolte protokol TCP/IP, takže se můžete vzdáleně připojit pomocí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="37488-169">hello steps below explain how toomanually enable TCP/IP so that you can connect remotely by IP address.</span></span>

<span data-ttu-id="37488-170">Nejdřív připojte počítač systému SQL Server toohello pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="37488-170">First, connect toohello SQL Server machine with remote desktop.</span></span>

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

<span data-ttu-id="37488-171">Dál povolte protokol hello TCP/IP s **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="37488-171">Next, enable hello TCP/IP protocol with **SQL Server Configuration Manager**.</span></span>

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a><span data-ttu-id="37488-172">Připojení přes SSMS</span><span class="sxs-lookup"><span data-stu-id="37488-172">Connect with SSMS</span></span>

<span data-ttu-id="37488-173">Hello následující kroky ukazují, jak toocreate volitelné DNS popisek pro virtuální počítač Azure a potom se připojte pomocí SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="37488-173">hello following steps show how toocreate an optional DNS Label for your Azure VM and then connect with SQL Server Management Studio (SSMS).</span></span>

[!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="37488-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37488-174">Next Steps</span></span>

<span data-ttu-id="37488-175">zřizování pokyny toosee společně s postupem připojení naleznete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="37488-175">toosee provisioning instructions along with these connectivity steps, see [Provisioning a SQL Server Virtual Machine on Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

<span data-ttu-id="37488-176">Další témata související s toorunning systému SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="37488-176">For other topics related toorunning SQL Server in Azure VMs, see [SQL Server on Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
