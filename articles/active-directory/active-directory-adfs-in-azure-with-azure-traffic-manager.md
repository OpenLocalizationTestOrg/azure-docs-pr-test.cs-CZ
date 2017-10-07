---
title: "aaaHigh dostupnosti geografické mezi AD FS nasazení v Azure s Azure Traffic Manageru | Microsoft Docs"
description: "V tomto dokumentu se dozvíte, jak toodeploy AD FS v Azure pro vysoké dostupnosti probíhá."
keywords: "Služba AD fs pomocí Azure traffic manager, služba AD FS s Azure Traffic Manager, geografické, více datacenter, geografické datových center, více geografické datovými centry, nasazení služby AD FS v azure, nasazení azure AD FS, azure AD FS, azure ad fs, nasazení služby AD FS, nasazení služby ad fs, služba AD FS ve službě azure nasazení služby AD FS v azure, nasazení služby AD FS v azure, azure AD FS, úvod tooAD služby FS, Azure, služby AD FS ve službě Azure iaas, služba AD FS, přesuňte tooazure služby AD FS"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: c5838d749cdc5c8aabbe62b255d568525da747ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Vysoká dostupnost mezi geografickými nasazeními služby AD FS v Azure pomocí Azure Traffic Manageru
[Nasazení služby AD FS v Azure](active-directory-aadconnect-azure-adfs.md) poskytuje podrobné pokyny jako toohow můžete nasadit jednoduchý infrastruktury služby AD FS pro vaši organizaci v Azure. Tento článek obsahuje další kroky hello toocreate cross zeměpisné nasazení služby AD FS v Azure pomocí [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Azure Traffic Manager vám pomůže vytvořit geograficky šíření vysokou dostupnost a vysoce výkonné infrastruktury služby AD FS pro vaši organizaci tím, že použití řadu metod směrování, které jsou k dispozici toosuit různé potřeby z infrastruktury hello.

Infrastruktura služby AD FS s vysokou dostupností mezi geografickými lokalitami umožňuje využití následujících funkcí:

* **Odstranění jediný bod selhání:** pomocí funkce převzetí služeb při selhání z Azure Traffic Manager, můžete dosáhnout vysoce dostupné infrastruktuře služby AD FS, i v případě, že jedno z datových center hello v rámci hello zeměkouli ocitne mimo provoz
* **Zvýšení výkonu:** můžete použít hello navrhované nasazení v této tooprovide článku vysoce výkonné infrastruktury služby AD FS, který vám pomůže rychleji ověření uživatele. 

## <a name="design-principles"></a>Principy návrhu
![Celkový návrh](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

základní principy Hello bude stejné jako uvedené v Principy návrhu v článku hello nasazení služby AD FS v Azure. Hello diagramu výše ukazuje jednoduchý rozšíření hello základní nasazení tooanother zeměpisné oblasti. V následující tabulce jsou některé body tooconsider při rozšíření vaší nasazení toonew zeměpisné oblasti

* **Virtuální sítě:** měli byste vytvořit novou virtuální síť v zeměpisné oblasti hello chcete infrastrukturu toodeploy další AD FS. V diagramu hello výše je zobrazena Geo1 virtuální sítě a virtuální sítí VNET Geo2 hello dvě virtuální sítě v každé geografické oblasti.
* **Řadiče domény a servery služby AD FS v nové sítě VNET zeměpisné:** toodeploy řadiče domény v nové zeměpisné oblasti hello je doporučeno, aby servery hello služby AD FS v nové oblasti hello nemají toocontact řadič domény v jiném daleko tokeny sítě toocomplete ověření a tím vylepšení výkonu hello.
* **Účty úložiště:** Účty úložiště jsou přidruženy k oblasti. Vzhledem k tomu, že budete nasazovat počítačů v nové geografické oblasti, budete mít nové účty úložiště toocreate toobe použité v oblasti hello.  
* **Skupiny zabezpečení sítě:** Jako účty úložiště nelze skupiny zabezpečení sítě vytvořené v určité oblasti použít v jiné geografické oblasti. Proto musíte toocreate nové sítě zabezpečení skupiny podobné toothose v první zeměpisné oblasti hello INT a DMZ podsíť v hello nové geografické oblasti.
* **Názvy DNS pro veřejné IP adresy:** Azure Traffic Manager se může vztahovat tooendpoints pouze prostřednictvím názvy DNS. Proto jsou požadované toocreate názvy DNS pro hello externí nástroje pro vyrovnávání zatížení se veřejné IP adresy.
* **Azure Traffic Manager:** Microsoft Azure Traffic Manager umožňuje distribuci hello toocontrol uživatele provoz tooyour koncové body služby spuštěné v různých datových centrech kolem hello, world. Azure Traffic Manager funguje v hello úrovně DNS. Používá DNS odpovědí toodirect koncového uživatele provoz distribuované tooglobally koncové body. Klienti se koncové body toothose potom připojují přímo. S použitím různých možností směrování výkonu, vážená a Priority můžete se rozhodnout hello směrování možnost nejlépe vyhovuje potřebám vaší organizace. 
* **V net tooV net připojení mezi dvěma oblastmi:** není nutné toohave připojení mezi virtuálními sítěmi hello sám sebe. Vzhledem k tomu, že každý virtuální sítě má přístup toodomain řadiče a má server služby AD FS a WAP sám o sobě, mohou pracovat bez jakékoli připojení mezi virtuálními sítěmi hello v různých oblastech. 

## <a name="steps-toointegrate-azure-traffic-manager"></a>Kroky toointegrate Azure Traffic Manager
### <a name="deploy-ad-fs-in-hello-new-geographical-region"></a>Nasazení služby AD FS v hello nové zeměpisné oblasti
Postupujte podle kroků hello a pokyny v [nasazení služby AD FS v Azure](active-directory-aadconnect-azure-adfs.md) toodeploy hello stejné topologii hello nové geografické oblasti.

### <a name="dns-labels-for-public-ip-addresses-of-hello-internet-facing-public-load-balancers"></a>Názvy DNS pro veřejné IP adresy hello Internet Facing (veřejné) nástroje pro vyrovnávání zatížení
Jak je uvedeno nahoře, hello Azure Traffic Manager se může vztahovat pouze popisky tooDNS jako koncové body a je proto důležité toocreate názvy DNS pro hello externí nástroje pro vyrovnávání zatížení se veřejné IP adresy. Následující snímek obrazovky zobrazuje konfiguraci vašeho popisek DNS pro hello veřejnou IP adresu. 

![Název DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Nasazení Azure Traffic Manageru
Postupujte podle kroků hello toocreate profil správce provozu. Další informace najdete také příliš[spravovat profilu Azure Traffic Manageru](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Vytvoření profilu Traffic Manageru:** Zadejte pro svůj profil Traffic Manageru jedinečný název. Tento název profilu hello je součástí názvu DNS hello a funguje jako předponu pro popisek názvu domény hello Traffic Manager. Název Hello / předpona je přidána too.trafficmanager.net toocreate popisek DNS pro traffic Manageru. Následující snímek obrazovky Hello ukazuje hello traffic Manageru se nastavuje jako mysts a výsledný popisek DNS budou mysts.trafficmanager.net předpona DNS. 
   
    ![Vytvoření profilu Traffic Manageru](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Metoda směrování provozu:** V Traffic Manageru jsou k dispozici tři možnosti směrování:
   
   * Priorita 
   * Výkon
   * Vážená
     
     **Výkon** doporučuje hello možnost tooachieve centry AD FS infrastruktury. Můžete ovšem zvolit kteroukoli metodu směrování, která nejlépe vyhovuje vašim potřebám nasazení. Funkce Hello AD FS nebude ovlivněné hello směrování možnost. Další informace najdete v článku [Metody směrování provozu Traffic Manageru](../traffic-manager/traffic-manager-routing-methods.md). Snímek obrazovky ukázkové výše můžete v hello zobrazíte hello **výkonu** vybrané metodě.
3. **Nakonfigurovat koncové body:** hello provoz manager stránce, klikněte na koncové body a vyberte Přidat. Otevře se přidat koncový bod stránky podobné toohello následující snímek obrazovky
   
   ![Konfigurace koncových bodů](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Pro různé vstupy hello postupujte podle obecných zásad hello níže:
   
   **Typ:** vyberte koncového bodu Azure, jak jsme odkazující tooan Azure veřejnou IP adresu.
   
   **Název:** vytvořit název, které chcete tooassociate s koncovým bodem hello. Toto není název DNS hello a nemá žádný vliv na záznamy DNS.
   
   **Cíl typ prostředku:** vyberte veřejnou IP adresu jako vlastnost toothis hodnotu hello. 
   
   **Cíl prostředků:** tato položka vám poskytne možnost toochoose z jiné názvy DNS hello máte k dispozici v rámci svého předplatného. Zvolte hello DNS popisku odpovídající toohello koncový bod, který konfigurujete.
   
   Přidání koncového bodu pro každé geografické oblasti, které chcete provoz tooroute Azure Traffic Manager hello.
   Další informace a podrobné kroky postupu tooadd / konfigurace koncových bodů v traffic Manageru, najdete v příliš[přidat, zakázání, povolení nebo odstranění koncové body](../traffic-manager/traffic-manager-endpoints.md)
4. **Konfigurace testu:** hello provoz manager klikněte v konfiguraci. Na stránce konfigurace hello budete potřebovat toochange hello monitorování nastavení tooprobe na HTTP port 80 a /adfs/probe relativní cestu
   
    ![Konfigurace testu](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Zajistěte, aby byl stav hello koncových bodů hello ONLINE po dokončení konfigurace hello**. Pokud všechny koncové body jsou ve stavu 'sníženou', provede Azure Traffic Manager pokus o tooroute hello provoz best za předpokladu, že hello diagnostiky jsou nesprávné a jsou dostupné všechny koncové body.
   > 
   > 
5. **Úpravy záznamů DNS pro Azure Traffic Manageru:** federační služby by měl být název CNAME toohello Azure Traffic Manager DNS. Vytvořte záznam CNAME ve hello veřejné záznamy DNS tak, aby kdo se pokouší tooreach hello federační služby ve skutečnosti dosáhne hello Azure Traffic Manager.
   
    Například toopoint hello federační služby fs.fabidentity.com toohello Traffic Manager, měli byste tooupdate vaše hello toobe záznamů prostředků DNS následující:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-hello-routing-and-ad-fs-sign-in"></a>Testování hello směrování a přihlášení služby AD FS
### <a name="routing-test"></a>Test směrování
Velmi základní test pro směrování hello by tootry ping hello název federační služby DNS z počítače v každé geografické oblasti. V závislosti na metodě směrování hello vybrané se projeví v zobrazení ping hello hello koncový bod, který ve skutečnosti testuje se příkazem ping. Například pokud jste vybrali hello výkonu směrování, pak koncový bod hello nejbližší toohello oblast hello klienta bude dostupný. Níže je snímek hello dva příkazy ping ze dvou různých oblast klientské počítače, jeden v oblasti EastAsia a jeden v západní USA. 

![Test směrování](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>Test přihlášení ke službě AD FS
Nejjednodušší způsob, jak tootest Hello služby AD FS je pomocí stránky IdpInitiatedSignon.aspx hello. V pořadí toobe toodo možné, že je požadovaná tooenable hello IdpInitiatedSignOn na vlastnosti hello služby AD FS. Postupujte podle kroků hello tooverify vaší instalace služby AD FS

1. Spustit hello níže na server hello AD FS, pomocí prostředí PowerShell, tooset rutinu ho tooenabled. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. Ze kteréhokoli externího počítače zobrazte tuto stránku: https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Měli byste vidět stránku hello služby AD FS, jako níže:
   
    ![Test služby AD FS – test ověřování](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    Po úspěšném přihlášení se zobrazí zpráva o úspěchu, jak je znázorněno níže:
   
    ![Test služby AD FS – úspěšné ověření](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Související odkazy
* [Základní nasazení služby AD FS v Azure](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
* [Metody směrování provozu Traffic Manageru](../traffic-manager/traffic-manager-routing-methods.md)

## <a name="next-steps"></a>Další kroky
* [Správa profilu Azure Traffic Manageru](../traffic-manager/traffic-manager-manage-profiles.md)
* [Přidávání, zakazování, povolování nebo odstraňování koncových bodů](../traffic-manager/traffic-manager-endpoints.md) 

