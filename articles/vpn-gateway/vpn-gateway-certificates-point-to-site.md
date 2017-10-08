---
title: "Generování a exportování certifikátů pro Point-to-Site: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek obsahuje kroky toocreate certifikát podepsaný svým držitelem kořenové, export veřejného klíče hello a generovat klientské certifikáty pomocí prostředí PowerShell ve Windows 10."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: 11dda015368cda5ce9799fcc4f01d7c542b84fe8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a>Generování a exportování certifikátů pro připojení Point-to-Site pomocí prostředí PowerShell ve Windows 10

Připojení point-to-Site pomocí tooauthenticate certifikáty. Tento článek ukazuje, jak toocreate a podepsaný svým držitelem kořenový certifikát a generovat klientské certifikáty pomocí prostředí PowerShell ve Windows 10. Pokud hledáte kroků konfigurace Point-to-Site, například jak tooupload kořenové certifikáty, vyberte jeden z článků hello ' konfigurace Point-to-Site, z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Vytvořit certifikáty podepsané svým držitelem – prostředí PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Vytvořit certifikáty podepsané svým držitelem - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Konfigurace Point-to-Site - Resource Manager – portál Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Konfigurace Point-to-Site - Resource Manager – prostředí PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Konfigurace Point-to-Site - Classic - portálu Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


Je třeba provést hello kroky v tomto článku v počítači se systémem Windows 10. rutiny prostředí PowerShell Hello použít toogenerate certifikáty jsou součástí operačního systému hello Windows 10 a nefungují v jiných verzích Windows. počítač Hello Windows 10 je jenom certifikáty potřebné toogenerate hello. Po vygenerování hello certifikáty jsou můžete odešlete, nebo je nainstalovat pro všechny podporované klientské operační systémy. 

Pokud nemáte přístup k počítači tooa Windows 10, můžete použít [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate certifikáty. Hello certifikáty, které generují pomocí obou těchto metod se dá nainstalovat na všechny [podporované](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) klientský operační systém.

## <a name="rootcert"></a>Vytvořit certifikát podepsaný svým držitelem kořenové

Použijte toocreate rutiny New-SelfSignedCertificate hello certifikát podepsaný svým držitelem kořenové. Parametr Další informace najdete v tématu [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

1. Z počítače se systémem Windows 10 otevřete konzolu prostředí Windows PowerShell se zvýšenými oprávněními.
2. Použijte následující příklad toocreate hello podepsaný svým držitelem kořenový certifikát hello. Hello následující příklad vytvoří certifikát podepsaný svým držitelem kořenové s názvem 'P2SRootCert', který je automaticky nainstalován v 'Certifikáty – aktuální User\Personal\Certificates'. Můžete zobrazit certifikát hello otevřením *certmgr.msc*, nebo *Správa uživatelských certifikátů*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <a name="cer"></a>Export hello veřejného klíče (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

nahrané tooAzure musí být soubor exported.cer Hello. Pokyny najdete v tématu [konfigurace připojení typu Point-to-Site](vpn-gateway-howto-point-to-site-rm-ps.md#upload). tooadd další důvěryhodný kořenový certifikát [v této části](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) hello článku.

### <a name="export-hello-self-signed-root-certificate-and-public-key-toostore-it-optional"></a>Exportovat certifikát podepsaný svým držitelem kořenové hello a veřejného klíče toostore ho (volitelné)

Můžete chtít tooexport hello kořenového certifikátu podepsaného svým držitelem a bezpečně uložit. Pokud třeba, můžete později jej nainstalovat na jiný počítač a generovat další klientské certifikáty nebo exportovat jiný soubor .cer. tooexport hello certifikátu podepsaného svým držitelem kořenové jako PFX, vyberte hello kořenový certifikát a použití hello stejný postup, jak je popsáno v [exportovat certifikát klienta](#clientexport).

## <a name="clientcert"></a>Vygenerování klientského certifikátu

Každý klientský počítač, který se připojuje tooa Point-to-Site pomocí virtuální síť musí mít nainstalovaný certifikát klienta. Vygenerujte certifikát klienta z hello podepsaný svým držitelem kořenový certifikát a pak je exportovat a nainstalovat certifikát klienta hello. Pokud hello klientský certifikát není nainstalovaná, ověření se nezdaří. 

Hello následující kroky vás provedou vygenerování klientského certifikátu z certifikát podepsaný svým držitelem kořenové. Více klientských certifikátů může vygenerovat z hello stejným kořenovým certifikátem. Při generování klientských certifikátů pomocí následujících kroků hello hello klientský certifikát je automaticky nainstaluje na počítač hello použitá toogenerate hello certifikátu. Pokud chcete tooinstall klientský certifikát na jiném počítači klienta, můžete exportovat certifikát hello.

Příklady Hello pomocí toogenerate rutiny New-SelfSignedCertificate hello klientský certifikát, který vyprší za jeden rok. Dalším parametr informace, například nastavení hodnoty různých vypršení platnosti certifikátu klienta se hello, najdete v části [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

### <a name="example-1"></a>Příklad 1

Tento příklad používá hello deklarovat proměnnou '$cert' z předchozí části hello. Pokud jste zavřeli konzole PowerShell hello po vytvoření hello certifikát podepsaný svým držitelem kořenové, nebo vytváříte další klientské certifikáty v nové relaci konzoly prostředí PowerShell, použijte kroky hello v [příklad 2](#ex2).

Upravit a spustit toogenerate příklad hello klientský certifikát. Pokud spustíte hello následující příklad beze změny jeho, výsledkem hello je klientský certifikát s názvem 'P2SChildCert'.  Pokud chcete certifikát podřízené hello tooname něco jiného, upravte hodnotu CN hello. Neměňte hello TextExtension při spuštění v tomto příkladu. Hello klientský certifikát, který je generovat se automaticky nainstaluje v 'Certifikáty – aktuální User\Personal\Certificates' ve vašem počítači.

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Příklad 2

Pokud vytváříte další klientské certifikáty, nebo jsou nepoužíváte hello stejné relace prostředí PowerShell, kterou jste použili toocreate certifikát podepsaný svým držitelem kořenové, hello použijte následující kroky:

1. Identifikujte hello podepsaný svým držitelem kořenový certifikát, který je nainstalován na počítači hello. Tato rutina vrátí seznam certifikátů, které jsou nainstalovány ve vašem počítači.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Najděte název subjektu hello z hello vrátí seznam, pak kryptografický otisk hello kopie, který je umístěn další text tooa tooit souboru. V následujícím příkladu hello existují dva certifikáty. Název CN Hello je název hello hello podepsaný svým držitelem kořenového certifikátu, ze kterého mají být toogenerate certifikát podřízené. V tomto případě 'P2SRootCert'.

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. Deklarujte proměnnou pro hello kořenový certifikát pomocí kryptografického otisku hello hello v předchozím kroku. Nahraďte OTISK hello kryptografický otisk hello kořenový certifikát, ze kterého mají být toogenerate certifikát podřízené.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Například pomocí hello kryptografický otisk pro P2SRootCert v předchozím kroku hello, proměnná hello vypadá takto:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  Upravit a spustit toogenerate příklad hello klientský certifikát. Pokud spustíte hello následující příklad beze změny jeho, výsledkem hello je klientský certifikát s názvem 'P2SChildCert'. Pokud chcete certifikát podřízené hello tooname něco jiného, upravte hodnotu CN hello. Neměňte hello TextExtension při spuštění v tomto příkladu. Hello klientský certifikát, který je generovat se automaticky nainstaluje v 'Certifikáty – aktuální User\Personal\Certificates' ve vašem počítači.

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="clientexport"></a>Export certifikátu klienta   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <a name="install"></a>Nainstalovat certifikát exportovaného klienta

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Další kroky

V konfiguraci Point-to-Site pokračujte. 

* Pro **Resource Manager** postup nasazení modelu najdete v tématu [konfigurace tooa připojení Point-to-Site VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* Pro **classic** postup nasazení modelu najdete v tématu [konfigurace tooa připojení Point-to-Site VPN virtuální sítě (klasické)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
