---
title: "aaaCannot odstranění virtuální sítě v Azure | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot hello problém, ve kterém nelze odstranit virtuální sítě v Azure."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: genli
ms.openlocfilehash: a9050ab238ccb0380fd46130430222efb8f42388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-failed-toodelete-a-virtual-network-in-azure"></a>Řešení potíží: Toodelete virtuální sítě v Azure se nezdařilo

Při pokusu o toodelete virtuální sítě v Microsoft Azure se může zobrazit chyby. Tento článek obsahuje řešení problémů s kroky toohelp vyřešíte tento problém. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Pokyny při řešení potíží 

1. [Zkontrolujte, zda bránu virtuální sítě běží ve virtuální síti hello](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Zkontrolujte, zda služby application gateway běží ve virtuální síti hello](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Zkontrolujte, zda je povolena služba Azure Active Directory Domain Services ve virtuální síti hello](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Zkontrolujte, zda hello virtuální sítě je připojený tooother prostředků](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. [Zkontrolujte, zda je virtuální počítač stále spuštěna ve virtuální síti hello](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Zkontrolujte, zda text hello virtuální sítě se zasekla v automatickém migrace](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>Řešení potíží

### <a name="check-whether-a-virtual-network-gateway-is-running-in-hello-virtual-network"></a>Zkontrolujte, zda bránu virtuální sítě běží ve virtuální síti hello

tooremove hello virtuální síť, musíte nejprve odebrat hello brány virtuální sítě.

Pro klasické virtuální sítě přejděte toohello **přehled** stránku hello v hello portál Azure classic virtuální sítě. V hello **připojení k síti VPN** část, pokud hello brána je spuštěná ve virtuální síti hello, zobrazí se hello IP adresu brány hello. 

![Zkontrolujte, jestli je brána spuštěná.](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Pro virtuální sítě přejděte toohello **přehled** stránku hello virtuální sítě. Zkontrolujte **připojená zařízení** pro bránu virtuální sítě hello.

![Zkontrolujte hello připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Před odebráním hello bránu, nejprve odeberte **připojení** objekty v hello brány. 

### <a name="check-whether-an-application-gateway-is-running-in-hello-virtual-network"></a>Zkontrolujte, zda je ve virtuální síti hello spuštěn aplikační brány

Přejděte toohello **přehled** stránku hello virtuální sítě. Zkontrolujte hello **připojená zařízení** hello aplikační brány.

![Zkontrolujte hello připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Pokud je služby application gateway, musí odebrat před odstraněním hello virtuální sítě.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-hello-virtual-network"></a>Zkontrolujte, zda je povolena služba Azure Active Directory Domain Services ve virtuální síti hello

Pokud hello služba Active Directory Domain Services je toohello povolené a připojené virtuální sítě, nelze odstranit tuto virtuální síť. 

![Zkontrolujte hello připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

toodisable hello služby, postupujte takto:

1. Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).
2. V levém podokně hello vyberte **služby Active Directory**.
3. Vyberte adresář hello Azure Active Directory (Azure AD), který má služba Active Directory Domain Services povoleno.
4. Vyberte hello **konfigurace** kartě.
5. V části **služby domény**, změňte hello **povolit doménové služby pro tento adresář** možnost příliš**ne**.  

### <a name="check-whether-hello-virtual-network-is-connected-tooother-resource"></a>Zkontrolujte, zda hello virtuální sítě je připojený tooother prostředků

Zkontrolujte propojení okruhu, připojení a partnerských vztahů virtuální sítě. Některý z těchto může způsobit odstranění toofail virtuální sítě. 

Hello doporučené odstranění pořadí je následující:

1. Připojení brány
2. Brány
3. IP adresy
4. Partnerské vztahy virtuální sítě
5. Služba App Service Environment (App Service Environment)

### <a name="check-whether-a-virtual-machine-is-still-running-in-hello-virtual-network"></a>Zkontrolujte, zda je virtuální počítač stále spuštěna ve virtuální síti hello

Ujistěte se, že žádný virtuální počítač je ve virtuální síti hello.

### <a name="check-whether-hello-virtual-network-is-stuck-in-migration"></a>Zkontrolujte, zda text hello virtuální sítě se zasekla v automatickém migrace

Pokud hello virtuální sítě se zasekla v automatickém migrace stavu, nelze jej odstranit. Spusťte následující příkaz tooabort hello migrace hello a pak odstraňte hello virtuální sítě.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Další kroky

- [Azure Virtual Network](virtual-networks-overview.md)
- [Nejčastější dotazy ke službě Azure Virtual Network](virtual-networks-faq.md)