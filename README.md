# Projet Cryptographie 

## Sommaire
- [Commandes pour lancer le programme](#commandes-pour-lancer-le-programme)
- [Tester les différents codes](#tester-les-différents-codes)
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

## Partie 3.1 - Validation d'un certificat d'autorité racine

### Certificat révoqué

```bash
python3 validate-cert.py PEM revoked-badssl-com.pem --issuer E5.pem
```

### Certificat du site lemonde.fr

```bash
python3 validate-cert.py PEM lemonde-fr.pem --issuer GlobalSign-Intermediate-2024.pem
```

### Certificat du site www.tbs-certificats.com

```bash
python3 validate-cert.py PEM www-tbs-certificats-com.pem --issuer Correct-Sectigo-Intermediate.pem
```

> **Remarque :**  
> Vous pouvez exécuter les commandes sans préciser l'issuer.  
> Cependant, le programme détectera que le certificat n'est pas auto-signé et demandera de fournir un issuer.


---


## Partie 3.2 - Validation d'une chaine de certificats

### Certificat révoqué

```bash
python3 validate-cert-chain.py PEM ISRGRootX1.pem E5.pem revoked-badssl-com.pem
```

### Certificat du site lemonde.fr

```bash
python3 validate-cert-chain.py PEM GlobalSign-Root.pem GlobalSign-Intermediate-2024.pem lemonde-fr.pem
```

### Certificat du site www.tbs-certificats.com

```bash
python3 validate-cert-chain.py PEM Root-USERTrust.pem Correct-Sectigo-Intermediate.pem www-tbs-certificats-com.pem
```

---


## Partie 3.3 - Vérification du status de révocation 

### Certificat révoqué

```bash
python3 validate-cert-chain2.py PEM ISRGRootX1.pem E5.pem revoked-badssl-com.pem
```

### Certificat du site lemonde.fr

```bash
python3 validate-cert-chain2.py PEM GlobalSign-Root.pem GlobalSign-Intermediate-2024.pem lemonde-fr.pem
```

### Certificat du site www.tbs-certificats.com

```bash
python3 validate-cert-chain2.py PEM Root-USERTrust.pem Correct-Sectigo-Intermediate.pem www-tbs-certificats-com.pem
```


---


# Captures d'écran

Des captures d'écran illustrant les résultats sont disponibles dans les branches suivantes :

- `tbs`
- `lemonde`
- `revokedbadssl`

