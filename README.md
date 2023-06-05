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

- Accéder et modifier le contenu des éléments HTML en utilisant les propriétés des éléments HTML
  
  > `element.textContent`, `element.innerHTML`

- Ajouter ou supprimer des éléments HTML dans le DOM en utilisant les méthodes du DOM en JavaScript

  > `document.createElement()`, `element.append()` , `element.appendChild()`, `element.removeChild()`

- Ajouter ou supprimer des écouteurs d'événements pour répondre aux actions de l'utilisateur, telles que les clics sur un bouton

  > `element.addEventListener()`, `element.removeEventListener()`

- Passer des fonctions en tant qu'arguments à d'autres fonctions (callbacks)

- Itérer sur des listes d'éléments HTML et effectuer des actions sur chacun d'eux, telles que l'accès à des valeurs, l'ajout ou la suppression d'événements, etc.

- Accéder et modifier les propriétés, les valeurs et les attributs des éléments HTML

  > `element.setAttribute()`, `element.getAttribute()`, `element.classList.add()`, `element.classList.remove()`, `classList`, `style`, `value`, `type`

  <br>
  <hr> 

</details>



## Introduction

Le commerce électronique s'est révélé être un grand changement de paradigme dans l'économie du XXIe siècle. En tant que l'un des plus grands canaux de vente, après le commerce de détail physique, le commerce électronique devrait être responsable de 6,3 billions de dollars de ventes mondiales d'ici 2024, selon les prévisions.

Le commerce électronique est un secteur très concurrentiel et il est crucial pour les entreprises d'investir considérablement dans l'optimisation du processus d'achat sur leurs plates-formes de commerce électronique afin de proposer une expérience utilisateur positive et d'améliorer les conversions.

L'un des éléments les plus importants de cette expérience est **le panier d'achat**.

Dans ce laboratoire, nous allons construire le **IronCart**, un panier d'achat pour la boutique de produits dérivés Ironhack non officielle. Les visiteurs devraient pouvoir ajouter et supprimer des produits du panier d'achat et modifier la quantité d'articles qu'ils souhaitent acheter. De plus, les utilisateurs devraient pouvoir voir le sous-total et le montant total des

## Exigences

- Forkez ce repo.
- Clonez-le sur votre machine.

  

## Soumission

Une fois terminé, exécutez les commandes suivantes :

```bash
$ git add .
$ git commit -m "Solved lab"
$ git push origin master
```

Créez une pull request pour que vos TAs puissent vérifier votre travail.

## Instructions

Vous ferez la plupart de votre travail dans le fichier `src/index.js`. Nous avons ajouté le balisage initial dans `index.html` et un peu de style de base. Lorsque vous développez, assurez-vous d'utiliser les mêmes noms de classe que ceux déjà utilisés (et disponibles dans le fichier CSS) pour rendre notre panier d'achat agréable et propre.

Allons-y !

<br>

<hr>

### Itération 1: `updateSubtotal`

Ouvrez le fichier `index.html` dans votre navigateur. Comme vous pouvez le voir, il n'y a qu'une seule ligne dans le tableau qui représente un produit. Dans cette première itération, vous vous concentrerez uniquement sur ce produit, et plus tard, nous vous aiderons à réfléchir aux moyens de passer à plusieurs produits.

Jetons un coup d'œil au code HTML fourni. Nous avons la balise de tableau avec l'identifiant `#cart`, comme indiqué ci-dessous :

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


Le produit que nous avons actuellement dans notre `#cart` est placé dans le `tr` avec la classe `product` (**qui va à l'intérieur de `tbody`**):

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

Le produit a un **prix** et une **quantité** (où la quantité représente le nombre d'articles d'un produit spécifique ajouté par un utilisateur au panier). Dans le code fourni, nous voyons également un **sous-total**. Le sous-total sera le résultat de la *multiplication* de ces valeurs.

Votre objectif est de calculer le prix sous-total, mais abordons-le progressivement. Décortiquons-le en quelques étapes :

- **Étape 0** : Dans cette étape, notre objectif est de vous aider à comprendre le code fourni dans `src/index.js`. Grâce au code fourni, le bouton `Calculate Prices` a déjà une certaine fonctionnalité. En utilisant la manipulation du DOM, nous avons obtenu l'élément avec l'`id="calculate"` et ajouté un écouteur d'événement `click` dessus. Ce bouton déclenchera la fonction `calculateAll()` lorsqu'il est cliqué. Le code suivant fait exactement ce que nous avons expliqué :

```javascript
// js/index.js

window.addEventListener('load', () => {
  const calculatePricesBtn = document.getElementById('calculate');
  calculatePricesBtn.addEventListener('click', calculateAll);
});
```

Ne vous laissez pas tromper par la méthode [.addEventListener()](https://www.w3schools.com/jsref/met_document_addeventlistener.asp), elle fait exactement la même chose que [onclick()](https://www.w3schools.com/tags/ev_onclick.asp), avec quelques différences que vous pouvez trouver [ici](https://stackoverflow.com/questions/6348494/addeventlistener-vs-onclick). Dans ce laboratoire, vous pouvez utiliser la méthode que vous préférez.

Ok, passons à la fonction `calculateAll()`. Nous avons utilisé `querySelector` dans cette fonction pour obtenir le premier (et actuellement le seul) élément DOM avec la classe `product`. Cet élément (que nous avons enregistré dans la variable nommée `singleProduct`) est passé en argument à la fonction `updateSubtotal()`. Comme vous pouvez le voir dans les commentaires, le code fourni est utilisé uniquement à des fins de test dans l'itération 1.


```js
function calculateAll() {
  // code in the following two lines is added just for testing purposes.
  // it runs when only iteration 1 is completed. at later point, it can be removed.
  const singleProduct = document.querySelector('.product');
  updateSubtotal(singleProduct);
  // end of test

  // ITERATION 2
  //...
  // ITERATION 3
  //...
}
```

Et enfin, nous avons commencé la fonction `updateSubtotal(product)`. Pour l'instant, cette fonction ne fait qu'afficher l'alerte `Calculate Prices clicked!` lorsque le bouton *Calculer les prix* est cliqué.

Et enfin, nous avons commencé la fonction `updateSubtotal(product)`. Pour l'instant, cette fonction se contente d'afficher une alerte `Calculate Prices clicked!` lorsque le bouton Calculate Prices est cliqué.

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_e50868b669d962f3ddb26802e5a16638.gif)

Commençons :

- **Étape 1** : À l'intérieur de `updateSubtotal()`, créez deux variables (nous vous suggérons de les nommer `price` et `quantity`) et utilisez vos compétences récemment acquises en manipulation DOM pour OBTENIR les éléments DOM qui contiennent le prix et la quantité. Pour vous donner un coup de pouce, vous pouvez utiliser le code suivant pour obtenir l'élément DOM contenant le `price` :

```js
// js/index.js
function updateSubtotal(product) {
  const price = product.querySelector('.price span');
  // ... your code goes here
}
```

- **Étape 2** : Maintenant, lorsque vous avez les éléments DOM mentionnés ci-dessus, votre prochaine étape devrait consister à extraire les valeurs spécifiques à partir d'eux. Astuce : peut-être que `innerHTML` vous rappelle quelque chose ? Au cas où vous seriez curieux de trouver d'autres moyens d'obtenir le même résultat, vous pouvez commencer par consulter [`textContent`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent) et [`innerText`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/innerText) sur Google. De plus, vous pouvez extraire la valeur d'un élément de saisie en accédant à [la propriété value](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#htmlattrdefvalue) de l'élément de saisie. Cependant, ne vous laissez pas distraire ici, laissez cela être votre *lecture supplémentaire* lorsque vous aurez terminé le laboratoire.

- **Étape 3** : Utilisez les valeurs extraites des éléments DOM mentionnés ci-dessus pour calculer le prix total hors taxes. Vous pouvez créer une nouvelle variable et sa valeur sera égale au produit de ces valeurs. Par exemple, si un utilisateur a choisi 3 canards en caoutchouc Ironhack, il devrait voir que le prix total hors taxes est de 75 dollars (25 $ * 3 = 75 $).

- **Étape 4** : Maintenant, lorsque vous devenez un ninja de la manipulation DOM, utilisez à nouveau vos compétences pour obtenir l'élément DOM qui devrait contenir le prix total hors taxes. Astuce : c'est l'élément avec la classe `subtotal`.

- **Étape 5** : À l'étape 3, vous avez calculé le prix total hors taxes, et à l'étape 4, vous avez obtenu l'élément DOM qui devrait afficher ce prix. Dans cette étape, votre objectif est de définir le prix total hors taxes sur l'élément DOM correspondant. Assurez-vous également que cette fonction retourne la valeur du prix total hors taxes afin de pouvoir l'utiliser ultérieurement si nécessaire.

Dans cette itération, vous avez terminé la création de la fonction **`updateSubtotal`** qui **calcule le prix total hors taxes** pour ce produit spécifique, **met à jour la valeur du prix total hors taxes** pour ce même produit dans le DOM et retourne la **valeur du prix total hors taxes**.

En tant qu'argument unique, la fonction doit prendre un **nœud DOM** correspondant à un unique élément tr avec une classe `product`. Dans le code de départ inclus, nous l'avons appelé `product`.

```js
function updateSubtotal(product) {
  // ...
}
```

:bulb: *Astuce* : Assurez-vous que votre fonction `calculateAll()` contient le code de test que nous avons mentionné précédemment. Si le code est à sa place après avoir terminé avec succès la fonction `updateSubtotal()`, vous devriez pouvoir voir les mises à jour correspondantes dans le champ `Subtotal` du tableau.

:bulb: Astuce : Assurez-vous que votre fonction `calculateAll()` contienne le code de test que nous avons mentionné précédemment. Si le code est à sa place une fois que vous avez terminé avec succès la fonction `updateSubtotal()`, vous devriez voir les mises à jour correspondantes dans le champ `Subtotal` du tableau.

Vérifiez [ici](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_30b87c596b79954f63b3482d2f320fe4.gif) la sortie attendue.

<hr>

### Itération 2 : `calculateAll()`

Notre code actuel fonctionne parfaitement pour un produit, mais nous prévoyons d'avoir plus d'un produit dans notre panier. Nous utiliserons donc `calculateAll` pour déclencher la mise à jour des sous-totaux pour chaque produit.

Complétez la fonction appelée `calculateAll()`. Son but est d'appeler la fonction `updateSubtotal` avec chaque nœud DOM `tr.product` dans le tableau `table#cart`.

Pour commencer, supprimez ou commentez le code existant à l'intérieur de `calculateAll()` (le code que nous avons utilisé à des fins de test). De plus, ajoutons un nouveau produit à notre fichier `index.html`. Dupliquez le `tr` avec la classe `product`, renommez le produit à l'intérieur et changez le prix du produit.

![](https://i.imgur.com/Pv4NmR8.png)

:bulb: *Astuce*: Commencez par obtenir les nœuds DOM pour chaque ligne de produit. Actuellement, nous avons deux produits ; donc nous avons deux lignes avec la classe `product`. Il serait peut-être utile d'utiliser [getElementsByClassName](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName) ici.


```js
function calculateAll() {
  // ...
}
```

Le résultat final devrait ressembler à ce qui suit :

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_0efb56fc0e5717469417e806fa7cde12.gif)

<hr>

### Itération 3 : Total

Notre fonctionnalité de calcul est encore incomplète. Le sous-total de chaque produit est mis à jour, mais la valeur totale reste inchangée.

À la fin de la fonction `calculateAll()`, réutilisez la valeur totale que vous venez de calculer lors de l'itération précédente et mettez à jour l'élément DOM correspondant. Calculez le prix total des produits dans votre panier en additionnant tous les sous-totaux renvoyés par `updateSubtotal()` lorsqu'il est appelé avec chaque produit.

Enfin, affichez cette valeur sur votre DOM.

![](https://i.imgur.com/SCtdzMd.png)

<hr>

## Itérations bonus

### Itération 4 : Suppression d'un produit

Les utilisateurs doivent pouvoir supprimer des produits de leur panier. À cet effet, chaque ligne de produit de notre tableau comporte un bouton "Remove" ("Supprimer") à la fin.

Mais essayons de résoudre notre problème petit à petit. À l'intérieur de la fonction existante que vous passez à `window.addEventListener()`, commencez par interroger le document pour tous les boutons "Supprimer", parcourez-les et ajoutez un écouteur d'événements `click` à chacun, en passant une fonction nommée `removeProduct` en tant qu'argument de rappel. Si vous avez besoin d'une indication sur la façon de faire cela, regardez comment nous l'avons fait en ajoutant un écouteur d'événements sur `calculatePricesBtn`.

Nous avons déjà déclaré `removeProduct(event)` et ajouté du code de démarrage. Une fois que vous avez terminé de sélectionner les boutons de suppression et d'ajouter l'écouteur d'événements click sur eux, ouvrez la console et cliquez sur n'importe quel bouton `Remove` ("Supprimer").

Comme nous pouvons le voir, `removeProduct(event)` attend l'événement en tant qu'argument unique, ce qui déclenchera la suppression du produit correspondant du panier.

bulb: Conseil : Pour accéder à l'élément sur lequel l'événement a été déclenché, vous pouvez vous référer à `event.currentTarget`. Pour supprimer un nœud du DOM, vous devez accéder à son nœud parent et appeler [`removeChild`](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild) sur celui-ci. Vous pouvez accéder au parent d'un nœud DOM à partir de sa propriété `parentNode`.

Assurez-vous que le prix est mis à jour lorsque vous supprimez des produits du panier.


Cliquez [ici](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_17b1e9e4d2606239163dddbc5b2a3d9f.gif) pour voir le résultat attendu.

<hr>


### Itération 5 : Création de nouveaux produits

Pour terminer, nous permettrons aux utilisateurs d'ajouter un produit personnalisé à leur panier.

Décommentez l'élément `tfoot` et ses éléments enfants dans le fichier `index.html` :

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


Les deux entrées à l'intérieur de `tfoot` représentent respectivement le nom du nouveau produit et le prix unitaire. Lorsqu'il est déclenché, le bouton "Créer le produit" devrait ajouter un nouveau produit au panier.

Ajoutez un gestionnaire d'événements `click` au bouton "Créer le produit" qui appellera une fonction nommée `createProduct` en tant que rappel.

Dans `createProduct`, vous devriez cibler les nœuds DOM des entrées de nom et de prix unitaire, extraire leurs valeurs, ajouter une nouvelle ligne au tableau avec le nom du produit et le prix unitaire, ainsi que l'entrée de quantité et le bouton "Remove" ("Supprimer"), et vous assurer que toute la fonctionnalité fonctionne comme prévu.

Rappelez-vous, le nouveau produit ne devrait pas se distinguer et se comporter comme l'un des produits précédemment inclus dans le panier. Ainsi, on devrait pouvoir calculer son sous-total lorsque le bouton "Calculer tout" est cliqué et supprimer le produit.

Lorsque la création du produit est finalisée, veuillez vider les champs de saisie dans le formulaire de création.

Cliquez [ici](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_00abbd15326ec24d93147196024458f6.gif) pour voir le résultat attendu.

<br>


**Bon travail ! 💙**

<br>


## FAQs

<br>

<details>
  <summary>Je suis bloqué dans l'exercice et je ne sais pas comment résoudre le problème ni par où commencer.</summary>
  <br>


  Si vous êtes bloqué dans votre code et que vous ne savez pas comment résoudre le problème ou par où commencer, vous devriez prendre du recul et essayer de formuler une question claire sur le problème spécifique auquel vous êtes confronté. Cela vous aidera à cibler le problème et à développer des solutions potentielles.

  Par exemple, s'agit-il d'un concept que vous ne comprenez pas, ou recevez-vous un message d'erreur que vous ne savez pas comment résoudre ? Il est généralement utile de tenter de formuler le problème le plus clairement possible, en incluant les messages d'erreur que vous recevez. Cela peut vous aider à communiquer le problème à d'autres personnes et éventuellement obtenir de l'aide de vos camarades de classe ou de ressources en ligne.

  Une fois que vous avez une compréhension claire du problème, vous pourrez commencer à travailler vers la solution.

  [Retour en haut](#faqs)
</details>

<details>
  <summary>Comment boucler sur un tableau en utilisant la méthode <code>forEach()</code> ?</summary>
  
  <br>

  La méthode `forEach()` exécute une fonction fournie une fois pour chaque élément du tableau. Elle ne renvoie pas un nouveau tableau, mais exécute la fonction sur chaque élément du tableau.

  La syntaxe de la méthode `forEach()` est la suivante :

  ```js
  array.forEach( function(element) {
    // code to be executed for each element
  });
  ```

  Voici un exemple qui utilise la méthode `forEach()` pour afficher chaque élément et son index dans un tableau dans la console :

  ```js
  const fruits = ['apple', 'banana', 'cherry'];

  fruits.forEach( function(element, index) {
    console.log(`${index}: ${element}`);
  });
  ```

  Vous pouvez également utiliser une fonction fléchée comme fonction de rappel pour `forEach()` :

  ```js
  fruits.forEach((element, index) => {
    console.log(`${index}: ${element}`);
  });
  ```

  [Retour en haut](#faqs)

</details>

<details>
  <summary>Comment puis-je ajouter un nouvel élément DOM à un élément existant ?</summary>

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

  This will create a new `p` element with the text `"This is a new paragraph."` and append it to the element with the ID `parent`.

  Cela créera un nouvel élément `p` avec le texte `"This is a new paragraph."` ("Il s'agit d'un nouveau paragraphe.") et l'ajoutera à l'élément avec l'ID `parent`.

[Retour en haut](#faqs)

</details>

<details>
  <summary>Pourquoi faut-il convertir certaines valeurs d'éléments du DOM en nombres alors qu'elles semblent déjà être des nombres ?</summary>
  <br>

  Cela est dû au fait que toutes les valeurs en HTML sont des chaînes de caractères et que toutes les valeurs d'attributs sont des chaînes de caractères. Par conséquent, les valeurs d'éléments du DOM sont renvoyées sous forme de chaînes de caractères même si elles contiennent des valeurs numériques.

  Si vous souhaitez utiliser une valeur d'un élément du DOM en tant que nombre, vous devrez la convertir en un type numérique.

  Voici un exemple d'accès et de conversion de la valeur de l'élément `price` en un nombre à l'aide de JavaScript :

  ```js
  // Get the input element
  const input = item.querySelector('input');

  // Convert the string value of the input element to a number
  const value = Number(input.value);
  ```

  [Retour en haut](#faqs)

</details>

<details>
  <summary>Je continue à obtenir le résultat `NaN` dans mon programme. Comment puis-je le corriger ?</summary>

  <br>


  En JavaScript, `NaN` signifie "Not a Number" (pas un nombre). Il s'agit d'une valeur spéciale qui représente un problème avec une opération numérique ou une tentative de coercition de type échouée. Il existe plusieurs raisons pour lesquelles vous pourriez obtenir `NaN` comme résultat dans votre code JavaScript :

  1. **Division d'un nombre par `0`** : Toute opération qui implique la division d'un nombre par `0` (zéro) donnera `NaN`. Exemple :

   ```js
   const result = 10 / 0;
   
   console.log(result); // NaN
   ```

   <br>

  2. **Analyse d'un nombre invalide** : Si vous essayez d'analyser `undefined` ou une chaîne de caractères qui ne peut pas être représentée comme un nombre à l'aide des fonctions `parseInt()` et `parseFloat()`, vous obtiendrez `NaN` comme résultat.
   
   <br>

   Exemple d'analyse d'une valeur invalide avec `parseInt()` :

   ```js
   const result1 = parseInt("ironhack");
   const result2 = parseInt(undefined);
   
   console.log(result1); // NaN
   console.log(result2); // NaN
   ```

   <br>
   Exemple d'analyse d'une valeur invalide avec `parseFloat()` :

   ```js
   const result1 = parseFloat("ironhack");
   const result2 = parseFloat(undefined);
   
   console.log(result1); // NaN
   console.log(result2); // NaN
   ```

   <br>
   
   Exemple d'analyse d'une valeur invalide avec `Number()` :

   ```js
   const result1 = Number("ironhack");
   const result2 = Number(undefined);
   
   console.log(result1); // NaN
   console.log(result2); // NaN
   ```

   <br>

   

  Pour résoudre le problème `NaN` dans votre code, vous pouvez essayer quelques choses :

  - Vérifiez si vous essayez de diviser un nombre par `0`.
  - Assurez-vous que les chaînes que vous essayez de convertir en nombres sont réellement des représentations valides de nombres. Vous pouvez utiliser `console.log()` pour vérifier les valeurs de vos variables et voir si c'est le problème.

  [Retour en haut](#faqs)

</details>

<details>
  <summary>Comment ajouter un écouteur d'événement ?</summary>
  <br>

  Utilisez la méthode `addEventListener()` pour ajouter un écouteur d'événement. Cette méthode prend deux arguments : le *type d'événement* et la *fonction gestionnaire d'événement* qui sera appelée lorsque l'événement se produit.

  Voici un exemple de la façon d'ajouter un écouteur d'événement click à un élément de bouton :

  ```js
  const button = document.querySelector('button');

  function handleClick() {
    console.log('Button was clicked');
  }

  button.addEventListener('click', handleClick);
  ```

  Cela ajoutera un écouteur d'événement de clic à l'élément `button` (bouton), qui appellera la fonction `handleClick()` chaque fois que le bouton est cliqué.

  Pour plus d'informations, consultez : [MDN - addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

  [Retour en haut](#faqs)

</details>

<details>
  <summary>Comment supprimer un écouteur d'événement ?</summary>
  
  <br>

  Utilisez la méthode `removeEventListener()` pour supprimer un écouteur d'événement. Cette méthode prend deux arguments : le *type d'événement* et la *fonction gestionnaire d'événement* qui a été initialement assignée lors de l'ajout de l'écouteur d'événement.

  Par exemple, supposons que vous ayez ajouté l'écouteur d'événement suivant :

  ```js
  button.addEventListener('click', handleClick);
  ```

  Pour supprimer cet écouteur d'événement, vous pouvez utiliser le code suivant :

  ```js
  button.removeEventListener('click', handleClick);
  ```

  Pour plus d'informations, consultez : [MDN - removeEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)

  [Retour en haut](#faqs)

</details>

<details>
  <summary>Pourquoi est-ce que j'obtiens <code> null </code> lorsque j'essaie d'accéder à un élément HTML ?</summary>

  <br>

  Il y a quelques raisons possibles pour lesquelles vous pourriez obtenir une valeur `null` lorsque vous essayez d'accéder à un élément DOM en JavaScript :

  1. **Vous utilisez le mauvais sélecteur ou vous faites une faute de frappe dans le nom** : Assurez-vous d'utiliser le sélecteur correct et l'orthographe pour accéder à l'élément. Si vous utilisez un sélecteur incorrect ou faites une faute de frappe dans le nom, vous obtiendrez une valeur null lorsque vous essayez d'accéder à l'élément.

  2. **L'élément n'est pas encore chargé** : Si vous essayez d'accéder à un élément qui n'est pas encore chargé dans le DOM (par exemple, un élément défini dans un script qui est chargé en bas de la page), vous obtiendrez une valeur `null` lorsque vous essayez d'y accéder. Vous pouvez résoudre ce problème en enveloppant votre code dans un gestionnaire d'événement `window.onload`, ce qui garantira que l'élément est disponible avant l'exécution de votre code :

   ```js
   window.addEventListener("load", function (event) {
     const element = document.querySelector('#my-element');
     // now you can safely access the element
   };
   ```

  [Retour en haut](#faqs)

</details>

<details>
  <summary>Je ne peux pas pousser les modifications vers le dépôt. Que dois-je faire ?</summary>
  <br>

There are a couple of possible reasons why you may be unable to *push* changes to a Git repository:

1. **Vous n'avez pas validé vos modifications** : Avant de pouvoir pousser vos modifications vers le dépôt, vous devez les valider en utilisant la commande `git commit`. Assurez-vous d'avoir validé vos modifications et essayez de pousser à nouveau. Pour ce faire, exécutez les commandes terminal suivantes à partir du dossier du projet :
  ```bash
  git add .
  git commit -m "Your commit message"
  git push
  ```
2. **Vous n'avez pas la permission de pousser vers le dépôt** : Si vous avez cloné le dépôt directement à partir du dépôt principal d'Ironhack sans faire d'abord une Fork, vous n'avez pas d'accès en écriture au dépôt.
Pour vérifier à quel dépôt distant vous avez cloné, exécutez la commande terminal suivante à partir du dossier du projet :
  ```bash
  git remote -v
  ```
Si le lien affiché est le même que le dépôt principal d'Ironhack, vous devrez d'abord effectuer un fork du dépôt vers votre compte GitHub, puis cloner votre fork sur votre machine locale pour pouvoir pousser les modifications.

**Note**: Vous devriez faire une copie de votre code local pour éviter de le perdre dans le processus.

  [Retour en haut](#faqs)

</details>
