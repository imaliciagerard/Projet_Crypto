# Projet_Crypto

python3 -m venv venv
source venv/bin/activate

pip install cryptography
pip install requests
pip install ec

Pour tester le code Python3 et Python2 sur certficiat révoqué --> python3 validate-cert.py PEM ISRGRootX1.pem E5.pem revoked-badssl-com.pem
Pour tester le code Python3 et Python2 sur certificat le monde --> python3 validate-cert.py PEM GlobalSign-Root.pem GlobalSign-Intermediate-2024.pem lemonde-fr.pem
Pour tester le code Python3 et Python2 sur certificat le tbs --> python3 validate-cert.py PEM ISRGRootX1.pem E5.pem revoked-badssl-com.pem
