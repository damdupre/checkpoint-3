## Partie 1 : Gestion des utilisateurs

### Q.1.1.1  
Pour commencer j'ai identifier les attribut de Kelly Rhameur avec la commande `Get-ADuser -Identity "Kelly.Rhameur" -Properties * | Select-Objey `  
ceux qui m'a donné 
```ps1
AccountExpirationDate                :
accountExpires                       : 9223372036854775807
AccountLockoutTime                   :
AccountNotDelegated                  : False
AllowReversiblePasswordEncryption    : False
AuthenticationPolicy                 : {}
AuthenticationPolicySilo             : {}
BadLogonCount                        : 0
badPasswordTime                      : 0
badPwdCount                          : 0
CannotChangePassword                 : False
CanonicalName                        : TSSR.LAN/LabUsers/DirectionDesRessourcesHumaines/Kelly.Rhameur
Certificates                         : {}
City                                 :
CN                                   : Kelly.Rhameur
codePage                             : 0
Company                              : CyberOps
CompoundIdentitySupported            : {}
Country                              :
countryCode                          : 0
Created                              : 21/12/2023 11:33:50
createTimeStamp                      : 21/12/2023 11:33:50
Deleted                              :
Department                           : Direction des Ressources Humaines
Description                          :
directReports                        : {CN=Chris.Shin,OU=Recrutement,OU=DirectionDesRessourcesHumaines,OU=LabUsers,DC=T
                                       SSR,DC=LAN, CN=Uriel.Hubert,OU=SanteEtSecuriteAuTravail,OU=DirectionDesRessource
                                       sHumaines,OU=LabUsers,DC=TSSR,DC=LAN, CN=Yves.Delavega,OU=Recrutement,OU=Directi
                                       onDesRessourcesHumaines,OU=LabUsers,DC=TSSR,DC=LAN, CN=Cedric.Caron,OU=GestionDe
                                       sPerformances,OU=DirectionDesRessourcesHumaines,OU=LabUsers,DC=TSSR,DC=LAN...}
DisplayName                          : Kelly.Rhameur
DistinguishedName                    : CN=Kelly.Rhameur,OU=DirectionDesRessourcesHumaines,OU=LabUsers,DC=TSSR,DC=LAN
Division                             : DirectionDesRessourcesHumaines
DoesNotRequirePreAuth                : False
dSCorePropagationData                : {01/01/1601 01:00:00}
EmailAddress                         : Kelly.Rhameur@TSSR.LAN
EmployeeID                           :
EmployeeNumber                       :
Enabled                              : True
Fax                                  :
GivenName                            : Kelly
HomeDirectory                        :
HomedirRequired                      : False
HomeDrive                            :
HomePage                             :
HomePhone                            :
Initials                             :
instanceType                         : 4
isDeleted                            :
KerberosEncryptionType               : {}
LastBadPasswordAttempt               :
LastKnownParent                      :
lastLogoff                           : 0
lastLogon                            : 0
LastLogonDate                        :
LockedOut                            : False
logonCount                           : 0
LogonWorkstations                    :
mail                                 : Kelly.Rhameur@TSSR.LAN
Manager                              : CN=Camille.Martin,OU=DirectionGenerale,OU=LabUsers,DC=TSSR,DC=LAN
MemberOf                             : {CN=GrpUsersDirectionDesRessourcesHumaines,OU=DirectionDesRessourcesHumaines,OU=
                                       LabUsers,DC=TSSR,DC=LAN}
MNSLogonAccount                      : False
MobilePhone                          :
Modified                             : 21/12/2023 12:44:21
modifyTimeStamp                      : 21/12/2023 12:44:21
msDS-User-Account-Control-Computed   : 8388608
Name                                 : Kelly.Rhameur
nTSecurityDescriptor                 : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                       : CN=Person,CN=Schema,CN=Configuration,DC=TSSR,DC=LAN
ObjectClass                          : user
ObjectGUID                           : 3401b8b0-de22-492a-a20b-b0bdbce7c0cb
objectSid                            : S-1-5-21-1634400263-1414186445-1484066983-6763
Office                               :
OfficePhone                          :
Organization                         :
OtherName                            :
PasswordExpired                      : True
PasswordLastSet                      : 21/12/2023 11:33:50
PasswordNeverExpires                 : False
PasswordNotRequired                  : False
POBox                                :
PostalCode                           :
PrimaryGroup                         : CN=Domain Users,CN=Users,DC=TSSR,DC=LAN
primaryGroupID                       : 513
PrincipalsAllowedToDelegateToAccount : {}
ProfilePath                          :
ProtectedFromAccidentalDeletion      : False
pwdLastSet                           : 133476284307155090
SamAccountName                       : Kelly.Rhameur
sAMAccountType                       : 805306368
ScriptPath                           :
sDRightsEffective                    : 15
ServicePrincipalNames                : {}
SID                                  : S-1-5-21-1634400263-1414186445-1484066983-6763
SIDHistory                           : {}
SmartcardLogonRequired               : False
sn                                   : Rhameur
State                                :
StreetAddress                        :
Surname                              : Rhameur
Title                                : Directrice des Ressources Humaines
TrustedForDelegation                 : False
TrustedToAuthForDelegation           : False
UseDESKeyOnly                        : False
userAccountControl                   : 512
userCertificate                      : {}
UserPrincipalName                    : Kelly.Rhameur@TSSR.LAN
uSNChanged                           : 63239
uSNCreated                           : 61740
whenChanged                          : 21/12/2023 12:44:21
whenCreated                          : 21/12/2023 11:33:50
PropertyNames                        : {AccountExpirationDate, accountExpires, AccountLockoutTime,
                                       AccountNotDelegated...}
AddedProperties                      : {}
RemovedProperties                    : {}
ModifiedProperties                   : {}
PropertyCount                        : 107
```
J'ai essayer de faire un script pout crée le nouvel utilisateur avec les attributs, mais aprés plusieurs tentatives infurctueuses , j'ai crée l'utilisateur manuellement via l'interface graphique de AD en faisant un copier coller entre les deux propriété des utilisateurs.


### Q.1.1.2
Pour la céation de l' OU je me place sur à la racine du domaine est je fait clic droit **New -> Organization Unit** .    
![création OU](https://github.com/damdupre/checkpoint-3/blob/main/images/cr%C3%A9ation%20OU.png)   
J'ai par la suite sélectionné l'utilisateur à déplacer puis clic droit sur **Move** .    
![move](https://github.com/damdupre/checkpoint-3/blob/main/images/move.png)  
On vois bien l'utilisateur dans la bonne OU .  
![desact](https://github.com/damdupre/checkpoint-3/blob/main/images/DESACT.png) .  
### Q.1.1.3
Je commence par crée un groupe `DeactivatedUsers`  
![newgroup](https://github.com/damdupre/checkpoint-3/blob/main/images/new%20group.png)  

On vois que l'utilisateur fait partie du groupe `GrpUsersDirectionDesRessourcesHumaines` et du groupe `Domain Users`  
![group](https://github.com/damdupre/checkpoint-3/blob/main/images/group.png)  
Je clique sur remove pour la supprimer du groupe `GrpUsersDirectionDesRessourcesHumaines` puis sur `Add` pour l'ajouter au nouveaux groupe `DeactivatedUsers`   

### Q.1.1.4
Pour créer le dossier et archiver l'autre j'ai utilisé un script :
```ps1
# Variables de l'environnement
$newUser = "Lionel.Lemarchand"
$newUserFolder = "F:\$newUser"  # Chemin local sur le disque F:
$oldUser = "kelly.rhameur"
$oldUserFolder = "F:\$oldUser"
$archiveFolder = "$oldUserFolder-ARCHIVE"

# Création du dossier du nouvel utilisateur
if (-not (Test-Path $newUserFolder)) {
    New-Item -ItemType Directory -Path $newUserFolder
    Write-Host "Dossier personnel créé pour $newUser à l'emplacement $newUserFolder"
} else {
    Write-Host "Le dossier $newUserFolder existe déjà."
}

# Archivage du dossier de Kelly Rhameur
if (Test-Path $oldUserFolder) {
    Rename-Item -Path $oldUserFolder -NewName $archiveFolder
    Write-Host "Le dossier de $oldUser a été renommé en $archiveFolder"
} else {
    Write-Host "Le dossier $oldUserFolder n'existe pas."
}

# Définir les permissions pour le dossier du nouvel utilisateur
$acl = Get-Acl $newUserFolder
$permission = "DOMAIN\$newUser", "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl -Path $newUserFolder -AclObject $acl
Write-Host "Permissions définies pour $newUser sur $newUserFolder"
```
resulat :  
![resultat](https://github.com/damdupre/checkpoint-3/blob/main/images/resultat.png)  
![dossier](https://github.com/damdupre/checkpoint-3/blob/main/images/image.png).  

## Partie 2 : Restriction utilisateurs

### Q.1.2.1
Pour trouver l'utilisateur **Gabriel Ghul** j'ai rechercher dans les membres du groupe `Domain Users` (car il y a une faute dans le nom) .    
![recherche](https://github.com/damdupre/checkpoint-3/blob/main/images/recherche.png)  

Je allé sur le'utilisateur en je suis aller dans les propriété du compte, la dans `Account` en cliquant sur `Logon Hours`on a une interface qui nous permet de gérer les heure de connexion.  
![blocage](https://github.com/damdupre/checkpoint-3/blob/main/images/blocage.png)  
on oublie pas d'appliquer avant de sortir .  

### Q.1.2.2
De la même maniére en cliquant sur `Log On To...` on peut bloquer la connexion .  
![pd](https://github.com/damdupre/checkpoint-3/blob/main/images/pc.png)  

### Q.1.2.3
J'ai créer une GPO lié à l'OU `LabUsers` pour définir la stratégie de mot de passe.  
![password](https://github.com/damdupre/checkpoint-3/blob/main/images/password.png).  

## Partie 3 : Lecteurs réseaux  

### Q.1.3.1 
Dans le gestionnaire de GPO je créer une GPO avec pour chemin `User Configuration->Préférence->Windows Settings->Drive Maps` clique droit `New` .  
![mappage 1](https://github.com/damdupre/checkpoint-3/blob/main/images/mappage%201.png)    
![mappage2](https://github.com/damdupre/checkpoint-3/blob/main/images/mappage%202.png)    
