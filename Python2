import argparse
import sys
import hashlib
import cryptography.x509
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.x509.oid import ExtensionOID

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

def get_hash_function(hash_algorithm):
    if isinstance(hash_algorithm, hashes.SHA256):
        return hashlib.sha256
    elif isinstance(hash_algorithm, hashes.SHA384):
        return hashlib.sha384
    elif isinstance(hash_algorithm, hashes.SHA512):
        return hashlib.sha512
    elif isinstance(hash_algorithm, hashes.SHA1):
        return hashlib.sha1
    else:
        raise ValueError(f"Algorithme de hachage non supporté: {hash_algorithm}")

def verify_signature_rsa(public_key, signature, data, hash_algorithm):
    n = public_key.public_numbers().n
    e = public_key.public_numbers().e
    signature_int = int.from_bytes(signature, byteorder='big')
    decrypted_int = pow(signature_int, e, n)
    hash_func = get_hash_function(hash_algorithm)
    data_hash = hash_func(data).digest()
    decrypted_bytes = decrypted_int.to_bytes((n.bit_length() + 7) // 8, byteorder='big')
    return data_hash in decrypted_bytes

def verify_signature_ecdsa(public_key, signature, data, hash_algorithm):
    try:
        public_key.verify(signature, data, cryptography.hazmat.primitives.asymmetric.ec.ECDSA(hash_algorithm))
        return True
    except Exception as e:
        print(f"Erreur lors de la vérification de signature ECDSA : {e}")
        return False

def verify_certificate_chain(certificates):
    for i in range(len(certificates) - 1):
        parent_cert = certificates[i]
        child_cert = certificates[i + 1]

        print(f"\n Vérification de la correspondance Issuer -> Subject")
        print(f"Issuer attendu : {parent_cert.subject}")
        print(f"Issuer du certificat enfant : {child_cert.issuer}")

        if child_cert.issuer != parent_cert.subject:
            print("Erreur : Le sujet de l'émetteur ne correspond pas au certificat parent.")
            return False

        public_key = parent_cert.public_key()
        signature = child_cert.signature
        data = child_cert.tbs_certificate_bytes
        hash_algorithm = child_cert.signature_hash_algorithm

        if isinstance(public_key, cryptography.hazmat.primitives.asymmetric.rsa.RSAPublicKey):
            if not verify_signature_rsa(public_key, signature, data, hash_algorithm):
                print("Échec de la vérification de signature RSA.")
                return False
        elif isinstance(public_key, cryptography.hazmat.primitives.asymmetric.ec.EllipticCurvePublicKey):
            if not verify_signature_ecdsa(public_key, signature, data, hash_algorithm):
                print("Échec de la vérification de signature ECDSA.")
                return False
        else:
            print(f"Type de clé non supporté: {type(public_key)}")
            return False
    return True

def check_basic_constraints(cert):
    try:
        ext = cert.extensions.get_extension_for_oid(ExtensionOID.BASIC_CONSTRAINTS).value
        return ext.ca
    except cryptography.x509.ExtensionNotFound:
        print(f"Avertissement: Extension BasicConstraints non trouvée")
        return False

def check_key_usage(cert, is_ca):
    try:
        key_usage = cert.extensions.get_extension_for_oid(ExtensionOID.KEY_USAGE).value
        if is_ca and not key_usage.key_cert_sign:
            print("Erreur: Certificat CA sans l'autorisation key_cert_sign")
            return False
        if not is_ca and key_usage.key_cert_sign:
            print("Avertissement: Certificat non-CA avec l'autorisation key_cert_sign")
        return True
    except cryptography.x509.ExtensionNotFound:
        print(f"Avertissement: Extension KeyUsage non trouvée")
        return False

def main():
    parser = argparse.ArgumentParser(description="Validation d'une chaîne de certificats X.509")
    parser.add_argument("format", choices=["DER", "PEM"], help="Format des certificats")
    parser.add_argument("certfiles", nargs='+', help="Fichiers de certificats (de la racine à la feuille)")
    args = parser.parse_args()

    certificates = [load_certificate(args.format, file) for file in args.certfiles]

    if not verify_certificate_chain(certificates):
        print("La chaîne de certificats n'est pas valide.")
        sys.exit(1)

    for i, cert in enumerate(certificates):
        is_ca = check_basic_constraints(cert)
        if not check_key_usage(cert, is_ca):
            print(f"Le certificat {i} a une extension KeyUsage incorrecte.")
            sys.exit(1)

    print("\n La chaîne de certificats est valide !")

if __name__ == "__main__":
    main()
