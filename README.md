# Country App

![Capture d'écran](./capture%20d%27%C3%A9cran.png)

Démo : https://projetsdevfanny.github.io/TP9_FromScratch_Country_app_JS/

Projet d'entraînement pour pratiquer les API, le `fetch` et la manipulation du DOM.

## Objectif

Afficher une liste de pays (cartes) issue de l'API Rest Countries, avec recherche,
tri et contrôle du nombre de résultats.

## API utilisée -> A tester dans le navigateur

Rest Countries v3.1 :
`https://restcountries.com/v3.1/all?fields=name,capital,cca3,flags,region,population,translations`

## Fonctionnalités

- Chargement des pays via `fetch`
- Affichage des cartes pays
- Recherche par nom de pays
- Tri par population, nom ou région
- Limitation du nombre de cartes via un range

## Structure conseillée (logique de rendu)

1. Récupérer les données et les stocker dans un tableau.
2. Filtrer selon la recherche.
3. Trier selon le bouton actif.
4. Limiter le nombre de résultats.
5. Mapper vers le HTML des cartes.

Exemple de pipeline de rendu :

```js
countriesContainer.innerHTML = countriesData
  .filter((country) =>
    country.name.common.toLowerCase().includes(inputSearch.value.toLowerCase())
  )
  .sort((a, b) => {
    if (sortBy === "population") return b.population - a.population;
    if (sortBy === "name") return a.name.common.localeCompare(b.name.common);
    if (sortBy === "region") return a.region.localeCompare(b.region);
    return 0;
  })
  .slice(0, Number(inputRange.value))
  .map((country) => `
    <div class="card">
      <img src="${country.flags.svg}" alt="Drapeau de ${country.name.common}">
      <h2>${country.translations.fra.common}</h2>
      <p>Capitale : ${country.capital?.[0] ?? "N/A"}</p>
      <p>Population : ${country.population.toLocaleString()}</p>
      <p>Région : ${country.region}</p>
    </div>
  `)
  .join("");
```

## À tester

1. Ouvrir `index.html` dans le navigateur.
2. Vérifier que les pays s'affichent.
3. Tester la recherche, le tri et le range.
