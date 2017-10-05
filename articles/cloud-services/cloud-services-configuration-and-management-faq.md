---
title: "Konfigurace a správy problémy pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy týkající se konfigurace a správa pro Microsoft Azure Cloud Services."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 42b5d2947df92b4486fe149d046168208083dde2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problémy se správou a konfigurace pro Azure Cloud Services: Časté otázky (FAQ)

Tento článek obsahuje nejčastější dotazy o konfiguraci a správě problémů pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Můžete také obrátit [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-add-nosniff-to-my-website"></a>Jak přidat "nosniff" my Web?
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

## <a name="how-do-i-customize-iis-for-a-web-role"></a>Jak přizpůsobit služby IIS pro webovou roli?
Použijte spouštěcí skript služby IIS z [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.

## <a name="i-cannot-scale-beyond-x-instances"></a>Nelze nastavit mimo X instancí
Vaše předplatné Azure limit je počet jader, které můžete použít. Škálování nebude fungovat, pokud jste použili všechna jádra, které jsou k dispozici. Například pokud máte limit 100 jader, znamená to může mít 100 instancí virtuální počítač A1 s nastavenou velikostí pro cloudové služby, nebo velikosti 50 A2 instancí virtuálního počítače.

## <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Jak můžete implementovat přístup na základě rolí pro cloudové služby?
Cloudové služby nepodporuje model řízení přístupu na základě Role (RBAC), protože se nejedná o službě Azure Resource Manager na základě.

V tématu [Azure RBAC oproti správci předplatného classic](../active-directory/role-based-access-control-what-is.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Jak nastavit časový limit nečinnosti pro vyrovnávání zatížení Azure?
Můžete zadat časový limit v souboru definice (csdef) služby takto:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
V tématu [nový: konfigurovat nečinnosti časový limit pro vyrovnávání zatížení Azure](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/) Další informace.

## <a name="can-microsoft-internal-engineers-rdp-to-cloud-service-instances-without-permission"></a>Můžete Microsoft interní technici protokolu RDP pro instance služby cloud bez oprávnění?
Microsoft vyplývá z vlastníka nebo jejich zmocněnci striktní proces, který nebude umožňují interní technikům RDP do cloudové služby bez písemného souhlasu (e-mailu nebo jiných napsané komunikace).

## <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Řetěz certifikátů my certifikátu SSL služby cloudu je nekompletní
Doporučujeme zákazníkům nainstalovat úplnou řetězu certifikátů (cert listu, certifikátů zprostředkující a kořenový certifikát) namísto právě certifikát typu list. Když instalujete pouze certifikát listu, byste tedy spoléhat na systému Windows se sestavit řetěz certifikátů rámci seznamu CTL. Dojde-li DNS problémy se sítí přerušované nebo v Azure nebo Windows Update při Windows k ověření certifikátu, certifikát může být považovány za neplatné. Řetěz certifikátů úplnou instalaci, lze předejít tomuto problému. Blog na [jak nainstalovat certifikát SSL a zřetězené](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) ukazuje, jak to udělat.

## <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Jak přiřadit statickou IP adresu do mé cloudové služby?
Pokud chcete nastavit statickou IP adresu, musíte vytvořit vyhrazenou IP adresu. Tato vyhrazená IP adresa může být přidružené k novou cloudovou službu nebo do stávajícího nasazení. Najdete v následujících dokumentech podrobnosti:
* [Jak vytvořit vyhrazenou IP adresu](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Rezervovat IP adresu existující cloudové služby](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Přidružení vyhrazené IP adresy na novou cloudovou službu](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Přidružení vyhrazené IP adresy na spuštěného nasazení](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Pomocí konfigurační soubor služby přidružit vyhrazenou IP adresu do cloudové služby](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

## <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Co je limit kvóty pro moje Cloudová služba?
V tématu [omezuje specifickou pro službu](../azure-subscription-service-limits.md#subscription-limits).

## <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Proč jednotku, na můj virtuální počítač cloudové služby zobrazit velmi málo volného místa na disku?
Toto chování je očekávané a jeho by nemělo způsobit jakýkoli problém do vaší aplikace. Pro % uproot % jednotky ve virtuálních počítačích PaaS Azure, která v podstatě využívá double množství místa, která obvykle zabírají soubory deníku zapnutý. Ale existuje několik věcí zajímat, v podstatě to změnit na jiný problém.

Velikost disku % approot % se počítá jako < .cspkg + velikost deníku maximální velikost > + okraj volného místa, nebo 1,5 GB, podle toho, která je větší. Velikost virtuálního počítače nemá žádný vliv na tohoto výpočtu. (Velikost virtuálního počítače pouze ovlivňuje velikost dočasné jednotce C:.) 

Ji není podporována pro zápis na disk % % approot. Pokud píšete do virtuálního počítače Azure, musíte jej v dočasných prostředků LocalStorage (nebo jiné možnosti, jako je například úložiště objektů Blob, soubory Azure, atd.). Množství volného místa ve složce % approot % tak není důležité. Pokud si nejste jistí, jestli vaše aplikace je zápis do jednotky % approot %, můžete je vždy nechat služby spustit několik dnů a potom porovnejte "před" a "po" velikosti. 

Azure nebude nic zapisovat do % jednotku % approot. Jakmile virtuální pevný disk je vytvořena z vaší .cspkg a připojené k virtuálnímu počítači Azure, jediné, co může zapisovat do této jednotky je vaše aplikace. 

Nastavení deníku jsou nekonfigurovatelnou, takže nelze ho vypnout.

## <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Jaké jsou funkce a možnosti, které poskytuje Azure základní IP adresy nebo ID a DDOS?
Azure má IP adresy nebo ID v datovém centru fyzické servery se chránit proti hrozbám. Kromě toho můžete zákazníkům nasadit řešení třetí strany zabezpečení, jako je například brány firewall webových aplikací, síťové brány firewall, proti malwaru, zjišťování neoprávněných vniknutí, prevence systémy (ID nebo IP adresy) a další. Další informace najdete v tématu [chránit vaše data a prostředky a v souladu s normami globálního zabezpečení](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft neustále monitoruje servery, sítě a aplikace k detekci hrozeb. Azure multipronged threat management přístup používá na detekci narušení, distributed denial of service (DDoS) prevence útoků, průnikům testování, chování analýza, detekce anomálií a strojového učení neustále posílit jeho obrany a snížení rizika. Antimalware od Microsoftu pro Azure chrání cloudových služeb Azure a virtuálních počítačů. Máte možnost nasadit řešení třetí strany zabezpečení kromě, jako je například webové aplikace ještě efektivněji bariéry, síťové brány firewall, proti malwaru, vniknutí odhalování a prevence systémy (ID nebo IP adresy) a další.

## <a name="why-does-iis-stop-writing-to-the-log-directory"></a>Proč služba IIS přestali zapisovat do adresáře protokolu?
Vyčerpáte místní úložiště kvóty pro zápis do adresáře protokolu. Chcete-li vyřešit tento problém, můžete provést jednu z následujících způsobů:
* Povolte diagnostiku pro službu IIS a mít k diagnostice pravidelně přesunout do úložiště objektů blob.
* Ručně odeberte soubory protokolu z adresáře protokolování.
* Zvýšit limit kvóty pro místní prostředky.

Další informace najdete v následujících dokumentech:
* [Ukládání a zobrazení diagnostických dat v Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
* [Protokoly služby IIS zastaví zápis v rámci cloudové služby](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>Co je účelem "Windows Azure šifrovací certifikát pro rozšíření nástrojů"?
Tyto certifikáty se automaticky vytvoří při každém přidání rozšíření do cloudové služby. Nejčastěji je to rozšíření WAD nebo rozšíření protokolu RDP, ale může to být jiné, jako je například rozšíření antimalwarových nebo kolektor protokolů. Tyto certifikáty se používají jenom pro šifrování a dešifrování privátní konfigurace rozšíření. Datum vypršení platnosti je nikdy zaškrtnutá políčka, takže není důležité, pokud platnost certifikátu. 

Tyto certifikáty, můžete ignorovat. Pokud chcete vyčistit certifikáty, můžete zkusit všechny jejich odstranění. Azure vyvolá chybu, pokud se pokusíte odstranit certifikát, který je používán.

## <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Jak můžu vygenerovat žádost (Podepsání certifikátu) bez "RDP-ing" v instanci?
Najdete v následujících dokumentech pokyny:

>[Získání certifikátu pro použití s Windows Azure webů (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

Upozorňujeme, že zástupce oddělení služeb je textový soubor. Nemá vytvořit z počítače, kde bude certifikát nakonec používat. I když tento dokument je napsán pro aplikační službu, je obecný vytvoření zástupce oddělení služeb zákazníkům a platí také pro cloudové služby.
