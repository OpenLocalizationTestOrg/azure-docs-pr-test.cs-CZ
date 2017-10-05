---
title: "Navrhnout první databáze Azure pro databázi MySQL. portálu Azure | Microsoft Docs"
description: "Tento kurz vysvětluje, jak vytvořit a spravovat Azure databáze MySQL server a databáze pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: c7b76cacbdc4e483353f64cc4e50c974867bb5b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="c7bfb-103">Navrhnout první databáze Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="c7bfb-103">Design your first Azure Database for MySQL database</span></span>
<span data-ttu-id="c7bfb-104">Azure Database for MySQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze MySQL v cloudu.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-104">Azure Database for MySQL is a managed service that enables you to run, manage, and scale highly available MySQL databases in the cloud.</span></span> <span data-ttu-id="c7bfb-105">Pomocí portálu Azure, můžete snadno spravovat váš server a návrhu databáze.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-105">Using the Azure portal, you can easily manage your server and design a database.</span></span>

<span data-ttu-id="c7bfb-106">V tomto kurzu pomocí portálu Azure další postup:</span><span class="sxs-lookup"><span data-stu-id="c7bfb-106">In this tutorial, you use the Azure portal to learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c7bfb-107">Vytvoření Azure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="c7bfb-107">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="c7bfb-108">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="c7bfb-108">Configure the server firewall</span></span>
> * <span data-ttu-id="c7bfb-109">Vytvořit databázi pomocí nástroje příkazového řádku mysql</span><span class="sxs-lookup"><span data-stu-id="c7bfb-109">Use mysql command-line tool to create a database</span></span>
> * <span data-ttu-id="c7bfb-110">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="c7bfb-110">Load sample data</span></span>
> * <span data-ttu-id="c7bfb-111">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="c7bfb-111">Query data</span></span>
> * <span data-ttu-id="c7bfb-112">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="c7bfb-112">Update data</span></span>
> * <span data-ttu-id="c7bfb-113">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="c7bfb-113">Restore data</span></span>

## <a name="sign-in-to-the-azure-portal"></a><span data-ttu-id="c7bfb-114">Přihlášení k webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c7bfb-114">Sign in to the Azure portal</span></span>
<span data-ttu-id="c7bfb-115">Otevřete Oblíbené webový prohlížeč a přejděte [portálu Microsoft Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c7bfb-115">Open your favorite web browser, and visit the [Microsoft Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="c7bfb-116">Zadejte přihlašovací údaje pro přihlášení k portálu.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-116">Enter your credentials to sign in to the portal.</span></span> <span data-ttu-id="c7bfb-117">Výchozím zobrazením je váš řídicí panel služby.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-117">The default view is your service dashboard.</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="c7bfb-118">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="c7bfb-118">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="c7bfb-119">Server Azure Database for MySQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c7bfb-119">An Azure Database for MySQL server is created with a defined set of [compute and storage resources](./concepts-compute-unit-and-storage.md).</span></span> <span data-ttu-id="c7bfb-120">Server se vytvoří v rámci [skupiny prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="c7bfb-120">The server is created within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

1. <span data-ttu-id="c7bfb-121">Přejděte na **databáze** > **Azure databáze pro databázi MySQL**.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-121">Navigate to **Databases** > **Azure Database for MySQL**.</span></span> <span data-ttu-id="c7bfb-122">Pokud nemůžete najít MySQL serveru v části **databáze** kategorii, klikněte na tlačítko **zobrazit všechny** zobrazíte všechny služby k dispozici databáze.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-122">If you cannot find MySQL Server under **Databases** category, click **See all** to show all available database services.</span></span> <span data-ttu-id="c7bfb-123">Můžete také zadat **Azure Database pro databázi MySQL** do vyhledávacího pole rychle najít službu.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-123">You can also type **Azure Database for MySQL** in the search box to quickly find the service.</span></span>
<span data-ttu-id="c7bfb-124">![2-1, přejděte na MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span><span class="sxs-lookup"><span data-stu-id="c7bfb-124">![2-1 Navigate to MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span></span>

2. <span data-ttu-id="c7bfb-125">Klikněte na tlačítko **Azure Database pro databázi MySQL** dlaždici a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-125">Click **Azure Database for MySQL** tile, and then click **Create**.</span></span>

<span data-ttu-id="c7bfb-126">V našem příkladu vyplňte Azure databáze MySQL formuláře s následujícími informacemi:</span><span class="sxs-lookup"><span data-stu-id="c7bfb-126">In our example, fill out the Azure Database for MySQL form with the following information:</span></span>

| <span data-ttu-id="c7bfb-127">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="c7bfb-127">**Setting**</span></span> | <span data-ttu-id="c7bfb-128">**Navrhovaná hodnota**</span><span class="sxs-lookup"><span data-stu-id="c7bfb-128">**Suggested value**</span></span> | <span data-ttu-id="c7bfb-129">**Popis pole**</span><span class="sxs-lookup"><span data-stu-id="c7bfb-129">**Field Description**</span></span> |
|---|---|---|
| <span data-ttu-id="c7bfb-130">*Název serveru*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-130">*Server name*</span></span> | <span data-ttu-id="c7bfb-131">myserver4demo</span><span class="sxs-lookup"><span data-stu-id="c7bfb-131">myserver4demo</span></span>  | <span data-ttu-id="c7bfb-132">Název serveru musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-132">Server name has to be globally unique.</span></span> |
| <span data-ttu-id="c7bfb-133">*Předplatné*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-133">*Subscription*</span></span> | <span data-ttu-id="c7bfb-134">mysubscription</span><span class="sxs-lookup"><span data-stu-id="c7bfb-134">mysubscription</span></span> | <span data-ttu-id="c7bfb-135">Vyberte vaše předplatné z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-135">Select your subscription from the drop-down.</span></span> |
| <span data-ttu-id="c7bfb-136">*Skupina prostředků*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-136">*Resource group*</span></span> | <span data-ttu-id="c7bfb-137">myresourcegroup</span><span class="sxs-lookup"><span data-stu-id="c7bfb-137">myresourcegroup</span></span> | <span data-ttu-id="c7bfb-138">Vytvořte skupinu prostředků nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-138">Create a resource group or use an existing one.</span></span> |
| <span data-ttu-id="c7bfb-139">*Přihlašovací jméno správce serveru*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-139">*Server admin login*</span></span> | <span data-ttu-id="c7bfb-140">myadmin</span><span class="sxs-lookup"><span data-stu-id="c7bfb-140">myadmin</span></span> | <span data-ttu-id="c7bfb-141">Instalační program název účtu správce.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-141">Setup admin account name.</span></span> |
| <span data-ttu-id="c7bfb-142">*Heslo*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-142">*Password*</span></span> |  | <span data-ttu-id="c7bfb-143">Nastavte silné heslo účtu správce.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-143">Set a strong admin account password.</span></span> |
| <span data-ttu-id="c7bfb-144">*Potvrdit heslo*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-144">*Confirm password*</span></span> |  | <span data-ttu-id="c7bfb-145">Potvrďte heslo účtu správce.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-145">Confirm the admin account password.</span></span> |
| <span data-ttu-id="c7bfb-146">*Umístění*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-146">*Location*</span></span> |  | <span data-ttu-id="c7bfb-147">Vyberte dostupnou oblast.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-147">Select an available region.</span></span> |
| <span data-ttu-id="c7bfb-148">*Verze*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-148">*Version*</span></span> | <span data-ttu-id="c7bfb-149">5.7</span><span class="sxs-lookup"><span data-stu-id="c7bfb-149">5.7</span></span> | <span data-ttu-id="c7bfb-150">Zvolte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-150">Choose the latest version.</span></span> |
| <span data-ttu-id="c7bfb-151">*Konfigurovat výkon*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-151">*Configure performance*</span></span> | <span data-ttu-id="c7bfb-152">Základní, 50 výpočetní jednotky, 50 GB</span><span class="sxs-lookup"><span data-stu-id="c7bfb-152">Basic, 50 compute units, 50 GB</span></span>  | <span data-ttu-id="c7bfb-153">Zvolte **Cenovou úroveň**, **Výpočetní jednotky**, **Úložiště (GB)** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-153">Choose **Pricing tier**, **Compute Units**, **Storage (GB)**, and then click **OK**.</span></span> |
| <span data-ttu-id="c7bfb-154">*Připnout na řídicí panel*</span><span class="sxs-lookup"><span data-stu-id="c7bfb-154">*Pin to Dashboard*</span></span> | <span data-ttu-id="c7bfb-155">Zaškrtnout</span><span class="sxs-lookup"><span data-stu-id="c7bfb-155">Check</span></span> | <span data-ttu-id="c7bfb-156">Doporučuje se toto zaškrtávací políčko zaškrtnout, abyste později server snadno našli.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-156">Recommended to check this box so you may find the server easily later on</span></span> |
<span data-ttu-id="c7bfb-157">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-157">Then, click **Create**.</span></span> <span data-ttu-id="c7bfb-158">Po jedné až dvou minutách bude server Azure Database for MySQL spuštěný v cloudu.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-158">In a minute or two, a new Azure Database for MySQL server is running in the cloud.</span></span> <span data-ttu-id="c7bfb-159">Můžete kliknout na **oznámení** tlačítka na panelu nástrojů ke sledování procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-159">You can click **Notifications** button on the toolbar to monitor the deployment process.</span></span>

## <a name="configure-firewall"></a><span data-ttu-id="c7bfb-160">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="c7bfb-160">Configure firewall</span></span>
<span data-ttu-id="c7bfb-161">Azure databáze pro databázi MySQL jsou chráněné bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-161">Azure Databases for MySQL are protected by a firewall.</span></span> <span data-ttu-id="c7bfb-162">Ve výchozím nastavení všechna připojení k serveru a databází uvnitř serveru odmítají.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-162">By default, all connections to the server and the databases inside the server are rejected.</span></span> <span data-ttu-id="c7bfb-163">Před připojením ke službě Azure Database pro databázi MySQL poprvé, nakonfigurujte bránu firewall, přidejte klientský počítač veřejné síti IP adresu (nebo rozsah IP adres).</span><span class="sxs-lookup"><span data-stu-id="c7bfb-163">Before connecting to Azure Database for MySQL for the first time, configure the firewall to add the client machine's public network IP address (or IP address range).</span></span>

1. <span data-ttu-id="c7bfb-164">Klikněte na nově vytvořený serveru a pak klikněte na tlačítko **zabezpečení připojení**.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-164">Click your newly created server, and then click **Connection security**.</span></span>
   <span data-ttu-id="c7bfb-165">![Zabezpečení připojení 3-1](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span><span class="sxs-lookup"><span data-stu-id="c7bfb-165">![3-1 Connection security](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span></span>
2. <span data-ttu-id="c7bfb-166">Můžete **přidat Moje IP**, nebo můžete nakonfigurovat pravidla brány firewall v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-166">You can **Add My IP**, or configure firewall rules here.</span></span> <span data-ttu-id="c7bfb-167">Po vytvoření pravidel nezapomeňte kliknout na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-167">Remember to click **Save** after you have created the rules.</span></span>
<span data-ttu-id="c7bfb-168">Nyní můžete připojit k serveru pomocí nástroje příkazového řádku mysql nebo MySQL Workbench grafickým uživatelským rozhraním.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-168">You can now connect to the server using mysql command-line tool or MySQL Workbench GUI tool.</span></span>

> [!TIP]
> <span data-ttu-id="c7bfb-169">Azure databáze MySQL server pro komunikuje přes port 3306.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-169">Azure Database for MySQL server communicates over port 3306.</span></span> <span data-ttu-id="c7bfb-170">Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 3306 bránou firewall vaší sítě povolený.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-170">If you are trying to connect from within a corporate network, outbound traffic over port 3306 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="c7bfb-171">Pokud ano, můžete nemůže připojit k Azure MySQL serveru Pokud vaše IT oddělení otevře port 3306.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-171">If so, you cannot connect to Azure MySQL server unless your IT department opens port 3306.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="c7bfb-172">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="c7bfb-172">Get connection information</span></span>
<span data-ttu-id="c7bfb-173">Získat plně kvalifikovaný **název serveru** a **přihlašovací jméno správce pro Server** pro vaši databázi Azure pro server databáze MySQL na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-173">Get the fully qualified **Server name** and **Server admin login name** for your Azure Database for MySQL server from the Azure portal.</span></span> <span data-ttu-id="c7bfb-174">Použijete název plně kvalifikovaný serveru k připojení k serveru pomocí nástroje příkazového řádku mysql.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-174">You use the fully qualified server name to connect to your server using mysql command-line tool.</span></span> 

1. <span data-ttu-id="c7bfb-175">V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **všechny prostředky** z nabídky na levé straně, zadejte název a vyhledávání pro vaši databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-175">In [Azure portal](https://portal.azure.com/), click **All resources** from the left-hand menu, type the name, and search for your Azure Database for MySQL server.</span></span> <span data-ttu-id="c7bfb-176">Vyberte název serveru zobrazíte podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-176">Select the server name to view the details.</span></span>

2. <span data-ttu-id="c7bfb-177">V části nastavení klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-177">Under the Settings heading, click **Properties**.</span></span> <span data-ttu-id="c7bfb-178">Poznamenejte si **název serveru** a **PŘIHLAŠOVACÍ jméno pro SERVER správce**.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-178">Note down **SERVER NAME** and **SERVER ADMIN LOGIN NAME**.</span></span> <span data-ttu-id="c7bfb-179">Může klikněte na tlačítko Kopírovat vedle každého pole zkopírovat do schránky.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-179">You may click the copy button next to each field to copy to the clipboard.</span></span>
   <span data-ttu-id="c7bfb-180">![Vlastnosti serveru 4-2](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span><span class="sxs-lookup"><span data-stu-id="c7bfb-180">![4-2 server properties](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span></span>

<span data-ttu-id="c7bfb-181">V tomto příkladu je název serveru *myserver4demo.mysql.database.azure.com* a přihlašovací jméno správce serveru je *myadmin@myserver4demo*.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-181">In this example, the server name is *myserver4demo.mysql.database.azure.com*, and the server admin login is *myadmin@myserver4demo*.</span></span>

## <a name="connect-to-the-server-using-mysql"></a><span data-ttu-id="c7bfb-182">Připojit k serveru pomocí mysql</span><span class="sxs-lookup"><span data-stu-id="c7bfb-182">Connect to the server using mysql</span></span>
<span data-ttu-id="c7bfb-183">Použijte [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) k navázání připojení k serveru Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-183">Use [mysql command-line tool](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) to establish a connection to your Azure Database for MySQL server.</span></span> <span data-ttu-id="c7bfb-184">Nástroj příkazového řádku mysql můžete spustit z prostředí cloudové služby Azure v prohlížeči nebo vlastní počítače pomocí nástroje mysql nainstalované místně.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-184">You can run the mysql command-line tool from the Azure Cloud Shell in the browser or from your own machine using mysql tools installed locally.</span></span> <span data-ttu-id="c7bfb-185">Chcete-li spustit prostředí cloudu Azure, klikněte na tlačítko `Try It` tlačítko blok kódu v tomto článku, nebo navštívit portál Azure a klikněte na tlačítko `>_` ikonu v pravém horním panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-185">To launch the Azure Cloud Shell, click the `Try It` button on a code block in this article, or visit the Azure portal and click the `>_` icon in the top right toolbar.</span></span> 

<span data-ttu-id="c7bfb-186">Zadejte příkaz pro připojení:</span><span class="sxs-lookup"><span data-stu-id="c7bfb-186">Type the command to connect:</span></span>
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="c7bfb-187">Vytvoření prázdné databáze</span><span class="sxs-lookup"><span data-stu-id="c7bfb-187">Create a blank database</span></span>
<span data-ttu-id="c7bfb-188">Po připojení k serveru vytvořte prázdnou databázi pro práci s.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-188">Once you’re connected to the server, create a blank database to work with.</span></span>
```sql
CREATE DATABASE mysampledb;
```

<span data-ttu-id="c7bfb-189">Do příkazového řádku spusťte následující příkaz k přepínači připojení k této nově vytvořené databáze:</span><span class="sxs-lookup"><span data-stu-id="c7bfb-189">At the prompt, run the following command to switch connection to this newly created database:</span></span>
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a><span data-ttu-id="c7bfb-190">Vytváření tabulek v databázi</span><span class="sxs-lookup"><span data-stu-id="c7bfb-190">Create tables in the database</span></span>
<span data-ttu-id="c7bfb-191">Teď, když víte, jak se připojit k databázi Azure pro databázi MySQL, jsme projít jak provést některé základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-191">Now that you know how to connect to the Azure Database for MySQL database, we can go over how to complete some basic tasks.</span></span>

<span data-ttu-id="c7bfb-192">Jsme nejprve vytvořit tabulku a načíst určitými daty.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-192">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="c7bfb-193">Umožňuje vytvořit tabulku, která ukládá informace o inventáři.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-193">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a><span data-ttu-id="c7bfb-194">Načtení dat do tabulky</span><span class="sxs-lookup"><span data-stu-id="c7bfb-194">Load data into the tables</span></span>
<span data-ttu-id="c7bfb-195">Teď, když máme tabulku, jsme do něj vložte některá data.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-195">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="c7bfb-196">V okně Otevřít příkazového řádku spusťte následující dotaz vložit některé řádky dat.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-196">At the open command prompt window, run the following query to insert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="c7bfb-197">Nyní máte dva řádky ukázkových dat do tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-197">Now you have two rows of sample data into the table you created earlier.</span></span>

## <a name="query-and-update-the-data-in-the-tables"></a><span data-ttu-id="c7bfb-198">Dotaz a aktualizovat data v tabulkách</span><span class="sxs-lookup"><span data-stu-id="c7bfb-198">Query and update the data in the tables</span></span>
<span data-ttu-id="c7bfb-199">Spustíte následující dotaz pro načtení informací z tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-199">Execute the following query to retrieve information from the database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="c7bfb-200">Můžete také aktualizovat data v tabulkách.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-200">You can also update the data in the tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="c7bfb-201">Načtení dat získá příslušným způsobem aktualizuje řádek.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-201">The row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a><span data-ttu-id="c7bfb-202">Obnovení databáze k dřívějšímu bodu v čase</span><span class="sxs-lookup"><span data-stu-id="c7bfb-202">Restore a database to a previous point in time</span></span>
<span data-ttu-id="c7bfb-203">Představte si, že omylem odstraněn k tabulce důležité databáze a nelze snadno obnovit data.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-203">Imagine you have accidentally deleted an important database table, and cannot recover the data easily.</span></span> <span data-ttu-id="c7bfb-204">Databáze pro databázi MySQL Azure umožňuje obnovit server k určitému bodu v čase, vytváření kopie databáze do nového serveru.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-204">Azure Database for MySQL allows you to restore the server to a point in time, creating a copy of the databases into new server.</span></span> <span data-ttu-id="c7bfb-205">Tento nový server můžete obnovit odstraněná data.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-205">You can use this new server to recover your deleted data.</span></span> <span data-ttu-id="c7bfb-206">Ukázka serveru bod před přidáním tabulky obnovit následující kroky.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-206">The following steps restore the sample server to a point before the table was added.</span></span>

1. <span data-ttu-id="c7bfb-207">Na portálu Azure vyhledejte Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-207">In the Azure portal, locate your Azure Database for MySQL.</span></span> <span data-ttu-id="c7bfb-208">Na **přehled** klikněte na tlačítko **obnovení** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-208">On the **Overview** page, click **Restore** on the toolbar.</span></span> <span data-ttu-id="c7bfb-209">Otevře se stránka obnovení.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-209">The Restore page opens.</span></span>

   ![10-1 obnovení databáze](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. <span data-ttu-id="c7bfb-211">Vyplňte **obnovení** formuláře se požadované informace.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-211">Fill out the **Restore** form with the required information.</span></span>
   
   ![Formulář obnovení 10-2](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - <span data-ttu-id="c7bfb-213">**Bod obnovení**: Vyberte bodu v čase, kterou chcete obnovit, v časovém rámci uvedené.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-213">**Restore point**: Select a point-in-time that you want to restore to, within the timeframe listed.</span></span> <span data-ttu-id="c7bfb-214">Zajistěte, aby převést vaše místní časové pásmo UTC.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-214">Make sure to convert your local timezone to UTC.</span></span>
   - <span data-ttu-id="c7bfb-215">**Obnovit na nový server**: Zadejte nový název serveru, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-215">**Restore to new server**: Provide a new server name you want to restore to.</span></span>
   - <span data-ttu-id="c7bfb-216">**Umístění**: oblast je stejný jako zdrojový server a nelze ji změnit.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-216">**Location**: The region is same as the source server, and cannot be changed.</span></span>
   - <span data-ttu-id="c7bfb-217">**Cenová úroveň**: cenová úroveň je stejný jako zdrojový server a nedá se změnit.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-217">**Pricing tier**: The pricing tier is the same as the source server, and cannot be changed.</span></span>
   
3. <span data-ttu-id="c7bfb-218">Klikněte na tlačítko **OK** a obnovte server do [obnovit k určitému bodu v čase](./howto-restore-server-portal.md) předtím, než tabulka byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-218">Click **OK** to restore the server to [restore to a point in time](./howto-restore-server-portal.md) before the table was deleted.</span></span> <span data-ttu-id="c7bfb-219">Obnovení serveru vytvoří novou kopii serveru od bodu v čase, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="c7bfb-219">Restoring a server creates a new copy of the server, as of the point in time you specify.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c7bfb-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7bfb-220">Next steps</span></span>
<span data-ttu-id="c7bfb-221">V tomto kurzu použijete Azure portálu na zjištěné postup:</span><span class="sxs-lookup"><span data-stu-id="c7bfb-221">In this tutorial, you use the Azure portal to learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c7bfb-222">Vytvoření Azure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="c7bfb-222">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="c7bfb-223">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="c7bfb-223">Configure the server firewall</span></span>
> * <span data-ttu-id="c7bfb-224">Vytvořit databázi pomocí nástroje příkazového řádku mysql</span><span class="sxs-lookup"><span data-stu-id="c7bfb-224">Use mysql command-line tool to create a database</span></span>
> * <span data-ttu-id="c7bfb-225">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="c7bfb-225">Load sample data</span></span>
> * <span data-ttu-id="c7bfb-226">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="c7bfb-226">Query data</span></span>
> * <span data-ttu-id="c7bfb-227">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="c7bfb-227">Update data</span></span>
> * <span data-ttu-id="c7bfb-228">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="c7bfb-228">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c7bfb-229">Postup připojení aplikace k databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="c7bfb-229">How to connect applications to Azure Database for MySQL</span></span>](./howto-connection-string.md)
