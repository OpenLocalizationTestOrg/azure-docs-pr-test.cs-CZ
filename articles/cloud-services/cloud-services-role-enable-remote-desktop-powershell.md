---
title: "Povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services pomocí prostředí PowerShell"
description: "Postup konfigurace aplikace služby azure cloud pomocí prostředí PowerShell umožňující připojení ke vzdálené ploše"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: bf2f70a4-20dc-4302-a91a-38cd7a2baa62
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 171f27c92ee9de14301ebb664e9ba3bcd98c394d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="ec2d0-103">Povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ec2d0-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ec2d0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ec2d0-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="ec2d0-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="ec2d0-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="ec2d0-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ec2d0-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="ec2d0-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec2d0-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="ec2d0-108">Vzdálená plocha umožňuje přístup k ploše role, která běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-108">Remote Desktop enables you to access the desktop of a role running in Azure.</span></span> <span data-ttu-id="ec2d0-109">Připojení ke vzdálené ploše můžete odstraňovat a diagnostikovat problémy s aplikací, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-109">You can use a Remote Desktop connection to troubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="ec2d0-110">Tento článek popisuje povolení vzdálené plochy na role cloudové služby pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-110">This article describes how to enable remote desktop on your Cloud Service Roles using PowerShell.</span></span> <span data-ttu-id="ec2d0-111">V tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) pro součásti potřebné k tomuto článku.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-111">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for the prerequisites needed for this article.</span></span> <span data-ttu-id="ec2d0-112">Prostředí PowerShell využívá příponou vzdálené plochy, Vzdálená plocha můžete povolit, jakmile je aplikace nasazená.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-112">PowerShell utilizes the Remote Desktop Extension so you can enable Remote Desktop after the application is deployed.</span></span>

## <a name="configure-remote-desktop-from-powershell"></a><span data-ttu-id="ec2d0-113">Konfigurace vzdálené plochy z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ec2d0-113">Configure Remote Desktop from PowerShell</span></span>
<span data-ttu-id="ec2d0-114">[Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutina umožňuje povolit vzdálené plochy na určené role nebo všechny role nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-114">The [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet allows you to enable Remote Desktop on specified roles or all roles of your cloud service deployment.</span></span> <span data-ttu-id="ec2d0-115">Rutina umožňuje zadat uživatelské jméno a heslo pro uživatele vzdálené plochy prostřednictvím *pověření* parametr, který přijímá objekt PSCredential.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-115">The cmdlet lets you specify the Username and Password for the remote desktop user through the *Credential* parameter that accepts a PSCredential object.</span></span>

<span data-ttu-id="ec2d0-116">Pokud používáte prostředí PowerShell interaktivně, můžete snadno nastavit objekt PSCredential voláním [Get-pověření](https://technet.microsoft.com/library/hh849815.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-116">If you are using PowerShell interactively, you can easily set the PSCredential object by calling the [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

```
$remoteusercredentials = Get-Credential
```

<span data-ttu-id="ec2d0-117">Tento příkaz zobrazí dialogové okno umožňuje zadejte uživatelské jméno a heslo pro vzdálené uživatele zabezpečeným způsobem.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-117">This command displays a dialog box allowing you to enter the username and password for the remote user in a secure manner.</span></span>

<span data-ttu-id="ec2d0-118">Vzhledem k tomu, že prostředí PowerShell pomáhá scénáře automatizace, můžete také nastavit **PSCredential** objekt způsobem, který nevyžaduje zásah uživatele.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-118">Since PowerShell helps in automation scenarios, you can also set up the **PSCredential** object in a way that doesn't require user interaction.</span></span> <span data-ttu-id="ec2d0-119">Nejdřív musíte nastavit zabezpečeného hesla.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-119">First, you need to set up a secure password.</span></span> <span data-ttu-id="ec2d0-120">Začínat zadání hesla v podobě prostého textu ho převést na zabezpečený řetězec pomocí [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec2d0-120">You begin with specifying a plain text password convert it to a secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span> <span data-ttu-id="ec2d0-121">Dále je třeba k pomocí zašifrovaný standardní řetězec převést tento zabezpečený řetězec [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec2d0-121">Next you need to convert this secure string into an encrypted standard string using [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span></span> <span data-ttu-id="ec2d0-122">Teď tento šifrovaný řetězec standardní můžete uložit do souboru pomocí [obsah sady](https://technet.microsoft.com/library/ee176959.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec2d0-122">Now you can save this encrypted standard string to a file using [Set-Content](https://technet.microsoft.com/library/ee176959.aspx).</span></span>

<span data-ttu-id="ec2d0-123">Můžete také vytvořit soubor zabezpečeného hesla tak, aby je nemuseli zadávat heslo pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-123">You can also create a secure password file so that you don't have to type in the password every time.</span></span> <span data-ttu-id="ec2d0-124">Navíc soubor zabezpečeného hesla je lepší, než soubor ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-124">Also, a secure password file is better than a plain text file.</span></span> <span data-ttu-id="ec2d0-125">Vytvořte soubor zabezpečeného hesla pomocí prostředí PowerShell následující:</span><span class="sxs-lookup"><span data-stu-id="ec2d0-125">Use the following PowerShell to create a secure password file:</span></span>

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> <span data-ttu-id="ec2d0-126">Při nastavování hesla, ujistěte se, že splňujete [požadavky na složitost](https://technet.microsoft.com/library/cc786468.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec2d0-126">When setting the password, make sure that you meet the [complexity requirements](https://technet.microsoft.com/library/cc786468.aspx).</span></span>
>
>

<span data-ttu-id="ec2d0-127">K vytvoření objektu pověření ze zabezpečeného heslo souboru, musí přečíst obsah souboru a je převést zpět na zabezpečený řetězec pomocí [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec2d0-127">To create the credential object from the secure password file, you must read the file contents and convert them back to a secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span>

<span data-ttu-id="ec2d0-128">[Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutina také umožňuje *vypršení platnosti* parametr, který určuje **data a času** ve který vyprší platnost uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-128">The [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet also accepts an *Expiration* parameter, which specifies a **DateTime** at which the user account expires.</span></span> <span data-ttu-id="ec2d0-129">Například můžete nastavit účet, který chcete několik dní od aktuální datum a čas vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-129">For example, you could set the account to expire a few days from the current date and time.</span></span>

<span data-ttu-id="ec2d0-130">Tento příklad PowerShell ukazuje, jak nastavit vzdálenou plochu rozšíření pro cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="ec2d0-130">This PowerShell example shows you how to set the Remote Desktop Extension on a cloud service:</span></span>

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
<span data-ttu-id="ec2d0-131">Můžete volitelně specifikovat slot nasazení a role, které chcete povolit vzdálená plocha na.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-131">You can also optionally specify the deployment slot and roles that you want to enable remote desktop on.</span></span> <span data-ttu-id="ec2d0-132">Pokud tyto parametry nejsou zadané, rutina umožňuje vzdálená plocha u všech rolí v **produkční** nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-132">If these parameters are not specified, the cmdlet enables remote desktop on all roles in the **Production** deployment slot.</span></span>

<span data-ttu-id="ec2d0-133">Rozšíření vzdálené plochy je přidruženým k nasazení.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-133">The Remote Desktop extension is associated with a deployment.</span></span> <span data-ttu-id="ec2d0-134">Pokud vytvoříte nové nasazení pro službu, budete muset povolit vzdálené plochy na toto nasazení.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-134">If you create a new deployment for the service, you have to enable remote desktop on that deployment.</span></span> <span data-ttu-id="ec2d0-135">Pokud chcete vždy mít povolena Vzdálená plocha, měli zvážit, integraci skriptů prostředí PowerShell do pracovního postupu nasazení.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-135">If you always want to have remote desktop enabled, then you should consider integrating the PowerShell scripts into your deployment workflow.</span></span>

## <a name="remote-desktop-into-a-role-instance"></a><span data-ttu-id="ec2d0-136">Vzdálená plocha do role instance</span><span class="sxs-lookup"><span data-stu-id="ec2d0-136">Remote Desktop into a role instance</span></span>
<span data-ttu-id="ec2d0-137">[Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) rutina se používá ke vzdálené ploše do instance určité role cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-137">The [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet is used to remote desktop into a specific role instance of your cloud service.</span></span> <span data-ttu-id="ec2d0-138">Můžete použít *Místnícesta* parametr ke stažení protokol RDP soubor místně.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-138">You can use the *LocalPath* parameter to download the RDP file locally.</span></span> <span data-ttu-id="ec2d0-139">Nebo můžete použít *spusťte* parametr přímo spustit dialogové okno připojení ke vzdálené ploše pro přístup k instanci role cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-139">Or you can use the *Launch* parameter to directly launch the Remote Desktop Connection dialog to access the cloud service role instance.</span></span>

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a><span data-ttu-id="ec2d0-140">Zkontrolujte, zda rozšíření vzdálené plochy je povolena ve službě</span><span class="sxs-lookup"><span data-stu-id="ec2d0-140">Check if Remote Desktop extension is enabled on a service</span></span>
<span data-ttu-id="ec2d0-141">[Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) zobrazuje rutina, která povolená nebo zakázaná na nasazení služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-141">The [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet displays that remote desktop is enabled or disabled on a service deployment.</span></span> <span data-ttu-id="ec2d0-142">Rutina vrátí uživatelské jméno pro uživatele vzdálené plochy a rolí, ve kterých je povolené vzdálené plochy rozšíření pro.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-142">The cmdlet returns the username for the remote desktop user and the roles that the remote desktop extension is enabled for.</span></span> <span data-ttu-id="ec2d0-143">Ve výchozím nastavení k tomu dochází slotu nasazení a můžete místo toho použít přípravný slot.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-143">By default, this happens on the deployment slot and you can choose to use the staging slot instead.</span></span>

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a><span data-ttu-id="ec2d0-144">Odebrání služby Vzdálená plocha rozšíření</span><span class="sxs-lookup"><span data-stu-id="ec2d0-144">Remove Remote Desktop extension from a service</span></span>
<span data-ttu-id="ec2d0-145">Pokud jste povolili vzdálené plochy rozšíření na nasazení a potřeba aktualizovat nastavení vzdálené plochy, nejprve odeberte rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-145">If you have already enabled the remote desktop extension on a deployment, and need to update the remote desktop settings, first remove the extension.</span></span> <span data-ttu-id="ec2d0-146">A znovu ji povolte s novým nastavením.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-146">And enable it again with the new settings.</span></span> <span data-ttu-id="ec2d0-147">Například pokud chcete nastavit nové heslo pro účet vzdáleného uživatele nebo platnost účtu.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-147">For example, if you want to set a new password for the remote user account, or the account expired.</span></span> <span data-ttu-id="ec2d0-148">To je potřeba na existující nasazení, které mají vzdálené plochy rozšíření povolené.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-148">Doing this is required on existing deployments that have the remote desktop extension enabled.</span></span> <span data-ttu-id="ec2d0-149">Pro nová nasazení můžete jednoduše provést rozšíření přímo.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-149">For new deployments, you can simply apply the extension directly.</span></span>

<span data-ttu-id="ec2d0-150">Chcete-li odebrat z nasazení vzdálené plochy rozšíření, můžete použít [odebrat AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-150">To remove the remote desktop extension from the deployment, you can use the [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="ec2d0-151">Můžete volitelně specifikovat slot nasazení a role, ze kterého chcete odebrat příponou vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-151">You can also optionally specify the deployment slot and role from which you want to remove the remote desktop extension.</span></span>

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> <span data-ttu-id="ec2d0-152">K úplnému odebrání konfigurace rozšíření, by měly volat *odebrat* rutiny s **UninstallConfiguration** parametr.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-152">To completely remove the extension configuration, you should call the *remove* cmdlet with the **UninstallConfiguration** parameter.</span></span>
>
> <span data-ttu-id="ec2d0-153">**UninstallConfiguration** parametr odinstaluje jakoukoli konfiguraci rozšíření, která se použije ke službě.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-153">The **UninstallConfiguration** parameter uninstalls any extension configuration that is applied to the service.</span></span> <span data-ttu-id="ec2d0-154">Všechny konfigurace rozšíření je přidružen konfiguraci služby.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-154">Every extension configuration is associated with the service configuration.</span></span> <span data-ttu-id="ec2d0-155">Volání *odebrat* rutiny bez **UninstallConfiguration** zrušíte <mark>nasazení</mark> z konfigurace rozšíření, proto efektivně odebírání rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-155">Calling the *remove* cmdlet without **UninstallConfiguration** disassociates the <mark>deployment</mark> from the extension configuration, thus effectively removing the extension.</span></span> <span data-ttu-id="ec2d0-156">Konfigurace rozšíření však zůstane službě přidružen.</span><span class="sxs-lookup"><span data-stu-id="ec2d0-156">However, the extension configuration remains associated with the service.</span></span>
>
>

## <a name="additional-resources"></a><span data-ttu-id="ec2d0-157">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ec2d0-157">Additional resources</span></span>

<span data-ttu-id="ec2d0-158">[Postup konfigurace cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="ec2d0-158">[How to Configure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
