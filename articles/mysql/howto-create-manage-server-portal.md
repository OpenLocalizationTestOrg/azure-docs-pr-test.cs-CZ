---
title: "aaaCreate a správě Azure databáze pro server databáze MySQL pomocí portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak můžete rychle vytvořit novou databázi Azure pro server databáze MySQL a spravovat server hello pomocí hello portálu Azure."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c532df43b3d2124d7bd34875b32d52357f162af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a><span data-ttu-id="e9b42-103">Vytvoření a správě Azure databáze pro server databáze MySQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9b42-103">Create and manage Azure Database for MySQL server using Azure portal</span></span>
<span data-ttu-id="e9b42-104">Tento článek popisuje, jak můžete rychle vytvořit novou databázi Azure pro server databáze MySQL a spravovat server hello pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9b42-104">This article describes how you can quickly create a new Azure Database for MySQL server and manage hello server using hello Azure Portal.</span></span> <span data-ttu-id="e9b42-105">Správa serveru zahrnuje zobrazení Podrobnosti o serveru a databází, resetování hesla a odstranění serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e9b42-105">Server management includes viewing server details & databases, resetting password and deleting hello server.</span></span>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="e9b42-106">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9b42-106">Log in toohello Azure portal</span></span>
<span data-ttu-id="e9b42-107">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e9b42-107">Log in toohello [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="e9b42-108">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="e9b42-108">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="e9b42-109">Postupujte podle těchto kroků toocreate Azure Database MySQL serveru s názvem "mysqlserver4demo"</span><span class="sxs-lookup"><span data-stu-id="e9b42-109">Follow these steps toocreate an Azure Database for MySQL server named “mysqlserver4demo”</span></span>

<span data-ttu-id="e9b42-110">1kliknutím **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9b42-110">1- Click **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

<span data-ttu-id="e9b42-111">Vyberte 2 **databáze** novou stránku hello a výběrem **Azure Database pro databázi MySQL** ze stránky hello databáze.</span><span class="sxs-lookup"><span data-stu-id="e9b42-111">2- Select **Databases** from hello New page, and select **Azure Database for MySQL** from hello Databases page.</span></span>

> <span data-ttu-id="e9b42-112">Azure databáze pro server databáze MySQL byla vytvořena s definovanou sadu [výpočetního prostředí a úložiště](./concepts-compute-unit-and-storage.md) prostředky.</span><span class="sxs-lookup"><span data-stu-id="e9b42-112">An Azure Database for MySQL server is created with a defined set of [compute and storage](./concepts-compute-unit-and-storage.md) resources.</span></span> <span data-ttu-id="e9b42-113">Hello databáze byla vytvořena v rámci skupiny prostředků Azure a v databázi aplikace Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="e9b42-113">hello database is created within an Azure resource group and in an Azure Database for MySQL server.</span></span>

![Vytvořit nový serverem](./media/howto-create-manage-server-portal/create-new-server.png)

<span data-ttu-id="e9b42-115">3 - vyplňte hello databáze Azure pro formulář MySQL s hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="e9b42-115">3- Fill out hello Azure Database for MySQL form with hello following information:</span></span>

| <span data-ttu-id="e9b42-116">**Pole formuláře**</span><span class="sxs-lookup"><span data-stu-id="e9b42-116">**Form Field**</span></span> | <span data-ttu-id="e9b42-117">**Popis pole**</span><span class="sxs-lookup"><span data-stu-id="e9b42-117">**Field Description**</span></span> |
|----------------|-----------------------|
| <span data-ttu-id="e9b42-118">*Název serveru*</span><span class="sxs-lookup"><span data-stu-id="e9b42-118">*Server name*</span></span> | <span data-ttu-id="e9b42-119">Azure mysql (název serveru je globálně jedinečný)</span><span class="sxs-lookup"><span data-stu-id="e9b42-119">azure-mysql (server name is globally unique)</span></span> |
| <span data-ttu-id="e9b42-120">*Předplatné*</span><span class="sxs-lookup"><span data-stu-id="e9b42-120">*Subscription*</span></span> | <span data-ttu-id="e9b42-121">MySQLaaS (vyberte z rozevíracího seznamu)</span><span class="sxs-lookup"><span data-stu-id="e9b42-121">MySQLaaS (select from drop down)</span></span> |
| <span data-ttu-id="e9b42-122">*Skupina prostředků*</span><span class="sxs-lookup"><span data-stu-id="e9b42-122">*Resource group*</span></span> | <span data-ttu-id="e9b42-123">MyResource (vytvořit novou skupinu prostředků nebo použijte existující)</span><span class="sxs-lookup"><span data-stu-id="e9b42-123">myresource (create a new resource group or use an existing one)</span></span> |
| <span data-ttu-id="e9b42-124">*Přihlašovací jméno správce serveru*</span><span class="sxs-lookup"><span data-stu-id="e9b42-124">*Server admin login*</span></span> | <span data-ttu-id="e9b42-125">myadmin (název účtu správce instalace)</span><span class="sxs-lookup"><span data-stu-id="e9b42-125">myadmin (setup admin account name)</span></span> |
| <span data-ttu-id="e9b42-126">*Heslo*</span><span class="sxs-lookup"><span data-stu-id="e9b42-126">*Password*</span></span> | <span data-ttu-id="e9b42-127">Instalační program heslo účtu správce</span><span class="sxs-lookup"><span data-stu-id="e9b42-127">setup admin account password</span></span> |
| <span data-ttu-id="e9b42-128">*Potvrdit heslo*</span><span class="sxs-lookup"><span data-stu-id="e9b42-128">*Confirm password*</span></span> | <span data-ttu-id="e9b42-129">potvrďte heslo účtu správce</span><span class="sxs-lookup"><span data-stu-id="e9b42-129">confirm admin account password</span></span> |
| <span data-ttu-id="e9b42-130">*Umístění*</span><span class="sxs-lookup"><span data-stu-id="e9b42-130">*Location*</span></span> | <span data-ttu-id="e9b42-131">Severní Evropa (vyberte mezi Severní Evropa a západní USA)</span><span class="sxs-lookup"><span data-stu-id="e9b42-131">North Europe (select between North Europe and West US)</span></span> |
| <span data-ttu-id="e9b42-132">*Verze*</span><span class="sxs-lookup"><span data-stu-id="e9b42-132">*Version*</span></span> | <span data-ttu-id="e9b42-133">5.6 (zvolte Azure databáze MySQL server verze)</span><span class="sxs-lookup"><span data-stu-id="e9b42-133">5.6 (choose Azure Database for MySQL server version)</span></span> |

<span data-ttu-id="e9b42-134">4 kliknutím **cenová úroveň** toospecify hello služby vrstvy a úroveň výkonu pro nový server.</span><span class="sxs-lookup"><span data-stu-id="e9b42-134">4- Click **Pricing tier** toospecify hello service tier and performance level for your new server.</span></span> <span data-ttu-id="e9b42-135">Výpočetní jednotky lze konfigurovat rozsahu 50 až 100 v základní vrstvě, 100 až 200 ve standardní vrstvě a úložiště lze přidat podle zahrnuté množství.</span><span class="sxs-lookup"><span data-stu-id="e9b42-135">Compute Unit can be configured between 50 and 100 in Basic tier, 100 and 200 in Standard tier, and storage can be added based on included amount.</span></span> <span data-ttu-id="e9b42-136">Tato příručka postupy umožňuje zvolte 50 výpočetní jednotka a 50GB.</span><span class="sxs-lookup"><span data-stu-id="e9b42-136">For this HowTo guide, let’s choose 50 Compute Unit and 50GB.</span></span> <span data-ttu-id="e9b42-137">Klikněte na tlačítko **OK** toosave váš výběr.</span><span class="sxs-lookup"><span data-stu-id="e9b42-137">Click **OK** toosave your selection.</span></span>
<span data-ttu-id="e9b42-138">![Vytvoření serveru – ceny vrstvy](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span><span class="sxs-lookup"><span data-stu-id="e9b42-138">![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span></span>

<span data-ttu-id="e9b42-139">Kliknutím 5 **vytvořit** tooprovision hello serveru.</span><span class="sxs-lookup"><span data-stu-id="e9b42-139">5- Click **Create** tooprovision hello server.</span></span> <span data-ttu-id="e9b42-140">Zřizování trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="e9b42-140">Provisioning takes a few minutes.</span></span>

> <span data-ttu-id="e9b42-141">Zkontrolujte hello **Pin toodashboard** možnost tooallow snadné sledování vašich nasazení.</span><span class="sxs-lookup"><span data-stu-id="e9b42-141">Check hello **Pin toodashboard** option tooallow easy tracking of your deployments.</span></span>
> [!NOTE]
> <span data-ttu-id="e9b42-142">I když too1000GB v základní vrstvě a 10000GB ve verzi Standard se úroveň nepodporuje pro úložiště pro verzi Public Preview, je maximální velikost úložiště hello stále omezené too1000GB dočasně.</span><span class="sxs-lookup"><span data-stu-id="e9b42-142">Although up too1000GB in Basic tier and 10000GB in Standard tier will be supported for storage, for Public Preview, hello maximum storage is still limited too1000GB temporarily.</span></span> 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a><span data-ttu-id="e9b42-143">Aktualizovat databázi služby Azure pro server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="e9b42-143">Update an Azure Database for MySQL server</span></span>
<span data-ttu-id="e9b42-144">Po zřízení nového serveru uživatel má 2 možnosti tooedit existující server: resetování hesla správce nebo škálování nahoru/dolů hello server změnou hello výpočetní jednotky.</span><span class="sxs-lookup"><span data-stu-id="e9b42-144">After new server is provisioned, user has 2 options tooedit an existing server: reset administrator password or scale up/down hello server by changing hello compute-units.</span></span>

### <a name="change-hello-administrator-user-password"></a><span data-ttu-id="e9b42-145">Změnit heslo uživatele správce hello</span><span class="sxs-lookup"><span data-stu-id="e9b42-145">Change hello administrator user password</span></span>
<span data-ttu-id="e9b42-146">1 - na serveru hello **přehled** okně klikněte na tlačítko **resetovat heslo** toopopulate okno zadání a potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="e9b42-146">1- On hello server **Overview** blade, click **Reset password** toopopulate a password input and confirmation window.</span></span>

<span data-ttu-id="e9b42-147">2 – zadejte nové heslo a potvrzení hesla hello v okně hello jak je uvedeno níže: ![resetování hesla](./media/howto-create-manage-server-portal/reset-password.png)</span><span class="sxs-lookup"><span data-stu-id="e9b42-147">2- Enter new password and confirm hello password in hello window as below: ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)</span></span>

<span data-ttu-id="e9b42-148">3 klikněte na tlačítko **OK** toosave hello nové heslo.</span><span class="sxs-lookup"><span data-stu-id="e9b42-148">3- Click **OK** toosave hello new password.</span></span>

### <a name="scale-updown-by-changing-compute-units"></a><span data-ttu-id="e9b42-149">Změnou výpočetní jednotky škálování nahoru/dolů</span><span class="sxs-lookup"><span data-stu-id="e9b42-149">Scale up/down by changing Compute Units</span></span>

<span data-ttu-id="e9b42-150">1 - v okně hello serveru v části **nastavení**, klikněte na tlačítko **cenová úroveň** tooopen hello cenová úroveň okně hello Azure databáze pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="e9b42-150">1- On hello server blade, under **Settings**, click **Pricing tier** tooopen hello Pricing tier blade for hello Azure Database for MySQL server.</span></span>

<span data-ttu-id="e9b42-151">2 – postupujte podle kroku 4 v **vytvoření databáze Azure pro server databáze MySQL** toochange výpočetní jednotky v hello stejné cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="e9b42-151">2- Follow Step 4 in **Create an Azure Database for MySQL server** toochange Compute Units in hello same pricing tier.</span></span>

## <a name="delete-an-azure-database-for-mysql-server"></a><span data-ttu-id="e9b42-152">Odstraňte databázi Azure pro server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="e9b42-152">Delete an Azure Database for MySQL server</span></span>

<span data-ttu-id="e9b42-153">1 - na serveru hello **přehled** okně klikněte na tlačítko **odstranit** příkaz tlačítko tooopen hello odstranění potvrzovací okno.</span><span class="sxs-lookup"><span data-stu-id="e9b42-153">1- On hello server **Overview** blade, click **Delete** command button tooopen hello Deleting confirmation blade.</span></span>

<span data-ttu-id="e9b42-154">2-type hello správný název serveru vstupní pole hello okno pro potvrzení double.</span><span class="sxs-lookup"><span data-stu-id="e9b42-154">2- Type hello correct server name in input box of hello blade for double confirmation.</span></span>

<span data-ttu-id="e9b42-155">3 klikněte na tlačítko **odstranit** tlačítko znovu tooconfirm odstraňování akce a počkejte, než pro "Odstranění úspěch" místní na hello oznamovací pruh.</span><span class="sxs-lookup"><span data-stu-id="e9b42-155">3- Click **Delete** button again tooconfirm deleting action and wait for “Deleting success” popup on hello notification bar.</span></span>

## <a name="list-hello-azure-database-for-mysql-databases"></a><span data-ttu-id="e9b42-156">Seznam hello databáze Azure pro databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="e9b42-156">List hello Azure Database for MySQL databases</span></span>
<span data-ttu-id="e9b42-157">Na serveru hello **přehled** okno, posuňte se dolů, dokud neuvidíte hello databáze na dolní hello dlaždici.</span><span class="sxs-lookup"><span data-stu-id="e9b42-157">On hello server **Overview** blade, scroll down until you see hello database tile on hello bottom.</span></span> <span data-ttu-id="e9b42-158">Všechny databáze hello se objeví v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="e9b42-158">All hello databases will be listed in hello table.</span></span> <span data-ttu-id="e9b42-159">Klikněte na tlačítko **odstranit** příkaz tlačítko tooopen hello odstranění potvrzovací okno.</span><span class="sxs-lookup"><span data-stu-id="e9b42-159">click **Delete** command button tooopen hello Deleting confirmation blade.</span></span>

![Zobrazit databáze](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a><span data-ttu-id="e9b42-161">Zobrazit podrobnosti databáze Azure pro server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="e9b42-161">Show details of an Azure Database for MySQL server</span></span>
<span data-ttu-id="e9b42-162">Klikněte na tlačítko **vlastnosti** pod **nastavení** na serveru hello se otevře okno hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="e9b42-162">Click **Properties** under **Settings** on hello server blade will open hello **Properties** blade.</span></span> <span data-ttu-id="e9b42-163">Potom můžete zobrazit všechny podrobné informace o serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e9b42-163">Then you can view all detailed information about hello server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9b42-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9b42-164">Next steps</span></span>

[<span data-ttu-id="e9b42-165">Rychlý úvod: Vytvoření databáze Azure pro server databáze MySQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9b42-165">Quickstart: Create Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
