# Úkol #1: PFL067 Statistické metody zpracování přirozeného jazyka

## 1. Entropie textu

V tomto experimentu určíte podmíněnou entropii distribuce slov v textu danou předchozím slovem. K tomu budete potřebovat nejprve spočítat $P(i,j)$, což je pravděpodobnost, že v jakékoli pozici v textu najdete slovo $i$ a bezprostředně za ním slovo $j$ a $P(j|i)$, což je pravděpodobnost, že v případě výskytu slova $i$ v textu následuje slovo $j$. Podmíněnou entropii distribuce slov v textu danou předchozím slovem, můžeme spočítat jako:
$$
H(J|I) = -\sum_{i\in I,j\in J}P(i,j)log_2 P(j|i)
$$
Perplexity se spočítá jako:
$$
PX(P(J|I)) = 2^{H(J|I)}
$$
Vypočtěte podmíněnou entropii a perplexity pro $\verb|TEXTEN1.txt|$.

Tento soubor má každé slovo na samostatném řádku. (Interpunkce je považována za slovo, stejně jako v mnoha jiných případech.) $i$ a $j$ uvedené výše budou také překrývat větné hranice, kde $i$ je poslední slovo jedné věty a $j$ je první slovo následující věty (ale zřejmě tam bude tečka na konci většiny vět). 

Dále budete poškozovat text a měřit, jak to změní podmíněnou entropii. Každý znak v textu poškodíme s pravděpodobností $10\%$. Každý vybraný znak změníme na náhodný znak z množiny znaků vyskytujících se v textu. Vzhledem k náhodě v experimentu, spusťte pokus 10 krát a vraťte minimální, maximální a střední hodnotu entropie. Ujistěte se, že nepoužíváte stejné semínko random generátoru, při opakování experimentu a také se ujistěte, že poškozujete originální text a ne již dříve poškozený. Proveďte stejný experiment pro pravděpodobnosti vybrání znaku $5\%$, $1\%$, $0.1\%$, $0.01\%$, $0.001\%$.

Dále pro každé slovo v textu s pravděpodobností $10\%$ vyberte slovo a náhodně ho zaměňte za jiné z množiny slov vyskytujících se v textu. Opět spusťte experiment 10 krát a vraťte minimální, maximální a střední hodnotu entropie. Proveďte stejný experiment pro pravděpodobnosti vybrání slova $5\%$, $1\%$, $0.1\%$, $0.01\%$, $0.001\%$.

Nyní udělejte přesně totéž pro soubor $\verb|TEXTCZ1.txt|$, který obsahuje pobodné množství textu.

Vytvořte tabulky, graf a vysvětlete své výsledky. Také se pokuste vysvětlit rozdíly mezi těmito dvěma jazyky. K podložení svého vysvětlení bude dobré udělat tabulky se základními charakteristikami těchto dvou textů (počet slov, počet znaků (celkový, na jedno slovo), frekvence nejčastějších slov, počet slov s frekvencí 1, atd).

Přiložte svůj zdrojový kód komentovaný takovým způsobem, že bude stačit přečtení komentářů na porozumění tomu, co jste udělali a jak jste to udělali.

Nyní předpokládejme, že dva jazyky $L_1$ a $L_2$ nesdílejí žádná slovíčka, a že podmíněná entropie, jak je popsáno výše, textu $T_1$ v jazyce $L_1$ je $E$, a že podmíněná entropie textu $T_2$ v jazyce $L_2$ je také $E$. Nyní vytvořte nový text přidáním $T_2$ za $T_1$. Bude podmíněná entropie tohoto nového textu větší, menší nebo rovna hodnotě $E$? Vysvětlete.

## 2. Cross-Entropie a modelování jazyka

Tento úkol Vám ukáže, že je důležité vyhlazování pro modelování jazyka a v některých detailech vám umožní pocítit jeho účinky.

Za prvé budete muset připravit data: vezměte stejná data jako v předchozí úloze, tj. $\verb|TEXTEN1.txt|$ a $\verb|TEXTCZ1.txt|$.

Připravte si 3 datasety: Posledních $20\,000$ slov nazvěte testovací data, posledních $40\,000$ slov ze zbytku nazvěte heldout a zbytek budou trénovací data.

Tady přichází programování: Spočítejte počet trénovacích slov. Nyní jste připraveni na výpočet unigram, bigram a trigram pravděpodobností. Vypočítejte také pravděpodobnost v závislosti na slovní zásobě. Nezapomeňte ($T$ je velikost textu a $V$ velikost slovní zásoby, tj. počet různých slovních tvarů nalezených v trénovacích datech):


$$
\begin{eqnarray*}
p_0(w_i) &=& \dfrac{1}{V}\\
p_1(w_i) &=& \dfrac{c_1(w_i)}{T}\\
p_2(w_i|w_{i-1}) &=& \dfrac{c_2(w_{i-1},w_i)}{c_1({w_{i-1})}}\\
p_3(w_i|w_{i-2},w_{i-1}) &=& \dfrac{c_3(w_{i-2},w_{i-1},w_i)}{c_2(w_{i-2},w_{i-1})}
\end{eqnarray*}
$$
**Buď opatrný**; Vzpomeň si, jak správně zacházet se začátkem a koncem trénovacích dat s ohledem na počítání bigramu a trigramu.

Nyní vypočtěte čtyři vyhlazovací parametry (tj. koeficienty, váhy, lambdy, parametry interpolace, nebo cokoli na trigram bigram unigram a uniform distribuci) z heldout dat algoritmem EM. (Pak udělejte totéž s použitím trénovacích dat: jaké vyhlazovací koeficienty jste dostali? Po odpovědi na tuto otázku je zahoďte!). Vzpomeňte si, že vyhlazovací model má následující podobu:
$$
p_s(w_i|w_{i-2},w_{i-1}) = l_0p_0(w_i) + l_1p_1(w_i) + l_2p_2(w_i|w_{i-1})+l_3p_3(w_i|w_{i-2},w_{i-1})
$$
kde
$$
l_0+l_1+l_2+l_3 = 1
$$


A konečně vypočítat cross-entropii testovacích dat s využitím nově vybudovaného vyhlazeného jazykového modelu. Nyní poupravte parametry vyhlazení následujícím způsobem: přidat $10\%$, $20\%$, $\ldots$,$90\%$, $95\%$ a $99\%$ rozdílu mezi trigram vyhlazení a $1.0$ k jeho hodnotě, proporciálně uprav zbývající tři parametry (připomeňme, že mají součet 1!!). Potom nastavte trigram vyhlazovací parametr na $90\%$, $80\%$, $\ldots$, $10\%$, $0\%$ z jeho hodnoty a proporciálně uprav zbývající parametry. Vypočítejte cross-entropii na testovacích datech pro těchto 22 příkladů (původní + 11 zvýšení trigram parametru + 10 snížení trigram vyhlazovacího parametru). Vytvořte tabulky a grafy a vysvětlete výsledky. Také se snažte vysvětlit rozdíly mezi těmito dvěma jazyky na základě podobných statistik jako v úloze č.2, plus graf pokrytí (definovaný jako procento slov v testovacích datech, které byly pozorovány v trénovacích datech).

Přiložte svůj zdrojový kód komentovaný takovým způsobem, že bude stačit přečtení komentářů na porozumění tomu, co jste udělali a jak jste to udělali.