---
title: "Pomocí vzdálené plochy s rolemi Azure | Microsoft Docs"
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
ms.openlocfilehash: eab135d10c0d6df8ca72ac47d6804017a998a3d2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a><span data-ttu-id="e51ee-103">Pomocí Azure role vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="e51ee-103">Using Remote Desktop with Azure Roles</span></span>
<span data-ttu-id="e51ee-104">Pomocí sady Azure SDK a Vzdálená plocha, můžete přístup k Azure role a virtuálních počítačů, které jsou hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="e51ee-104">By using the Azure SDK and Remote Desktop Services, you can access Azure roles and virtual machines that are hosted by Azure.</span></span> <span data-ttu-id="e51ee-105">V sadě Visual Studio můžete nakonfigurovat vzdálené plochy z projektu Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="e51ee-105">In Visual Studio, you can configure Remote Desktop Services from an Azure cloud service project.</span></span> <span data-ttu-id="e51ee-106">Pokud chcete povolit vzdálenou plochu, musíte vytvořit pracovní projekt, který obsahuje jednu nebo více rolí a potom jej publikujte do Azure.</span><span class="sxs-lookup"><span data-stu-id="e51ee-106">To enable Remote Desktop Services, you must create a working project that contains one or more roles and then publish it to Azure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e51ee-107">By měl přístup Azure role pro řešení problémů nebo pouze vývoj.</span><span class="sxs-lookup"><span data-stu-id="e51ee-107">You should access an Azure role for troubleshooting or development only.</span></span> <span data-ttu-id="e51ee-108">Účelem každého virtuálního počítače je spustit určité role v aplikaci Azure, aby se nespouštěla jiné klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="e51ee-108">The purpose of each virtual machine is to run a specific role in your Azure application, not to run other client applications.</span></span> <span data-ttu-id="e51ee-109">Pokud chcete používat Azure jako hostitele virtuálního počítače, který můžete použít pro jakýkoli účel, najdete v části přístup virtuálních počítačů Azure z Průzkumníka serveru.</span><span class="sxs-lookup"><span data-stu-id="e51ee-109">If you want to use Azure to host a virtual machine that you can use for any purpose, see Accessing Azure Virtual Machines from Server Explorer.</span></span>
> 
> 

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a><span data-ttu-id="e51ee-110">Povolení a použití služby Vzdálená plocha pro Role v Azure</span><span class="sxs-lookup"><span data-stu-id="e51ee-110">To enable and use Remote Desktop for an Azure Role</span></span>
1. <span data-ttu-id="e51ee-111">V Průzkumníku řešení otevřete místní nabídku pro projekt cloudové služby a potom zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="e51ee-111">In Solution Explorer, open the shortcut menu for your cloud service project, and then choose **Publish**.</span></span>
   
    <span data-ttu-id="e51ee-112">**Publikování aplikaci Azure** zobrazí se průvodce.</span><span class="sxs-lookup"><span data-stu-id="e51ee-112">The **Publish Azure Application** wizard appears.</span></span>
   
    ![Publikování příkaz pro projekt cloudové služby](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. <span data-ttu-id="e51ee-114">V dolní části **nastavením publikování v Microsoft Azure** stránce průvodce vyberte **povolení vzdálené plochy** pro všechny role zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="e51ee-114">At the bottom of **Microsoft Azure Publish Settings** page of the wizard, select the **Enable Remote Desktop** for all roles check box.</span></span> 
   
    <span data-ttu-id="e51ee-115">**Konfigurace vzdálené plochy** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e51ee-115">The **Remote Desktop Configuration** dialog box appears.</span></span>
3. <span data-ttu-id="e51ee-116">V dolní části **konfigurace vzdálené plochy** dialogovém okně vyberte **další možnosti** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e51ee-116">At the bottom of the **Remote Desktop Configuration** dialog box, choose the **More Options** button.</span></span> 
   
    <span data-ttu-id="e51ee-117">Zobrazí rozevírací seznam, který umožňuje vytvořit nebo vybrat si certifikát tak, aby při připojování přes vzdálenou plochu můžete šifrovat přihlašovací údaje informace.</span><span class="sxs-lookup"><span data-stu-id="e51ee-117">This displays a dropdown list box that lets you create or choose a certificate so that you can encrypt credentials information when connecting via remote desktop.</span></span>
4. <span data-ttu-id="e51ee-118">V rozevíracím seznamu vyberte  **&lt;vytvořit >**, nebo vyberte existující šablonu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="e51ee-118">In the dropdown list, choose **&lt;Create>**, or choose an existing one from the list.</span></span> 
   
    <span data-ttu-id="e51ee-119">Pokud zvolíte existující certifikát, můžete přeskočte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="e51ee-119">If you choose an existing certificate, skip the following steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e51ee-120">Certifikáty, které potřebujete pro připojení ke vzdálené ploše se liší od certifikáty, které používáte pro jiné operace v Azure.</span><span class="sxs-lookup"><span data-stu-id="e51ee-120">The certificates that you need for a remote desktop connection are different from the certificates that you use for other Azure operations.</span></span> <span data-ttu-id="e51ee-121">Certifikát vzdáleného přístupu musí mít privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="e51ee-121">The remote access certificate must have a private key.</span></span>
   > 
   > 
   
    <span data-ttu-id="e51ee-122">**Vytvořit certifikát** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e51ee-122">The **Create Certificate** dialog box appears.</span></span>
   
   1. <span data-ttu-id="e51ee-123">Zadejte popisný název pro nový certifikát a potom vyberte **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e51ee-123">Provide a friendly name for the new certificate, and then choose the **OK** button.</span></span> <span data-ttu-id="e51ee-124">V rozevíracím seznamu se zobrazí nový certifikát.</span><span class="sxs-lookup"><span data-stu-id="e51ee-124">The new certificate appears in the dropdown list box.</span></span>
   2. <span data-ttu-id="e51ee-125">V **konfigurace vzdálené plochy** dialogovém okně zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="e51ee-125">In the **Remote Desktop Configuration** dialog box, provide a user name and a password.</span></span>
      
       <span data-ttu-id="e51ee-126">Nelze použít existující účet.</span><span class="sxs-lookup"><span data-stu-id="e51ee-126">You can’t use an existing account.</span></span> <span data-ttu-id="e51ee-127">Nezadávejte jako uživatelské jméno pro nový účet správce.</span><span class="sxs-lookup"><span data-stu-id="e51ee-127">Don’t specify Administrator as the user name for the new account.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="e51ee-128">Pokud heslo nesplňuje požadavky na složitost, zobrazí se červená ikona vedle textového pole heslo.</span><span class="sxs-lookup"><span data-stu-id="e51ee-128">If the password doesn’t meet the complexity requirements, a red icon appears next to the password text box.</span></span> <span data-ttu-id="e51ee-129">Heslo musí obsahovat velká písmena, malá písmena a čísla nebo symboly.</span><span class="sxs-lookup"><span data-stu-id="e51ee-129">The password must include capital letters, lowercase letters, and numbers or symbols.</span></span>
      > 
      > 
   3. <span data-ttu-id="e51ee-130">Zvolte datum, na který platnosti účtu a po které připojení ke vzdálené ploše se zablokuje.</span><span class="sxs-lookup"><span data-stu-id="e51ee-130">Choose a date on which the account will expire and after which remote desktop connections will be blocked.</span></span>
   4. <span data-ttu-id="e51ee-131">Po všechny požadované informace, které jste zadali, klikněte **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e51ee-131">After you've provided all the required information, choose the **OK** button.</span></span>
      
       <span data-ttu-id="e51ee-132">Několik nastavení, které umožňují přístup k vzdálené se přidají do soubory .cscfg a .csdef.</span><span class="sxs-lookup"><span data-stu-id="e51ee-132">Several settings that enable Remote Access Services are added to the .cscfg and .csdef files.</span></span>
5. <span data-ttu-id="e51ee-133">V **nastavením publikování v Microsoft Azure** průvodce, vyberte **OK** tlačítko až budete připraveni k publikování cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e51ee-133">In the **Microsoft Azure Publish Settings** wizard, choose the **OK** button when you’re ready to publish your cloud service.</span></span>
   
    <span data-ttu-id="e51ee-134">Pokud si nejste připravena k publikování, vyberte **zrušit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e51ee-134">If you're not ready to publish, choose the **Cancel** button.</span></span> <span data-ttu-id="e51ee-135">Konfigurace nastavení se ukládají a cloudové služby můžete publikovat později.</span><span class="sxs-lookup"><span data-stu-id="e51ee-135">The configuration settings are saved, and you can publish your cloud service later.</span></span>

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a><span data-ttu-id="e51ee-136">Připojení do Azure Role pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="e51ee-136">Connect to an Azure Role by using Remote Desktop</span></span>
<span data-ttu-id="e51ee-137">Po publikování cloudové služby na platformě Azure můžete Průzkumníka serveru pro přihlášení na virtuální počítače, který je hostitelem Azure.</span><span class="sxs-lookup"><span data-stu-id="e51ee-137">After you publish your cloud service on Azure, you can use Server Explorer to log into the virtual machines that Azure hosts.</span></span> 

1. <span data-ttu-id="e51ee-138">V Průzkumníku serveru rozbalte **Azure** uzel a pak rozbalte uzel pro cloudové služby a jeden z jeho role můžete zobrazit seznam instancí.</span><span class="sxs-lookup"><span data-stu-id="e51ee-138">In Server Explorer, expand the **Azure** node, and then expand the node for a cloud service and one of its roles to display a list of instances.</span></span>
2. <span data-ttu-id="e51ee-139">Otevřete místní nabídku pro uzel instanci a potom zvolte **připojit pomocí vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="e51ee-139">Open the shortcut menu for an instance node, and then choose **Connect Using Remote Desktop**.</span></span>
   
    ![Připojování přes vzdálenou plochu](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. <span data-ttu-id="e51ee-141">Zadejte uživatelské jméno a heslo, které jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e51ee-141">Enter the user name and password that you created previously.</span></span> <span data-ttu-id="e51ee-142">Nyní jste přihlášeni ke vzdálené relaci.</span><span class="sxs-lookup"><span data-stu-id="e51ee-142">You are now logged into your remote session.</span></span>

