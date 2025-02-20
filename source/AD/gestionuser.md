# Documentation : Création des utilisateurs et groupes dans Active Directory (Windows Server 2019)

## 1. Prérequis
- Un serveur Windows Server 2019 configuré en tant que Contrôleur de Domaine.
- L'outil **Active Directory Users and Computers** (ADUC) installé.
- Un compte administrateur AD pour effectuer les modifications.

---

## 2. Création des Groupes
### Accéder à Active Directory Users and Computers
1. Ouvrir **"Active Directory Users and Computers"** (`dsa.msc`).
2. Naviguer jusqu'à l'OU (Organizational Unit) où vous souhaitez créer les groupes.
3. Clic droit sur l'OU > **New** > **Group**.
4. Renseigner les informations :
   - **Nom du groupe** : Exemple `RH`, `Dev`, `SSI`
   - **Scope du groupe** : Global
   - **Type du groupe** : Security
5. Cliquer sur **OK**.

### Groupes à créer
- **RH** (Ressources Humaines)
- **Dev** (Développement)
- **SSI** (Sécurité des Systèmes d'Information)

---

## 3. Création des Utilisateurs
1. Dans **Active Directory Users and Computers**, naviguer vers l'OU cible.
2. Clic droit sur l'OU > **New** > **User**.
3. Renseigner les champs :
   - **Prénom et Nom**
   - **Nom d'utilisateur** (sAMAccountName)
4. Cliquer sur **Next**.
5. Renseigner le mot de passe et cocher :
   - **User cannot change password**
   - **Password never expires**
6. Cliquer sur **Finish**.

### Utilisateurs à créer
| Nom Complet | Nom d'utilisateur | Groupe |
|-------------|------------------|--------|
| Rose Rouge  | r.rouge          | RH (Responsable) |
| Rachel Rime | r.rime           | RH |
| David Drean | d.drean          | Dev (Responsable) |
| Dwayne Dig  | d.dig            | Dev |
| Sacha Souf  | s.souf           | SSI (Responsable) |
| Sandrine Sodo | s.sodo         | SSI |


---

## 4. Ajout des Utilisateurs aux Groupes
1. Dans **Active Directory Users and Computers**, ouvrir l'utilisateur.
2. Aller dans l'onglet **Member Of**.
3. Cliquer sur **Add** et sélectionner le groupe correspondant.
4. Valider avec **OK**.

---

## 5. Automatisation avec PowerShell

Pour automatiser la création des utilisateurs et leur assignation aux groupes, utiliser le script suivant :

```powershell
$users = @(
    @{Name="Rose Rouge";Username="r.rouge";Password="AQWzsxEDCrfv12345.";Group="RH"},
    @{Name="Rachel Rime";Username="r.rime";Password="AQWzsxEDCrfv12345.";Group="RH"},
    @{Name="David Drean";Username="d.drean";Password="AQWzsxEDCrfv12345.";Group="Dev"},
    @{Name="Dwayne Dig";Username="d.dig";Password="AQWzsxEDCrfv12345.";Group="Dev"},
    @{Name="Sacha Souf";Username="s.souf";Password="AQWzsxEDCrfv12345.";Group="SSI"},
    @{Name="Sandrine Sodo";Username="s.sodo";Password="AQWzsxEDCrfv12345.";Group="SSI"}
)

foreach ($user in $users) {
    New-ADUser -Name $user.Name -SamAccountName $user.Username -UserPrincipalName "$($user.Username)@domain.local" \
    -Path "OU=Employes,DC=domain,DC=local" -AccountPassword (ConvertTo-SecureString $user.Password -AsPlainText -Force) \
    -Enabled $true -PasswordNeverExpires $true
    
    Add-ADGroupMember -Identity $user.Group -Members $user.Username
}
```

---

## 6. Vérification des Utilisateurs et Groupes
- Utiliser la commande **PowerShell** suivante pour lister les utilisateurs d'un groupe :

```powershell
Get-ADGroupMember -Identity "RH"
Get-ADGroupMember -Identity "Dev"
Get-ADGroupMember -Identity "SSI"
```

- Pour vérifier qu'un utilisateur existe :

```powershell
Get-ADUser -Identity "r.rouge"
```

---

## 7. Conclusion
Cette documentation explique comment créer des utilisateurs et des groupes dans Active Directory sur Windows Server 2019, que ce soit via l'interface graphique ou en utilisant PowerShell pour automatiser le processus.

