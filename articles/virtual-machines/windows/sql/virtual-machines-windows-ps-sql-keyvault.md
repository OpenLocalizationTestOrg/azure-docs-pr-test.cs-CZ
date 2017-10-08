---
title: "aaaIntegrate Key Vault se systémem SQL Server na virtuálních počítačích Windows v Azure (Resource Manager) | Microsoft Docs"
description: "Zjistěte, jak tooautomate hello konfigurace systému SQL Server šifrování pro použití s Azure Key Vault. Toto téma vysvětluje, jak vytvořit toouse Azure Key Vault integrace s virtuálními počítači systému SQL Server pomocí Správce prostředků."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: jroth
ms.openlocfilehash: 0d36d3d075d6538c18cd5ecb43c19a4b000a99e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a><span data-ttu-id="5ca4e-104">Konfigurace integrace Azure Key Vault pro SQL Server na virtuálních počítačích Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="5ca4e-104">Configure Azure Key Vault Integration for SQL Server on Azure Virtual Machines (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5ca4e-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5ca4e-105">Resource Manager</span></span>](virtual-machines-windows-ps-sql-keyvault.md)
> * [<span data-ttu-id="5ca4e-106">Classic</span><span class="sxs-lookup"><span data-stu-id="5ca4e-106">Classic</span></span>](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="5ca4e-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="5ca4e-107">Overview</span></span>
<span data-ttu-id="5ca4e-108">Existuje více funkcí šifrování systému SQL Server, například [transparentní šifrování dat (šifrování TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [šifrování na úrovni sloupce (Vymazat)](https://msdn.microsoft.com/library/ms173744.aspx), a [zálohu šifrovacího](https://msdn.microsoft.com/library/dn449489.aspx).</span><span class="sxs-lookup"><span data-stu-id="5ca4e-108">There are multiple SQL Server encryption features, such as [transparent data encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [column level encryption (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), and [backup encryption](https://msdn.microsoft.com/library/dn449489.aspx).</span></span> <span data-ttu-id="5ca4e-109">Tyto formuláře šifrování vyžadovat toomanage a uložení hello kryptografických klíčů, které používáte pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-109">These forms of encryption require you toomanage and store hello cryptographic keys you use for encryption.</span></span> <span data-ttu-id="5ca4e-110">Hello službou Azure Key Vault (AZURE) služby je navrženou tooimprove hello zabezpečení a správu tyto klíče v umístění zabezpečené a vysoce dostupné.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-110">hello Azure Key Vault (AKV) service is designed tooimprove hello security and management of these keys in a secure and highly available location.</span></span> <span data-ttu-id="5ca4e-111">Hello [konektor služby serveru SQL](http://www.microsoft.com/download/details.aspx?id=45344) umožňuje tyto klíče z Azure Key Vault toouse systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-111">hello [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) enables SQL Server toouse these keys from Azure Key Vault.</span></span>

<span data-ttu-id="5ca4e-112">Pokud jste s místním systémem SQL Server počítačů existuje jsou [kroky, které vám pomůžou tooaccess Azure Key Vault z vašeho místního počítače systému SQL Server](https://msdn.microsoft.com/library/dn198405.aspx).</span><span class="sxs-lookup"><span data-stu-id="5ca4e-112">If you running SQL Server with on-premises machines, there are [steps you can follow tooaccess Azure Key Vault from your on-premises SQL Server machine](https://msdn.microsoft.com/library/dn198405.aspx).</span></span> <span data-ttu-id="5ca4e-113">Ale pro SQL Server ve virtuálních počítačích Azure, můžete ušetřit čas pomocí hello *Azure Key Vault integrace* funkce.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-113">But for SQL Server in Azure VMs, you can save time by using hello *Azure Key Vault Integration* feature.</span></span>

<span data-ttu-id="5ca4e-114">Pokud je tato funkce povolena, automaticky se nainstaluje hello konektor služby serveru SQL, nakonfiguruje hello EKM provider tooaccess Azure Key Vault a vytvoří tooallow hello přihlašovacích údajů můžete tooaccess svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-114">When this feature is enabled, it automatically installs hello SQL Server Connector, configures hello EKM provider tooaccess Azure Key Vault, and creates hello credential tooallow you tooaccess your vault.</span></span> <span data-ttu-id="5ca4e-115">Pokud hledá v hello kroky v hello už jsme zmínili místní dokumentaci, uvidíte, že tato funkce automatizuje kroky 2 a 3.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-115">If you looked at hello steps in hello previously mentioned on-premises documentation, you can see that this feature automates steps 2 and 3.</span></span> <span data-ttu-id="5ca4e-116">Hello pouze věc stále nutné toodo ručně, je trezor klíčů hello toocreate a klíče.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-116">hello only thing you would still need toodo manually is toocreate hello key vault and keys.</span></span> <span data-ttu-id="5ca4e-117">Z tohoto místa se automatizované hello celý nastavení virtuálního počítače SQL.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-117">From there, hello entire setup of your SQL VM is automated.</span></span> <span data-ttu-id="5ca4e-118">Po dokončení této instalace této funkce můžete spustit T-SQL příkazy toobegin šifrování databáze nebo zálohy běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-118">Once this feature has completed this setup, you can execute T-SQL statements toobegin encrypting your databases or backups as you normally would.</span></span>

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a><span data-ttu-id="5ca4e-119">Povolení a konfigurace integrace se službou AZURE</span><span class="sxs-lookup"><span data-stu-id="5ca4e-119">Enabling and configuring AKV integration</span></span>
<span data-ttu-id="5ca4e-120">Můžete povolit integrace se službou AZURE při zřizování nebo ho nakonfigurovat pro existující virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-120">You can enable AKV integration during provisioning or configure it for existing VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="5ca4e-121">Nové virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="5ca4e-121">New VMs</span></span>
<span data-ttu-id="5ca4e-122">Pokud zřizujete nového virtuálního počítače systému SQL Server s Resource Managerem, hello portál Azure poskytuje krok tooenable integrace se službou Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-122">If you are provisioning a new SQL Server virtual machine with Resource Manager, hello Azure portal provides a step tooenable Azure Key Vault integration.</span></span> <span data-ttu-id="5ca4e-123">Funkce Hello Azure Key Vault je dostupná pouze pro hello Enterprise, Developer a zkušební edice systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-123">hello Azure Key Vault feature is available only for hello Enterprise, Developer and Evaluation Editions of SQL Server.</span></span>

![Integrace se službou Azure Key Vault pro SQL](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

<span data-ttu-id="5ca4e-125">Podrobný návod k zřizování, najdete v části [zřízení virtuálního počítače s SQL Server v hello portálu Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="5ca4e-125">For a detailed walkthrough of provisioning, see [Provision a SQL Server virtual machine in hello Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="5ca4e-126">Existující virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="5ca4e-126">Existing VMs</span></span>
<span data-ttu-id="5ca4e-127">Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-127">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="5ca4e-128">Potom vyberte hello **konfigurace systému SQL Server** části hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-128">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![Integrace se službou AZURE SQL pro existující virtuální počítače](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

<span data-ttu-id="5ca4e-130">V hello **konfigurace systému SQL Server** okně klikněte na tlačítko hello **upravit** tlačítka na hello části Integrace automatizované Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-130">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated Key Vault integration section.</span></span>

![Konfigurace integrace se službou AZURE SQL pro existující virtuální počítače](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

<span data-ttu-id="5ca4e-132">Po dokončení klikněte na tlačítko hello **OK** na konci hello hello tlačítko **konfigurace systému SQL Server** okno toosave změny.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-132">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

> [!NOTE]
> <span data-ttu-id="5ca4e-133">Můžete také nakonfigurovat integrace se službou AZURE pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="5ca4e-133">You can also configure AKV integration using a template.</span></span> <span data-ttu-id="5ca4e-134">Další informace najdete v tématu [šablony Azure rychlý start pro integraci Azure Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span><span class="sxs-lookup"><span data-stu-id="5ca4e-134">For more information, see [Azure quickstart template for Azure Key Vault integration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span></span>
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

