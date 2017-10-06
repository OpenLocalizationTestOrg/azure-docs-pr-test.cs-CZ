---
title: "aaaDesign vaše první Azure databáze pro databázi MySQL. portálu Azure | Microsoft Docs"
description: "Tento kurz vysvětluje, jak toocreate a správě Azure databáze MySQL serveru a databáze pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: 06dd952acc5356b8cccaf36917df1ff8db4f7139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="a6923-103">Navrhnout první databáze Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="a6923-103">Design your first Azure Database for MySQL database</span></span>
<span data-ttu-id="a6923-104">Azure databáze pro databázi MySQL je spravovaná služba, která vám umožní toorun, spravovat a škálování vysoce dostupné databáze MySQL v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-104">Azure Database for MySQL is a managed service that enables you toorun, manage, and scale highly available MySQL databases in hello cloud.</span></span> <span data-ttu-id="a6923-105">Pomocí hello portálu Azure, můžete snadno spravovat váš server a návrhu databáze.</span><span class="sxs-lookup"><span data-stu-id="a6923-105">Using hello Azure portal, you can easily manage your server and design a database.</span></span>

<span data-ttu-id="a6923-106">V tomto kurzu použijete hello Azure portálu toolearn jak na:</span><span class="sxs-lookup"><span data-stu-id="a6923-106">In this tutorial, you use hello Azure portal toolearn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6923-107">Vytvoření Azure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="a6923-107">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="a6923-108">Konfigurace brány firewall serveru hello</span><span class="sxs-lookup"><span data-stu-id="a6923-108">Configure hello server firewall</span></span>
> * <span data-ttu-id="a6923-109">Použijte nástroj příkazového řádku toocreate mysql databáze</span><span class="sxs-lookup"><span data-stu-id="a6923-109">Use mysql command-line tool toocreate a database</span></span>
> * <span data-ttu-id="a6923-110">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="a6923-110">Load sample data</span></span>
> * <span data-ttu-id="a6923-111">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="a6923-111">Query data</span></span>
> * <span data-ttu-id="a6923-112">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="a6923-112">Update data</span></span>
> * <span data-ttu-id="a6923-113">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="a6923-113">Restore data</span></span>

## <a name="sign-in-toohello-azure-portal"></a><span data-ttu-id="a6923-114">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a6923-114">Sign in toohello Azure portal</span></span>
<span data-ttu-id="a6923-115">Otevřete Oblíbené webový prohlížeč a přejděte hello [portálu Microsoft Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a6923-115">Open your favorite web browser, and visit hello [Microsoft Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="a6923-116">Zadejte vaše přihlašovací údaje toosign toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="a6923-116">Enter your credentials toosign in toohello portal.</span></span> <span data-ttu-id="a6923-117">Výchozí zobrazení Hello je váš řídicí panel služby.</span><span class="sxs-lookup"><span data-stu-id="a6923-117">hello default view is your service dashboard.</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="a6923-118">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="a6923-118">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="a6923-119">Server Azure Database for MySQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="a6923-119">An Azure Database for MySQL server is created with a defined set of [compute and storage resources](./concepts-compute-unit-and-storage.md).</span></span> <span data-ttu-id="a6923-120">Hello server je vytvořen v rámci [skupina prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="a6923-120">hello server is created within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

1. <span data-ttu-id="a6923-121">Přejděte příliš**databáze** > **Azure Database pro databázi MySQL**.</span><span class="sxs-lookup"><span data-stu-id="a6923-121">Navigate too**Databases** > **Azure Database for MySQL**.</span></span> <span data-ttu-id="a6923-122">Pokud nemůžete najít MySQL serveru v části **databáze** kategorii, klikněte na tlačítko **zobrazit všechny** tooshow všechny dostupné databáze služby.</span><span class="sxs-lookup"><span data-stu-id="a6923-122">If you cannot find MySQL Server under **Databases** category, click **See all** tooshow all available database services.</span></span> <span data-ttu-id="a6923-123">Můžete také zadat **Azure Database pro databázi MySQL** v tooquickly pole hledání hello najít službu hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-123">You can also type **Azure Database for MySQL** in hello search box tooquickly find hello service.</span></span>
<span data-ttu-id="a6923-124">![Navigovat tooMySQL 2-1](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span><span class="sxs-lookup"><span data-stu-id="a6923-124">![2-1 Navigate tooMySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span></span>

2. <span data-ttu-id="a6923-125">Klikněte na tlačítko **Azure Database pro databázi MySQL** dlaždici a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a6923-125">Click **Azure Database for MySQL** tile, and then click **Create**.</span></span>

<span data-ttu-id="a6923-126">V našem příkladu vyplňte hello databáze Azure pro formulář MySQL s hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="a6923-126">In our example, fill out hello Azure Database for MySQL form with hello following information:</span></span>

| <span data-ttu-id="a6923-127">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="a6923-127">**Setting**</span></span> | <span data-ttu-id="a6923-128">**Navrhovaná hodnota**</span><span class="sxs-lookup"><span data-stu-id="a6923-128">**Suggested value**</span></span> | <span data-ttu-id="a6923-129">**Popis pole**</span><span class="sxs-lookup"><span data-stu-id="a6923-129">**Field Description**</span></span> |
|---|---|---|
| <span data-ttu-id="a6923-130">*Název serveru*</span><span class="sxs-lookup"><span data-stu-id="a6923-130">*Server name*</span></span> | <span data-ttu-id="a6923-131">myserver4demo</span><span class="sxs-lookup"><span data-stu-id="a6923-131">myserver4demo</span></span>  | <span data-ttu-id="a6923-132">Název serveru má toobe globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="a6923-132">Server name has toobe globally unique.</span></span> |
| <span data-ttu-id="a6923-133">*Předplatné*</span><span class="sxs-lookup"><span data-stu-id="a6923-133">*Subscription*</span></span> | <span data-ttu-id="a6923-134">mysubscription</span><span class="sxs-lookup"><span data-stu-id="a6923-134">mysubscription</span></span> | <span data-ttu-id="a6923-135">Vyberte své předplatné z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-135">Select your subscription from hello drop-down.</span></span> |
| <span data-ttu-id="a6923-136">*Skupina prostředků*</span><span class="sxs-lookup"><span data-stu-id="a6923-136">*Resource group*</span></span> | <span data-ttu-id="a6923-137">myresourcegroup</span><span class="sxs-lookup"><span data-stu-id="a6923-137">myresourcegroup</span></span> | <span data-ttu-id="a6923-138">Vytvořte skupinu prostředků nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="a6923-138">Create a resource group or use an existing one.</span></span> |
| <span data-ttu-id="a6923-139">*Přihlašovací jméno správce serveru*</span><span class="sxs-lookup"><span data-stu-id="a6923-139">*Server admin login*</span></span> | <span data-ttu-id="a6923-140">myadmin</span><span class="sxs-lookup"><span data-stu-id="a6923-140">myadmin</span></span> | <span data-ttu-id="a6923-141">Instalační program název účtu správce.</span><span class="sxs-lookup"><span data-stu-id="a6923-141">Setup admin account name.</span></span> |
| <span data-ttu-id="a6923-142">*Heslo*</span><span class="sxs-lookup"><span data-stu-id="a6923-142">*Password*</span></span> |  | <span data-ttu-id="a6923-143">Nastavte silné heslo účtu správce.</span><span class="sxs-lookup"><span data-stu-id="a6923-143">Set a strong admin account password.</span></span> |
| <span data-ttu-id="a6923-144">*Potvrdit heslo*</span><span class="sxs-lookup"><span data-stu-id="a6923-144">*Confirm password*</span></span> |  | <span data-ttu-id="a6923-145">Potvrďte heslo pro účet správce hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-145">Confirm hello admin account password.</span></span> |
| <span data-ttu-id="a6923-146">*Umístění*</span><span class="sxs-lookup"><span data-stu-id="a6923-146">*Location*</span></span> |  | <span data-ttu-id="a6923-147">Vyberte dostupnou oblast.</span><span class="sxs-lookup"><span data-stu-id="a6923-147">Select an available region.</span></span> |
| <span data-ttu-id="a6923-148">*Verze*</span><span class="sxs-lookup"><span data-stu-id="a6923-148">*Version*</span></span> | <span data-ttu-id="a6923-149">5.7</span><span class="sxs-lookup"><span data-stu-id="a6923-149">5.7</span></span> | <span data-ttu-id="a6923-150">Vyberte nejnovější verzi hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-150">Choose hello latest version.</span></span> |
| <span data-ttu-id="a6923-151">*Konfigurovat výkon*</span><span class="sxs-lookup"><span data-stu-id="a6923-151">*Configure performance*</span></span> | <span data-ttu-id="a6923-152">Základní, 50 výpočetní jednotky, 50 GB</span><span class="sxs-lookup"><span data-stu-id="a6923-152">Basic, 50 compute units, 50 GB</span></span>  | <span data-ttu-id="a6923-153">Zvolte **Cenovou úroveň**, **Výpočetní jednotky**, **Úložiště (GB)** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6923-153">Choose **Pricing tier**, **Compute Units**, **Storage (GB)**, and then click **OK**.</span></span> |
| <span data-ttu-id="a6923-154">*TooDashboard PIN kód*</span><span class="sxs-lookup"><span data-stu-id="a6923-154">*Pin tooDashboard*</span></span> | <span data-ttu-id="a6923-155">Zaškrtnout</span><span class="sxs-lookup"><span data-stu-id="a6923-155">Check</span></span> | <span data-ttu-id="a6923-156">Doporučená toocheck toto políčko, a může se stát hello server později snadno na</span><span class="sxs-lookup"><span data-stu-id="a6923-156">Recommended toocheck this box so you may find hello server easily later on</span></span> |
<span data-ttu-id="a6923-157">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a6923-157">Then, click **Create**.</span></span> <span data-ttu-id="a6923-158">V minutu nebo dvě nové databáze Azure pro MySQL server běží v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-158">In a minute or two, a new Azure Database for MySQL server is running in hello cloud.</span></span> <span data-ttu-id="a6923-159">Můžete kliknout na **oznámení** tlačítko v procesu nasazení hello nástrojů toomonitor hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-159">You can click **Notifications** button on hello toolbar toomonitor hello deployment process.</span></span>

## <a name="configure-firewall"></a><span data-ttu-id="a6923-160">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="a6923-160">Configure firewall</span></span>
<span data-ttu-id="a6923-161">Azure databáze pro databázi MySQL jsou chráněné bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="a6923-161">Azure Databases for MySQL are protected by a firewall.</span></span> <span data-ttu-id="a6923-162">Ve výchozím nastavení všechna připojení toohello server hello databáze a uvnitř hello server odmítají.</span><span class="sxs-lookup"><span data-stu-id="a6923-162">By default, all connections toohello server and hello databases inside hello server are rejected.</span></span> <span data-ttu-id="a6923-163">Před připojením tooAzure databáze pro databázi MySQL hello poprvé, nakonfigurujte veřejnou síť IP adresu hello brány firewall tooadd hello klientské počítače (nebo rozsah IP adres).</span><span class="sxs-lookup"><span data-stu-id="a6923-163">Before connecting tooAzure Database for MySQL for hello first time, configure hello firewall tooadd hello client machine's public network IP address (or IP address range).</span></span>

1. <span data-ttu-id="a6923-164">Klikněte na nově vytvořený serveru a pak klikněte na tlačítko **zabezpečení připojení**.</span><span class="sxs-lookup"><span data-stu-id="a6923-164">Click your newly created server, and then click **Connection security**.</span></span>
   <span data-ttu-id="a6923-165">![Zabezpečení připojení 3-1](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span><span class="sxs-lookup"><span data-stu-id="a6923-165">![3-1 Connection security](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span></span>
2. <span data-ttu-id="a6923-166">Můžete **přidat Moje IP**, nebo můžete nakonfigurovat pravidla brány firewall v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="a6923-166">You can **Add My IP**, or configure firewall rules here.</span></span> <span data-ttu-id="a6923-167">Mějte na paměti, tooclick **Uložit** po vytvoření pravidla hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-167">Remember tooclick **Save** after you have created hello rules.</span></span>
<span data-ttu-id="a6923-168">Teď se můžete připojit server toohello pomocí nástroje příkazového řádku mysql nebo MySQL Workbench grafickým uživatelským rozhraním.</span><span class="sxs-lookup"><span data-stu-id="a6923-168">You can now connect toohello server using mysql command-line tool or MySQL Workbench GUI tool.</span></span>

> [!TIP]
> <span data-ttu-id="a6923-169">Azure databáze MySQL server pro komunikuje přes port 3306.</span><span class="sxs-lookup"><span data-stu-id="a6923-169">Azure Database for MySQL server communicates over port 3306.</span></span> <span data-ttu-id="a6923-170">Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 3306 nemusí mít povolený bránou firewall vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="a6923-170">If you are trying tooconnect from within a corporate network, outbound traffic over port 3306 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="a6923-171">Pokud ano, pokud vaše IT oddělení otevře port 3306 nemůžete připojit tooAzure MySQL server.</span><span class="sxs-lookup"><span data-stu-id="a6923-171">If so, you cannot connect tooAzure MySQL server unless your IT department opens port 3306.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="a6923-172">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="a6923-172">Get connection information</span></span>
<span data-ttu-id="a6923-173">Plně kvalifikovaný hello Get **název serveru** a **přihlašovací jméno správce pro Server** pro vaši databázi Azure pro server databáze MySQL z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a6923-173">Get hello fully qualified **Server name** and **Server admin login name** for your Azure Database for MySQL server from hello Azure portal.</span></span> <span data-ttu-id="a6923-174">Použijete hello plně kvalifikovaný název tooconnect tooyour serveru pomocí nástroje příkazového řádku mysql.</span><span class="sxs-lookup"><span data-stu-id="a6923-174">You use hello fully qualified server name tooconnect tooyour server using mysql command-line tool.</span></span> 

1. <span data-ttu-id="a6923-175">V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **všechny prostředky** z nabídky na levé straně hello, název typu hello a vyhledávání pro vaši databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="a6923-175">In [Azure portal](https://portal.azure.com/), click **All resources** from hello left-hand menu, type hello name, and search for your Azure Database for MySQL server.</span></span> <span data-ttu-id="a6923-176">Vyberte hello serveru název tooview hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a6923-176">Select hello server name tooview hello details.</span></span>

2. <span data-ttu-id="a6923-177">V části hello nastavení klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="a6923-177">Under hello Settings heading, click **Properties**.</span></span> <span data-ttu-id="a6923-178">Poznamenejte si **název serveru** a **PŘIHLAŠOVACÍ jméno pro SERVER správce**.</span><span class="sxs-lookup"><span data-stu-id="a6923-178">Note down **SERVER NAME** and **SERVER ADMIN LOGIN NAME**.</span></span> <span data-ttu-id="a6923-179">Kliknutí na tlačítko hello kopírování tlačítko Další tooeach pole toocopy toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="a6923-179">You may click hello copy button next tooeach field toocopy toohello clipboard.</span></span>
   <span data-ttu-id="a6923-180">![Vlastnosti serveru 4-2](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span><span class="sxs-lookup"><span data-stu-id="a6923-180">![4-2 server properties](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span></span>

<span data-ttu-id="a6923-181">V tomto příkladu je název serveru hello *myserver4demo.mysql.database.azure.com*, a je přihlašovací jméno správce serveru hello  *myadmin@myserver4demo* .</span><span class="sxs-lookup"><span data-stu-id="a6923-181">In this example, hello server name is *myserver4demo.mysql.database.azure.com*, and hello server admin login is *myadmin@myserver4demo*.</span></span>

## <a name="connect-toohello-server-using-mysql"></a><span data-ttu-id="a6923-182">Připojení serveru toohello pomocí mysql</span><span class="sxs-lookup"><span data-stu-id="a6923-182">Connect toohello server using mysql</span></span>
<span data-ttu-id="a6923-183">Použití [nástroj příkazového řádku mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish tooyour připojení databáze Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="a6923-183">Use [mysql command-line tool](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish a connection tooyour Azure Database for MySQL server.</span></span> <span data-ttu-id="a6923-184">Nástroj příkazového řádku mysql hello můžete spustit z hello prostředí cloudu Azure v prohlížeči hello nebo vlastní počítače pomocí nástroje mysql nainstalované místně.</span><span class="sxs-lookup"><span data-stu-id="a6923-184">You can run hello mysql command-line tool from hello Azure Cloud Shell in hello browser or from your own machine using mysql tools installed locally.</span></span> <span data-ttu-id="a6923-185">toolaunch hello prostředí cloudu Azure, klikněte na tlačítko hello `Try It` tlačítko blok kódu v tomto článku, nebo navštívit hello portál Azure a klikněte na hello `>_` ikonu v horním pravém panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-185">toolaunch hello Azure Cloud Shell, click hello `Try It` button on a code block in this article, or visit hello Azure portal and click hello `>_` icon in hello top right toolbar.</span></span> 

<span data-ttu-id="a6923-186">Zadejte příkaz tooconnect hello:</span><span class="sxs-lookup"><span data-stu-id="a6923-186">Type hello command tooconnect:</span></span>
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="a6923-187">Vytvoření prázdné databáze</span><span class="sxs-lookup"><span data-stu-id="a6923-187">Create a blank database</span></span>
<span data-ttu-id="a6923-188">Jakmile jste server připojený toohello, vytvořte prázdnou databázi toowork s.</span><span class="sxs-lookup"><span data-stu-id="a6923-188">Once you’re connected toohello server, create a blank database toowork with.</span></span>
```sql
CREATE DATABASE mysampledb;
```

<span data-ttu-id="a6923-189">V řádku hello spusťte následující příkaz tooswitch připojení toothis nově vytvořený databáze hello:</span><span class="sxs-lookup"><span data-stu-id="a6923-189">At hello prompt, run hello following command tooswitch connection toothis newly created database:</span></span>
```sql
USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="a6923-190">Vytváření tabulek v databázi hello</span><span class="sxs-lookup"><span data-stu-id="a6923-190">Create tables in hello database</span></span>
<span data-ttu-id="a6923-191">Teď, když víte, jak tooconnect toohello Azure Database pro databázi MySQL, jsme můžete projít postupy toocomplete některé základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="a6923-191">Now that you know how tooconnect toohello Azure Database for MySQL database, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="a6923-192">Jsme nejprve vytvořit tabulku a načíst určitými daty.</span><span class="sxs-lookup"><span data-stu-id="a6923-192">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="a6923-193">Umožňuje vytvořit tabulku, která ukládá informace o inventáři.</span><span class="sxs-lookup"><span data-stu-id="a6923-193">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="a6923-194">Načtení dat do tabulky hello</span><span class="sxs-lookup"><span data-stu-id="a6923-194">Load data into hello tables</span></span>
<span data-ttu-id="a6923-195">Teď, když máme tabulku, jsme do něj vložte některá data.</span><span class="sxs-lookup"><span data-stu-id="a6923-195">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="a6923-196">V okně hello spusťte příkazový řádek spusťte hello následující dotaz tooinsert některé řádky dat.</span><span class="sxs-lookup"><span data-stu-id="a6923-196">At hello open command prompt window, run hello following query tooinsert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="a6923-197">Nyní máte dva řádky ukázková data do hello tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="a6923-197">Now you have two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="a6923-198">Dotazování a aktualizovat hello data v tabulkách hello</span><span class="sxs-lookup"><span data-stu-id="a6923-198">Query and update hello data in hello tables</span></span>
<span data-ttu-id="a6923-199">Spusťte následující dotaz tooretrieve informace z tabulky databáze hello hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-199">Execute hello following query tooretrieve information from hello database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="a6923-200">Můžete také aktualizovat hello data v tabulkách hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-200">You can also update hello data in hello tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="a6923-201">Při načítání dat, získá Hello řádek příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="a6923-201">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="a6923-202">Obnovit do databáze tooa předchozího bodu v čase</span><span class="sxs-lookup"><span data-stu-id="a6923-202">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="a6923-203">Představte si, že omylem odstraněn k tabulce důležité databáze a nelze snadno obnovit hello data.</span><span class="sxs-lookup"><span data-stu-id="a6923-203">Imagine you have accidentally deleted an important database table, and cannot recover hello data easily.</span></span> <span data-ttu-id="a6923-204">Databáze pro databázi MySQL Azure vám umožní toorestore hello server tooa bod v čase, vytváření kopie databáze hello do nového serveru.</span><span class="sxs-lookup"><span data-stu-id="a6923-204">Azure Database for MySQL allows you toorestore hello server tooa point in time, creating a copy of hello databases into new server.</span></span> <span data-ttu-id="a6923-205">Můžete použít tento nový server toorecover odstraněná data.</span><span class="sxs-lookup"><span data-stu-id="a6923-205">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="a6923-206">Následující kroky obnovení hello ukázkový server tooa bod před přidáním tabulky hello Hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-206">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

1. <span data-ttu-id="a6923-207">V hello portálu Azure vyhledejte Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a6923-207">In hello Azure portal, locate your Azure Database for MySQL.</span></span> <span data-ttu-id="a6923-208">Na hello **přehled** klikněte na tlačítko **obnovení** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-208">On hello **Overview** page, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="a6923-209">Otevře se stránka obnovení Hello.</span><span class="sxs-lookup"><span data-stu-id="a6923-209">hello Restore page opens.</span></span>

   ![10-1 obnovení databáze](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. <span data-ttu-id="a6923-211">Vyplňte hello **obnovení** formulář hello požadované informace.</span><span class="sxs-lookup"><span data-stu-id="a6923-211">Fill out hello **Restore** form with hello required information.</span></span>
   
   ![Formulář obnovení 10-2](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - <span data-ttu-id="a6923-213">**Bod obnovení**: Vyberte bodu v čase, které chcete toorestore, v časovém rámci hello uvedené.</span><span class="sxs-lookup"><span data-stu-id="a6923-213">**Restore point**: Select a point-in-time that you want toorestore to, within hello timeframe listed.</span></span> <span data-ttu-id="a6923-214">Ujistěte se, že tooconvert tooUTC vaše místní časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="a6923-214">Make sure tooconvert your local timezone tooUTC.</span></span>
   - <span data-ttu-id="a6923-215">**Obnovení serveru toonew**: Zadejte nový název serveru, který chcete toorestore k.</span><span class="sxs-lookup"><span data-stu-id="a6923-215">**Restore toonew server**: Provide a new server name you want toorestore to.</span></span>
   - <span data-ttu-id="a6923-216">**Umístění**: oblast hello je stejný jako zdrojový server hello a nedá se změnit.</span><span class="sxs-lookup"><span data-stu-id="a6923-216">**Location**: hello region is same as hello source server, and cannot be changed.</span></span>
   - <span data-ttu-id="a6923-217">**Cenová úroveň**: hello cenová úroveň je hello stejný jako hello zdrojového serveru a nelze je změnit.</span><span class="sxs-lookup"><span data-stu-id="a6923-217">**Pricing tier**: hello pricing tier is hello same as hello source server, and cannot be changed.</span></span>
   
3. <span data-ttu-id="a6923-218">Klikněte na tlačítko **OK** toorestore hello serveru příliš[obnovit tooa bodu v čase](./howto-restore-server-portal.md) před hello tabulka byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="a6923-218">Click **OK** toorestore hello server too[restore tooa point in time](./howto-restore-server-portal.md) before hello table was deleted.</span></span> <span data-ttu-id="a6923-219">Obnovení serveru vytvoří novou kopii server hello od hello bodu v čase, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="a6923-219">Restoring a server creates a new copy of hello server, as of hello point in time you specify.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a6923-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6923-220">Next steps</span></span>
<span data-ttu-id="a6923-221">V tomto kurzu použijete hello Azure portálu toolearned jak na:</span><span class="sxs-lookup"><span data-stu-id="a6923-221">In this tutorial, you use hello Azure portal toolearned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6923-222">Vytvoření Azure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="a6923-222">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="a6923-223">Konfigurace brány firewall serveru hello</span><span class="sxs-lookup"><span data-stu-id="a6923-223">Configure hello server firewall</span></span>
> * <span data-ttu-id="a6923-224">Použijte nástroj příkazového řádku toocreate mysql databáze</span><span class="sxs-lookup"><span data-stu-id="a6923-224">Use mysql command-line tool toocreate a database</span></span>
> * <span data-ttu-id="a6923-225">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="a6923-225">Load sample data</span></span>
> * <span data-ttu-id="a6923-226">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="a6923-226">Query data</span></span>
> * <span data-ttu-id="a6923-227">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="a6923-227">Update data</span></span>
> * <span data-ttu-id="a6923-228">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="a6923-228">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a6923-229">Jak aplikace tooAzure tooconnect databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="a6923-229">How tooconnect applications tooAzure Database for MySQL</span></span>](./howto-connection-string.md)
