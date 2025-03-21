#!/bin/bash


function configure_backup() {
    echo "=== Configuration d'une Sauvegarde Automatique ==="
    
    # Demander à l'utilisateur le chemin source et destination
    read -p "Entrez le chemin du fichier ou dossier source à sauvegarder : " src
    if [ ! -e "$src" ]; then
        echo "Le fichier ou dossier source '$src' n'existe pas."
        return
    fi

    read -p "Entrez le chemin de destination de la sauvegarde : " dest
    mkdir -p "$dest"
    if [ ! -d "$dest" ]; then
        echo "Le chemin de destination '$dest' est invalide."
        return
    fi

    # Demander l'intervalle de sauvegarde
    echo "À quelle fréquence voulez-vous effectuer cette sauvegarde ?"
    echo "1. Toutes les minutes"
    echo "2. Toutes les heures"
    echo "3. Tous les jours"
    echo "4. Tous les dimanches (hebdomadaire)"
    echo "5. Tous les premiers jours du mois (mensuel)"
    read -p "Votre choix : " frequency

    # Définir la configuration cron en fonction du choix
    case $frequency in
        1) cron_time="* * * * *" ;;
        2) cron_time="0 * * * *" ;;
        3) cron_time="0 0 * * *" ;;
        4) cron_time="0 0 * * 0" ;;
        5) cron_time="0 0 1 * *" ;;
        *)
            echo "Choix invalide. Annulation."
            return
            ;;
    esac

    # Créer une commande de sauvegarde avec rsync
    cron_command="rsync -a --delete \"$src\" \"$dest\""

    # Ajouter la tâche au crontab
    (crontab -l 2>/dev/null; echo "$cron_time $cron_command") | crontab -

    echo "La sauvegarde automatique a été configurée avec succès."
    echo "Source : $src"
    echo "Destination : $dest"
    echo "Fréquence : $frequency"
}


function restore_backup() {
    echo "=== Restauration d'une Sauvegarde ==="
    
    # Demander à l'utilisateur le chemin de la sauvegarde à restaurer
    read -p "Entrez le chemin de la sauvegarde à restaurer : " backup_src
    if [ ! -e "$backup_src" ]; then
        echo "La sauvegarde '$backup_src' n'existe pas."
        return
    fi

    read -p "Entrez le chemin de destination de la restauration : " restore_dest
    mkdir -p "$restore_dest"
    if [ ! -d "$restore_dest" ]; then
        echo "Le chemin de destination '$restore_dest' est invalide."
        return
    fi

    # Restaurer avec rsync
    rsync -a "$backup_src" "$restore_dest"

    echo "La restauration a été effectuée avec succès."
    echo "Sauvegarde : $backup_src"
    echo "Destination : $restore_dest"
}


while true; do
    echo "Que voulez-vous faire ?"
    echo "1. Configurer une sauvegarde automatique"
    echo "2. Restaurer une sauvegarde"
    echo "3. Quitter"
    read -p "Votre choix : " choice

    case $choice in
        1) configure_backup ;;
        2) restore_backup ;;
        3) echo "Au revoir !"; exit 0 ;;
        *) echo "Choix invalide, veuillez réessayer." ;;
    esac
done
