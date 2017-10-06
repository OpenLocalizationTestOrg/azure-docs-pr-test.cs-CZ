---
title: "aaaCreate služby Azure Application Gateway - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate služby Application Gateway pomocí Azure CLI 1.0 ve službě Správce prostředků"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3c0d2d96b6be404d0372d00f0deb2a32959ca419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli"></a>Vytvoření služby application gateway pomocí hello rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)
> 
> 

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně. Application gateway poskytuje následující funkce doručování aplikací hello: HTTP načíst vlastní stavu sondy vyrovnávání, spřažení relace na základě souborů cookie a přesměrování zpracování Secure Sockets Layer (SSL) a podpora pro více lokalit.

## <a name="prerequisite-install-hello-azure-cli"></a>Předpoklad: Instalace hello rozhraní příkazového řádku Azure

tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](../xplat-cli-install.md) a potřebujete příliš[přihlásit tooAzure](../xplat-cli-connect.md). 

> [!NOTE]
> Pokud nemáte účet Azure, budete potřebovat. Zde si můžete zaregistrovat [bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scénář

V tomto scénáři zjistíte, jak hello toocreate brány aplikace pomocí portálu Azure.

Tento scénář se:

* Vytvoření střední application gateway se dvěma instancemi.
* Vytvořte virtuální síť s názvem ContosoVNET s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem subnet01, který používá jako jeho blok CIDR 10.0.0.0/28.

> [!NOTE]
> Další konfigurace hello aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po hello aplikace brána je nakonfigurovaná a ne během počátečního nasazení.

## <a name="before-you-begin"></a>Než začnete

Služba Azure Application Gateway vyžaduje vlastní podsíti. Při vytváření virtuální sítě, ujistěte se, ponechte dostatek místa toohave adresu více podsítí. Jakmile nasadíte tooa podsítě application gateway, jsou pouze další aplikaci brány možné toobe přidat toohello podsítě.

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Otevřete hello **Microsoft Azure příkazového řádku**a přihlaste se. 

```azurecli-interactive
azure login
```

Jakmile zadáte hello předchozím příkladu, je k dispozici kód. Přejděte toohttps://aka.ms/devicelogin v procesu přihlášení hello toocontinue prohlížeče.

![cmd zobrazující zařízení přihlášení][1]

V prohlížeči hello zadejte kód hello, kterou jste obdrželi. Jste přihlašovací stránku přesměrovaného tooa.

![Prohlížeč tooenter kódu][2]

Jakmile byl zadán kód hello jste přihlášeni, Zavřít hello prohlížeče toocontinue se scénářem hello.

![Úspěšné přihlášení][3]

## <a name="switch-tooresource-manager-mode"></a>Přepínač tooResource režimu Manager

```azurecli-interactive
azure config mode arm
```

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Před vytvořením hello aplikační bránu, se vytvoří skupinu prostředků toocontain hello aplikační brány. Následující Hello ukazuje příkaz hello.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě

Po vytvoření skupiny prostředků hello vytvoření virtuální sítě pro službu hello application gateway.  V následujícím příkladu hello hello adresní prostor se jako 10.0.0.0/16 jak je definována v předchozím scénáři poznámky hello.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Vytvoření podsítě

Po vytvoření virtuální sítě hello je hello aplikační brány přidat podsíť.  Pokud máte v plánu toouse aplikační bránu s webové aplikace hostované v hello stejné virtuální síť jako hello aplikační brány, nebude se tooleave dostatek místa pro jinou podsíť.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-hello-application-gateway"></a>Vytvoření hello application gateway

Po vytvoření hello virtuální síť a podsíť hello předpoklady pro službu hello application gateway jsou dokončeny. Kromě toho jsou požadovány pro hello následující krok .pfx dříve exportovaný certifikát a hello heslo pro certifikát hello: hello adres IP použitých pro back-end hello jsou hello IP adresy pro back-end serveru. Tyto hodnoty mohou být buď soukromé IP adresy ve virtuální síti hello, veřejné IP adresy nebo plně kvalifikované názvy domény pro back-end serverů.

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> Seznam parametrů, které lze zadat během vytváření, spusťte následující příkaz hello: **síť azure aplikace gateway vytvořit – pomáhají**.

Tento příklad vytvoří základní aplikační brána s výchozím nastavením pro naslouchací proces hello, fond back-end, nastavení http back-end a pravidla. Můžete upravit tyto toosuit nastavení nasazení po úspěšné hello zřizování.
Pokud již máte webovou aplikaci definované s fondem back-end hello v předchozích krocích, po vytvoření, hello Vyrovnávání zatížení začne.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak testy toocreate vlastní stavu navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)

Zjistěte, jak tooconfigure snižování zátěže protokolu SSL a proveďte hello nákladná SSL dešifrování vypnout navštivte stránky webových serverů [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
