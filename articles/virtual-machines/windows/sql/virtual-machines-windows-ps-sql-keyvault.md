---
title: "Trezor klíčů integraci se službou SQL Server na virtuálních počítačích Windows v Azure (Resource Manager) | Microsoft Docs"
description: "Zjistěte, jak k automatizaci konfigurace systému SQL Server šifrování pro použití s Azure Key Vault. Toto téma vysvětluje, jak používat Azure Key Vault integrace s SQL Server na virtuální počítače vytvořené pomocí Resource Manageru."
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
ms.openlocfilehash: 32b9564fa5c9ca6864ade343fda309b2c3edf123
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a><span data-ttu-id="76c40-104">Konfigurace integrace Azure Key Vault pro SQL Server na virtuálních počítačích Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="76c40-104">Configure Azure Key Vault Integration for SQL Server on Azure Virtual Machines (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="76c40-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="76c40-105">Resource Manager</span></span>](virtual-machines-windows-ps-sql-keyvault.md)
> * [<span data-ttu-id="76c40-106">Classic</span><span class="sxs-lookup"><span data-stu-id="76c40-106">Classic</span></span>](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="76c40-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="76c40-107">Overview</span></span>
<span data-ttu-id="76c40-108">Existuje více funkcí šifrování systému SQL Server, například [transparentní šifrování dat (šifrování TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [šifrování na úrovni sloupce (Vymazat)](https://msdn.microsoft.com/library/ms173744.aspx), a [zálohu šifrovacího](https://msdn.microsoft.com/library/dn449489.aspx).</span><span class="sxs-lookup"><span data-stu-id="76c40-108">There are multiple SQL Server encryption features, such as [transparent data encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [column level encryption (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), and [backup encryption](https://msdn.microsoft.com/library/dn449489.aspx).</span></span> <span data-ttu-id="76c40-109">Tyto formuláře šifrování vyžadovat ke správě a ukládání kryptografických klíčů, které používáte pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="76c40-109">These forms of encryption require you to manage and store the cryptographic keys you use for encryption.</span></span> <span data-ttu-id="76c40-110">Službu službou Azure Key Vault (AZURE) slouží k vylepšení zabezpečení a správu tyto klíče v umístění zabezpečené a vysoce dostupné.</span><span class="sxs-lookup"><span data-stu-id="76c40-110">The Azure Key Vault (AKV) service is designed to improve the security and management of these keys in a secure and highly available location.</span></span> <span data-ttu-id="76c40-111">[Konektor služby serveru SQL](http://www.microsoft.com/download/details.aspx?id=45344) umožňuje serveru SQL pro použití těchto klíčů z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="76c40-111">The [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) enables SQL Server to use these keys from Azure Key Vault.</span></span>

<span data-ttu-id="76c40-112">Pokud jste s místním systémem SQL Server počítačů existuje jsou [kroky, pomocí kterých můžete pro přístup k Azure Key Vault z vašeho místního počítače systému SQL Server](https://msdn.microsoft.com/library/dn198405.aspx).</span><span class="sxs-lookup"><span data-stu-id="76c40-112">If you running SQL Server with on-premises machines, there are [steps you can follow to access Azure Key Vault from your on-premises SQL Server machine](https://msdn.microsoft.com/library/dn198405.aspx).</span></span> <span data-ttu-id="76c40-113">Ale pro SQL Server ve virtuálních počítačích Azure, můžete ušetřit čas pomocí *Azure Key Vault integrace* funkce.</span><span class="sxs-lookup"><span data-stu-id="76c40-113">But for SQL Server in Azure VMs, you can save time by using the *Azure Key Vault Integration* feature.</span></span>

<span data-ttu-id="76c40-114">Pokud je tato funkce povolena, automaticky se nainstaluje konektor serveru SQL, nakonfiguruje zprostředkovatele EKM. pro přístup k Azure Key Vault a vytvoří pověření umožňují přístup k trezoru.</span><span class="sxs-lookup"><span data-stu-id="76c40-114">When this feature is enabled, it automatically installs the SQL Server Connector, configures the EKM provider to access Azure Key Vault, and creates the credential to allow you to access your vault.</span></span> <span data-ttu-id="76c40-115">Pokud zvážení kroky v dokumentaci k výše uvedených v místě, uvidíte, že tato funkce automatizuje kroky 2 a 3.</span><span class="sxs-lookup"><span data-stu-id="76c40-115">If you looked at the steps in the previously mentioned on-premises documentation, you can see that this feature automates steps 2 and 3.</span></span> <span data-ttu-id="76c40-116">Jediné, co by se stále potřeba udělat ručně je vytvoření trezoru klíčů a klíče.</span><span class="sxs-lookup"><span data-stu-id="76c40-116">The only thing you would still need to do manually is to create the key vault and keys.</span></span> <span data-ttu-id="76c40-117">Z tohoto místa se automatizované celé nastavení virtuálního počítače SQL.</span><span class="sxs-lookup"><span data-stu-id="76c40-117">From there, the entire setup of your SQL VM is automated.</span></span> <span data-ttu-id="76c40-118">Po dokončení této instalace této funkce můžete spustit příkazů T-SQL zahájíte šifrování databáze nebo zálohy běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="76c40-118">Once this feature has completed this setup, you can execute T-SQL statements to begin encrypting your databases or backups as you normally would.</span></span>

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a><span data-ttu-id="76c40-119">Povolení a konfigurace integrace se službou AZURE</span><span class="sxs-lookup"><span data-stu-id="76c40-119">Enabling and configuring AKV integration</span></span>
<span data-ttu-id="76c40-120">Můžete povolit integrace se službou AZURE při zřizování nebo ho nakonfigurovat pro existující virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="76c40-120">You can enable AKV integration during provisioning or configure it for existing VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="76c40-121">Nové virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="76c40-121">New VMs</span></span>
<span data-ttu-id="76c40-122">Pokud zřizujete nového virtuálního počítače systému SQL Server s Resource Managerem, portál Azure poskytuje krok k povolení integrace se službou Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="76c40-122">If you are provisioning a new SQL Server virtual machine with Resource Manager, the Azure portal provides a step to enable Azure Key Vault integration.</span></span> <span data-ttu-id="76c40-123">Funkce Azure Key Vault je dostupná pouze pro Enterprise, Developer a zkušební edice systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="76c40-123">The Azure Key Vault feature is available only for the Enterprise, Developer and Evaluation Editions of SQL Server.</span></span>

![Integrace se službou Azure Key Vault pro SQL](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

<span data-ttu-id="76c40-125">Podrobný návod k zřizování, najdete v části [zřízení virtuálního počítače s SQL serverem na portálu Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="76c40-125">For a detailed walkthrough of provisioning, see [Provision a SQL Server virtual machine in the Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="76c40-126">Existující virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="76c40-126">Existing VMs</span></span>
<span data-ttu-id="76c40-127">Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="76c40-127">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="76c40-128">Vyberte **konfigurace systému SQL Server** části **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="76c40-128">Then select the **SQL Server configuration** section of the **Settings** blade.</span></span>

![Integrace se službou AZURE SQL pro existující virtuální počítače](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

<span data-ttu-id="76c40-130">V **konfigurace systému SQL Server** okně klikněte **upravit** tlačítko v části Integrace automatizované Key Vault.</span><span class="sxs-lookup"><span data-stu-id="76c40-130">In the **SQL Server configuration** blade, click the **Edit** button in the Automated Key Vault integration section.</span></span>

![Konfigurace integrace se službou AZURE SQL pro existující virtuální počítače](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

<span data-ttu-id="76c40-132">Po dokončení klikněte **OK** tlačítko v dolní části **konfigurace systému SQL Server** okno a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="76c40-132">When finished, click the **OK** button on the bottom of the **SQL Server configuration** blade to save your changes.</span></span>

> [!NOTE]
> <span data-ttu-id="76c40-133">Můžete také nakonfigurovat integrace se službou AZURE pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="76c40-133">You can also configure AKV integration using a template.</span></span> <span data-ttu-id="76c40-134">Další informace najdete v tématu [šablony Azure rychlý start pro integraci Azure Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span><span class="sxs-lookup"><span data-stu-id="76c40-134">For more information, see [Azure quickstart template for Azure Key Vault integration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span></span>
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

