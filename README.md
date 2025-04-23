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
