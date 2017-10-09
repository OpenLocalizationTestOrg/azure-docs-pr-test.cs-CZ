V tomto kroku vytvoříte testu port brány firewall pravidla tooopen hello endpoint hello s vyrovnáváním zatížení (59999, jak je uvedeno výše) a jiné pravidlo port naslouchacího procesu skupiny dostupnosti hello tooopen. Vzhledem k tomu, že jste vytvořili koncový bod Vyrovnávání zatížení sítě hello na hello virtuálních počítačů, které obsahují replik skupin dostupnosti, je nutné port testu hello tooopen a port naslouchacího procesu hello na hello příslušných virtuálních počítačů.

1. Na virtuálních počítačích, které jsou hostiteli repliky, spusťte **brány Windows Firewall s pokročilým zabezpečením**.

2. Klikněte pravým tlačítkem na **příchozí pravidla**a potom klikněte na **nové pravidlo**.

3. Na hello **typ pravidla** vyberte **Port**a potom klikněte na **Další**.

4. Na hello **protokol a porty** vyberte **TCP**, typ **59999** v hello **určité místní porty** pole a pak klikněte na **Další**.

5. Na hello **akce** ponechte **povolit připojení hello** vybrané a potom klikněte na **Další**.

6. Na hello **profil** přijměte hello výchozí nastavení a pak klikněte na tlačítko **Další**.

7. Na hello **název** stránku hello **název** textové pole, zadejte název pravidla, například **vždy na Port naslouchacího procesu sběru dat**a potom klikněte na **Dokončit**.

8. Opakujte hello předchozích kroků pro port naslouchacího procesu skupiny dostupnosti hello (uvedený dříve v parametru hello $EndpointPort skriptu hello) a pak zadejte název příslušné pravidlo, jako například **vždy na Port naslouchacího procesu**.

