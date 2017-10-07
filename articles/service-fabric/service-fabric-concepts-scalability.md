---
title: "aaaScalability služby Service Fabric | Microsoft Docs"
description: "Popisuje, jak tooscale služby Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: ed324f23-242f-47b7-af1a-e55c839e7d5d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5af06f8f71ad5dee32ba115b922842684867e654
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-in-service-fabric"></a>Nastavení velikosti v Service Fabric
Azure Service Fabric umožňuje snadno toobuild škálovatelné aplikace pomocí správy služeb hello, oddíly a repliky na hello uzlů clusteru. Řadu úloh systémem hello stejný hardware umožňuje maximální prostředků využití, ale také poskytuje flexibilitu z hlediska volbu tooscale úlohy. 

Nastavení velikosti v Service Fabric se provádí několika různými způsoby:

1. Škálování vytvořením nebo odebrání instance bezstavové služby
2. Škálování podle vytváření nebo odebírání nové služby s názvem
3. Škálování podle vytváření nebo odebírání nové pojmenované instance aplikace
4. Škálování pomocí oddílů služby
5. Škálování podle přidávání a odebírání uzlů z clusteru hello 
6. Pomocí Správce prostředků clusteru metriky škálování

## <a name="scaling-by-creating-or-removing-stateless-service-instances"></a>Škálování vytvořením nebo odebrání instance bezstavové služby
Jedním z nejjednodušší tooscale způsoby hello v Service Fabric funguje s bezstavové služby. Když vytvoříte bezstavové služby, získáte toodefine příležitosti `InstanceCount`. `InstanceCount`Určuje, kolik kopií spuštěného kódu této služby se vytvoří při spuštění služby hello. Řekněme například, že jsou 100 uzly v clusteru hello. Dále předpokládejme, že je služba vytvořena s `InstanceCount` 10. Během doby běhu tyto 10 spuštěná kopie hello kódu by se mohly všechny stát příliš zaneprázdněn (nebo může není dostatečně zaneprázdněn). Jedním ze způsobů tooscale této úlohy je toochange hello počet instancí. Některá část kódu, monitorování nebo správu můžete například změnit hello existující počet instancí too50 nebo too5, v závislosti na tom, jestli zatížení hello musí tooscale příchozí nebo odchozí na základě hello načíst. 

C#:

```csharp
StatelessServiceUpdateDescription updateDescription = new StatelessServiceUpdateDescription(); 
updateDescription.InstanceCount = 50;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

Prostředí PowerShell:

```posh
Update-ServiceFabricService -Stateless -ServiceName $serviceName -InstanceCount 50
```
### <a name="using-dynamic-instance-count"></a>Použití dynamické počet instancí
Konkrétně pro bezstavové služby Service Fabric nabízí počet instancí automatické způsob toochange hello. To umožňuje hello služby tooscale dynamicky s hello počet uzlů, které jsou k dispozici. Hello způsob tooopt do toto chování je počet instancí hello tooset = -1. InstanceCount = -1 je instrukce tooService Fabric, která říká "Spustit tento bezstavové služby na všech uzlech." Pokud se změní hello počet uzlů, Service Fabric automaticky změní hello instance počet toomatch, zajistíte, že služba hello běží na všech uzlech, platný. 

C#:

```csharp
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//Set other service properties necessary for creation....
serviceDescription.InstanceCount = -1;
await fc.ServiceManager.CreateServiceAsync(serviceDescription);
```

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateless -PartitionSchemeSingleton -InstanceCount "-1"
```

## <a name="scaling-by-creating-or-removing-new-named-services"></a>Škálování podle vytváření nebo odebírání nové služby s názvem
Instance s názvem služby je konkrétní instanci typu služby (viz [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md)) v rámci některé instance s názvem aplikace v clusteru hello. 

Nové služby s názvem instance může vytvořit (nebo odstranit) jako služby stát více nebo méně zaneprázdněn. To umožňuje rozloženy více instancí služby, obvykle umožňuje zatížení na stávající služby toodecrease toobe požadavky. Při vytváření služby, umístí hello správce prostředků clusteru Service Fabric hello služby clusteru hello distribuované způsobem. Hello přesný rozhodnutí, která se řídí hello [metriky](service-fabric-cluster-resource-manager-metrics.md) v clusteru hello a další pravidla pro umístění. Služby můžete vytvořit několik různých způsobů, ale hello nejčastějším jsou buď prostřednictvím akce správy, jako je někdo volání [ `New-ServiceFabricService` ](https://docs.microsoft.com/en-us/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps), nebo kód volání [ `CreateServiceAsync` ](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync?view=azure-dotnet). `CreateServiceAsync`Můžete dokonce volat z v rámci jiných služeb spuštěných v clusteru hello.

Vytváření služby dynamicky mohou být používány nejrůznějším scénářům a je běžný vzor. Představte si třeba stavové služby, který představuje určitý pracovní postup. Volání představující pracovní budou tooshow až toothis služby a tato služba je probíhající tooexecute hello kroky toothat pracovního postupu a záznamů průběhu. 

By se tak tohoto měřítka konkrétní službu? Hello služba může být více klientů v některé formuláře a přijímat volání a ji kroky pro mnoho různých instancí hello stejný pracovní postup všechny najednou. Ale který provádět hello kód složitější, protože teď má tooworry o mnoha různými instancemi hello témže pracovním postupu, všechny v různých fázích a z různých zákazníků. Navíc zpracování více pracovních postupů v hello současně nevyřeší hello škálování problém. Je to proto, že v určitém okamžiku bude tuto službu využívat příliš mnoho prostředků toofit na konkrétní počítač. Mnoho služeb není vytvořené pro tento vzor na prvním místě hello taky nastat potíže z důvodu vyplývajících problémové místo toosome nebo zpomalení v svůj kód. Tyto typy problémů způsobit hello služby není toowork také při hello počet souběžných pracovních postupů, které je sledování získá větší.  

Řešení je toocreate instance této služby pro všechny různé instance pracovního postupu hello chcete tootrack. Toto je skvělým vzor a funguje, zda je služba hello bezstavové nebo stateful. Pro tento vzor toowork je obvykle jiné službě, která funguje jako "Služba Správce úloh". Úloha Hello této služby je tooreceive požadavky a tooroute tyto požadavky tooother služby. Hello manager můžete dynamicky vytvoření instance služby hello zatížení při přijetí uvítací zprávu a pak předejte na požadavky toothose služby. Služba Správce Hello může také přijímat zpětná volání, po dokončení úlohy služby daného pracovního procesu. Když správce hello přijme tyto zpětná volání ho může odstranit tuto instanci služby pracovního postupu hello nebo necháte, pokud se očekává více volání. 

Pokročilé verze tohoto typu správce můžete dokonce vytvářet fondy hello služeb, které spravuje. fond Hello pomáhá zajistit, že při přijetí nového požadavku nemá toowait pro hello služby toospin. Místo toho hello manager můžete právě vyberte služby pracovního postupu, který není aktuálně zaneprázdněn z fondu hello nebo náhodně trasy. Zachování fond dostupnost služeb díky nové žádosti o zpracování rychlejší, vzhledem k tomu, že je méně pravděpodobné, že tento požadavek hello má toowait pro nové toobe služby spuštěné. Vytvoření nové služby je rychlý, ale není volné nebo okamžitou. fond Hello pomáhá minimalizovat hello množství času hello žádost obsahuje toowait před probíhá údržba. Tento vzor manager a fondu se často zobrazí, když odezvy hello vás zajímají. Žádost o služby Řízení front hello a vytváření služby hello hello pozadí a _pak_ předávání je také oblíbených manager vzor, jako je vytváření a odstraňování služeb aplikace založené na některé sledování hello množství práce, že služba aktuálně má čekající na vyřízení. 

## <a name="scaling-by-creating-or-removing-new-named-application-instances"></a>Škálování podle vytváření nebo odebírání nové pojmenované instance aplikace
Vytváření a odstraňování instancí celou aplikaci je podobný Princip toohello při vytváření a odstraňování služeb aplikace. Pro tento vzor je některé manager service, která je rozhodování hello podle hello požadavků, které se zobrazuje a hello hello informace, které přijímá od jiných služeb uvnitř hello clusteru. 

Při vytvoření nové instance s názvem aplikace použít místo vytvoření nové instance s názvem služby v některé stávající aplikaci? Existuje několik případů:

  * novou instanci aplikace Hello je pro zákazníka, jejíž kód potřebuje toorun pod některé konkrétní identity nebo nastavení zabezpečení.
    * Service Fabric umožňuje definovat toorun balíčky různý kód v rámci konkrétní identity. V pořadí toolaunch hello stejného balíčku kódu pod různými identitami, hello aktivací nutné toooccur v případech jinou aplikaci. Představte si případ, kdy máte stávající zákazník úlohy nasazené. Tyto může být spuštěna konkrétní identitou, takže můžete sledovat a řídit tooother jejich přístup k prostředkům například vzdálené databáze nebo jiných systémů. V takovém případě pokud nové zákazník zaregistruje, pravděpodobně nechcete, aby tooactivate svůj kód v hello stejný kontext (proces místa). Když vám může tento ztěžuje tooact kódu vaší služby v rámci kontextu hello konkrétní identity. Obvykle musíte mít další zabezpečení, izolace a kódu správy identit. Místo použití různých služby s názvem instance v hello stejnou instanci aplikace a proto hello stejnému adresnímu prostoru procesu, můžete použít různé instance s názvem aplikace Service Fabric. Díky tomu je snazší kontexty toodefine jinou identitu.
  * novou instanci aplikace Hello slouží taky jako způsob konfigurace
    * Ve výchozím nastavení všechny hello s názvem instance služby typu konkrétní služby v rámci instance aplikace poběží v hello stejný proces v daném uzlu. To znamená, že každá instance služby nakonfigurovat různě, takový postup je složitá. Musí mít některé token používají toolook si své konfigurace v rámci konfigurace balíčku. Obvykle je to právě hello služby název. To funguje bez problémů, ale jeho páry v odstupu hello konfigurace toohello názvy instancí hello jednotlivých s názvem služby v rámci této instance aplikace. To může být matoucí a pevné toomanage od konfigurace je obvykle artefaktem návrhu času s konkrétními hodnotami instanci aplikace. Vytvoření další služby vždy znamená více upgradů aplikací toochange hello informace v rámci hello balíčky konfigurace nebo toodeploy nové tak, aby hello nové služby můžete vyhledat jejich konkrétní informace. Často je snazší toocreate celý novou instanci s názvem aplikace. Pak můžete použít tooset parametry aplikace hello ať konfigurace je nezbytné pro služby hello. Tímto způsobem všechny hello služby, které jsou vytvořené v rámci které pojmenovanou instanci aplikace lze dědit nastavení konkrétní konfigurace. Například místo nutnosti jednom konfiguračním souboru s nastavením hello a vlastní nastavení pro každou zákazníka, jako je například tajných klíčů nebo za zákazníka omezení prostředků, byste místo toho máte instanci jinou aplikaci pro každého zákazníka a to s těmito nastaveními přepsat. 
  * Nová aplikace Hello slouží jako hranici upgradu
    * V rámci Service Fabric jinou aplikaci s názvem instance slouží jako hranice pro upgrade. Upgrade jedné instance s názvem aplikace nemá vliv hello kód, který je spuštěna jiná instance s názvem aplikace. Hello různé aplikace dojdete k s různými verzemi hello stejný kód na hello stejným uzlům. To může být faktorem, když potřebujete toomake škálování rozhodnutí, protože můžete vybrat, že zda nový kód hello postupujte podle hello stejné upgraduje jako jiná služba nebo ne. Řekněme například, že volání dorazí na hello manager service, která je odpovědná za vytváření a odstraňování služeb aplikace dynamicky škálování úloh konkrétního zákazníka. V takovém případě však hello volání je pro zatížení přidružené _nové_ zákazníka. Většina zákazníků jako navzájem izolované nejen z důvodů zabezpečení a konfigurace hello uvedených výše, ale protože poskytuje větší flexibilitu z hlediska konkrétní verzemi softwaru hello a výběr při jejich získat upgradovat. Můžete také vytvořit novou instanci aplikace a vytvoření služby hello existuje jednoduše toofurther oddílu hello množství vaší služby, které bude touch jakéhokoli jeden upgradu. Instance samostatné aplikace poskytují větší členitost při provádění upgradu aplikace a také povolit A / B testování a modrá nebo zelená nasazení. 
  * existující instance aplikace Hello je plná
    * V Service Fabric [kapacity aplikace](service-fabric-cluster-resource-manager-application-groups.md) je koncept používané toocontrol hello objem prostředků, které jsou k dispozici pro konkrétní aplikaci instance. Například může rozhodnout, že uvedená služba potřebuje toohave jiná instance vytvořené v tooscale pořadí. Tato instance aplikace je však mimo kapacity pro určité metriky. Pokud tento konkrétní zákazník nebo zatížení stále udělení více prostředků, pak můžete buď zvýšit hello existující kapacitu pro tuto aplikaci nebo vytvořte novou aplikaci. 

## <a name="scaling-at-hello-partition-level"></a>Škálování na úrovni oddílu hello
Dělení na oddíly podporuje Service Fabric. Vytváření oddílů rozdělí služby do několika oddílů logické a fyzické, z nichž každý pracuje nezávisle. To je užitečné s stavové služby, vzhledem k tomu, že žádná sada replik má toohandle všechna volání hello a všechny hello stavu najednou upravit. Hello [dělení přehled](service-fabric-concepts-partitioning.md) poskytuje informace o typech hello rozdělení schémat, které jsou podporovány. Hello repliky každý oddíl jsou rozloženy hello uzlů v clusteru, distribuci zatížení tuto službu a zajistíte, že služba ani hello jako celek nebo oddíl má jediný bod selhání. 

Zvažte službu, která používá ranged schéma rozdělení oddílů s nízkou klíč 0, vysoká hodnota klíče 99 a počet oddílů na 4. V tři uzly clusteru může být nastíněny hello služby s čtyři replikami, které sdílejí prostředky hello na každém uzlu, jak je vidět tady:

<center>
![Rozložení oddílů s tři uzly](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Pokud zvýšíte hello počet uzlů, Service Fabric přesune některé existující repliky hello existuje. Řekněme například, hello počet uzlů zvyšuje toofour a získat distribuovat hello repliky. Teď hello služby teď má tři repliky na jednotlivých uzlech spuštěné, každý patřící toodifferent oddíly. To umožňuje lepší využití prostředků, vzhledem k tomu, že není cold hello nový uzel. Obvykle se také zlepšuje výkon jako další prostředky k dispozici tooit má každá služba.

<center>
![Rozložení oddílů se čtyřmi uzly](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-by-using-hello-service-fabric-cluster-resource-manager-and-metrics"></a>Škálování pomocí hello správce prostředků clusteru Service Fabric a metriky
[Metriky](service-fabric-cluster-resource-manager-metrics.md) jsou jak služby express jejich tooService spotřeby prostředků infrastruktury. Pomocí metriky dává hello správce prostředků clusteru tooreorganize příležitost a optimalizovat hello rozložení hello clusteru. Například může být dost prostředků v clusteru hello, ale nemusí být přidělené toohello služby, které aktuálně pracujeme. Použití metriky hello tooreorganize clusteru Resource Manager umožňuje tooensure hello clusteru, aby služby měly přístup toohello dostupné prostředky. 


## <a name="scaling-by-adding-and-removing-nodes-from-hello-cluster"></a>Škálování podle přidávání a odebírání uzlů z clusteru hello 
Další možností škálování s Service Fabric je toochange hello velikost clusteru hello. Změna velikosti hello hello clusteru znamená přidávání a odebírání uzlů pro jeden nebo více typů hello uzlu v clusteru hello. Například Představte si případ, kde jsou všechny hello uzly v clusteru hello aktivní. To znamená, zda jsou prostředky clusteru hello téměř všechny využívat. V takovém případě přidání toohello další uzly clusteru je nejlepší způsob, jak tooscale hello. Po připojení hello nové uzly clusteru hello hello přesune správce prostředků clusteru Service Fabric toothem služby, výsledkem je menší celkové zatížení na uzlech existujícího hello. Pro bezstavové služby se počet instancí = -1, další služby, jsou automaticky vytvořeny instance. To umožňuje některé toomove volání z hello existující uzly toohello nové uzly. 

Přidávání a odebírání uzlů toohello clusteru se dá udělat pomocí modulu prostředí PowerShell služby infrastruktury Azure Resource Manager hello.

```posh
Add-AzureRmServiceFabricNode -ResourceGroupName $resourceGroupName -Name $clusterResourceName -NodeType $nodeTypeName  -NumberOfNodesToAdd 5 
Remove-AzureRmServiceFabricNode -ResourceGroupName $resourceGroupName -Name $clusterResourceName -NodeType $nodeTypeName -NumberOfNodesToRemove 5
```

## <a name="putting-it-all-together"></a>Třeba umisťovat všechny společně
Podívejme se na všechny hello nápady, které jsme probrali sem a komunikovat prostřednictvím příklad. Vezměte v úvahu hello následující služby: pokoušíte toobuild služba, která funguje jako adresáře, která uchovává na toonames a kontaktních informací. 

Právo předem máte spoustu otázky související tooscale: počet uživatelů, kteří jsou probíhající toohave? Kolik kontakty bude každý uživatel uložit? Při operaci toofigure to všechny odhlašování když jste se stálého instalace služby pro hello poprvé je obtížné. Řekněme, že budete pryč toogo jedinou statické službou s počtem konkrétního oddílu. Výběr hello nesprávný počet oddílů může způsobit, že jste toohave dopadech Hello později škálovat problémy. Podobně i v případě, že vyberete hello správné počet, který nemusí mít všechny informace o hello potřebujete. Například máte také toodecide hello velikost clusteru hello předem, jak z hlediska hello počet uzlů a jejich velikost. Obvykle pevný toopredict tom, kolik prostředků může služba se bude tooconsume přes celé jeho životnosti. Může být také pevný tooknow před čas hello provoz vzor, který ve skutečnosti uvidí hello služby. Uživatelé možná přidávat a odebírat jejich kontakty pouze první thing v hello ráno nebo možná ho jsou rovnoměrně průběhu hello hello den. Založené na to, které byste měli tooscale v a dynamicky. Možná další toopredict když budete tooneed tooscale v a, ale v obou případech pravděpodobně budete spotřeby prostředků toochanging tooreact tooneed vaší službou. To může zahrnovat Změna velikosti hello hello clusteru v pořadí tooprovide více prostředků při reorganizace použití existujících prostředků není dost. 

Ale proč i zkuste toopick jednoho oddílu schématu se pro všechny uživatele? Proč omezit sami tooone služby a jeden cluster statické? Hello skutečné situace je obvykle dynamičtější. 

Při sestavování škálování, zvažte následující dynamické vzor hello. Může být nutné tooadapt ho tooyour situaci:

1. Namísto pokusu o toopick schéma rozdělení oddílů pro každého předem, sestavení "manager service".
2. Úloha Hello služby Správce hello je toolook na informace o zákazníkovi, při registraci pro vaši službu. Pak v závislosti na tyto informace hello manager service vytvořit instanci vaše _skutečné_ kontaktujte úložiště služby _jen pro tohoto zákazníka_. Pokud vyžadují konkrétní konfigurací, izolaci nebo upgrady, můžete taky rozhodnout toospin do instance aplikace pro tohoto zákazníka. 

Tato dynamického vytváření vzor řadu výhod:

  - Nepokoušíte tooguess hello počet správné oddílů pro všechny uživatele předem nebo vytvořit jednu službu, která je nekonečnou škálovatelné všechny sama o sobě. 
  - Různí uživatelé nemají toohave hello stejné oddílu počet, počet replik, omezení umístění, metriky, výchozí zatížení, názvy služeb, nastavení dns nebo žádné hello ostatní vlastnosti zadané hello služby nebo aplikace. 
  - Získáte segmentace další data. Každý zákazník má své vlastní kopie hello služby
    - Každý zákazník služby můžete nakonfigurována jinak než a udělit více nebo méně prostředků s více nebo méně oddíly nebo repliky podle potřeby podle jejich očekávaný rozsah.
      - Řekněme například, hello zákazníka zaplacení hello "Zlatá" vrstvy – se může získat další repliky nebo větší počet oddílů a potenciálně prostředky vyhrazené tootheir služby prostřednictvím kapacity metriky a aplikace.
      - Nebo se, že zadané informace o tom, hello počet kontakty, které budou potřebné byl "Malá" – by získat pouze několik oddílů, nebo dokonce možné zařadit do fondu sdílené služby s ostatními zákazníky.
  - Spoustu instance služby nebo repliky nejsou spuštěné, zatímco čekáte pro zákazníky tooshow nahoru
  - Pokud zákazník někdy ponechá, pak odebrání své informace ze služby je jednoduše tak, že má hello správce odstranit této služby nebo aplikace, která ji vytvořila.

## <a name="next-steps"></a>Další kroky
Další informace o konceptech Service Fabric najdete v tématu hello následující články:

* [Dostupnost služeb Service Fabric](service-fabric-availability-services.md)
* [Vytváření oddílů služby Service Fabric](service-fabric-concepts-partitioning.md)
