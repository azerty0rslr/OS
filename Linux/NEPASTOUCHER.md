(myenv) root@vbox:~/mealie# history
    1  sudo apt install python3
    2  sudo apt install python3-pip
    3  sudo apt install python3-vnev
    4  sudo apt install python3-vnev
    5  sudo apt install python3-venv
    6  git clone https://github.com/mealie-recipies/mealie
    7  sudo apt install git-all
    8  git clone https://github.com/mealie-recipies/mealie
    9  git clone https://github.com/mealie-recipies/mealie
   10  git clone https://github.com/mealie-recipies/mealie
   11  git clone https://github.com/mealie-recipies/mealie
   12  git clone https://github.com/mealie-recipes/mealie/
   13  # Télécharger et installer nvm :
   14  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
   15  # au lieu de redémarrer le shell
   16  \. "$HOME/.nvm/nvm.sh"
   17  # Télécharger et installer Node.js :
   18  nvm install 24
   19  # Vérifiez la version de Node.js :
   20  node -v # Doit afficher "v24.11.1".
   21  # Vérifier la version de npm :
   22  npm -v # Doit afficher "11.6.2".
   23  apt install curl
   24  # Télécharger et installer nvm :
   25  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
   26  # au lieu de redémarrer le shell
   27  \. "$HOME/.nvm/nvm.sh"
   28  # Télécharger et installer Node.js :
   29  nvm install 24
   30  # Vérifiez la version de Node.js :
   31  node -v # Doit afficher "v24.11.1".
   32  # Vérifier la version de npm :
   33  npm -v # Doit afficher "11.6.2".
   34  npm install --global yarn
   35  yarn --version
   36  ls
   37  cd mealie
   38  cd frontend
   39  yarn --version
   40  yarn install --prefer-offline
   41  yarn install --frozen-lockfile 
   42      --non-interactive 
   43      --production=false 
   44      --network-timeout 1000000
   45  yarn install --frozen-lockfile 
   46  yarn install --non-interactive 
   47  yarn install --production=false
   48  yarn install --network-timeout 1000000 
   49  yarn generate
   50  ls
   51  env
   52  env
   53  export PYTHONUNBUFFERED=1
   54  export PYTHONDONTWRITEBYTECODE=1
   55  export PIP_NO_CACHE_DIR=off
   56  export PIP_DISABLE_PIP_VERSION_CHECK=on
   57  export PIP_DEFAULT_TIMEOUT=100
   58  export VENV_PATH="/opt/mealie"
   59  export PATH="$VENV_PATH/bin:$PATH"
   60  env
   61  cd ..
   62  ls
   63  unset MEALIE_HOME
   64  env
   65  pip install uv
   66  python3 pip install uv
   67  apt-get update     && apt-get install --no-install-recommends -y     curl     && rm -rf /var/lib/apt/lists/*
   68  pip install uv
   69  python3 pip install uv
   70  apt install python3-xyz
   71  apt install python3-xyz pip install uv
   72  apt install pipx
   73  pipx install uv
   74  apt install pipx
   75  pip install uv
   76  apt install python3-xyz
   77  pip install uv
   78  apt install uv
   79  apt install python3-uv
   80  python3 -m venv /mealie 
   81  source mealie/bin/activate
   82  python3 -m venv myenv
   83  source myenv/bin/activate
   84  pip install uv
   85  exit
   86  apt install pipx
   87  cd mealie
   88  apt install pipx
   89  source myenv/bin/activate
   90  copy uv.lock pyproject.toml ./
   91  cp uv.lock pyproject.toml ./
   92  ls
   93  exit
   94  cd mealie
   95  ls
   96  cp uv.lock pyproject.toml ./
   97  cp mealie ./mealie
   98  clochard
   99  cp --from=frontend-builder /frontend/dist ./mealie/frontend
  100  cp from=frontend-builder /frontend/dist ./mealie/frontend
  101  cp frontend-builder /frontend/dist ./mealie/frontend
  102  cp frontend-builder /frontend/dist /frontend
  103  ls
  104  cd mealie
  105  ls
  106  cp uv.lock pyproject.toml ./
  107  cd ../
  108  cp uv.lock mealie/uv.lock
  109  cp pyproject.toml mealie/pyproject.toml
  110  cp mealie mealie/mealie
  111  cp --from=frontend-builder /frontend/dist ./mealie/frontend
  112  cp /frontend/dist ./mealie/frontend
  113  ls
  114  cd frontend
  115  ls
  116  cd ../
  117  ls
  118  cd frontend
  119  ls
  120  cp /frontend/dist ../frontend
  121  cp dist ./frontend
  122  cp dist ./frontend
  123  cp dist ../frontend
  124  uv build --out-dir dist
  125  source myenv/bin/activate
  126  ../
  127  cd ../
  128  source myenv/bin/activate
  129  uv build --out-dir dist
  130  history
  131  uv export --no-editable --no-emit-project --extra pgsql --format requirements-txt --output-file dist/requirements.txt     && MEALIE_VERSION=$(python -c "import tomllib; print(tomllib.load(open('pyproject.toml', 'rb'))['project']['version'])")     && echo "mealie[pgsql]==${MEALIE_VERSION} \\" >> dist/requirements.txt     && pip hash dist/mealie-${MEALIE_VERSION}-py3-none-any.whl | tail -n1 | tr -d '\n' >> dist/requirements.txt     && echo " \\" >> dist/requirements.txt     && pip hash dist/mealie-${MEALIE_VERSION}.tar.gz | tail -n1 >> dist/requirements.txt
  132  ls
  133  ls dist
  134  pip install -r dist/requirements.txt
  135  pip install libxml2
  136  apt-get update     && apt-get install --no-install-recommends -y     build-essential     libpq-dev     libwebp-dev \
  137      libsasl2-dev libldap2-dev libssl-dev     gnupg gnupg2 gnupg1     && rm -rf /var/lib/apt/lists/*
  138  python3 -m venv --upgrade-deps $VENV_PATH
  139  pip install --require-hashes -r /dist/requirements.txt --find-links /dist
  140  apt-get update     && apt-get install --no-install-recommends -y     gosu     iproute2     libldap-common     libldap2     && rm -rf /var/lib/apt/lists/*
  141  mkdir -p /run/secrets
  142  pip install -r dist/requirements.txt
  143  history

