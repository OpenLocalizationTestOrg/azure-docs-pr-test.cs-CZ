---
title: "aaaConnect stávající služby Azure App Service tooAzure databáze pro databázi MySQL | Microsoft Docs"
description: "Pokyny, jak tooproperly připojit stávající služby Azure App Service tooAzure databáze pro databázi MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 6d5b16f316e186d665370adcd8b7c7bb38c8d51a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-an-existing-azure-app-service-tooazure-database-for-mysql-server"></a><span data-ttu-id="79516-103">Připojit existující tooAzure Azure App Service databáze MySQL serveru</span><span class="sxs-lookup"><span data-stu-id="79516-103">Connect an existing Azure App Service tooAzure Database for MySQL server</span></span>
<span data-ttu-id="79516-104">Tento dokument popisuje, jak tooconnect stávající služby Azure App Service tooyour Azure databáze pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="79516-104">This document explains how tooconnect an existing Azure App Service tooyour Azure Database for MySQL server.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="79516-105">Než začnete</span><span class="sxs-lookup"><span data-stu-id="79516-105">Before you begin</span></span>
<span data-ttu-id="79516-106">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79516-106">Log in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="79516-107">Vytvoření databáze Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="79516-107">Create an Azure Database for MySQL server.</span></span> <span data-ttu-id="79516-108">Podrobnosti najdete příliš[jak toocreate Azure databáze MySQL serveru z portálu](quickstart-create-mysql-server-database-using-azure-portal.md) nebo [jak toocreate Azure databáze MySQL serveru pomocí rozhraní příkazového řádku](quickstart-create-mysql-server-database-using-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="79516-108">For details, refer too[How toocreate Azure Database for MySQL server from Portal](quickstart-create-mysql-server-database-using-azure-portal.md) or [How toocreate Azure Database for MySQL server using CLI](quickstart-create-mysql-server-database-using-azure-cli.md).</span></span>

<span data-ttu-id="79516-109">Nyní nejsou k dispozici dva přístup řešení tooenable ze Azure App Service tooan Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="79516-109">Currently there are two solutions tooenable access from an Azure App Service tooan Azure Database for MySQL.</span></span> <span data-ttu-id="79516-110">Obě řešení zahrnují nastavení pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="79516-110">Both solutions involve setting up server-level firewall rules.</span></span>

## <a name="solution-1---create-a-firewall-rule-tooallow-all-ips"></a><span data-ttu-id="79516-111">Řešení 1 – Vytvoření tooallow pravidlo brány firewall všechny IP adresy</span><span class="sxs-lookup"><span data-stu-id="79516-111">Solution 1 - Create a firewall rule tooallow all IPs</span></span>
<span data-ttu-id="79516-112">Databáze pro databázi MySQL Azure poskytuje přístup k zabezpečení pomocí brány firewall tooprotect vaše data.</span><span class="sxs-lookup"><span data-stu-id="79516-112">Azure Database for MySQL provides access security using a firewall tooprotect your data.</span></span> <span data-ttu-id="79516-113">Při připojení z Azure App Service tooAzure databáze pro server databáze MySQL, mějte na paměti této hello odchozí jsou ve své podstatě dynamické IP adresy služby App Service.</span><span class="sxs-lookup"><span data-stu-id="79516-113">When connecting from an Azure App Service tooAzure Database for MySQL server, keep in mind that hello outbound IPs of App Service are dynamic in nature.</span></span> 

<span data-ttu-id="79516-114">tooensure hello dostupnost služby Azure App Service, doporučujeme používat toto řešení tooallow všechny IP adresy.</span><span class="sxs-lookup"><span data-stu-id="79516-114">tooensure hello availability of your Azure App Service, we recommend using this solution tooallow ALL IPs.</span></span>

> [!NOTE]
> <span data-ttu-id="79516-115">Společnost Microsoft pracuje pro dlouhodobé tooavoid řešení, aby vám umožnil všechny IP adresy pro služby Azure tooconnect tooAzure databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="79516-115">Microsoft is working for a long-term solution tooavoid allowing all IPs for Azure services tooconnect tooAzure Database for MySQL.</span></span>

1. <span data-ttu-id="79516-116">V okně serveru MySQL hello, v části nastavení klikněte na **zabezpečení připojení** tooopen hello zabezpečení připojení okně hello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="79516-116">On hello MySQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for MySQL.</span></span>

   ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="79516-118">Zadejte **název pravidla**, **počáteční IP adresa**, a **KONCOVÁ IP adresa**.</span><span class="sxs-lookup"><span data-stu-id="79516-118">Enter **RULE NAME**, **START IP**, and **END IP**.</span></span> <span data-ttu-id="79516-119">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="79516-119">Then click **Save**.</span></span>
   - <span data-ttu-id="79516-120">Název pravidla: Povolit – všechny-IP adresy</span><span class="sxs-lookup"><span data-stu-id="79516-120">Rule name: Allow-All-IPs</span></span>
   - <span data-ttu-id="79516-121">Spustit IP: 0.0.0.0</span><span class="sxs-lookup"><span data-stu-id="79516-121">Start IP: 0.0.0.0</span></span>
   - <span data-ttu-id="79516-122">Ukončení IP: 255.255.255.255</span><span class="sxs-lookup"><span data-stu-id="79516-122">End IP: 255.255.255.255</span></span>

   ![Portál Azure – přidání všechny IP adresy](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-tooexplicitly-allow-outbound-ips"></a><span data-ttu-id="79516-124">Řešení 2 – Vytvoření brány firewall povolit pravidlo tooexplicitly odchozí IP adresy</span><span class="sxs-lookup"><span data-stu-id="79516-124">Solution 2 - Create a firewall rule tooexplicitly allow outbound IPs</span></span>
<span data-ttu-id="79516-125">Můžete explicitně přidat, že všechny hello odchozí IP adresy služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="79516-125">You can explicitly add all hello outbound IPs of your Azure App Service.</span></span>

1. <span data-ttu-id="79516-126">V okně Vlastnosti služby aplikace hello, zobrazit vaše **ODCHOZÍ IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="79516-126">On hello App Service Properties blade, view your **OUTBOUND IP ADDRESS**.</span></span>

   ![Portál Azure – zobrazení odchozí IP adresy](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. <span data-ttu-id="79516-128">V okně zabezpečení hello připojení MySQL přidáte odchozí IP adres po jednom.</span><span class="sxs-lookup"><span data-stu-id="79516-128">On hello MySQL Connection security blade, add outbound IPs one by one.</span></span>

   ![Portál Azure – přidání explicitní IP adresy](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. <span data-ttu-id="79516-130">Mějte na paměti, příliš**Uložit** vašich pravidlech brány firewall.</span><span class="sxs-lookup"><span data-stu-id="79516-130">Remember too**Save** your firewall rules.</span></span>

<span data-ttu-id="79516-131">I když hello služby Azure App service pokusí tookeep IP adresy konstanta v čase, jsou případy, kde může změnit hello IP adresy.</span><span class="sxs-lookup"><span data-stu-id="79516-131">Though hello Azure App service attempts tookeep IP addresses constant over time, there are cases where hello IP addresses may change.</span></span> <span data-ttu-id="79516-132">Například když hello aplikace recykluje nebo dojde k operaci zvětšení nebo při přidávání nových počítačů v Azure místní data centra tooincrease hello kapacity.</span><span class="sxs-lookup"><span data-stu-id="79516-132">For example, when hello app recycles or a scale operation occurs, or when new machines are added in Azure regional data centers tooincrease hello capacity.</span></span> <span data-ttu-id="79516-133">Při změně adres hello IP, aplikace hello se může setkat s výpadku v případě hello už můžete připojit toohello MySQL server.</span><span class="sxs-lookup"><span data-stu-id="79516-133">When hello IP addresses change, hello app could experience downtime in hello event it can no longer connect toohello MySQL server.</span></span> <span data-ttu-id="79516-134">Zvažte tato potenciální při výběru jednoho hello předcházející řešení.</span><span class="sxs-lookup"><span data-stu-id="79516-134">Consider this potential when choosing one of hello preceding solutions.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="79516-135">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="79516-135">SSL configuration</span></span>
<span data-ttu-id="79516-136">Azure databáze MySQL má ve výchozím nastavení povolen protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="79516-136">Azure Database for MySQL has SSL Enabled by default.</span></span> <span data-ttu-id="79516-137">Pokud vaše aplikace nepoužívá SSL tooconnect toohello databáze, musíte toodisable SSL na serveru databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="79516-137">If your application is not using SSL tooconnect toohello database, then you need toodisable SSL on MySQL server.</span></span> <span data-ttu-id="79516-138">Podrobnosti o tom tooconfigure SSL, najdete v tématu [pomocí protokolu SSL s Azure Database pro databázi MySQL](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="79516-138">For details on how tooconfigure SSL, See [Using SSL with Azure Database for MySQL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="79516-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79516-139">Next steps</span></span>
<span data-ttu-id="79516-140">Další informace o připojovacích řetězcích najdete příliš[připojovací řetězce](howto-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="79516-140">For more information about connection strings, refer too[Connection Strings](howto-connection-string.md).</span></span>
