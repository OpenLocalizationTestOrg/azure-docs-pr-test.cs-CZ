---
title: "aaaSet až nepřetržité integrace pro Azure mikroslužeb | Microsoft Docs"
description: "Získat přehled o tooset průběžnou integraci a nasazení pro aplikace Service Fabric pomocí Visual Studio Team Services (VSTS)."
services: service-fabric
documentationcenter: na
author: mthalman-msft
manager: timlt
editor: 
ms.assetid: 3e8c2290-9e7a-456a-9b2c-db44d1b3988d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2016
ms.author: mthalman;mikhegn
ms.openlocfilehash: 041e081f4a42f379394f2d21f07db4f6de045b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-service-fabric-continuous-integration-and-deployment-with-visual-studio-team-services"></a>Nastavit Service Fabric průběžnou integraci a nasazení s Visual Studio Team Services
Tento článek popisuje kroky tooset hello průběžnou integraci a nasazení pro aplikaci Azure Service Fabric pomocí Visual Studio Team Services (VSTS).

Tento dokument odráží aktuální procedury hello a je očekávané toochange v čase.

## <a name="prerequisites"></a>Požadavky
tooget spustili, postupujte takto:

1. Zajistěte, abyste měli přístup k účtu Team Services tooa nebo [vytvořit](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) sami.
2. Zajistěte, abyste měli přístup tooa Team Services týmového projektu nebo [vytvořit](https://www.visualstudio.com/docs/setup-admin/create-team-project) sami.
3. Ujistěte se, že máte cluster toowhich Service Fabric, můžete nasadit aplikace nebo vytvořit pomocí hello [portál Azure](service-fabric-cluster-creation-via-portal.md), [šablony Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md), nebo [Visual Studio ](service-fabric-cluster-creation-via-visual-studio.md).
4. Ujistěte se, že jste již vytvořili projekt aplikace Service Fabric (.sfproj). Projekt, který byl vytvořen nebo upgradovat s Service Fabric SDK 2.1 nebo vyšší musí mít (hello .sfproj soubor by měl obsahovat hodnotu vlastnosti ProjectVersion 1.1 nebo vyšší).

> [!NOTE]
> Vlastní sestavovací agentů se už nevyžadují. Týmové služby hostované agentů je dodávána předinstalované software pro správu clusteru Service Fabric, povolení pro nasazení aplikací přímo z těchto agentů.
> 
> 

## <a name="configure-and-share-your-source-files"></a>Konfigurace a sdílet zdrojové soubory
Hello první věcí, kterou chcete toodo je připravit profil publikování pro použití hello procesu nasazení, který spouští v rámci Team Services.  profil publikování se Hello by měl být nakonfigurovaný tootarget hello clusteru, který jste připravili dříve:

1. Vyberte profil publikování v rámci projektu aplikace, že chcete toouse pro pracovní postup průběžnou integraci. Postupujte podle hello [publikovat pokyny](service-fabric-publish-app-remote-cluster.md) o toopublish vzdáleného clusteru služby aplikace tooa. Nepotřebujete ve skutečnosti toopublish aplikace ale. Můžete kliknout na hello **Uložit** hypertextový odkaz v hello dialogového okna publikování, když jste správně nakonfigurovali věcí.
2. Pokud chcete, aby vaše aplikace toobe upgradovat pro každé nasazení, který se vyskytuje v Team Services, budete chtít tooconfigure hello Publikovat profil tooenable upgradu. V hello dialogového okna použít stejné publikování v kroku 1, ujistěte se, že hello **upgradu hello aplikace** je zaškrtnuté políčko.  Další informace o [konfigurace dalších nastavení upgradu](service-fabric-visualstudio-configure-upgrade.md). Klikněte na tlačítko hello **Uložit** hypertextový odkaz toosave hello nastavení toohello profilu publikování.
3. Zkontrolujte, zda jste uložili změny toohello Publikovat profil a zrušit hello dialogového okna publikování.
4. Nyní je čas příliš[sdílet zdrojové soubory vašeho projektu aplikace](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) s Team Services. Jakmile zdrojové soubory jsou přístupné v Team Services, nyní se můžete přesunout na další krok toohello generování sestavení. 

## <a name="create-a-build-definition"></a>Vytvořit definici sestavení
Definice sestavení Team Services popisuje pracovní postup, který se skládá ze sady kroky sestavení, které jsou prováděny postupně. cílem Hello hello definice sestavení, který vytváříte je tooproduce k balíčku aplikace Service Fabric a artefaktů, které se dají použít toodeploy hello aplikace. Další informace o Team Services [sestavení definice](https://www.visualstudio.com/docs/build/define/create).

### <a name="create-a-definition-from-hello-build-template"></a>Vytvoření definice z šablony sestavení hello
1. Otevřete váš týmový projekt v sadě Visual Studio Team Services.
2. Vyberte hello **sestavení** kartě.
3. Vyberte hello zelená  **+**  přihlásit toocreate nové definice buildu.
4. V dialogovém okně hello, které se otevře, vyberte **aplikace Azure Service Fabric** v rámci hello **sestavení** kategorie šablony.
5. Vyberte **Další**.
6. Vyberte úložiště hello a větve přidružená aplikace Service Fabric.
7. Vyberte fronty agenta hello chcete toouse. Hostovaní agenti jsou podporovány.
8. Vyberte **Vytvořit**.
9. Uložit definici sestavení hello a zadejte název.
10. Hello následující odstavce je uveden popis kroků sestavení hello generované hello šablony:

| Krok sestavení | Popis |
| --- | --- |
| Obnovení NuGet |Obnoví hello balíčků NuGet pro řešení hello. |
| Vytvoření řešení \*.sln |Sestaví hello celé řešení. |
| Vytvoření řešení \*.sfproj |Generuje hello balíček aplikace Service Fabric, který je použité toodeploy hello aplikace. umístění balíčku aplikace Hello je zadaný toobe v adresáři artefaktů sestavení hello. |
| Aktualizace verze aplikace Service Fabric |Aktualizace hodnoty verze hello obsažené v balíčku aplikace hello souborů manifestu tooallow podpora upgradu. V tématu hello [stránky dokumentace, která úloha](https://go.microsoft.com/fwlink/?LinkId=820529) Další informace. |
| Kopírování souborů |Kopie hello Publikovat profil a aplikace parametry soubory toohello sestavení na artefakty toobe využité pro nasazení. |
| Publikování artefaktů |Publikuje hello sestavení artefaktů. Umožňuje definici verze sestavení hello tooconsume artefakty. |

### <a name="verify-hello-default-set-of-tasks"></a>Ověřte hello výchozí sadu úloh
1. Ověřte hello **řešení** vstupní pole pro hello **obnovení NuGet** a **sestavení řešení** kroky sestavení.  Ve výchozím nastavení, tyto kroky sestavení spuštění na všechny soubory řešení, které jsou obsaženy v úložišti hello přidružené.  Pokud chcete pouze toooperate definice sestavení hello na jednom z těchto souborů řešení, budete potřebovat tooexplicitly aktualizace hello cesta toothat soubor.
2. Ověřte hello **řešení** vstupní pole pro hello **balíčku aplikace** kroku sestavení.  Ve výchozím nastavení tento krok sestavení předpokládá, že existuje jenom jedna aplikace Service Fabric projektu (.sfproj) v úložišti hello.  Pokud máte více tyto soubory do úložiště a chcete tootarget, pouze jedna je pro tuto definici sestavení, budete potřebovat tooexplicitly aktualizace hello cesta toothat soubor.  Pokud chcete ve svém úložišti toopackage projekty více aplikací, je nutné toocreate Další **Visual Studio sestavení** kroky v definici sestavení Dobrý den, každý cílí projekt aplikace.  Pak také potřebujete tooupdate hello **argumenty MSBuild** pole pro každý z nich kroky sestavení tak, aby hello umístění balíčku je jedinečný pro každý z nich.
3. Ověřte hello Správa verzí chování definované v hello **verze aktualizace Service Fabric aplikace** kroku sestavení.  Ve výchozím nastavení tento krok sestavení připojí hello sestavení číslo tooall hodnoty verze v balíčku aplikace hello souborů manifestu. V tématu hello [stránky dokumentace, která úloha](https://go.microsoft.com/fwlink/?LinkId=820529) Další informace. To je užitečné pro podporu upgradu vaší aplikace, vzhledem k tomu, že každé nasazení upgradu vyžaduje jinou verzi hodnoty z předchozích nasazení hello. Pokud ve svém pracovním postupu nejsou záměrné upgradu aplikace toouse, zvažte zákaz tento krok sestavení. Musí být zakázáno, má-li tooproduce sestavení, které můžou být použité toooverwrite stávající aplikace Service Fabric. Hello nasazení selže, pokud hello verze aplikace hello vyprodukované hello sestavení neodpovídá verzi hello hello aplikace v clusteru hello.
4. Pokud vaše řešení obsahuje projektu .NET Core, je nutné zajistit, že vaše definice sestavení obsahuje krok sestavení, která obnoví hello závislosti:
   1. Vyberte **krok sestavení přidat...** .
   2. Vyhledejte hello **příkazového řádku** v rámci kartu nástroj hello a klikněte na jeho tlačítko Přidat.
   3. Pro úlohu hello vstupní pole, použijte hello následující hodnoty:
   4. Nástroj: dotnet.
   5. Argumenty: obnovení
   6. Přetáhněte hello úloh tak, aby se okamžitě po hello **obnovení NuGet** krok.
5. Uložte změny, které jste udělali toohello definici sestavení.

### <a name="try-it"></a>Vyzkoušet
Vyberte **fronty sestavení** toomanually spuštění sestavení. Vytvoří také aktivační události nabízené nebo vrácení se změnami. Jakmile se ujistíte, že sestavení hello úspěšně spouští, nyní se můžete přesunout na toodefining definici verze, která nasadí cluster tooa aplikace.

## <a name="create-a-release-definition"></a>Vytvoření definice verze
Verze definice Team Services popisuje pracovní postup, který se skládá z sadu úloh, které jsou prováděny postupně. cílem Hello hello verze definice, které vytváříte je tootake balíček aplikace a nasaďte ji tooa clusteru. Při použití společně, hello sestavení definice a definice verzi můžete provést celý pracovní postup hello z počínaje tooending zdrojové soubory s běžící aplikací v clusteru. Další informace o Team Services [verze definice](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-definition-from-hello-release-template"></a>Vytvoření definice z šablony verze hello
1. Otevřete projekt v sadě Visual Studio Team Services.
2. Vyberte hello **verze** kartě.
3. Vyberte hello zelená  **+**  přihlásit toocreate novou definici verzí a vybrat **vytvořit verze definice** v nabídce hello.
4. V dialogovém okně hello, které se otevře, vyberte **Azure Service Fabric nasazení** v rámci hello **nasazení** kategorie šablony.
5. Vyberte **Další**.
6. Vyberte definici sestavení hello toouse chcete jako zdroj hello této verze definice.  Hello verze definice odkazy hello artefakty vydaných společností hello vybrané sestavení definice.
7. Zkontrolujte hello **průběžné nasazování** zaškrtávací políčko, pokud chcete toohave Team Services automaticky vytvořit novou verzi a nasadit aplikace Service Fabric hello vždy, když sestavení se dokončí.
8. Vyberte fronty agenta hello chcete toouse. Hostovaní agenti jsou podporovány.
9. Vyberte **Vytvořit**.
10. Název definice hello upravte kliknutím na ikonu tužky hello hello horní části stránky hello.
11. Vyberte toowhich hello clusteru musí být nasazené aplikace hello **připojení clusteru** vstupní pole hello úlohy. připojení clusteru Hello poskytuje hello potřebné informace, které umožňuje hello nasazení úloh tooconnect toohello clusteru. Pokud ještě nemáte připojení clusteru pro cluster, vyberte hello **spravovat** hyperlink další toohello pole tooadd jeden. Na stránce hello, které se otevře proveďte následující kroky hello:
    1. Vyberte **nový koncový bod služby** a pak vyberte **Azure Service Fabric** nabídce hello.
    2. Vyberte typ ověřování používá cílem tento koncový bod clusteru hello hello.
    3. Definujte název připojení v hello **název připojení** pole.  Obvykle byste použili hello názvem vašeho clusteru.
    4. Definování hello adresu URL koncového bodu připojení klienta v hello **koncový bod clusteru** pole.  Příklad: tcp://contoso.westus.cloudapp.azure.com:19000.
    5. U přihlašovacích údajů Azure Active Directory, definovat hello pověření, které chcete cluster toohello tooconnect toouse v hello **uživatelské jméno** a **heslo** pole.
    6. Pro ověřování pomocí certifikátu, zadejte hello Base64 kódování souboru certifikátu klienta hello v hello **klientský certifikát** pole.  Zobrazit automaticky otevírané okno nápovědy hello na informace o tom, že pole tooget, která hodnota.  Pokud váš certifikát je chráněný heslem, zadejte heslo hello v hello **heslo** pole.
    7. Potvrďte změny kliknutím **OK**. Po přechodu pozadí tooyour verze definice, klikněte na ikonu aktualizace hello na hello **připojení clusteru** pole toosee hello koncový bod jste právě přidali.
12. Uložte definici verze hello.

> [!NOTE]
> Accounts Microsoft (například @hotmail.com nebo @outlook.com) nejsou podporovány pomocí ověřování Azure Active Directory.
> 
> 

definice Hello, který se vytvoří se skládá z úloh typu **nasazení aplikace Service Fabric**. V tématu hello [stránky dokumentace, která úloha](https://go.microsoft.com/fwlink/?LinkId=820528) Další informace o této úloze.

### <a name="verify-hello-template-defaults"></a>Zkontrolujte výchozí nastavení šablony hello
1. Ověřte hello **profil publikování** vstupní pole pro hello **nasazení aplikace Service Fabric** úloh. Ve výchozím nastavení odkazuje na toto pole s názvem Cloud.xml obsažené v hello sestavení artefaktů profil publikování. Pokud chcete, aby tooreference profil různých publikování nebo hello sestavení obsahuje několik balíčků aplikace v některé jeho artefakty, tooupdate hello cestu je třeba správně.
2. Ověřte hello **balíčku aplikace** vstupní pole pro hello **nasazení aplikace Service Fabric** úloh. Ve výchozím nastavení v tomto poli odkazuje hello výchozí aplikace balíčku cestu použitou v šablony definice sestavení hello.  Pokud jste změnili hello výchozí cesta balíčku aplikace v hello sestavení definici, musíte tooupdate hello cesta správně zde také.

### <a name="try-it"></a>Vyzkoušet
Vyberte **vytvořit verzi** z hello **verze** tlačítko nabídky toomanually vytvořit verzi. V dialogovém okně hello, které se otevře, vyberte hello build, který chcete toobase hello verze na a pak klikněte na tlačítko **vytvořit**. Pokud jste povolili průběžné nasazování, verze vytvoří automaticky po dokončení definice sestavení hello přidružené sestavení.

## <a name="next-steps"></a>Další kroky
Další informace o průběžnou integraci s aplikací Service Fabric, přečtěte si následující články hello toolearn:

* [Dokumentace služby Team domácí](https://www.visualstudio.com/docs/overview)
* [Sestavení správy v nástroji Team Services](https://www.visualstudio.com/docs/build/overview)
* [Správy verzí v Team Services](https://www.visualstudio.com/docs/release/overview)

