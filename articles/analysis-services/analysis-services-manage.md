---
title: "Správa služby Azure Analysis Services | Microsoft Docs"
description: "Zjistěte, jak ke správě serveru služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 79491d0b-b00d-4e02-9ca7-adc99bc02fdb
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: b897e81351ebee11c292e67ac76ba8202a6f0108
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-analysis-services"></a><span data-ttu-id="f3b14-103">Správa služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="f3b14-103">Manage Analysis Services</span></span>
<span data-ttu-id="f3b14-104">Po vytvoření serveru služby Analysis Services v Azure, mohou být některé úlohy správy a údržby, které je třeba provést hned nebo zopakovat dolů na cestách.</span><span class="sxs-lookup"><span data-stu-id="f3b14-104">Once you've created an Analysis Services server in Azure, there may be some administration and management tasks you need to perform right away or sometime down the road.</span></span> <span data-ttu-id="f3b14-105">Například spusťte zpracování aktualizace dat, určovat, kdo může získat přístup modely na vašem serveru, nebo sledování stavu vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="f3b14-105">For example, run processing to the refresh data, control who can access the models on your server, or monitor your server's health.</span></span> <span data-ttu-id="f3b14-106">Některé úlohy správy lze provést pouze na portálu Azure, jiné v SQL Server Management Studio (SSMS) a některé úlohy lze provádět buď.</span><span class="sxs-lookup"><span data-stu-id="f3b14-106">Some management tasks can only be performed in Azure portal, others in SQL Server Management Studio (SSMS), and some tasks can be done in either.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="f3b14-107">portál Azure</span><span class="sxs-lookup"><span data-stu-id="f3b14-107">Azure portal</span></span>
<span data-ttu-id="f3b14-108">[Portál Azure](http://portal.azure.com/) je, kde můžete vytvořit a odstraněním serverů, monitorování prostředky serveru, změnit velikost a spravovat, kdo má přístup k serverům.</span><span class="sxs-lookup"><span data-stu-id="f3b14-108">[Azure portal](http://portal.azure.com/) is where you can create and delete servers, monitor server resources, change size, and manage who has access to your servers.</span></span>  <span data-ttu-id="f3b14-109">Pokud máte nějaké problémy, můžete také odeslat žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="f3b14-109">If you're having some problems, you can also submit a support request.</span></span>

![Získání názvu serveru v Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a><span data-ttu-id="f3b14-111">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="f3b14-111">SQL Server Management Studio</span></span>
<span data-ttu-id="f3b14-112">Připojení k serveru v Azure je stejně jako připojení k instanci serveru ve vaší vlastní organizaci.</span><span class="sxs-lookup"><span data-stu-id="f3b14-112">Connecting to your server in Azure is just like connecting to a server instance in your own organization.</span></span> <span data-ttu-id="f3b14-113">Z aplikace SSMS můžete provádět mnoho úloh, jako je například zpracování dat nebo vytvořit skript zpracování, správě rolí a pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f3b14-113">From SSMS, you can perform many of the same tasks such as process data or create a processing script, manage roles, and use PowerShell.</span></span>
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a><span data-ttu-id="f3b14-115">Stažení a instalace aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="f3b14-115">Download and install SSMS</span></span>
<span data-ttu-id="f3b14-116">Chcete-li získat všechny nejnovější funkce a nejhladší zkušenosti při připojování k serveru Azure Analysis Services, ujistěte se, že používáte nejnovější verzi aplikace SSMS.</span><span class="sxs-lookup"><span data-stu-id="f3b14-116">To get all the latest features, and the smoothest experience when connecting to your Azure Analysis Services server, be sure you're using the latest version of SSMS.</span></span> 

<span data-ttu-id="f3b14-117">[Stáhněte si SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="f3b14-117">[Download SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>


### <a name="to-connect-with-ssms"></a><span data-ttu-id="f3b14-118">Připojit pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="f3b14-118">To connect with SSMS</span></span>
 <span data-ttu-id="f3b14-119">Při použití aplikace SSMS, před připojením k serveru poprvé, zkontrolujte, zda že vaše uživatelské jméno je zahrnutá ve skupině Admins služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="f3b14-119">When using SSMS, before connecting to your server the first time, make sure your username is included in the Analysis Services Admins group.</span></span> <span data-ttu-id="f3b14-120">Další informace najdete v tématu [správci serveru](#server-administrators) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f3b14-120">To learn more, see [Server administrators](#server-administrators) later in this article.</span></span>

1. <span data-ttu-id="f3b14-121">Než připojíte, potřebujete získat název serveru.</span><span class="sxs-lookup"><span data-stu-id="f3b14-121">Before you connect, you need to get the server name.</span></span> <span data-ttu-id="f3b14-122">Na portálu **Azure Portal** > Server > **Přehled** > **Název serveru** zkopírujte název serveru.</span><span class="sxs-lookup"><span data-stu-id="f3b14-122">In **Azure portal** > server > **Overview** > **Server name**, copy the server name.</span></span>
   
    ![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="f3b14-124">V aplikaci SSMS > **Průzkumník objektů**, klikněte na tlačítko **připojit** > **služby Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="f3b14-124">In SSMS > **Object Explorer**, click **Connect** > **Analysis Services**.</span></span>
3. <span data-ttu-id="f3b14-125">V **připojit k serveru** dialogové okno, vložte v názvu serveru, a potom v **ověřování**, vyberte jednu z následujících typů ověřování:</span><span class="sxs-lookup"><span data-stu-id="f3b14-125">In the **Connect to Server** dialog box, paste in the server name, then in **Authentication**, choose one of the following authentication types:</span></span>
   
    <span data-ttu-id="f3b14-126">**Ověřování systému Windows** používat přihlašovací údaje doména\uživatelské jméno a heslo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f3b14-126">**Windows Authentication** to use your Windows domain\username and password credentials.</span></span>

    <span data-ttu-id="f3b14-127">**Ověřování hesla Active Directory** použít účet organizace.</span><span class="sxs-lookup"><span data-stu-id="f3b14-127">**Active Directory Password Authentication** to use an organizational account.</span></span> <span data-ttu-id="f3b14-128">Například když připojení z jiné domény připojený počítač.</span><span class="sxs-lookup"><span data-stu-id="f3b14-128">For example, when connecting from a non-domain joined computer.</span></span>

    <span data-ttu-id="f3b14-129">**Univerzální ověřování služby Active Directory** používat [neinteraktivní nebo vícefaktorového ověřování](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f3b14-129">**Active Directory Universal Authentication** to use [non-interactive or multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span> 
   
    ![V aplikaci SSMS připojit](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a><span data-ttu-id="f3b14-131">Správci serveru a databáze uživatelů</span><span class="sxs-lookup"><span data-stu-id="f3b14-131">Server administrators and database users</span></span>
<span data-ttu-id="f3b14-132">V Azure Analysis Services existují dva typy uživatelů, správce serveru a uživatele databáze.</span><span class="sxs-lookup"><span data-stu-id="f3b14-132">In Azure Analysis Services, there are two types of users, server administrators and database users.</span></span> <span data-ttu-id="f3b14-133">Oba typy uživatelů, musí být ve vašem Azure Active Directory a musí být určena organizační e-mailovou adresu nebo hlavní název uživatele.</span><span class="sxs-lookup"><span data-stu-id="f3b14-133">Both types of users must be in your Azure Active Directory and must be specified by organizational email address or UPN.</span></span> <span data-ttu-id="f3b14-134">Další informace najdete v tématu [Ověřování a uživatelská oprávnění](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="f3b14-134">To learn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>


## <a name="troubleshooting-connection-problems"></a><span data-ttu-id="f3b14-135">Odstraňování potíží s připojením</span><span class="sxs-lookup"><span data-stu-id="f3b14-135">Troubleshooting connection problems</span></span>
<span data-ttu-id="f3b14-136">Pokud se připojujete pomocí aplikace SSMS, pokud narazíte na problém, musíte vymazat mezipaměť přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f3b14-136">When connecting using SSMS, if you run into problems, you may need to clear the login cache.</span></span> <span data-ttu-id="f3b14-137">Nic se uloží do mezipaměti na disku. Vymazání mezipaměti, ukončete a restartujte proces připojení.</span><span class="sxs-lookup"><span data-stu-id="f3b14-137">Nothing is cached to disc. To clear the cache, close and restart the connect process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f3b14-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3b14-138">Next steps</span></span>
<span data-ttu-id="f3b14-139">Pokud již jste nenasadili tabulkový model na nový server, teď je vhodná doba.</span><span class="sxs-lookup"><span data-stu-id="f3b14-139">If you haven't already deployed a tabular model to your new server, now is a good time.</span></span> <span data-ttu-id="f3b14-140">Další informace najdete v tématu [Nasazení do služby Azure Analysis Services](analysis-services-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="f3b14-140">To learn more, see [Deploy to Azure Analysis Services](analysis-services-deploy.md).</span></span>

<span data-ttu-id="f3b14-141">Pokud model jste nasadili na server, jste připraveni připojit se pomocí klienta nebo prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f3b14-141">If you've deployed a model to your server, you're ready to connect to it using a client or browser.</span></span> <span data-ttu-id="f3b14-142">Další informace najdete v tématu [načíst data ze serveru Azure Analysis Services](analysis-services-connect.md).</span><span class="sxs-lookup"><span data-stu-id="f3b14-142">To learn more, see [Get data from Azure Analysis Services server](analysis-services-connect.md).</span></span>

