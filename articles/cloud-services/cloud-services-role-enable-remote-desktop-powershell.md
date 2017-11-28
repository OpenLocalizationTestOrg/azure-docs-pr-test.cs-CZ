---
title: "aaaEnable připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services pomocí prostředí PowerShell"
description: "Jak tooconfigure vaše azure cloudové aplikace služby pomocí prostředí PowerShell tooallow připojení ke vzdálené ploše"
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
ms.openlocfilehash: 3f46b014f29f1c0be0e1b485d2f0152424162bb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="e7b7d-103">Povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e7b7d-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e7b7d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e7b7d-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="e7b7d-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="e7b7d-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="e7b7d-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e7b7d-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="e7b7d-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7b7d-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="e7b7d-108">Vzdálená plocha umožňuje tooaccess hello ploše role, která běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-108">Remote Desktop enables you tooaccess hello desktop of a role running in Azure.</span></span> <span data-ttu-id="e7b7d-109">Můžete použít tootroubleshoot připojení vzdálené plochy a diagnostikovat problémy s vaší aplikací, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-109">You can use a Remote Desktop connection tootroubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="e7b7d-110">Tento článek popisuje, jak tooenable Vzdálená plocha u role cloudové služby pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-110">This article describes how tooenable remote desktop on your Cloud Service Roles using PowerShell.</span></span> <span data-ttu-id="e7b7d-111">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) pro hello součásti potřebné k tomuto článku.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-111">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span> <span data-ttu-id="e7b7d-112">Prostředí PowerShell využívá hello rozšíření vzdálené plochy, Vzdálená plocha můžete povolit po nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-112">PowerShell utilizes hello Remote Desktop Extension so you can enable Remote Desktop after hello application is deployed.</span></span>

## <a name="configure-remote-desktop-from-powershell"></a><span data-ttu-id="e7b7d-113">Konfigurace vzdálené plochy z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e7b7d-113">Configure Remote Desktop from PowerShell</span></span>
<span data-ttu-id="e7b7d-114">Hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutiny vám umožní tooenable vzdálené plochy na určené role nebo všechny role nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-114">hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet allows you tooenable Remote Desktop on specified roles or all roles of your cloud service deployment.</span></span> <span data-ttu-id="e7b7d-115">rutiny Hello umožňuje určit hello uživatelské jméno a heslo pro uživatele vzdálené plochy hello prostřednictvím hello *pověření* parametr, který přijímá objekt PSCredential.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-115">hello cmdlet lets you specify hello Username and Password for hello remote desktop user through hello *Credential* parameter that accepts a PSCredential object.</span></span>

<span data-ttu-id="e7b7d-116">Pokud používáte prostředí PowerShell interaktivně, můžete snadno nastavit objektu PSCredential hello pomocí volání hello [Get-pověření](https://technet.microsoft.com/library/hh849815.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-116">If you are using PowerShell interactively, you can easily set hello PSCredential object by calling hello [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

```
$remoteusercredentials = Get-Credential
```

<span data-ttu-id="e7b7d-117">Tento příkaz zobrazí dialogové okno, umožní vám tooenter hello uživatelské jméno a heslo pro vzdálené uživatele hello zabezpečeným způsobem.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-117">This command displays a dialog box allowing you tooenter hello username and password for hello remote user in a secure manner.</span></span>

<span data-ttu-id="e7b7d-118">Vzhledem k tomu, že prostředí PowerShell pomáhá scénáře automatizace, můžete také nastavit hello **PSCredential** objekt způsobem, který nevyžaduje zásah uživatele.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-118">Since PowerShell helps in automation scenarios, you can also set up hello **PSCredential** object in a way that doesn't require user interaction.</span></span> <span data-ttu-id="e7b7d-119">Nejprve je třeba tooset zabezpečené heslo.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-119">First, you need tooset up a secure password.</span></span> <span data-ttu-id="e7b7d-120">Začínat zadání hesla v podobě prostého textu ji převést zabezpečený řetězec tooa pomocí [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7b7d-120">You begin with specifying a plain text password convert it tooa secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span> <span data-ttu-id="e7b7d-121">Další tooconvert tento řetězec budete potřebovat zabezpečené do k zašifrovaný standardní řetězec pomocí [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7b7d-121">Next you need tooconvert this secure string into an encrypted standard string using [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span></span> <span data-ttu-id="e7b7d-122">Teď můžete uložit tento zašifrovaný standardní řetězec tooa souboru pomocí [obsah sady](https://technet.microsoft.com/library/ee176959.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7b7d-122">Now you can save this encrypted standard string tooa file using [Set-Content](https://technet.microsoft.com/library/ee176959.aspx).</span></span>

<span data-ttu-id="e7b7d-123">Můžete také vytvořit soubor zabezpečeného hesla tak, že nemáte tootype v hesle hello pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-123">You can also create a secure password file so that you don't have tootype in hello password every time.</span></span> <span data-ttu-id="e7b7d-124">Navíc soubor zabezpečeného hesla je lepší, než soubor ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-124">Also, a secure password file is better than a plain text file.</span></span> <span data-ttu-id="e7b7d-125">Použijte následující prostředí PowerShell toocreate soubor zabezpečeného hesla hello:</span><span class="sxs-lookup"><span data-stu-id="e7b7d-125">Use hello following PowerShell toocreate a secure password file:</span></span>

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> <span data-ttu-id="e7b7d-126">Při nastavování hesla hello, ujistěte se, že splňujete hello [požadavky na složitost](https://technet.microsoft.com/library/cc786468.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7b7d-126">When setting hello password, make sure that you meet hello [complexity requirements](https://technet.microsoft.com/library/cc786468.aspx).</span></span>
>
>

<span data-ttu-id="e7b7d-127">objekt pověření toocreate hello ze souboru hello zabezpečené heslo, musí přečíst obsah souboru hello a převést je zpět tooa zabezpečení řetězec pomocí [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7b7d-127">toocreate hello credential object from hello secure password file, you must read hello file contents and convert them back tooa secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span>

<span data-ttu-id="e7b7d-128">Hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutina také umožňuje *vypršení platnosti* parametr, který určuje **data a času** na které uživatele hello vypršení platnosti účtu.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-128">hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet also accepts an *Expiration* parameter, which specifies a **DateTime** at which hello user account expires.</span></span> <span data-ttu-id="e7b7d-129">Například můžete nastavit účet tooexpire hello několik dní od hello aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-129">For example, you could set hello account tooexpire a few days from hello current date and time.</span></span>

<span data-ttu-id="e7b7d-130">Tento příklad PowerShell ukazuje, jak tooset hello rozšíření vzdálené plochy na cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="e7b7d-130">This PowerShell example shows you how tooset hello Remote Desktop Extension on a cloud service:</span></span>

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
<span data-ttu-id="e7b7d-131">Můžete volitelně specifikovat hello nasazovací slot a rolí, které chcete tooenable vzdálené plochy na.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-131">You can also optionally specify hello deployment slot and roles that you want tooenable remote desktop on.</span></span> <span data-ttu-id="e7b7d-132">Pokud tyto parametry nejsou zadané, hello rutina umožňuje vzdálená plocha u všech rolí v hello **produkční** nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-132">If these parameters are not specified, hello cmdlet enables remote desktop on all roles in hello **Production** deployment slot.</span></span>

<span data-ttu-id="e7b7d-133">Hello rozšíření vzdálené plochy je přidružen k nasazení.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-133">hello Remote Desktop extension is associated with a deployment.</span></span> <span data-ttu-id="e7b7d-134">Pokud vytvoříte nové nasazení služby hello, máte tooenable vzdálené plochy na toto nasazení.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-134">If you create a new deployment for hello service, you have tooenable remote desktop on that deployment.</span></span> <span data-ttu-id="e7b7d-135">Pokud chcete vždy povolena Vzdálená plocha toohave, měli zvážit, integraci skriptů prostředí PowerShell hello do vašeho pracovního postupu nasazení.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-135">If you always want toohave remote desktop enabled, then you should consider integrating hello PowerShell scripts into your deployment workflow.</span></span>

## <a name="remote-desktop-into-a-role-instance"></a><span data-ttu-id="e7b7d-136">Vzdálená plocha do role instance</span><span class="sxs-lookup"><span data-stu-id="e7b7d-136">Remote Desktop into a role instance</span></span>
<span data-ttu-id="e7b7d-137">Hello [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) rutina je použité tooremote plochu do instance konkrétní role cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-137">hello [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet is used tooremote desktop into a specific role instance of your cloud service.</span></span> <span data-ttu-id="e7b7d-138">Můžete použít hello *Místnícesta* hello toodownload parametru protokolu RDP soubor místně.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-138">You can use hello *LocalPath* parameter toodownload hello RDP file locally.</span></span> <span data-ttu-id="e7b7d-139">Nebo můžete použít hello *spusťte* parametr toodirectly spuštění hello připojení ke vzdálené ploše dialogové okno tooaccess hello instance role cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-139">Or you can use hello *Launch* parameter toodirectly launch hello Remote Desktop Connection dialog tooaccess hello cloud service role instance.</span></span>

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a><span data-ttu-id="e7b7d-140">Zkontrolujte, zda rozšíření vzdálené plochy je povolena ve službě</span><span class="sxs-lookup"><span data-stu-id="e7b7d-140">Check if Remote Desktop extension is enabled on a service</span></span>
<span data-ttu-id="e7b7d-141">Hello [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) zobrazuje rutina, která povolená nebo zakázaná na nasazení služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-141">hello [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet displays that remote desktop is enabled or disabled on a service deployment.</span></span> <span data-ttu-id="e7b7d-142">Hello rutina vrátí uživatelské jméno hello hello uživatele vzdálené plochy a hello rolí, které vzdálené plochy rozšíření hello je povolený pro.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-142">hello cmdlet returns hello username for hello remote desktop user and hello roles that hello remote desktop extension is enabled for.</span></span> <span data-ttu-id="e7b7d-143">Ve výchozím nastavení k tomu dochází hello nasazovací slot a toouse hello pracovní pozici místo toho můžete zvolit.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-143">By default, this happens on hello deployment slot and you can choose toouse hello staging slot instead.</span></span>

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a><span data-ttu-id="e7b7d-144">Odebrání služby Vzdálená plocha rozšíření</span><span class="sxs-lookup"><span data-stu-id="e7b7d-144">Remove Remote Desktop extension from a service</span></span>
<span data-ttu-id="e7b7d-145">Pokud jste povolili rozšíření vzdálené plochy hello na nasazení a potřebovat tooupdate hello nastavení vzdálené plochy, odeberte nejprve hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-145">If you have already enabled hello remote desktop extension on a deployment, and need tooupdate hello remote desktop settings, first remove hello extension.</span></span> <span data-ttu-id="e7b7d-146">A znovu ji povolte s novým nastavením hello.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-146">And enable it again with hello new settings.</span></span> <span data-ttu-id="e7b7d-147">Například pokud chcete, aby tooset vypršení platnosti nové heslo pro účet hello vzdáleného uživatele nebo účet hello.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-147">For example, if you want tooset a new password for hello remote user account, or hello account expired.</span></span> <span data-ttu-id="e7b7d-148">To je potřeba na existující nasazení, které mají vzdálené plochy rozšíření hello povolené.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-148">Doing this is required on existing deployments that have hello remote desktop extension enabled.</span></span> <span data-ttu-id="e7b7d-149">Pro nová nasazení můžete jednoduše provést rozšíření hello přímo.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-149">For new deployments, you can simply apply hello extension directly.</span></span>

<span data-ttu-id="e7b7d-150">tooremove hello vzdálené plochy rozšíření z hello nasazení, můžete použít hello [odebrat AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-150">tooremove hello remote desktop extension from hello deployment, you can use hello [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="e7b7d-151">Můžete volitelně specifikovat hello slot nasazení a role, ze kterého mají být tooremove hello vzdálené plochy rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-151">You can also optionally specify hello deployment slot and role from which you want tooremove hello remote desktop extension.</span></span>

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> <span data-ttu-id="e7b7d-152">Konfigurace rozšíření hello toocompletely odebrat, by měly volat hello *odebrat* rutiny s hello **UninstallConfiguration** parametr.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-152">toocompletely remove hello extension configuration, you should call hello *remove* cmdlet with hello **UninstallConfiguration** parameter.</span></span>
>
> <span data-ttu-id="e7b7d-153">Hello **UninstallConfiguration** parametr odinstaluje všechny konfigurace rozšíření tedy použité toohello služby.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-153">hello **UninstallConfiguration** parameter uninstalls any extension configuration that is applied toohello service.</span></span> <span data-ttu-id="e7b7d-154">Všechny konfigurace rozšíření je přidružen hello konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-154">Every extension configuration is associated with hello service configuration.</span></span> <span data-ttu-id="e7b7d-155">Volání hello *odebrat* rutiny bez **UninstallConfiguration** zrušíte hello <mark>nasazení</mark> z konfigurace rozšíření hello, takže znamenalo vyjmutí rozšíření Hello.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-155">Calling hello *remove* cmdlet without **UninstallConfiguration** disassociates hello <mark>deployment</mark> from hello extension configuration, thus effectively removing hello extension.</span></span> <span data-ttu-id="e7b7d-156">Konfigurace rozšíření hello však zůstane hello službě přidružen.</span><span class="sxs-lookup"><span data-stu-id="e7b7d-156">However, hello extension configuration remains associated with hello service.</span></span>
>
>

## <a name="additional-resources"></a><span data-ttu-id="e7b7d-157">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e7b7d-157">Additional resources</span></span>

<span data-ttu-id="e7b7d-158">[Jak tooConfigure cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="e7b7d-158">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
