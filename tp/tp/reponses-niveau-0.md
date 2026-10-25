# Schema relationel des 4 tables

## 0.2

1.

categorie(<u>id</u> , libelle).

plat(<u>id</u> , nom , prix , #id_categorie).

commande(<u>id</u> , date_commande , numero_table).

commande_ligne(<u>id</u> , #id_commande , #id_plat ,
quantite , prix_unitaire).

2. Le prix est dans commande_ligne pour faciliter le calcul du prix total , et aussi pour eviter que les donnees a l'ancien change quand on change le prix d'un plat
   Par exemple le cafe devient 1500 , sans Prix unitaire dans la table commande_ligne , on utilise le prix du plat pour calculer le prix d un commande , C est a dire , Les prix totaux d'avant change aussi , ce qui n'est pas logique , Du coup , On met le prix unitaire dans commande_ligne pour stocker le prix actuel du plat
   . si le prix du plat change demain, les anciennes commandes restent justes.

3. Quand je supprime une commande , Les commandes_lignes associees a cette commande sont aussi supprimer , L'instruction responsable est ON DELETE CASCADE

4. On peux inserer un prix negatif ,

## 0.3

a) 200
b) 201
c) 422
d) 409
e) 404

5. parce que 201 est de le code HTTP de created
6. c'est PostgreSQL qui bloque la suppression, et PHP qui la transforme en 409.
   id_plat INTEGER NOT NULL REFERENCES plat(id),
7.

```bash
curl -i X POST http://localhost:8080/api/commandes \
            -H "Content-type:application/json" \
            -d '{"numero_table":6 , "lignes":[{"id_plat":4 , "quantite":2}  , {"id_plat":10 , "quantite":1 }]}'

```

## 0.4

| #   | Étape                                   | Fichier                   | Fonction / ligne |
| --- | --------------------------------------- | ------------------------- | ---------------- |
| 1   | Le formulaire est soumis                | `components/PlatForm.vue` | `enregistrer`    |
| 2   | La requête HTTP est construite          | `api.js`                  | `requete()`      |
| 3   | Vite transmet `/api/...` au port 8080   | `vite.config.js`          | `defineConfig`   |
| 4   | CodeIgniter choisit le contrôleur       | `Plats.php`               | `create`         |
| 5   | Le contrôleur lit le JSON               | `Plats.php`               | `create`         |
| 6   | Les règles de validation sont vérifiées | `Plats.php`               | `create`         |
| 7   | La ligne est insérée                    | `Plats.php`               | `create`         |
| 8   | Le formulaire prévient l’écran parent   | `PlatForm.vue`            | `emit(…)`        |
| 9   | L’écran recharge la liste               | `views/CarteView.vue`     | `charger`        |

8. elle se trouve dans CommandeView.vue (envoyer)qui appelle CommandeService dans Service .

- Le contrôleur s’occupe de HTTP : recevoir la demande et répondre au client.
- Le service s’occupe des règles de l’application : décider si la commande est valide et comment l’enregistrer.

## 0.5

| Notion                 | À quoi ça sert                                                    | Votre exemple       |
| ---------------------- | ----------------------------------------------------------------- | ------------------- |
| `ref()`                | une variable dont le changement met l’écran à jour                | carteView           |
| `computed()`           | une valeur calculée à partir d’autres, recalculée automatiquement | CommandeView        |
| `v-for`                | répéter un élément pour chaque élément d’une liste                | HistoriquueView(31) |
| `v-if` / `v-else`      | afficher un élément seulement si une condition est vraie          | HistoriquueView(31) |
| `v-model`              | lier un champ de formulaire à une variable, dans les deux sens    | CommandeView(74)    |
| `defineProps`          | recevoir des données du composant parent                          | CommandeDetail(4)   |
| `defineEmits` / `emit` | envoyer un événement au composant parent                          | Panier(9)           |
| `onMounted`            | exécuter du code quand le composant apparaît                      | CarteView(40)       |

9) C’est la vue parente, CommandeView.vue, qui possède l’état du panier (panier) et modifie réellement les quantités :

ajouter() augmente la quantité ou ajoute une nouvelle ligne ;
retirer() la diminue, puis supprime la ligne si elle atteint zéro.

10) Le watch surveille la prop plat , Sans le watch, le formulaire resterait rempli avec les valeurs du premier plat sélectionné 

## 0.6
- Dans CarteView.vue, la fonction charger() appelle api.categories.lister().
-     $routes->get('categories', 'Categories::index');
- categories(controller)
- findAll
- CarteView passe le résultat au composant PlatForm.vue avec la prop :categories="categories".
- Boucle sur categorie dans Plat Form
- => les categories sont charge depuis la base 
