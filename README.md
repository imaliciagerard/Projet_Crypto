# Projet Cryptographie 

## Sommaire
- [Commandes pour lancer le programme](#commandes-pour-lancer-le-programme)
- [Tester les différents codes (Python1, Python2, Python3)](#tester-les-différents-codes)
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

# Tester les différents codes 

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
python3 validate-cert-chain.py PEM ISRGRootX1.pem E5.pem revoked-badssl-com.pem
```

### 2.2. Certificat du site lemonde.fr

```bash
python3 validate-cert-chain.py PEM GlobalSign-Root.pem GlobalSign-Intermediate-2024.pem lemonde-fr.pem
```

### 2.3. Certificat du site www.tbs-certificats.com

```bash
python3 validate-cert-chain.py PEM Root-USERTrust.pem Correct-Sectigo-Intermediate.pem www-tbs-certificats-com.pem
```

---

# Captures d'écran

Des captures d'écran illustrant les résultats sont disponibles dans les branches suivantes :

- `tbs`
- `lemonde`
- `revokedbadssl`

