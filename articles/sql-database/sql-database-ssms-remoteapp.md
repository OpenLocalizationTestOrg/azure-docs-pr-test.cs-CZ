---
title: "Připojení k SQL Database pomocí SQL Server Management Studio v Azure Remoteappu | Microsoft Docs"
description: "Použití v tomto kurzu se dozvíte, jak používat SQL Server Management Studio v Azure Remoteappu pro zabezpečení a výkonu při připojení k databázi SQL"
services: sql-database
documentationcenter: 
author: adhurwit
manager: jhubbard
ms.assetid: 1052c83c-e7f5-4736-922f-216194d8874b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: adhurwit
ms.openlocfilehash: ae1f2fa38d38fe6c10bc7960fddb07ae330d1eeb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a><span data-ttu-id="ec6ae-103">Pomocí SQL Server Management Studio v Azure Remoteappu pro připojení k databázi SQL</span><span class="sxs-lookup"><span data-stu-id="ec6ae-103">Use SQL Server Management Studio in Azure RemoteApp to connect to SQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec6ae-104">Azure RemoteApp se přestává používat.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-104">Azure RemoteApp is being discontinued.</span></span> <span data-ttu-id="ec6ae-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="ec6ae-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
>

## <a name="introduction"></a><span data-ttu-id="ec6ae-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="ec6ae-106">Introduction</span></span>
<span data-ttu-id="ec6ae-107">V tomto kurzu se dozvíte, jak používat SQL Server Management Studio (SSMS) v Azure Remoteappu pro připojení k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-107">This tutorial shows you how to use SQL Server Management Studio (SSMS) in Azure RemoteApp to connect to SQL Database.</span></span> <span data-ttu-id="ec6ae-108">Vás provede procesem nastavení služby SQL Server Management Studio v Azure Remoteappu, vysvětluje výhody a zobrazuje funkce zabezpečení, které můžete použít v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-108">It walks you through the process of setting up SQL Server Management Studio in Azure RemoteApp, explains the benefits, and shows security features that you can use in Azure Active Directory.</span></span>

<span data-ttu-id="ec6ae-109">**Odhadovaný čas dokončení:** 45 minut</span><span class="sxs-lookup"><span data-stu-id="ec6ae-109">**Estimated time to complete:** 45 minutes</span></span>

## <a name="ssms-in-azure-remoteapp"></a><span data-ttu-id="ec6ae-110">Aplikace SSMS v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="ec6ae-110">SSMS in Azure RemoteApp</span></span>
<span data-ttu-id="ec6ae-111">Azure RemoteApp je služba Vzdálená plocha v Azure, který zajišťuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-111">Azure RemoteApp is an RDS service in Azure that delivers applications.</span></span> <span data-ttu-id="ec6ae-112">Další informace o něm zde: [co je RemoteApp?](../remoteapp/remoteapp-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="ec6ae-112">You can learn more about it here: [What is RemoteApp?](../remoteapp/remoteapp-whatis.md)</span></span>

<span data-ttu-id="ec6ae-113">Aplikace SSMS spuštění v Azure Remoteappu vám dává stejné prostředí jako aplikace SSMS spuštěná místně.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-113">SSMS running in Azure RemoteApp gives you the same experience as running SSMS locally.</span></span>

![Snímek obrazovky aplikace SSMS spuštění v Azure Remoteappu][1]

## <a name="benefits"></a><span data-ttu-id="ec6ae-115">Výhody</span><span class="sxs-lookup"><span data-stu-id="ec6ae-115">Benefits</span></span>
<span data-ttu-id="ec6ae-116">Existuje mnoho výhod pomocí aplikace SSMS v Azure Remoteappu, včetně:</span><span class="sxs-lookup"><span data-stu-id="ec6ae-116">There are many benefits to using SSMS in Azure RemoteApp, including:</span></span>

* <span data-ttu-id="ec6ae-117">Na serveru Azure SQL port 1433 nemá mají být exponovány externě (mimo Azure).</span><span class="sxs-lookup"><span data-stu-id="ec6ae-117">Port 1433 on Azure SQL server does not have to be exposed externally (outside of Azure).</span></span>
* <span data-ttu-id="ec6ae-118">Není potřeba zachovat přidávání a odebírání adres IP v bráně firewall serveru Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-118">No need to keep adding and removing IP addresses in the Azure SQL server firewall.</span></span>
* <span data-ttu-id="ec6ae-119">Všechna připojení Azure RemoteApp probíhá přes protokol HTTPS na portu 443 pomocí šifrované protokolu vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="ec6ae-119">All Azure RemoteApp connections occur over HTTPS on port 443 using encrypted Remote Desktop protocol</span></span>
* <span data-ttu-id="ec6ae-120">Je více uživatelů a možné škálovat.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-120">It is multi-user and can scale.</span></span>
* <span data-ttu-id="ec6ae-121">Je výkonnější z s SSMS ve stejné oblasti jako databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-121">There is a performance gain from having SSMS in the same region as the SQL Database.</span></span>
* <span data-ttu-id="ec6ae-122">Můžete auditovat použití služby Azure RemoteApp s na edici Premium služby Azure Active Directory, který má protokolování aktivit uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-122">You can audit use of Azure RemoteApp with the Premium edition of Azure Active Directory which has user activity reports.</span></span>
* <span data-ttu-id="ec6ae-123">Můžete povolit vícefaktorové ověřování (MFA).</span><span class="sxs-lookup"><span data-stu-id="ec6ae-123">You can enable multi-factor authentication (MFA).</span></span>
* <span data-ttu-id="ec6ae-124">Přístup aplikace SSMS kdekoli při použití některého z podporovaných klientů Azure RemoteApp, včetně iOS, Android, Mac, Windows Phone a počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-124">Access SSMS anywhere when using any of the supported Azure RemoteApp clients which includes iOS, Android, Mac, Windows Phone, and Windows PC’s.</span></span>

## <a name="create-the-azure-remoteapp-collection"></a><span data-ttu-id="ec6ae-125">Vytvořte kolekci Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="ec6ae-125">Create the Azure RemoteApp collection</span></span>
<span data-ttu-id="ec6ae-126">Tady jsou kroky k vytvoření kolekce vzdálené aplikace Azure RemoteApp pomocí SSMS:</span><span class="sxs-lookup"><span data-stu-id="ec6ae-126">Here are the steps to create your Azure RemoteApp collection with SSMS:</span></span>

### <a name="1-create-a-new-windows-vm-from-image"></a><span data-ttu-id="ec6ae-127">1. Vytvoření nového virtuálního počítače Windows z bitové kopie</span><span class="sxs-lookup"><span data-stu-id="ec6ae-127">1. Create a new Windows VM from Image</span></span>
<span data-ttu-id="ec6ae-128">Chcete-li nový virtuální počítač použijte Image "Windows Server vzdálené plochy relace hostitele Windows serveru 2012 R2" z galerie.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-128">Use the "Windows Server Remote Desktop Session Host Windows Server 2012 R2" Image from the Gallery to make your new VM.</span></span>

### <a name="2-install-ssms-from-sql-express"></a><span data-ttu-id="ec6ae-129">2. Nainstalujte aplikaci SSMS z SQL Express.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-129">2. Install SSMS from SQL Express</span></span>
<span data-ttu-id="ec6ae-130">Přejděte na nový virtuální počítač a přejděte na tuto stránku stahování: [Express Microsoft® SQL Server® 2014](https://www.microsoft.com/download/details.aspx?id=42299)</span><span class="sxs-lookup"><span data-stu-id="ec6ae-130">Go onto the new VM and navigate to this download page: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)</span></span>

<span data-ttu-id="ec6ae-131">Je k dispozici možnost pro stahování pouze SSMS.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-131">There is an option to only download SSMS.</span></span> <span data-ttu-id="ec6ae-132">Po stažení přejděte do instalačního adresáře a spusťte instalaci aplikace SSMS.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-132">After download, go into the install directory and run Setup to install SSMS.</span></span>

<span data-ttu-id="ec6ae-133">Také musíte nainstalovat aktualizaci Service Pack 1 pro systém SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-133">You also need to install SQL Server 2014 Service Pack 1.</span></span> <span data-ttu-id="ec6ae-134">Si můžete stáhnout tady: [aktualizace Service Pack 1 (SP1) pro systém Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=46694)</span><span class="sxs-lookup"><span data-stu-id="ec6ae-134">You can download it here: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)</span></span>

<span data-ttu-id="ec6ae-135">SQL Server 2014 Service Pack 1 zahrnuje základní funkce pro práci s Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-135">SQL Server 2014 Service Pack 1 includes essential functionality for working with Azure SQL Database.</span></span>

### <a name="3-run-validate-script-and-sysprep"></a><span data-ttu-id="ec6ae-136">3. Spuštění skriptu ověřením a nástroj Sysprep</span><span class="sxs-lookup"><span data-stu-id="ec6ae-136">3. Run Validate script and Sysprep</span></span>
<span data-ttu-id="ec6ae-137">Na ploše virtuálního počítače je skript prostředí PowerShell volána ověřením.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-137">On the desktop of the VM is a PowerShell script called Validate.</span></span> <span data-ttu-id="ec6ae-138">Spusťte tento dvojitým kliknutím.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-138">Run this by double-clicking.</span></span> <span data-ttu-id="ec6ae-139">Ověří, že je virtuální počítač připravený k použití pro vzdálené hostování aplikací.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-139">It will verify that the VM is ready to be used for remote hosting of applications.</span></span> <span data-ttu-id="ec6ae-140">Po dokončení ověření se zobrazí dotaz spustit nástroj sysprep-zvolte ji spustit.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-140">When verification is complete, it will ask to run sysprep - choose to run it.</span></span>

<span data-ttu-id="ec6ae-141">Po dokončení nástroj sysprep vypne virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-141">When sysprep completes, it will shut down the VM.</span></span>

<span data-ttu-id="ec6ae-142">Další informace o vytváření obrazem Azure RemoteApp najdete v tématu: [postup vytvoření image šablony vzdálené aplikace RemoteApp v Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="ec6ae-142">To learn more about creating a Azure RemoteApp image, see: [How to create a RemoteApp template image in Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span></span>

### <a name="4-capture-image"></a><span data-ttu-id="ec6ae-143">4. Zachycení bitové kopie</span><span class="sxs-lookup"><span data-stu-id="ec6ae-143">4. Capture image</span></span>
<span data-ttu-id="ec6ae-144">Když virtuální počítač se zastavila, najít na aktuálním portálu a zachycení ho.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-144">When the VM has stopped running, find it in the current portal and capture it.</span></span>

<span data-ttu-id="ec6ae-145">Další informace o zachycení bitové kopie najdete v tématu [zaznamenat bitovou kopii virtuálního počítače Azure Windows vytvořené pomocí modelu nasazení classic](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="ec6ae-145">To learn more about capturing an image, see [Capture an image of an Azure Windows virtual machine created with the classic deployment model](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

### <a name="5-add-to-azure-remoteapp-template-images"></a><span data-ttu-id="ec6ae-146">5. Přidejte do Image šablony vzdálené aplikace RemoteApp Azure</span><span class="sxs-lookup"><span data-stu-id="ec6ae-146">5. Add to Azure RemoteApp Template images</span></span>
<span data-ttu-id="ec6ae-147">V části Azure RemoteApp na aktuálním portálu přejděte na kartu Image šablony a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-147">In the Azure RemoteApp section of the current portal, go to the Template Images tab and click Add.</span></span> <span data-ttu-id="ec6ae-148">V dialogovém okně automaticky otevírané okno Vyberte "Importovat bitovou kopii z knihovny virtuální počítače" a pak vyberte bitovou kopii, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-148">In the pop-up box, select "Import an image from your Virtual Machines library" and then choose the Image that you just created.</span></span>

### <a name="6-create-cloud-collection"></a><span data-ttu-id="ec6ae-149">6. Vytvoření cloudové kolekce</span><span class="sxs-lookup"><span data-stu-id="ec6ae-149">6. Create cloud collection</span></span>
<span data-ttu-id="ec6ae-150">Na aktuálním portálu vytvořte nové cloudové kolekce Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-150">In the current portal, create a new Azure RemoteApp Cloud Collection.</span></span> <span data-ttu-id="ec6ae-151">Zvolte Image šablony, který jste právě naimportovali pomocí SSMS na něm nainstalován.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-151">Choose the Template Image that you just imported with SSMS installed on it.</span></span>

![Vytvořit nové cloudové kolekce][2]

### <a name="7-publish-ssms"></a><span data-ttu-id="ec6ae-153">7. Publikování aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="ec6ae-153">7. Publish SSMS</span></span>
<span data-ttu-id="ec6ae-154">Na publikování nové kolekce cloudu, vyberte na kartě publikovat aplikaci z nabídky Start a poté zvolte SSMS ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-154">On the Publishing tab of your new cloud collection, select Publish an application from the Start Menu and then choose SSMS from the list.</span></span>

![Publikování aplikace][5]

### <a name="8-add-users"></a><span data-ttu-id="ec6ae-156">8. Přidání uživatelů</span><span class="sxs-lookup"><span data-stu-id="ec6ae-156">8. Add users</span></span>
<span data-ttu-id="ec6ae-157">Na kartě přístup uživatelů můžete vybrat uživatele, kteří budou mít přístup k této kolekci Azure Remoteappu, obsahující pouze SSMS.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-157">On the User Access tab you can select the users that will have access to this Azure RemoteApp collection which only includes SSMS.</span></span>

![Přidání uživatele][6]

### <a name="9-install-the-azure-remoteapp-client-application"></a><span data-ttu-id="ec6ae-159">9. Instalace klienta aplikace Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="ec6ae-159">9. Install the Azure RemoteApp client application</span></span>
<span data-ttu-id="ec6ae-160">Můžete stáhnout a nainstalovat klienta Azure RemoteApp zde: [stáhnout | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span><span class="sxs-lookup"><span data-stu-id="ec6ae-160">You can download and install a Azure RemoteApp client here: [Download | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span></span>

## <a name="configure-azure-sql-server"></a><span data-ttu-id="ec6ae-161">Konfigurace serveru Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ec6ae-161">Configure Azure SQL server</span></span>
<span data-ttu-id="ec6ae-162">Pouze konfigurace potřebná je zajistit, že pro bránu firewall je povolena služba Azure.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-162">The only configuration needed is to ensure that Azure Services is enabled for the firewall.</span></span> <span data-ttu-id="ec6ae-163">Pokud použijete toto řešení, není potřeba přidat všechny IP adresy, otevření brány firewall.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-163">If you use this solution, then you do not need to add any IP addresses to open the firewall.</span></span> <span data-ttu-id="ec6ae-164">Síťový provoz, který je povolen k systému SQL Server je z jiných služeb systému Azure.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-164">The network traffic that is allowed to the SQL Server is from other Azure services.</span></span>

![Povolit Azure][4]

## <a name="multi-factor-authentication-mfa"></a><span data-ttu-id="ec6ae-166">Vícefaktorové ověřování (MFA)</span><span class="sxs-lookup"><span data-stu-id="ec6ae-166">Multi-Factor Authentication (MFA)</span></span>
<span data-ttu-id="ec6ae-167">Vícefaktorové ověřování můžete konkrétně povolit pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-167">MFA can be enabled for this application specifically.</span></span> <span data-ttu-id="ec6ae-168">Přejděte na kartu aplikace služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-168">Go to the Applications tab of your Azure Active Directory.</span></span> <span data-ttu-id="ec6ae-169">Bude najít položku aplikace Microsoft Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-169">You will find an entry for Microsoft Azure RemoteApp.</span></span> <span data-ttu-id="ec6ae-170">Pokud kliknutím na tuto aplikaci a pak nakonfigurujte, zobrazí se stránka níže, kde můžete povolit MFA pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-170">If you click that application and then configure, you will see the page below where you can enable MFA for this application.</span></span>

![Povolit MFA][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a><span data-ttu-id="ec6ae-172">Audit aktivity uživatelů s Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="ec6ae-172">Audit user activity with Azure Active Directory Premium</span></span>
<span data-ttu-id="ec6ae-173">Pokud nemáte Azure AD Premium, budete muset zapnout v části licence adresáře.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-173">If you do not have Azure AD Premium, then you have to turn it on in the Licenses section of your directory.</span></span> <span data-ttu-id="ec6ae-174">S Premium povolená můžete přiřadit uživatele na úrovni Premium.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-174">With Premium enabled, you can assign users to the Premium level.</span></span>

<span data-ttu-id="ec6ae-175">Když přejdete na uživatele ve vašem Azure Active Directory, můžete pak přejděte na kartu aktivity chcete zobrazit informace o přihlášení k Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-175">When you go to a user in your Azure Active Directory, you can then go to the Activity tab to see login information to Azure RemoteApp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec6ae-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec6ae-176">Next steps</span></span>
<span data-ttu-id="ec6ae-177">Po dokončení všech výše uvedených kroků, budete moci spustit klientovi Azure Remoteappu a přihlásit se s přiřazený uživatel.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-177">After completing all the above steps, you will be able to run the Azure RemoteApp client and log-in with an assigned user.</span></span> <span data-ttu-id="ec6ae-178">Zobrazí pomocí SSMS jako jednu z vašich aplikací a můžete ji spustit jako při byly nainstalovány v počítači s přístupem k serveru Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-178">You will be presented with SSMS as one of your applications, and you can run it as you would if it were installed on your computer with access to Azure SQL server.</span></span>

<span data-ttu-id="ec6ae-179">Další informace o tom, aby se připojení k databázi SQL najdete v tématu [připojit k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="ec6ae-179">For more information on how to make the connection to SQL Database, see [Connect to SQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>

<span data-ttu-id="ec6ae-180">To je všechno, co teď.</span><span class="sxs-lookup"><span data-stu-id="ec6ae-180">That's everything for now.</span></span> <span data-ttu-id="ec6ae-181">Užijte si ji!</span><span class="sxs-lookup"><span data-stu-id="ec6ae-181">Enjoy!</span></span>

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png