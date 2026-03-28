# eBay Scraper - GitHub Actions Integration

Ce projet a été réorganisé pour fonctionner avec GitHub Actions, permettant de traiter plus de 12 000 produits par lots de 200 afin d'éviter les blocages par eBay.

## Structure du Projet

*   `fetch_parts_images_github_actions.py` : Le script de scraping principal, modifié pour accepter des paramètres de lot (`--start-index` et `--end-index`).
*   `merge_results.py` : Un script utilitaire pour fusionner les fichiers Excel générés par chaque lot en un seul fichier final.
*   `ebay_scraper_workflow.yml` : Le fichier de configuration du workflow GitHub Actions.
*   `merged_corrected_final_FIXED.xlsx` : Votre fichier source contenant les produits.

## Comment l'utiliser sur GitHub

1.  **Créer un nouveau dépôt GitHub** (privé de préférence).
2.  **Uploader tous les fichiers** de ce dossier dans le dépôt.
3.  **Activer les GitHub Actions** dans l'onglet "Actions" de votre dépôt.
4.  **Lancer le workflow manuellement** :
    *   Allez dans l'onglet "Actions".
    *   Sélectionnez "eBay Scraper Batch Processing" dans la liste de gauche.
    *   Cliquez sur "Run workflow".
    *   Vous pouvez ajuster la taille des lots (par défaut 200) et le nombre total de produits (par défaut 12 000).
5.  **Récupérer les résultats** :
    *   Une fois que tous les jobs de la matrice sont terminés, le job `merge-results` s'exécutera.
    *   Le fichier final `final_ebay_results.xlsx` sera disponible en tant qu'artefact dans le résumé de l'exécution du workflow.

## Avantages de cette approche

*   **Parallélisation** : GitHub Actions peut exécuter plusieurs jobs en même temps, ce qui accélère considérablement le processus.
*   **Rotation d'IP** : Chaque runner GitHub Actions possède sa propre adresse IP, ce qui réduit drastiquement les chances d'être bloqué par eBay.
*   **Résilience** : Si un lot échoue, vous pouvez relancer uniquement ce lot sans recommencer tout le processus.

## Remarques importantes

*   **Limites de GitHub Actions** : GitHub Actions a des limites de temps d'exécution et de nombre de jobs parallèles (généralement 20 jobs simultanés pour les comptes gratuits). Le workflow est conçu pour gérer cela.
*   **Stockage des Artefacts** : Les résultats intermédiaires sont conservés pendant 1 jour pour économiser de l'espace, tandis que le résultat final est conservé selon vos paramètres par défaut.
