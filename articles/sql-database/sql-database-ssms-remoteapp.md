---
title: "aaaConnect tooSQL databázi pomocí aplikace SQL Server Management Studio v Azure Remoteappu | Microsoft Docs"
description: "Jak používat tento kurz toolearn toouse SQL Server Management Studio v Azure Remoteappu pro zabezpečení a výkonu při připojování tooSQL databáze"
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
ms.openlocfilehash: 73994f9a1eb3e48efa5d7c4f976b00cfcbc88d75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-tooconnect-toosql-database"></a><span data-ttu-id="c3d03-103">Pomocí SQL Server Management Studio v Azure Remoteappu tooconnect tooSQL databáze</span><span class="sxs-lookup"><span data-stu-id="c3d03-103">Use SQL Server Management Studio in Azure RemoteApp tooconnect tooSQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c3d03-104">Azure RemoteApp se přestává používat.</span><span class="sxs-lookup"><span data-stu-id="c3d03-104">Azure RemoteApp is being discontinued.</span></span> <span data-ttu-id="c3d03-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c3d03-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
>

## <a name="introduction"></a><span data-ttu-id="c3d03-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="c3d03-106">Introduction</span></span>
<span data-ttu-id="c3d03-107">Tento kurz ukazuje, jak toouse SQL Server Management Studio (SSMS) v Azure Remoteappu tooconnect tooSQL databáze.</span><span class="sxs-lookup"><span data-stu-id="c3d03-107">This tutorial shows you how toouse SQL Server Management Studio (SSMS) in Azure RemoteApp tooconnect tooSQL Database.</span></span> <span data-ttu-id="c3d03-108">Vás provede procesem hello nastavení SQL Server Management Studio v Azure Remoteappu, vysvětluje výhody hello a zobrazuje funkce zabezpečení, které můžete použít v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c3d03-108">It walks you through hello process of setting up SQL Server Management Studio in Azure RemoteApp, explains hello benefits, and shows security features that you can use in Azure Active Directory.</span></span>

<span data-ttu-id="c3d03-109">**Odhadovaný čas toocomplete:** 45 minut</span><span class="sxs-lookup"><span data-stu-id="c3d03-109">**Estimated time toocomplete:** 45 minutes</span></span>

## <a name="ssms-in-azure-remoteapp"></a><span data-ttu-id="c3d03-110">Aplikace SSMS v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="c3d03-110">SSMS in Azure RemoteApp</span></span>
<span data-ttu-id="c3d03-111">Azure RemoteApp je služba Vzdálená plocha v Azure, který zajišťuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3d03-111">Azure RemoteApp is an RDS service in Azure that delivers applications.</span></span> <span data-ttu-id="c3d03-112">Další informace o něm zde: [co je RemoteApp?](../remoteapp/remoteapp-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="c3d03-112">You can learn more about it here: [What is RemoteApp?](../remoteapp/remoteapp-whatis.md)</span></span>

<span data-ttu-id="c3d03-113">Aplikace SSMS spuštění v Azure RemoteApp umožňuje hello stejné prostředí jako aplikace SSMS spuštěná místně.</span><span class="sxs-lookup"><span data-stu-id="c3d03-113">SSMS running in Azure RemoteApp gives you hello same experience as running SSMS locally.</span></span>

![Snímek obrazovky aplikace SSMS spuštění v Azure Remoteappu][1]

## <a name="benefits"></a><span data-ttu-id="c3d03-115">Výhody</span><span class="sxs-lookup"><span data-stu-id="c3d03-115">Benefits</span></span>
<span data-ttu-id="c3d03-116">Existuje mnoho výhod toousing SSMS v Azure Remoteappu, včetně:</span><span class="sxs-lookup"><span data-stu-id="c3d03-116">There are many benefits toousing SSMS in Azure RemoteApp, including:</span></span>

* <span data-ttu-id="c3d03-117">Na serveru Azure SQL port 1433 nemá toobe zveřejněné externě (mimo Azure).</span><span class="sxs-lookup"><span data-stu-id="c3d03-117">Port 1433 on Azure SQL server does not have toobe exposed externally (outside of Azure).</span></span>
* <span data-ttu-id="c3d03-118">Bez nutnosti tookeep přidávání a odebírání adres IP v bráně firewall serveru Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="c3d03-118">No need tookeep adding and removing IP addresses in hello Azure SQL server firewall.</span></span>
* <span data-ttu-id="c3d03-119">Všechna připojení Azure RemoteApp probíhá přes protokol HTTPS na portu 443 pomocí šifrované protokolu vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="c3d03-119">All Azure RemoteApp connections occur over HTTPS on port 443 using encrypted Remote Desktop protocol</span></span>
* <span data-ttu-id="c3d03-120">Je více uživatelů a možné škálovat.</span><span class="sxs-lookup"><span data-stu-id="c3d03-120">It is multi-user and can scale.</span></span>
* <span data-ttu-id="c3d03-121">Je výkonnější z s SSMS ve hello stejné oblasti jako hello databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="c3d03-121">There is a performance gain from having SSMS in hello same region as hello SQL Database.</span></span>
* <span data-ttu-id="c3d03-122">Můžete auditovat použití služby Azure RemoteApp s hello Premium edition služby Azure Active Directory, která má protokolování aktivit uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c3d03-122">You can audit use of Azure RemoteApp with hello Premium edition of Azure Active Directory which has user activity reports.</span></span>
* <span data-ttu-id="c3d03-123">Můžete povolit vícefaktorové ověřování (MFA).</span><span class="sxs-lookup"><span data-stu-id="c3d03-123">You can enable multi-factor authentication (MFA).</span></span>
* <span data-ttu-id="c3d03-124">Přístup aplikace SSMS kdekoli při použití některého z hello Podporovaní klienti Azure RemoteApp, což zahrnuje iOS, Android, Mac, Windows Phone a počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="c3d03-124">Access SSMS anywhere when using any of hello supported Azure RemoteApp clients which includes iOS, Android, Mac, Windows Phone, and Windows PC’s.</span></span>

## <a name="create-hello-azure-remoteapp-collection"></a><span data-ttu-id="c3d03-125">Vytvořte kolekci Azure Remoteappu hello</span><span class="sxs-lookup"><span data-stu-id="c3d03-125">Create hello Azure RemoteApp collection</span></span>
<span data-ttu-id="c3d03-126">Zde jsou kroky toocreate hello kolekcí vzdálené aplikace Azure RemoteApp pomocí SSMS:</span><span class="sxs-lookup"><span data-stu-id="c3d03-126">Here are hello steps toocreate your Azure RemoteApp collection with SSMS:</span></span>

### <a name="1-create-a-new-windows-vm-from-image"></a><span data-ttu-id="c3d03-127">1. Vytvoření nového virtuálního počítače Windows z bitové kopie</span><span class="sxs-lookup"><span data-stu-id="c3d03-127">1. Create a new Windows VM from Image</span></span>
<span data-ttu-id="c3d03-128">Použijte hello "Windows Server vzdálené plochy relace hostitele Windows serveru 2012 R2" Image z Galerie toomake hello nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c3d03-128">Use hello "Windows Server Remote Desktop Session Host Windows Server 2012 R2" Image from hello Gallery toomake your new VM.</span></span>

### <a name="2-install-ssms-from-sql-express"></a><span data-ttu-id="c3d03-129">2. Nainstalujte aplikaci SSMS z SQL Express.</span><span class="sxs-lookup"><span data-stu-id="c3d03-129">2. Install SSMS from SQL Express</span></span>
<span data-ttu-id="c3d03-130">Přejděte do hello nový virtuální počítač a přejděte stránky pro stažení toothis: [Express Microsoft® SQL Server® 2014](https://www.microsoft.com/download/details.aspx?id=42299)</span><span class="sxs-lookup"><span data-stu-id="c3d03-130">Go onto hello new VM and navigate toothis download page: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)</span></span>

<span data-ttu-id="c3d03-131">Není stažení tooonly možnost SSMS.</span><span class="sxs-lookup"><span data-stu-id="c3d03-131">There is an option tooonly download SSMS.</span></span> <span data-ttu-id="c3d03-132">Po stažení přejděte do instalačního adresáře hello a spusťte instalační program tooinstall SSMS.</span><span class="sxs-lookup"><span data-stu-id="c3d03-132">After download, go into hello install directory and run Setup tooinstall SSMS.</span></span>

<span data-ttu-id="c3d03-133">Budete také potřebovat tooinstall SQL Server 2014 Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="c3d03-133">You also need tooinstall SQL Server 2014 Service Pack 1.</span></span> <span data-ttu-id="c3d03-134">Si můžete stáhnout tady: [aktualizace Service Pack 1 (SP1) pro systém Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=46694)</span><span class="sxs-lookup"><span data-stu-id="c3d03-134">You can download it here: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)</span></span>

<span data-ttu-id="c3d03-135">SQL Server 2014 Service Pack 1 zahrnuje základní funkce pro práci s Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c3d03-135">SQL Server 2014 Service Pack 1 includes essential functionality for working with Azure SQL Database.</span></span>

### <a name="3-run-validate-script-and-sysprep"></a><span data-ttu-id="c3d03-136">3. Spuštění skriptu ověřením a nástroj Sysprep</span><span class="sxs-lookup"><span data-stu-id="c3d03-136">3. Run Validate script and Sysprep</span></span>
<span data-ttu-id="c3d03-137">Na hello ploše hello virtuálního počítače je skript prostředí PowerShell s názvem ověřením.</span><span class="sxs-lookup"><span data-stu-id="c3d03-137">On hello desktop of hello VM is a PowerShell script called Validate.</span></span> <span data-ttu-id="c3d03-138">Spusťte tento dvojitým kliknutím.</span><span class="sxs-lookup"><span data-stu-id="c3d03-138">Run this by double-clicking.</span></span> <span data-ttu-id="c3d03-139">Ověří, že tento hello virtuálního počítače je připraven toobe používá pro vzdálené hostování aplikací.</span><span class="sxs-lookup"><span data-stu-id="c3d03-139">It will verify that hello VM is ready toobe used for remote hosting of applications.</span></span> <span data-ttu-id="c3d03-140">Po dokončení ověření požádá toorun sysprep – zvolte toorun ho.</span><span class="sxs-lookup"><span data-stu-id="c3d03-140">When verification is complete, it will ask toorun sysprep - choose toorun it.</span></span>

<span data-ttu-id="c3d03-141">Po dokončení nástroj sysprep vypne hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c3d03-141">When sysprep completes, it will shut down hello VM.</span></span>

<span data-ttu-id="c3d03-142">toolearn Další informace o vytvoření image Azure Remoteappu, najdete v části: [jak toocreate šablony vzdálené aplikace RemoteApp bitové kopie v Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="c3d03-142">toolearn more about creating a Azure RemoteApp image, see: [How toocreate a RemoteApp template image in Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span></span>

### <a name="4-capture-image"></a><span data-ttu-id="c3d03-143">4. Zachycení bitové kopie</span><span class="sxs-lookup"><span data-stu-id="c3d03-143">4. Capture image</span></span>
<span data-ttu-id="c3d03-144">Hello virtuálních počítačů byla zastavena, když ji najít v aktuálním portálu hello a zachycení ho.</span><span class="sxs-lookup"><span data-stu-id="c3d03-144">When hello VM has stopped running, find it in hello current portal and capture it.</span></span>

<span data-ttu-id="c3d03-145">toolearn Další informace o zachycení bitové kopie, najdete v části [zaznamenat bitovou kopii virtuálního počítače Azure Windows vytvořené pomocí modelu nasazení classic hello](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c3d03-145">toolearn more about capturing an image, see [Capture an image of an Azure Windows virtual machine created with hello classic deployment model](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

### <a name="5-add-tooazure-remoteapp-template-images"></a><span data-ttu-id="c3d03-146">5. Přidat tooAzure Image šablony vzdálené aplikace RemoteApp</span><span class="sxs-lookup"><span data-stu-id="c3d03-146">5. Add tooAzure RemoteApp Template images</span></span>
<span data-ttu-id="c3d03-147">V hello části hello aktuální portál Azure RemoteApp přejděte na kartu Image šablony toohello a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="c3d03-147">In hello Azure RemoteApp section of hello current portal, go toohello Template Images tab and click Add.</span></span> <span data-ttu-id="c3d03-148">V poli hello automaticky otevírané okno Vyberte "Importovat bitovou kopii z knihovny virtuální počítače" a potom zvolte hello bitovou kopii, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c3d03-148">In hello pop-up box, select "Import an image from your Virtual Machines library" and then choose hello Image that you just created.</span></span>

### <a name="6-create-cloud-collection"></a><span data-ttu-id="c3d03-149">6. Vytvoření cloudové kolekce</span><span class="sxs-lookup"><span data-stu-id="c3d03-149">6. Create cloud collection</span></span>
<span data-ttu-id="c3d03-150">Hello aktuálním portálu vytvořte nové cloudové kolekce Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="c3d03-150">In hello current portal, create a new Azure RemoteApp Cloud Collection.</span></span> <span data-ttu-id="c3d03-151">Zvolte hello Image šablony, který jste právě naimportovali pomocí SSMS na něm nainstalován.</span><span class="sxs-lookup"><span data-stu-id="c3d03-151">Choose hello Template Image that you just imported with SSMS installed on it.</span></span>

![Vytvořit nové cloudové kolekce][2]

### <a name="7-publish-ssms"></a><span data-ttu-id="c3d03-153">7. Publikování aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="c3d03-153">7. Publish SSMS</span></span>
<span data-ttu-id="c3d03-154">Na hello publikování kartě nové cloudové kolekce, vyberte možnost publikovat aplikace z hello nabídce Start a poté zvolte ze seznamu hello SSMS.</span><span class="sxs-lookup"><span data-stu-id="c3d03-154">On hello Publishing tab of your new cloud collection, select Publish an application from hello Start Menu and then choose SSMS from hello list.</span></span>

![Publikování aplikace][5]

### <a name="8-add-users"></a><span data-ttu-id="c3d03-156">8. Přidání uživatelů</span><span class="sxs-lookup"><span data-stu-id="c3d03-156">8. Add users</span></span>
<span data-ttu-id="c3d03-157">Na kartě hello přístupu uživatele můžete vybrat hello uživatele, kteří budou mít kolekci Azure Remoteappu toothis přístup pouze zahrnující SSMS.</span><span class="sxs-lookup"><span data-stu-id="c3d03-157">On hello User Access tab you can select hello users that will have access toothis Azure RemoteApp collection which only includes SSMS.</span></span>

![Přidání uživatele][6]

### <a name="9-install-hello-azure-remoteapp-client-application"></a><span data-ttu-id="c3d03-159">9. Nainstalovat klientské aplikace Azure RemoteApp hello</span><span class="sxs-lookup"><span data-stu-id="c3d03-159">9. Install hello Azure RemoteApp client application</span></span>
<span data-ttu-id="c3d03-160">Můžete stáhnout a nainstalovat klienta Azure RemoteApp zde: [stáhnout | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span><span class="sxs-lookup"><span data-stu-id="c3d03-160">You can download and install a Azure RemoteApp client here: [Download | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span></span>

## <a name="configure-azure-sql-server"></a><span data-ttu-id="c3d03-161">Konfigurace serveru Azure SQL</span><span class="sxs-lookup"><span data-stu-id="c3d03-161">Configure Azure SQL server</span></span>
<span data-ttu-id="c3d03-162">Hello pouze potřeba konfigurace je tooensure, která obsluhuje Azure je povoleno pro bránu firewall hello.</span><span class="sxs-lookup"><span data-stu-id="c3d03-162">hello only configuration needed is tooensure that Azure Services is enabled for hello firewall.</span></span> <span data-ttu-id="c3d03-163">Pokud použijete toto řešení, pak nepotřebujete tooadd všechny IP adresy tooopen hello firewall.</span><span class="sxs-lookup"><span data-stu-id="c3d03-163">If you use this solution, then you do not need tooadd any IP addresses tooopen hello firewall.</span></span> <span data-ttu-id="c3d03-164">Hello síťový provoz, který je povolen toohello systému SQL Server je z jiných služeb systému Azure.</span><span class="sxs-lookup"><span data-stu-id="c3d03-164">hello network traffic that is allowed toohello SQL Server is from other Azure services.</span></span>

![Povolit Azure][4]

## <a name="multi-factor-authentication-mfa"></a><span data-ttu-id="c3d03-166">Vícefaktorové ověřování (MFA)</span><span class="sxs-lookup"><span data-stu-id="c3d03-166">Multi-Factor Authentication (MFA)</span></span>
<span data-ttu-id="c3d03-167">Vícefaktorové ověřování můžete konkrétně povolit pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c3d03-167">MFA can be enabled for this application specifically.</span></span> <span data-ttu-id="c3d03-168">Přejděte toohello karta aplikace služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c3d03-168">Go toohello Applications tab of your Azure Active Directory.</span></span> <span data-ttu-id="c3d03-169">Bude najít položku aplikace Microsoft Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="c3d03-169">You will find an entry for Microsoft Azure RemoteApp.</span></span> <span data-ttu-id="c3d03-170">Pokud kliknutím na tuto aplikaci a pak nakonfigurujte, zobrazí se stránka hello níže, kde můžete povolit MFA pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c3d03-170">If you click that application and then configure, you will see hello page below where you can enable MFA for this application.</span></span>

![Povolit MFA][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a><span data-ttu-id="c3d03-172">Audit aktivity uživatelů s Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="c3d03-172">Audit user activity with Azure Active Directory Premium</span></span>
<span data-ttu-id="c3d03-173">Pokud nemáte Azure AD Premium, pak máte tooturn ho na hello licence oddílu adresáře.</span><span class="sxs-lookup"><span data-stu-id="c3d03-173">If you do not have Azure AD Premium, then you have tooturn it on in hello Licenses section of your directory.</span></span> <span data-ttu-id="c3d03-174">Premium povolená můžete přiřadit uživatele toohello úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="c3d03-174">With Premium enabled, you can assign users toohello Premium level.</span></span>

<span data-ttu-id="c3d03-175">Jakmile se uživatel tooa ve vašem Azure Active Directory, můžete přejít toohello aktivity karta toosee přihlašovací informace tooAzure vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="c3d03-175">When you go tooa user in your Azure Active Directory, you can then go toohello Activity tab toosee login information tooAzure RemoteApp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3d03-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3d03-176">Next steps</span></span>
<span data-ttu-id="c3d03-177">Po dokončení všech hello výše kroky se vám bude klientovi Azure Remoteappu možné toorun hello a přihlásit se s přiřazený uživatel.</span><span class="sxs-lookup"><span data-stu-id="c3d03-177">After completing all hello above steps, you will be able toorun hello Azure RemoteApp client and log-in with an assigned user.</span></span> <span data-ttu-id="c3d03-178">Zobrazí pomocí SSMS jako jednu z vašich aplikací a můžete ji spustit jako při byly nainstalovány v počítači se serverem SQL tooAzure přístup.</span><span class="sxs-lookup"><span data-stu-id="c3d03-178">You will be presented with SSMS as one of your applications, and you can run it as you would if it were installed on your computer with access tooAzure SQL server.</span></span>

<span data-ttu-id="c3d03-179">Další informace o tom, jak toomake hello tooSQL připojení databáze najdete v tématu [připojení tooSQL databáze s SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="c3d03-179">For more information on how toomake hello connection tooSQL Database, see [Connect tooSQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>

<span data-ttu-id="c3d03-180">To je všechno, co teď.</span><span class="sxs-lookup"><span data-stu-id="c3d03-180">That's everything for now.</span></span> <span data-ttu-id="c3d03-181">Užijte si ji!</span><span class="sxs-lookup"><span data-stu-id="c3d03-181">Enjoy!</span></span>

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png