---
title: "aaaAzure virtuální sítě – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi toohello nejvíce nejčastější dotazy k Microsoft Azure virtual Network."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/18/2017
ms.author: jdial
ms.openlocfilehash: c2f9faee41b9c45e8bb196c58282d597ae732e54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Virtuální síť Azure nejčastější dotazy (FAQ)

## <a name="virtual-network-basics"></a>Základy virtuální sítě

### <a name="what-is-an-azure-virtual-network-vnet"></a>Co je virtuální síť Azure (VNet)?
Virtuální síť Azure (VNet) je reprezentace vlastní sítě v cloudu hello. Je to logická izolace cloudu Azure vyhrazeného tooyour předplatné hello. Můžete použít tooprovision virtuální sítě a spravovat virtuální privátní sítě (VPN) ve službě Azure a volitelně hello propojení virtuálních sítí jiných virtuálních sítí v Azure nebo s místní IT infrastrukturu toocreate hybridní nebo mezi různými místy řešení. Každý virtuální sítě, které vytvoříte má blokem CIDR a můžou být propojené tooother virtuální sítě a místní sítě, dokud se nepřekrývají bloků CIDR hello. Máte také kontrolu nad nastavení serveru DNS pro virtuální sítě a segmentaci hello virtuální sítě do podsítě.

Použijte virtuální sítě na:

* Vytvořte vyhrazené privátní cloudové virtuální síť někdy nevyžadují konfigurace mezi různými místy pro vaše řešení. Když vytvoříte virtuální síť, služeb a virtuálních počítačů v rámci virtuální sítě můžete přímo a bezpečně vzájemně komunikovat v cloudu hello. To zajišťuje provoz bezpečně v rámci hello virtuální sítě, ale stále umožňuje tooconfigure koncový bod připojení pro hello virtuální počítače a služby, které vyžadují internetovou komunikaci v rámci vašeho řešení.
* Bezpečně prodloužit datové centrum s virtuální sítě, můžete vytvořit tradiční site-to-site (S2S) VPN toosecurely škálování kapacitu vašeho datového centra. S2S VPN pomocí protokolu IPSEC tooprovide zabezpečeného spojení mezi vaší podnikové brány sítě VPN a Azure.
* Povolit hybridní cloudové scénáře, které poskytují virtuální sítě hello flexibilitu toosupport řadu hybridní cloudové scénáře. Typ tooany cloudové aplikace v místním systému, jako je například sálové počítače a systémů Unix můžete bezpečně připojit.

### <a name="how-do-i-know-if-i-need-a-vnet"></a>Jak poznám, kdy je třeba virtuální sítě?
Hello [Přehled virtuálních sítí](virtual-networks-overview.md) článek obsahuje tabulku rozhodnutí, která vám pomohou určit hello nejlepší sítě návrhu možnost za vás.

### <a name="how-do-i-get-started"></a>Jak mám začít?
Navštivte hello [dokumentaci virtuální sítě](https://docs.microsoft.com/azure/virtual-network/) tooget spuštěna. Tato část poskytuje přehled a nasazení informace pro všechny součásti hello virtuální sítě.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Můžete použít virtuální sítě bez připojení mezi různými místy?
Ano. Můžete vytvořit virtuální síť bez použití hybridní připojení. To je zvlášť užitečné, pokud chcete toorun řadiče domény Microsoft Windows Server Active Directory a farmy služby SharePoint v Azure.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Můžete provést optimalizace sítě WAN mezi virtuálními sítěmi nebo virtuální síť a Moje místní datové centrum?

Ano. Můžete nasadit [WAN optimalizace sítě virtuálního zařízení](https://azure.microsoft.com/marketplace/?term=wan+optimization) od více dodavatelů prostřednictvím hello Azure Marketplace.

## <a name="configuration"></a>Konfigurace

### <a name="what-tools-do-i-use-toocreate-a-vnet"></a>Co dělat, nástroje pro použití toocreate virtuální sítě?
Můžete použít následující nástroje toocreate hello nebo konfiguraci virtuální sítě:

* Portál Azure (pro classic a Resource Manager sítě Vnet).
* Soubor konfigurace sítě (netcfg - pro virtuální sítě classic pouze). V tématu hello [konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md) článku.
* Prostředí PowerShell (pro classic a Resource Manager sítě Vnet).
* Rozhraní příkazového řádku Azure (pro classic a Resource Manager sítě Vnet).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Jaké rozsahy adres můžete použít v mé virtuální sítě?
Všechny rozsah IP adres definované v [RFC 1918](http://tools.ietf.org/html/rfc1918). Například 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Může mít veřejné IP adresy v mé virtuální sítě?
Ano. Další informace o rozsahů veřejných IP adres najdete v tématu hello [adresní prostor veřejných IP adres ve virtuální síti](virtual-networks-public-ip-within-vnet.md) článku. Veřejné IP adresy nebudou přímo přístupné z Internetu hello.

### <a name="is-there-a-limit-toohello-number-of-subnets-in-my-vnet"></a>Existuje limit toohello, počet podsítí ve virtuální síti?
Ano. Čtení hello [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku. Adresní prostory podsítě se nesmí překrývat jednu na druhou.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Existují nějaká omezení na pomocí IP adresy v rámci těchto podsítí?
Ano. Azure si vyhrazuje některé IP adresy v rámci každé podsíti. Hello první a poslední IP adres podsítě hello jsou vyhrazené pro protokol shoda, společně s 3 Další adresy používané pro služby Azure.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Jak malý a jak velká může být virtuální sítě a podsítě?
Hello nejmenší podsítě, které podporujeme je/29 a hello největší podsíť/8 (pomocí definice podsítě CIDR).

### <a name="can-i-bring-my-vlans-tooazure-using-vnets"></a>Můžete zahrnout Moje sítí VLAN tooAzure pomocí virtuální sítě?
Ne. Virtuální sítě jsou překryvy vrstvy 3. Azure nepodporuje žádné sémantiku vrstvy 2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Můžete zadat vlastní zásady směrování na můj virtuální sítě a podsítě?
Ano. Můžete vytvořit uživateli definované směrování (UDR). Další informace o UDR najdete na adrese [trasy definované uživatelem a předávání IP](virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Podporují virtuální sítě vícesměrového vysílání nebo všesměrového vysílání?
Ne. Nepodporujeme vícesměrového vysílání nebo všesměrového vysílání.

### <a name="what-protocols-can-i-use-within-vnets"></a>Jaké protokoly můžete použít v rámci virtuální sítě?
Můžete použít protokoly TCP, UDP a ICMP TCP/IP v rámci virtuálních sítí. V rámci virtuální sítě jsou blokovány vícesměrového vysílání, všesměrové vysílání, IP-in-IP zapouzdřené pakety a pakety zapouzdření GRE (Generic Routing). 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Můžete příkaz ping Moje výchozí směrovačů v rámci virtuální sítě?
Ne.

### <a name="can-i-use-tracert-toodiagnose-connectivity"></a>Můžete použít tracert toodiagnose připojení?
Ne.

### <a name="can-i-add-subnets-after-hello-vnet-is-created"></a>Můžete přidat podsítě po hello, které se vytvoří virtuální síť?
Ano. Podsítě mohou být přidány tooVNets kdykoli, dokud adresa podsítě hello není součástí jinou podsítí v hello virtuální sítě.

### <a name="can-i-modify-hello-size-of-my-subnet-after-i-create-it"></a>Můžete upravit velikost hello mou podsíť, po jeho vytvoření?
Ano. Můžete přidat, odebrat, zvětšení nebo zmenšení podsíť, pokud nejsou žádné virtuální počítače nebo službám nasazeným v něm.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Můžete změnit po vytvoření je podsítě?
Ano. Můžete přidat, odebrat a upravit hello bloků CIDR používaných ve virtuální síti.

### <a name="can-i-connect-toohello-internet-if-i-am-running-my-services-in-a-vnet"></a>Můžete připojit toohello Internetu, pokud používám Moje služby ve virtuální síti?
Ano. Všechny služby nasazuje v rámci virtuální sítě můžete připojit toohello Internetu. Každé cloudové služby nasazené v Azure má veřejně adresovatelné VIP přiřazené tooit. Vstupní koncové body toodefine rolí PaaS a koncové body pro virtuální počítače tooenable bude mít tato připojení služby tooaccept z hello Internetu.

### <a name="do-vnets-support-ipv6"></a>Podporují virtuální sítě IPv6?
Ne. V tuto chvíli nelze použít protokol IPv6 s virtuální sítě.

### <a name="can-a-vnet-span-regions"></a>Virtuální síť, která může mít rozsah oblastí?
Ne. Virtuální síť, která je omezená tooa jedné oblasti.

### <a name="can-i-connect-a-vnet-tooanother-vnet-in-azure"></a>Je možné připojit virtuální síť tooanother virtuální sítě v Azure?
Ano. Můžete se připojit jednu virtuální síť tooanother virtuální sítě pomocí:
- Služby Azure VPN Gateway. Čtení hello [konfigurace připojení typu VNet-to-VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) článku. 
- Virtuální síť partnerský vztah. Čtení hello [VNet partnerského vztahu přehled](virtual-network-peering-overview.md) článku.

## <a name="name-resolution-dns"></a>Překlad adres (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Jaké jsou možnosti DNS pro virtuální sítě?
Použití hello rozhodovací tabulky na hello [překlad názvů pro virtuální počítače a instance rolí](virtual-networks-name-resolution-for-vms-and-role-instances.md) stránky tooguide všemi hello DNS možnosti k dispozici.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Můžete určit servery DNS pro virtuální síť?
Ano. V nastavení sítě VNet hello můžete zadat IP adresy serverů DNS. To se použijí jako servery DNS hello výchozí pro všechny virtuální počítače v hello virtuální sítě.

### <a name="how-many-dns-servers-can-i-specify"></a>Počet serverů DNS, které můžete zadat
Referenční dokumentace hello [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-hello-network"></a>Můžete upravit Moje servery DNS, po vytvoření hello sítě?
Ano. Kdykoli můžete změnit hello seznam serverů DNS pro vaši virtuální síť. Pokud změníte váš seznam serverů DNS, budete potřebovat toorestart hello virtuálních počítačů ve vaší virtuální síti, aby k nim toopick hello nový server DNS.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Co je Azure DNS a pracuje s virtuálními sítěmi?
Azure DNS je služba DNS víceklientské nabízených společností Microsoft. Azure zaregistruje všechny virtuální počítače a instance role cloudové služby v rámci této služby. Tato služba poskytuje překlad podle názvu hostitele pro virtuální počítače a instance rolí, které jsou obsažené v hello stejné cloudové služby a hello ve plně kvalifikovaný název domény pro virtuální počítače a instance rolí ve stejné virtuální síti. Čtení hello [překlad názvů pro virtuální počítače a instance rolí](virtual-networks-name-resolution-for-vms-and-role-instances.md) článku toolearn více informací o DNS.

> [!NOTE]
> Existuje omezení v tento čas toohello nejprve 100 cloudové služby ve virtuální síti mezi klienta překladu IP adres pomocí DNS poskytnutých platformou Azure. Pokud používáte serveru DNS, toto omezení neplatí.
> 
> 

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Můžete přepsat nastavení DNS na jednotlivé virtuální počítače / service základ?
Ano. Servery DNS můžete nastavit v nastavení toooverride hello výchozí sítě za cloudové služby zvlášť. Doporučujeme však používat co nejvíc celé sítě DNS.

### <a name="can-i-bring-my-own-dns-suffix"></a>Můžete zahrnout vlastní příponu DNS?
Ne. Nelze zadat vlastní přípony DNS pro vaše virtuální sítě.

## <a name="connecting-virtual-machines"></a>Propojování virtuálních počítačů

### <a name="can-i-deploy-vms-tooa-vnet"></a>Můžete nasadit virtuální počítače tooa virtuální síť?
Ano. Všechny tooa rozhraní (NIC) připojené sítě, kterou virtuální počítač nasazen pomocí modelu nasazení Resource Manager hello musí být připojené tooa virtuální sítě. Virtuální počítače nasazené prostřednictvím modelu nasazení classic hello může být volitelně tooa připojené virtuální sítě.

### <a name="what-are-hello-different-types-of-ip-addresses-i-can-assign-toovms"></a>Jaké jsou různé typy hello IP adres přiřadím tooVMs?
* **Privátní:** přiřazené tooeach síťový adaptér v rámci jednotlivých virtuálních počítačů. je přiřazená Hello adresa pomocí metody statickou nebo dynamickou hello. Privátní IP adresy přiřazené z hello rozsahu, který jste zadali v nastavení podsítě hello sítě vnet. Prostředky nasazené prostřednictvím modelu nasazení classic hello přiřazené soukromé IP adresy, i když nejsou připojeni tooa virtuální sítě. Privátní IP adresy přiřazeny hello dynamické metoda zůstane přiřazená tooa prostředků až do odstranění prostředků hello (virtuální počítače nebo Cloudová služba nasazovací sloty). Privátní IP adresy přiřazené pomocí metody dynamického hello může změnit při restartování virtuálního počítače, po které byly v hello zastavení (deallocated) stavu. Privátní IP adresy přiřazeny hello statickou metodu zůstane přiřazená tooa prostředků až do odstranění prostředků hello. Pokud potřebujete tooensure tuto hello privátní IP adresu pro prostředek nikdy změní až do odstranění hello prostředků, přiřadit privátní IP adresy s statickou metodu hello.
* **Veřejná:** volitelně přiřazené tooNICs připojené tooVMs nasazené prostřednictvím modelu nasazení Azure Resource Manager hello. Adresa Hello lze přiřadit pomocí metody statické nebo dynamické přidělování hello. Všechny virtuální počítače a cloudové služby instance rolí, které jsou nasazené prostřednictvím hello modelu nasazení classic existovat v rámci cloudové služby, který je přiřazen *dynamické*, veřejné virtuální adresy IP (VIP). Veřejné *statické* IP adresu, názvem [vyhrazená IP adresa](virtual-networks-reserved-public-ip.md), volitelně může být přiřazen jako virtuální IP adresu. Můžete přiřadit veřejné IP adresy tooindividual instance rolí virtuálních počítačů nebo cloudových služeb nasadit v rámci modelu nasazení classic hello. Tyto adresy se nazývají [Instance úrovně veřejnou IP adresu (splnění](virtual-networks-instance-level-public-ip.md) adresy a lze dynamicky přiřadit.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Můžete vyhradit privátní IP adresy pro virtuální počítač, který vytvořím později?
Ne. Nelze rezervovat privátní IP adresy. Pokud je k dispozici privátní IP adresy bude jí přiřazeno tooa virtuálního počítače nebo instanci role Server DHCP hello. Aby virtuální počítač může nebo nemusí být hello, který chcete hello privátní IP adresu toobe přiřazen. Můžete však změnit hello privátní IP adresa již vytvořené virtuální počítač tooany dostupnou privátní IP adresu.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Proveďte privátní měnit IP adresy pro virtuální počítače ve virtuální síti?
To záleží na okolnostech. Dynamické privátní IP adresy zůstat v případě virtuálních počítačů do jeho zastaveném (nepřiřazeném) nebo odstranit. Z virtuálního počítače nejsou vydané statickou privátní IP adresy, dokud nebude odstraněn.

### <a name="can-i-manually-assign-ip-addresses-toonics-within-hello-vm-operating-system"></a>Můžete I ručně přiřadit IP adresy tooNICs operačního systému hello virtuálního počítače?
Ano, ale není to doporučováno. Ruční změna hello IP adresu pro síťový adaptér v rámci operačního systému Virtuálního počítače může potenciálně vést toohello toolosing připojení virtuálních počítačů, kdyby toochange přiřazené tooa síťový adaptér v rámci hello virtuálního počítače Azure hello IP adresy.

### <a name="what-happens-toomy-ip-addresses-if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-hello-operating-system"></a>Co se stane toomy IP adresy je-li zabránit slot nasazení cloudové služby nebo vypnout virtuální počítač z hello operačního systému?
Nic. Hello IP adresy (veřejné VIP, veřejné a privátní) zůstat nasazovací slot. přiřazené toohello cloudové služby nebo virtuálního počítače. Dynamické adresy vydávají pouze pokud je virtuální počítač je zastavena (deallocated) nebo odstraněn, nebo slotu nasazení cloudové služby se odstraní. Kliknutím na hello **Zastavit** tlačítko pro virtuální počítač v rámci hello portál Azure nastaví její stav tooStopped (nepřiřazeném). Hello virtuálních počítačů v takovém případě dojde ke ztrátě jeho IP adresy.

### <a name="can-i-move-vms-from-one-subnet-tooanother-subnet-in-a-vnet-without-re-deploying"></a>Lze přesunout virtuální počítače z jedné podsítě tooanother podsíť ve virtuální síti bez opětovného nasazení?
Ano. Další informace najdete v hello [jak toomove virtuálního počítače nebo role instance jiné podsíti tooa](virtual-networks-move-vm-role-to-subnet.md) článku.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Můžete nakonfigurovat statickou adresu MAC pro virtuální počítač?
Ne. Adresa MAC se nedá nakonfigurovat staticky.

### <a name="will-hello-mac-address-remain-hello-same-for-my-vm-once-it-has-been-created"></a>Zůstane hello adresa MAC hello stejné pro virtuální počítač po jeho vytvoření?
Ano, hello adresa MAC zůstane hello, které stejné pro virtuální počítač nasadili pomocí modelu nasazení classic i hello Resource Manager, dokud nebude odstraněn. Hello adresa MAC dříve, byla vydána, pokud hello virtuálních počítačů byla zastavena (deallocated), ale nyní je adresa MAC hello zachovány i v případě, že hello virtuální počítač je v hello navrácena stavu.

### <a name="can-i-connect-toohello-internet-from-a-vm-in-a-vnet"></a>Můžete připojit toohello Internetu z virtuálního počítače ve virtuální síti?
Ano. Všechny virtuální počítače a cloudové služby instance rolí, které jsou nasazené v rámci virtuální sítě se může připojit toohello Internetu.

## <a name="azure-services-that-connect-toovnets"></a>Služby Azure, které se připojují tooVNets

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Můžete použít Azure App Service Web Apps s virtuální sítě?
Ano. Můžete nasadit webové aplikace uvnitř virtuální sítě pomocí App Service Environment (App Service Environment). Všechny webové aplikace můžete bezpečně připojit a přístup k prostředkům ve vaší virtuální síti Azure, pokud máte nakonfigurované pro virtuální síť připojení point-to-site. Další informace najdete v tématu hello následující články:

* [Vytvoření webové aplikace ve službě App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Integrace aplikace pomocí virtuální sítě Azure](../app-service-web/web-sites-integrate-with-vnet.md)
* [Integrace virtuální sítě a hybridních připojení pomocí webové aplikace](../app-service-web/web-sites-integrate-with-vnet.md#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Můžete nasadit cloudové služby s webových a pracovních rolí (PaaS) ve virtuální síti?
Ano. (Volitelně) můžete nasadit instance role cloudové služby v rámci virtuálních sítí. toodo Ano, zadejte název virtuální sítě hello a mapování hello role a podsítě v oddílu konfigurace sítě hello konfigurace služby. Není nutné tooupdate všechny vaše binárních souborů.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-tooa-vnet"></a>Můžete připojení nastavit škálování virtuálního počítače (VMSS) tooa virtuální síť?
Ano. Je nutné připojit VMSS tooa virtuální sítě.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Můžete přesunout Moje služby do aplikace a z virtuální sítě?
Ne. Nelze přesunout služby do aplikace a z virtuální sítě. Budete mít toodelete a znovu nasaďte hello služby toomove ho tooanother virtuální sítě.

## <a name="security"></a>Zabezpečení

### <a name="what-is-hello-security-model-for-vnets"></a>Co je hello model zabezpečení pro virtuální sítě?
Virtuální sítě jsou naprosto izolované od sebe navzájem a další služby hostované v hello infrastruktury Azure. Virtuální síť je hranice vztahu důvěryhodnosti.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-toovnet-connected-resources"></a>Můžete omezit příchozích a odchozích přenosů toku připojené tooVNet prostředky?
Ano. Můžete použít [skupin zabezpečení sítě](virtual-networks-nsg.md) tooindividual podsítí v rámci virtuální sítě, síťové adaptéry připojené tooa virtuální síť, nebo obojí.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Můžete implementovat brány firewall mezi prostředky připojené virtuální sítě?
Ano. Můžete nasadit [virtuální zařízení brány firewall sítě](https://azure.microsoft.com/en-us/marketplace/?term=firewall) od více dodavatelů prostřednictvím hello Azure Marketplace.

### <a name="is-there-information-available-about-securing-vnets"></a>Je k dispozici informace o zabezpečení virtuální sítě k dispozici?
Ano. V tématu hello [Přehled zabezpečení sítě Azure](../security/security-network-overview.md) článku.

## <a name="apis-schemas-and-tools"></a>Rozhraní API, schémat a nástroje

### <a name="can-i-manage-vnets-from-code"></a>Můžete spravovat virtuální sítě z kódu?
Ano. Rozhraní REST API pro virtuální sítě můžete použít v hello [Azure Resource Manager](https://msdn.microsoft.com/library/mt163658.aspx) a [classic (správou služeb)](http://go.microsoft.com/fwlink/?LinkId=296833)) modely nasazení.

### <a name="is-there-tooling-support-for-vnets"></a>Je k dispozici podpora nástrojů pro virtuální sítě?
Ano. Další informace o používání:
- Hello portálu toodeploy Azure virtuální sítě prostřednictvím hello [Azure Resource Manager](virtual-networks-create-vnet-arm-pportal.md) a [classic](virtual-networks-create-vnet-classic-pportal.md) modely nasazení.
- Prostředí PowerShell toomanage virtuálních sítí nasazené prostřednictvím hello [Resource Manager](/powershell/resourcemanager/azurerm.network/v3.1.0/azurerm.network.md) a [classic](/powershell/module/azure/?view=azuresmps-3.7.0) modely nasazení.
- Hello [rozhraní příkazového řádku Azure (CLI)](../virtual-machines/azure-cli-arm-commands.md#azure-network-commands-to-manage-network-resources) toomanage nasazené prostřednictvím obou modelů nasazení virtuální sítě.  
