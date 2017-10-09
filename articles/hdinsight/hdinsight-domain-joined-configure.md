---
title: "clustery HDInsight připojený k doméně aaaConfigure - Azure | Microsoft Docs"
description: "Zjistěte, jak tooset registrace a konfigurace clusterů HDInsight připojený k doméně"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 8c4b3d269a7662d27a49b839e5cd05a3e24f7023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Konfigurace clusterů HDInsight připojený k doméně (Preview)

Zjistěte, jak cluster tooset si Azure HDInsight s Azure Active Directory (Azure AD) a [Apache škálu](http://hortonworks.com/apache/ranger/) tootake výhodou silného ověřování a zásady řízení (RBAC) bohaté přístupu na základě rolí.  HDInsight připojený k doméně se dá nakonfigurovat jenom na clusterech se systémem Linux. Další informace najdete v tématu [clustery HDInsight připojený k doméně zavádět](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie není povoleno v doméně HDInsight.

Tento článek je první kurz hello řady:

* Vytvoření HDInsight clusteru připojené tooAzure AD (prostřednictvím hello schopnost Azure Directory Domain Services) s Apache škálu povoleno.
* Vytvořit a použít zásady Hive prostřednictvím Apache škálu a povolit uživatelům (například datových vědců) tooconnect tooHive pomocí rozhraní ODBC nástrojů, například aplikace Excel, Tableau atd. Společnost Microsoft pracuje na přidávání dalších úloh, jako jsou HBase, Spark a Storm, připojený k tooDomain HDInsight brzy k dispozici.

Příklad konečné topologie hello vypadá takto:

![Topologie HDInsight připojený k doméně](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Protože Azure AD aktuálně podporuje pouze klasické virtuální sítě (virtuální sítě) a podporuje se jen clustery HDInsight se systémem Linux Azure Resource Manager na základě virtuálních sítí, integrace HDInsight Azure AD vyžaduje dvě virtuální sítě a partnerský vztah mezi nimi. Informace o porovnání hello mezi hello dva modely nasazení najdete v tématu [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a hello stav svých prostředků](../azure-resource-manager/resource-manager-deployment-model.md). Hello dvou virtuálních sítí musí být v hello stejné oblasti jako hello služby Azure AD DS.

Služba Azure názvy musí být globálně jedinečný. v tomto kurzu se používají následující názvy Hello. Contoso je fiktivní název. Je třeba nahradit *contoso* s jiným názvem při můžete absolvovat kurz hello. 

**Názvy:**

| Vlastnost | Hodnota |
| --- | --- |
| Virtuální síť Azure AD |contosoaadvnet |
| Skupina prostředků Azure AD virtuální sítě |contosoaadrg |
| Adresář Azure AD |contosoaaddirectory |
| Název domény služby Azure AD |společnosti Contoso (contoso.onmicrosoft.com) |
| Virtuální síť HDInsight |contosohdivnet |
| Skupina prostředků HDInsight virtuální sítě |contosohdirg |
| HDInsight cluster |contosohdicluster |

Tento kurz obsahuje hello postup pro konfiguraci clusteru HDInsight připojený k doméně. Každá část obsahuje odkazy tooother články s podrobnější informace.

## <a name="prerequisite"></a>Předpoklad:
* Seznamte se s [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) jeho [ceny](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktura.
* Zajistěte, aby vaše předplatné seznam povolených adres pro tuto verzi public preview. Můžete to provést pomocí e-mailu toohdipreview@microsoft.com s vaším ID předplatného.
* Certifikát SSL, který je podepsaný podpisový autoritou pro vaši doménu. Hello certifikát je vyžadován nakonfigurováním zabezpečený LDAP. Nelze použít certifikáty podepsané svým držitelem.

## <a name="procedures"></a>Postupy
1. Vytvoření Azure klasické virtuální sítě pro vaši službu Azure AD.  
2. Vytvoření a konfigurace Azure AD a Azure AD DS.
3. Vytvořte virtuální síť HDInsight v režimu správy hello prostředků Azure.
4. Sdílené hello dvě virtuální sítě.
5. Vytvoření clusteru HDInsight.

> [!NOTE]
> Tento kurz předpokládá, že nemáte Azure AD. Pokud nemáte, můžete přeskočit část hello v kroku 2.
> 
> 

Je skript prostředí PowerShell, které automatizuje kroky 3 až 7.  Další informace najdete v tématu [clustery HDInsight připojený k doméně nakonfigurovat pomocí prostředí Azure PowerShell](hdinsight-domain-joined-configure-use-powershell.md).

## <a name="create-an-azure-virtual-network-classic"></a>Vytvoření virtuální sítě Azure (klasický)
V této části vytvoříte virtuální sítě (klasické) pomocí hello portálu Azure. V další části hello povolíte pro vaši službu Azure AD ve virtuální síti hello hello služby Azure AD DS. Další informace o hello následující postup a použití jiných metod vytvoření virtuální sítě najdete v tématu [vytvoření virtuální sítě (klasické) pomocí portálu Azure hello](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

**toocreate klasické virtuální sítě**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com). 
2. Klikněte na tlačítko **nové** > **sítě** > **virtuální sítě**.
3. V **vybrat model nasazení**, vyberte **Classic**a potom klikněte na **vytvořit**.
4. Zadejte nebo vyberte hello následující hodnoty:
   
   * **Název**: contosoaadvnet
   * **Adresní prostor**: 10.1.0.0/16
   * **Název podsítě**: Subnet1
   * **Rozsah adres podsítě**: 10.1.0.0/24
   * **Předplatné**: (vybrat odběr, použitý k vytvoření této virtuální sítě.)
   * **ResourceGroup**: contosoaadrg
   * **Umístění**: (vyberte oblast pro váš cluster HDInsight.)
     
     > [!IMPORTANT]
     > Musíte zvolit umístění, které podporuje Azure AD DS. Další informace najdete v tématu [Dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/en-us/regions/services/). 
     > 
     > Obě hello klasické virtuální sítě a hello VNet skupiny prostředků musí být v hello stejné oblasti jako hello služby Azure AD DS.
     > 
     > 
5. Klikněte na tlačítko **vytvořit** toocreate hello virtuální sítě.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Vytvoření a konfiguraci služby Azure AD DS pro vaši službu Azure AD
V této části provedete následující:

1. Vytvořte Azure AD.
2. Vytvořte uživatele Azure AD. Tito uživatelé jsou uživatele domény. První uživatel hello se používá pro konfiguraci clusteru HDInsight hello s hello Azure AD.  Hello další dva uživatelé jsou volitelné pro účely tohoto kurzu. Bude použita v [Hive nakonfigurovat zásady pro clustery služby HDInsight připojený k doméně](hdinsight-domain-joined-run-hive.md) při konfiguraci zásad Apache škálu.
3. Vytvoření skupiny Administrators řadič domény AAD hello a přidat skupinu toohello uživatelů Azure AD hello. Je-li použít tuto organizační jednotku uživatele toocreate hello.
4. Povolení služby Azure AD Domain Services (Azure AD DS) pro hello Azure AD.
5. Nakonfigurujte LDAPS pro hello Azure AD. Hello přístup protokolu LDAP (Lightweight Directory) je použité tooread z a zapisovat tooAzure AD.

Pokud dáváte přednost toouse na stávající služby Azure AD, můžete přeskočit kroky 1 a 2.

**toocreate na Azure AD**

1. Z hello [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **nový** > **App Services** > **služby Active Directory**  >  **Directory** > **vytvořit vlastní**. 
2. Zadejte nebo vyberte hello následující hodnoty:
   
   * **Název**: contosoaaddirectory
   * **Název domény**: contoso.  Tento název musí být globálně jedinečný.
   * **Země nebo oblast**: vyberte vaši zemi nebo oblast.
3. Klikněte na **Dokončit**.

**Vytvořit uživatele Azure AD**

1. Z hello [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **služby Active Directory** -> **contosoaaddirectory**. 
2. Klikněte na tlačítko **uživatelé** hello hlavní nabídce.
3. Klikněte na tlačítko **přidat uživatele**.
4. Zadejte **uživatelské jméno**a potom klikněte na **Další**. 
5. Konfigurace profilu uživatele; V **Role**, vyberte **globálního správce**; a potom klikněte na **Další**.  role globálního správce Hello je potřebné toocreate organizační jednotky.
6. Klikněte na tlačítko **vytvořit** tooget dočasné heslo.
7. Vytvořit kopii hello heslo a potom klikněte na **Complete**. Později v tomto kurzu použijete tento cluster globálního správce uživatele toocreate hello HDInsight.

Postupujte podle hello stejný postup toocreate dva další uživatelé s hello **uživatele** role, hiveuser1 a hiveuser2. Hello následující uživatele bude použita v [Hive nakonfigurovat zásady pro clustery služby HDInsight připojený k doméně](hdinsight-domain-joined-run-hive.md).

**toocreate hello AAD řadič domény správci skupiny a přidat uživatele Azure AD**

1. Z hello [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **služby Active Directory** > **contosoaaddirectory**. 
2. Klikněte na tlačítko **skupiny** hello hlavní nabídce.
3. Klikněte na tlačítko **přidat skupinu** nebo **přidat skupinu**.
4. Zadejte nebo vyberte hello následující hodnoty:
   
   * **Název**: Správci AAD řadič domény.  Název skupiny hello se nezmění.
   * **Typ skupiny**: zabezpečení.
5. Klikněte na **Dokončit**.
6. Klikněte na tlačítko **AAD řadič domény správci** tooopen hello skupiny.
7. Klikněte na tlačítko **přidat členy**.
8. Vyberte první uživatel hello jste vytvořili v předchozím kroku hello a pak klikněte na tlačítko **Complete**.
9. Opakování hello stejné kroky toocreate jiná skupina s názvem **HiveUsers**, a přidejte hello dva Hive uživatelé toohello skupiny.

Další informace najdete v tématu [Azure AD Domain Services (Preview) – vytvořit hello 'správci AAD řadič domény, skupina](../active-directory-domain-services/active-directory-ds-getting-started.md).

**tooenable služby Azure AD DS pro vaši službu Azure AD**

1. Z hello [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **služby Active Directory** > **contosoaaddirectory**. 
2. Klikněte na tlačítko **konfigurace** hello hlavní nabídce.
3. Posuňte se dolů příliš**Domain Services**, a sadu hello následující hodnoty:
   
   * **Povolit doménové služby pro tento adresář**: Ano.
   * **Název domény DNS služby domain services**: zobrazuje hello výchozí název DNS hello Azure directory. Například contoso.onmicrosoft.com.
   * **Připojit virtuální síť toothis domény služby**: Vyberte hello klasickou virtuální síť, které jste vytvořili dříve, tj. **contosoaadvnet**.
4. Klikněte na tlačítko **Uložit** hello dolní části stránky hello. Zobrazí se **čekající na vyřízení...**  další příliš**povolit doménové služby pro tento adresář**.  
5. Počkejte na **čekající na vyřízení...**  zmizí, a **IP adresu** získá naplněno. Dvě IP adresy budou obsazeny. Toto jsou hello IP adresy řadičů domény hello vytváří Domain Services. Každou IP adresu budou viditelné po hello odpovídající řadič domény je zřízený a připravený. Poznamenejte si hello dvě IP adresy. Ty budete potřebovat později.

Další informace najdete v tématu [Azure AD Domain Services (Preview) – povolit Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-enableaadds.md).

**toosynchronize heslo**

Pokud chcete použít vlastní doménu, potřebujete toosynchronize hello heslo. V tématu [povolit heslo synchronizace tooAzure AD domain services pro Azure výhradně cloudový adresář AD](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**tooconfigure LDAPS pro hello Azure AD**

1. Získejte certifikát SSL, který je podepsaný podpisový autoritou pro vaši doménu. Pokud chcete toouse certifikát podepsaný svým držitelem, prosím oslovení toohdipreview@microsoft.com pro výjimku.
2. Z hello [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **služby Active Directory** > **contosoaaddirectory**. 
3. Klikněte na tlačítko **konfigurace** hello hlavní nabídce.
4. Posuňte se příliš**služby domény**.
5. Klikněte na tlačítko **konfigurace certifikátu**.
6. Postupujte podle soubor certifikátu hello instrukce toospecify hello a hello heslo. Zobrazí se **čekající na vyřízení...**  další příliš**povolit doménové služby pro tento adresář**.  
7. Počkejte na **čekající na vyřízení...**  zmizí, a **zabezpečení certifikátu LDAP** tu naplněno.  To může trvat až 10 minut nebo déle.

> [!NOTE]
> Pokud některé úlohy na pozadí běží na hello služby Azure AD DS, zobrazí chybu při odesílání certifikát – <i>je operace prováděna pro tohoto klienta. Zkuste to prosím znovu později</i>.  V případě, že dojde k této chybě, zkuste to prosím po nějaké době znovu. Druhá doména Hello IP řadič může trvat až toobe hodin too3 zřízený.
> 
> 

Další informace najdete v tématu [konfigurace zabezpečení protokolu LDAP (LDAPS) pro Azure AD Domain Services spravované domény](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Vytvoření virtuální sítě Resource Manageru pro HDInsight cluster
V této části vytvoříte o virtuální síť Azure Resource Manager, který se použije pro hello HDInsight cluster. Další informace o vytváření virtuální sítě Azure pomocí jiných metod najdete v tématu [vytvoření virtuální sítě](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)

Po vytvoření hello virtuální síť, můžete nakonfigurovat hello virtuální sítě Resource Manageru toouse hello stejné servery DNS jako pro hello virtuální síť Azure AD. Pokud jste postupovali podle kroků hello v tento kurz toocreate hello klasické virtuální sítě a hello Azure AD, jsou servery DNS hello 10.1.0.4 a 10.1.0.5.

**toocreate virtuální sítě Resource Manageru**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **nový**, **sítě**a potom **virtuální síť**. 
3. V **vybrat model nasazení**, vyberte **Resource Manager**a potom klikněte na **vytvořit**.
4. Zadejte nebo vyberte hello následující hodnoty:
   
   * **Název**: contosohdivnet
   * **Adresní prostor**: 10.2.0.0/16. Zajistěte, aby rozsah adres hello se nesmí překrývat s rozsah IP adres hello hello klasické virtuální sítě.
   * **Název podsítě**: Subnet1
   * **Rozsah adres podsítě**: 10.2.0.0/24
   * **Předplatné**: (vyberte předplatné Azure.)
   * **Skupina prostředků**: contosohdirg
   * **Umístění**: (vyberte hello stejné umístění jako virtuální síť Azure AD, tj. contosoaadvnet hello.)
5. Klikněte na možnost **Vytvořit**.

**tooconfigure DNS pro hello virtuální sítě Resource Manageru**

1. Z hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **další služby** -> **virtuální sítě**. Ujistěte se, není tooclick **virtuální sítě (klasické)**.
2. Klikněte na tlačítko **contosohdivnet**.
3. Klikněte na tlačítko **servery DNS** z hello levé straně hello nové okno.
4. Klikněte na tlačítko **vlastní**a pak zadejte hello následující hodnoty:
   
   * 10.1.0.4
   * 10.1.0.5
     
     Tyto IP adresy serveru DNS se musí shodovat toohello servery DNS v hello virtuální síť Azure AD (klasické virtuální sítě).
5. Klikněte na **Uložit**.

## <a name="peer-hello-azure-ad-vnet-and-hello-hdinsight-vnet"></a>Sdílené hello virtuální síť Azure AD a hello HDInsight virtuální sítě
**toopeer hello dvě virtuální sítě**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **další služby** hello levé nabídce.
3. Klikněte na tlačítko **virtuální sítě**. Nemáte klikněte na tlačítko **virtuální sítě (klasické)**.
4. Klikněte na tlačítko **contosohdivnet**.  Toto je hello HDInsight virtuální sítě.
5. Klikněte na tlačítko **partnerských vztahů** v levé nabídce hello hello okna.
6. Klikněte na tlačítko **přidat** hello hlavní nabídce. Otevře se hello **přidat partnerský vztah** okno.
7. Na hello **partnerský vztah přidat** okno, nastavte nebo vybrat možnost hello následující hodnoty:
   
   * **Název**: ContosoAADHDIVNetPeering
   * **Virtuální síť modelu nasazení**: Classic
   * **Předplatné**: vyberte název odběru použít pro virtuální síť hello classic (Azure AD).
   * **Virtuální síť**: contosoaadvnet.
   * **Povolit přístup k virtuální síti**: (zkontrolujte, zda)
   * **Povolit přesměrovaných přenosů**: (zkontrolujte, zda). Nechte hello dalších dvou políček nezaškrtnuté.
8. Klikněte na **OK**.

## <a name="create-hdinsight-cluster"></a>Vytvoření clusteru HDInsight
V této části vytvoříte clusteru systémem Linux Hadoop v HDInsight pomocí portálu Azure buď hello nebo [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). Další metody vytváření clusterů a principy hello nastavení, najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Další informace o používání správce prostředků šablony toocreate Hadoop clusterů v HDInsight, najdete v části [vytvoření Hadoop clusterů v HDInsight pomocí šablony Resource Manageru](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**hello toocreate clusteru HDInsight připojený k doméně pomocí portálu Azure**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **nový**, **Intelligence + analýzy**a potom **HDInsight**.
3. Z hello **clusteru HDInsight se nové** okno, zadejte nebo vyberte hello následující hodnoty:
   
   * **Název clusteru**: Zadejte nový název clusteru pro cluster HDInsight připojený k doméně hello.
   * **Předplatné**: Vyberte předplatné Azure použitý k vytvoření tohoto clusteru.
   * **Konfigurace clusteru**:
     
     * **Typ clusteru**: Hadoop. Připojené k doméně HDInsight je aktuálně pouze podporované na Hadoop clusterů.
     * **Operační systém**: Linux.  Připojené k doméně HDInsight je podporována pouze na clusterech HDInsight se systémem Linux.
     * **Verze**: HDI 3.6. Připojené k doméně HDInsight je podporována pouze na verzi clusteru HDInsight 3.6.
     * **Typ clusteru**: PREMIUM
       
       Klikněte na tlačítko **vyberte** toosave hello změny.
   * **Přihlašovací údaje**: Nakonfigurujte hello přihlašovací údaje pro uživatele hello clusteru a uživatel SSH hello.
   * **Zdroj dat**: Vytvořte nový účet úložiště nebo použít stávající účet úložiště, jako výchozí účet úložiště pro hello HDInsight cluster hello. Hello umístění musí být hello stejné jako hello dvě virtuální sítě.  umístění Hello je také umístění hello hello clusteru HDInsight.
   * **Ceny**: Vyberte hello počet uzlů pracovního procesu ve vašem clusteru.
   * **Pokročilé konfigurace**: 
     
     * **Připojení k doméně & virtuální sítě a podsítě**: 
       
       * **Nastavení domény**: 
         
         * **Název domény**: contoso.onmicrosoft.com
         * **Uživatelské jméno domény**: Zadejte uživatelské jméno domény. Tato doména musí mít následující oprávnění hello: připojení k doméně počítače toohello a umístit je hello organizační jednotka je zadat při vytváření clusteru; Vytvořit objekty služby v rámci hello organizační jednotka, kterou zadáte během vytváření clusteru; Vytvořte reverzní záznamy DNS. Tento uživatel domény bude hello správce tohoto clusteru HDInsight připojený k doméně.
         * **Heslo domény**: Zadejte heslo uživatele domény hello.
         * **Organizační jednotka**: zadejte rozlišující název hello hello organizační jednotky, které chcete toouse s clusterem HDInsight. Například: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com. Pokud tuto organizační jednotku neexistuje, pokusí clusteru HDInsight toocreate tuto organizační jednotku. Zajistěte, aby se již nachází hello organizační jednotky nebo hello doménový účet má oprávnění toocreate novou. Pokud používáte hello doménový účet, který je součástí AADDC správci, bude mít nezbytná oprávnění toocreate hello organizační jednotky.
         * **Adresa URL LDAPS**: ldaps://contoso.onmicrosoft.com:636
         * **Skupina uživatelů přístup**: Zadejte skupinu zabezpečení hello uživatele, jehož chcete toosync toohello clusteru. Například HiveUsers.
           
           Klikněte na tlačítko **vyberte** toosave hello změny.
           
           ![Portál HDInsight připojený k doméně nakonfigurovat nastavení domény](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuální síť**: contosohdivnet
       * **Podsíť**: Subnet1
         
         Klikněte na tlačítko **vyberte** toosave hello změny.        
         Klikněte na tlačítko **vyberte** toosave hello změny.
   * **Skupina prostředků**: Skupina prostředků vyberte hello používá pro hello HDInsight virtuální síť (contosohdirg).
4. Klikněte na možnost **Vytvořit**.  

Další možnost pro vytvoření clusteru HDInsight se připojený k doméně je šablona toouse Správa prostředků Azure. Hello následující postup ukazuje, jak:

**toocreate cluster HDInsight se připojený k doméně pomocí šablony Správa prostředků**

1. Klikněte na tlačítko hello následující tooopen image šablony Resource Manageru v hello portálu Azure. šablony Resource Manageru Hello se nachází v kontejneru veřejného objektu blob. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Z hello **parametry** okno, zadejte hello následující hodnoty:
   
   * **Předplatné**: (vyberte předplatné Azure).
   * **Skupina prostředků**: klikněte na tlačítko **použít existující**a zadejte hello stejnou skupinu prostředků, které jste dosud používali.  Například contosohdirg. 
   * **Umístění**: Zadejte umístění skupiny prostředků.
   * **Název clusteru**: Zadejte název pro cluster Hadoop hello, který chcete vytvořit. Například contosohdicluster.
   * **Typ clusteru**: Vyberte typ clusteru.  Hello výchozí hodnota je **hadoop**.
   * **Umístění**: Vyberte umístění pro hello cluster.  Hello výchozí účet úložiště, že používá hello stejné umístění.
   * **Počet uzlů pracovního procesu clusteru**: Vyberte hello počet uzlů pracovního procesu.
   * **Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je **správce**.
   * **SSH uživatelské jméno a heslo**: výchozí uživatelské jméno hello **sshuser**.  Můžete ho změnit. 
   * **Id virtuální sítě**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >
   * **Podsíť virtuální sítě**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >/podsítě/Subnet1
   * **Název domény**: contoso.onmicrosoft.com
   * **Rozlišující název organizační jednotky**: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
   * **Skupiny uživatelů clusteru DNs**: [\"HiveUsers\"]
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (zadejte hello domény uživatelské jméno správce)
   * **Skupině**: (zadejte heslo uživatele správce domény hello)
   * **Souhlasím toohello podmínky a ujednání, které jsou uvedené výše**: (zkontrolujte, zda)
   * **PIN kód toodashboard**: (zkontrolujte, zda)
3. Klikněte na **Koupit**. Zobrazí se nová dlaždice s názvem **Nasazení šablony**. Trvá přibližně 20 minut toocreate cluster. Po vytvoření clusteru hello můžete kliknout na okno hello clusteru v portálu tooopen hello ho.

Po dokončení kurzu hello můžete chtít toodelete hello clusteru. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že hello poplatky za hello clusteru jsou mnohokrát víc než hello poplatky za úložiště, dává ekonomický smysl clustery toodelete které nejsou používána. Pokyny hello odstranění clusteru najdete v tématu [hello spravovat Hadoop clusterů v HDInsight pomocí portálu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Pomocí tooconnect SSH, připojený k tooDomain clusterů HDInsight, naleznete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

