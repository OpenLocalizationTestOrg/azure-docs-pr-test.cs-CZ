---
title: "aaaUsing Azure Storage s řešením volaných nepřetržité integrace | Microsoft Docs"
description: "V tomto kurzu ukazují, jak toouse hello Azure blob službu jako úložiště pro sestavení artefakty vytvořené volaných průběžnou integraci řešení."
services: storage
documentationcenter: java
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: f4e5ca75-f6cb-4f74-981b-2aa06bb8de45
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.openlocfilehash: a8a8c6f3b03cf61d7ad98f0b38e9421dd0b47bfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Použití Azure Storage s řešením Jenkins Continuous Integration
## <a name="overview"></a>Přehled
Hello následující informace ukazuje, jak používat úložiště objektů Blob toouse jako úložiště artefaktů sestavení vytvořené řešení volaných nepřetržité integrace (CI) nebo jako zdroj toobe soubory ke stažení v procesu sestavení. Jeden z hello scénáře, kde jste by to užitečné je když jste kódování agilní vývojovém prostředí (pomocí Java nebo jiné jazyky), sestavení běží v závislosti na průběžnou integraci a potřebujete úložiště pro sestavení artefaktů, tak, aby vám může , například je sdílet se členy jiné organizace, vašim zákazníkům nebo udržujte archiv. Další možností je při sestavení úlohu samotné vyžaduje další soubory, například závislosti toodownload jako součást hello sestavení vstup.

V tomto kurzu budete používat hello modulu plug-in úložiště Azure pro nepřetržitou Integraci volaných zpřístupněné společností Microsoft.

## <a name="overview-of-jenkins"></a>Přehled volaných
Volaných umožňuje průběžnou integraci projektu softwaru tím, že vývojáři tooeasily integrovat jejich změny kódu a mít sestavení vytvořené automaticky a často, a zvýšení produktivity hello hello vývojářů. Sestavení jsou verzí a artefaktů sestavení může být nahrané toovarious úložiště. Toto téma vám ukáže, jak toouse Azure blob storage jako hello úložiště artefaktů sestavení hello. Zobrazí také jak toodownload závislosti z Azure blob storage.

Další informace o volaných lze najít na [splňovat volaných](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-hello-blob-service"></a>Výhody použití služby Blob hello
Výhody používání toohost služby objektů Blob hello sestavení artefaktů agilní vývoj patří:

* Vysoká dostupnost artefaktů sestavení nebo ke stažení závislostí.
* Výkon, pokud je vaše řešení volaných CI ukládání artefaktů sestavení.
* Výkon, pokud zákazníci a partneři stažení artefaktů sestavení.
* Ovládat zásady přístupu uživatelů s volbou mezi anonymní přístup, přístup na základě vypršení platnosti sdílený přístupový podpis, privátní přístup, atd.

## <a name="prerequisites"></a>Požadavky
Můžete se třeba hello následující toouse hello služby objektů Blob s vaším řešením volaných CI:

* Průběžnou integraci volaných řešení.
  
    Pokud aktuálně nemáte volaných CI řešení, můžete spustit volaných CI řešení pomocí hello následujícím způsobem:
  
  1. Na počítači povolené Java, stáhněte si jenkins.war z <http://jenkins-ci.org>.
  2. Na příkazovém řádku, který je otevřen toohello složku, která obsahuje jenkins.war spusťte příkaz:
     
      `java -jar jenkins.war`

  3. V prohlížeči otevřete `http://localhost:8080/`. Tím se otevře řídicí panel volaných hello, které budete používat tooinstall a nakonfigurovat modul plug-in Azure Storage hello.
     
      Při typických volaných CI řešení by nastavit toorun jako služba, bude spuštěn hello volaných war v příkazovém řádku hello dostatečná pro účely tohoto kurzu.
* Účet Azure. Můžete zaregistrovat k účtu Azure v <http://www.azure.com>.
* Účet úložiště Azure. Pokud nemáte účet úložiště, můžete vytvořit jeden postupem hello v [vytvořit účet úložiště](storage-create-storage-account.md#create-a-storage-account).
* Znalost hello volaných CI řešení je doporučená, ale není potřeba, protože hello následující obsah bude používat základní příklad tooshow, hello kroky potřebné při použití služby objektů Blob hello jako úložiště pro nepřetržitou Integraci volaných sestavení artefaktů.

## <a name="how-toouse-hello-blob-service-with-jenkins-ci"></a>Jak toouse hello služby objektů Blob s volaných CI
toouse hello služby objektů Blob s volaných, budete potřebovat tooinstall hello modul plug-in Azure Storage, nakonfigurovat modul plug-in toouse hello účtu úložiště a pak vytvořte akce po sestavení, která odesílá vašeho účtu úložiště tooyour artefaktů sestavení. Tyto kroky jsou popsané v následující části hello.

## <a name="how-tooinstall-hello-azure-storage-plugin"></a>Jak tooinstall hello modul plug-in Azure Storage
1. V rámci hello volaných řídicí panel, klikněte na **spravovat volaných**.
2. V hello **spravovat volaných** klikněte na tlačítko **Správa modulů plug-in**.
3. Klikněte na tlačítko hello **dostupné** kartě.
4. V hello **artefaktů Uploaders** část, zkontrolujte **modul plug-in služby Microsoft Azure Storage**.
5. Klikněte na možnost **nainstalovat bez restartování** nebo **stáhnout a nainstalovat po restartování**.
6. Restartujte volaných.

## <a name="how-tooconfigure-hello-azure-storage-plugin-toouse-your-storage-account"></a>Jak tooconfigure hello toouse modul plug-in Azure Storage účtu úložiště
1. V rámci hello volaných řídicí panel, klikněte na **spravovat volaných**.
2. V hello **spravovat volaných** klikněte na tlačítko **nakonfigurujte systém**.
3. V hello **konfigurace účtu úložiště Microsoft Azure** části:
   1. Zadejte název účtu úložiště, který můžete získat z hello [portálu Azure](https://portal.azure.com).
   2. Zadejte klíč účtu úložiště, také dosažitelný z hello [portálu Azure](https://portal.azure.com).
   3. Použít výchozí hodnotu hello **adresu URL koncového bodu služby objektů Blob** Pokud používáte hello veřejného cloudu Azure. Pokud používáte jiný Azure cloud, použijte koncový bod hello jako zadaný v hello [portálu Azure](https://portal.azure.com) pro váš účet úložiště. 
   4. Klikněte na tlačítko **ověření přihlašovacích údajů úložiště** toovalidate účtu úložiště. 
   5. [Nepovinné] Pokud máte další účty úložiště, které chcete k dispozici tooyour učiněna volaných CI, klikněte na tlačítko **přidat další účty úložiště**.
   6. Klikněte na tlačítko **Uložit** toosave nastavení.

## <a name="how-toocreate-a-post-build-action-that-uploads-your-build-artifacts-tooyour-storage-account"></a>Jak toocreate akce po sestavení, která odesílá vašeho účtu úložiště tooyour artefaktů sestavení
Pro účely instrukce nejdřív potřebujeme toocreate úlohu, která bude vytvářet několik souborů a poté přidejte v účtu úložiště tooyour hello hello akce po sestavení tooupload soubory.

1. V rámci hello volaných řídicí panel, klikněte na **novou položku**.
2. Název úlohy hello **MyJob**, klikněte na tlačítko **sestavení projektu, uvolněte stylu softwaru**a potom klikněte na **OK**.
3. V hello **sestavení** části hello úlohy konfigurace, klikněte na tlačítko **přidat krok sestavení** a zvolte **dávkové spuštění Windows příkaz**.
4. V **příkaz**, použijte hello následující příkazy:

    ```   
    md text
    cd text
    echo Hello Azure Storage from Jenkins > hello.txt
    date /t > date.txt
    time /t >> date.txt
    ```

5. V hello **akce po sestavení** části hello úlohy konfigurace, klikněte na tlačítko **přidat akci po sestavení** a zvolte **nahrát úložiště objektů Blob tooAzure artefakty**.
6. Pro **název účtu úložiště**, vyberte hello toouse účet úložiště.
7. Pro **název kontejneru**, zadejte název kontejneru hello. (hello kontejneru budou vytvořeny, pokud již neexistuje když jsou odeslány hello artefaktů sestavení.) Můžete použít proměnné prostředí, tak v tomto příkladu zadejte **${JOB_NAME}** jako název kontejneru hello.
   
    **Tip**
   
    Níže hello **příkaz** část, kde jste zadali skript pro **dávkové spuštění Windows příkaz** je odkaz toohello prostředí proměnné, které volaných. Klikněte na tento odkaz toolearn hello prostředí proměnné názvy a popisy. Všimněte si, že prostředí proměnné, které obsahují zvláštní znaky, jako je například hello **BUILD_URL** proměnné prostředí, nejsou povoleny jako název kontejneru nebo běžné virtuální cestu.
8. Klikněte na tlačítko **zveřejnit nový kontejner ve výchozím nastavení** v tomto příkladu. (Pokud chcete toouse kontejner privátní, budete potřebovat pro přístup k sdílený přístupový podpis tooallow toocreate. To je mimo obor hello tohoto tématu. Další informace o sdílených přístupových podpisů v [pomocí sdíleného přístupového podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Nepovinné] Klikněte na tlačítko **čisté kontejneru před nahráním** Pokud chcete hello kontejneru toobe vymazat obsah předtím, než se odešlou artefaktů sestavení (nechte nezaškrtnuté Pokud nechcete, aby tooclean hello obsah kontejneru hello).
10. Pro **seznamu artefakty tooupload**, zadejte  **text /*.txt**.
11. Pro **běžné virtuální cestu pro nahraném artefakty**, pro účely tohoto kurzu, zadejte **${sestavení\_ID} / ${sestavení\_číslo}**.
12. Klikněte na tlačítko **Uložit** toosave nastavení.
13. Na řídicím panelu volaných hello, klikněte na tlačítko **sestavení teď** toorun **MyJob**. Zkontrolujte výstup konzoly hello stavu. Stavové zprávy pro úložiště Azure bude výstup konzoly hello součástí, při spuštění akce po sestavení hello tooupload sestavení artefaktů.
14. Po úspěšném dokončení úlohy hello můžete zkontrolovat otevřením veřejného objektu blob hello hello sestavení artefaktů.
    1. Přihlášení toohello [portálu Azure](https://portal.azure.com).
    2. Klikněte na tlačítko **úložiště**.
    3. Klikněte na název účtu úložiště hello, který jste použili pro volaných.
    4. Klikněte na tlačítko **kontejnery**.
    5. Klikněte na tlačítko hello kontejner s názvem **myjob**, což je hello malá verzi hello název úlohy, který jste přiřadili při vytvoření úlohy volaných hello. Názvy kontejnerů a objektů blob jsou malá písmena (a malá a velká písmena) v úložišti Azure. V rámci hello seznam objektů blob pro hello kontejner s názvem **myjob** byste měli vidět **hello.txt** a **date.txt**. Kopírovat adresu URL hello pro kteroukoli z těchto položek a otevřete jej v prohlížeči. Zobrazí se hello textový soubor, který byl odeslán jako artefaktů sestavení.

Lze vytvořit pouze jeden akce po sestavení, která odesílá úložiště objektů blob tooAzure artefakty na úlohu. Všimněte si, že hello jednu akci po sestavení tooupload artefakty tooAzure úložiště objektů blob můžete zadat různé soubory (včetně zástupné znaky) a toofiles cest v rámci **tooupload seznamu artefakty** pomocí středníkem jako oddělovač. Například pokud vaše volaných sestavení vytváří JAR soubory a soubory TXT v pracovním prostoru **sestavení** a vy chcete tooupload obou tooAzure úložiště objektů blob, použijte hello pro hello **seznamu artefakty tooupload** hodnota: **sestavení nebo\*.jar; sestavení nebo\*.txt**. Můžete také použít dvěma dvojtečkami syntaxe toospecify toouse cestu v rámci název objektu blob hello. Například, pokud chcete, aby tooget JAR hello načten pomocí **binární soubory** v tooget soubory TXT hello a cesta k objektu blob hello načten pomocí **oznámení** v cestě objektu blob hello použijte následující hello pro hello  **Seznam artefakty tooupload** hodnota: **sestavení nebo\*. jar::binaries; sestavení nebo\*. txt::notices**.

## <a name="how-toocreate-a-build-step-that-downloads-from-azure-blob-storage"></a>Jak toocreate krok sestavení, stáhne z Azure blob storage
Hello následující kroky ukazují, jak tooconfigure sestavení krok toodownload položky z Azure blob storage. To může být užitečné, pokud chcete položky tooinclude v buildu, například JAR, která uchovává v Azure blob storage.

1. V hello **sestavení** části hello úlohy konfigurace, klikněte na tlačítko **přidat krok sestavení** a zvolte **stažení ze služby Azure Blob storage**.
2. Pro **název účtu úložiště**, vyberte hello toouse účet úložiště.
3. Pro **název kontejneru**, zadejte název hello hello kontejneru, který obsahuje objekty BLOB hello chcete toodownload. Můžete použít proměnné prostředí.
4. Pro **název objektu Blob**, zadejte název objektu blob hello. Můžete použít proměnné prostředí. Navíc můžete použít hvězdičku, jako zástupný znak po zadání hello počáteční písmena názvu objektu blob hello. Například **projektu\***  by zadejte všech objektů BLOB, jejichž názvy začínají **projektu**.
5. [Nepovinné] Pro **cestu pro stažení**, zadejte cestu hello na hello volaných počítači, kde chcete toodownload soubory z Azure blob storage. Proměnné prostředí lze také použít. (Pokud nezadáte hodnotu **cestu pro stažení**, budou hello soubory z Azure blob storage stažené toohello úlohy pracovního prostoru.)

Pokud máte další položky, které chcete toodownload z Azure blob storage, můžete vytvořit další kroky.

Po spuštění sestavení, můžete zkontrolovat hello sestavení výstup konzoly historie a podívejte se na umístění stahování, toosee tom, zda text hello objekty BLOB, očekávána byly úspěšně staženy.  

## <a name="components-used-by-hello-blob-service"></a>Komponenty používané stránkami hello služby objektů Blob
Hello následující text uvádí přehled hello součásti služby objektů Blob.

* **Účet úložiště**: všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště. Toto je nejvyšší úroveň hello hello oboru názvů pro přístup k objektům BLOB. Účet může obsahovat neomezený počet kontejnerů, tak dlouho, dokud jejich celková velikost je v části 100TB.
* **Kontejner**: kontejner zajišťuje seskupení sady objektů BLOB. Všechny objekty blob musí být v kontejneru. Účet může obsahovat neomezený počet kontejnerů. Kontejner můžete pojmout neomezený počet objektů blob.
* **Objekt BLOB**: soubor libovolného typu a velikosti. Existují dva typy objektů BLOB, které mohou být uloženy ve službě Azure Storage: objekty BLOB bloků a stránek. Většina souborů jsou objekty BLOB bloku. Objekt blob jeden blok může být až too200GB velikost. Tento kurz používá objekty BLOB bloku. Objekty BLOB stránky, jiný typ objektu blob, může být až too1TB velikostí a jsou efektivnější, když jsou často upravit rozsah bajtů v souboru. Další informace o objekty BLOB najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).
* **Formát adresy URL**: objekty BLOB jsou adresovatelné hello následující formát adresy URL:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (hello formátu výše uvedené platí toohello veřejného cloudu Azure. Pokud používáte jiný Azure cloud, použijte hello koncový bod v hello [portálu Azure](https://portal.azure.com) toodetermine váš koncový bod adresy URL.)
  
    Ve formátu hello výše `storageaccount` představuje hello název účtu úložiště `container_name` představuje hello název kontejneru, a `blob_name` představuje hello název objektu blob služby v uvedeném pořadí. V rámci hello název kontejneru, může mít více cest, oddělených lomítkem,  **/** . název kontejneru Hello příklad v tomto kurzu se **MyJob**, a **${sestavení\_ID} / ${sestavení\_číslo}** byl použit pro hello běžné virtuální cesty, které s hello objektů blob Adresa URL hello následující formulář:
  
    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Další kroky
* [Splňovat volaných](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
* [Úložiště Azure SDK pro jazyk Java](https://github.com/azure/azure-storage-java)
* [Odkaz na sadu SDK klienta úložiště Azure](http://dl.windowsazure.com/storage/javadoc/)
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)

Další informace najdete také v tématu hello [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).
