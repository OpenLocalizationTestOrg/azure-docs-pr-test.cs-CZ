---
title: aaaActive Directory Federation Services v Azure | Microsoft Docs
description: "V tomto dokumentu se dozvíte, jak toodeploy AD FS v Azure pro vysoké dostupnosti probíhá."
keywords: "nasazení služby AD FS v azure, nasazení azure AD FS, azure AD FS, azure ad fs, nasazení služby AD FS, nasazení služby ad fs, služba AD FS v azure, nasazení služby AD FS v azure, nasazení služby AD FS v azure, azure AD FS, úvod tooAD služby FS, Azure, služby AD FS ve službě Azure iaas, služba AD FS, přesuňte tooazure služby AD FS"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2c39271f7569b9ce395dce2f53f5ba5a4897b132
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Nasazení služby AD FS (Active Directory Federation Service) v Azure
Služby AD FS nabízí zjednodušené možnosti zabezpečené federace identit a jednotného přihlašování na webu (SSO). Federace se službou Azure AD nebo O365 umožňuje uživatelům tooauthenticate pomocí místní přihlašovací údaje a přístup ke všem prostředkům v cloudu. V důsledku toho bude důležité tooensure infrastruktury toohave vysokou dostupnost služby AD FS přístup tooresources jak místně a v cloudu hello. Nasazení služby AD FS v Azure může pomoci dosáhnout vysoké dostupnosti hello požadované s minimálním úsilí.
Níže uvádíme některé z řady výhod, které nasazení služby AD FS v Azure přináší:

* **Vysoká dostupnost** -s výkonem hello sad dostupnosti Azure, zkontrolujte infrastrukturu vysoce dostupný.
* **Snadno tooScale** – potřebovat další výkonu? Efektivní počítače toomore snadno migrujte pomocí několika kliknutí v Azure
* **Mezi geografická redundance** – s můžete si být jistí, že vaše infrastruktura je vysoce dostupný v celém světě hello s Azure geografickou redundancí
* **Snadno tooManage** – s možnostmi vysoce zjednodušenou správu na portálu Azure, Správa infrastruktuře je velmi snadné a bezproblémové 

## <a name="design-principles"></a>Principy návrhu
![Návrh nasazení](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Hello diagramu výše zobrazuje hello doporučená toostart základní topologie nasazení infrastruktury služby AD FS v Azure. Principy Hello za hello různé součásti hello topologie jsou uvedeny níže:

* **Řadič domény a servery služby AD FS**: Pokud máte méně než 1000 uživatelů, můžete roli služby AD FS jednoduše nainstalovat na řadiče domény. Pokud nechcete, aby žádný vliv na výkon na řadičích domény hello nebo pokud máte více než 1 000 uživatelů, pak nasazení služby AD FS na samostatných serverech.
* **WAP Server** – je nezbytné toodeploy Proxy serverech webových aplikací, aby uživatelé dosáhnout hello AD FS, když nejsou v síti společnosti hello také.
* **DMZ**: hello Proxy serverech webových aplikací bude uložena v umístění hello DMZ a mezi hello DMZ a hello interní podsíť je povolen přístup pouze TCP/443.
* **Nástroje pro vyrovnávání zatížení**: tooensure vysokou dostupnost serverů služby AD FS a Proxy webových aplikací, doporučujeme používat interní nástroj pro servery služby AD FS a vyrovnávání zatížení Azure pro servery služby Proxy webových aplikací.
* **Skupiny dostupnosti**: tooprovide redundance tooyour AD FS nasazení, doporučujeme seskupit dva nebo více virtuálních počítačů do skupiny dostupnosti pro podobné úlohy. Tato konfigurace zajišťuje, aby během plánované nebo neplánované události údržby zůstal dostupný alespoň jeden virtuální počítač.
* **Účty úložiště**: se doporučuje toohave dva účty úložiště. S účet jednoho úložiště může vést toocreation jediný bod selhání a může způsobit hello nasazení toobecome není k dispozici ve scénáři s nepravděpodobné, kde se účet úložiště hello ocitne mimo provoz. Když budete mít dva účty úložiště, můžete ke každé chybové linii přidružit jeden účet úložiště.
* **Oddělení sítí**: Proxy servery webových aplikací musí být nasazené v samostatné síti DMZ. Můžete rozdělit na dvě podsítě jednu virtuální síť a pak nasadit servery služby Proxy webových aplikací hello v izolované podsíti. Jednoduše můžete nakonfigurovat nastavení skupiny zabezpečení sítě hello pro každou podsíť a povolit pouze požadované komunikaci mezi dvěma podsítěmi hello. Další podrobnosti jsou popsány v níže uvedeném scénáři nasazení.

## <a name="steps-toodeploy-ad-fs-in-azure"></a>Kroky toodeploy služby AD FS v Azure
Hello kroků v této části outline hello Průvodce toodeploy hello níže použité v ukázkách infrastruktury služby AD FS v Azure.

### <a name="1-deploying-hello-network"></a>1. Nasazení sítě hello
Jak je uvedeno výše, můžete buď vytvořit dvě podsítě v jedné virtuální síti, nebo můžete vytvořit dvě zcela odlišné virtuální sítě (VNet). Tento článek se zaměří na nasazení jedné virtuální sítě a její rozdělení na dvě podsítě. Toto je momentálně snadnější přístup jako dva samostatné virtuální sítě by vyžadovaly bránu VNet tooVNet pro komunikaci.

**1.1 Vytvoření virtuální sítě**

![Vytvoření virtuální sítě](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)

V hello portálu Azure můžete nasadit vyberte virtuální síť a jste hello virtuální sítě a jednu podsíť okamžitě s jedním kliknutím. INT podsíť je rovněž definovaný a je nyní připraven pro virtuální počítače toobe přidat.
dalším krokem Hello je tooadd jinou podsíť toohello sítí, tj hello DMZ podsítě. toocreate hello podsíti DMZ, jednoduše

* Vyberte síť, nově vytvořený hello
* Ve vlastnostech hello vyberte podsíť
* V podsíti hello panelu klikněte na hello tlačítko Přidat
* Zadejte hello podsíť název a adresu místa informace toocreate hello podsítě

![Podsíť](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)

![Podsíť DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. Vytváření skupin zabezpečení sítě hello**

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odpírají síťový provoz tooyour instance virtuálních počítačů ve virtuální síti. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL hello platí tooall hello instance virtuálních počítačů v této podsíti.
Za účelem hello tyto pokyny, vytvoříme dvě skupiny Nsg: jeden pro interní síť a zónou DMZ. Budou označeny NSG_INT a NSG_DMZ.

![Vytvoření skupiny zabezpečení sítě](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Po hello je vytvořena skupina NSG bude 0 příchozí a 0 odchozí pravidla. Jakmile jsou hello role na příslušných serverech hello nainstalovaná a funkční, pak hello příchozí a odchozí pravidla můžete provést podle potřeby toohello úroveň zabezpečení.

![Inicializace skupiny zabezpečení sítě](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Po vytvoření skupiny Nsg hello přidružení podsítě NSG_INT INT a NSG_DMZ s podsítí hraniční sítě. Níže je uvedený snímek obrazovky s příkladem:

![Konfigurace skupiny zabezpečení sítě](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Klikněte na panel hello tooopen podsítě pro podsítě
* Vyberte podsíť tooassociate hello s hello NSG 

Po konfiguraci hello panel pro podsítě by měl vypadat jako následující:

![Panel Podsítě po přidružení ke skupinám zabezpečení sítě](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. Vytvoření tooon místní připojení**

Potřebujeme tooon místních připojení v pořadí toodeploy hello řadiče domény (DC) v azure. Azure nabízí různé možnosti tooconnect připojení vaší místní infrastruktury tooyour infrastruktury Azure.

* Point-to-Site
* Virtuální síť s připojením typu Site-to-Site
* ExpressRoute

Doporučujeme toouse ExpressRoute. ExpressRoute vám umožňuje vytvářet privátní připojení mezi datovými centry Azure a infrastrukturou, která se nachází ve vašem umístění nebo v prostředí ve společném umístění. Připojení ExpressRoute se nepřenášejí prostřednictvím hello veřejného Internetu. Nabízí další spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než Typická připojení přes hello Internet.
Když se doporučuje toouse ExpressRoute, můžete zvolit libovolnou metodu připojení pro vaši organizaci nejvhodnější. Další informace o ExpressRoute a hello toolearn číst různé možnosti připojení pomocí služby ExpressRoute, [technický přehled ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Vytvoření účtů úložiště
V pořadí toomaintain vysokou dostupnost a vyhnout závislost na účet jedno úložiště, můžete vytvořit dva účty úložiště. Rozdělení hello počítačů v každé sadě dostupnosti do dvou skupin a potom přiřadit každé skupiny účet samostatného úložiště.

![Vytvoření účtů úložiště](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Vytvoření skupin dostupnosti
Pro každou roli (řadiče domény nebo AD FS připojeném protokolem WAP) vytvořte skupiny dostupnosti, které budou obsahovat 2 počítače každý hello minimální. Tím dosáhnete vyšší dostupnost pro každou roli. Při vytváření dostupnosti hello nastaví, je nezbytné toodecide na hello následující:

* **Poruch domén**: hello virtuálních počítačů v doméně se stejným selhání sdílejí hello stejný zdroj energie a fyzický síťový přepínač. Jako minimum doporučujeme dvě domény selhání. Hello výchozí hodnota je 3 a můžete nechat ji jako je pro hello účel tohoto nasazení
* **Aktualizovat domén**: počítače, které patří toohello jedné aktualizační doméně se během aktualizace restartují společně. Chcete toohave minimálně 2 aktualizaci domény. Hello výchozí hodnota je 5 a můžete nechat ji jako je pro hello účel tohoto nasazení

![Skupiny dostupnosti](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Vytvořte následující skupiny dostupnosti hello

| Skupina dostupnosti | Role | Domény selhání | Aktualizační domény |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Nasazení virtuálních počítačů
dalším krokem Hello je toodeploy virtuálních počítačů, které budou hostiteli hello různé role ve vaší infrastruktuře. Jako minimum doporučujeme dva počítače v každé skupině dostupnosti. Vytvořte čtyři virtuální počítače pro základní nasazení hello.

| Počítač | Role | Podsíť | Skupina dostupnosti | Účet úložiště | IP adresa |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Statická |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Statická |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Statická |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Statická |

Pravděpodobně jste si všimli, že jsme nezadali žádnou skupinu zabezpečení sítě. Je to proto, že azure umožňuje použít NSG na úrovni podsítě hello. Potom můžete řídit síťový provoz počítače pomocí hello jednotlivých NSG přidruženého buď hello podsíť, jinak se objekt síťové karty hello. Další informace si můžete přečíst v článku [Skupina zabezpečení sítě (NSG)](https://aka.ms/Azure/NSG).
Statická IP adresa se doporučuje, když spravujete hello DNS. Můžete použít Azure DNS a místo toho v hello záznamy DNS pro doménu, označují toohello nové počítače kvalifikovanými jejich Azure.
Vaše podokně virtuální počítač by měl vypadat jako níže po dokončení nasazení hello:

![Nasazené virtuální počítače](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-hello-domain-controller--ad-fs-servers"></a>5. Konfigurace hello řadič domény nebo servery služby AD FS
 V pořadí tooauthenticate budou potřebovat všechny příchozí žádosti o služby AD FS řadič domény toocontact hello. toosave hello nákladná cesty od Azure tooon místní řadič domény pro ověřování, je doporučeno toodeploy repliku řadiče domény hello v Azure. V pořadí tooattain vysokou dostupnost doporučujeme toocreate řadičů domény 2 alespoň sadu dostupnosti.

| Řadič domény | Role | Účet úložiště |
|:---:|:---:|:---:|
| contosodc1 |Replika |contososac1 |
| contosodc2 |Replika |contososac2 |

* Povýšit hello dva servery repliky řadičů domény s DNS
* Konfigurace serverů hello služby AD FS instalací role hello služby AD FS pomocí Správce serveru hello.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Nasazení interního nástroje pro vyrovnávání zatížení (ILB)
**6.1. Vytvoření hello ILB**

toodeploy ILB, vyberte Vyrovnávání zatížení v hello portál Azure a kliknutím na tlačítko Přidat (+).

> [!NOTE]
> Pokud se nezobrazí **nástroje pro vyrovnávání zatížení** v nabídce, klikněte na tlačítko **Procházet** v hello snížit nalevo od hello portál a přejděte do části **nástroje pro vyrovnávání zatížení**.  Pak klikněte na hvězdičky tooadd hello žlutý ho tooyour nabídky. Nyní vyberte hello novou konfiguraci služby Vyrovnávání zatížení ikonu tooopen hello panely toobegin nástroje pro vyrovnávání zatížení hello.
> 
> 

![Vyhledání nástroje pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Název**: poskytnout všech Vyrovnávání zatížení toohello vhodný název
* **Schéma**: vzhledem k tomu, že tato služba Vyrovnávání zatížení bude umístěna před hello služby AD FS servery a je určen pro připojení k interní síti, vyberte "Interní"
* **Virtuální síť**: Zvolte hello virtuální sítě, který nasazujete službě AD FS
* **Podsíť**: Zvolte hello interní podsíť
* **Přiřazení IP adresy**: Statické.

![Interní nástroj pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)

Po kliknutí na tlačítko Vytvořit a je nasazena hello ILB, byste měli vidět v seznamu hello nástrojů pro vyrovnávání zatížení:

![Nástroje pro vyrovnávání zatížení, po nasazení interního nástroje pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)

Dalším krokem je tooconfigure hello fond back-end a back-end kontroly hello.

**6.2. Konfigurace back-endového fondu interního nástroje pro vyrovnávání zatížení**

Vyberte hello nově vytvořený ILB panelu hello nástroje pro vyrovnávání zatížení. Otevře se panel nastavení hello. 

1. Vyberte back-endové fondy z panelu nastavení hello
2. V hello přidat panel fond back-end, kliknutím na tlačítko Přidat virtuální počítač
3. Zobrazí se panel, na kterém můžete vybrat skupinu dostupnosti.
4. Vyberte sadu dostupnosti hello služby AD FS

![Konfigurace back-endového fondu interního nástroje pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)

**6.3. Konfigurace testu**

V panelu nastavení ILB hello vyberte sondy.

1. Klikněte na Přidat.
2. Zadejte podrobnosti testu. a. **Název**: Název testu. b. **Protokol**: TCP. c. **Port**: 443 (HTTPS). d. **Interval**: 5 (výchozí hodnota) – Toto je hello interval, ve kterém bude ILB testu hello počítače ve fondu back-end hello e. **Prahová hodnota špatného stavu limit**: 2 (výchozí val ue) – Toto je po sobě jdoucích test selhání, po které bude ILB deklarovat na počítač v hello back-end fondu přestane reagovat a zastavení odesílání přenosů tooit hello prahovou hodnotu.

![Konfigurace testu interního nástroje pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)

**6.4. Vytvoření pravidel vyrovnávání zatížení**

V pořadí tooeffectively vyrovnávání hello přenosů by měly být nakonfigurované hello ILB pravidla Vyrovnávání zatížení. V pořadí toocreate pravidlo Vyrovnávání zatížení, 

1. Vyberte z panelu nastavení hello hello ILB pravidlo Vyrovnávání zatížení
2. Klikněte na Přidat v hello panely pravidlo Vyrovnávání zatížení
3. V hello přidat načíst vyrovnávání panely pravidlo. **Název**: Zadejte název pravidla hello b. **Protokol**: Vyberte TCP. c. **Port**: 443. d. **Back-endový port**: 443. e. **Fond back-end**: vyberte fond hello jste vytvořili pro hello služby AD FS clusteru starší f. **Test**: Test vyberte hello předtím vytvořili pro servery služby AD FS

![Konfigurace pravidel interního nástroje pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. Aktualizace DNS pomocí interního nástroje pro vyrovnávání zatížení**

Přejděte tooyour DNS server a vytvořit záznam CNAME pro hello ILB. Hello CNAME musí být pro službu federation hello s IP adresou hello odkazující hello ILB toohello IP adresu. Například pokud hello ILB DIP adresu 10.3.0.8 a služba hello federation service je fs.contoso.com, pak vytvořte záznam CNAME pro fs.contoso.com odkazující too10.3.0.8.
Tím bude zajištěno, že všechny komunikace týkající se konec fs.contoso.com až na hello ILB a jsou správně směrované.

### <a name="7-configuring-hello-web-application-proxy-server"></a>7. Konfigurace serveru služby Proxy webových aplikací hello
**7.1. Konfigurace hello Proxy serverech webových aplikací servery tooreach AD FS**

V pořadí tooensure, zda jsou servery Proxy webových aplikací mít tooreach hello služby AD FS servery za hello ILB vytvořte záznam v hello %systemroot%\system32\drivers\etc\hosts pro hello ILB. Všimněte si, že hello rozlišující název (DN) musí být název federační služby hello, třeba fs.contoso.com. A hello IP adresu by měl být u hello ILB IP adresu (10.3.0.8 jako hello příkladu).

**7.2. Instalace role Proxy webových aplikací hello**

Po zajištění, že jsou servery Proxy webových aplikací mít tooreach hello služby AD FS servery za ILB, můžete nainstalovat další servery Proxy webových aplikací hello. Webové servery služby Proxy aplikace není třeba toohello připojené k doméně. Nainstalujte role Proxy webových aplikací hello na dvou serverech Proxy webových aplikací hello výběrem hello role vzdáleného přístupu. Správce serveru Hello Průvodce toocomplete hello WAP instalace.
Další informace o tom, přečtěte si toodeploy WAP, [instalaci a konfiguraci Proxy serveru webové aplikace hello](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-hello-internet-facing-public-load-balancer"></a>8.  Nasazení hello Internet Facing (veřejných) pro vyrovnávání zatížení
**8.1.  Vytvoření internetového (veřejného) nástroje pro vyrovnávání zatížení**

V hello portálu Azure vyberte nástroje pro vyrovnávání zatížení a potom klikněte na Přidat. V panelu Nástroje pro vyrovnávání zatížení hello vytvořit zadejte následující informace hello

1. **Název**: název pro vyrovnávání zatížení hello
2. **Schéma**: Veřejné – tato možnost informuje Azure, že nástroj pro vyrovnávání zatížení bude potřebovat veřejnou adresu.
3. **IP adresa**: Vytvořte novou IP adresu (dynamickou).

![Internetový nástroj pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Po nasazení nástroje pro vyrovnávání zatížení hello objeví v seznamu nástroje pro vyrovnávání zatížení hello.

![Seznam nástrojů pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)

**8.2. Přiřadit DNS popisek toohello veřejnou IP adresu**

Klikněte na položku Nástroje pro vyrovnávání zatížení hello nově vytvořený v hello zatížení nástroje pro vyrovnávání panely toobring až hello panelů pro konfiguraci. Postupujte podle níže popisek DNS hello tooconfigure kroky pro veřejnou IP adresu hello:

1. Klikněte na hello veřejnou IP adresu. Otevře se panel hello pro hello veřejnou IP adresu a nastavení
2. Klikněte na Konfigurace.
3. Zadejte název DNS. To se stane hello veřejný název DNS, může přistupovat z libovolného místa, například contosofs.westus.cloudapp.azure.com. Můžete přidat položku v hello externí službu DNS pro hello federační služby (např. fs.contoso.com), která přeloží popisek DNS toohello hello externí službu Vyrovnávání zatížení (contosofs.westus.cloudapp.azure.com).

![Konfigurace internetového nástroje pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Konfigurace internetového nástroje pro vyrovnávání zatížení (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. Konfigurace back-endového fondu internetového (veřejného) nástroje pro vyrovnávání zatížení** 

Postupujte podle hello stejné kroky jako vytváření Vyrovnávání zatížení pro vnitřní hello tooconfigure hello back-endový fond pro Internet Facing (veřejných) nástroj pro vyrovnávání zatížení jako hello dostupnosti nastavit pro serverech WAP hello. Například contosowapset.

![Konfigurace back-endového fondu internetového nástroje pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)

**8.4. Konfigurace testu**

Postupujte podle stejné kroky jako konfigurace hello hello sondu tooconfigure nástroje pro vyrovnávání zatížení interní pro hello fond back-end serverech WAP hello.

![Konfigurace testu internetového nástroje pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)

**8.5. Vytvoření pravidel vyrovnávání zatížení**

Postupujte podle kroků stejná jako Vyrovnávání zatížení pro hello ILB tooconfigure pravidla pro TCP 443 hello.

![Konfigurace pravidel vyrovnávání pro internetový nástroj pro vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)

### <a name="9-securing-hello-network"></a>9. Zabezpečení sítě hello
**9.1. Zabezpečení interní podsíť hello**

Celkově platí budete potřebovat hello následující pravidla tooefficiently zabezpečit vaše interní podsíť (v pořadí hello, jak je uvedeno níže)

| Pravidlo | Popis | Tok |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Povolit komunikaci pomocí protokolu HTTPS hello z hraniční sítě |Příchozí |
| DenyInternetOutbound |Žádný přístup toointernet |Odchozí |

![pravidla přístupu INT (příchozí)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[komentář]: <> (![pravidla přístupu INT (příchozí)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [komentář]: <> (![pravidla přístupu INT (odchozí)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))

**9.2. Zabezpečení podsíti DMZ hello**

| Pravidlo | Popis | Tok |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Povolit HTTPS z Internetu toohello hraniční sítě |Příchozí |
| DenyInternetOutbound |Jiný než HTTPS toointernet je blokovaný |Odchozí |

![pravidla přístupu EXT (příchozí)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[komentář]: <> (![pravidla přístupu EXT (příchozí)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [komentář]: <> (![pravidla přístupu EXT (odchozí)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

> [!NOTE]
> Pokud je požadováno ověření klienta uživatelským certifikátem (ověřování klienta protokolem TLS pomocí uživatelských certifikátů X509), potom služba AD FS vyžaduje, aby port 49443 TCP měl povolený příchozí přístup.
> 
> 

### <a name="10-test-hello-ad-fs-sign-in"></a>10. Testování přihlášení hello služby AD FS
Hello nejjednodušší způsob je tootest, které se služba AD FS pomocí hello IdpInitiatedSignon.aspx stránky. V pořadí toobe toodo možné, že je požadovaná tooenable hello IdpInitiatedSignOn na vlastnosti hello služby AD FS. Postupujte podle kroků hello tooverify vaší instalace služby AD FS

1. Spustit hello níže na server hello AD FS, pomocí prostředí PowerShell, tooset rutinu ho tooenabled.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. Z jakékoli externího počítače zobrazte https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3. Měli byste vidět stránku hello služby AD FS, jako níže:

![Přihlašovací stránka testu](./media/active-directory-aadconnect-azure-adfs/test1.png)

Po úspěšném přihlášení zobrazí zprávu o úspěchu, jak je uvedeno níže:

![Úspěch testu](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Šablona pro nasazení AD FS v Azure
Šablona Hello nasadí instalace 6 počítači s 2 pro řadiče domény, služba AD FS a WAP.

[Šablona pro nasazení AD FS v Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Při nasazování této šablony můžete použít stávající virtuální síť nebo vytvořit novou. Hello různé parametry, které jsou k dispozici pro přizpůsobení hello nasazení jsou uvedeny níže s hello popis použití parametru hello v procesu nasazení hello. 

| Parametr | Popis |
|:--- |:--- |
| Umístění |Hello oblast toodeploy hello prostředky do, například východní USA. |
| StorageAccountType |Typ Hello hello vytvořený účet úložiště |
| VirtualNetworkUsage |Označuje, zda se vytvoří nová virtuální síť nebo se použije již existující. |
| VirtualNetworkName |Název Hello tooCreate hello virtuální sítě, povinná na využití existující nebo nové virtuální sítě |
| VirtualNetworkResourceGroupName |Určuje název skupiny prostředků hello, kde se nachází existující virtuální síť hello hello. Při použití existující virtuální síť, to všechno bude povinný parametr, hello nasazení můžete zjistit ID hello hello existující virtuální síť |
| VirtualNetworkAddressRange |rozsah adres hello Hello nové sítě VNET, povinné, pokud se vytváří nové virtuální sítě |
| InternalSubnetName |Název Hello hello interní podsíť, povinná na obě možnosti využití virtuální sítě (nový nebo existující) |
| InternalSubnetAddressRange |rozsah adres Hello hello interní podsíť, která obsahuje hello řadičů domény a služby AD FS servery, povinné, pokud vytvoření nové virtuální sítě. |
| DMZSubnetAddressRange |rozsah adres Hello hello dmz podsítě, který obsahuje hello Windows aplikace proxy servery, povinné, pokud se vytváří nové virtuální sítě. |
| DMZSubnetName |Název Hello hello interní podsíť, povinná na obě možnosti využití virtuální sítě (nový nebo existující). |
| ADDC01NICIPAddress |hello Hello interní IP adresu z prvního řadiče domény, tuto IP adresu budou staticky přiřazeny toohello řadič domény a musí být platná ip adresa v podsíti interní hello |
| ADDC02NICIPAddress |Hello interní IP adresu hello druhého řadiče domény, tuto IP adresu budou staticky přiřazeny toohello řadič domény a musí být platná ip adresa v podsíti interní hello |
| ADFS01NICIPAddress |Hello interní IP adresu serveru služby AD FS první hello, tuto IP adresu budou staticky přiřazeny serveru služby AD FS toohello a musí být platná ip adresa v podsíti interní hello |
| ADFS02NICIPAddress |Hello interní IP adresu serveru služby AD FS druhý hello, tuto IP adresu budou staticky přiřazeny serveru služby AD FS toohello a musí být platná ip adresa v podsíti interní hello |
| WAP01NICIPAddress |Hello interní IP adresu prvního serveru WAP hello, tuto IP adresu budou staticky přiřazeny serveru WAP toohello a musí být platná ip adresa v podsíti DMZ hello |
| WAP02NICIPAddress |Hello interní IP adresu hello druhý server WAP, tuto IP adresu budou staticky přiřazeny serveru WAP toohello a musí být platná ip adresa v podsíti DMZ hello |
| ADFSLoadBalancerPrivateIPAddress |Nástroj pro vyrovnávání zatížení Hello interní IP adresu hello služby AD FS, tuto IP adresu budou staticky přiřazeny toohello nástroj pro vyrovnávání zatížení a musí být platná ip adresa v podsíti interní hello |
| ADDCVMNamePrefix |Prefix názvu virtuálního počítače pro řadiče domény |
| ADFSVMNamePrefix |Prefix názvu virtuálního počítače pro servery ADFS |
| WAPVMNamePrefix |Prefix názvu virtuálního počítače pro servery WAP |
| ADDCVMSize |velikost virtuálního počítače Hello hello řadiče domény |
| ADFSVMSize |velikost virtuálního počítače Hello hello serverů služby AD FS |
| WAPVMSize |velikost virtuálního počítače Hello hello WAP serverů |
| AdminUserName |Název Hello hello místního správce hello virtuálních počítačů |
| AdminPassword |Hello heslo pro účet místního správce hello hello virtuálních počítačů |

## <a name="additional-resources"></a>Další zdroje
* [Skupiny dostupnosti](https://aka.ms/Azure/Availability) 
* [Nástroj pro vyrovnávání zatížení Azure](https://aka.ms/Azure/ILB)
* [Interní nástroj pro vyrovnávání zatížení.](https://aka.ms/Azure/ILB/Internal)
* [Internetový nástroj pro vyrovnávání zatížení](https://aka.ms/Azure/ILB/Internet)
* [Účty úložiště](https://aka.ms/Azure/Storage)
* [Virtuální sítě Azure](https://aka.ms/Azure/VNet)
* [Služba AD FS a odkazy na proxy webové aplikace](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Další kroky
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
* [Konfigurace a správa služby AD FS pomocí služby Azure AD Connect](active-directory-aadconnectfed-whatis.md)
* [Vysoká dostupnost mezi geografickými nasazeními služby AD FS v Azure pomocí Azure Traffic Manageru](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)

