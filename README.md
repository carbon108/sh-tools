# Shell Tools

Shell tools for automating Docker image builds

## Installation

Add script `get-sh-tools` with the following contents to your image build: 

```shell script
#!/bin/bash

SHTOOLS_VERSION=${1}
SHTOOLS_DIR=${2}

[[ -z $SHTOOLS_VERSION || -z $SHTOOLS_DIR ]] &&\
    printf "\nsh-tools verson and/or directory not specified. Exiting...\n" &&\
    exit 1

curl -L# https://raw.githubusercontent.com/carbon108/sh-tools/${SHTOOLS_VERSION}/install-sh-tools \
        --output /tmp/install-sh-tools &&\
    chmod +x /tmp/install-sh-tools &&\
    /tmp/install-sh-tools "$SHTOOLS_VERSION" "$SHTOOLS_DIR"  &&\
    rm /tmp/install-sh-tools
```

Set environment variables `SHTOOLS_VERSION` and `SHTOOLS_DIR`
in `Dockerfile`. Add `SHTOOLS_DIR` to your `PATH`:

```dockerfile
ENV BIN_DIR /opt

ENV SHTOOLS_VERSION 0.X.Y
ENV SHTOOLS_DIR $BIN_DIR/sh-tools
ENV PATH "$SHTOOLS_DIR:$BIN_DIR:$PATH"

COPY get-sh-tools $SHTOOLS_DIR/
RUN chmod +x $SHTOOLS_DIR/get-sh-tools
```
 
Run `get-sh-tools` to download and install `sh-tools` scripts:

```dockerfile
RUN get-sh-tools "$SHTOOLS_VERSION" "$SHTOOLS_DIR"
```

Call required scripts in Dockerfile or in running container:

```dockerfile
RUN curl-tini $BIN_DIR 
```


## Usage


### `curl-cvetest`

Get CVE testing tools from `carbon108/cve-test` 

```dockerfile
RUN curl-cvetest ${download_dir} CVE_type_1 CVE_type_2...
```   
Supported CVE types: `linux`, `python` 

#### Environment

`${CVETEST_VERSION}` release version if not set defaults to `v0.1.XX`
    
#### Example 

```dockerfile
ENV CVETEST_VERSION '0.1.1'
RUN curl-cvetest /opt linux python
```
creates two executables`test-linux-cve` and `test-python-cve` in `/opt` directory  


### `curl-exists`

Return `true` if URL response header reports no errors, returns `false` otherwise 

```shell script
curl-exists ${url}
```

#### Example

```shell script
if curl-exists "https://google.com/"
then
    echo "google.com is up and running"
else
    echo "google.com is down"
fi
```


### `curl-github`

Extract file(s) from GitHub release archive into a flat directory:

```dockerfile
RUN curl-github ${repo_path} ${release_tag} ${download_dir} file_1 file_2...
```
File attributes are preserved. Zip-file archive paths are ignored in output.

`curl-github` is a convenience script that can be used for building 
new `curl-{REPO_NAME}` tools like `curl-cvetest` in this collection
     
#### Example

Extract two files `curl-exists` and `test_curl-exists` from `sh-tools` 
release `0.1.4` into `/opt` directory

```dockerfile
RUN curl-github carbon108/sh-tools 0.1.9 /opt curl-exists tests/test_curl-exists 
``` 

 
### `curl-microscanner`

Get Aquasec Microscanner executable into destination directory `${download_dir}`

```dockerfile
RUN curl-microscanner ${download_dir}
```    


### `curl-tini`

Get `krallin/tini` executable from GitHub into destination directory `${download_dir}`

```dockerfile
RUN curl-tini ${download_dir}
```    

#### Environment

`${TINI_VERSION}` the archive release version. If not set defaults to `v0.18.XX`
    
#### Example 

```dockerfile
ENV TINI_VERSION 'v0.18.0'
RUN curl-tini /opt
```
creates `/opt/tini` executable       

### `py-install-dev`

Install Python packages for development environment

```dockerfile
RUN py-install-dev PACKAGE_1  PACKAGE_2==VERSION_2 ...
```

### `py-install-dev`

Install Python packages for production environment

```dockerfile
RUN py-install-prod PACKAGE_1  PACKAGE_2==VERSION_2 ...
```
