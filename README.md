![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# LAB | Panier Ironhack

![giphy (2)](https://user-images.githubusercontent.com/76580/167435963-34b5ddf0-e318-446a-b59f-2edeed3eb030.gif)

<details>
  <summary>
   <h2>Objectifs d'apprentissage</h2>
  </summary>

Cet exercice vous permet de pratiquer et d'appliquer les concepts et les techniques enseignés en classe.

À la fin de cet exercice, vous serez capable de :

- Sélectionner des éléments HTML en utilisant les méthodes et propriétés du DOM

  > `querySelector()`, `querySelectorAll()`, `getElementsByClassName()`, `getElementById()`, `element.currentTarget`, `element.parentNode`

- Accéder et modifier _le contenu des éléments HTML_ en utilisant les propriétés des éléments HTML

  > `element.textContent`, `element.innerHTML`

- Ajouter ou supprimer des _éléments HTML_ dans le DOM en utilisant les méthodes du DOM en JavaScript

  > `document.createElement()`, `element.append()` , `element.appendChild()`, `element.removeChild()`

- Ajouter ou supprimer des _event listeners_ pour répondre aux actions de l'utilisateur, telles que les clics sur un bouton

  > `element.addEventListener()`, `element.removeEventListener()`

- Passer des fonctions en tant qu'arguments à d'autres fonctions (callbacks)

- Itérer sur des _listes d'éléments HTML_ et effectuer des actions sur chacun d'eux, telles que l'accès à des valeurs, l'ajout ou la suppression d'événements, etc.

- Accéder et modifier les propriétés, les valeurs et les attributs des éléments HTML

  > `element.setAttribute()`, `element.getAttribute()`, `element.classList.add()`, `element.classList.remove()`, `classList`, `style`, `value`, `type`

  <br>
  <hr>

</details>

## Introduction

Le commerce électronique s'est révélé être un grand bouleversement dans l'économie du 21e siècle. En tant que l'un des plus grands canaux de vente, après les magasins physiques, le commerce électronique [devrait](https://www.statista.com/statistics/379046/worldwide-retail-e-commerce-sales/) être responsable de 6,3 milliards de dollars de ventes mondiales d'ici 2024, selon les prévisions.

Le commerce électronique est un secteur hautement concurrentiel et il est essentiel de créer une expérience utilisateur positive pour fidéliser les clients et améliorer les conversions. Il n'est pas rare que les entreprises investissent massivement dans l'optimisation du processus d'achat sur leurs plateformes de commerce électronique.

L'un des éléments les plus importants de cette expérience est **le panier d'achat**.

Dans ce laboratoire, nous allons créer **IronCart**, un panier d'achat pour la boutique de produits dérivés non officielle d'Ironhack. Les visiteurs devraient pouvoir ajouter et supprimer des produits du panier d'achat, ainsi que modifier la quantité d'articles qu'ils souhaitent acheter. De plus, les utilisateurs devraient pouvoir voir le sous-total et le prix total des articles qu'ils ont ajoutés.

## Exigences

- Forkez ce repo.
- Clonez-le sur votre machine.

## Soumission

- Une fois terminé, exécutez les commandes suivantes :

```bash
$ git add .
$ git commit -m "Solved lab"
$ git push origin master
```

- Créez une pull request pour que vos TAs puissent vérifier votre travail.

## Instructions

La plupart de votre travail sera réalisé dans le fichier `js/index.js`. Nous avons ajouté le balisage initial dans `index.html` ainsi qu'un style de base. Lors du développement, assurez-vous d'utiliser les mêmes noms de classes que ceux déjà utilisés (et disponibles dans le fichier CSS) pour rendre notre panier d'achat propre et agréable.

C'est parti !
<br>

<br>

### Itération 1: `updateSubtotal`

Ouvrez `index.html` dans votre navigateur. Comme vous pouvez le voir, il n'y a qu'une seule ligne dans le tableau qui représente un produit. Dans cette première itération, vous vous concentrerez uniquement sur ce produit, et plus tard, nous vous aiderons à réfléchir à des moyens d'inclure plusieurs produits.

Jetons un coup d'œil au code HTML fourni. Nous avons la **balise table avec l'identifiant `#cart`**, comme indiqué ci-dessous :

```html
<table id="cart">
  <thead>
    <tr>
      <th>Product Name</th>
      <th>Unit Price</th>
      <th>Quantity</th>
      <th>Subtotal</th>
      <th>Action</th>
    </tr>
  </thead>
  <tbody>
    <tr class="product">
      <!-- ... -->
    </tr>
  </tbody>
  <!-- ... -->
</table>
```

![](https://i.imgur.com/zCWQYg2.png)

Le seul produit que nous avons actuellement dans notre `#cart` est placé dans le `tr` avec la classe `product` (**qui se trouve à l'intérieur de `tbody`**) :

```html
<tr class="product">
  <td class="name">
    <span>Ironhack Rubber Duck</span>
  </td>
  <td class="price">$<span>25.00</span></td>
  <td class="quantity">
    <input type="number" value="0" min="0" placeholder="Quantity" />
  </td>
  <td class="subtotal">$<span>0</span></td>
  <td class="action">
    <button class="btn btn-remove">Remove</button>
  </td>
</tr>
```

Le produit a un **prix** et une **quantité** (où la quantité représente le nombre d'articles d'un produit spécifique ajoutés au panier par l'utilisateur). Dans le code fourni, on voit également un **prix sous-total**. Le prix sous-total sera le résultat de la _multiplication_ de ces valeurs.

Votre objectif est de calculer le prix sous-total, mais abordons-le progressivement. Décortiquons-le en quelques étapes :

- **Étape 0** : Dans cette étape, notre objectif est de vous aider à comprendre le code fourni dans `js/index.js`. Grâce au code fourni, le bouton `Calculate Prices` a déjà une certaine fonctionnalité. En utilisant la manipulation du DOM, nous avons récupéré l'élément avec `id="calculate"` et ajouté un event listener `click` dessus. Lorsque ce bouton est cliqué, il déclenchera la fonction `calculateAll()`. Le code suivant fait exactement ce que nous avons expliqué :

```javascript
// js/index.js

window.addEventListener("load", () => {
  const calculatePricesBtn = document.getElementById("calculate");
  calculatePricesBtn.addEventListener("click", calculateAll);
});
```

Ne vous laissez pas tromper par la méthode [.addEventListener()](https://www.w3schools.com/jsref/met_document_addeventlistener.asp), elle fait exactement la même chose que [onclick()](https://www.w3schools.com/tags/ev_onclick.asp), avec quelques différences que vous pouvez trouver [ici](https://stackoverflow.com/questions/6348494/addeventlistener-vs-onclick). Dans ce laboratoire, vous pouvez utiliser la méthode qui vous convient le mieux.

D'accord, passons à la fonction `calculateAll()`. Dans cette fonction, nous avons utilisé `querySelector` pour récupérer le premier (et actuellement le seul) élément DOM avec la classe `product`. Cet élément (que nous avons enregistré dans la variable `singleProduct`) est passé en argument à la fonction `updateSubtotal()`. Comme vous pouvez le voir dans les commentaires, le code fourni est utilisé uniquement à des fins de test dans l'itération 1.

```js
function calculateAll() {
  // code in the following two lines is added just for testing purposes.
  // it runs when only iteration 1 is completed. at later point, it can be removed.
  const singleProduct = document.querySelector(".product");
  updateSubtotal(singleProduct);
  // end of test

  // ITERATION 2
  //...
  // ITERATION 3
  //...
}
```

Et enfin, nous avons commencé la fonction `updateSubtotal(product)`. Pour l'instant, cette fonction ne fait qu'afficher l'alerte `Calculate Prices clicked!` lorsque le bouton _Calculer les prix_ est cliqué.

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_e50868b669d962f3ddb26802e5a16638.gif)

Commençons :

- **Étape 1** : À l'intérieur de `updateSubtotal()`, créez deux variables (nous vous suggérons de les nommer `price` et `quantity`) et utilisez vos compétences récemment acquises en manipulation du DOM pour OBTENIR les éléments DOM qui contiennent le prix et la quantité. Pour vous donner une longueur d'avance, vous pouvez utiliser le code suivant pour obtenir l'élément DOM contenant le `price` :

```js
// js/index.js
function updateSubtotal(product) {
  const price = product.querySelector(".price span");
  // ... your code goes here
}
```

- **Étape 2** : Maintenant, lorsque vous avez obtenu les éléments DOM mentionnés ci-dessus, votre prochaine étape devrait consister à extraire les valeurs spécifiques à partir d'eux. _Indice_ : peut-être que `innerHTML` vous dit quelque chose ? Au cas où vous seriez curieux de trouver d'autres moyens d'obtenir le même résultat, vous pouvez commencer par consulter [`textContent`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent) et [`innerText`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/innerText) sur Google. De plus, vous pouvez extraire la valeur d'un champ de saisie en accédant à [la propriété value](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#htmlattrdefvalue) de l'élément de saisie. Cependant, ne vous laissez pas distraire ici, considérez cela comme _votre lecture supplémentaire_ une fois que vous aurez terminé le laboratoire.

- **Étape 3** : Utilisez les valeurs que vous avez extraites des éléments DOM mentionnés ci-dessus pour calculer le prix total. Vous pouvez créer une nouvelle variable dont la valeur sera égale au produit de ces valeurs. Ex. Si un utilisateur choisit 3 Ironhack Rubber Ducks (canards en caoutchouc Ironhack), il devrait voir que le sous-total est de 75 dollars ($25 \* 3 = $75).

- **Étape 4** : Maintenant, alors que vous devenez un expert en manipulation DOM, utilisez à nouveau vos compétences pour obtenir l'élément DOM qui doit contenir le sous-total. _Indice_ : il s'agit de l'élément avec la classe `subtotal`.

- **Étape 5** : À l'étape 3, vous avez calculé le prix total, et à l'étape 4, vous avez obtenu l'élément DOM qui devrait afficher ce prix. Dans cette étape, votre objectif est de définir le prix total dans l'élément DOM correspondant. Assurez-vous également que cette fonction renvoie la valeur du sous-total afin de pouvoir l'utiliser ultérieurement si nécessaire.

Dans cette itération, vous avez terminé la création de la fonction `updateSubtotal` qui **calcule le sous-total** pour ce produit spécifique, **met à jour la valeur du sous-total** pour ce même produit dans le DOM et renvoie la **valeur du sous-total**.

En tant qu'argument unique, la fonction doit prendre **un nœud DOM** qui correspond à un seul élément `tr` avec une classe `product`. Dans le code de départ inclus, nous l'avons appelé `product`.

```js
function updateSubtotal(product) {
  // ...
}
```

:bulb: _Astuce_ : Assurez-vous que votre fonction `calculateAll()` contient le code de test que nous avons mentionné précédemment. Si le code est à sa place après avoir terminé avec succès la fonction `updateSubtotal()`, vous devriez pouvoir voir les mises à jour correspondantes dans le champ `Subtotal` du tableau.

Vérifiez [ici](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_30b87c596b79954f63b3482d2f320fe4.gif) la sortie attendue.

<hr>

### Itération 2 : `calculateAll()`

Notre code actuel fonctionne parfaitement pour un produit, mais nous prévoyons d'avoir plus d'un produit dans notre panier. Par conséquent, nous utiliserons `calculateAll` pour déclencher la mise à jour des sous-totaux pour chaque produit.

Complétez la fonction appelée `calculateAll()`. Son but est d'appeler la fonction `updateSubtotal` pour chaque nœud DOM `tr.product` dans la table `table#cart`.

Pour commencer, supprimez ou mettez en commentaire le code existant à l'intérieur de `calculateAll()` (le code que nous avons utilisé à des fins de test). De plus, ajoutons un nouveau produit à notre fichier `index.html`. Dupliquez le `tr` avec la classe `product`, renommez le produit à l'intérieur et changez le prix du produit.

![](https://i.imgur.com/Pv4NmR8.png)

:bulb: _Astuce_: Commencez par obtenir les nœuds DOM pour chaque ligne de product. Actuellement, nous avons deux produits ; nous avons donc deux lignes avec la classe `product`. Utiliser peut-être [getElementsByClassName](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName) pourrait être utile ici.

```js
function calculateAll() {
  // ...
}
```

Le résultat final devrait ressembler à ceci :

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_0efb56fc0e5717469417e806fa7cde12.gif)

<hr>

### Itération 3 : Total

Notre fonctionnalité de calcul est encore incomplète. Le sous-total de chaque produit est mis à jour, mais la valeur totale reste inchangée.

À la fin de la fonction `calculateAll()`, réutilisez la valeur totale que vous venez de calculer lors de l'itération précédente et mettez à jour l'élément DOM correspondant. Calculez le prix total des produits de votre panier en additionnant tous les sous-totaux retournés par `updateSubtotal()` lorsqu'il est appelé avec chaque produit.

Enfin, affichez cette valeur sur votre DOM.

![](https://i.imgur.com/SCtdzMd.png)

<hr>

## Itérations bonus

### Itération 4 : Suppression d'un produit

Les utilisateurs doivent pouvoir supprimer des produits de leur panier. Dans ce but, chaque ligne de produit de notre table comporte un bouton "Remove" ("Supprimer") à la fin.

Mais essayons de résoudre notre problème petit à petit. À l'intérieur de la fonction existante que vous transmettez à `window.addEventListener()`, commencez par interroger le document pour tous les boutons "Remove" ("Supprimer"), parcourez-les et ajoutez un event listener `click` à chacun d'eux, en passant une fonction nommée `removeProduct` en tant qu'argument de rappel. Si vous avez besoin d'un indice sur la façon de faire cela, jetez un coup d'œil à la façon dont nous l'avons fait avec l'ajout d'un event listener sur `calculatePricesBtn`.

Nous avons déjà déclaré `removeProduct(event)` et ajouté un code de démarrage. Une fois que vous avez terminé d'interroger les boutons de suppression et d'ajouter l'event listener click sur eux, ouvrez la console et cliquez sur n'importe quel bouton `Remove` ("Supprimer").

Comme nous pouvons le voir, `removeProduct(event)` attend l'événement en tant qu'argument unique, et cela va déclencher la suppression du produit correspondant du panier.

:bulb: Astuce : Pour accéder à l'élément sur lequel un événement a été déclenché, vous pouvez faire référence à `event.currentTarget`. Pour supprimer un nœud du DOM, vous devez accéder à son nœud parent et appeler [`removeChild`](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild) sur celui-ci. Vous pouvez accéder au parent d'un nœud DOM à partir de sa propriété `parentNode`.

Assurez-vous que le prix est mis à jour correctement lorsque vous supprimez des produits du panier.

Cliquez [ici](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_17b1e9e4d2606239163dddbc5b2a3d9f.gif) pour voir le résultat attendu.

<hr>

### Itération 5 : Création de nouveaux produits

Pour finir, nous permettrons à l'utilisateur d'ajouter un produit personnalisé à son panier.

Décommentez l'élément `tfoot` ainsi que ses enfants dans le fichier `index.html` :

```html
<table>
  <tbody>
    <!-- ... -->
  </tbody>
  <!-- <tfoot>
    <tr class="create-product">
      <td>
        <input type="text" placeholder="Product Name" />
      </td>
      <td>
        <input type="number" min="0" value="0" placeholder="Product Price" />
      </td>
      <td></td>
      <td></td>
      <td>
        <button id="create" class="btn">Create Product</button>
      </td>
    </tr>
  </tfoot> -->
</table>
```

![](https://i.imgur.com/J8aserm.png)

Les deux champs de saisie à l'intérieur de `tfoot` représentent respectivement le nom du nouveau produit et le prix unitaire. Le bouton "Create Product" devrait ajouter un nouveau produit au panier lorsqu'il est déclenché.

Ajoutez un gestionnaire d'événement (event handler) `click` au bouton "Create Product" ("Créer un produit") qui appellera une fonction nommée `createProduct` en tant que rappel.

Dans `createProduct`, vous devez cibler les nœuds DOM des champs de saisie du nom et du prix unitaire, extraire leurs valeurs, ajouter une nouvelle ligne au tableau avec le nom et le prix unitaire du produit, ainsi que le champ de saisie de la quantité et le bouton "Remove" ("Supprimer"), et vous assurer que toute la fonctionnalité fonctionne comme prévu.

N'oubliez pas que le nouveau produit doit avoir l'air indistinct et se comporter comme l'un des produits précédemment inclus dans le panier. Ainsi, il devrait être possible de calculer son sous-total lorsque le bouton "Calculate All" est cliqué, et de supprimer le produit.

Lorsque la création du produit est finalisée, veuillez vider les champs de saisie du formulaire de création.

Cliquez [ici](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_00abbd15326ec24d93147196024458f6.gif) pour voir le résultat attendu.

<br>

<!-- ## Testez votre code

Nous allons à nouveau travailler avec des tests automatisés !

Veuillez ouvrir votre terminal, changer de répertoire pour vous rendre à la racine du laboratoire, et exécutez la commande `npm install` pour installer le programme de test. Maintenant, vous pouvez exécuter la commande `npm run test:watch` pour exécuter les tests automatisés en mode watch. Ouvrez le fichier lab-solution.html résultant à l'aide de l'extension "Live Server" de VSCode pour voir toujours les résultats des tests.

```bash
$ cd lab-dom-ironhack-cart
$ npm install
$ npm run test:watch
```

Si vous souhaitez consulter les tests pour plus de détails, ils se trouvent dans le fichier `tests/ironhack-cart.spec.js`. -->

**Bon travail ! 💙**

## FAQs

<br>

<details>
  <summary>Je suis bloqué dans l'exercice et je ne sais pas comment résoudre le problème ou par où commencer.</summary>
  <br>

Si vous êtes bloqué dans votre code et que vous ne savez pas comment résoudre le problème ou par où commencer, prenez du recul et essayez de formuler une question claire sur le problème spécifique auquel vous êtes confronté. Cela vous aidera à circonscrire le problème et à développer des solutions potentielles.

Par exemple, s'agit-il d'un concept que vous ne comprenez pas ou d'un message d'erreur que vous ne savez pas comment résoudre ? Il est généralement utile d'essayer d'énoncer le problème aussi clairement que possible, y compris les messages d'erreur que vous recevez. Cela peut vous aider à communiquer le problème à d'autres personnes et éventuellement à obtenir de l'aide de vos camarades de classe ou de ressources en ligne.

Une fois que vous aurez bien compris le problème, vous pourrez commencer à chercher une solution.

[Retour en haut](#faqs)

</details>

<details>
  <summary>Comment boucler un tableau à l'aide de la méthode <code>forEach()</code> ?</summary>
  <br>

La méthode `forEach()` exécute une fonction fournie une fois pour chaque élément du tableau. Elle ne renvoie pas un nouveau tableau, mais exécute la fonction sur chaque élément du tableau.

La syntaxe de la méthode `forEach()` est la suivante :

```js
array.forEach(function (element) {
  // code to be executed for each element
});
```

Voici un exemple qui utilise la méthode `forEach()` pour enregistrer chaque élément et son index dans un tableau sur la console :

```js
const fruits = ["apple", "banana", "cherry"];

fruits.forEach(function (element, index) {
  console.log(`${index}: ${element}`);
});
```

Vous pouvez également utiliser une fonction de flèche comme fonction de rappel pour `forEach()` :

```js
fruits.forEach((element, index) => {
  console.log(`${index}: ${element}`);
});
```

[Retour en haut](#faqs)

</details>

<details>
  <summary>>Comment ajouter un nouvel élément DOM à un élément existant ?</summary>
  <br>

Pour ajouter un nouvel élément DOM à un élément existant en JavaScript, vous pouvez utiliser la méthode `appendChild()`.

**Exemple:**

```js
// Get the parent element
var parentElement = document.getElementById("parent");

// Create the new element
var newElement = document.createElement("p");

// Set the text content of the new element
newElement.textContent = "This is a new paragraph.";

// Append the new element to the parent element
parentElement.appendChild(newElement);
```

Ceci créera un nouvel élément `p` avec le texte `"This is a new paragraph."` et l'ajoutera à l'élément avec l'ID `parent`.

[Retour en haut](#faqs)

</details>

<details>
  <summary>Pourquoi certaines valeurs d'éléments DOM doivent-elles être converties en nombres alors qu'elles semblent déjà être des nombres ?</summary>
  <br>

En effet, toutes les valeurs en HTML sont des chaînes et toutes les valeurs d'attributs sont des chaînes. Par conséquent, les valeurs des éléments DOM sont renvoyées sous forme de chaînes, même si elles contiennent des valeurs numériques.

Si vous voulez utiliser une valeur d'un élément DOM comme un nombre, vous devrez la convertir en un type de nombre.

Voici un exemple de la manière d'accéder à la valeur de l'élément `prix` et de la convertir en nombre à l'aide de JavaScript :

```js
// Get the input element
const input = item.querySelector("input");

// Convert the string value of the input element to a number
const value = Number(input.value);
```

[Retour en haut](#faqs)

</details>

<details>
  <summary>Je continue à obtenir le résultat <code>NaN</code> dans mon programme. Comment puis-je y remédier ?</summary>
  <br>

En JavaScript, `NaN` signifie "Not a Number". Il s'agit d'une valeur spéciale qui représente un problème avec une opération numérique ou un type de coercion qui a échoué. Il y a plusieurs raisons pour lesquelles vous pouvez obtenir un résultat `NaN` dans votre code JavaScript :

1. **Division d'un nombre par `0`** : Toute opération consistant à diviser un nombre par `0` (zéro) aboutira à `NaN`. Exemple :

```js
const result = 10 / 0;

console.log(result); // NaN
```

   <br>

2.  **Analyse d'un nombre invalide** : Si vous essayez d'analyser `undefined` ou une chaîne qui ne peut pas être représentée comme un nombre en utilisant les fonctions `parseInt()` et `parseFloat()`, vous obtiendrez `NaN` comme résultat.
    <br>

Exemple d'analyse d'une valeur invalide avec `parseInt()` :

```js
const result1 = parseInt("ironhack");
const result2 = parseInt(undefined);

console.log(result1); // NaN
console.log(result2); // NaN
```

<br>Exemple d'analyse d'une valeur invalide avec `parseFloat()` :

```js
const result1 = parseFloat("ironhack");
const result2 = parseFloat(undefined);

console.log(result1); // NaN
console.log(result2); // NaN
```

<br>Exemple d'analyse d'une valeur invalide avec `Number()` :

```js
const result1 = Number("ironhack");
const result2 = Number(undefined);

console.log(result1); // NaN
console.log(result2); // NaN
```

   <br>

Pour résoudre le problème `NaN` dans votre code, vous pouvez essayer plusieurs choses :

- Vérifiez que vous n'essayez pas de diviser un nombre par `0`.
- Assurez-vous que les chaînes de caractères que vous essayez d'analyser comme des nombres sont en fait des représentations valides de nombres. Vous pouvez utiliser `console.log()` pour vérifier les valeurs de vos variables et voir si c'est le problème.

[Retour en haut](#faqs)

</details>

<details>
  <summary>Comment ajouter un event listener?</summary>
  <br>

Utilisez la méthode `addEventListener()` pour ajouter un écouteur d'événement. Cette méthode prend deux arguments : le _type d'événement_ et la _fonction de gestion d'événement_ qui sera appelée lorsque l'événement se produira.

Voici un exemple d'ajout d'un écouteur d'événement _click_ à un élément de type bouton :

```js
const button = document.querySelector("button");

function handleClick() {
  console.log("Button was clicked");
}

button.addEventListener("click", handleClick);
```

Ceci ajoutera à l'élément `button` un écouteur d'événement click, qui appellera la fonction `handleClick()` à chaque fois que le bouton sera cliqué.

Pour plus d'informations, consultez le site : [MDN - addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

[Retour en haut](#faqs)

</details>

<details>
  <summary>Comment supprimer un event listener?</summary>
  <br>

Utilisez la méthode `removeEventListener()` pour supprimer un écouteur d'événement. Cette méthode prend deux arguments : le _type d'événement_ et la _fonction de gestion d'événement_ qui a été assignée à l'origine lors de l'ajout de l'écouteur d'événement.

Par exemple, supposons que vous ayez ajouté l'écouteur d'événement suivant :

```js
button.addEventListener("click", handleClick);
```

Pour supprimer cet écouteur d'événements, vous pouvez utiliser le code suivant :

```js
button.removeEventListener("click", handleClick);
```

Pour plus d'informations, consultez le site : [MDN - removeEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)

[Retour en haut](#faqs)

</details>

<details>
  <summary>Pourquoi est-ce que j'obtiens <code>null</code> lorsque j'essaie d'accéder à un élément HTML ?</summary>
  <br>

Il y a plusieurs raisons possibles pour lesquelles vous obtenez une valeur `null` lorsque vous essayez d'accéder à un élément du DOM en JavaScript :

1. **Vous utilisez le mauvais sélecteur ou vous vous trompez de nom** : Assurez-vous que vous utilisez le bon sélecteur et la bonne orthographe pour accéder à l'élément. Si vous utilisez un sélecteur incorrect ou si vous vous trompez dans le nom, vous obtiendrez une valeur `null` lorsque vous tenterez d'accéder à l'élément.

2. **L'élément n'est pas encore chargé** : Si vous essayez d'accéder à un élément qui n'est pas encore chargé dans le DOM (par exemple, un élément qui est défini dans un script qui est chargé en bas de la page), vous obtiendrez une valeur `null` lorsque vous essayerez d'y accéder. Vous pouvez résoudre ce problème en enveloppant votre code dans un gestionnaire d'événement `window.onload`, qui s'assurera que l'élément est disponible avant que votre code ne s'exécute :

```js
window.addEventListener("load", function (event) {
  const element = document.querySelector('#my-element');
  // now you can safely access the element
};
```

[Retour en haut](#faqs)

</details>

<details>
  <summary>Je ne peux pas pousser les modifications vers le dossier. Que dois-je faire ?</summary>
  <br>

Il y a plusieurs raisons possibles pour lesquelles vous ne pouvez pas _pousser_ des modifications vers un dépôt Git :

1. **Vous n'avez pas validé vos modifications:** Avant de pouvoir pousser vos modifications vers le dépôt, vous devez les valider en utilisant la commande `git commit`. Assurez-vous d'avoir validé vos modifications et essayez de les pousser à nouveau. Pour ce faire, exécutez les commandes suivantes à partir du dossier du projet :

```bash
git add .
git commit -m "Your commit message"
git push
```

2. **Vous n'avez pas la permission de pousser vers le dépôt:** Si vous avez cloné le dépôt directement depuis le dépôt principal d'Ironhack sans faire un _Fork_ d'abord, vous n'avez pas d'accès en écriture au dépôt.
   Pour vérifier quel dépôt distant vous avez cloné, exécutez la commande suivante à partir du dossier du projet :
   `bash
git remote -v
`

Si le lien affiché est le même que le dépôt principal d'Ironhack, vous devrez d'abord forker le dépôt sur votre compte GitHub et ensuite cloner votre fork sur votre machine locale pour être en mesure de pousser les changements.

**Note** : Vous devriez faire une copie de votre code local pour éviter de le perdre au cours du processus.

[Retour en haut](#faqs)

</details>
