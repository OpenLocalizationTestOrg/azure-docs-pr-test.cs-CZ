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
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Portál Azure Classic](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)
>
>

Vzdálená plocha umožňuje tooaccess hello ploše role, která běží v Azure. Můžete použít tootroubleshoot připojení vzdálené plochy a diagnostikovat problémy s vaší aplikací, když je spuštěná.

Tento článek popisuje, jak tooenable Vzdálená plocha u role cloudové služby pomocí prostředí PowerShell. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) pro hello součásti potřebné k tomuto článku. Prostředí PowerShell využívá hello rozšíření vzdálené plochy, Vzdálená plocha můžete povolit po nasazení aplikace hello.

## <a name="configure-remote-desktop-from-powershell"></a>Konfigurace vzdálené plochy z prostředí PowerShell
Hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutiny vám umožní tooenable vzdálené plochy na určené role nebo všechny role nasazení cloudové služby. rutiny Hello umožňuje určit hello uživatelské jméno a heslo pro uživatele vzdálené plochy hello prostřednictvím hello *pověření* parametr, který přijímá objekt PSCredential.

Pokud používáte prostředí PowerShell interaktivně, můžete snadno nastavit objektu PSCredential hello pomocí volání hello [Get-pověření](https://technet.microsoft.com/library/hh849815.aspx) rutiny.

```
$remoteusercredentials = Get-Credential
```

Tento příkaz zobrazí dialogové okno, umožní vám tooenter hello uživatelské jméno a heslo pro vzdálené uživatele hello zabezpečeným způsobem.

Vzhledem k tomu, že prostředí PowerShell pomáhá scénáře automatizace, můžete také nastavit hello **PSCredential** objekt způsobem, který nevyžaduje zásah uživatele. Nejprve je třeba tooset zabezpečené heslo. Začínat zadání hesla v podobě prostého textu ji převést zabezpečený řetězec tooa pomocí [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Další tooconvert tento řetězec budete potřebovat zabezpečené do k zašifrovaný standardní řetězec pomocí [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx). Teď můžete uložit tento zašifrovaný standardní řetězec tooa souboru pomocí [obsah sady](https://technet.microsoft.com/library/ee176959.aspx).

Můžete také vytvořit soubor zabezpečeného hesla tak, že nemáte tootype v hesle hello pokaždé, když. Navíc soubor zabezpečeného hesla je lepší, než soubor ve formátu prostého textu. Použijte následující prostředí PowerShell toocreate soubor zabezpečeného hesla hello:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> Při nastavování hesla hello, ujistěte se, že splňujete hello [požadavky na složitost](https://technet.microsoft.com/library/cc786468.aspx).
>
>

objekt pověření toocreate hello ze souboru hello zabezpečené heslo, musí přečíst obsah souboru hello a převést je zpět tooa zabezpečení řetězec pomocí [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).

Hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutina také umožňuje *vypršení platnosti* parametr, který určuje **data a času** na které uživatele hello vypršení platnosti účtu. Například můžete nastavit účet tooexpire hello několik dní od hello aktuální datum a čas.

Tento příklad PowerShell ukazuje, jak tooset hello rozšíření vzdálené plochy na cloudové služby:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Můžete volitelně specifikovat hello nasazovací slot a rolí, které chcete tooenable vzdálené plochy na. Pokud tyto parametry nejsou zadané, hello rutina umožňuje vzdálená plocha u všech rolí v hello **produkční** nasazovací slot.

Hello rozšíření vzdálené plochy je přidružen k nasazení. Pokud vytvoříte nové nasazení služby hello, máte tooenable vzdálené plochy na toto nasazení. Pokud chcete vždy povolena Vzdálená plocha toohave, měli zvážit, integraci skriptů prostředí PowerShell hello do vašeho pracovního postupu nasazení.

## <a name="remote-desktop-into-a-role-instance"></a>Vzdálená plocha do role instance
Hello [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) rutina je použité tooremote plochu do instance konkrétní role cloudové služby. Můžete použít hello *Místnícesta* hello toodownload parametru protokolu RDP soubor místně. Nebo můžete použít hello *spusťte* parametr toodirectly spuštění hello připojení ke vzdálené ploše dialogové okno tooaccess hello instance role cloudové služby.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Zkontrolujte, zda rozšíření vzdálené plochy je povolena ve službě
Hello [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) zobrazuje rutina, která povolená nebo zakázaná na nasazení služby Vzdálená plocha. Hello rutina vrátí uživatelské jméno hello hello uživatele vzdálené plochy a hello rolí, které vzdálené plochy rozšíření hello je povolený pro. Ve výchozím nastavení k tomu dochází hello nasazovací slot a toouse hello pracovní pozici místo toho můžete zvolit.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Odebrání služby Vzdálená plocha rozšíření
Pokud jste povolili rozšíření vzdálené plochy hello na nasazení a potřebovat tooupdate hello nastavení vzdálené plochy, odeberte nejprve hello rozšíření. A znovu ji povolte s novým nastavením hello. Například pokud chcete, aby tooset vypršení platnosti nové heslo pro účet hello vzdáleného uživatele nebo účet hello. To je potřeba na existující nasazení, které mají vzdálené plochy rozšíření hello povolené. Pro nová nasazení můžete jednoduše provést rozšíření hello přímo.

tooremove hello vzdálené plochy rozšíření z hello nasazení, můžete použít hello [odebrat AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) rutiny. Můžete volitelně specifikovat hello slot nasazení a role, ze kterého mají být tooremove hello vzdálené plochy rozšíření.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> Konfigurace rozšíření hello toocompletely odebrat, by měly volat hello *odebrat* rutiny s hello **UninstallConfiguration** parametr.
>
> Hello **UninstallConfiguration** parametr odinstaluje všechny konfigurace rozšíření tedy použité toohello služby. Všechny konfigurace rozšíření je přidružen hello konfigurace služby. Volání hello *odebrat* rutiny bez **UninstallConfiguration** zrušíte hello <mark>nasazení</mark> z konfigurace rozšíření hello, takže znamenalo vyjmutí rozšíření Hello. Konfigurace rozšíření hello však zůstane hello službě přidružen.
>
>

## <a name="additional-resources"></a>Další zdroje

[Jak tooConfigure cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)
