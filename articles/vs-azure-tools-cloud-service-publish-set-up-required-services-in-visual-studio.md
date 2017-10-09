---
title: aaaPrepare toopublish nebo nasadit aplikaci Azure ze sady Visual Studio | Microsoft Docs
description: "Další postupy tooset hello do cloudu a úložiště účet služby a konfiguraci aplikace Azure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: b5231d400e2ad9e20c3f21bad48a77c328b1f7a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-toopublish-or-deploy-an-azure-application-from-visual-studio"></a>Příprava tooPublish nebo nasazení aplikace Azure ze sady Visual Studio
## <a name="overview"></a>Přehled
Před publikováním projekt cloudové služby, je potřeba nastavit hello následující služby:

* A **Cloudová služba** toorun role v hello prostředí Azure
* A **účet úložiště** který poskytuje přístup toohello službám Blob, fronty a tabulky.

Použijte následující postupy tooset si tyto služby hello a konfiguraci vaší aplikace

## <a name="create-a-cloud-service"></a>Vytvoření cloudové služby
toopublish tooAzure cloudové služby, musíte nejprve vytvořit cloudové služby, který se spouští vaše role v hello prostředí Azure. Cloudové služby můžete vytvořit v hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), jak je popsáno v části hello **hello toocreate cloudové služby pomocí portálu Azure classic**dál v tomto tématu. Cloudové služby v sadě Visual Studio můžete také vytvořit pomocí Průvodce publikováním hello.

### <a name="toocreate-a-cloud-service-by-using-visual-studio"></a>toocreate cloudové služby pomocí sady Visual Studio
1. Otevřete hello místní nabídku pro hello projektu Azure a zvolte **publikovat**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)
2. Pokud nejste přihlášeni, přihlaste se pomocí uživatelského jména a hesla pro účet Microsoft hello nebo účtu organizace, který je spojen s předplatným Azure.
3. Zvolte hello **Další** tlačítko tooadvance toohello **nastavení** stránky.

    ![Běžné nastavení Průvodce publikování](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)
4. V hello **cloudové služby** vyberte **vytvořit nový**. Hello **vytvoření služby Azure** otevře se dialogové okno.
5. Zadejte název hello cloudové služby. Název Hello součástí hello adresa URL služby a proto musí být globálně jedinečný. Název Hello není malá a velká písmena.

### <a name="toocreate-a-cloud-service-by-using-hello-azure-classic-portal"></a>toocreate cloudové služby s použitím hello portál Azure classic
1. Přihlaste se toohello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103) na webu společnosti Microsoft hello.
2. (volitelné) toodisplay seznam cloudových služeb, které jste už vytvořili, vyberte odkaz hello cloudové služby na levé straně stránky hello hello.
3. Zvolte hello  **+**  ikonu v hello levém dolním rohu a potom vyberte **Cloudová služba** v zobrazené nabídce hello. Jiné obrazovky s dvě možnosti, **rychle vytvořit** a **vytvořit vlastní**, se zobrazí. Pokud se rozhodnete **rychle vytvořit**, můžete vytvořit cloudovou službu právě zadáním jeho adresu URL a hello oblast, kde se bude fyzicky hostovat. Pokud se rozhodnete **vytvořit vlastní**, můžete publikovat Cloudová služba okamžitě zadáním balíček (soubor .cspkg), souboru (.csdef) a certifikát. Vytvořit vlastní není povinné, pokud máte v úmyslu toopublish cloudové služby s použitím hello **publikovat** v projektu Azure. Hello **publikovat** příkaz je k dispozici v místní nabídce hello Azure projektu.
4. Zvolte **rychle vytvořit** toolater publikování cloudové služby pomocí sady Visual Studio.
5. Zadejte název pro cloudové služby. Hello úplnou adresu URL se zobrazí další toohello název.
6. V seznamu hello zvolte hello oblasti, kde se nachází většina uživatelů.
7. V dolní části hello okna hello, zvolte hello **vytvoření cloudové služby** odkaz.

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
Účet úložiště poskytuje přístup toohello službám Blob, fronty a tabulky. Účet úložiště můžete vytvořit pomocí sady Visual Studio nebo hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="toocreate-a-storage-account-by-using-visual-studio"></a>toocreate účet úložiště pomocí sady Visual Studio
1. V **Průzkumníku řešení**, otevřete hello místní nabídku pro hello **úložiště** uzel a potom zvolte **vytvořit účet úložiště**.

    ![Vytvořit nový účet úložiště Azure](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)
2. Vyberte nebo zadejte následující informace pro nový účet úložiště hello v hello hello **vytvořit účet úložiště** dialogové okno.

   * Chcete účet úložiště hello tooadd toowhich předplatného Azure Hello.
   * Chcete použít pro nový účet úložiště hello toouse název Hello.
   * Hello oblast nebo skupinu vztahů (například západní USA nebo jihovýchodní Asie).
   * Hello typ replikace pro účet úložiště hello, jako je například geograficky redundantní chcete toouse.
3. Až budete hotoví, zvolte **vytvořit**.hello nový účet úložiště se zobrazí v hello **úložiště** seznamu v **Průzkumníka serveru**.

### <a name="toocreate-a-storage-account-by-using-hello-azure-classic-portal"></a>toocreate účet úložiště pomocí hello portál Azure classic
1. Přihlaste se toohello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103) na webu společnosti Microsoft hello.
2. (Volitelné) tooview účty úložiště, zvolte hello **úložiště** odkaz panelu hello na levé straně stránky hello hello.
3. V levém dolním hello hello stránky, zvolte hello  **+**  ikonu.
4. V zobrazené nabídce hello zvolte **úložiště**a potom zvolte **rychle vytvořit**.
5. Zadejte název, který bude mít za následek jedinečnou adresou url účtu úložiště hello.
6. Zadejte název cloudové služby. Hello úplnou adresu URL se zobrazí další toohello název.
7. V seznamu hello oblastí vyberte v oblasti, kde se nachází většina uživatelů.
8. Určete, jestli má tooenable geografická replikace. Pokud povolíte geografická replikace, data se uloží do více fyzických umístění tooreduce hello riziko ztráty. Díky této funkci úložiště dražší, ale můžete snížit náklady na hello povolením geografického umístění, při vytváření účtu úložiště hello místo přidávání funkce hello později. Další informace najdete v tématu [geografická replikace](http://go.microsoft.com/fwlink/?LinkId=253108).
9. V dolní části hello okna hello, zvolte hello **vytvořit účet úložiště** odkaz.

Po vytvoření účtu úložiště, zobrazí se hello adresy URL, že můžete používat tooaccess prostředky v každé z hello služby Azure storage a také hello primární a sekundární přístupové klíče pro váš účet. Pomocí těchto klíčů tooauthenticate požadavků na služby úložiště hello přístup.

> [!NOTE]
> Hello sekundární přístupový klíč poskytuje hello stejný přístup k účtu úložiště tooyour jako hello primární přístupový klíč a je generována jako záložní ohrožené primární přístupový klíč. Kromě toho se doporučuje znovu vygenerovat klíče pro přístup k v pravidelných intervalech. Můžete upravit připojovací řetězec nastavení toouse hello sekundární klíč zatímco si znovu vygenerujete hello primární klíč, a můžete ho upravit toouse hello se znova vygeneroval primární klíč zatímco si znovu vygenerujete hello sekundární klíč.
>
>

## <a name="configure-your-app-toouse-services-provided-by-hello-storage-account"></a>Konfigurace vaše aplikace toouse služby poskytované hello účtu úložiště
Je nutné nakonfigurovat žádné role, které má přístup k úložišti služby toouse hello úložiště Azure služby, které jste vytvořili. toodo, můžete použít více konfigurace služby pro Azure projektu. Ve výchozím nastavení dva jsou vytvořené v projektu Azure. Pomocí několika konfigurace služby, můžete použít hello stejný připojovací řetězec v kódu, ale mít jinou hodnotu pro připojovací řetězec v každé konfiguraci služby. Můžete například použít jeden toorun konfigurace služby a ladění aplikace místně pomocí emulátoru úložiště Azure hello a jinou službu, pro konfiguraci toopublish tooAzure vaší aplikace. Další informace o konfiguracích služby najdete v tématu [konfigurace si Azure pomocí více služby konfigurace projektu](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="tooconfigure-your-application-toouse-services-that-hello-storage-account-provides"></a>poskytuje tooconfigure služby toouse vaší aplikace, které hello účtu úložiště
1. V sadě Visual Studio otevřete řešení Azure. V Průzkumníku řešení otevřete hello místní nabídky pro každou roli v projektu Azure, který přistupuje k službě úložiště hello a zvolte **vlastnosti**. Na stránce s názvem hello hello role se zobrazí v editoru Visual Studio hello. Hello stránka zobrazí pole hello hello **konfigurace** kartě.
2. Na stránkách vlastností hello hello role, zvolte **nastavení**.
3. V hello **konfigurace služby** seznamu, vyberte název hello hello konfigurace služby, které chcete tooedit. Pokud chcete, aby toomake změny tooall hello konfigurace služby pro tuto roli, můžete vybrat **všechny konfigurace**.  Další informace o tom, jak tooupdate služby konfigurace, najdete v tématu hello **spravovat připojovací řetězce pro účty úložiště** v tématu hello [nakonfigurovat hello role pro cloudové služby Azure pomocí sady Visual Studio ](vs-azure-tools-configure-roles-for-cloud-service.md).
4. toomodify žádné nastavení připojovacího řetězce, vyberte hello **...** tlačítko Další toohello nastavení. Hello **vytvoření připojovacího řetězce úložiště** zobrazí se dialogové okno.
5. V části **připojit pomocí**, zvolte hello **vaše předplatné** možnost.
6. V hello **předplatné** vyberte své předplatné. Pokud hello seznam předplatných neobsahuje hello, který chcete, zvolte hello **stáhnout nastavení publikování** odkaz.
7. V hello **název účtu** seznamu, vyberte název účtu úložiště. Přihlašovací údaje účtu úložiště Azure nástroje automaticky získá pomocí souboru .publishsettings hello. toospecify ručně, přihlašovací údaje účtu úložiště vyberte hello **ručně zadali přihlašovací údaje** možnost a potom pokračujte v tomto postupu. Název účtu úložiště a primární klíč můžete získat z hello [portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=213885). Pokud nechcete, aby toospecify nastavení svého účtu úložiště ručně, vyberte hello **OK** dialogové okno tlačítko tooclose hello.
8. Zvolte hello **zadejte účet úložiště** odkaz přihlašovací údaje.
9. V hello **název účtu** zadejte hello název svého účtu úložiště.

   > [!NOTE]
   > Přihlaste se k hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885)a potom zvolte hello **úložiště** tlačítko. Hello portál zobrazuje seznam účtů úložiště. Pokud si zvolíte účet, otevře se stránka pro ni. Hello název účtu úložiště hello můžete zkopírovat z této stránky. Pokud používáte předchozí verzi portálu classic hello, hello název účtu úložiště se zobrazí v hello **účty úložiště** zobrazení. toocopy tento název, zvýrazní ji v hello **vlastnosti** okno tohoto zobrazení a potom vyberte hello Ctrl-C klíče. Název hello toopaste do sady Visual Studio, vyberte hello **název účtu** textové pole a potom vyberte klávesy Ctrl + V hello.
   >
   >
10. V hello **klíč účtu** pole, zadejte primární klíč, nebo zkopírujte a vložte ho z hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
     toocopy tento klíč:

    1. V dolní části hello hello stránky pro účet hello příslušné úložiště, zvolte hello **Správa klíčů** tlačítko.
    2. Na hello **spravovat přístup klíče** vyberte hello textu hello primární přístupový klíč a potom zvolte hello klávesy Ctrl + C.
    3. V nástrojích pro Azure, vložte klíč hello do hello **klíč účtu** pole.
    4. Musíte vybrat jednu z následujících možností toodetermine hello přístupu účtu úložiště hello hello služby:

       * **Prostřednictvím protokolu HTTP**. Toto je možnost Standardní hello. Například, `http://<account name>.blob.core.windows.net`.
       * **Použití protokolu HTTPS** pro zabezpečené připojení. Například, `https://<accountname>.blob.core.windows.net`.
       * **Zadejte vlastní koncové body** pro každou hello tři služby. Potom můžete zadat těchto koncových bodů do hello pole pro konkrétní službu hello.

         > [!NOTE]
         > Pokud vytvoříte vlastní koncové body, můžete vytvořit složitější připojovací řetězec. Pokud použijete tento formát řetězce, můžete zadat koncové body služby úložiště, které obsahují název vlastní domény, který je zaregistrovaný pro váš účet úložiště s hello služby objektů Blob. Také můžete udělit přístup jen k prostředkům tooblob ve jediný kontejner prostřednictvím sdílený přístupový podpis. Další informace o tom, toocreate vlastní koncové body, najdete v části [nakonfigurování připojovacích řetězců úložiště Azure](storage/common/storage-configure-connection-string.md).
         >
         >
11. toosave tyto změny řetězec připojení, zvolte hello **OK** tlačítko a potom zvolte hello **Uložit** tlačítka na panelu nástrojů hello. Po uložení těchto změn, můžete získat hodnotu hello tento připojovací řetězec v kódu pomocí [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Když publikujete tooAzure vaší aplikace, vyberte hello konfigurace služby, který obsahuje hello účtu úložiště Azure pro hello připojovací řetězec. Po publikování aplikace, ověřte, že aplikace hello funguje podle očekávání proti hello služby Azure storage

## <a name="next-steps"></a>Další kroky
toolearn Další informace o publikování aplikací tooAzure ze sady Visual Studio, najdete v části [publikování cloudové služby pomocí nástroje Azure hello](vs-azure-tools-publishing-a-cloud-service.md).
