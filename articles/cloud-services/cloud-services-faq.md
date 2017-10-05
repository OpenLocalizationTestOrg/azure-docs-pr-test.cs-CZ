---
title: "Cloudové služby role Azure – nejčastější dotazy | Microsoft Docs"
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
ms.openlocfilehash: d887f3b31693c414254dc01dac4dbdd6d9224b6d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="cloud-services-faq"></a>Cloud Services – nejčastější dotazy
Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se služby Microsoft Azure Cloud. Další informace získáte [Azure podporují – nejčastější dotazy](http://go.microsoft.com/fwlink/?LinkID=185083) obecné Azure – ceny a podporu informace. Můžete také obrátit [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

## <a name="certificates"></a>Certifikáty
### <a name="where-should-i-install-my-certificate"></a>Kde nainstalujte tento certifikát?
* **Moje**  
  Aplikace certifikát s privátním klíčem (\*.pfx, \*.p12).
* **CERTIFIKAČNÍ AUTORITY**  
  Všechny zprostředkující certifikáty přejděte v tomto úložišti (zásady a Sub CAs).
* **KOŘENOVÉ**  
  Kořenové certifikační Autority uložte, aby vaše hlavní kořenový certifikát certifikační Autority by měl přejděte sem.

### <a name="i-cant-remove-expired-certificate"></a>Vypršela platnost certifikát nelze odebrat
Azure zabrání odebírání certifikátu, pokud se používá. Musíte buď odstraňte nasazení, které používá certifikát, nebo aktualizaci nasazení s jinou nebo obnovený certifikát.

### <a name="delete-an-expired-certificate"></a>Odstranit certifikát s vypršenou platností
Také certifikát není používán, můžete použít [odebrat AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) rutiny prostředí PowerShell odebrat certifikát.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>I vypršela platnost certifikátů s názvem Windows Azure Service Management pro rozšíření
Tyto certifikáty se vytvoří při každém přidání rozšíření do cloudové služby, jako je například rozšíření vzdálené plochy. Tyto certifikáty se používají jenom pro šifrování a dešifrování privátní konfigurace rozšíření. Pokud tyto certifikáty vyprší nezáleží. Datum vypršení platnosti není zaškrtnuto.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Zobrazují se certifikátů, které mají vymazání
Tyto zobrazují se pravděpodobně z důvodu nástroj, který používáte, jako je například Visual Studio. Vždy, když se znovu připojíte s nástroj, který používá certifikát, bude znovu nahrán do Azure.

### <a name="my-certificates-keep-disappearing"></a>Zachovat zmizení certifikáty
Dojde k recyklování instanci virtuálního počítače, všechny místní změny budou ztraceny. Použití [úloha spuštění](cloud-services-startup-tasks.md) instalace certifikátů k virtuálnímu počítači při každém spuštění role.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Certifikáty pro správu nelze najít na portálu
[Certifikáty pro správu](../azure-api-management-certs.md) jsou dostupné jenom v portálu Azure Classic. Na aktuálním portálu Azure nepoužívá certifikáty pro správu. 

### <a name="how-can-i-disable-a-management-certificate"></a>Jak můžete zakázat certifikát pro správu?
[Certifikáty pro správu](../azure-api-management-certs.md) nelze zakázat. Můžete je odstranit prostřednictvím portálu Azure Classic když nechcete, aby se už používá.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Jak vytvořit certifikát protokolu SSL pro konkrétní IP adresu?
Postupujte podle pokynů [vytvořit certifikát kurzu](cloud-services-certs-create.md). Použijte IP adresu jako název DNS.

## <a name="security"></a>Zabezpečení
### <a name="disable-ssl-30"></a>Protokol SSL 3.0 zakázat
Zákaz protokolu SSL 3.0 a TLS zabezpečení použít, vytvořte spuštění úloh, která je popsaná v tomto příspěvku na blogu: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

### <a name="add-nosniff-to-your-website"></a>Přidat **nosniff** na váš web
Pokud chcete klientům zabránit sledování toku dat typy MIME, přidejte nastavení ve vaší *web.config* souboru.

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

To můžete také přidat jako nastavení ve službě IIS. Použijte následující příkaz se [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a>Přizpůsobení služby IIS pro webovou roli
Použijte spouštěcí skript služby IIS z [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.

## <a name="scaling"></a>Škálování
### <a name="i-cannot-scale-beyond-x-instances"></a>Nelze nastavit mimo X instancí
Vaše předplatné Azure limit je počet jader, které můžete použít. Škálování nebude fungovat, pokud jste použili všechna jádra, které jsou k dispozici. Například pokud máte limit 100 jader, znamená to může mít 100 instancí virtuální počítač A1 s nastavenou velikostí pro cloudové služby, nebo velikosti 50 A2 instancí virtuálního počítače.

## <a name="networking"></a>Sítě
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I nelze rezervovat IP adresy v cloudové službě více virtuálních IP adres
Zkontrolujte, jestli je zapnutá instanci virtuálního počítače, který se pokoušíte rezervovat IP adresu pro. Poté se ujistěte, že používáte vyhrazené IP adresy pro nepokoušejte pracovní a provozní nasazení. **Nechcete** změnit nastavení při nasazení je upgradu.

## <a name="remote-desktop"></a>Vzdálená plocha
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Jak mohu vzdálené plochy Pokud skupinu NSG?
Přidat pravidla k této skupině, která povolí komunikaci na portech **3389** a **20000**.  Vzdálená plocha používá port **3389**.  Instance cloudové služby jsou Vyrovnávané, takže nemůže přímo řídit kterou instanci pro připojení k.  *RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a umožňují klientu odesílat soubor cookie s RDP a zadejte jednotlivé instance pro připojení k.  *RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000*** otevřít, což může být zablokován, pokud máte skupinu NSG.
