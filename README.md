# Projet_Crypto
Commandes pour lancer le programme :

python3 -m venv venv

source venv/bin/activate


pip install cryptography

pip install requests

pip install ec


Pour tester les différents codes (Python1, Python2 et Python3)

Pour tester le code Python1 : - python3 validate-cert.py PEM revoked-badssl-com.pem --issuer E5.pem

                              - python3 validate-cert.py PEM lemonde-fr.pem --issuer GlobalSign-Intermediate-2024.pem 
                          
                              - python3 validate-cert.py PEM www-tbs-certificats-com.pem --issuer Correct-Sectigo-Intermediate.pem
                              
On peut les lancer sans l'issuer mais le code va préciser que les certificats ne sont pas auto-signés et donc qu'il faut préciser l'issuer.

Pour tester le code Python3 et Python2 sur certficiat révoqué --> python3 validate-cert.py PEM ISRGRootX1.pem E5.pem revoked-badssl-com.pem

Pour tester le code Python3 et Python2 sur certificat le monde --> python3 validate-cert.py PEM GlobalSign-Root.pem GlobalSign-Intermediate-2024.pem lemonde-fr.pem

Pour tester le code Python3 et Python2 sur certificat le tbs --> python3 validate-cert.py PEM Root-USERTrust.pem Correct-Sectigo-Intermediate.pem www-tbs-certificats-com.pem

Il y a des captures d'écrans de chaque codes dans les branches concernées (tbs, lemonde, revokedbadssl)




# Projet Crypto

## Sommaire
- [Commandes pour lancer le programme](#commandes-pour-lancer-le-programme)
- [Tester les différents codes (Python1, Python2, Python3)](#tester-les-différents-codes-python1-python2-python3)
- [Captures d'écran](#captures-décran)

---

# Commandes pour lancer le programme

## 1. Créer un environnement virtuel

```bash
python3 -m venv venv
```

## 2. Activer l'environnement virtuel

```bash
source venv/bin/activate
```

## 3. Installer les dépendances

```bash
pip install cryptography
```

```bash
pip install requests
```

```bash
pip install ec
```

---

# Tester les différents codes (Python1, Python2, Python3)

## 1. Tester Python1

### 1.1. Certificat révoqué

```bash
python3 validate-cert.py PEM revoked-badssl-com.pem --issuer E5.pem
```

### 1.2. Certificat du site lemonde.fr

```bash
python3 validate-cert.py PEM lemonde-fr.pem --issuer GlobalSign-Intermediate-2024.pem
```

### 1.3. Certificat du site www.tbs-certificats.com

```bash
python3 validate-cert.py PEM www-tbs-certificats-com.pem --issuer Correct-Sectigo-Intermediate.pem
```

> **Remarque :**  
> Vous pouvez exécuter les commandes sans préciser l'issuer.  
> Cependant, le programme détectera que le certificat n'est pas auto-signé et demandera de fournir un issuer.

---

## 2. Tester Python2 et Python3

### 2.1. Certificat révoqué

```bash
python3 validate-cert.py PEM ISRGRootX1.pem E5.pem revoked-badssl-com.pem
```

### 2.2. Certificat du site lemonde.fr

```bash
python3 validate-cert.py PEM GlobalSign-Root.pem GlobalSign-Intermediate-2024.pem lemonde-fr.pem
```

### 2.3. Certificat du site www.tbs-certificats.com

```bash
python3 validate-cert.py PEM Root-USERTrust.pem Correct-Sectigo-Intermediate.pem www-tbs-certificats-com.pem
```

---

# Captures d'écran

Des captures d'écran illustrant les résultats sont disponibles dans les branches suivantes :

- `tbs`
- `lemonde`
- `revokedbadssl`

