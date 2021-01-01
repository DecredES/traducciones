# Verificador de Decred

### 1. Introducción
Siempre que se recomienda verificar la firma digital y los hashes de un archivo para evitar que los (troyanos) o cualquier otro código malicioso modifiquen archivos en tránsito o en reposo, el usuario promedio encuentra algunas dificultades para comprender el proceso y ejecutar los comandos necesarios. A veces porque el usuario carece de conocimientos técnicos o porque el proceso puede variar en función de cómo se generó la firma o método criptográfico elegido.
El script Decred Verifier hace precisamente eso: descarga los paquetes Decred, sus firmas digitales y hashes del repositorio oficial de Github y verifica que sean válidos. Es un script (bash o python) que no necesita privilegios de root y se ejecuta en macOS y Linux (se supone que la versión de python se ejecuta en Windows).
Para descargar la clave de desarrolladores Decred, el script iniciará una conexión TCP 11371 (hkp) al servidor de claves MIT, y para descargar los paquetes de Github será necesario "hablar" TCP 443 (https).
Obtenga más información sobre la [verificación de firmas digitales](en español....).

## 2. Bash
Para ejecutar el script, pegue las ~ 180 líneas de código a continuación en un archivo local y agregue el atributo ejecutable como se muestra a continuación:

```
$ chmod +x decred_verifier.sh
```
verificador de Decred:
```
#! /bin/bash -

# Decred Verifier v0.2.1
# This script downloads and verifies the digital signature and hash of Decred packages.
# Decrediton and the Decred installer are developed by Decred developers and are available at decred.org
# Reference: https://github.com/decred/decred-binaries/
# Author: Marcelo Martins (stakey.club)

# Syntax: decred_verifier.sh [version number] [mode]
# Version defaults to the latest auto-detected version if left blank
# Mode of operation must be:
# 0 = Decrediton (default)
# 1 = Decred installer
# Examples: ./decred_verifier.sh
#           ./decred_verifier.sh 1.4.0
#           ./decred_verifier.sh 1.4.0 1

print_help () {
    echo "Version defaults to the latest auto-detected version if left blank"
    echo "Mode of operation must be:"
    echo "0 = Decrediton (default)"
    echo "1 = Decred installer"
    echo "Syntax:   $0 [version] [mode]"
    echo "Examples: $0"
    echo "          $0 1.4.0"
    echo "          $0 1.4.0 0"
    exit 1
}

if [[ `uname -a | grep -c "Darwin"` -gt 0 ]]; then
	FLAVOR="Mac"
elif [[ `uname -a | grep -c "Linux"` -gt 0 ]]; then
	FLAVOR="Linux"
elif [[ `uname -m | grep -c "arm"` -gt 0 ]]; then
	FLAVOR="ARM"
else
    FLAVOR="Unknown"
fi

if [[ -z $1 ]]; then
    if [[ $FLAVOR == "Mac" ]]; then
        VERSION=`curl -Ssf -L https://github.com/decred/decred-binaries/tags | grep -o 'v[0-9]\.[0-9]\{1,2\}\.[0-9]\{1,2\}' | head -n 1 | cut -d'v' -f 2`
    else
        VERSION=`wget -S -nv -q -O - https://github.com/decred/decred-binaries/tags | grep -o 'v[0-9]\.[0-9]\{1,2\}\.[0-9]\{1,2\}' | head -n 1 | cut -d'v' -f 2`
    fi
    if [[ -z $VERSION ]]; then
        echo -e "ERROR: Package version must be provided.\n"
        print_help
    fi
elif [[ $1 =~ ^[0-9]\.[0-9]{1,2}\.?[0-9]{0,2} ]]; then
    VERSION=$1
else
    echo -e "ERROR: Package version seems to be in the wrong format.\n"
    print_help
fi

if [[ -z $2 ]]; then
    MODE=0
elif [[ $2 =~ ^[0-1]$ ]]; then
    MODE=$2
else
    echo -e "ERROR: Mode of operation is invalid.\n"
    print_help
fi

DCR_KEYHOST="pgp.mit.edu"
DCR_KEY="0x518A031D"
DCR_GITHUB="https://github.com/decred/decred-binaries/releases/download"
DCR_GITHUB_TAG="https://github.com/decred/decred-binaries/releases/tag"
MANIFEST_DECREDITON="manifest-decrediton-v$VERSION.txt"
MANIFEST_ASC_DECREDITON="manifest-decrediton-v$VERSION.txt.asc"
DECREDITON_DMG="decrediton-v$VERSION.dmg"
DECREDITON_TAR="decrediton-v$VERSION.tar.gz"
MANIFEST_DECRED="manifest-v$VERSION.txt"
MANIFEST_ASC_DECRED="manifest-v$VERSION.txt.asc"
DECRED_DARWIN="decred-darwin-amd64-v$VERSION.tar.gz"
DECRED_PKG_AMD64="decred-linux-amd64-v$VERSION.tar.gz"
DECRED_PKG_386="decred-linux-386-v$VERSION.tar.gz"
DECRED_PKG_ARM="decred-linux-arm-v$VERSION.tar.gz"
DECRED_PKG_ARM64="decred-linux-arm64-v$VERSION.tar.gz"

[[ ! -x `which gpg` ]] && echo "ERROR: Could not locate gpg." && exit 5
if [[ $FLAVOR == "Mac" ]]; then
	[[ ! -x `which curl` ]] && echo "ERROR: Could not locate curl." && exit 6
	if [[ $MODE -eq 0 ]]; then
		MANIFEST=$MANIFEST_DECREDITON
		MANIFEST_ASC=$MANIFEST_ASC_DECREDITON
		DECRED_PKG=$DECREDITON_DMG
	else
		MANIFEST=$MANIFEST_DECRED
		MANIFEST_ASC=$MANIFEST_ASC_DECRED
		DECRED_PKG=$DECRED_DARWIN
	fi
elif [[ $FLAVOR == "Linux" ]]; then
	[[ ! -x `which wget` ]] && echo "ERROR: Could not locate wget." && exit 7
	if [[ $MODE -eq 0 ]]; then
		MANIFEST=$MANIFEST_DECREDITON
		MANIFEST_ASC=$MANIFEST_ASC_DECREDITON
		DECRED_PKG=$DECREDITON_TAR
	else
		MANIFEST=$MANIFEST_DECRED
		MANIFEST_ASC=$MANIFEST_ASC_DECRED
		if [[ `uname -a | grep -c "386"` -eq 0 ]]; then
			DECRED_PKG=$DECRED_PKG_AMD64
		else
			DECRED_PKG=$DECRED_PKG_386
		fi
	fi
else
	[[ ! -x `which wget` ]] && echo "ERROR: Could not locate wget." && exit 7
	if [[ $MODE -eq 0 ]]; then
		echo "ERROR: There is no Decrediton for ARM platform."
		exit 8
	else
		MANIFEST=$MANIFEST_DECRED
		MANIFEST_ASC=$MANIFEST_ASC_DECRED
		if [[ `uname -a | grep -c "arm64"` -gt 0 ]]; then
			DECRED_PKG=$DECRED_PKG_ARM64
		else
			DECRED_PKG=$DECRED_PKG_ARM
		fi
	fi
fi

if [[ `gpg --with-colons -k $DCR_KEY | grep -c "Decred"` -eq 1 ]]; then
	echo "Decred developers' public key $DCR_KEY has already been imported."
else
	echo "Importing Decred developers' key $DCR_KEY from $DCR_KEYHOST"
	GPG_OUT=$(gpg --with-colons --status-fd 1 --keyserver $DCR_KEYHOST --recv-keys $DCR_KEY 2> /dev/null)
	if [[ `echo $GPG_OUT | grep -c FAILURE` -eq 1 ]]; then
		echo "[ FAIL ] GPG failed to import key $DCR_KEY from $DCR_KEYHOST. It may have timed out."
		echo "Make sure you can send traffic using port TCP 11371."
		exit 2
	fi
fi

if [[ $FLAVOR == "Mac" ]]; then
  if [[ `curl -Is $DCR_GITHUB_TAG/v$VERSION | head -n 1 | grep -c "404"` -gt 0 ]]; then
    echo "ERROR: could not find release version $VERSION on Github."
    exit 8
  fi
else
  if [[ `wget -S -nv -q -O - $DCR_GITHUB_TAG/v$VERSION 2>&1 | head -n 1 | grep -c "404"` -gt 0 ]]; then
    echo "ERROR: could not find release version $VERSION on Github."
    exit 8
  fi
fi

if [[ ! -f $MANIFEST || ! -f $MANIFEST_ASC || ! -f $DECRED_PKG ]]; then
	echo "Downloading files from Github..."
	if [[ $FLAVOR == "Mac" ]]; then
		[[ ! -f $MANIFEST ]] && echo "- $MANIFEST" && curl -L -# $DCR_GITHUB/v$VERSION/$MANIFEST -o $MANIFEST
		[[ ! -f $MANIFEST_ASC ]] && echo "- $MANIFEST_ASC" && curl -L -# $DCR_GITHUB/v$VERSION/$MANIFEST_ASC -o $MANIFEST_ASC
		[[ ! -f $DECRED_PKG ]] && echo "- $DECRED_PKG" && curl -L -# $DCR_GITHUB/v$VERSION/$DECRED_PKG -o $DECRED_PKG
	else
		[[ ! -f $MANIFEST ]] && wget -nv -q --show-progress $DCR_GITHUB/v$VERSION/$MANIFEST
		[[ ! -f $MANIFEST_ASC ]] && wget -nv -q --show-progress $DCR_GITHUB/v$VERSION/$MANIFEST_ASC
		[[ ! -f $DECRED_PKG ]] && wget -nv -q --show-progress $DCR_GITHUB/v$VERSION/$DECRED_PKG
	fi
else
	echo "All files have already been copied locally."
	echo "- $MANIFEST"
	echo "- $MANIFEST_ASC"
	echo "- $DECRED_PKG"
fi

echo -e "\nVerifying digital signature and hash...\n"
GPG_OUT2=$(gpg --with-colons --status-fd 1 --verify $MANIFEST_ASC 2> /dev/null)

if [[ `echo $GPG_OUT2 | grep -c GOOD` -eq 1 ]]; then
	if [[ $FLAVOR == "Mac" ]]; then
		[[ ! -x `which shasum` ]] && echo "ERROR: Could not locate shasum." && exit 9
		SHASUM=`shasum -a 256 $DECRED_PKG | cut -d' ' -f1`
	else # Linux or ARM
		[[ ! -x `which sha256sum` ]] && echo "ERROR: Could not locate sha256sum." && exit 9
		SHASUM=`sha256sum $DECRED_PKG | cut -d' ' -f1`
	fi

	if [[ `grep -c $SHASUM $MANIFEST` -eq 1 ]]; then
		echo "[ SUCCESS ] GPG signature for $MANIFEST successfully verified."
		echo -e "[ SUCCESS ] SHA-256 hash of $DECRED_PKG successfully verified.\n"
		echo -e "It's now safe to delete files $MANIFEST and $MANIFEST_ASC\n" # The script won't delete files.
		echo "RESULT: SUCCESS. It means that $DECRED_PKG wasn't modified after its creation"
		echo "and that the file that contains the proof was signed by Decred developers' private key."
		echo -e "Learn more at https://stakey.club/en/verifying-digital-signatures/\n"
		# The script won't install or run the wallet.
		if [[ $FLAVOR == "Mac" && $MODE -eq 0 ]]; then
			echo "To install, open $DECRED_PKG and move Decrediton.app to the Applications folder."
		else
			echo "To install, unpack $DECRED_PKG, enter the directory and run the executable."
		fi
	else
		echo -e "[ SUCCESS ] GPG signature for $DECRED_PKG successfully verified."
		echo -e "[ FAIL ] SHA-256 hash of $DECRED_PKG verification FAILED!\n"
		echo "RESULT: FAIL. It means that the manifest wasn't modified after its creation"
		echo "but $DECRED_PKG is incomplete or was probably modified after its creation."
		exit 3
	fi
else
	echo -e "[ FAIL ] GPG did not find a \"good signature\" for $MANIFEST. Verification FAILED!"
	exit 4
fi
```
## 3. Python Script
Para ejecutar este script en Python, los siguientes paquetes deben estar instalados en la computadora. En Windows, se deben instalar Python y gpg4win:
```
$ sudo pip install python-gnupg
$ sudo pip install certifi

$ python decred_verifier.sh
```
verificador de Decred:
```
#!/usr/bin/env python3

import argparse
from pathlib import Path
import re
import platform
import urllib.request
import ssl
import hashlib
import gnupg
import shutil
import certifi
import os

# Decred Verifier v0.2
# This script downloads and verifies the digital signature and hash of Decred packages.
# Decrediton and the Decred installer are developed by Decred developers and are available at decred.org
# Reference: https://github.com/decred/decred-binaries/
# Author: Marcelo Martins (stakey.club)

# Syntax: python decred_verifier.py -v [version number] -m [mode]
# Version defaults to the latest auto-detected version if left blank
# Mode of operation must be:
# 0 = Decrediton (default)
# 1 = Decred installer
# Examples: $ python decred_verifier.py
#           $ python decred_verifier.py -v 1.4.0
#           $ python decred_verifier.py -v 1.4.0 -m 0
#           $ python decred_verifier.py -v 1.4.0 -m 1

parser = argparse.ArgumentParser()
parser.add_argument("-v", "--version", help="Version to download", dest='version')
parser.add_argument("-m", "--mode", help="Mode of operation", dest='mode')
args = parser.parse_args()

__version__ = '0.2'


def print_help():
    print("Version defaults to the latest auto-detected version if left blank")
    print("Mode of operation must be:")
    print("0 = Decrediton (default)")
    print("1 = Decred installer")
    print("Syntax:  $0 -v [version] -m [mode]")
    print("Example: $0")
    print("         $0 -v 1.4.0")
    print("         $0 -v 1.4.0 -m 0")
    exit(1)


client_context = ssl.SSLContext(protocol=ssl.PROTOCOL_TLS_CLIENT)
client_context.options |= ssl.OP_NO_TLSv1
client_context.options |= ssl.OP_NO_TLSv1_1
client_context.load_default_certs(purpose=ssl.Purpose.SERVER_AUTH)
client_context.load_verify_locations(capath=certifi.where())

if not args.version:
    reVERSION = re.search('archive/v([0-9]{1,2}.[0-9]{1,2}.?[0-9]{0,2}).',
                          str(urllib.request.urlopen("https://github.com/decred/decred-binaries/tags",
                                                     context=client_context).read(90000)))
    VERSION = reVERSION.group(1)
    if not VERSION:
        print("ERROR: Package version must be provided.\n")
        print_help()
elif re.search('^[0-9]{1,2}.[0-9]{1,2}.?[0-9]{0,2}', args.version):
    VERSION = args.version
else:
    VERSION = ""
    print("ERROR: Package version seems to be in the wrong format.\n")
    print_help()


if not args.mode:
    MODE = 0
elif re.search('^[0-1]$', args.mode):
    MODE = args.mode
else:
    MODE = 0
    print("ERROR: Mode of operation is invalid.\n")
    print_help()

HASH_BLOCKSIZE = 65536
DCR_KEYHOST = "pgp.mit.edu"
DCR_KEY = "0x518A031D"
DCR_KEY_LONG_FP = "6D897EDF518A031D"
GITHUB_HOST = "github.com"
DCR_GITHUB_DIR = "/decred/decred-binaries/releases/download"
DCR_GITHUB = "https://github.com/decred/decred-binaries/releases/download"
DCR_GITHUB_TAG = "https://github.com/decred/decred-binaries/releases/tag"
DCR_GITHUB_TAG_DIR = "/decred/decred-binaries/releases/tag"
MANIFEST_DECREDITON = "manifest-decrediton-v" + VERSION + ".txt"
MANIFEST_ASC_DECREDITON = "manifest-decrediton-v" + VERSION + ".txt.asc"
DECREDITON_DMG = "decrediton-v" + VERSION + ".dmg"
DECREDITON_TAR = "decrediton-v" + VERSION + ".tar.gz"
MANIFEST_DECRED = "manifest-v" + VERSION + ".txt"
MANIFEST_ASC_DECRED = "manifest-v" + VERSION + ".txt.asc"
DECRED_DARWIN = "decred-darwin-amd64-v" + VERSION + ".tar.gz"
DECRED_PKG_AMD64 = "decred-linux-amd64-v" + VERSION + ".tar.gz"
DECRED_PKG_386 = "decred-linux-386-v" + VERSION + ".tar.gz"
DECRED_PKG_ARM = "decred-linux-arm-v" + VERSION + ".tar.gz"
DECRED_PKG_ARM64 = "decred-linux-arm64-v" + VERSION + ".tar.gz"
DECRED_EXE_386 = "decred-windows-386-v" + VERSION + ".zip"
DECRED_EXE_AMD64 = "decred-windows-amd64-v" + VERSION + ".zip"
DECREDITON_EXE = "decrediton-v" + VERSION + ".exe"
MANIFEST, MANIFEST_ASC, DECRED_PKG = "", "", ""

if platform.system() == 'Darwin':
    if int(MODE) == 0:
        MANIFEST = MANIFEST_DECREDITON
        MANIFEST_ASC = MANIFEST_ASC_DECREDITON
        DECRED_PKG = DECREDITON_DMG
    else:
        MANIFEST = MANIFEST_DECRED
        MANIFEST_ASC = MANIFEST_ASC_DECRED
        DECRED_PKG = DECRED_DARWIN
elif platform.system() == 'Linux':
    if os.uname()[4].startswith("arm"):
        if int(MODE) == 0:
            print("ERROR: There is no Decrediton for ARM platform.")
            exit(8)
        else:
            MANIFEST = MANIFEST_DECRED
            MANIFEST_ASC = MANIFEST_ASC_DECRED
            if platform.architecture()[0] == '64bit':
                DECRED_PKG = DECRED_PKG_ARM64
            else:
                DECRED_PKG = DECRED_PKG_ARM
    else:
        if int(MODE) == 0:
            MANIFEST = MANIFEST_DECREDITON
            MANIFEST_ASC = MANIFEST_ASC_DECREDITON
            DECRED_PKG = DECREDITON_TAR
        else:
            MANIFEST = MANIFEST_DECRED
            MANIFEST_ASC = MANIFEST_ASC_DECRED
            if platform.architecture()[0] == '64bit':
                DECRED_PKG = DECRED_PKG_AMD64
            else:
                DECRED_PKG = DECRED_PKG_386
elif platform.system() == 'Windows':
    if int(MODE) == 0:
        MANIFEST = MANIFEST_DECREDITON
        MANIFEST_ASC = MANIFEST_ASC_DECREDITON
        DECRED_PKG = DECREDITON_EXE
    else:
        MANIFEST = MANIFEST_DECRED
        MANIFEST_ASC = MANIFEST_ASC_DECRED
        if platform.architecture()[0] == '64bit':
            DECRED_PKG = DECRED_EXE_AMD64
        else:
            DECRED_PKG = DECRED_EXE_386
else:
    print("ERROR: Unknown platform.")
    exit(9)

gpg = gnupg.GPG()
dcr_key = gpg.list_keys(keys=DCR_KEY_LONG_FP)
if dcr_key:
    print("Decred developers' public key", DCR_KEY, "has already been imported.")
else:
    print("Importing Decred developers' key", DCR_KEY, "from", DCR_KEYHOST)
    gpg_out = gpg.recv_keys(DCR_KEYHOST, DCR_KEY_LONG_FP)
    if not gpg_out.results[0]['ok']:
        print("[ FAIL ] GPG failed to import key", DCR_KEY, "from", DCR_KEYHOST + ". It may have timed out.")
        print("Make sure you can send traffic using port TCP 11371.")
        exit(2)

response = urllib.request.urlopen(DCR_GITHUB_TAG + "/v" + VERSION, context=client_context)
if response.code == 404:
    print("ERROR: could not find release version", VERSION, "on Github.")
    exit(8)

if not Path(MANIFEST).is_file() and not Path(MANIFEST_ASC).is_file() and not Path(DECRED_PKG).is_file():
    print("Downloading files from Github...")
    if not Path(MANIFEST).is_file():
        print("-", MANIFEST)
        with urllib.request.urlopen(DCR_GITHUB + "/v" + VERSION + "/" + MANIFEST, context=client_context) \
                as response, open(MANIFEST, 'wb') as out_file:
            shutil.copyfileobj(response, out_file)
    if not Path(MANIFEST_ASC).is_file():
        print("-", MANIFEST_ASC)
        with urllib.request.urlopen(DCR_GITHUB + "/v" + VERSION + "/" + MANIFEST_ASC, context=client_context) \
                as response, open(MANIFEST_ASC, 'wb') as out_file:
            shutil.copyfileobj(response, out_file)
    if not Path(DECRED_PKG).is_file():
        print("-", DECRED_PKG)
        with urllib.request.urlopen(DCR_GITHUB + "/v" + VERSION + "/" + DECRED_PKG, context=client_context) \
                as response, open(DECRED_PKG, 'wb') as out_file:
            shutil.copyfileobj(response, out_file)
else:
    print("All files have already been copied locally.")
    print("- ", MANIFEST)
    print("- ", MANIFEST_ASC)
    print("- ", DECRED_PKG)


print("\nVerifying digital signature and hash...\n")
v = open(MANIFEST_ASC, "rb")
verify_result = gpg.verify_file(v, MANIFEST)
v.close()

if verify_result.status == "signature valid":
    hasher = hashlib.sha256()
    with open(DECRED_PKG, 'rb') as hf:
        buf = hf.read(HASH_BLOCKSIZE)
        while len(buf) > 0:
            hasher.update(buf)
            buf = hf.read(HASH_BLOCKSIZE)
    DIGEST = hasher.hexdigest()

    hash_found = False
    file = open(MANIFEST, "r")
    for line in file:
        if re.search(DIGEST, line):
            hash_found = True
    file.close()
    if hash_found:
        print("[ SUCCESS ] GPG signature for", MANIFEST, "successfully verified.")
        print("[ SUCCESS ] SHA-256 hash of", DECRED_PKG, "successfully verified.\n")
        # The script won't delete files.
        print("It's now safe to delete files", MANIFEST, "and", MANIFEST_ASC, "\n")
        print("RESULT: SUCCESS. It means that", DECRED_PKG, "wasn't modified after its creation")
        print("and that the file that contains the proof was signed by Decred developers' private key.")
        print("Learn more at https://stakey.club/en/verifying-digital-signatures/\n")
        # The script won't install or run the wallet.
        if platform.system() == 'Darwin' and MODE == 0:
            print("To install, open", DECRED_PKG, "and move Decrediton.app to the Applications folder.")
        elif platform.system() == 'Windows' and MODE == 0:
            print("To install, run", DECRED_PKG)
        else:
            print("To install, unpack", DECRED_PKG + ", enter the directory and run the executable.")

    else:
        print("[ SUCCESS ] GPG signature for", DECRED_PKG, "successfully verified.")
        print("[ FAIL ] SHA-256 hash of", DECRED_PKG, "verification FAILED!\n")
        print("RESULT: FAIL. It means that the manifest wasn't modified after its creation")
        print("but", DECRED_PKG, "is incomplete or was probably modified after its creation.")
        exit(3)

else:
    print("[ FAIL ] GPG did not find a \"good signature\" for", MANIFEST + ". Verification FAILED!")
    exit(4)
```
Articulo original: https://stakey.club/en/decred-verifier/
