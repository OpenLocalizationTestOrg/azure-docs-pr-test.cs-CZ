---
title: "aaaConfiguring projektu Azure pomocí více konfigurace služby | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure cloudové služby projektu změnou hello ServiceDefinition.csdef a souboru ServiceConfiguration.cscfg soubory."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 14222266093eb876db0ac9ce8d3d17a04c65d1a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfigurace projektu Azure pomocí více konfigurace služby
Projekt Azure cloud service zahrnuje dvě konfigurační soubory: ServiceDefinition.csdef a souboru ServiceConfiguration.cscfg. Tyto soubory jsou zabalit s aplikací Azure cloud service a nasazení tooAzure.

* Hello **ServiceDefinition.csdef** soubor obsahuje hello metadat, který je požadován hello prostředí Azure pro hello požadavky aplikace cloudové služby, včetně rolí, které obsahuje. Tento soubor zároveň obsahuje nastavení konfigurace, které se vztahují tooall instance. Tato nastavení konfigurace můžete přečíst v době běhu pomocí hello API Runtime hostování služby Azure. Tento soubor nelze aktualizovat, když vaše služba běží v Azure.
* Hello **souboru ServiceConfiguration.cscfg** souboru nastaví hodnoty pro nastavení konfigurace hello definované v souboru definice služby hello a určuje hello počet instancí toorun pro každou roli. Tento soubor můžete aktualizovat, když cloudové služby běží v Azure.

Hello nástroje Azure pro sadu Microsoft Visual Studio poskytují stránky vlastností, které můžete použít nastavení konfigurace tooset uložené v těchto souborech. tooaccess hello stránky vlastností, dvakrát klikněte na odkaz na role hello pod hello projektu Azure cloudové služby v Průzkumníku řešení, nebo klikněte pravým tlačítkem na odkaz hello roli a vyberte možnost **vlastnosti**, jak ukazuje následující obrázek hello.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Informace o hello základní schémata hello definice služby a soubory konfigurace služby najdete v tématu hello [– odkaz schématu](https://msdn.microsoft.com/library/azure/dd179398.aspx). Další informace o konfiguraci služby najdete v tématu [jak tooConfigure cloudových služeb](cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Konfigurace vlastností role
stránky vlastností Hello pro webovou roli a roli pracovního procesu jsou podobné, i když existuje několik rozdílů, na kterou v následující části hello.

Z hello **ukládání do mezipaměti** stránky, můžete nakonfigurovat hello ukládání do mezipaměti služby Azure.

### <a name="configuration-page"></a>Stránka Konfigurace
Na hello **konfigurace** stránky, můžete nastavit tyto vlastnosti:

**Instance**

Sada hello **Instance** počet vlastnost toohello počet instancí služby hello měly být spuštěny pro tuto roli.

Sada hello **velikost virtuálního počítače** vlastnost příliš**navíc malé**, **malé**, **střední**, **velké**, nebo **Velmi velké**.  Další informace najdete v tématu [velikosti pro cloudové služby](cloud-services/cloud-services-sizes-specs.md).

**Po spuštění akce** (jenom webové Role)

Nastavte tuto vlastnost toospecify, který Visual Studio by měla spustit webový prohlížeč, koncové body hello HTTP nebo HTTPS hello koncových bodů nebo obojí, při spuštění ladění.

Hello možnost koncový bod HTTPS je k dispozici pouze v případě, že jste definovali koncový bod HTTPS již pro vaši roli. Můžete definovat koncový bod HTTPS na hello **koncové body** stránku vlastností.

Pokud jste už přidali koncový bod HTTPS, hello možnost koncový bod HTTPS je ve výchozím nastavení povolené a Visual Studio spustí prohlížeč pro tento koncový bod, když spustíte, ladění, kromě prohlížeče tooa pro svůj koncový bod HTTP. Předpokladem je, že jsou povolené oba možnosti spuštění.

**Diagnostika**

Diagnostika je ve výchozím nastavení povolena pro webovou roli hello. emulátor místního úložiště hello toouse jsou nastavené Hello projektu Azure cloudové služby a účet úložiště. Pokud jste připravené toodeploy tooAzure, můžete vybrat tlačítko Tvůrce hello (**...** ) tooupdate hello úložiště účet toouse úložiště Azure v cloudu hello. Účet úložiště toohello data diagnostiky hello můžete přenést na vyžádání nebo v automaticky naplánovaných intervalech. Další informace o Azure diagnostics najdete v tématu [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Nastavení stránky
Na hello **nastavení** stránky, můžete přidat nastavení konfigurace pro vaši službu. Nastavení konfigurace jsou páry název hodnota. Kód spuštěný v roli hello může číst hello hodnoty nastavení konfigurace v době běhu pomocí třídy poskytované hello [spravované knihovny Azure](http://go.microsoft.com/fwlink?LinkID=171026). Konkrétně hello [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) metoda vrátí hodnotu hello nastavení s názvem konfigurace za běhu.

### <a name="configuring-a-connection-string-tooa-storage-account"></a>Konfigurace účtu úložiště tooa řetězec připojení
Připojovací řetězec je nastavení konfigurace, která poskytuje informace o připojení a ověřování pro emulátor úložiště hello nebo pro účet úložiště Azure. Vždy, když kód musí přístup k datům služby úložiště Azure – to znamená, objektů blob, fronty nebo dat v tabulce – Kód spuštěný v roli, budete mít toodefine připojovací řetězec pro daný účet úložiště.

Připojovací řetězec, který účet úložiště Azure tooan bodů musí mít definovaný formát. Informace o tom najdete v části toocreate připojovací řetězce, [nakonfigurování připojovacích řetězců úložiště Azure](storage/common/storage-configure-connection-string.md).

Když jsou připravené tootest služby proti hello služby Azure storage, nebo pokud jste připraveni toodeploy tooAzure vaše cloudové služby, můžete změnit hodnotu hello všechny připojovací řetězce toopoint tooyour účtu úložiště Azure. Vyberte (**...** ), vyberte **zadejte přihlašovací údaje účtu úložiště**. Zadejte informací o vašem účtu, který obsahuje název účtu a klíč účtu. V hello **připojovací řetězce k účtu úložiště** dialogové okno, můžete také určit, jestli chcete toouse hello výchozí HTTPS koncové body (hello výchozí možnost), koncové body HTTP hello výchozí nebo vlastní koncové body. Můžete se rozhodnout vlastní koncové body toouse Pokud je zaregistrovaný vlastní název domény pro vaši službu, jak je popsáno v [nakonfigurovat vlastní název domény pro data objektů blob v účtu úložiště Azure](storage/blobs/storage-custom-domain-name.md).

> [!IMPORTANT]
> Před nasazením služby, je třeba upravit vaše připojení řetězce toopoint tooan účtu úložiště Azure. Není-li toodo to může způsobit vaše role není toostart nebo toocycle prostřednictvím hello inicializace, zaneprázdněn a ukončení stavů.
> 
> 

## <a name="endpoints-page"></a>Stránka Koncové body
Role pracovních procesů může mít libovolný počet koncových bodů protokolu HTTP, HTTPS nebo TCP. Koncové body můžou být vstupních koncových bodů, které jsou k dispozici tooexternal klientů, nebo vnitřních koncových bodů, které jsou k dispozici tooother role, které běží ve službě hello.

* toomake HTTP koncový bod k dispozici tooexternal klientů a webové prohlížeče, změňte hello koncový bod typu tooinput a zadejte název a číslo veřejného portu.
* toomake HTTPS koncový bod k dispozici tooexternal klientů a webových prohlížečů, změnit typ koncového bodu hello příliš**vstupní**a zadejte název, číslo veřejného portu a název certifikátu pro správu.
  
    Všimněte si, že abyste mohli zadat certifikát pro správu, je nutné na hello definovat hello certifikát **certifikáty** stránku vlastností.
* toomake koncový bod k dispozici pro interní přístupu podle rolí v hello cloudové služby, změňte hello koncový bod typu toointernal a zadejte název a privátní porty možné pro tento koncový bod.

## <a name="local-storage-page"></a>Stránka Místní úložiště
Můžete použít hello **místní úložiště** tooreserve vlastností stránky, jeden nebo více prostředků místní úložiště pro roli. Prostředek Místní úložiště je vyhrazené adresář v systému souborů hello hello virtuální počítač Azure ve kterém je spuštěna instance role.

## <a name="certificates-page"></a>Stránka certifikátů
Na hello **certifikáty** stránky, certifikáty můžete přidružit vaši roli. Hello certifikáty, které přidáte, může být použité tooconfigure koncové body HTTPS na hello **koncové body** stránku vlastností.

Hello **certifikáty** stránka vlastností přidá informace o konfiguraci certifikátů tooyour služby. Všimněte si, že certifikáty nejsou spojených s službou; je potřeba nahrát certifikáty samostatně tooAzure prostřednictvím hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).

tooassociate certifikát s rolí, zadejte název pro certifikát hello. Při konfiguraci koncový bod HTTPS na hello se použít tento certifikát název toorefer toohello **koncové body** stránku vlastností. V dalším kroku určete, zda je úložiště certifikátů hello **místního počítače** nebo **aktuální uživatel** a název hello hello úložiště. Nakonec zadejte kryptografický otisk certifikátu hello. Pokud certifikát hello je v hello aktuální User\Personal (My) úložiště, můžete zadat kryptografický otisk certifikátu hello výběrem hello certifikát z vyplněná seznamu. Pokud se nachází v jiném umístění, zadejte hodnotu kryptografického otisku hello ručně.

Když přidáte certifikát z úložiště certifikátů hello, všechny zprostředkující certifikáty se automaticky přidají toohello nastavení konfigurace pro vás. Tyto zprostředkující certifikáty musí být také nahrát tooAzure v pořadí toocorrectly konfigurace služby pro protokol SSL.

Všechny certifikáty správy, které přidružit služby použít službu tooyour jenom v případě, že je spuštěna v cloudu hello. Pokud vaše služba běží v hello místní vývojové prostředí, používá standardní certifikát, který je spravován pomocí emulátoru služby výpočty hello.

## <a name="configuring-hello-azure-cloud-service-project"></a>Konfigurace projektu hello Azure cloudové služby
tooconfigure nastavení, které se vztahuje projekt tooan celý Azure cloudové služby, poprvé otevřete hello místní nabídky pro tento uzel projektu a pak vyberte vlastnosti tooopen jeho stránky vlastností. Hello následující tabulka znázorňuje tyto stránky vlastností.

| Stránky vlastností | Popis |
| --- | --- |
| Aplikace |Z této stránky můžete zobrazit informace o hello verzi nástroje Azure, který používá tento projekt cloudové služby a můžete upgradovat toohello aktuální verze nástroje hello. |
| Události sestavení |Na této stránce můžete nastavit před a po sestavení události. |
| Vývoj |Z této stránky můžete zadat pokyny ke konfiguraci sestavení a hello podmínky, za kterých se spustit žádné události po sestavení. |
| Web |Z této stránky můžete nakonfigurovat nastavení, které se týkají toohello webový server. |

