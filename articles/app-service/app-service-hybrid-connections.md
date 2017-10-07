---
title: "AAA \"hybridní připojení služby Azure App Service | Microsoft Docs\""
description: "Jak toocreate a používání prostředků tooaccess hybridní připojení v různorodých sítě"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: 61d58068ab0a7c803019e3f0e92bde4273d1a053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Hybridní připojení služby Azure App Service #

## <a name="overview"></a>Přehled ##

Hybridní připojení je součástí služby v Azure jak funkce hello Azure App Service.  Jako služba má použití a možnosti nad rámec těch, které jsou využity v hello Azure App Service.  Další informace o hybridních připojení a jejich využití mimo hello Azure App Service můžete spustit zde toolearn [Azure předávání hybridní připojení][HCService]

V rámci hello Azure App Service může být hybridní připojení prostředky používané tooaccess aplikace v jiných sítích. Poskytuje přístup z koncového bodu se vaše aplikace tooan aplikace.  Ho neumožňuje tooaccess alternativní schopnosti vaší aplikace.  Používá v hello služby App Service, každý hybridní připojení korelaci tooa jednoho hostitele a portu kombinace TCP.  To znamená, že tohoto koncového bodu hello hybridní připojení může být v jakémkoliv operačním systému a všechny aplikace, pokud že jste nedosáhli naslouchání portu TCP. Hybridní připojení neznáte nebo vědět, jaké aplikační protokol hello je nebo co se připojujete.  Jednoduše poskytuje přístup k síti.  


## <a name="how-it-works"></a>Jak to funguje ##
Funkce Hello hybridního připojení se skládá z dvě odchozí volání tooService Bus Relay.  Připojení z knihovny na hello hostitele, kde vaše aplikace běží v hello app service a nejsou k dispozici připojení z hello hybridní připojení Manager(HCM) tooService Bus relay existuje.  Hello HCM je předávací služba, která můžete nasadit v rámci hello sítě hostování 

Prostřednictvím hello dvě připojené k připojení má vaše aplikace TCP tunel tooa pevné kombinace hostitele a portu na hello druhé straně hello HCM.  připojení Hello používá TLS 1.2 pro zabezpečení a klíče SAS pro ověřování nebo autorizace.    

![][1]

Pokud vaše aplikace odešle požadavek DNS odpovídající koncový bod konfigurovat hybridní připojení a pak hello odchozí TCP provoz bude přesměrován dolů hello hybridní připojení.  

> [!NOTE]
> To znamená, že byste měli zkusit tooalways použijte název DNS pro hybridní připojení.  Některé klientský software neprovádí vyhledávání DNS, pokud koncový bod hello místo toho používá IP adresu.
>
>

Existují dva typy hybridní připojení, hello nové hybridní připojení, která jsou nabízeny jako službu v Azure předávání a hello starší BizTalk hybridní připojení.  Hello starší BizTalk hybridní připojení jsou odkazované tooas Classic hybridní připojení hello portálu.  Další informace dále v tomto dokumentu o nich není k dispozici.

### <a name="app-service-hybrid-connection-benefits"></a>Služby App Service hybridní připojení výhody ###

Existuje několik výhod toohello hybridní připojení schopností včetně

- Aplikací mít bezpečný přístup na místní systémy a služby bezpečně
- Funkce Hello nevyžaduje internet dostupný koncový bod
- je rychlý a snadný tooset nahoru  
- každé hybridní připojení odpovídá kombinací tooa jednoho hostitele a portu, která je vynikající zabezpečení aspekt
- za normálních okolností nevyžaduje brány firewall díry přes standardní webu porty jsou všechny odchozí připojení hello
- protože funkce hello je úroveň sítě, který také znamená, že je lhostejné toohello jazyk používaný podle vaší aplikace a hello technologie, která používá koncový bod hello
- dá se použít tooprovide přístupu ve více sítí z jedné aplikace 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Kroky, které nelze provést s hybridní připojení ###

Existuje několik možností, nemůžete dělat s hybridní připojení a patří mezi ně:

- připojení na jednotku
- pomocí protokolu UDP
- přístup k protokolu TCP na základě služby, které používají dynamické porty třeba pasivní režim FTP nebo rozšířený pasivní režim
- Podporu protokolu LDAP, jako je někdy vyžaduje UDP
- Podpora služby Active Directory

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Přidávání a vytváření hybridní připojení v aplikaci ##

Hybridní připojení lze vytvořit prostřednictvím portálu aplikace hello nebo z portálu služby hello hybridní připojení.  Důrazně doporučujeme použít hello aplikace portálu toocreate hello hybridní připojení chcete toouse s vaší aplikací.  toocreate hybridní připojení přejděte toohello [portál Azure] [ portal] a přejděte do hello uživatelského rozhraní pro vaši aplikaci.  Vyberte **sítě > Konfigurovat koncové body hybridního připojení**.  Zde vidíte hello hybridní připojení, které jsou nakonfigurované s vaší aplikací  

![][2]

tooadd hybridní připojení, klikněte na tlačítko Přidat hybridní připojení.  Hello uživatelského rozhraní, které se otevře uvádí hello hybridní připojení, který jste už vytvořili.  tooadd minimálně jeden z nich tooyour aplikace, klikněte na hello ty, které jsou vhodné a stiskněte tlačítko **přidat vybrané hybridní připojení**.  

![][3]

Pokud chcete, aby toocreate nové hybridní připojení, klikněte na tlačítko **vytvořit nové hybridní připojení**.  Tady zadáte: 

- název koncového bodu
- hostitele koncového bodu
- koncový port
- obor názvů sběrnice chcete toouse

![][4]

Každý hybridní připojení je obor názvů sběrnice vázanou tooa a každý obor názvů sběrnice je v oblasti Azure.  Je důležité, že tootry a používání oboru názvů ve sběrnici služby hello stejné oblasti jako vaše aplikace, jako latence sítě vyvolané tooavoid.

Pokud chcete tooremove hybridní připojení z vaší aplikace, klikněte na něj pravým tlačítkem a vyberte **odpojení**.  

Jakmile hybridní připojení se přidá tooyour webové aplikace, uvidíte na něm podrobnosti jednoduše kliknutím na něm.  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a>Hybridní připojení a plány služby App Service ##

Hello pouze hybridní připojení, které můžete provést nyní jsou hello nové hybridní připojení.  Jsou k dispozici v Basic, Standard, Premium a izolovaná ceny SKU.  Existují omezení svázané toohello ceny plánu.  

| Ceny plán | Počet použitelné v plánu hello hybridní připojení |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Vzhledem k tomu, že existují omezení plán služby App Service je zde také uživatelského rozhraní v hello plán služby App Service, ukazuje, kolik hybridní připojení se používají a které aplikace.  

![][6]

Stejně jako s hello aplikace zobrazení, se zobrazí podrobnosti na hybridní připojení kliknutím na.  Ve vlastnostech hello zobrazeny zde můžete zobrazit všechny hello informace, které jste měli v zobrazení aplikace hello ale můžete také zjistit, kolik aplikací v hello jsou stejné plán služby App Service pomocí této hybridní připojení.

![][7]

Zatímco je omezený počet hello koncové body hybridního připojení, které mohou být používány plánu služby App Service, každý hybridní připojení používají lze napříč libovolný počet aplikací v tomto plánu služby App Service.  To je toosay, pokud došlo k 1 hybridní připojení, který lze použít v 5 samostatných aplikací my plán služby App Service, která je stále právě 1 hybridní připojení.

Je připojení toohybrid dalších poplatků nad rámec se dá se použít v Basic, Standard, Premium nebo izolované SKU.  Podrobnosti o cenách pro hybridní připojení prosím naleznete zde: [ceny služby Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Správce hybridního připojení ##

V pořadí pro hybridní připojení toowork musíte přenosového agenta v síti hello, který je hostitelem váš koncový bod hybridní připojení.  Tento přenosový agent se nazývá hello hybridní připojení správce (HCM).  Tento nástroj můžete stáhnout z hello **sítě > Konfigurovat koncové body hybridního připojení** uživatelského rozhraní, které jsou k dispozici z vaší aplikace v hello [portál Azure][portal].  

Tento nástroj běží na systému Windows server 2008 R2 a novějších verzích systému Windows.  Po instalaci hello HCM spuštěn jako služba.  Tato služba připojí předávání sběrnice tooAzure podle hello nakonfigurované koncové body.  Hello připojení z hello HCM jsou odchozí tooports 80 a 443.    

Hello HCM má uživatelské rozhraní tooconfigure ho.  Po hello je nainstalována HCM můžete zprovoznit hello uživatelského rozhraní, spuštěním hello HybridConnectionManagerUi.exe která se nachází v instalačním adresáři hello správce hybridního připojení.  Je také snadno dosáhla ve Windows 10 zadáním *uživatelského rozhraní správce hybridního připojení* vyhledávacího pole.  

Při spuštění uživatelského rozhraní HCM hello hello nejprve thing jste je viz tabulku, která obsahuje seznam všech hello hybridní připojení, které jsou nakonfigurovány k této instanci hello HCM.  Pokud chcete toomake změny budete potřebovat tooauthenticate s Azure. 

tooadd jeden nebo více hybridní připojení tooyour HCM:

1. Spustit hello HCM uživatelského rozhraní
1. Klikněte na tlačítko Konfigurovat další hybridní připojení![][8]

1. Přihlaste se pomocí účtu Azure
1. Zvolit předplatné
1. Klikněte na hello hybridní připojení chcete tento toorelay HCM![][9]

1. Kliknutí na Uložit

V tomto okamžiku se zobrazí hello hybridní připojení, které jste přidali.  Můžete také kliknutím na hello nakonfigurované hybridní připojení a zobrazit podrobnosti o hello připojení.

![][10]

HCM toobe možné toosupport hello hybridní připojení, které je nakonfigurované musí:

- TCP tooAzure přístup přes porty 80 a 443
- Koncový bod TCP přístup toohello hybridní připojení
- možnost toodo DNS vzhled ups v hostiteli hello koncový bod a obor názvů sběrnice azure hello

Hello HCM podporuje nové hybridní připojení jak hello starší BizTalk hybridní připojení.

### <a name="redundancy"></a>Redundance ###

Každý HCM může podporovat víc hybridní připojení.  Jakékoli dané hybridní připojení může také podporovat více HCMs.  Hello výchozí chování je, že tooround každý s každým provoz mezi hello nakonfigurovaná HCMs pro libovolný koncový bod dané.  Pokud chcete vysoké dostupnosti na hybridní připojení z vaší sítě, jednoduše vytvoří instanci více HCMs na samostatných počítačích. 

### <a name="manually-adding-a-hybrid-connection"></a>Ručně přidejte hybridní připojení ###

Pokud chcete někomu mimo vaše předplatné toohost HCM instance pro danou hybridní připojení, můžete sdílet s nimi hello připojovací řetězec brány pro hello hybridní připojení.  Můžete to vidět ve vlastnostech hello pro hybridní připojení v hello [portál Azure][portal]. toouse, který řetězce, klikněte na tlačítko hello **nakonfigurovat ručně** tlačítka na hello HCM a vložte připojovací řetězec hello brány.


## <a name="troubleshooting"></a>Řešení potíží ##

Při hybridní připojení nastavená s běžící aplikací a je alespoň jeden HCM, která má toto hybridní připojení nakonfigurovaná a potom se dozvíte hello stav **připojeno** hello portálu.  Pokud není vyslovení **připojeno** pak to znamená, že buď aplikace je mimo provoz nebo vaše HCM se nemůže připojit se tooAzure na porty 80 nebo 443.  

Hlavním důvodem Hello, klienti se nemohou připojit tootheir koncový bod je, protože koncový bod hello byl zadán pomocí IP adresy místo názvu DNS.  Pokud vaše aplikace nebudou moci připojit hello požadovaného koncového bodu a použít IP adresy, přepínače toousing název DNS, který je platný na hostiteli hello hello HCM se spuštěným systémem.  Jiných věcí toocheck se, že tento hello název DNS správně přeloží na hostiteli hello kde hello HCM běží a zda je připojení z hostitele hello hello HCM se spuštěným systémem koncového bodu toohello hybridní připojení.  

V App Service, která se může vyvolat z konzoly hello názvem tcpping hello je nástroj.  Tento nástroj vám sdělí, pokud máte koncový bod TCP tooa přístup, ale neobsahuje žádné informace můžete Pokud máte přístup tooa hybridní připojení koncového bodu.  Pokud se použije v konzole hello koncový bod hybridní připojení, úspěšný příkaz ping pouze zjistíte, že máte hybridní připojení nakonfigurovaná pro vaši aplikaci, která používá tuto kombinaci hostitele a portu.  

## <a name="biztalk-hybrid-connections"></a>Hybridní připojení BizTalk ##

Hello starší schopností BizTalk hybridní připojení bylo ukončeno, vypněte vytváření toofurther BizTalk hybridní připojení.  Můžete pokračovat pomocí dříve existující připojení BizTalk hybridní aplikace, ale by bylo nutné migrovat toohello novou službu.  Mezi hello výhody v nové služby hello přes hello BizTalk verze jsou:

- není třeba žádné další účet BizTalk
- Je protokol TLS 1.2 místo 1.0 jako BizTalk hybridní připojení
- Komunikace je přes porty 80 a 443 pomocí tooreach název DNS Azure místo IP adresy a řadu dalších další porty.  

tooadd BizTalk hybridní připojení tooyour aplikace, přejděte tooyour aplikace v hello [portál Azure] [ portal] a klikněte na tlačítko **sítě > Konfigurovat koncové body hybridního připojení**.  V tabulce připojení hybridní Classic hello klikněte na tlačítko **přidat classic hybridní připojení**.  Zde uvidíte seznam BizTalk hybridní připojení.  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/

