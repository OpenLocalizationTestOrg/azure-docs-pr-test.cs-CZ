---
title: "aaaCreate externí Azure App Service environment"
description: "Vysvětluje, jak vytvořit toocreate služby App Service environment, když jste aplikace nebo samostatné"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 94dd0222-b960-469c-85da-7fcb98654241
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: f8619534ddd889ea65063733ac6ec11b206e799c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-external-app-service-environment"></a>Vytvoření externího App Service environment #

Azure App Service Environment je nasazení služby Azure App Service do používanou podsíť virtuální sítě Azure (VNet). Existují dva způsoby toodeploy služby App Service environment (App Service Environment):

- V VIP na externí IP adresu často označuje jako externí App Service Environment.
- S hello VIP na interní IP adresu často říká App Service Environment ILB protože hello vnitřní koncový bod je vyrovnávání interní zatížení (ILB).

Tento článek ukazuje, jak toocreate externí App Service Environment. Přehled hello App Service Environment, najdete v části [Úvod toohello App Service Environment][Intro]. Informace o tom, jak toocreate ILB App Service Environment, najdete v části [vytvoření a použití App Service Environment ILB][MakeILBASE].

## <a name="before-you-create-your-ase"></a>Před vytvořením vaší App Service Environment ##

Po vytvoření vaší App Service Environment, nelze změnit hello následující:

- Umístění
- Předplatné
- Skupina prostředků
- Virtuální síť použít
- Podsítě používané
- Velikost podsítě

> [!NOTE]
> Vyberete virtuální síť a zadejte podsíť, ujistěte se, že je dostatečně velké na to tooaccommodate budoucímu růstu. Doporučujeme velikost `/25` adresy 128.
>

## <a name="three-ways-toocreate-an-ase"></a>Tři způsoby toocreate App Service Environment ##

Existují tři způsoby toocreate App Service Environment:

- **Při vytváření plán služby App Service**. Tato metoda vytvoří hello App Service Environment a hello plán služby App Service v jednom kroku.
- **Jako samostatné akce**. Tato metoda vytváří samostatný App Service Environment, což je App Service Environment bez obsahu. Tato metoda je pokročilejší toocreate proces App Service Environment. Můžete použít toocreate App Service Environment s ILB.
- **Z šablony Azure Resource Manager**. Tato metoda je pro zkušené uživatele. Další informace najdete v tématu [ze šablony vytvořit App Service Environment][MakeASEfromTemplate].

Externí App Service Environment má veřejné VIP, což znamená, že všechny aplikace toohello provoz protokolu HTTP nebo HTTPS v hello App Service Environment dotkne přístupné z Internetu IP adresu. App Service Environment s ILB, obsahuje IP adresu z podsítě hello používá hello App Service Environment. Hello aplikacím hostovaných v App Service Environment ILB nejsou zveřejněné přímo toohello Internetu.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Vytvořit App Service Environment a plán služby App Service společně ##

Hello plán služby App Service je kontejnerem aplikace. Když vytvoříte aplikaci v App Service, zvolte nebo vytvořte plán služby App Service. Hello kontejneru modelu prostředí podržte plány služby App Service a plány služby App Service uchování aplikace.

toocreate App Service Environment při vytváření plán služby App Service:

1. V hello [portál Azure](https://portal.azure.com/), vyberte **nový** > **Web + mobilní** > **webové aplikace**.

    ![Vytvoření webové aplikace][1]

2. Vyberte své předplatné. aplikace Hello hello App Service Environment jsou vytvořeny a hello stejné předplatné.

3. Vyberte nebo vytvořte skupinu prostředků. Pomocí skupin prostředků můžete spravovat souvisejících prostředků Azure jako jednotku. Skupiny prostředků také jsou užitečné, když vytvoříte pravidla řízení přístupu na základě rolí pro vaše aplikace. Další informace najdete v tématu hello [přehled Azure Resource Manageru][ARMOverview].

4. Vyberte hello plán služby App Service a pak vyberte **vytvořit nový**.

    ![Nový plán aplikační služby][2]

5. V hello **umístění** rozevíracího seznamu, vyberte hello oblast, kam chcete toocreate hello App Service Environment. Pokud vyberete existující App Service Environment, se nevytvoří nové App Service Environment. Hello plán služby App Service je vytvořená ve hello App Service Environment, kterou jste vybrali. 

6. Vyberte **cenová úroveň**a vyberte jednu z hello **izolovaná** ceny SKU. Pokud se rozhodnete **izolovaná** karty SKU a umístění, které není App Service Environment, nové App Service Environment se vytvoří v tomto umístění. Vyberte toostart hello proces toocreate App Service Environment, **vyberte**. Hello **izolovaná** SKU je k dispozici pouze ve spojení s App Service Environment. Také nemůžete použít jiné cenové SKU App Service Environment jiné než **izolovaná**.

    ![Výběr cenové úrovně][3]

7. Zadejte název vaší App Service Environment hello. Tento název se používá v hello adresovatelné název pro vaše aplikace. Pokud je název hello hello App Service Environment _appsvcenvdemo_, je název domény hello *. appsvcenvdemo.p.azurewebsites.net*. Pokud vytvoříte aplikaci s názvem *mytestapp*, je adresovatelné v mytestapp.appsvcenvdemo.p.azurewebsites.net. Prázdné znaky nelze použít v názvu hello. Pokud používáte velká písmena, název domény hello je celkový počet malých verze hello s tímto názvem.

    ![Nový název plánu aplikační služby][4]

8. Zadejte vaše Azure podrobnosti virtuální sítě. Vyberte buď **vytvořit nový** nebo **vyberte existující**. je dostupná pouze v případě, že máte virtuální síť ve vybrané oblasti hello Hello možnost tooselect stávající virtuální síť. Pokud vyberete **vytvořit nový**, zadejte název hello virtuální sítě. Vytvoří se nové virtuální sítě Resource Manageru s tímto názvem. Používá hello adresní prostor `192.168.250.0/23` v hello vybrané oblasti. Pokud vyberete **vybrat existující**, budete muset:

    a. Blok adres hello virtuální sítě, vyberte, pokud máte více než jeden.

    b. Zadejte nový název podsítě.

    c. Vyberte velikost hello hello podsítě. *Mějte na paměti, tooselect velikost dostatečně velké na to tooaccommodate budoucího růstu vašeho App Service Environment.* Doporučujeme, abyste `/25`, která je 128 adresy a dokáže zpracovat App Service Environment maximální velikost. Nedoporučujeme `/28`, například, protože nejsou k dispozici pouze 16 adres. Infrastruktura používá alespoň pět adres. V `/28` podsíť, jste zbývajících s maximální škálování 11 instancí.

    d. Vyberte rozsah hello podsítě IP adres.

9. Vyberte **vytvořit** toocreate hello App Service Environment. Tento proces vytvoří také hello plán aplikační služby a aplikace hello. Hello App Service Environment, plán aplikační služby a aplikace jsou pod hello stejného předplatného, a také v hello stejné skupiny prostředků. Pokud vaše App Service Environment musí skupinu samostatné prostředků nebo pokud potřebujete App Service Environment ILB, postupujte podle kroků toocreate hello App Service Environment samostatně.

## <a name="create-an-ase-by-itself"></a>Vytvořit App Service Environment samostatně ##

Pokud vytvoříte samostatné App Service Environment, má nic v ní. Prázdný App Service Environment stále způsobuje měsíčních poplatků pro infrastrukturu hello. Postupujte podle těchto kroků toocreate App Service Environment s ILB nebo toocreate App Service Environment ve vlastní skupině prostředků. Po vytvoření vaší App Service Environment, se můžou vytvářet aplikace pomocí běžných postupů hello. Vyberte vaše nové App Service Environment jako hello umístění.

1. Hledání hello Azure Marketplace pro **App Service Environment**, nebo vyberte **nový** > **mobilní webové** > **služby App Service Prostředí**. 

2. Zadejte název vaší App Service Environment hello. Tento název se používá pro aplikace hello vytvořené v hello App Service Environment. Pokud je název hello *mynewdemoase*, je název subdomény hello *. mynewdemoase.p.azurewebsites.net*. Pokud vytvoříte aplikaci s názvem *mytestapp*, je adresovatelné v mytestapp.mynewdemoase.p.azurewebsites.net. Prázdné znaky nelze použít v názvu hello. Pokud používáte velká písmena, je název domény hello hello celkový malá verze hello název. Pokud používáte ILB, název App Service Environment není používáno ve vašem subdomény, ale je místo výslovně uvedeno během vytváření App Service Environment.

    ![Pojmenování App Service Environment][5]

3. Vyberte své předplatné. Toto předplatné je také hello, který použít všechny aplikace hello App Service Environment. Nelze vložit vaše App Service Environment ve virtuální síti, který je v jiné předplatné.

4. Vyberte nebo zadejte novou skupinu prostředků. Skupina prostředků Hello používá pro vaše App Service Environment musí být hello stejné ten, který se používá pro vaši virtuální síť. Pokud vyberete stávající virtuální síť, hello výběr skupiny prostředků pro vaše App Service Environment je aktualizovaný tooreflect u vaší virtuální sítě. *Můžete vytvořit App Service Environment se skupinou prostředků, které se liší od skupiny prostředků hello virtuální sítě, pokud používáte šablonu Resource Manager.* toocreate App Service Environment ze šablony, najdete v části [vytvoření služby App Service environment ze šablony][MakeASEfromTemplate].

    ![Výběr skupiny prostředků][6]

5. Vyberte virtuální síť a umístění. Můžete vytvořit novou virtuální síť nebo vybrat stávající virtuální síť: 

    * Pokud vyberete novou virtuální síť, můžete zadat název a umístění. Hello nové virtuální sítě má 192.168.250.0/23 rozsah adres hello a podsíť s názvem výchozí. podsíť Hello je definován jako 192.168.250.0/24. Můžete vybrat pouze virtuální sítě Resource Manageru. Hello **VIP typ** výběr určuje, pokud vaše App Service Environment je přímo přístupný z Internetu (externí) hello nebo, pokud používá ILB. toolearn více o těchto možnostech najdete v části [vytvoření a použití vyrovnávání zatížení interní služby App Service environment][MakeILBASE]. 

      * Pokud vyberete **externí** pro hello **VIP typ**, můžete vybrat, kolik externího systému hello adresy IP je vytvořen s pro účely založená na protokolu IP. 
    
      * Pokud vyberete **interní** pro hello **VIP typ**, je nutné zadat hello domény, který používá vaše App Service Environment. App Service Environment můžete nasadit do virtuální sítě využívající veřejného nebo soukromého adresního rozsahy. toouse virtuální síť s rozsahem veřejnou adresu, musíte toocreate hello virtuální síť předem. 
    
    * Pokud vyberete stávající virtuální síť, podsíť nové se vytvoří při vytvoření hello App Service Environment. *V portálu hello nelze použít předem vytvořené podsíť. Pokud používáte šablonu Resource Manager, můžete vytvořit App Service Environment s existující podsítí.* toocreate App Service Environment ze šablony, najdete v části [vytvoření služby App Service Environment ze šablony][MakeASEfromTemplate].

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##

Přesto můžete vytvořit instance hello první verzi služby App Service Environment (ASEv1). toostart, že proces vyhledávání hello Marketplace pro **App Service Environment v1**. Vytvořit App Service Environment hello v hello stejným způsobem, že vytvoříte hello samostatné App Service Environment. Po dokončení, vaše ASEv1 má dva elementy end front a dvě pracovních procesů. S ASEv1 musí spravovat hello front-end a pracovních procesů. Budou se automaticky přidá, není při vytváření plánu služby App Service. Hello front-end fungovat jako koncové body hello protokolu HTTP nebo HTTPS a odeslat provoz toohello pracovníky. pracovníci Hello jsou hello role, které umožňuje hostování vašich aplikací. Po vytvoření vaší App Service Environment, můžete upravit hello objemu front-end a pracovních procesů. 

toolearn Další informace o ASEv1, najdete v části [toohello Úvod App Service Environment v1][ASEv1Intro]. Další informace o škálování, správu a sledování ASEv1, najdete v části [jak tooconfigure služby App Service Environment][ConfigureASEv1].

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png



<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
