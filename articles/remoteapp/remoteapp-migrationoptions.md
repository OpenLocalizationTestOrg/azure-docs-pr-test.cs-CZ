---
title: "Možnosti pro migraci z Azure Remoteappu | Microsoft Docs"
description: "Informace o možnostech pro migraci z Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c4e0e5bc-5c13-4487-b1b6-ebf2a5edc1f0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9ab63124e2521ee1922d15c1e388c54d50eb8301
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Možnosti pro migraci z Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).


Pokud je nutné zastavit pomocí Azure Remoteappu z důvodu [vyřazení oznámení](https://go.microsoft.com/fwlink/?linkid=821148) nebo protože dokončení testování, je potřeba migrovat z Azure RemoteApp do jiné služby app service. Existují dva různé přístupy k migraci: spravuje vlastními silami (často říká infrastruktury jako služby [IaaS]) nasazení nebo plně spravovaná (často označované jako platforma jako služba) nebo softwaru jako služby [PaaS nebo SaaS] nabídky. 

Samoobslužné služby IaaS je samoobslužné nasazení, které je spravovaná, provozovat a vlastní můžete nasadit přímo na virtuální počítače (VM) nebo fyzickými systémy. Na druhém konci plně spravovaná PaaS nebo SaaS nabídka je informace, například Azure RemoteApp – partnera poskytuje služby vrstvu nad vzdálenou komunikaci řešení, která zpracovává provozní a obsluhu, při, jako zákazník, do správy některé image a aplikací.

[Zobrazit webináře Azure RemoteApp na možnosti migrace](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), nebo si můžete přečíst na další informace (včetně příkladů různé možnosti hostování).

## <a name="self-managed-iaas-solutions"></a>Spravuje vlastními silami řešení (IaaS)
### <a name="rds-on-iaas"></a>**Vzdálená plocha na IaaS**
Nasazení můžete nativní na bázi relace služby Vzdálená plocha (v systému Windows Server) pomocí vzdálené aplikace RemoteApp nebo klienty na místně nebo v hostovaném prostředí (jako na virtuálních počítačích Azure). Vzdálená plocha na IaaS nasazení jsou nejlepší pro zákazníky už znáte a které mají existující technických otázek u nasazení vzdálené plochy. 

> [!NOTE]
> Musíte Volume Licensing s Software Assurance (SA) pro vzdálené plochy licencí pro klientský přístup pro použití této možnosti nasazení.

Nasazení vzdálené plochy na virtuálních počítačích Azure je jednodušší než kdy při použití nasazení a opravy chyb šablony (čtení [přehled](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) a potom [přejděte získat je](https://aka.ms/rdautomation)). Můžete získat stejné možnosti elastické škálování s Azure classic nasazení modelu prostředky (ne prostředky Model prostředků Azure) v rámci Azure RemoteApp pomocí [automatické škálování skriptu](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), i když existují další přizpůsobení a konfigurace. Při nasazení vzdálené plochy na virtuálních počítačích Azure, je poskytována prostřednictvím podpory [podporu Azure](https://azure.microsoft.com/support/plans/), stejné podporují Odborníci v oblasti, která je podporována s Azure Remoteappem. Můžete získat náklady odhady založené na vaší stávající využití kontaktováním [podporu Azure](https://azure.microsoft.com/support/plans/), nebo můžete výpočty provést sami pomocí brzy být vydané kalkulačky náklady.  S virtuálními počítači N-series (aktuálně v soukromém náhledu) můžete také přidat vGPU - informace o přidávání vGPU a o tom, jak nás [budou využívat vylepšení vzdálené plochy v systému Windows Server 2016](https://myignite.microsoft.com/videos/2794) v našem Ignite relaci.   

Máme průvodcích nasazením krok za krokem pro [Windows Server 2012 R2](http://aka.ms/rdsonazure) a [systému Windows Server 2016](http://aka.ms/rdsonazure2016) pomoct s nasazením. Podívejte se [vzdálené plochy blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) nejnovější informace.

### <a name="citrix-on-iaas"></a>**Citrix na IaaS**
Nativní Citrix nasazení na základě relace XenApp nebo XenDesktop lze nasadit místně nebo v hostovaném prostředí (například na virtuálních počítačích Azure). 

Podívejte se na průvodci podrobný postup nasazení [Citrix XA 7.6 v Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), další informace. Další informace o [Citrix v Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), včetně kalkulačky ceny. Můžete také vyhledat [Citrix obraťte se na](http://citrix.com/English/contact/index.asp) dohodnout se vaše možnosti.

## <a name="fully-managed-paassaas-offerings"></a>Plně spravovaná nabídky (PaaS nebo SaaS)

### <a name="citrix-xenapp-essentials-released-april-2017"></a>Citrix XenApp Essentials (vydané dubna 2017)
Nyní dostupné na [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials je nová služba virtualizace aplikace, kombinace výkon a flexibilitu Citrix Cloudová platforma s jednoduchý, doporučený a snadno využívat vizi Microsoft Azure RemoteApp. 

Stávající zákazníky služby Azure RemoteApp můžete [zaregistrovat k bezplatné zkušební verzi](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).  Poznámka: Pouze Citrix poplatek za uživatele služby je volné, použít Azure náklady na výpočetních operací a úložiště

Víc se uč:
- [Migrace z Azure Remoteappu k Citrix XenApp Essentials](remoteapp-migrate-citrix.md)
- [Citrix a společnosti Microsoft](https://www.citrix.com/global-partners/microsoft/remote-app.html)
- [Prezentace Citrix XenApp Essentials](https://www.youtube.com/watch?v=91Z7CCfQ-9k).  

### <a name="citrix-cloud-xenapp-service-and-xendesktop-service"></a>Cloud XenApp Citrix a XenDesktop služba 

[Cloud XenApp Citrix a služba XenDesktop](https://www.citrix.com/products/citrix-cloud/services.html) je nejlepší řešení pro doručování aplikace a stolní počítače, plus pokročilé správy i možnosti monitorování. 

#### <a name="conexlink-platform-name-mycloudit"></a>Conexlink (název platformy: MyCloudIT)
[MyCloudIT](https://mycloudit.com) platformu automatizace pro IT společnosti zjednodušit, optimalizace a škálovat migrace a doručování vzdálené plochy, vzdálené aplikace a infrastrukturu v cloudu Microsoft Azure. 

Platforma MyCloudIT snižuje dobu nasazení 95 %, Azure náklady 30 % a přesouvá celé infrastruktury IT jejich klienta do cloudu v řádu tahy pár klíčů. Partneři teď můžou zákazníci spravovat z jedné globální řídicího panelu, koncoví uživatelé služby po celém světě jako nikdy předtím a růst výnosů bez přidání dalších zásahů nebo rozsáhlé Azure školení.  

> Primární umístění: Dallas, TX, USA
> 
> Operace oblast: po celém světě
> 
> Partner stav: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)
> 
> Zprostředkovatele služby Microsoft Cloud: Ano
> 
> Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak
> 
> Migrace řešení Azure RemoteApp: Ano, [Další informace](https://mycloudit.com/remote-app-microsoft/)
> 
> Brian Garoutte, VP obchodní vývoj
> 
> Phone: 972-218-0741
>   
> E-mailu:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

### <a name="frame"></a>Rámce

Organizace IT v podniku a government, zprostředkovatelé spravované služby a předními dodavateli softwaru vybrat rámce vytvářet a spravovat jejich zabezpečení, softwarově definované pracovní prostory v cloudu. Z malé a velké organizace, rámeček umožňuje velmi snadno tak, aby uživatelé přístup k aplikacím Windows prostřednictvím libovolného prohlížeče z libovolného zařízení. Platforma rámce obsahuje všechno, co správce potřebuje k nasazení aplikací z cloudu, včetně infrastruktury Azure a licencí Vzdálené plochy (přinesou vlastní účet Azure a licencí je volitelný). 

Další informace o [rámce v Azure](https://www.fra.me/ara). 

> Primární umístění: Mateo sítě San, certifikační Autority, USA
>
> Operace oblast: po celém světě
>
> Partner společnosti Microsoft: Ano
> 
> Telefon: 1-480-269-4668

### <a name="awingu"></a>Awingu
Awingu poskytuje jednoduché online prostoru řešení spuštěná starší verze aplikace, SaaS a dokumenty z prohlížeče html5. Jako takový zpřístupnění všechny aplikace bezpečně na všech typů zařízení. Pro služby SaaS je k dispozici široké škále op Single-Sign-On možnosti. Systémy souborů různých (cloud) také může být úzce integruje, do pracovního prostoru. Vedle úplné mobility získáte bohaté online prostoru na Awingu optimální zabezpečení granulární ovládacích prvků (například stahování nebo odesílání), úplná využití auditování, Multi-Factor Authentication (např. Azure MFA), záznam relace a další. Out-of-the-box, Awingu umožňuje dokumentu a sdílení aplikací relace pro optimalizované a zabezpečenou spolupráci.
Na Awingu řešení je více klientů, službu AD a otevřené rozhraní API. Se používají v malých a velkých firmách, poskytovatele cloudových služeb a [ISV](http://www.isv2saas.com). Těchto zákazníků ocení zejména snadné používání, snadná instalace a nízké náklady na vlastnictví.

Je Awingu vše v jednom [dostupné v Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) s 2 předdefinované souběžných uživatelů. Další licence jsou k dispozici prostřednictvím [širokou škálu distributorů a prodejce](http://www.awingu.com/reseller).

Další informace o [Awingu na jako alternativní do Azure Remoteappu](http://alternative-for-azure-remoteapp.awingu.com/).


> Primární umístění: Belgie
> 
> Operační oblastí: Severní Americe, regionu EMEA a Brazílie
> 
> Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak 
> 
> **Globální**:
> 
> Arnaudu Marlière, podílu
> 
> E-mailu:[arnaud@awingu.com](mailto:arnaud@awingu.com)
> 
> Phone: +1 646 583 3025
> 
> **Ústředí Belgie**:
> 
> Ottergemsesteenweg-Zuid 808 B44
> 
> 9000 služ
> 
> E-mailu:[info@awingu.com](mailto:info@awingu.com) 
> 
> Telefon: + 32 9 296 40 11
> 
> **USA**:
> 
> 7. podlaží 1177 průměr z Americas,
> 
> New York, NY 10036
> 
> E-mailu:[info.us@awingu.com](mailto:info.us@awingu.com)

### <a name="microsoft-hosted-service-provider"></a>Microsoft hostované poskytovatele služeb
Hostování partnery obvykle nabízejí plně spravovaná hostované Windows desktop a služba aplikace, které mohou zahrnovat Správa prostředků Azure, operačních systémů, aplikací a pomocí partnerovi technickou podporu této licenční smlouvy s Microsoft a jiných softwarové poskytovatele spolu se služba Zprostředkovatel licenční smlouvu umožňující následný z odběratele přístup licence (SAL). Následující informace poskytují podrobnosti a kontaktní informace pro některé z hostitelů, které se specializují na zákazníky s jejich Azure RemoteApp migrace, které. Podívejte se na [aktuální seznam zprostředkovatelů služby hostované](http://aka.ms/rdsonazurecertified) který dokončili vzdálené plochy na IaaS učení cestu a hodnocení.  

### <a name="citrix-service-provider-program"></a>Program poskytovatele služeb systému Citrix
Program poskytovatele služeb Citrix snadno pro poskytovatele služeb k poskytování jednoduchost virtuální cloud computing chcete SMB, jejich nabízení služby, které chtějí ve model snadno, průběžnými platbami. Poskytovatelé služeb Citrix rozvoji svého podnikání Microsoft SPLA a rozbalte jejich investic platformy vzdálené plochy pro jakékoli zařízení, přístup odkudkoli, nejširší podporu aplikace, bohaté prostředí, zvýšení zabezpečení a vyšší škálovatelnost. Naopak poskytovatelé služeb Citrix přilákat další odběratele, zvýšit spokojenost zákazníků a snížit provozní náklady. [Další informace](http://www.citrix.com/products/service-providers.html) nebo [najít partnera](https://www.citrix.com/buy/partnerlocator.html).

#### <a name="acuutech"></a>Acuutech
[Acuutech](http://www.acuutech.com) se specializuje na poskytování hostované plochy řešení doručování úplné ploše a aplikacím ISV prostředí založený na technologii Microsoft na základní globální klienta z Azure a vlastních datových center.

> Primární umístění: Londýn, Spojené království; Singapur; Houston, TX
> 
> Operace oblast: po celém světě
> 
> Partner stav: Gold
> 
> Zprostředkovatele služby Microsoft Cloud: Ano
> 
> Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak
> 
> Migrace řešení Azure RemoteApp: Ano, [Další informace](http://www.acuutech.com/ara-migration/)
> 
> **Spojené království**:
>   
> 5/6 Yorku úklidové, Langston silniční
>   
> Loughton, Essexu IG10 3TQ
>   
> Telefon: + 44 (0) 20 8502 2155
> 
> **Singapur**:
>   
> 100 Cecil ulici, #09-02 
>   
> Celém světě, Singapur 069532
> 
> Phone: 4933 6709 + 65
>   
> **Severní Amerika**:
>   
> 3601 S. Sandman St.
>   
> Sada 200, Houston, TX 77098
>   
> Phone: +1 713 691 0800

#### <a name="aspex"></a>ASPEX
[ASPEX](http://www.aspex.be/en) se specializuje na ISV přechodu do cloudu a ISV' vyhledávání za účelem optimalizace jejich aktuální nastavení cloudu. ASPEX nabízí širokou škálu spravované služby, devops a konzultační služby.  

> Primární umístění: Antverpy, Belgie
> 
> Operace oblast: západní Evropa
> 
> Partner stav: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)
> 
> Zprostředkovatele služby Microsoft Cloud: Ano
> 
> Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak
> 
> Migrace řešení Azure RemoteApp: Ano, [Další informace](https://www.aspex.be/en/azure-remote-apps)
> 
> Phone: +3232202198
> 
> E-mailu:[info@aspex.be](mailto:info@aspex.be)
> 
> Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="caasecom"></a>Caase.com
[Caase.com](http://www.caase.com/) pomáhá podnikům, místní správy, jiných vládních subjektů a poskytovatelem zdravotní instituce s cestě směrem inteligentnější způsob práce v Microsoft cloudu. Probíhá produktivitu a zabezpečení v jakémkoli místě s jakýmkoli a nízké náklady na IT. Caase.com je true specialisty Microsoft Office365, Azure, Enterprise Mobility a zabezpečení a systému Windows. U naše poradenské služby migrace, přijetí programy, školení správy a podpory Caase.com vytvoří optimalizované a zabezpečené platformu pro spolupráci pro obě zákazníků zaměstnance, partnery a dodavateli.
Caase.com je mastermind vzdálené prostoru Azure (mobilní síti na pracovišti) a digitální síti na pracovišti (sociální Intranet). Obě řešení – dosáhnout s přijetím – jsou foundation, která zajistí, že uživatelé z těchto řešení prostředí příjemný, úspěšné a efektivní v jejich trasy ke cloudu Microsoftu.
Holandská překlad ánd podpůrné film přes zde: http://caase.com/over-ons/

> Operace oblast: na základě holandština, globální sítě
> 
> Partner stav: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)
> 
> Zprostředkovatele služby Microsoft Cloud: Ano
> 
> Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak
> 
> Migrace řešení Azure RemoteApp: Ano, [Další](http://caase.com/diensten/microsoft-azure/).
> 
> 
> Nizozemsko:
> 
> Rigtersbleek-Zandvoort 10 (De Spinnerij)
> 
> 7521 BE, nizozemském Enschede
> 
> Phone: +31 (0) 88 4320 000


#### <a name="nerdio"></a>Nerdio
[Nerdio pro Azure](http://getnerdio.com/nfa/) platformu automatizace IT, který nabízí ridiculously jednoduché zřizování, správu a optimalizace dokončení prostředí IT v cloudu Microsoft. Stát virtuálním plochám, vzdálené aplikace a servery v několika hodin. Spravovat prostředí v tři klepnutí nebo méně s Nerdio portál pro správu. Použití inteligentního automatické škálování a uložit 40 až 60 % prostředky Azure IaaS.

> Primární umístění: operace Chicagu, IL oblast: stav po celém světě partnera: [Gold](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) poskytovatele cloudové služby společnosti Microsoft: Ano
> 
> Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak
> 
> Migrace řešení Azure RemoteApp: Ano
> 
> 
> 8001 Lincoln průměr
> 
> Suite 212
> 
> Skokie, IL 60077
> 
> USA
> 
> Ext. 4NERDIO (844) 6
> 
> [sayhello@getnerdio.com](mailto:sayhello@getnerdio.com)

#### <a name="saasplaza"></a>**SaaSplaza**
[SaaSplaza](http://www.saasplaza.com/) nabízí kompletní Microsoft Dynamics portfolií (navigaci, AX, zásad skupiny, SL, CRM) privátní a veřejné cloudu (Azure).

> Primární umístění: Nizozemsko
> 
> Operace oblast: po celém světě
> 
> Partner stav: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)
> 
> Zprostředkovatele služby Microsoft Cloud: Ano
> 
> Nabízí řešení RemoteApp a vzdálené ploše na bázi relací: Ano, jak
> 
> **EMEA**:
> 
> Prins Mauritslaan 29 35
> 
> Badhoevedorp 71 lineárního programování ÚLOH
> 
> Nizozemsko
> 
> Phone: 8060 547 20 +31 
> 
>  **Americas**:
> 
> 171 Sasko silniční, Suite 105
> 
> 92024 Encinitas, certifikační Autority
> 
> Síť San Diego
> 
> Spojené státy
> 
> Phone: +1 858 385 8900 
> 
> **APAC**:
> 
> 105 Cecil ulice
>    
> \#11-08, Osmiúhelník
> 
> Singapur 069534
> 
> Singapur
>   
> Phone - Singapur: 6591 6222 + 65
> 
> Phone – Austrálie: + 61 2 8310 5568 
>    
> Phone – Nový Zéland: 0321 488 4 + 64
> 
## <a name="need-more-help"></a>Potřebujete další pomoc?
Stále potřebujete pomoc, výběr nebo máte další otázky? Jak získat nápovědu, použijte jednu z následujících metod. 

1. E-mailu nás na adrese [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).
2. Obraťte se na [podporu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Začněte otevřením [případu podpory Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3. Kontaktovat nás telefonicky. [Nalezení místní prodejní čísla](https://azure.microsoft.com/overview/sales-number/).

