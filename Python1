import argparse
import sys
import cryptography.x509
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives.asymmetric import rsa, padding, ec
from cryptography.hazmat.primitives import hashes, serialization
from datetime import datetime, timezone

def load_certificate(cert_format, cert_file):
    try:
        with open(cert_file, "rb") as f:
            cert_data = f.read()

        if cert_format.upper() == "PEM":
            return cryptography.x509.load_pem_x509_certificate(cert_data, default_backend())
        elif cert_format.upper() == "DER":
            return cryptography.x509.load_der_x509_certificate(cert_data, default_backend())
        else:
            print("Format non supporté. Utilisez DER ou PEM.")
            sys.exit(1)
    except Exception as e:
        print(f"Erreur lors du chargement du certificat : {e}")
        sys.exit(1)

def extract_certificate_info(cert):
    try:
        key_usage_ext = cert.extensions.get_extension_for_class(cryptography.x509.KeyUsage).value
    except cryptography.x509.ExtensionNotFound:
        key_usage_ext = "Non défini"

    info = {
        "Sujet": cert.subject.rfc4514_string(),
        "Émetteur": cert.issuer.rfc4514_string(),
        "Validité": f"{cert.not_valid_before_utc} - {cert.not_valid_after_utc}",
        "Numéro de série": cert.serial_number,
        "Signature Algorithme": cert.signature_algorithm_oid._name,
        "\n Clé publique": cert.public_key().public_bytes(
            encoding=serialization.Encoding.PEM,
            format=serialization.PublicFormat.SubjectPublicKeyInfo
        ).decode(),
        "\n Key Usage": key_usage_ext
    }
    return info

def is_self_signed(cert):
    return cert.issuer == cert.subject

def verify_certificate_signature(cert, issuer_cert):
    try:
        public_key = issuer_cert.public_key()
        signature = cert.signature
        data = cert.tbs_certificate_bytes
        hash_algorithm = cert.signature_hash_algorithm

        if isinstance(public_key, rsa.RSAPublicKey):
            public_key.verify(
                signature,
                data,
                padding.PKCS1v15(),
                hash_algorithm
            )
        elif isinstance(public_key, ec.EllipticCurvePublicKey):
            public_key.verify(
                signature,
                data,
                ec.ECDSA(hash_algorithm)
            )
        else:
            return "Type de clé publique non supporté."
        return "La signature du certificat est valide."
    except Exception as e:
        return f"Échec de la vérification de la signature : {e}"

def check_validity_period(cert):
    now = datetime.now(timezone.utc)
    not_before = cert.not_valid_before_utc
    not_after = cert.not_valid_after_utc
    if not_before <= now <= not_after:
        return "Le certificat est actuellement valide (période OK)."
    else:
        return "Le certificat est expiré ou non encore valide."

def main():
    parser = argparse.ArgumentParser(description="Validation d’un certificat X.509 avec signature et période")
    parser.add_argument("format", choices=["DER", "PEM"], help="Format du certificat")
    parser.add_argument("certfile", help="Fichier du certificat à analyser")
    parser.add_argument("--issuer", help="Fichier du certificat émetteur (si non auto-signé)", default=None)
    args = parser.parse_args()

    cert = load_certificate(args.format, args.certfile)
    cert_info = extract_certificate_info(cert)

    print("\n" + "*"*60)
    print("Informations extraites du certificat :".center(60))
    print("*"*60)
    for key, value in cert_info.items():
        print(f"{key}: {value}")
        
    print("\n" + "*"*60)
    print("Résultats des vérifications :".center(60))
    print("*"*60)

    # Vérification de la signature
    if is_self_signed(cert):
        print("Certificat auto-signé détecté (racine). Vérification avec sa propre clé...")
        verification_result = verify_certificate_signature(cert, cert)
    elif args.issuer:
        issuer_cert = load_certificate(args.format, args.issuer)
        print(f"\n Vérification de la signature avec le certificat émetteur : {args.issuer}")
        verification_result = verify_certificate_signature(cert, issuer_cert)
    else:
        verification_result = " Ce certificat n'est pas auto-signé. Spécifiez --issuer pour vérifier sa signature."

    print(verification_result)

    # Vérification de la période de validité
    validity_result = check_validity_period(cert)
    print(validity_result)
    print()

if __name__ == "__main__":
    main()
