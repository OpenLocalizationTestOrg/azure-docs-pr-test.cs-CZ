---
title: "problémy se správou a aaaConfiguration pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy týkající se konfigurace a správa pro Microsoft Azure Cloud Services hello."
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
ms.openlocfilehash: 62ece142ac0ef5d45081cab333375b1a0a8f0ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problémy se správou a konfigurace pro Azure Cloud Services: Časté otázky (FAQ)

Tento článek obsahuje nejčastější dotazy o konfiguraci a správě problémů pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Můžete také obrátit hello [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-add-nosniff-toomy-website"></a>Jak přidat web toomy "nosniff"?
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

## <a name="how-do-i-customize-iis-for-a-web-role"></a>Jak přizpůsobit služby IIS pro webovou roli?
Použít skript spuštění služby IIS hello z hello [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.

## <a name="i-cannot-scale-beyond-x-instances"></a>Nelze nastavit mimo X instancí
Vaše předplatné Azure může mít na hello počet jader, které můžete použít. Škálování nebude fungovat, pokud jste použili všechny jader hello k dispozici. Například pokud máte limit 100 jader, znamená to může mít 100 instancí virtuální počítač A1 s nastavenou velikostí pro cloudové služby, nebo velikosti 50 A2 instancí virtuálního počítače.

## <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Jak můžete implementovat přístup na základě rolí pro cloudové služby?
Cloudové služby nepodporuje hello model řízení přístupu na základě Role (RBAC), protože se nejedná o službě Azure Resource Manager na základě.

V tématu [Azure RBAC oproti správci předplatného classic](../active-directory/role-based-access-control-what-is.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="how-do-i-set-hello-idle-timeout-for-azure-load-balancer"></a>Jak nastavit časový limit nečinnosti hello nástroje pro vyrovnávání zatížení Azure?
Můžete zadat časový limit hello v souboru definice (csdef) služby takto:

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

## <a name="can-microsoft-internal-engineers-rdp-toocloud-service-instances-without-permission"></a>Můžete Microsoft interní technici instance služby toocloud RDP bez oprávnění?
Microsoft způsobem, který striktní proces, který nebude povolovat interní pracovníkům tooRDP do cloudové služby bez písemného povolení (e-mailu nebo jiných napsané komunikace) z hello vlastníka nebo jejich zmocněnci.

## <a name="why-is-hello-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Řetěz certifikátů hello Moje certifikátu SSL služby cloud je nekompletní
Doporučujeme zákazníkům nainstalovat hello úplné řetěz certifikátů (cert listu, certifikátů zprostředkující a kořenový certifikát) namísto právě hello listu certifikátu. Když instalujete pouze hello listu certifikát, byste tedy spoléhat na řetěz certifikátů hello toobuild Windows rámci hello seznamu CTL. Dojde-li DNS problémy se sítí přerušované nebo v Azure nebo Windows Update při Windows je toovalidate hello certifikát, hello certifikát může být považovány za neplatné. Nainstalováním řetěz certifikátů úplné hello se vyhnout tomuto problému. Hello blog na [jak tooinstall zřetězené certifikát SSL](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) ukazuje, jak toodo to.

## <a name="how-do-i-associate-a-static-ip-address-toomy-cloud-service"></a>Jak přidružení statická IP adresa toomy Cloudová služba?
tooset si statickou IP adresu, musíte toocreate vyhrazenou IP adresu. Tato vyhrazená IP adresa může být přidružené tooa nové cloudové služby nebo tooan existující nasazení. V tématu hello následující dokumenty podrobnosti:
* [Jak toocreate vyhrazenou IP adresu](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Rezervujte hello IP adresu existující cloudové služby](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Přidružení vyhrazené IP tooa novou cloudovou službu](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Přidružení vyhrazené IP tooa běží nasazení](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Pomocí konfigurační soubor služby přidružení vyhrazené IP tooa cloudové služby](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

## <a name="what-is-hello-quota-limit-for-my-cloud-service"></a>Co je hello kvótu pro moje Cloudová služba?
V tématu [omezuje specifickou pro službu](../azure-subscription-service-limits.md#subscription-limits).

## <a name="why-does-hello-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Proč jednotku hello na můj virtuální počítač cloudové služby zobrazit velmi málo volného místa na disku?
Toto chování je očekávané a jeho by nemělo způsobit žádnou aplikaci tooyour problém. Pro hello % uproot % jednotky ve virtuálních počítačích PaaS Azure, která v podstatě využívá double hello množství místa, která obvykle zabírají soubory deníku zapnutý. Ale existuje několik věcí toobe vědět, která v podstatě změnit na jiný problém to.

Hello velikost disku % approot % počítá se jako < .cspkg + velikost deníku maximální velikost > + okraj volného místa, nebo 1,5 GB, podle toho, která je větší. Hello velikost virtuálního počítače nemá žádný vliv na tohoto výpočtu. (hello velikost virtuálního počítače se týká pouze hello velikost dočasné jednotce C: hello.) 

Je nepodporovaná toowrite toohello % approot % jednotky. Pokud píšete toohello virtuálního počítače Azure, musíte jej v dočasných prostředků LocalStorage (nebo jiné možnosti, jako je například úložiště objektů Blob, soubory Azure, atd.). Proto není smysluplný hello množství volného místa ve složce % approot % hello. Pokud si nejste jistí, jestli vaše aplikace je zápis jednotku toohello % approot %, můžete vždy nechat služby spustit několik dnů a potom porovnejte hello "před" a "po" velikosti. 

Azure nezapíše nic jednotku toohello % approot %. Jakmile hello virtuálního pevného disku je vytvořen z vaší .cspkg a připojit do hello virtuálního počítače Azure, hello jediné, co může zapsat toothis jednotky je vaše aplikace. 

Nastavení deníku Hello jsou nekonfigurovatelnou, proto nelze ho vypnout.

## <a name="what-are-hello-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Jaké jsou hello funkce a možnosti, které poskytuje Azure základní IP adresy nebo ID a DDOS?
Azure má IP adresy nebo ID v datovém centru fyzických serverů toodefend proti hrozbám. Kromě toho můžete zákazníkům nasadit řešení třetí strany zabezpečení, jako je například brány firewall webových aplikací, síťové brány firewall, proti malwaru, zjišťování neoprávněných vniknutí, prevence systémy (ID nebo IP adresy) a další. Další informace najdete v tématu [chránit vaše data a prostředky a v souladu s normami globálního zabezpečení](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft neustále monitoruje servery, sítě a aplikace toodetect hrozeb. Způsob multipronged threat správy Azure používá zjišťování neoprávněných vniknutí, distribuované denial-of-service (DDoS) útokům prevence, průnikům testování, analýzy chování, detekce anomálií a strojové učení tooconstantly posílení jeho obrany a snížení rizika. Antimalware od Microsoftu pro Azure chrání cloudových služeb Azure a virtuálních počítačů. Máte například webové aplikace ještě efektivněji bariéry, síťové brány firewall, proti malwaru, vniknutí odhalování a prevence systémy (ID nebo IP adresy) a další řešení třetí strany zabezpečení toodeploy možnost hello navíc.

## <a name="why-does-iis-stop-writing-toohello-log-directory"></a>Proč služba IIS ukončení psaní adresář protokolu toohello?
Vyčerpáte hello místní úložiště kvóty pro zápis toohello adresář protokolu. Chcete-li vyřešit tento problém, můžete provést jednu z následujících způsobů:
* Povolte diagnostiku pro službu IIS a mít hello diagnostiky pravidelně přesunout tooblob úložiště.
* Ručně odeberte soubory protokolu z hello adresář protokolování.
* Zvýšit limit kvóty pro místní prostředky.

Další informace najdete v tématu hello následující dokumenty:
* [Ukládání a zobrazení diagnostických dat v Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
* [Protokoly služby IIS zastaví zápis v rámci cloudové služby](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="what-is-hello-purpose-of-hello-windows-azure-tools-encryption-certificate-for-extensions"></a>Co je hello účel hello "Windows Azure šifrovací certifikát pro rozšíření nástrojů"?
Tyto certifikáty se automaticky vytvoří vždy, když rozšíření je přidány toohello cloudové služby. Nejčastěji je to hello WAD rozšíření nebo hello rozšíření protokolu RDP, ale jiné, může to být například hello antimalwarových nebo kolektor protokolů rozšíření. Tyto certifikáty se používají jenom pro šifrování a dešifrování hello privátní konfigurace rozšíření hello. Datum vypršení platnosti Hello je nikdy zaškrtnutá políčka, takže není důležité, pokud vypršela platnost certifikátu hello. 

Tyto certifikáty, můžete ignorovat. Pokud chcete vyčistit hello certifikáty, můžete zkusit všechny jejich odstranění. Azure bude vyvolá chybu, pokud se pokusíte toodelete certifikát, který je používán.

## <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-toohello-instance"></a>Jak můžete vygenerovat žádost (Podepsání certifikátu) bez "RDP-ing" v instanci toohello?
Najdete v následujících pokynech hello:

>[Získání certifikátu pro použití s Windows Azure webů (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

Upozorňujeme, že zástupce oddělení služeb je textový soubor. Nemá toobe vytvořeného z počítače hello kde hello se nakonec použije certifikát. I když tento dokument je napsán pro aplikační službu, je obecný hello CSR vytvoření a platí také pro cloudové služby.
