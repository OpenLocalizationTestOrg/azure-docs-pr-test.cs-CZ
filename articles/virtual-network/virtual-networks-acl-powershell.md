---
title: "seznamy řízení aaaManage přístup koncového bodu Azure | Prostředí PowerShell | Classic | Microsoft Docs"
description: "Zjistěte, jak toomanage seznamy ACL v prostředí PowerShell"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: a7ca241ea108a266085bfb689b742d781e58da1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-hello-classic-deployment-model"></a>Správa seznamů řízení přístupu koncový bod pomocí prostředí PowerShell v modelu nasazení classic hello
Můžete vytvořit a spravovat sítě seznamy řízení přístupu (ACL) pro koncové body pomocí prostředí Azure PowerShell nebo v hello portálu pro správu. V tomto tématu najdete postupy pro seznam ACL běžné úkoly, které můžete dokončit pomocí prostředí PowerShell. Hello seznam prostředí Azure PowerShell rutin najdete v části [rutiny pro správu Azure](http://go.microsoft.com/fwlink/?LinkId=317721). Další informace o seznamy řízení přístupu najdete v tématu [co je seznamu pro řízení přístupu sítě (ACL)?](virtual-networks-acl.md). Pokud toomanage chcete pomocí portálu pro správu hello vaše seznamy ACL, najdete v části [jak tooSet až koncové body tooa virtuální počítač](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Spravovat seznamy ACL sítě pomocí prostředí Azure PowerShell
Můžete použít toocreate rutin prostředí Azure PowerShell, odebrat a nakonfigurovat (set) sítě seznamy řízení přístupu (ACL). Jsme zahrnuli několik příkladů hello způsoby, kterými můžete nakonfigurovat seznam ACL pomocí prostředí PowerShell.

Úplný seznam rutin prostředí PowerShell seznamu ACL hello tooretrieve, můžete použít některý z následujících hello:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Vytvoření seznamu ACL sítě pomocí pravidel, které umožňují přístup ze vzdálené podsíti
Hello následující příklad ukazuje způsob toocreate nového seznamu ACL, který obsahuje pravidla. Tento seznam ACL se potom použije tooa koncový bod virtuálního počítače. pravidla seznamu ACL Hello v následujícím příkladu hello vám umožní přístup ze vzdálené podsíti. toocreate nového seznamu ACL sítě s povolení pravidel pro vzdálené podsíti, otevřete PowerShell ISE Azure. Zkopírujte a vložte níže, nakonfigurováním skriptu hello vlastními hodnotami hello skript a spusťte skript hello.

1. Vytvořte nový objekt seznamu ACL sítě hello.
   
        $acl1 = New-AzureAclConfig
2. Nastavte pravidlo, která umožňuje přístup ze vzdálené podsíti. V příkladu hello níže můžete nastavit pravidlo *100* (která má přednost před pravidlo 200 a vyšší) tooallow hello vzdálené podsíti *10.0.0.0/8* přístup toohello koncový bod virtuálního počítače. Nahraďte hodnoty hello vlastní požadavky na konfiguraci. Název Hello "Konfigurace služby SharePoint ACL" by měl být nahrazen hello popisný název, který má toocall toto pravidlo.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. Pro další pravidla opakujte hello rutinu a nahraďte hodnoty hello s vlastní požadavky na konfiguraci. Být, že toochange hello pravidlo číslo pořadí tooreflect hello pořadí, ve kterém chcete toobe hello pravidla použít. Hello nižší číslo pravidla mají přednost před hello vyšší číslo.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. Potom můžete vytvořit nový koncový bod (Přidat) nebo nastavit hello seznamu ACL pro existující koncový bod (Set). V tomto příkladu přidáme nový koncový bod virtuálního počítače s názvem "web" a aktualizací hello koncový bod virtuálního počítače s hello nastavení seznamu ACL.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. V dalším kroku kombinovat hello rutiny a spusťte skript hello. V tomto příkladu hello kombinované rutiny bude vypadat takto:
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Odebrat pravidlo seznamu ACL sítě, která umožňuje přístup ze vzdálené podsíti
Hello následující příklad ukazuje způsob tooremove pravidlo seznamu ACL sítě.  tooremove pravidlo seznamu ACL sítě s povolení pravidel pro vzdálené podsíti, otevřete PowerShell ISE Azure. Zkopírujte a vložte níže, nakonfigurováním skriptu hello vlastními hodnotami hello skript a spusťte skript hello.

1. Prvním krokem je objekt sítě ACL hello tooget pro koncový bod virtuálního počítače hello. Budete pak odeberte pravidlo seznamu ACL hello. V takovém případě jsme se odebrat ji podle ID pravidla. ID pravidla hello 0 tato akce odebere pouze z hello seznamu ACL. Objekt seznamu ACL hello nejsou odebrány z hello koncový bod virtuálního počítače.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. Dále musíte použít hello seznamu ACL sítě objekt toohello koncový bod virtuálního počítače a aktualizovat hello virtuální počítač.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Odeberte seznam ACL sítě z koncový bod virtuálního počítače
V některých případech můžete chtít tooremove objekt seznamu ACL sítě od koncový bod virtuálního počítače. toodo, otevřete PowerShell ISE Azure. Zkopírujte a vložte níže, nakonfigurováním skriptu hello vlastními hodnotami hello skript a spusťte skript hello.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Další kroky
[Co je seznamu pro řízení přístupu sítě (ACL)?](virtual-networks-acl.md)

