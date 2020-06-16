# Eventmaker website theme squeleton

This is a starting point for an Eventmaker Website theme.

## Folder structure

    ├── _data
    │   ├── templates
    │   │   ├── mobicheckin
    |   |   |   ├── website
    |   |   |   │   ├── 01-standard.yml
    ├── mobicheckin
    │   ├── website
    │   │   ├── standard
    |   |   |   ├── config
    |   |   |   |   ├── main_settings_schema.json
    |   |   |   |   ├── presets.en.json
    |   |   |   |   ├── presets.fr.json
    |   |   |   |   ├── sections-groups.json
    |   |   |   |   ├── translations.json
    |   |   |   ├── layouts
    |   |   |   |   ├── // fichier de layout
    |   |   |   |   ├── ...
    |   |   |   ├── sections
    |   |   |   |   ├── // fichier de section
    |   |   |   |   ├── ...
    |   |   |   ├── snippets
    |   |   |   |   ├── // fichier de snippet
    |   |   |   |   ├── ...
    |   |   |   ├── templates
    |   |   |   |   ├── // fichier pour les pages d'erreurs
    |   |   |   |   ├── ...
    └── README.md

# Working on a theme

Le language utilisé est le "Liquid" développé par Shopify, une doc est disponible [ici](https://shopify.github.io/liquid).


# Config

## 01-standard.yml

Ce fichier de config est utilisé pour charger le thème ainsi que lors des rechercher (mises à jour).
Il recense les chemins vers les fichiers du thème (sections, snippets, presets, traductions, layout, etc...).
Lors de l'ajout d'une section par exemple, il ne faut pas oublier de l'ajouté dans le YAML pour qu'elle puisse être séléctionnée via l'interface de personnalisation du site web.

## main\_settings\_schema.json

Ce fichier permet de créer les paramettres généraux du site (réseaux sociaux, couleur des titres, couleur du fond, couleur du texte, espacement des sections, police de caractères, etc...). Tout ce qui n'est pas propre à une seule section et/ou qui doit apparaitre dans le layout devra exister ici.

Pour réutiliser ces paramettres dans le HTML il faut faire `settings.id_of_setting`.
Par exemple pour appeler la couleur du texte du site web: `settigns.color_body_text`.

## presets.fr.json et presets.en.json

Il s'agit des paramettres par défaut à la création d'un site web avec ce thème. Afin d'avoir directement un site web fonctionel vous pouvez choisir les paramettres par défaut, comme les couleurs du site, les pages existantes et les sections présentent sainsi que leur contenu de chacune.

## sections-groups.json

Ce fichier permet de créer différents groupes de sections pour pouvoir les ordonners. Lors de l'ajout d'une section via l'interface de personnalisation les différentes sections sont alors regroupées par groupe (basic, média, inscription, etc...).

## translations.json

Ce fichier permet de configuré tous les textes qui ne sont pas personnalisable mais qui doivent être traduit dans la langue du site web.
Chaque langue est représenté par sa clé ISO (fr pour français, it pour italien, etc...) et chaque traduction est sous la forme `"clé": "valeur"`.

Les traductions peuvent être utiliser partout sur le site en faisant `t.key_of_translation`.

Si vous souhaitez avoir un contenu dynamique dans une traduction celle-ci se présentera comme ceci: `"clé": "Valeur avec un texte %{ variable_ }"`. Pour que cela fonctionne il faut aussi que la où cette traduction est appeler il existe une variable liquid dont la valeur est la partie changeante (`{% capture variable_ %}{{ guest.first_name }}{% endcapture %}`).

# Layout

Les layout sont des squelettes pour chaque page du site. On y retrouve les éléments qui doivent être présent à chaque fois (CSS, Js, Favicon, etc...).

# Sections

Les sections sont les éléments que l'on peut ajouter à une page via l'interface de personnalisation du site web.

Chaque section est constituée de deux parties distinctes: le code HTML et le schéma.

## Le code HTML d'une section

Dans le code HTML vous pouvez utiliser les paramettres généraux `settings.id_of_the_setting`. Mais aussi et surtout les paramettres de la section créer dans le schéma: `section.settings.id_of_the_section_setting`.

## Le schéma d'une section

Le schéma donne des informations sur la section.

    {% schema %}
      {
        ...
      }
    {% endschema %}


Les premiers éléments du schéma sont des informations générales du schéma.

    {% schema %}
      {
        "name": "English title show on the website builder",
        "name_translations": { "fr": "Titre en français afficher dans l'interface de personnalisation du website" },
        "icon": "fa fa-font", // Classes Font-awesome pour l'icone afficher dans l'interface de personnalisation. Ruby gems font-awesome-rails-4.7.0.5
        "hidden_from_user": false,
        "group_key": "basic", // Dans quelle groupe il sera afficher
        "group_rank": 1000, // son ordre dans le groupe
        "settings": [
          ...
        ]
      }
    {% endschema %}

### Les settings

Comment se presentent les `settings` ?

`settings` est un tableau des différents paramettres de la section. De manière général un setting est présenté sous cette forme:

    {
      "type": "text",
      "id": "title",
      "label": "Title",
      "label_translations": { "fr": "Titre" },
      "default": "Title",
      "default_translations": { "fr": "Titre" }
    }

- `type` au choix parmi la liste suivante:

| Type                    | Description |
| ----------------------- | ----------- |
| header                  | Renvois un titre pour une meilleure présentation des paramètres. |
| text                    | Renvois un champ texte simple. |
| paragraph               | Renvois un champ de type textarea. |
| rte                     | Renvois un RTE. |
| image\_picker           | Renvois un uploader d'image. |
| select                  | Renvois une list déroulante des options qui lui sont fournit. |
| checkbox                | Renvois une case à cocher. |
| color                   | Renvois une palette de choix de couleur. |
| guest\_category\_picker | Renvois une liste déroulante des catégories de l'évènement. |
| segments\_picker        | Renvois une liste déroulante des segments de l'évènement. |
| website\_page\_picker   | Renvois une liste déroulante des pages du site. |
| guest\_field\_picker    | Renvois une liste déroulante des champs de participants. |

- `id` est la clé utilisé dans le code HTML pour utilisé la valeur de ce setting: `section.settings.title`.
- `label` est pas défaut en anglais.
- `label_translations` est un tableau de traduction. La plateforme est disponible en français et en anglais uniquement.
- `default` est la valeur remplis par défaut si le back office est en anglais.
- `default_translations` fonctionne de la même façon que `label_translation`

#### Spécificités

Certains types de setting on besoin de plus qu'un label, un id et un type pour fonctionné.

- `select` : les settings de type _select_ on besoin d'options pour fonctionné. L'option vide est générer automatiquement.

Exemple:

    {
      "type": "select",
      "id": "text_size",
      "label": "Text size",
      "label_translations": { "fr": "Taille du texte" },
      "default": "medium",
      "options": [
        {
          "label": "Medium",
          "label_translations": { "fr": "Moyen" },
          "value": "medium"
        },
        {
          "label": "Large",
          "label_translations": { "fr": "Gros" },
          "value": "large"
        }
      ]
    }

- `checkbox` : sa valeur par defaut sera un booléen (`true` / `false`) mais pas une String.
- `color` : sa valeur par defaut doit être un code héxadécimal.

#### Condition d'affichage - Show_if

À chaque setting il est possible d'ajouté une condition d'affichage (`show_if`).
Par exemple on laissera la possibilité de personnalisé la **Couleur du bouton** seulement si la checkbox **"Appliquer un style personnalisé"** est cochée:

    {
      "type": "checkbox",
      "id": "enable_custom_css",
      "label": "Apply a custom style",
      "label_translations": { "fr": "Appliquer un style personnalisé" },
      "default": false
    },
    {
      "type": "color",
      "id": "button_color",
      "label": "Button color",
      "label_translations": { "fr": "Couleur du bouton" },
      "default": "#000",
      "show_if": [
        [
          {
            "source_id": "enable_custom_css",
            "operator": "==",
            "value": true
          }
        ]
      ]
    }



### Les blocs

Les blocs sont des éléments que l'utilisateur de l'interface de personalisation peuvent ajouter dans la limite disponible (si `max_blocks` est définit).

`"type": "text_block"` permet de faire des presets pour qu'un certain nombre de blocs soient créer au moment de l'ajout de la section à une page.
`display_property` prend comme valeur l'id d'un des settings de bloc pour en afficher la valeur dans l'interface de personnalisation. Par exemple `"display_property" : "title"` pour que le titre d'un bloc soit afficher pour le nommer et le retrouver plus facilement dans l'interface de personnalisation de la section.

    {% schema %}
      {
        "max_blocks": 4,
        "settings": [
          ...
        ],
        "blocks": [
          {
            "type": "text_block",
            "name": "Blocks",
            "name_translations": { "fr": "Blocs" },
            "add_block": "Text for a new block",
            "add_block_translations": { "fr": "Texte pour un Nnuveau bloc" },
            "remove_block": "Text to remove this block",
            "remove_block_translations": { "fr": "Texte pour retirer ce bloc" },
            "display_property": "id_of_block_settings",
            "settings": [
              ...
            ]
          }
        ],
        "presets": [
          {
            "name": "Name of the section",
            "name_translations": { "fr": "Nom de la section" },
            "blocks": [
              {
                "type": "text_block"
              },
              {
                "type": "text_block"
              },
              {
                "type": "text_block"
              }
            ]
          }
        ]
      }
    {% endschema %}

# Snippets

Les snippets sont des fragments de codes utilisables dans plusieurs sections. Pour éviter d'avoir plusieurs fois le même code dans différentes sections, vous pouvez inclure un snippet.
Pour cela il suffit de faire `{% include "filename_du_snippet" %}`.

Vous pouvez aussi passer des paramètres à ce snippet: `{% include "filename_du_snippet", section_settings: section.settings %}`.
Et ainsi dans le snippet vous avez accès à ce paramètre `section_settings`.

Contrairement aux sections, les snippets ne comporte pas de schéma.

# Templates

Le dossier `templates` contient des gabaries de pages poru différents cas (fonctionnement normal, erreur 401, erreur 404, Offline, etc...).