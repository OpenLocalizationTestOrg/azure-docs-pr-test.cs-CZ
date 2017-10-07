---
title: "cloudové služby role aaaAzure – nejčastější dotazy | Microsoft Docs"
description: "Časté otázky k Azure Cloud Services. Odpovídá na některé běžné dotazy týkající se certifikátů, webových rolí a rolí pracovního procesu."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: b07a1990e031e60ae919a5f7c636945b89c7d3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-services-faq"></a>Cloud Services – nejčastější dotazy
Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se služby Microsoft Azure Cloud. Můžete také navštívit hello [Azure podporují – nejčastější dotazy](http://go.microsoft.com/fwlink/?LinkID=185083) obecné Azure – ceny a podporu informace. Můžete také obrátit hello [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

## <a name="certificates"></a>Certifikáty
### <a name="where-should-i-install-my-certificate"></a>Kde nainstalujte tento certifikát?
* **Moje**  
  Aplikace certifikát s privátním klíčem (\*.pfx, \*.p12).
* **CERTIFIKAČNÍ AUTORITY**  
  Všechny zprostředkující certifikáty přejděte v tomto úložišti (zásady a Sub CAs).
* **KOŘENOVÉ**  
  Hello kořenové certifikační Autority uložte, aby vaše hlavní kořenový certifikát certifikační Autority by měl přejděte sem.

### <a name="i-cant-remove-expired-certificate"></a>Vypršela platnost certifikát nelze odebrat
Azure zabrání odebírání certifikátu, pokud se používá. Potřebujete tooeither odstranění hello nasazení, které používá certifikát hello nebo nasazení hello aktualizace s jinou nebo obnovený certifikát.

### <a name="delete-an-expired-certificate"></a>Odstranit certifikát s vypršenou platností
Dokud není certifikát hello používá, můžete použít hello [odebrat AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) tooremove rutiny prostředí PowerShell certifikát.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>I vypršela platnost certifikátů s názvem Windows Azure Service Management pro rozšíření
Tyto certifikáty se vytvoří vždy, když rozšíření je přidány toohello cloudové službě, například hello rozšíření vzdálené plochy. Tyto certifikáty se používají jenom pro šifrování a dešifrování hello privátní konfigurace rozšíření hello. Pokud tyto certifikáty vyprší nezáleží. Datum vypršení platnosti Hello není zaškrtnuto.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Zobrazují se certifikátů, které mají vymazání
Tyto zobrazují se pravděpodobně z důvodu nástroj, který používáte, jako je například Visual Studio. Vždy, když se znovu připojíte s nástroj, který používá certifikát, bude znovu nahrané tooAzure.

### <a name="my-certificates-keep-disappearing"></a>Zachovat zmizení certifikáty
Dojde k recyklování hello instanci virtuálního počítače, všechny místní změny budou ztraceny. Použití [úloha spuštění](cloud-services-startup-tasks.md) tooinstall certifikáty toohello virtuální počítač spustí každou roli hello čas.

### <a name="i-cannot-find-my-management-certificates-in-hello-portal"></a>Nelze najít certifikáty pro správu portálu hello
[Certifikáty pro správu](../azure-api-management-certs.md) jsou dostupné jenom v hello portálu Azure Classic. aktuální portál Azure Hello nepoužívá certifikáty pro správu. 

### <a name="how-can-i-disable-a-management-certificate"></a>Jak můžete zakázat certifikát pro správu?
[Certifikáty pro správu](../azure-api-management-certs.md) nelze zakázat. Můžete je odstranit prostřednictvím portálu Azure Classic hello když nechcete, aby je toobe už používá.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Jak vytvořit certifikát protokolu SSL pro konkrétní IP adresu?
Postupujte podle pokynů hello v hello [vytvořit certifikát kurzu](cloud-services-certs-create.md). Použijte hello IP adresu jako hello název DNS.

## <a name="security"></a>Zabezpečení
### <a name="disable-ssl-30"></a>Protokol SSL 3.0 zakázat
toodisable SSL 3.0 a TLS zabezpečení pomocí vytvoření spuštění úloh, která je popsána v tomto příspěvku na blogu: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

### <a name="add-nosniff-tooyour-website"></a>Přidat **nosniff** tooyour webu
Klienti tooprevent z sledování toku dat hello typy MIME, přidejte nastavení vaší *web.config* souboru.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

To můžete také přidat jako nastavení ve službě IIS. Použití hello následující příkaz s hello [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a>Přizpůsobení služby IIS pro webovou roli
Použít skript spuštění služby IIS hello z hello [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.

## <a name="scaling"></a>Škálování
### <a name="i-cannot-scale-beyond-x-instances"></a>Nelze nastavit mimo X instancí
Vaše předplatné Azure může mít na hello počet jader, které můžete použít. Škálování nebude fungovat, pokud jste použili všechny jader hello k dispozici. Například pokud máte limit 100 jader, znamená to může mít 100 instancí virtuální počítač A1 s nastavenou velikostí pro cloudové služby, nebo velikosti 50 A2 instancí virtuálního počítače.

## <a name="networking"></a>Sítě
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I nelze rezervovat IP adresy v cloudové službě více virtuálních IP adres
Nejdřív zkontrolujte, zda je zapnutý tuto instanci hello virtuální počítač, který se pokoušíte tooreserve hello IP pro. Poté se ujistěte, že používáte vyhrazené IP adresy pro nepokoušejte hello pracovní a provozní nasazení. **Nechcete** změnit nastavení hello při nasazení hello je upgradu.

## <a name="remote-desktop"></a>Vzdálená plocha
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Jak mohu vzdálené plochy Pokud skupinu NSG?
Přidat toohello pravidla NSG, který povolí komunikaci na portech **3389** a **20000**.  Vzdálená plocha používá port **3389**.  Cloudové služby instance jsou vyrovnáváním, zatížení, takže nemůže přímo řídit které tooconnect instance k.  Hello *RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a povolit hello klienta toosend soubor cookie s RDP a zadejte jednotlivé instance tooconnect k.  Hello *RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000*** otevřít, což může být zablokován, pokud máte skupinu NSG.
