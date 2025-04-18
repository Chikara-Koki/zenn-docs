---
title: "チェックボックス - コピペで使えるアクセシビリティ対応モジュール"
emoji: "😺"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["アクセシビリティ", "HTML", "CSS", "Javascript"]
published: false
---
## はじめに

こんにちは。[株式会社VOWZ](https://vowz.jp/) の Chikara です。  
弊社では、定期的なアクセシビリティ講習の実施や、制作したWebページに対するアクセシビリティチェックなど、企業として継続的にアクセシビリティに取り組んでいます。  

今回は、チェックボックスに関する解説をお届けします。  
HTML標準のセマンティクスの`input type="checkbox"`を使用したモジュールにあわせて、`div`要素などをカスタマイズして実装したモジュールの2例を紹介させていただきます。  
モジュールだけを確認したい場合は、目次の【モジュール】をご参照ください。

## 標準的なチェックボックス

### HTML

```html
<h3 id="id-group-label">好きなアニメにチェックを入れてください</h3>
<div role="group" aria-labelledby="id-group-label">
  <ul class="checkboxes">
    <li>
      <label class="c-check-label">
        <input type="checkbox" class="c-check">
        SPY×FAMILY
      </label>
    </li>
    <li>
      <label class="c-check-label">
        <input type="checkbox" class="c-check">
        ドラえもん
      </label>
    </li>
    <li>
      <label class="c-check-label">
        <input type="checkbox" class="c-check">
        鬼滅の刃
      </label>
    </li>
    <li>
      <label class="c-check-label">
        <input type="checkbox" class="c-check">
        僕のヒーローアカデミア
      </label>
    </li>
  </ul>
</div>
```

<br>

### CSS

```css
.c-check-label {
  display: flex;
  align-items: center;
  gap: 4px;
  padding: 4px 8px;
  width: fit-content;
  cursor: pointer;
  transition: .3s;
}
.c-check {
  appearance: none; /* デフォルトスタイルの打ち消し */
  -webkit-appearance: none;
  width: 20px;
  height: 20px;
  margin: 0;
  border: 2px solid #ccc;
  border-radius: 2px;
  transition: .3s;
  position: relative;
  cursor: pointer;
}
.c-check-label:hover {
  color: #008060;
}
.c-check-label:hover .c-check {
  border-color: #008060;
  box-shadow: 0 0 0 1px #008060 inset;
}
.c-check:checked {
  background-color: #008060;
  border-color: #008060;
}
.c-check:checked::before {
  position: absolute;
  top: 2px;
  left: 5px;
  transform: rotate(50deg);
  width: 6px;
  height: 9px;
  border-right: 2px solid #fff;
  border-bottom: 2px solid #fff;
  content: '';
}
```

### 標準チェックボックスのコード説明

こちらのコードの場合、HTMLの標準機能を使用しているので様々なサポートがあります。  
第一に、aria-checked等の管理を行うためのJSが必要ありません。  
HTMLとCSSのみで機能するため、シンプルさと堅牢さを確保しています。

<br>
<br>

```html
<h3 id="id-group-label">好きなアニメにチェックを入れてください</h3>
<div role="group" aria-labelledby="id-group-label">
```

1. id属性を使用することで、aria-labelledby属性によってラベルを紐付けすることができます。
2. role="group"によって、含まれる要素が区切られた一つのグループであることを明示します。（ul属性にrole="group"を付与したら、ul本来のrole="list"が上書きされてしまうため厳禁）

<br>
<br>

```html
<label class="c-check-label">
  <input type="checkbox" class="c-check">
  SPY×FAMILY
</label>
```

1. label要素の中にinput要素とラベルテキストを含めることで、idとforによる紐付けを省略できる。
2. label要素のinput要素がある場合、idとforによって紐付けが必要。inputのみの場合にはaria-labelを指定する。

<br>
<br>

```css
.c-check {
  appearance: none; /* デフォルトスタイルの打ち消し */
  -webkit-appearance: none;
  width: 20px;
  height: 20px;
  margin: 0;
  border: 2px solid #ccc;
  border-radius: 2px;
  transition: .3s;
  position: relative;
}
.c-check-label:hover {
  color: #008060;
}
.c-check-label:hover .c-check {
  border-color: #008060;
  box-shadow: 0 0 0 1px #008060 inset;
}
```

1. スタイルが制限されがちなinput要素について、`appearance: none;`を付与することで、デフォルトのスタイルを無効化しています。
2. `appearance`は基本的なブラウザに対応していますが、念のために`-webkit-`も併用しています。
3. デフォルトスタイルの無効化に伴い、ステータス変化によるスタイル追加に注意してください。

## カスタムチェックボックス

[実際の挙動はこちら](https://test-chikara.vowz.co.jp/a11y/checkbox/custom.html)

### HTML

```html
<h3 id="id-group-label">好きなアニメにチェックを入れてください</h3>
<div role="group" aria-labelledby="id-group-label">
  <ul class="checkboxes">
    <li><div role="checkbox" aria-checked="false" tabindex="0" aria-label="SPY×FAMILYを選択">SPY×FAMILY</div></li>
    <li><div role="checkbox" aria-checked="true" tabindex="0" aria-label="ドラえもんを選択">ドラえもん</div></li>
    <li><div role="checkbox" aria-checked="false" tabindex="0" aria-label="鬼滅の刃を選択">鬼滅の刃</div></li>
    <li><div role="checkbox" aria-checked="false" tabindex="0" aria-label="僕のヒーローアカデミアを選択">僕のヒーローアカデミア</div></li>
  </ul>
</div>
```

<br>
<br>

### CSS

```css
ul.checkboxes {
  list-style: none;
  margin: 20px 0 0 0;
  padding: 0;
  padding-left: 1em;
  max-width: fit-content;
}

ul.checkboxes li {
  list-style: none;
  margin: 0;
  padding: 0;
}

[role="checkbox"] {
  display: inline-block;
  padding: 4px 8px;
  cursor: pointer;
  transition: .3s;
  width: 100%;
}

[role="checkbox"]::before {
  position: relative;
  top: 1px;
  left: -4px;
  content: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' height='16' width='16' style='forced-color-adjust: auto;'%3E%3Crect x='2' y='2' height='13' width='13' rx='2' stroke='currentcolor' stroke-width='1' fill-opacity='0' /%3E%3C/svg%3E");
}

[role="checkbox"][aria-checked="true"]::before {
  position: relative;
  top: 1px;
  content: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' height='16' width='16' style='forced-color-adjust: auto;'%3E%3Crect x='2' y='2' height='13' width='13' rx='2' stroke='currentcolor' stroke-width='1' fill-opacity='0' /%3E%3Cpolyline points='4,8 7,12 12,5' fill='none' stroke='currentcolor' stroke-width='2' /%3E%3C/svg%3E");
}

[role="checkbox"]:focus,
[role="checkbox"]:hover {
  border-radius: 5px;
  background-color: #def;
}

[role="checkbox"]:hover {
  cursor: pointer;
}
```

<br>
<br>

### Javascript

```JSX
/*
 *   This content is licensed according to the W3C Software License at
 *   https://www.w3.org/Consortium/Legal/2015/copyright-software-and-document
 *
 *   File:   Checkbox.js
 *
 *   Desc:   Checkbox widget that implements ARIA Authoring Practices
 */

 'use strict';

class Checkbox {
  constructor(domNode) {
    this.domNode = domNode;
    this.domNode.tabIndex = 0;

    if (!this.domNode.getAttribute('aria-checked')) {
      this.domNode.setAttribute('aria-checked', 'false');
    }

    this.domNode.addEventListener('keydown', this.onKeydown.bind(this));
    this.domNode.addEventListener('keyup', this.onKeyup.bind(this));
    this.domNode.addEventListener('click', this.onClick.bind(this));
  }

  toggleCheckbox() {
    if (this.domNode.getAttribute('aria-checked') === 'true') {
      this.domNode.setAttribute('aria-checked', 'false');
    } else {
      this.domNode.setAttribute('aria-checked', 'true');
    }
  }

  /* EVENT HANDLERS */

  // Make sure to prevent page scrolling on space down
  onKeydown(event) {
    if (event.key === ' ') {
      event.preventDefault();
    }
  }

  onKeyup(event) {
    var flag = false;

    switch (event.key) {
      case ' ':
        this.toggleCheckbox();
        flag = true;
        break;

      default:
        break;
    }

    if (flag) {
      event.stopPropagation();
    }
  }

  onClick() {
    this.toggleCheckbox();
  }
}

// Initialize checkboxes on the page
window.addEventListener('load', function () {
  let checkboxes = document.querySelectorAll('.checkboxes [role="checkbox"]');
  for (let i = 0; i < checkboxes.length; i++) {
    new Checkbox(checkboxes[i]);
  }
});
```

### カスタムチェックボックスのコード解説

カスタム要素であるため、スタイル的な拡張性が高いです。  
ただし、機能としてはariaやjavascriptに依存しているため、実装が正しくない場合、アクセシビリティに問題が生じる可能性があります。

<br>

#### HTML

```html
<li>
  <div role="checkbox" 
       aria-checked="false"
       tabindex="0"
       aria-label="SPY×FAMILYを選択">SPY×FAMILY
  </div>
</li>
```

1. `role="checkbox"`を使用することで、`input type="checkbox"`を再現。
2. `aria-checked`によって、ステータスを管理。
3. `tabindex="0"`によって、div要素をフォーカス可能に。
4. `aria-label`に`〜を選択`という文言を設定することによって、よりアクセシブルに。

## まとめ

チェックボックスに関しては、HTML標準の機能でも十分なカスタム性を持っているのではないかと思いました。  
どうしても必要な場合等にはカスタム要素の使用も問題ありませんが、role属性など適切な構築が前提となりますのでご注意ください。

---

※本記事で掲載されているモジュールは、W3Cによる [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/) を引用・調整したものです。ライセンスは [W3C Software License](https://www.w3.org/copyright/software-license/) に準拠します。