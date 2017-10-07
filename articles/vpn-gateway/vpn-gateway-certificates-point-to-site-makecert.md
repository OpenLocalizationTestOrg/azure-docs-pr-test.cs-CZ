---
title: "Generování a exportování certifikátů pro Point-to-Site: MakeCert: Azure | Microsoft Docs"
description: "Tento článek obsahuje kroky toocreate certifikát podepsaný svým držitelem kořenové, export veřejného klíče hello a generovat klientské certifikáty pomocí nástroje MakeCert."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: 0b795ccf74f1f97fbd3de8a0dc339f9cb0b48183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Generování a exportování certifikátů pro připojení Point-to-Site pomocí nástroje MakeCert

Připojení point-to-Site pomocí tooauthenticate certifikáty. Tento článek ukazuje, jak toocreate a podepsaný svým držitelem kořenový certifikát a generovat klientské certifikáty pomocí nástroje MakeCert. Pokud hledáte kroků konfigurace Point-to-Site, například jak tooupload kořenové certifikáty, vyberte jeden z článků hello ' konfigurace Point-to-Site, z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Vytvořit certifikáty podepsané svým držitelem – prostředí PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Vytvořit certifikáty podepsané svým držitelem - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Konfigurace Point-to-Site - Resource Manager – portál Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Konfigurace Point-to-Site - Resource Manager – prostředí PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Konfigurace Point-to-Site - Classic - portálu Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Když vám doporučujeme používat hello [kroky Windows 10 PowerShell](vpn-gateway-certificates-point-to-site.md) toocreate certifikáty, jsou tyto pokyny MakeCert jako metodu volitelné. Hello certifikáty, které můžete vygenerovat pomocí obou těchto metod se dá nainstalovat na [žádný operační systém podporovaný klient](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). MakeCert má však hello následující omezení:

* MakeCert je zastaralý. To znamená, že tento nástroj nebylo možné odebrat kdykoli. Všechny certifikáty, které již vygenerován pomocí nástroje MakeCert nebude mít vliv, pokud MakeCert již není k dispozici. MakeCert není jenom využité toogenerate hello certifikáty, jako mechanismus ověřování.

## <a name="rootcert"></a>Vytvořit certifikát podepsaný svým držitelem kořenové

Hello následující kroky vám ukážou, jak toocreate a podepsaný svým držitelem certifikátu pomocí nástroje MakeCert. Tyto kroky nejsou specifické pro model nasazení. Jsou platné pro Resource Manager a Klasický model.

1. Stáhněte a nainstalujte [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. Po instalaci se obvykle nachází nástroje makecert.exe hello pod touto cestou: ' C:\Program Files (x86) \Windows Kits\10\bin\<architektura >'. Přestože je možné, že byla nainstalovaná tooanother umístění. Otevřete příkazový řádek jako správce a přejděte toohello umístění hello nástroj MakeCert. Můžete použít následující příklad, úpravě pro správné umístění hello hello:

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Vytvoření a instalaci certifikátu v úložišti osobních certifikátů hello ve vašem počítači. Hello následující příklad vytvoří odpovídající *.cer* soubor nahrát tooAzure, při konfiguraci P2S. Nahraďte název hello chcete toouse pro certifikát hello 'P2SRootCert' a 'P2SRootCert.cer'. Hello certifikát je umístěn ve vaší 'certifikáty – aktuální User\Personal\Certificates'.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
  ```

## <a name="cer"></a>Export hello veřejného klíče (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

nahrané tooAzure musí být soubor exported.cer Hello. Pokyny najdete v tématu [konfigurace připojení typu Point-to-Site](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). tooadd další důvěryhodný kořenový certifikát, najdete v části [v této části](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) hello článku.

### <a name="export-hello-self-signed-certificate-and-private-key-toostore-it-optional"></a>Exportovat certifikát podepsaný svým držitelem hello a privátního klíče toostore ho (volitelné)

Můžete chtít tooexport hello kořenového certifikátu podepsaného svým držitelem a bezpečně uložit. Pokud třeba, můžete později jej nainstalovat na jiný počítač a generovat další klientské certifikáty nebo exportovat jiný soubor .cer. tooexport hello certifikátu podepsaného svým držitelem kořenové jako PFX, vyberte hello kořenový certifikát a použití hello stejný postup, jak je popsáno v [exportovat certifikát klienta](#clientexport).

## <a name="create-and-install-client-certificates"></a>Vytvoření a instalace klientských certifikátů

Certifikát podepsaný svým držitelem hello neinstalujte přímo na klientském počítači hello. Je nutné toogenerate certifikát klienta z hello certifikát podepsaný svým držitelem. Budete pak exportovat a nainstalujte hello klientský certifikát toohello klientský počítač. Následující kroky Hello nejsou specifické pro model nasazení. Jsou platné pro Resource Manager a Klasický model.

### <a name="clientcert"></a>Vygenerování klientského certifikátu

Každý klientský počítač, který se připojuje tooa Point-to-Site pomocí virtuální síť musí mít nainstalovaný certifikát klienta. Vygenerujte certifikát klienta z hello podepsaný svým držitelem kořenový certifikát a pak je exportovat a nainstalovat certifikát klienta hello. Pokud hello klientský certifikát není nainstalovaná, ověření se nezdaří. 

Hello následující kroky vás provedou vygenerování klientského certifikátu z certifikát podepsaný svým držitelem kořenové. Více klientských certifikátů může vygenerovat z hello stejným kořenovým certifikátem. Při generování klientských certifikátů pomocí následujících kroků hello hello klientský certifikát je automaticky nainstaluje na počítač hello použitá toogenerate hello certifikátu. Pokud chcete tooinstall klientský certifikát na jiném počítači klienta, můžete exportovat certifikát hello.
 
1. Na hello stejný počítač, kterou jste použili toocreate hello certifikátu podepsaného svým držitelem, otevřete příkazový řádek jako správce.
2. Upravit a spustit ukázkový toogenerate hello klientský certifikát.
  * Změna *"P2SRootCert"* toohello název hello podepsaný svým držitelem kořenové, které generují hello klientský certifikát z. Zajistěte, aby se pomocí názvu hello hello kořenového certifikátu, který je ať hello "CN =' hodnota byla, kterou jste zadali při vytvoření hello podepsaný svým držitelem kořenové.
  * Změna *P2SChildCert* toohello název, který má toogenerate toobe certifikát klienta.

  Pokud spustíte hello následující příklad beze změny jeho, výsledkem hello je klientský certifikát s názvem P2SChildcert v osobním úložišti certifikátů generované z kořenového certifikátu P2SRootCert.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
  ```

### <a name="clientexport"></a>Export certifikátu klienta

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Nainstalovat certifikát exportovaného klienta

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Další kroky

V konfiguraci Point-to-Site pokračujte. 

* Pro **Resource Manager** postup nasazení modelu najdete v tématu [konfigurace tooa připojení Point-to-Site VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Pro **classic** postup nasazení modelu najdete v tématu [konfigurace tooa připojení Point-to-Site VPN virtuální sítě (klasické)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
