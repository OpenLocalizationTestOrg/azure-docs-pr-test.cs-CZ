Otevřete port, nebo vytvořit koncový bod, tooa virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů. Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě připojené toohello prostředku, který přijímá provoz hello.

Použijeme Běžným příkladem webové přenosy na portu 80. Jakmile je virtuální počítač, který je nakonfigurovaný tooserve webových požadavků na hello standardní port TCP 80 (mějte na paměti, toostart hello odpovídající služby a otevřít všechna pravidla brány firewall operačního systému na hello virtuálních počítačů i), můžete:

1. Vytvořte skupinu zabezpečení sítě.
2. Vytvoření příchozího pravidla umožňuje provoz se:
   * Rozsah cílových portů Hello "80"
   * Rozsah zdrojových portů z Hello "*" (což jakéhokoli zdrojového portu)
   * hodnotou priority menší 65 500 (toobe vyšší priorita, než hello catch všechny výchozí příchozí pravidlo Odepřít)
3. Přidružte hello rozhraní sítě virtuálních počítačů nebo podsíť hello skupinu zabezpečení sítě.

Konfigurace toosecure nejsložitějších sítí můžete vytvořit prostředí pomocí skupin zabezpečení sítě a pravidla. Naše Ukázka používá jenom jednu nebo dvě pravidla, která povolit provoz protokolu HTTP nebo pro vzdálenou správu. Další informace najdete v tématu hello následující ["Další informace"](#more-information-on-network-security-groups) části nebo [co je skupina zabezpečení sítě?](../articles/virtual-network/virtual-networks-nsg.md)

