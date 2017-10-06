---
title: "aaaHow toouse Hudsonem pomocí úložiště objektů Blob | Microsoft Docs"
description: "Popisuje, jak toouse Hudsonem s úložištěm Azure Blob jako úložiště pro artefaktů sestavení."
services: storage
documentationcenter: java
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 119becdd-72c4-4ade-a439-070233c1e1ac
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.openlocfilehash: 196b5d014b0318c5972a052f7822b568cfcc23df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Použití Azure Storage s řešením Hudson Continuous Integration
## <a name="overview"></a>Přehled
Hello následující informace ukazuje, jak používat úložiště objektů Blob toouse jako úložiště artefaktů sestavení vytvořené řešení Hudsonem nepřetržité integrace (CI) nebo jako zdroj toobe soubory ke stažení v procesu sestavení. Jeden z hello scénáře, kde jste by to užitečné je když jste kódování agilní vývojovém prostředí (pomocí Java nebo jiné jazyky), sestavení běží v závislosti na průběžnou integraci a potřebujete úložiště pro sestavení artefaktů, tak, aby vám může , například je sdílet se členy jiné organizace, vašim zákazníkům nebo udržujte archiv.  Další možností je při sestavení úlohu samotné vyžaduje další soubory, například závislosti toodownload jako součást hello sestavení vstup.

V tomto kurzu budete používat modul plug-in hello Azure Storage pro nepřetržitou Integraci Hudsonem zpřístupněné společností Microsoft.

## <a name="introduction-toohudson"></a>TooHudson Úvod
Hudsonem umožňuje průběžnou integraci projektu softwaru tím, že vývojáři tooeasily integrovat jejich změny kódu a mít sestavení vytvořené automaticky a často, a zvýšení produktivity hello hello vývojářů. Sestavení jsou verzí a artefaktů sestavení může být nahrané toovarious úložiště. Tento článek vám ukáže, jak vytvářet toouse úložiště objektů Blob v Azure jako úložiště hello hello artefakty. Zobrazí také jak toodownload závislosti z Azure Blob storage.

Další informace o Hudsonem lze najít na [splňovat Hudsonem](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-hello-blob-service"></a>Výhody použití služby Blob hello
Výhody používání toohost služby objektů Blob hello sestavení artefaktů agilní vývoj patří:

* Vysoká dostupnost artefaktů sestavení nebo ke stažení závislostí.
* Výkon, pokud je vaše řešení Hudsonem CI ukládání artefaktů sestavení.
* Výkon, pokud zákazníci a partneři stažení artefaktů sestavení.
* Ovládat zásady přístupu uživatelů s volbou mezi anonymní přístup, přístup na základě vypršení platnosti sdílený přístupový podpis, privátní přístup, atd.

## <a name="prerequisites"></a>Požadavky
Můžete se třeba hello následující toouse hello služby objektů Blob s vaším řešením Hudsonem CI:

* Průběžnou integraci Hudsonem řešení.
  
    Pokud aktuálně nemáte Hudsonem CI řešení, můžete spustit Hudsonem CI řešení pomocí hello následujícím způsobem:
  
  1. Na počítači povolené Java, stažení hello Hudsonem WAR z <http://hudson-ci.org/>.
  2. Na příkazovém řádku, který je otevřen toohello složku, která obsahuje hello Hudsonem WAR spusťte hello Hudsonem WAR. Například pokud jste si stáhli verzi 3.1.2:
     
      `java -jar hudson-3.1.2.war`

  3. V prohlížeči otevřete `http://localhost:8080/`. Otevře se hello Hudsonem řídicího panelu.
  4. Při prvním použití Hudsonem, dokončete počáteční instalace hello `http://localhost:8080/`.
  5. Po dokončení počáteční nastavení hello zrušit hello spuštěna instance hello Hudsonem WAR, spusťte znovu hello Hudsonem WAR a znovu ho otevřete hello Hudsonem řídicí panel, `http://localhost:8080/`, které budete používat tooinstall a nakonfigurovat modul plug-in Azure Storage hello.
     
      Při typických Hudsonem CI řešení by nastavit toorun jako služba, bude spuštěn hello Hudsonem war v příkazovém řádku hello dostatečná pro účely tohoto kurzu.
* Účet Azure. Můžete zaregistrovat k účtu Azure v <http://www.azure.com>.
* Účet úložiště Azure. Pokud nemáte účet úložiště, můžete vytvořit jeden postupem hello v [vytvořit účet úložiště](../common/storage-create-storage-account.md#create-a-storage-account).
* Znalost hello Hudsonem CI řešení je doporučená, ale není potřeba, protože hello následující obsah bude používat základní příklad tooshow, hello kroky potřebné při použití služby objektů Blob hello jako úložiště pro nepřetržitou Integraci Hudsonem sestavení artefaktů.

## <a name="how-toouse-hello-blob-service-with-hudson-ci"></a>Jak toouse hello služby objektů Blob s Hudsonem CI
toouse hello služby objektů Blob s Hudsonem, budete potřebovat tooinstall hello modul plug-in Azure Storage, nakonfigurovat modul plug-in toouse hello účtu úložiště a pak vytvořte akce po sestavení, která odesílá vašeho účtu úložiště tooyour artefaktů sestavení. Tyto kroky jsou popsané v následující části hello.

## <a name="how-tooinstall-hello-azure-storage-plugin"></a>Jak tooinstall hello modul plug-in Azure Storage
1. V rámci hello Hudsonem řídicí panel, klikněte na **spravovat Hudsonem**.
2. Na hello **spravovat Hudsonem** klikněte na tlačítko **Správa modulů plug-in**.
3. Klikněte na tlačítko hello **dostupné** kartě.
4. Klikněte na tlačítko **ostatní**.
5. V hello **artefaktů Uploaders** vyberte **modul plug-in služby Microsoft Azure Storage**.
6. Klikněte na **Nainstalovat**.
7. Po dokončení instalace hello restartujte Hudsonem.

## <a name="how-tooconfigure-hello-azure-storage-plugin-toouse-your-storage-account"></a>Jak tooconfigure hello toouse modul plug-in Azure Storage účtu úložiště
1. V rámci hello Hudsonem řídicí panel, klikněte na **spravovat Hudsonem**.
2. Na hello **spravovat Hudsonem** klikněte na tlačítko **nakonfigurujte systém**.
3. V hello **konfigurace účtu úložiště Microsoft Azure** části:
   
    a. Zadejte název účtu úložiště, který můžete získat z hello [portálu Azure](https://portal.azure.com).
   
    b. Zadejte klíč účtu úložiště, také dosažitelný z hello [portálu Azure](https://portal.azure.com).
   
    c. Použít výchozí hodnotu hello **adresu URL koncového bodu služby objektů Blob** Pokud používáte hello veřejného cloudu Azure. Pokud používáte jiný Azure cloud, použijte koncový bod hello jako zadaný v hello [portálu Azure](https://portal.azure.com) pro váš účet úložiště.
   
    d. Klikněte na tlačítko **ověření přihlašovacích údajů úložiště** toovalidate účtu úložiště.
   
    e. [Nepovinné] Pokud máte další účty úložiště, které chcete k dispozici tooyour učiněna Hudsonem CI, klikněte na tlačítko **přidat další účty úložiště**.
   
    f. Klikněte na tlačítko **Uložit** toosave nastavení.

## <a name="how-toocreate-a-post-build-action-that-uploads-your-build-artifacts-tooyour-storage-account"></a>Jak toocreate akce po sestavení, která odesílá vašeho účtu úložiště tooyour artefaktů sestavení
Pro účely instrukce nejdřív potřebujeme toocreate úlohu, která bude vytvářet několik souborů a poté přidejte v účtu úložiště tooyour hello hello akce po sestavení tooupload soubory.

1. V rámci hello Hudsonem řídicí panel, klikněte na **nová úloha**.
2. Název úlohy hello **MyJob**, klikněte na tlačítko **sestavení úloha softwaru bez stylu**a potom klikněte na **OK**.
3. V hello **sestavení** části hello úlohy konfigurace, klikněte na tlačítko **přidat krok sestavení** a zvolte **dávkové spuštění Windows příkaz**.
4. V **příkaz**, použijte hello následující příkazy:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. V hello **akce po sestavení** části hello úlohy konfigurace, klikněte na tlačítko **nahrát artefakty tooMicrosoft Azure Blob storage**.
6. Pro **název účtu úložiště**, vyberte hello toouse účet úložiště.
7. Pro **název kontejneru**, zadejte název kontejneru hello. (hello kontejneru budou vytvořeny, pokud již neexistuje když jsou odeslány hello artefaktů sestavení.) Můžete použít proměnné prostředí, tak v tomto příkladu zadejte **${JOB_NAME}** jako název kontejneru hello.
   
    **Tip**
   
    Níže hello **příkaz** část, kde jste zadali skript pro **dávkové spuštění Windows příkaz** je odkaz proměnné prostředí toohello rozpoznána Hudsonem. Klikněte na tento odkaz toolearn hello prostředí proměnné názvy a popisy. Všimněte si, že prostředí proměnné, které obsahují zvláštní znaky, jako je například hello **BUILD_URL** proměnné prostředí, nejsou povoleny jako název kontejneru nebo běžné virtuální cestu.
8. Klikněte na tlačítko **zveřejnit nový kontejner ve výchozím nastavení** v tomto příkladu. (Pokud chcete toouse kontejner privátní, budete potřebovat pro přístup k sdílený přístupový podpis tooallow toocreate. To je nad rámec tohoto článku hello. Další informace o sdílených přístupových podpisů v [pomocí sdíleného přístupového podpisy (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. [Nepovinné] Klikněte na tlačítko **čisté kontejneru před nahráním** Pokud chcete hello kontejneru toobe vymazat obsah předtím, než se odešlou artefaktů sestavení (nechte nezaškrtnuté Pokud nechcete, aby tooclean hello obsah kontejneru hello).
10. Pro **seznamu artefakty tooupload**, zadejte  **text /*.txt**.
11. Pro **běžné virtuální cestu pro nahraném artefakty**, zadejte **${sestavení\_ID} / ${sestavení\_číslo}**.
12. Klikněte na tlačítko **Uložit** toosave nastavení.
13. V hello Hudsonem řídicí panel, klikněte na **sestavení teď** toorun **MyJob**. Zkontrolujte výstup konzoly hello stavu. Stavové zprávy pro Azure Storage bude výstup konzoly hello součástí, při spuštění akce po sestavení hello tooupload sestavení artefaktů.
14. Po úspěšném dokončení úlohy hello můžete zkontrolovat otevřením veřejného objektu blob hello hello sestavení artefaktů.
    
    a. Přihlaste se toohello [portálu Azure](https://portal.azure.com).
    
    b. Klikněte na tlačítko **úložiště**.
    
    c. Klikněte na název účtu úložiště hello, který jste použili pro Hudsonem.
    
    d. Klikněte na tlačítko **kontejnery**.
    
    e. Klikněte na tlačítko hello kontejner s názvem **myjob**, což je hello malá verzi hello název úlohy, který jste přiřadili při jeho vytváření hello Hudsonem úlohy. Názvy kontejnerů a objektů blob jsou malá písmena (a malá a velká písmena) ve službě Azure Storage. V rámci hello seznam objektů blob pro hello kontejner s názvem **myjob** byste měli vidět **hello.txt** a **date.txt**. Kopírovat adresu URL hello pro kteroukoli z těchto položek a otevřete jej v prohlížeči. Zobrazí se hello textový soubor, který byl odeslán jako artefaktů sestavení.

Jednu úlohu lze vytvořit pouze jednu akci po sestavení, která ukládání artefaktů tooAzure úložiště objektů Blob. Všimněte si, že hello jednu akci po sestavení tooupload artefakty tooAzure úložiště objektů Blob můžete zadat různé soubory (včetně zástupné znaky) a toofiles cest v rámci **tooupload seznamu artefakty** pomocí středníkem jako oddělovač. Například pokud vaše Hudsonem sestavení vytváří JAR soubory a soubory TXT v pracovním prostoru **sestavení** a vy chcete tooupload obou tooAzure úložiště objektů Blob, použijte pro hello hello **seznamu artefakty tooupload** hodnota: **sestavení nebo\*.jar; sestavení nebo\*.txt**. Můžete také použít dvěma dvojtečkami syntaxe toospecify toouse cestu v rámci název objektu blob hello. Například, pokud chcete, aby tooget JAR hello načten pomocí **binární soubory** v tooget soubory TXT hello a cesta k objektu blob hello načten pomocí **oznámení** v cestě objektu blob hello použijte následující hello pro hello  **Seznam artefakty tooupload** hodnota: **sestavení nebo\*. jar::binaries; sestavení nebo\*. txt::notices**.

## <a name="how-toocreate-a-build-step-that-downloads-from-azure-blob-storage"></a>Jak toocreate krok sestavení, stáhne z Azure Blob storage
Hello následující kroky ukazují, jak tooconfigure sestavení krok toodownload položky z Azure Blob storage. To může být užitečné, pokud chcete položky tooinclude v buildu, například JAR, která uchovává v Azure Blob storage.

1. V hello **sestavení** části hello úlohy konfigurace, klikněte na tlačítko **přidat krok sestavení** a zvolte **stažení ze služby Azure Blob storage**.
2. Pro **název účtu úložiště**, vyberte hello toouse účet úložiště.
3. Pro **název kontejneru**, zadejte název hello hello kontejneru, který obsahuje objekty BLOB hello chcete toodownload. Můžete použít proměnné prostředí.
4. Pro **název objektu Blob**, zadejte název objektu blob hello. Můžete použít proměnné prostředí. Navíc můžete použít hvězdičku, jako zástupný znak po zadání hello počáteční písmena názvu objektu blob hello. Například **projektu\***  by zadejte všech objektů BLOB, jejichž názvy začínají **projektu**.
5. [Nepovinné] Pro **cestu pro stažení**, zadejte cestu hello hello Hudsonem počítače, kde chcete toodownload soubory z úložiště objektů Blob Azure. Proměnné prostředí lze také použít. (Pokud nezadáte hodnotu **cestu pro stažení**, budou hello soubory z Azure Blob storage stažené toohello úlohy pracovního prostoru.)

Pokud máte další položky, které chcete toodownload z Azure Blob storage, můžete vytvořit další kroky.

Po spuštění sestavení, můžete zkontrolovat hello sestavení výstup konzoly historie a podívejte se na umístění stahování, toosee tom, zda text hello objekty BLOB, očekávána byly úspěšně staženy.

## <a name="components-used-by-hello-blob-service"></a>Komponenty používané stránkami hello služby objektů Blob
Hello následující text uvádí přehled hello součásti služby objektů Blob.

* **Účet úložiště**: všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště. Toto je nejvyšší úroveň hello hello oboru názvů pro přístup k objektům BLOB. Účet může obsahovat neomezený počet kontejnerů, tak dlouho, dokud jejich celková velikost je v části 100 TB.
* **Kontejner**: kontejner zajišťuje seskupení sady objektů BLOB. Všechny objekty blob musí být v kontejneru. Účet může obsahovat neomezený počet kontejnerů. Kontejner můžete pojmout neomezený počet objektů blob.
* **Objekt BLOB**: soubor libovolného typu a velikosti. Existují dva typy objektů BLOB, které mohou být uloženy ve službě Azure Storage: objekty BLOB bloků a stránek. Většina souborů jsou objekty BLOB bloku. Objekt blob jeden blok může být až do velikosti too200 GB. Tento kurz používá objekty BLOB bloku. Objekty BLOB stránky, jiný typ objektu blob, může být až too1 TB velikosti a jsou efektivnější, když jsou často upravit rozsah bajtů v souboru. Další informace o objekty BLOB najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).
* **Formát adresy URL**: objekty BLOB jsou adresovatelné hello následující formát adresy URL:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (hello formátu výše uvedené platí toohello veřejného cloudu Azure. Pokud používáte jiný Azure cloud, použijte hello koncový bod v hello [portálu Azure](https://portal.azure.com) toodetermine váš koncový bod adresy URL.)
  
    Ve formátu hello výše `storageaccount` představuje hello název účtu úložiště `container_name` představuje hello název kontejneru, a `blob_name` představuje hello název objektu blob služby v uvedeném pořadí. V rámci hello název kontejneru, může mít více cest, oddělených lomítkem,  **/** . název kontejneru Hello příklad v tomto kurzu se **MyJob**, a **${sestavení\_ID} / ${sestavení\_číslo}** byl použit pro hello běžné virtuální cesty, které s hello objektů blob Adresa URL hello následující formulář:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Další kroky
* [Splňovat Hudsonem](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Úložiště Azure SDK pro jazyk Java](https://github.com/azure/azure-storage-java)
* [Odkaz na sadu SDK klienta úložiště Azure](http://dl.windowsazure.com/storage/javadoc/)
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)

Další informace najdete na webu [Azure pro vývojáře v Javě](/java/azure).