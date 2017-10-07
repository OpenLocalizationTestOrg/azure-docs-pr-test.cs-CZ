---
title: "aaaDeploy svítilna na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Kurz – zásobník svítilna hello instalace na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: a3d0ecb3277f15bd0a2fdc0d85b738a760e68865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a><span data-ttu-id="0fbf5-103">Nainstalujte svítilna webový server na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="0fbf5-103">Install a LAMP web server on an Azure VM</span></span>
<span data-ttu-id="0fbf5-104">Tento článek vás provede jak toodeploy platformě Apache webový server, MySQL a PHP (hello svítilna stack) na virtuálního počítače s Ubuntu v Azure.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="0fbf5-105">Pokud dáváte přednost hello NGINX webový server, najdete v části hello [LEMP zásobníku](tutorial-lemp-stack.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-105">If you prefer hello NGINX web server, see hello [LEMP stack](tutorial-lemp-stack.md) tutorial.</span></span> <span data-ttu-id="0fbf5-106">toosee hello svítilna serveru v akci, můžete volitelně nainstalovat a nakonfigurovat web WordPress.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-106">toosee hello LAMP server in action, you can optionally install and configure a WordPress site.</span></span> <span data-ttu-id="0fbf5-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="0fbf5-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0fbf5-108">Vytvoření virtuálního počítače s Ubuntu (hello "L" v zásobníku svítilna hello)</span><span class="sxs-lookup"><span data-stu-id="0fbf5-108">Create an Ubuntu VM (hello 'L' in hello LAMP stack)</span></span>
> * <span data-ttu-id="0fbf5-109">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="0fbf5-109">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="0fbf5-110">Instalace Apache, MySQL a PHP</span><span class="sxs-lookup"><span data-stu-id="0fbf5-110">Install Apache, MySQL, and PHP</span></span>
> * <span data-ttu-id="0fbf5-111">Ověření instalace a konfigurace</span><span class="sxs-lookup"><span data-stu-id="0fbf5-111">Verify installation and configuration</span></span>
> * <span data-ttu-id="0fbf5-112">Instalace aplikace WordPress na serveru svítilna hello</span><span class="sxs-lookup"><span data-stu-id="0fbf5-112">Install WordPress on hello LAMP server</span></span>


<span data-ttu-id="0fbf5-113">Další informace o hello svítilna zásobníku, včetně doporučení pro produkční prostředí, najdete v části hello [Ubuntu dokumentaci](https://help.ubuntu.com/community/ApacheMySQLPHP).</span><span class="sxs-lookup"><span data-stu-id="0fbf5-113">For more on hello LAMP stack, including recommendations for a production environment, see hello [Ubuntu documentation](https://help.ubuntu.com/community/ApacheMySQLPHP).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0fbf5-114">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="0fbf5-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="0fbf5-116">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0fbf5-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a><span data-ttu-id="0fbf5-117">Instalace Apache, MySQL a PHP</span><span class="sxs-lookup"><span data-stu-id="0fbf5-117">Install Apache, MySQL, and PHP</span></span>

<span data-ttu-id="0fbf5-118">Spusťte následující příkaz tooupdate Ubuntu balíček zdroje hello a nainstalujte Apache, MySQL a PHP.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-118">Run hello following command tooupdate Ubuntu package sources and install Apache, MySQL, and PHP.</span></span> <span data-ttu-id="0fbf5-119">Všimněte si hello šipka nahoru (^) na konci hello hello příkazu.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-119">Note hello caret (^) at hello end of hello command.</span></span>


```bash
sudo apt update && sudo apt install lamp-server^
```



<span data-ttu-id="0fbf5-120">Jste výzvami tooinstall hello balíčky a další závislosti.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-120">You are prompted tooinstall hello packages and other dependencies.</span></span> <span data-ttu-id="0fbf5-121">Po zobrazení výzvy nastavte kořenové heslo pro MySQL a potom toocontinue [Enter].</span><span class="sxs-lookup"><span data-stu-id="0fbf5-121">When prompted, set a root password for MySQL, and then [Enter] toocontinue.</span></span> <span data-ttu-id="0fbf5-122">Postupujte podle zbývajících pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-122">Follow hello remaining prompts.</span></span> <span data-ttu-id="0fbf5-123">Tento proces nainstaluje hello minimální požadované PHP rozšíření potřeby toouse PHP s MySQL.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-123">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![Stránka MySQL kořenového hesla][1]

## <a name="verify-installation-and-configuration"></a><span data-ttu-id="0fbf5-125">Ověření instalace a konfigurace</span><span class="sxs-lookup"><span data-stu-id="0fbf5-125">Verify installation and configuration</span></span>


### <a name="apache"></a><span data-ttu-id="0fbf5-126">Apache</span><span class="sxs-lookup"><span data-stu-id="0fbf5-126">Apache</span></span>

<span data-ttu-id="0fbf5-127">Zkontrolujte verzi hello Apache s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0fbf5-127">Check hello version of Apache with hello following command:</span></span>
```bash
apache2 -v
```

<span data-ttu-id="0fbf5-128">S Apache nainstalovaná a port 80 otevřete tooyour virtuálních počítačů, hello webový server je nyní přístupná z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-128">With Apache installed, and port 80 open tooyour VM, hello web server can now be accessed from hello internet.</span></span> <span data-ttu-id="0fbf5-129">hello tooview Apache2 Ubuntu výchozí stránka otevřete webový prohlížeč a zadejte hello veřejnou IP adresu hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-129">tooview hello Apache2 Ubuntu Default Page, open a web browser, and enter hello public IP address of hello VM.</span></span> <span data-ttu-id="0fbf5-130">Použijte hello veřejnou IP adresu použitou tooSSH toohello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="0fbf5-130">Use hello public IP address you used tooSSH toohello VM:</span></span>

![Apache výchozí stránka][3]


### <a name="mysql"></a><span data-ttu-id="0fbf5-132">MySQL</span><span class="sxs-lookup"><span data-stu-id="0fbf5-132">MySQL</span></span>

<span data-ttu-id="0fbf5-133">Zkontrolujte verzi hello MySQL s hello následující příkaz (Všimněte si, velké hello `V` parametr):</span><span class="sxs-lookup"><span data-stu-id="0fbf5-133">Check hello version of MySQL with hello following command (note hello capital `V` parameter):</span></span>

```bash
msql -V
```

<span data-ttu-id="0fbf5-134">Doporučujeme spustit následující skript toohelp zabezpečené hello instalaci MySQL hello:</span><span class="sxs-lookup"><span data-stu-id="0fbf5-134">We recommend running hello following script toohelp secure hello installation of MySQL:</span></span>

```bash
mysql_secure_installation
```

<span data-ttu-id="0fbf5-135">Zadejte své heslo kořenové MySQL a konfigurovat nastavení zabezpečení hello pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-135">Enter your MySQL root password, and configure hello security settings for your environment.</span></span>

<span data-ttu-id="0fbf5-136">Pokud chcete toocreate databázi MySQL, přidat uživatele nebo změnit nastavení konfigurace, tooMySQL přihlášení:</span><span class="sxs-lookup"><span data-stu-id="0fbf5-136">If you want toocreate a MySQL database, add users, or change configuration settings, login tooMySQL:</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="0fbf5-137">Až budete hotoví, ukončete hello mysql řádku zadáním `\q`.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-137">When done, exit hello mysql prompt by typing `\q`.</span></span>

### <a name="php"></a><span data-ttu-id="0fbf5-138">PHP</span><span class="sxs-lookup"><span data-stu-id="0fbf5-138">PHP</span></span>

<span data-ttu-id="0fbf5-139">Zkontrolujte hello verzi PHP s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0fbf5-139">Check hello version of PHP with hello following command:</span></span>

```bash
php -v
```

<span data-ttu-id="0fbf5-140">Pokud chcete další tootest, vytvořte rychlé tooview PHP informace o stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-140">If you want tootest further, create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="0fbf5-141">Hello následující příkaz vytvoří stránku s informacemi o PHP hello:</span><span class="sxs-lookup"><span data-stu-id="0fbf5-141">hello following command creates hello PHP info page:</span></span>

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

<span data-ttu-id="0fbf5-142">Nyní můžete zkontrolovat, stránku s informacemi o PHP hello, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-142">Now you can check hello PHP info page you created.</span></span> <span data-ttu-id="0fbf5-143">Otevřete prohlížeč a přejděte příliš`http://yourPublicIPAddress/info.php`.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-143">Open a browser and go too`http://yourPublicIPAddress/info.php`.</span></span> <span data-ttu-id="0fbf5-144">Nahraďte hello veřejnou IP adresu vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-144">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="0fbf5-145">Měl by vypadat podobně jako toothis bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-145">It should look similar toothis image.</span></span>

![Stránka informace o PHP][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a><span data-ttu-id="0fbf5-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fbf5-147">Next steps</span></span>

<span data-ttu-id="0fbf5-148">V tomto kurzu jste nasadili server svítilna v Azure.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-148">In this tutorial, you deployed a LAMP server in Azure.</span></span> <span data-ttu-id="0fbf5-149">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="0fbf5-149">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0fbf5-150">Vytvoření virtuálního počítače s Ubuntu</span><span class="sxs-lookup"><span data-stu-id="0fbf5-150">Create an Ubuntu VM</span></span>
> * <span data-ttu-id="0fbf5-151">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="0fbf5-151">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="0fbf5-152">Instalace Apache, MySQL a PHP</span><span class="sxs-lookup"><span data-stu-id="0fbf5-152">Install Apache, MySQL, and PHP</span></span>
> * <span data-ttu-id="0fbf5-153">Ověření instalace a konfigurace</span><span class="sxs-lookup"><span data-stu-id="0fbf5-153">Verify installation and configuration</span></span>
> * <span data-ttu-id="0fbf5-154">Instalace aplikace WordPress na serveru svítilna hello</span><span class="sxs-lookup"><span data-stu-id="0fbf5-154">Install WordPress on hello LAMP server</span></span>

<span data-ttu-id="0fbf5-155">Jak zálohy další kurz toolearn toohello toosecure webových serverů s certifikáty protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="0fbf5-155">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0fbf5-156">Zabezpečení webového serveru pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="0fbf5-156">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png