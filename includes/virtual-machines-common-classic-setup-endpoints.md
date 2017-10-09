
Každý koncový bod má *veřejný port* a *privátní port*:

* Hello veřejný port je používán toolisten nástroje pro vyrovnávání zatížení Azure hello pro příchozí provoz toohello virtuální počítač z hello Internetu.
* Hello privátní port je používán toolisten hello virtuálního počítače pro příchozí provoz, obvykle určených tooan aplikace nebo služby, které jsou spuštěny na virtuálním počítači hello.

Výchozí hodnoty pro hello protokol IP a porty TCP nebo UDP pro dobře známé síťové protokoly jsou dostupné při vytváření koncových bodů s hello portálu Azure. Pro vlastní koncové body budete potřebovat toospecify hello správný protokol IP (TCP nebo UDP) a hello veřejné a privátní porty. příchozí provoz toodistribute náhodně napříč více virtuálních počítačů, budete potřebovat toocreate sadu Vyrovnávání zatížení sítě skládající se z více koncových bodů.

Po vytvoření koncového bodu, můžete vytvořit pravidla přístupu ovládacího prvku seznam (ACL) toodefine, která povolení nebo odmítnutí příchozí provoz hello toohello veřejný port koncového bodu hello na základě jeho zdrojové IP adresy. Ale pokud hello virtuální počítač je ve virtuální síť Azure, je vhodnější použít skupiny zabezpečení sítě. Podrobnosti najdete v tématu [o skupinách zabezpečení sítě](../articles/virtual-network/virtual-networks-nsg.md).

> [!NOTE]
> Konfigurace brány firewall pro virtuální počítače Azure se provádí automaticky pro porty, které jsou přidružené k připojení k vzdálené koncové body, které Azure nastaví automaticky. Pro porty určené pro všechny ostatní koncové body žádná konfigurace se provádí automaticky brány firewall toohello hello virtuálního počítače. Když vytvoříte koncový bod pro hello virtuální počítač, budete potřebovat tooensure, který hello brány firewall hello virtuálního počítače umožňuje také hello přenos pro protokol hello a privátní port odpovídající toohello konfigurace koncového bodu. tooconfigure hello brány firewall, naleznete v dokumentaci hello nebo online nápověda pro hello operační systém spuštěný na virtuálním počítači hello.
>
>

## <a name="create-an-endpoint"></a>Vytvoření koncového bodu
1. Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **virtuální počítače**a potom klikněte na název hello hello virtuálního počítače, které chcete tooconfigure.
3. Klikněte na tlačítko **koncové body** v hello **nastavení** skupiny. Hello **koncové body** stránka obsahuje seznam všech hello aktuální koncové body pro hello virtuálního počítače. (V tomto příkladu je virtuální počítač s Windows. Linux virtuálního počítače se ve výchozím nastavení zobrazí koncový bod SSH.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Koncové body](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)

4. V panelu příkazů hello nad hello koncový bod položky, klikněte na **přidat**.
5. Na hello **přidání koncového bodu** stránky, zadejte název pro koncový bod hello v **název**.
6. V **protokol**, vyberte buď **TCP** nebo **UDP**.
7. V **veřejný Port**, zadejte číslo portu hello hello příchozí provoz z Internetu hello. V **privátní Port**, zadejte číslo portu hello, na které hello naslouchá virtuálního počítače. Tato čísla portů se může lišit. Ujistěte se, že hello brány firewall na hello virtuální počítač byl nakonfigurovaná tooallow hello provoz odpovídající toohello protokolu (v kroku 6) a privátní port.
10. Klikněte na tlačítko **OK**.

Nový koncový bod Hello bude uvedený na hello **koncové body** stránky.

![Úspěšné vytvoření koncového bodu](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-hello-acl-on-an-endpoint"></a>Spravovat hello seznamu ACL v koncovém bodě
toodefine hello sadu počítačů, které mohou odesílat provoz, provoz na základě zdrojové IP adresy můžete omezit hello seznamu ACL v koncovém bodě. Postupujte podle těchto kroků tooadd, upravovat nebo odebírat ACL v koncovém bodě.

> [!NOTE]
> Pokud hello koncový bod je součástí skupiny s vyrovnáváním zatížení, provedené změny toohello seznamu ACL v koncovém bodě jsou použité tooall koncové body v sadě hello.
>
>

Pokud hello virtuální počítač je ve virtuální síť Azure, doporučujeme skupin zabezpečení sítě místo seznamy ACL. Podrobnosti najdete v tématu [o skupinách zabezpečení sítě](../articles/virtual-network/virtual-networks-nsg.md).

1. Pokud jste tak již neučinili, přihlaste se toohello portálu Azure.
2. Klikněte na tlačítko **virtuální počítače**a potom klikněte na název hello hello virtuálního počítače, které chcete tooconfigure.
3. Klikněte na **Koncové body**. Hello seznamu vyberte příslušný koncový bod hello. seznamu ACL Hello je na hello dolní části stránky hello.

   ![Zadejte podrobnosti seznamu ACL](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Použijte řádků v seznamu tooadd hello, odstranit nebo upravit pravidla pro seznam ACL a změnit jejich pořadí. Hello **vzdálené podsíti** hodnota je rozsah IP adres pro příchozí provoz z Internetu, který hello toopermit používá nástroj pro vyrovnávání zatížení Azure nebo zakážou provoz hello na základě jeho IP adresy zdrojového hello. Rozsah IP adres se toospecify hello být ve formátu CIDR, také známé jako formát předponu adresy. Příkladem je `10.1.0.0/8`.

 ![Nová položka seznamu ACL](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


Můžete vytvořit pravidla tooallow jenom přenosy z určitých počítačů odpovídající tooyour počítače v Internetu nebo toodeny provoz z rozsahů adres konkrétní, známé hello.

Hello pravidla jsou vyhodnocovány v pořadí od první pravidlo hello a konče hello poslední pravidlo. To znamená, že by měla být z nejméně omezující toomost omezující seřazena pravidla. Příklady a další informace najdete v tématu [co je seznam řízení přístupu sítě](../articles/virtual-network/virtual-networks-acl.md).
