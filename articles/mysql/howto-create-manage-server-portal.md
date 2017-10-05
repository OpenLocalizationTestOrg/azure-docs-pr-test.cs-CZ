---
title: "Vytvoření a správě Azure databáze pro server databáze MySQL pomocí portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak můžete rychle vytvořit novou databázi MySQL server Azure a spravovat server pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4605518b6955d9943e76c25df2d4105a6a94433d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a><span data-ttu-id="11a68-103">Vytvoření a správě Azure databáze pro server databáze MySQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="11a68-103">Create and manage Azure Database for MySQL server using Azure portal</span></span>
<span data-ttu-id="11a68-104">Tento článek popisuje, jak můžete rychle vytvořit novou databázi MySQL server Azure a spravovat server pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="11a68-104">This article describes how you can quickly create a new Azure Database for MySQL server and manage the server using the Azure Portal.</span></span> <span data-ttu-id="11a68-105">Správa serveru zahrnuje zobrazení Podrobnosti o serveru a databází, resetování hesla a odstranění serveru.</span><span class="sxs-lookup"><span data-stu-id="11a68-105">Server management includes viewing server details & databases, resetting password and deleting the server.</span></span>

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="11a68-106">Přihlášení k portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="11a68-106">Log in to the Azure portal</span></span>
<span data-ttu-id="11a68-107">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="11a68-107">Log in to the [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="11a68-108">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="11a68-108">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="11a68-109">Postupujte podle těchto kroků k vytvoření databáze MySQL serveru s názvem "mysqlserver4demo" Azure</span><span class="sxs-lookup"><span data-stu-id="11a68-109">Follow these steps to create an Azure Database for MySQL server named “mysqlserver4demo”</span></span>

<span data-ttu-id="11a68-110">1kliknutím **nový** nalezeno tlačítko v levém horním rohu portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="11a68-110">1- Click **New** button found on the upper left-hand corner of the Azure portal.</span></span>

<span data-ttu-id="11a68-111">Vyberte 2 **databáze** z novou stránku a vyberte **Azure Database pro databázi MySQL** ze stránky databáze.</span><span class="sxs-lookup"><span data-stu-id="11a68-111">2- Select **Databases** from the New page, and select **Azure Database for MySQL** from the Databases page.</span></span>

> <span data-ttu-id="11a68-112">Azure databáze pro server databáze MySQL byla vytvořena s definovanou sadu [výpočetního prostředí a úložiště](./concepts-compute-unit-and-storage.md) prostředky.</span><span class="sxs-lookup"><span data-stu-id="11a68-112">An Azure Database for MySQL server is created with a defined set of [compute and storage](./concepts-compute-unit-and-storage.md) resources.</span></span> <span data-ttu-id="11a68-113">Vytvoření databáze v rámci skupiny prostředků Azure a v databázi aplikace Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="11a68-113">The database is created within an Azure resource group and in an Azure Database for MySQL server.</span></span>

![Vytvořit nový serverem](./media/howto-create-manage-server-portal/create-new-server.png)

<span data-ttu-id="11a68-115">3 - vyplňte Azure databáze MySQL formuláře s následujícími informacemi:</span><span class="sxs-lookup"><span data-stu-id="11a68-115">3- Fill out the Azure Database for MySQL form with the following information:</span></span>

| <span data-ttu-id="11a68-116">**Pole formuláře**</span><span class="sxs-lookup"><span data-stu-id="11a68-116">**Form Field**</span></span> | <span data-ttu-id="11a68-117">**Popis pole**</span><span class="sxs-lookup"><span data-stu-id="11a68-117">**Field Description**</span></span> |
|----------------|-----------------------|
| <span data-ttu-id="11a68-118">*Název serveru*</span><span class="sxs-lookup"><span data-stu-id="11a68-118">*Server name*</span></span> | <span data-ttu-id="11a68-119">Azure mysql (název serveru je globálně jedinečný)</span><span class="sxs-lookup"><span data-stu-id="11a68-119">azure-mysql (server name is globally unique)</span></span> |
| <span data-ttu-id="11a68-120">*Předplatné*</span><span class="sxs-lookup"><span data-stu-id="11a68-120">*Subscription*</span></span> | <span data-ttu-id="11a68-121">MySQLaaS (vyberte z rozevíracího seznamu)</span><span class="sxs-lookup"><span data-stu-id="11a68-121">MySQLaaS (select from drop down)</span></span> |
| <span data-ttu-id="11a68-122">*Skupina prostředků*</span><span class="sxs-lookup"><span data-stu-id="11a68-122">*Resource group*</span></span> | <span data-ttu-id="11a68-123">MyResource (vytvořit novou skupinu prostředků nebo použijte existující)</span><span class="sxs-lookup"><span data-stu-id="11a68-123">myresource (create a new resource group or use an existing one)</span></span> |
| <span data-ttu-id="11a68-124">*Přihlašovací jméno správce serveru*</span><span class="sxs-lookup"><span data-stu-id="11a68-124">*Server admin login*</span></span> | <span data-ttu-id="11a68-125">myadmin (název účtu správce instalace)</span><span class="sxs-lookup"><span data-stu-id="11a68-125">myadmin (setup admin account name)</span></span> |
| <span data-ttu-id="11a68-126">*Heslo*</span><span class="sxs-lookup"><span data-stu-id="11a68-126">*Password*</span></span> | <span data-ttu-id="11a68-127">Instalační program heslo účtu správce</span><span class="sxs-lookup"><span data-stu-id="11a68-127">setup admin account password</span></span> |
| <span data-ttu-id="11a68-128">*Potvrdit heslo*</span><span class="sxs-lookup"><span data-stu-id="11a68-128">*Confirm password*</span></span> | <span data-ttu-id="11a68-129">potvrďte heslo účtu správce</span><span class="sxs-lookup"><span data-stu-id="11a68-129">confirm admin account password</span></span> |
| <span data-ttu-id="11a68-130">*Umístění*</span><span class="sxs-lookup"><span data-stu-id="11a68-130">*Location*</span></span> | <span data-ttu-id="11a68-131">Severní Evropa (vyberte mezi Severní Evropa a západní USA)</span><span class="sxs-lookup"><span data-stu-id="11a68-131">North Europe (select between North Europe and West US)</span></span> |
| <span data-ttu-id="11a68-132">*Verze*</span><span class="sxs-lookup"><span data-stu-id="11a68-132">*Version*</span></span> | <span data-ttu-id="11a68-133">5.6 (zvolte Azure databáze MySQL server verze)</span><span class="sxs-lookup"><span data-stu-id="11a68-133">5.6 (choose Azure Database for MySQL server version)</span></span> |

<span data-ttu-id="11a68-134">4 kliknutím **cenová úroveň** k určení úrovně služby a výkonu pro nový server.</span><span class="sxs-lookup"><span data-stu-id="11a68-134">4- Click **Pricing tier** to specify the service tier and performance level for your new server.</span></span> <span data-ttu-id="11a68-135">Výpočetní jednotky lze konfigurovat rozsahu 50 až 100 v základní vrstvě, 100 až 200 ve standardní vrstvě a úložiště lze přidat podle zahrnuté množství.</span><span class="sxs-lookup"><span data-stu-id="11a68-135">Compute Unit can be configured between 50 and 100 in Basic tier, 100 and 200 in Standard tier, and storage can be added based on included amount.</span></span> <span data-ttu-id="11a68-136">Tato příručka postupy umožňuje zvolte 50 výpočetní jednotka a 50GB.</span><span class="sxs-lookup"><span data-stu-id="11a68-136">For this HowTo guide, let’s choose 50 Compute Unit and 50GB.</span></span> <span data-ttu-id="11a68-137">Klikněte na tlačítko **OK** uložte svůj výběr.</span><span class="sxs-lookup"><span data-stu-id="11a68-137">Click **OK** to save your selection.</span></span>
<span data-ttu-id="11a68-138">![Vytvoření serveru – ceny vrstvy](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span><span class="sxs-lookup"><span data-stu-id="11a68-138">![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span></span>

<span data-ttu-id="11a68-139">Kliknutím 5 **vytvořit** ke zřízení serveru.</span><span class="sxs-lookup"><span data-stu-id="11a68-139">5- Click **Create** to provision the server.</span></span> <span data-ttu-id="11a68-140">Zřizování trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="11a68-140">Provisioning takes a few minutes.</span></span>

> <span data-ttu-id="11a68-141">Zaškrtněte možnost **Připnout na řídicí panel**, abyste povolili snadné sledování vašich nasazení.</span><span class="sxs-lookup"><span data-stu-id="11a68-141">Check the **Pin to dashboard** option to allow easy tracking of your deployments.</span></span>
> [!NOTE]
> <span data-ttu-id="11a68-142">I když až 1 000 GB v základní vrstvě a 10000 ve standardní vrstvě budou podporované pro úložiště pro verzi Public Preview, je maximální velikost úložiště stále omezen na 1000GB dočasně.</span><span class="sxs-lookup"><span data-stu-id="11a68-142">Although up to 1000GB in Basic tier and 10000GB in Standard tier will be supported for storage, for Public Preview, the maximum storage is still limited to 1000GB temporarily.</span></span> 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a><span data-ttu-id="11a68-143">Aktualizovat databázi služby Azure pro server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="11a68-143">Update an Azure Database for MySQL server</span></span>
<span data-ttu-id="11a68-144">Po zřízení nového serveru uživatel má 2 možností pro upravit existující server: resetování hesla správce nebo škálování nahoru/dolů serveru tak, že změníte výpočetní jednotky.</span><span class="sxs-lookup"><span data-stu-id="11a68-144">After new server is provisioned, user has 2 options to edit an existing server: reset administrator password or scale up/down the server by changing the compute-units.</span></span>

### <a name="change-the-administrator-user-password"></a><span data-ttu-id="11a68-145">Změna hesla správce uživatele</span><span class="sxs-lookup"><span data-stu-id="11a68-145">Change the administrator user password</span></span>
<span data-ttu-id="11a68-146">1 - na serveru **přehled** okně klikněte na tlačítko **resetovat heslo** k naplnění okno zadání a potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="11a68-146">1- On the server **Overview** blade, click **Reset password** to populate a password input and confirmation window.</span></span>

<span data-ttu-id="11a68-147">2 – zadejte nové heslo a potvrzení hesla do okna, jak je uvedeno níže: ![resetování hesla](./media/howto-create-manage-server-portal/reset-password.png)</span><span class="sxs-lookup"><span data-stu-id="11a68-147">2- Enter new password and confirm the password in the window as below: ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)</span></span>

<span data-ttu-id="11a68-148">3 klikněte na tlačítko **OK** uložit nové heslo.</span><span class="sxs-lookup"><span data-stu-id="11a68-148">3- Click **OK** to save the new password.</span></span>

### <a name="scale-updown-by-changing-compute-units"></a><span data-ttu-id="11a68-149">Změnou výpočetní jednotky škálování nahoru/dolů</span><span class="sxs-lookup"><span data-stu-id="11a68-149">Scale up/down by changing Compute Units</span></span>

<span data-ttu-id="11a68-150">1 - v okně serveru v části **nastavení**, klikněte na tlačítko **cenová úroveň** otevřete okno cenová úroveň pro databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="11a68-150">1- On the server blade, under **Settings**, click **Pricing tier** to open the Pricing tier blade for the Azure Database for MySQL server.</span></span>

<span data-ttu-id="11a68-151">2 – postupujte podle kroku 4 v **vytvoření databáze Azure pro server databáze MySQL** změnit výpočetní jednotky ve stejné cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="11a68-151">2- Follow Step 4 in **Create an Azure Database for MySQL server** to change Compute Units in the same pricing tier.</span></span>

## <a name="delete-an-azure-database-for-mysql-server"></a><span data-ttu-id="11a68-152">Odstraňte databázi Azure pro server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="11a68-152">Delete an Azure Database for MySQL server</span></span>

<span data-ttu-id="11a68-153">1 - na serveru **přehled** okně klikněte na tlačítko **odstranit** příkazového tlačítka, otevřete okno potvrzení odstranění.</span><span class="sxs-lookup"><span data-stu-id="11a68-153">1- On the server **Overview** blade, click **Delete** command button to open the Deleting confirmation blade.</span></span>

<span data-ttu-id="11a68-154">2 – zadejte správný název serveru do vstupního pole v okně pro dvojité potvrzení.</span><span class="sxs-lookup"><span data-stu-id="11a68-154">2- Type the correct server name in input box of the blade for double confirmation.</span></span>

<span data-ttu-id="11a68-155">3 klikněte na tlačítko **odstranit** tlačítko znovu odstraňování akci potvrďte a počkejte "Odstranění úspěch" místní na panelu oznámení.</span><span class="sxs-lookup"><span data-stu-id="11a68-155">3- Click **Delete** button again to confirm deleting action and wait for “Deleting success” popup on the notification bar.</span></span>

## <a name="list-the-azure-database-for-mysql-databases"></a><span data-ttu-id="11a68-156">Seznam databáze Azure pro databází MySQL</span><span class="sxs-lookup"><span data-stu-id="11a68-156">List the Azure Database for MySQL databases</span></span>
<span data-ttu-id="11a68-157">Na serveru **přehled** okno, posuňte se dolů, dokud neuvidíte databázi na dolní dlaždici.</span><span class="sxs-lookup"><span data-stu-id="11a68-157">On the server **Overview** blade, scroll down until you see the database tile on the bottom.</span></span> <span data-ttu-id="11a68-158">Zobrazí se všechny databáze v tabulce.</span><span class="sxs-lookup"><span data-stu-id="11a68-158">All the databases will be listed in the table.</span></span> <span data-ttu-id="11a68-159">Klikněte na tlačítko **odstranit** příkazového tlačítka, otevřete okno potvrzení odstranění.</span><span class="sxs-lookup"><span data-stu-id="11a68-159">click **Delete** command button to open the Deleting confirmation blade.</span></span>

![Zobrazit databáze](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a><span data-ttu-id="11a68-161">Zobrazit podrobnosti databáze Azure pro server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="11a68-161">Show details of an Azure Database for MySQL server</span></span>
<span data-ttu-id="11a68-162">Klikněte na tlačítko **vlastnosti** pod **nastavení** na serveru se otevře okno **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="11a68-162">Click **Properties** under **Settings** on the server blade will open the **Properties** blade.</span></span> <span data-ttu-id="11a68-163">Potom můžete zobrazit všechny podrobné informace o serveru.</span><span class="sxs-lookup"><span data-stu-id="11a68-163">Then you can view all detailed information about the server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11a68-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11a68-164">Next steps</span></span>

[<span data-ttu-id="11a68-165">Rychlý úvod: Vytvoření databáze Azure pro server databáze MySQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="11a68-165">Quickstart: Create Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
