# Harbor on Labs

## Pre-requisitos

- docker 17.10 ou superior
- docker-compose instalado
- openssl


## 1. Configurações iniciais



Faça o Download do Harbor:

```
$ wget https://github.com/goharbor/harbor/releases/download/v2.8.0/harbor-online-installer-v2.8.0.tgz

```

Descompacte o arquivo:

```
$ tar xzvf harbor-online-installer-v2.8.0.tgz
```

Gere os certificados:

```
$ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout registry.gpxlnx.me.key -out registry.gpxlnx.me.crt -subj "/CN=registry/O=4Labs/OU=DevOps" -addext "subjectAltName = DNS:registry.gpxlnx.me"

```

Gere um PEM do certificado:

```
$ openssl x509 -inform PEM -in registry.gpxlnx.me.crt -out registry.gpxlnx.me.cert
```

Crie o diretório que vai armezenar o certificado

```
$ sudo mkdir -p /data/certs

```

Realize a cópia dos arquivos gerados:

```
$ sudo cp registry.gpxlnx.me.* /data/certs
```

Crie o diretório para seu Registry no Docker:

```
$ sudo mkdir -p /etc/docker/certs.d/registry.gpxlnx.me/

```

Copie os arquivos gerados para o o diretório criado:

```
sudo cp registry.gpxlnx.me.cert /etc/docker/certs.d/registry.gpxlnx.me/
sudo cp registry.gpxlnx.me.key /etc/docker/certs.d/registry.gpxlnx.me/
sudo cp registry.gpxlnx.me.crt /etc/docker/certs.d/registry.gpxlnx.me/ca.crt
```

## 2. Inicializando o Harbor

Realize o clone do repositório harbor-labs e copie o arquivo harbor.yml para o diretório do harbor

```
git clone https://github.com/gpxlnx/harbor-labs.git

Ou

copiar o arquivo de exemplo harbor.yml.tmpl e editar os itens desejados
```

```
cp harbor-labs/harbor/harbor.yml ~/harbor

```

Execute os scrips abaixo e aguarde o Harbor inicializar:

```
cd ~/harbor
```
Prepare o ambiente:
```
sudo ./prepare
```
Execute a criação do ambiente com o examinador Trivy:

```
sudo ./install.sh --with-trivy --with-notary --with-chartmuseum
```

Mapeie no seu /etc/hosts a seguinte linha:

```
172.16.0.103 registry registry.gpxlnx.me
```

Por fim, acesse o Harbor pelo navegador:

https://registry.gpxlnx.me

usuário: admin

senha: teste123@
