---
title: "aaaConfigure clusterů HDInsight připojený k doméně pomocí prostředí PowerShell – Azure | Microsoft Docs"
description: "Zjistěte, jak tooset registrace a konfigurace clusterů HDInsight připojený k doméně pomocí prostředí Azure PowerShell"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a13b2f7a-612d-4800-bc92-7fc0524f3e89
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 49da3439513d1e51171f0f7f7f9c3d967d55cb7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview-using-azure-powershell"></a>Konfigurace clusterů HDInsight připojený k doméně (Preview) pomocí prostředí Azure PowerShell
Zjistěte, jak cluster tooset si Azure HDInsight s Azure Active Directory (Azure AD) a [Apache škálu](http://hortonworks.com/apache/ranger/) pomocí Azure PowerShell. Skript Azure PowerShell poskytuje toomake hello konfigurace rychlejší a méně náchylný. HDInsight připojený k doméně se dá nakonfigurovat jenom na clusterech se systémem Linux. Další informace najdete v tématu [clustery HDInsight připojený k doméně zavádět](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie není povoleno v doméně HDInsight.

Obvyklá konfigurace clusteru připojený k doméně HDInsight zahrnuje hello následující kroky:

1. Vytvoření Azure klasické virtuální sítě pro vaši službu Azure AD.  
2. Vytvoření a konfigurace Azure AD a Azure AD DS.
3. Přidat virtuální počítač toohello klasické virtuální sítě pro vytvoření organizační jednotky. 
4. Vytvořte organizační jednotku pro Azure AD DS.
5. Vytvořte virtuální síť HDInsight v režimu správy hello prostředků Azure.
6. Instalační program reverzním systému DNS zóny pro hello služby Azure AD DS.
7. Sdílené hello dvě virtuální sítě.
8. Vytvoření clusteru HDInsight.

Hello skript prostředí PowerShell zadat provádí kroky 3 až 7. Musíte přejít do kroku 1 a 2 ručně.  Pokud dáváte přednost toouse prostředí Azure PowerShell, najdete v části [clustery HDInsight připojený k doméně nakonfigurovat](hdinsight-domain-joined-configure.md). 

Příklad konečné topologie hello vypadá takto:

![Topologie HDInsight připojený k doméně](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Protože Azure AD aktuálně podporuje pouze klasické virtuální sítě (virtuální sítě) a podporuje se jen clustery HDInsight se systémem Linux Azure Resource Manager na základě virtuálních sítí, integrace HDInsight Azure AD vyžaduje dvě virtuální sítě a partnerský vztah mezi nimi. Informace o porovnání hello mezi hello dva modely nasazení najdete v tématu [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a hello stav svých prostředků](../azure-resource-manager/resource-manager-deployment-model.md). Hello dvou virtuálních sítí musí být v hello stejné oblasti jako hello služby Azure AD DS.

> [!NOTE]
> Tento kurz předpokládá, že nemáte Azure AD. Pokud nemáte, můžete přeskočit část hello v kroku 2.
> 
> 

## <a name="prerequisites"></a>Požadavky
Hello následující položky toogo prostřednictvím tohoto kurzu musíte mít:

* Seznamte se s [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) jeho [ceny](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktura.
* Zajistěte, aby vaše předplatné seznam povolených adres pro tuto verzi public preview. Můžete to provést pomocí e-mailu toohdipreview@microsoft.com s vaším ID předplatného.
* Certifikát SSL, který je podepsaný podpisový autoritou pro vaši doménu. Hello certifikát je vyžadován nakonfigurováním zabezpečený LDAP. Nelze použít certifikáty podepsané svým držitelem.
* Azure Powershell  V tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).

## <a name="create-an-azure-classic-vnet-for-your-azure-ad"></a>Vytvoření Azure klasické virtuální sítě pro vaši službu Azure AD.
Hello pokyny najdete v tématu [zde](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic).

## <a name="create-and-configure-azure-ad-and-azure-ad-ds"></a>Vytvoření a konfigurace Azure AD a Azure AD DS.
Hello pokyny najdete v tématu [zde](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).

## <a name="run-hello-powershell-script"></a>Spustit skript prostředí PowerShell hello
Hello skript prostředí PowerShell si můžete stáhnout z [Githubu](https://github.com/hdinsight/DomainJoinedHDInsight). Extrahujte soubor zip hello a uložte soubory hello místně.

**tooedit hello skript prostředí PowerShell**

1. Pomocí Windows PowerShell ISE nebo libovolného textového editoru otevřete run.ps1.
2. Zadejte hodnoty hello hello následující proměnné:
   
   * **$SubscriptionName** – hello název hello předplatné Azure, kde se má toocreate clusteru HDInsight. Jste již vytvořili klasické virtuální síti v tomto předplatném a bude vytvářet virtuální síť Azure Resource Manageru pro cluster HDInsight hello v rámci předplatného.
   * **$ClassicVNetName** -hello klasické virtuální síti, která obsahuje hello služby Azure AD DS. Tato virtuální síť musí být v hello stejného předplatného, které je uvedené výše. Tato virtuální síť musí být vytvořena pomocí hello portálu Azure a není pomocí portálu classic. Pokud budete postupovat podle pokynů hello v [clustery HDInsight připojený k doméně nakonfigurovat (Preview)](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic), hello výchozí název je contosoaadvnet.
   * **$ClassicResourceGroupName** – hello Resource Manager název skupiny pro hello klasické virtuální síť, která je uvedený výše. Například contosoaadrg. 
   * **$ArmResourceGroupName** – hello název skupiny prostředků v rámci, které chcete toocreate hello HDInsight cluster. Můžete použít hello stejné skupině prostředků jako $ArmResourceGroupName.  Pokud skupina prostředků hello neexistuje, vytvoří skript hello hello skupinu prostředků.
   * **$ArmVNetName** -název virtuální sítě Resource Manager hello, ve kterém chcete clusteru HDInsight toocreate hello. Tato virtuální síť se umístí do $ArmResourceGroupName.  Pokud hello virtuální síť neexistuje, hello skript prostředí PowerShell ji vytvoří. Když neexistuje, měla by být součástí hello skupiny prostředků, které zadáváte výše.
   * **$AddressVnetAddressSpace** – hello sítě adresního prostoru virtuální sítě Resource Manager hello. Ujistěte se, že tento adresní prostor je k dispozici. Tento adresní prostor se nesmí překrývat hello klasické virtuální sítě adresní prostor. Například "10.1.0.0/16"
   * **$ArmVnetSubnetName** -hello Resource Manager název podsítě virtuální sítě v rámci kterého chcete clusteru HDInsight hello tooplace virtuálních počítačů. Pokud podsíť hello neexistuje, hello skript prostředí PowerShell ji vytvoří. Pokud neexistuje, je nutné součást hello virtuální síť, kterou zadáte výše.
   * **$AddressSubnetAddressSpace** – hello sítě rozsah adres pro podsíť virtuální sítě Resource Manager hello. Hello virtuálních počítačů pro IP adresy, které budou probíhat od tento rozsah adres podsítě clusteru HDInsight. Například "10.1.0.0/24".
   * **$ActiveDirectoryDomainName** – název domény hello Azure AD, které chcete toojoin hello HDInsight clusteru virtuálních počítačů do. Například "contoso.onmicrosoft.com"
   * **$ClusterUsersGroups** – běžný název skupiny zabezpečení hello ze služby AD, které chcete toosync toohello HDInsight cluster hello. Hello uživatelé v této skupině zabezpečení nebudou mít toolog na řídicí panel clusteru toohello pomocí svých přihlašovacích údajů domény služby active directory. Tyto skupiny zabezpečení musí existovat ve službě active directory hello. Například "hiveusers" nebo "clusteroperatorusers".
   * **$OrganizationalUnitName** -hello organizační jednotku v doméně hello, ve kterém chcete clusteru HDInsight hello tooplace virtuální počítače a hello objekty služby používá hello cluster. Hello skript prostředí PowerShell vytvoří tuto organizační jednotku, pokud neexistuje. Například "HDInsightOU".
3. Uložte změny hello.

**toorun hello skriptu**

1. Spustit **prostředí Windows PowerShell** jako správce.
2. Vyhledejte složku toohello run.ps1. 
3. Spusťte skript hello zadáním názvu souboru text hello a stiskněte tlačítko **ENTER**.  Objeví 3 přihlášení dialogová okna:
   
   1. **Přihlášení na portálu classic tooAzure** – zadejte své přihlašovací údaje, které používáte toosign tooAzure klasickém portálu. Musí mít vytvořit hello Azure AD a Azure AD DS pomocí těchto přihlašovacích údajů.
   2. **Přihlášení portálu správce prostředků tooAzure** – zadejte své přihlašovací údaje, které používáte toosign portálu tooAzure Resource Manager.
   3. **Uživatelské jméno domény** – zadejte přihlašovací údaje hello hello doména uživatelské jméno, které chcete toobe správce na clusteru HDInsight hello. Pokud jste vytvořili Azure AD od začátku, vytvořili jste musí tento uživatel použijete tuto dokumentaci. 
      
      > [!IMPORTANT]
      > Zadejte uživatelské jméno hello v tomto formátu: 
      > 
      > Název_domény\uživatelské_jméno (například contoso.onmicrosoft.com\clusteradmin)
      > 
      > 
      
      Tento uživatel musí mít oprávnění pro 3: toojoin počítače toohello zadané domény služby Active Directory. objekty služby toocreate a objekty počítačů v rámci hello Zadaná organizační jednotka; a tooadd zpětné DNS proxy pravidla.

Při vytváření zpětné zóny DNS, hello skript zobrazí výzvu tooenter síť ID. Toto ID sítě musí být předponou adresy hello Resource Manager virtuální sítě. Například pokud vaše adresního prostoru podsítě virtuální sítě Resource Manager je 10.2.0.0/24, zadejte 10.2.0.0/24, když nástroj hello vás vyzve k zadání ID hello sítě. 

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
     * **Verze**: Hadoop 2.7.3 (HDI 3.5). Připojené k doméně HDInsight je podporována pouze na verzi clusteru HDInsight 3.5.
     * **Typ clusteru**: PREMIUM
       
       Klikněte na tlačítko **vyberte** toosave hello změny.
   * **Přihlašovací údaje**: Nakonfigurujte hello přihlašovací údaje pro uživatele hello clusteru a uživatel SSH hello.
   * **Zdroj dat**: Vytvořte nový účet úložiště nebo použít stávající účet úložiště, jako výchozí účet úložiště pro hello HDInsight cluster hello. Hello umístění musí být hello stejné jako hello dvě virtuální sítě.  umístění Hello je také umístění hello hello clusteru HDInsight.
   * **Ceny**: Vyberte hello počet uzlů pracovního procesu ve vašem clusteru.
   * **Pokročilé konfigurace**: 
     
     * **Připojení k doméně & virtuální sítě a podsítě**: 
       
       * **Nastavení domény**: 
         
         * **Název domény**: contoso.onmicrosoft.com
         * **Uživatelské jméno domény**: Zadejte uživatelské jméno domény. Tato doména musí mít následující oprávnění hello: připojení k doméně počítače toohello a umístit je hello organizační jednotka jste nakonfigurovali dříve; Vytvořit objekty služby v rámci hello organizační jednotka, kterou jste nakonfigurovali dříve; Vytvořte reverzní záznamy DNS. Tento uživatel domény bude hello správce tohoto clusteru HDInsight připojený k doméně.
         * **Heslo domény**: Zadejte heslo uživatele domény hello.
         * **Organizační jednotka**: zadejte rozlišující název hello hello organizační jednotku, kterou jste nakonfigurovali dříve. Například: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
         * **Adresa URL LDAPS**: ldaps://contoso.onmicrosoft.com:636
         * **Skupina uživatelů přístup**: Zadejte skupinu zabezpečení hello uživatele, jehož jste wan toosync toohello clusteru. Například HiveUsers.
           
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
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure-use-powershell/deploy-to-azure.png" alt="Deploy tooAzure"></a>
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
   * **Skupiny uživatelů clusteru D Ns**: "\"CN HiveUsers, OU = AADDC Users, DC = =<DomainName>, DC = onmicrosoft, DC = com\""
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (zadejte hello domény uživatelské jméno správce)
   * **Skupině**: (zadejte heslo uživatele správce domény hello)
   * **Souhlasím toohello podmínky a ujednání, které jsou uvedené výše**: (zkontrolujte, zda)
   * **PIN kód toodashboard**: (zkontrolujte, zda)
3. Klikněte na **Koupit**. Zobrazí se nová dlaždice s názvem **Nasazení šablony**. Trvá přibližně 20 minut toocreate cluster. Po vytvoření clusteru hello můžete kliknout na okno hello clusteru v portálu tooopen hello ho.

Po dokončení kurzu hello můžete chtít toodelete hello clusteru. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že hello poplatky za hello clusteru jsou mnohokrát víc než hello poplatky za úložiště, dává ekonomický smysl clustery toodelete které nejsou používána. Pokyny hello odstranění clusteru najdete v tématu [hello spravovat Hadoop clusterů v HDInsight pomocí portálu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Další kroky

* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Pomocí tooconnect SSH, připojený k tooDomain clusterů HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

