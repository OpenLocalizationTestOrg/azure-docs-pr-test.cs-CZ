---
title: "aaaUsing vzdálené plochy s rolemi Azure | Microsoft Docs"
description: "Pomocí Azure role vzdálené plochy"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d35fd421cde8be9e3caa474db95974a54e528bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a><span data-ttu-id="3aa58-103">Pomocí Azure role vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="3aa58-103">Using Remote Desktop with Azure Roles</span></span>
<span data-ttu-id="3aa58-104">Pomocí hello Azure SDK a Vzdálená plocha mají přístup k Azure role a virtuálních počítačů, které jsou hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="3aa58-104">By using hello Azure SDK and Remote Desktop Services, you can access Azure roles and virtual machines that are hosted by Azure.</span></span> <span data-ttu-id="3aa58-105">V sadě Visual Studio můžete nakonfigurovat vzdálené plochy z projektu Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="3aa58-105">In Visual Studio, you can configure Remote Desktop Services from an Azure cloud service project.</span></span> <span data-ttu-id="3aa58-106">tooenable služby Vzdálená plocha, je nutné vytvořit pracovní projekt, který obsahuje jednu nebo více rolí a pak ho publikujete tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3aa58-106">tooenable Remote Desktop Services, you must create a working project that contains one or more roles and then publish it tooAzure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3aa58-107">By měl přístup Azure role pro řešení problémů nebo pouze vývoj.</span><span class="sxs-lookup"><span data-stu-id="3aa58-107">You should access an Azure role for troubleshooting or development only.</span></span> <span data-ttu-id="3aa58-108">Hello účel každého virtuálního počítače je toorun určité role v aplikaci Azure, ne toorun jiné klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="3aa58-108">hello purpose of each virtual machine is toorun a specific role in your Azure application, not toorun other client applications.</span></span> <span data-ttu-id="3aa58-109">Pokud chcete toouse Azure toohost virtuální počítač, který můžete použít pro jakýkoli účel, najdete v části přístup virtuálních počítačů Azure z Průzkumníka serveru.</span><span class="sxs-lookup"><span data-stu-id="3aa58-109">If you want toouse Azure toohost a virtual machine that you can use for any purpose, see Accessing Azure Virtual Machines from Server Explorer.</span></span>
> 
> 

## <a name="tooenable-and-use-remote-desktop-for-an-azure-role"></a><span data-ttu-id="3aa58-110">tooenable a používání vzdálené plochy pro Role v Azure</span><span class="sxs-lookup"><span data-stu-id="3aa58-110">tooenable and use Remote Desktop for an Azure Role</span></span>
1. <span data-ttu-id="3aa58-111">V Průzkumníku řešení otevřete hello místní nabídku pro projekt cloudové služby a potom zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="3aa58-111">In Solution Explorer, open hello shortcut menu for your cloud service project, and then choose **Publish**.</span></span>
   
    <span data-ttu-id="3aa58-112">Hello **publikování aplikaci Azure** zobrazí se průvodce.</span><span class="sxs-lookup"><span data-stu-id="3aa58-112">hello **Publish Azure Application** wizard appears.</span></span>
   
    ![Publikování příkaz pro projekt cloudové služby](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. <span data-ttu-id="3aa58-114">Na konci hello **nastavením publikování v Microsoft Azure** hello průvodce vyberte hello **povolení vzdálené plochy** pro všechny role zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="3aa58-114">At hello bottom of **Microsoft Azure Publish Settings** page of hello wizard, select hello **Enable Remote Desktop** for all roles check box.</span></span> 
   
    <span data-ttu-id="3aa58-115">Hello **konfigurace vzdálené plochy** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aa58-115">hello **Remote Desktop Configuration** dialog box appears.</span></span>
3. <span data-ttu-id="3aa58-116">Na konci hello hello **konfigurace vzdálené plochy** dialogovém okně vyberte hello **další možnosti** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3aa58-116">At hello bottom of hello **Remote Desktop Configuration** dialog box, choose hello **More Options** button.</span></span> 
   
    <span data-ttu-id="3aa58-117">Zobrazí rozevírací seznam, který umožňuje vytvořit nebo vybrat si certifikát tak, aby při připojování přes vzdálenou plochu můžete šifrovat přihlašovací údaje informace.</span><span class="sxs-lookup"><span data-stu-id="3aa58-117">This displays a dropdown list box that lets you create or choose a certificate so that you can encrypt credentials information when connecting via remote desktop.</span></span>
4. <span data-ttu-id="3aa58-118">V rozevíracím seznamu hello, zvolte  **&lt;vytvořit >**, nebo vyberte existující šablonu ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="3aa58-118">In hello dropdown list, choose **&lt;Create>**, or choose an existing one from hello list.</span></span> 
   
    <span data-ttu-id="3aa58-119">Pokud zvolíte existující certifikát, přeskočte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="3aa58-119">If you choose an existing certificate, skip hello following steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3aa58-120">Hello certifikáty, které potřebujete pro připojení ke vzdálené ploše se liší od hello certifikáty, které používáte pro jiné operace v Azure.</span><span class="sxs-lookup"><span data-stu-id="3aa58-120">hello certificates that you need for a remote desktop connection are different from hello certificates that you use for other Azure operations.</span></span> <span data-ttu-id="3aa58-121">certifikát Hello vzdáleného přístupu musí mít privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="3aa58-121">hello remote access certificate must have a private key.</span></span>
   > 
   > 
   
    <span data-ttu-id="3aa58-122">Hello **vytvořit certifikát** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aa58-122">hello **Create Certificate** dialog box appears.</span></span>
   
   1. <span data-ttu-id="3aa58-123">Zadejte popisný název pro nový certifikát hello a potom vyberte hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3aa58-123">Provide a friendly name for hello new certificate, and then choose hello **OK** button.</span></span> <span data-ttu-id="3aa58-124">v rozevíracím seznamu hello se zobrazí nový certifikát Hello.</span><span class="sxs-lookup"><span data-stu-id="3aa58-124">hello new certificate appears in hello dropdown list box.</span></span>
   2. <span data-ttu-id="3aa58-125">V hello **konfigurace vzdálené plochy** dialogovém okně zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="3aa58-125">In hello **Remote Desktop Configuration** dialog box, provide a user name and a password.</span></span>
      
       <span data-ttu-id="3aa58-126">Nelze použít existující účet.</span><span class="sxs-lookup"><span data-stu-id="3aa58-126">You can’t use an existing account.</span></span> <span data-ttu-id="3aa58-127">Nezadávejte správce jako hello uživatelské jméno pro nový účet hello.</span><span class="sxs-lookup"><span data-stu-id="3aa58-127">Don’t specify Administrator as hello user name for hello new account.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="3aa58-128">Pokud hello heslo nesplňuje požadavky na složitost hello, červenou ikonu se zobrazí další toohello heslo textové pole.</span><span class="sxs-lookup"><span data-stu-id="3aa58-128">If hello password doesn’t meet hello complexity requirements, a red icon appears next toohello password text box.</span></span> <span data-ttu-id="3aa58-129">Hello heslo musí obsahovat velká písmena, malá písmena a čísla nebo symboly.</span><span class="sxs-lookup"><span data-stu-id="3aa58-129">hello password must include capital letters, lowercase letters, and numbers or symbols.</span></span>
      > 
      > 
   3. <span data-ttu-id="3aa58-130">Zvolte datum, na který účet hello vyprší a po které připojení ke vzdálené ploše se zablokuje.</span><span class="sxs-lookup"><span data-stu-id="3aa58-130">Choose a date on which hello account will expire and after which remote desktop connections will be blocked.</span></span>
   4. <span data-ttu-id="3aa58-131">Poté, co jste zadaný všechny hello požadované informace, vyberte hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3aa58-131">After you've provided all hello required information, choose hello **OK** button.</span></span>
      
       <span data-ttu-id="3aa58-132">Několik nastavení, které umožňují přístup k vzdálené se přidají toohello soubory .cscfg a .csdef.</span><span class="sxs-lookup"><span data-stu-id="3aa58-132">Several settings that enable Remote Access Services are added toohello .cscfg and .csdef files.</span></span>
5. <span data-ttu-id="3aa58-133">V hello **nastavením publikování v Microsoft Azure** průvodce, zvolte hello **OK** tlačítko až budete připraveni toopublish cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="3aa58-133">In hello **Microsoft Azure Publish Settings** wizard, choose hello **OK** button when you’re ready toopublish your cloud service.</span></span>
   
    <span data-ttu-id="3aa58-134">Pokud si nejste připravené toopublish, zvolte hello **zrušit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3aa58-134">If you're not ready toopublish, choose hello **Cancel** button.</span></span> <span data-ttu-id="3aa58-135">nastavení konfigurace Hello se ukládají a cloudové služby můžete publikovat později.</span><span class="sxs-lookup"><span data-stu-id="3aa58-135">hello configuration settings are saved, and you can publish your cloud service later.</span></span>

## <a name="connect-tooan-azure-role-by-using-remote-desktop"></a><span data-ttu-id="3aa58-136">Připojit tooan Role v Azure pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="3aa58-136">Connect tooan Azure Role by using Remote Desktop</span></span>
<span data-ttu-id="3aa58-137">Po publikování cloudové služby na platformě Azure, můžete použít Průzkumníka serveru toolog do virtuálních počítačů hello který je hostitelem Azure.</span><span class="sxs-lookup"><span data-stu-id="3aa58-137">After you publish your cloud service on Azure, you can use Server Explorer toolog into hello virtual machines that Azure hosts.</span></span> 

1. <span data-ttu-id="3aa58-138">V Průzkumníku serveru rozbalte hello **Azure** uzel a potom rozbalte uzel hello pro cloudové služby a jeden z jeho role toodisplay seznam instancí.</span><span class="sxs-lookup"><span data-stu-id="3aa58-138">In Server Explorer, expand hello **Azure** node, and then expand hello node for a cloud service and one of its roles toodisplay a list of instances.</span></span>
2. <span data-ttu-id="3aa58-139">Otevřete hello místní nabídky pro uzel instanci a potom zvolte **připojit pomocí vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="3aa58-139">Open hello shortcut menu for an instance node, and then choose **Connect Using Remote Desktop**.</span></span>
   
    ![Připojování přes vzdálenou plochu](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. <span data-ttu-id="3aa58-141">Zadejte hello uživatelské jméno a heslo, které jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3aa58-141">Enter hello user name and password that you created previously.</span></span> <span data-ttu-id="3aa58-142">Nyní jste přihlášeni ke vzdálené relaci.</span><span class="sxs-lookup"><span data-stu-id="3aa58-142">You are now logged into your remote session.</span></span>

