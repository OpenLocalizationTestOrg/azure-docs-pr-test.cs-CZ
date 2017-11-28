---
title: aaaManage Azure Analysis Services | Microsoft Docs
description: "Zjistěte, jak toomanage Analysis Services serveru v Azure."
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
ms.openlocfilehash: b03bc440801a68162039e28cdb4f863da374014e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-analysis-services"></a><span data-ttu-id="0e59e-103">Správa služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="0e59e-103">Manage Analysis Services</span></span>
<span data-ttu-id="0e59e-104">Po vytvoření serveru služby Analysis Services v Azure, může být některé úlohy správy a správy musíte tooperform hned nebo zopakovat dolů silniční hello.</span><span class="sxs-lookup"><span data-stu-id="0e59e-104">Once you've created an Analysis Services server in Azure, there may be some administration and management tasks you need tooperform right away or sometime down hello road.</span></span> <span data-ttu-id="0e59e-105">Například spusťte zpracování toohello aktualizace dat, určovat, kdo může získat přístup hello modely na vašem serveru, nebo sledování stavu vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="0e59e-105">For example, run processing toohello refresh data, control who can access hello models on your server, or monitor your server's health.</span></span> <span data-ttu-id="0e59e-106">Některé úlohy správy lze provést pouze na portálu Azure, jiné v SQL Server Management Studio (SSMS) a některé úlohy lze provádět buď.</span><span class="sxs-lookup"><span data-stu-id="0e59e-106">Some management tasks can only be performed in Azure portal, others in SQL Server Management Studio (SSMS), and some tasks can be done in either.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="0e59e-107">portál Azure</span><span class="sxs-lookup"><span data-stu-id="0e59e-107">Azure portal</span></span>
<span data-ttu-id="0e59e-108">[Portál Azure](http://portal.azure.com/) je, kde můžete vytvořit a odstraněním serverů, monitorování prostředky serveru, změnit velikost a nastavit, kdo má přístup tooyour servery.</span><span class="sxs-lookup"><span data-stu-id="0e59e-108">[Azure portal](http://portal.azure.com/) is where you can create and delete servers, monitor server resources, change size, and manage who has access tooyour servers.</span></span>  <span data-ttu-id="0e59e-109">Pokud máte nějaké problémy, můžete také odeslat žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="0e59e-109">If you're having some problems, you can also submit a support request.</span></span>

![Získání názvu serveru v Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a><span data-ttu-id="0e59e-111">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="0e59e-111">SQL Server Management Studio</span></span>
<span data-ttu-id="0e59e-112">Připojení serveru tooyour v Azure je stejně jako připojení tooa instance serveru ve vaší vlastní organizaci.</span><span class="sxs-lookup"><span data-stu-id="0e59e-112">Connecting tooyour server in Azure is just like connecting tooa server instance in your own organization.</span></span> <span data-ttu-id="0e59e-113">Pomocí aplikace SSMS je možné provádět řadu hello stejné úlohy, jako je například zpracování dat nebo vytvořte skript zpracování, správě rolí a pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0e59e-113">From SSMS, you can perform many of hello same tasks such as process data or create a processing script, manage roles, and use PowerShell.</span></span>
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a><span data-ttu-id="0e59e-115">Stažení a instalace aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="0e59e-115">Download and install SSMS</span></span>
<span data-ttu-id="0e59e-116">tooget všechny hello nejnovější funkce a možnosti nejhladší hello při připojování serveru Azure Analysis Services tooyour, ujistěte se, že používáte nejnovější verzi aplikace SSMS hello.</span><span class="sxs-lookup"><span data-stu-id="0e59e-116">tooget all hello latest features, and hello smoothest experience when connecting tooyour Azure Analysis Services server, be sure you're using hello latest version of SSMS.</span></span> 

<span data-ttu-id="0e59e-117">[Stáhněte si SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="0e59e-117">[Download SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>


### <a name="tooconnect-with-ssms"></a><span data-ttu-id="0e59e-118">tooconnect pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="0e59e-118">tooconnect with SSMS</span></span>
 <span data-ttu-id="0e59e-119">Při použití aplikace SSMS, než server hello připojování tooyour poprvé, ujistěte se, že vaše uživatelské jméno je součástí skupiny Admins služby Analysis hello.</span><span class="sxs-lookup"><span data-stu-id="0e59e-119">When using SSMS, before connecting tooyour server hello first time, make sure your username is included in hello Analysis Services Admins group.</span></span> <span data-ttu-id="0e59e-120">Další, najdete v části toolearn [správci serveru](#server-administrators) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="0e59e-120">toolearn more, see [Server administrators](#server-administrators) later in this article.</span></span>

1. <span data-ttu-id="0e59e-121">Než připojíte, je třeba název serveru tooget hello.</span><span class="sxs-lookup"><span data-stu-id="0e59e-121">Before you connect, you need tooget hello server name.</span></span> <span data-ttu-id="0e59e-122">V **portál Azure** > serveru > **přehled** > **název serveru**, název serveru hello kopie.</span><span class="sxs-lookup"><span data-stu-id="0e59e-122">In **Azure portal** > server > **Overview** > **Server name**, copy hello server name.</span></span>
   
    ![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="0e59e-124">V aplikaci SSMS > **Průzkumník objektů**, klikněte na tlačítko **připojit** > **služby Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="0e59e-124">In SSMS > **Object Explorer**, click **Connect** > **Analysis Services**.</span></span>
3. <span data-ttu-id="0e59e-125">V hello **připojit tooServer** dialogové okno, vložte v hello název serveru, a potom v **ověřování**, vyberte jednu z hello následující typy ověřování:</span><span class="sxs-lookup"><span data-stu-id="0e59e-125">In hello **Connect tooServer** dialog box, paste in hello server name, then in **Authentication**, choose one of hello following authentication types:</span></span>
   
    <span data-ttu-id="0e59e-126">**Ověřování systému Windows** toouse přihlašovací údaje doména\uživatelské jméno a heslo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="0e59e-126">**Windows Authentication** toouse your Windows domain\username and password credentials.</span></span>

    <span data-ttu-id="0e59e-127">**Ověřování hesla Active Directory** toouse účet organizace.</span><span class="sxs-lookup"><span data-stu-id="0e59e-127">**Active Directory Password Authentication** toouse an organizational account.</span></span> <span data-ttu-id="0e59e-128">Například když připojení z jiné domény připojený počítač.</span><span class="sxs-lookup"><span data-stu-id="0e59e-128">For example, when connecting from a non-domain joined computer.</span></span>

    <span data-ttu-id="0e59e-129">**Univerzální ověřování služby Active Directory** toouse [neinteraktivní nebo vícefaktorového ověřování](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="0e59e-129">**Active Directory Universal Authentication** toouse [non-interactive or multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span> 
   
    ![V aplikaci SSMS připojit](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a><span data-ttu-id="0e59e-131">Správci serveru a databáze uživatelů</span><span class="sxs-lookup"><span data-stu-id="0e59e-131">Server administrators and database users</span></span>
<span data-ttu-id="0e59e-132">V Azure Analysis Services existují dva typy uživatelů, správce serveru a uživatele databáze.</span><span class="sxs-lookup"><span data-stu-id="0e59e-132">In Azure Analysis Services, there are two types of users, server administrators and database users.</span></span> <span data-ttu-id="0e59e-133">Oba typy uživatelů, musí být ve vašem Azure Active Directory a musí být určena organizační e-mailovou adresu nebo hlavní název uživatele.</span><span class="sxs-lookup"><span data-stu-id="0e59e-133">Both types of users must be in your Azure Active Directory and must be specified by organizational email address or UPN.</span></span> <span data-ttu-id="0e59e-134">Další, najdete v části toolearn [ověřování a uživatel oprávnění](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="0e59e-134">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>


## <a name="troubleshooting-connection-problems"></a><span data-ttu-id="0e59e-135">Odstraňování potíží s připojením</span><span class="sxs-lookup"><span data-stu-id="0e59e-135">Troubleshooting connection problems</span></span>
<span data-ttu-id="0e59e-136">Pokud se připojujete pomocí aplikace SSMS, pokud narazíte na problém, může být nutné tooclear hello přihlášení mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0e59e-136">When connecting using SSMS, if you run into problems, you may need tooclear hello login cache.</span></span> <span data-ttu-id="0e59e-137">Nic je uložená v mezipaměti toodisc.</span><span class="sxs-lookup"><span data-stu-id="0e59e-137">Nothing is cached toodisc.</span></span> <span data-ttu-id="0e59e-138">tooclear mezipaměti hello, zavřete a restartujte hello připojit proces.</span><span class="sxs-lookup"><span data-stu-id="0e59e-138">tooclear hello cache, close and restart hello connect process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0e59e-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e59e-139">Next steps</span></span>
<span data-ttu-id="0e59e-140">Pokud již jste nenasadili nový server tooyour tabulkový model, teď je vhodná doba.</span><span class="sxs-lookup"><span data-stu-id="0e59e-140">If you haven't already deployed a tabular model tooyour new server, now is a good time.</span></span> <span data-ttu-id="0e59e-141">Další, najdete v části toolearn [nasazení služby Analysis Services tooAzure](analysis-services-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="0e59e-141">toolearn more, see [Deploy tooAzure Analysis Services](analysis-services-deploy.md).</span></span>

<span data-ttu-id="0e59e-142">Pokud jste nasadili server tooyour modelu, jste připravené tooconnect tooit pomocí klienta nebo prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0e59e-142">If you've deployed a model tooyour server, you're ready tooconnect tooit using a client or browser.</span></span> <span data-ttu-id="0e59e-143">Další, najdete v části toolearn [načíst data ze serveru Azure Analysis Services](analysis-services-connect.md).</span><span class="sxs-lookup"><span data-stu-id="0e59e-143">toolearn more, see [Get data from Azure Analysis Services server](analysis-services-connect.md).</span></span>

