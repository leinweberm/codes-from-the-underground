# CONTAINER QUERY (css)

> ### CSS Containment
>
> - [co je to CSS containment](#co-je-css-containment)
> - [základní containment vlastnosti](#základní-termíny-a-vlastnosti)
> - [speciální containment vlastnosti](#speciální-vlastnosti)
> - [content-visibility](#content-visibility)
> - [relevantní pro uživatele](#relevant-to-user)
>
> ### Container Query
>
> - [co je to container query](#container-query)
> - [pojmenování contextu](#pojmenování-container-contextu)
> - [zkrácený zápis](#zkrácený-zápis)
> - [jednotky velikosti](#container-query-jednotky)

<br />

## **Co je to CSS Containment?**

CSS containment nám pomáhá optimalizovat performance stránky tím, že prohlížeči řekneme, že tato část webu ja samostatná nezávislá jednotka a prohlížeč tento subtree z DOMu odizoluje.

Vývojář naopak může kontrolovat zda má tento element vyrenderovat svůj obsah či nikoliv, například pokud je element offscreen. Tím, že vyrenderujeme obsah onoho elementu až je skutečně potřeba zvyšujeme performane naší stránky. Modifikovat tyto vlastnosti elementu můžeme pomocí CSS vlastností `contain` a `content-visibility`. Více o CSS container queries se dozvíme [zde](#container-queries).

Jednoduchý příklad<br />
`index.html:`

```html
<h1>My blog</h1>
<article>
	<h2>Heading of a nice article</h2>
	<p>Content here.</p>
</article>
<article>
	<h2>Another heading of another article</h2>
	<p>More content here.</p>
</article>
```

`style.css`

```css
article {
	contain: content;
}
```

Každý `<article>` na stránce je nezávislý na ostatních articlech, protože mají CSS vlastnost **contain** nastavenou na **content**. Prohlížeč tedy může sám vyhodnotit, které articly vyrenderovat a které ne. Ve většine případů nevyrenderuje ty, které jsou mimo viewport návštěvníka. Zároveň obsah articlu je odseparován od zbytku obsahu webu a je ohraničen hranicí elementu articlu, tudíž například nemůže docházet k žádnému viditelnému overflow obsahu. Ačkoliv nám to z hlediska logiky dává přijde samozřejmé, tak prohlížeč takto nebude uvažovat dokud mu to neumožníme.
<br /><br />

## **Základní termíny a vlastnosti**

### **Layout containment**

```css
article {
	contain: layout;
}
```

Za normálních okolností je má layout scope přes celý dokument, což znamená, že pokud se pohne jeden element v layoutu, tak prohlížeč předpokládá, že se mohl změnit celý dokument a celý jej zkontroluje. Pokud použijeme `containt: layout;` tak prohlížeči říkáme, že všechny elementy uvnitř mají scope pouze v rámci tohoto layoutu a stačí zkontrolovat jenom je.

> - marginy vnitřních elementů jsou ohraničeny hranicí layoutu
> - lze použít z-index, layout vytváří vlastní stacking context
> - Okraj layoutu bude hranice pro absolutně a fixně pozicované elementy

<br />

### **Paint containment**

```css
article {
	contain: paint;
}
```

Paint containment vyplní nadřazený div po hranici paddingu. Paint containment neumožňuje viditelný overflow. Je-li element s paint containmentem offscreen, nebude vyrenderovat a stejně tak nebudou vyrenderovány všechny jeho child elementy.

### **Size containment**

```css
article {
	contain: size;
}
```

Samotné použití size containmentu nemá velký efekt na optimalizaci a výkon stránky. Zjednodušeně řečeno funguje tak, že vlastnost jeho child elementů nemá vliv na jeho vlastní velikost. Záchovává si velikost jako by v něm nebyly žádné child elementy. Proto mu musíme nadefinovat velikost, jinak bude mít velikost .

### **Style containment**

```css
article {
	contain: style;
}
```

Tato vlastnost zatím není podporována všemi prohlížeči. Zabraňuje tomu, aby ve chvíli kdz je v elementu změněn [CSS Counter](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Counter_Styles/Using_CSS_counters) byl ovlivněn i zbytek DOM tree.

## **Speciální vlastnosti**

**`content`**
contain: content;<br />
_= layout + paint - size containment_

**`strict`**
contain: strict;<br />
_= layout + size + paint + style containment_

<br />

## **Content-Visibility**

Kontroluje zda je nebo není obsah renderován a případně může vynucovat nastavení konkrétních containment možností. Díky tomu můžeme odložit renderování konkrétního obsahu na dobu až bude potřeba.

**`visibile:`**
Defaultní hodnota. Obsah je na stránku umístěn a vyrenderován normálně.

**`hideen:`**
Přeskočí vyrenderování obsahu. Obsah nesmí být přístupý uživateli přes nástroje jako hledání ve stránce, tab-order navigace, ani nesmí jít označit nebo focusnout.

**`auto:`**
Elementu se nastaví layout, style a paint containment. Pokud není obsah relevantní pro uživatele, přeskočí se jeho renderování. Obsah stále musí být dostupný uživateli přes nástroje jako hledání ve stránce, tab-order navigace, musí jít označit nebo focusnout

<br />

## **Relevant to user**

Pod pojmem relevantní pro uživatele se myslí element, který splňuje některé z těchto podmínek:

> - Element je zobrazen ve viewportu nebo v user-agent-defined margin (plocha o velikosti cca 50% viewportu, kde čekají předrenderované elementy na chvíli, kdy budou potřeba)
> - Element nebo jeho obsah může být focusován
> - Element nebo jeho obsah může být selectován. Vybrán do například tahem myši nebo jinou zvýrazňovací operací.
> - Element nebo jeho obsah je umístěn v top layer dokumentu

<br /><br />

# CONTAINER QUERIES

Je to alternativa ke běžně použiváné media query při které měníme styly v závlistosti na změně breakpointu. Umožňuje nám to rozdělit změny naši stránku na více na sobě nezavislých prvků a definovat jim vlastnosti na základě zařízení uživatele nebo změny breakpointu.

K jejich použití musíme deklarovat containment context, aby prohlížeč věděl, že budeme chtít později měnit vlastnosti tohoto elementu přes query. To uděláme použitím vlastnosti **`container-type`** která může mít hodnoty **size, inline-size nebo normal**.

### **`size`**

Bude vycházet z inline a block rozměrů containeru. Aplikuje layout, style a size containment na container.

### **`inline-size`**

Bude vycházet z inline rozměrů containeru. Aplikuje layout, style a inline-size containment na element.

### **`normal`**

Element není považován za query container pro žádnou z container size queries, ale stále jsou na něm aplikovány container style queries.<br />

**Příklad:**<br />
`index.html`

```html
<div class="post">
	<div class="card">
		<h2>Card title</h2>
		<p>Card content</p>
	</div>
</div>
```

`style.css`

```css
.post {
	container-type: inline-size;
}

/* Defaultní velikost písma */
.card h2 {
	font-size: 1em;
}

/* Změna velikosti písma, pokud je container větší než 700px */
@container (min-width: 700px) {
	.card h2 {
		font-size: 2em;
	}
}
```

Pomocí tohoto můžeme kontrolovat, jak bude daný prvek na stránce vypadat nezávisle na umístění ve stránce a velikosti displeje zařízení. Můžeme tedy velice snadno tento prvek opakovaně používat napříč našim webem. Query se vždy řídí nejbližším nadřazeným elementem s container contextem.

<br/>

## **Pojmenování container contextu**

Když si pojmenujeme container context, můžeme použít toto jméno pak k zacílení konkrétního contextu v container query. Context pojmenujeme pomocí **`container-name`**. Více v následujícím příkladu.<br />
`style.css`

```css
.post {
	container-type: inline-size;
	container-name: sidebar;
}

@container sidebar (min-width: 700px) {
	.card {
		font-size: 2em;
	}
}
```

<br/>

## **Zkrácený zápis**

Můžeme použít zkrácený zápis pro deklarování context typu a jména.

```css
.post {
	container: sidebar / inline-size;
}
```

<br/>

## **Container query jednotky**

Při práci s container queries můžeme použít speciální container jednotky. Z hlediska optimalizace jsou rychlejší než klasické jednotky pro definování velikosti, protože je to procentuální zastoupení rozměru containeru. Není tedy nutný přepočet na klasické jednotky jako px, vh, vww pokaždé, když se změní velikost containeru.

> - **cqw** 1% container šířky
> - **cqh** 1% container výšky
> - **cqi** 1% container inline velikosti
> - **cqb** 1% container block velikosti
> - **cqmin** Vybere menší z jednotek cqi nebo cqb
> - **cqmax** Vybere větší z jednotek cqi nebo cqb

**Příklad:**

```css
@container (min-width: 700px) {
	.card h2 {
		font-size: max(1.5em, 1.23em + 2cqi);
	}
}
```
