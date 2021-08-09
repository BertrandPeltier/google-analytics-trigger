# google-analytics-trigger

Méthode pour gérer / déclencher ou pas le tracking par Google Analytics selon le consentement du visiteur.

## Init & config du script gtag

### Balise head
```html
<head>
  ...
  <!-- Global site tag (gtag.js) - Google Analytics -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXX"></script>  <!-- "XXX" ID personnelle fournie par GA" -->
  <script type="text/javascript" src="./js/gtagInit.js"></script> <!-- Script config de gtag exporté dans un fichier, il est possible de le laisser en dur dans la balise <head> -->
  ...
</head>
```

### Script config gtag "gtagInit.js"
```Javascript
  window.dataLayer = window.dataLayer || [];
  function gtag(){
      dataLayer.push(arguments);
  }
  // Comportement par défaut : Aucune donnée n'est stocké, le tracking du visiteur n'est pas activé
  gtag('consent', 'default', {
    'ad_storage': 'denied',
    'analytics_storage': 'denied'
  });
  // Paramétrage de la méthode gtag
  gtag('js', new Date());
  gtag('config', 'G-XXX', {     // "XXX" ID personnelle
    'anonymize_ip': true,       // Anonymisation des IPs = RGPD friendly
    'cookie_expires': 33696000  // Durée de stockage des cookies (13 mois) = RGPD friendly
  });
  
  /**
   * Fonction permettant de mettre à jour le comportement de gtag, son exécution permet d'activer le tracking et le stockage des données
   */
  function consentGranted() {
      gtag('consent', 'update', {
          'ad_storage': 'granted',
          'analytics_storage': 'granted'
      });
  };
  
  /**
   * Fonction permettant de lister les paramètres de la méthode gtag, et donc de savoir si le tracking et le stockage des données sont activés
   * @param {object} w (window)
   * @param {string} d ("dataLayer")
   * @param {string} t ("")
   * @returns string
   */
   function consentStatus(w, d, t) {
      for (i of w[d]) {
          t += JSON.stringify(i).replaceAll(/\"\d{1,}\":/g, "") + "\n";
      }
      return t;
  };
  // Exemple d'appel : console.log(consentStatus(window, "dataLayer", ""));
```

## Trigger

### Body
```html
  <!-- Script Trigger à positionner en début de balse <body>, avant la modal de consentement
  <script type="text/javascript" src="./js/gtagHandler.js"></script>
```
