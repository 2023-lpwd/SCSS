# Sass

Liste des fonctionnalités Sass.

# 1. Les variables

Nous allons pouvoir stocker une valeur dans une variable et pouvoir la réutiliser partout dans la suite de notre code.

#### CSS :

```css
h1{
  color: red;
}
p{
  color: red;
}
```

#### SCSS :

```scss
// Création d'une variable color
$color : red;

// Utilisation de la variable color
h1{
  color: $color;
}
p{
  color: $color;
}
```

# 2. Concaténation

## 2.1. Signe "&"

Sass a un signe spécifique pour concaténer les sélecteurs parent et enfant : l’esperluette (&).

Pour les pseudosélecteurs (ex: hover, before, etc)

#### SCSS :

```scss
.button {
  background: red;
  &:hover{
    background: blue;
  }
  &:before{
    content: ">";
  }
}
```

#### CSS : 

```css
.button{
  background: red;
}
.button:hover {
   background: blue;
}
.button:before {
   content: ">";
}
```

### 2.1.1. Pour la méthode BEM 

#### SCSS :

```scss
.product {
  padding : 0.5rem;
  &__title {
    text-transform: uppercase;
  }
  &__desc{
    margin-top: 1rem;
  }
}
```

#### CSS : 

```css
.product{
  padding : 0.5rem;
}
.product__title {
   text-transform: uppercase;
}
.product__desc {
   margin-top: 1rem;
}
```

### 2.1.2. Utilisation avancée du signe "&"

On peut utiliser le signe "&" pour concaténer le sélecteur parent après le sélecteur sur lequel nous sommes en train de travailler.

Un exemple très utile : Le darkmode

#### HTML : 

```HTML
<html class="dark-mode">
...
  <section class="page">
    <button class="button">Button</button>
  </section>
...
</html>
```

#### SCSS :

```scss
.button {
  background : black;
  .dark-mode &{ 
    background : white; 
  }
}
.page {
  background : white;
  .dark-mode &{ 
    background : black; 
  }
}
```

#### CSS :

```css
.button {
  background : black;
 }
.dark-mode .button{ 
  background : white; 
}

.page {
  background : white;
 }
.dark-mode .page{ 
  background : black; 
}

```

On peut stocker le signe "&" dans une variable pour pouvoir l'utiliser en dehors du "scope" du sélecteur parent

#### SCSS :

```scss
.product {
  $this: &;
  &__title {
    background: red;
  }
  &:hover{
    #{$this}__title{
      background: blue;
    }
  }
}
```

#### CSS :

```css
.product__title {
  background: red;
}
.product:hover .product__title{
  background: blue;
}

```

## 2.2. signe ">", "+", "~", etc

```scss
.parent {
  > .enfant {
    padding: 1rem;
  }
}
.element {
  + suivant{
    padding: 1rem;
  }
}
```

#### CSS : 

```css
.parent > .enfant {
  padding: 1rem;
}
```

# 3. Découpage des fichiers

Sass va nous permettre de découper notre code CSS en plusieurs fichiers. Cette fonctionnalité est très utile car nous pourrons créer des fichiers pour un composant en particulier.

#### SCSS : 

```scss
// /components/_product.scss
.product {
  padding: 0.5rem;
  &__title {
    margin-top: 1rem;
  }
}
```

```scss
// /components/_nav.scss
.nav {
  display: flex;
  list-style: none;
  &__item {
    padding: 0.5rem;
  }
}
```

```scss
// main.scss
@import '/components/nav';
@import '/components/product';
```

Attention, l'ordre des import est très important pour utiliser des variables ou des fonctions. Vous devez impérativement les importer avant le reste.

# 4. Opérations

Faire des opérations peut être utile dans notre code scss. Pour celà nous avons les signes "+", "-", "\*" et "math.div"
Attention, seul petit bémol pour utiliser la division, on va devoir utiliser le module "@use "sass:math";"

#### SCSS :

```scss
@use 'sass:math';

.content {
  padding-top: math.div(9, 16) * 100%;
}
```

#### CCSS :

```css
.content {
  padding-top: 56.25%;
}
```

# 5. Mixins

Les variables nous permettent de stocker une valeur réutilisables dans notre code. Mais quelques fois nous aimerions pouvoir stocker plusieurs attributs et valeurs pour les réutiliser. C'est le rôle du "Mixin".

### Définir un mixin avec "@mixin"

#### SCSS :

```scss
@mixin title {
  font-weight: bold;
  text-transform: uppercase;
}
```

### Utilisation d'un mixin avec @include

#### SCSS :

```scss
.title {
  font-size: 1rem;
  @include title();
}

.sous-titre {
  font-size: 0.8rem;
  @include title();
}
```

#### CSS :

```css
.title {
  font-size: 1rem;
  font-weight: bold;
  text-transform: uppercase;
}

.sous-titre {
  font-size: 0.8rem;
  font-weight: bold;
  text-transform: uppercase;
}
```

### Utilisation d'un mixin avec du contenu dynamique grace à "@content"

Un mixin permet de stocker plusieurs attributs mais il permet également d'ajouter du contenu dynamique. Très utile pour les mediaqueries, par exemple.

#### SCSS :

```scss
@mixin small-up {
   @media all and (min-width: 640px) {
      @content;
   }
}

.title{
  font-size: 1rem;
  @include small-up(){
    font-size: 2rem;
  }
}
.text {
 font-size: 0.8rem;
  @include small-up(){
    font-size: 1.6rem;
  }
}
```

```css
.title{
  font-size: 1rem;
}
@media all and (min-width: 640px) {
  .title{
    font-size: 2rem;
  }
}
.text{
  font-size: 0.8rem;
}
@media all and (min-width: 640px) {
  .text{
    font-size: 1.6rem;
  }
}
```

# 6. Fonctions

Certaines fois, nous allons avoir besoin de faire des petites tâches répétitives. Pour éviter de les refaire à plusieurs reprises, nous allons créer des fonctions.

### Définir une fonction avec "@function" et retourner une valeur avec "@return"

#### SCSS :

```scss
$rootFontSize: 16;
@function pxToRem($value) {
  @return math.div($value, $rootFontSize) + rem;
}
```

Fonction qui prends en paramètre $value (valeur en px) et nous retourne la valeur en rem

### Utilisation d'une fonction

Pour utiliser une fonction, il suffit de l'appeler à la place d'une valeur pour un attribut

#### SCSS :

```scss
.title {
  font-size: pxToRem(16);
}
.title {
  font-size: pxToRem(14);
}
```

#### CSS :

```css
.title {
  font-size: 1rem;
}
.title {
  font-size: 0.875rem;
}
```

# 7. Les boucles

Parfois, les boucles sont intéressantes pour éviter d'avoir du code similaire à copier coller

### Création d'une boucles avec @for

#### SCSS :

```scss
@use 'sass:math';

.row {
  display: flex;
  flex-wrap: wrap;
}

@for $i from 1 through 12 {
  .col-#{$i} {
    width: math.div($i, 12) * 100%;
    flex-basis: math.div($i, 12) * 100%;
  }
}
```

Exemple pour faire sa propre grille en seulement quelques lignes.

#### CSS:

```css
.row {
  display: flex;
  flex-wrap: wrap;
}
.col-1 {
  width: 8.3333333333%;
  flex-basis: 8.3333333333%;
}

.col-2 {
  width: 16.6666666667%;
  flex-basis: 16.6666666667%;
}

.col-3 {
  width: 25%;
  flex-basis: 25%;
}

.col-4 {
  width: 33.3333333333%;
  flex-basis: 33.3333333333%;
}

.col-5 {
  width: 41.6666666667%;
  flex-basis: 41.6666666667%;
}

.col-6 {
  width: 50%;
  flex-basis: 50%;
}

.col-7 {
  width: 58.3333333333%;
  flex-basis: 58.3333333333%;
}

.col-8 {
  width: 66.6666666667%;
  flex-basis: 66.6666666667%;
}

.col-9 {
  width: 75%;
  flex-basis: 75%;
}

.col-10 {
  width: 83.3333333333%;
  flex-basis: 83.3333333333%;
}

.col-11 {
  width: 91.6666666667%;
  flex-basis: 91.6666666667%;
}

.col-12 {
  width: 100%;
  flex-basis: 100%;
}
```
