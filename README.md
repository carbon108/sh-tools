# Shell Tools

Shell tools for automating Docker image builds

## Installation

Follow instructions below in your Dockerfile.

Set environment variables:

```dockerfile
ENV SHTOOLS_VERSION 0.1.3
ENV SCRIPTS_DIR /opt/carbon108   
```

Get and run installer `install-sh-tools-rc`:

```dockerfile
RUN curl -Lk https://github.com/carbon108/sh-tools/releases/download/get/install-sh-tools \
    --output /tmp/install-sh-tools && chmod +x /tmp/install-sh-tools \
    && /tmp/install-sh-tools "$SHTOOLS_VERSION" "$SCRIPTS_DIR" && rm /tmp/install-sh-tools     
```

Add `sh-tools` directory to PATH:

```dockerfile
ENV PATH "$SCRIPTS_DIR:$PATH"  
```

Call `sh-tools` scripts in Dockerfile or in running container:

```dockerfile
RUN curl-tini ${TINI_VERSION} /opt 
```

## Usage

### `curl-exists`

Return `true` if URL response header reports no errors, returns `false` otherwise 

```shell script
curl-exists ${url}
```

Page content is not retrieved

Example:

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
RUN curl-github "${repo_path}" "${release_tag}" "$download_dir" file_1 file_2...
```
File attributes are preserved, archive paths ignored in output.
     
Example: 

```dockerfile
RUN curl-github krallin/tini v0.18.0 /opt/tini ddist.sh ci/run_build.sh 
``` 
extracts two files `ddist.sh` and `run_build.sh` from repository release 
`https://github.com/krallin/tini/archive/v0.18.0.zip` into `/opt/tini` directory
 
### `curl-tini`

Get `tini` executable from GitHub into destination directory

```dockerfile
RUN curl-tini ${tini_path}
```    
`${TINI_VERSION}` should be specified in environment, default version is used otherwise
    
Example: 

```dockerfile
RUN curl-tini /opt/bin
```
extracts `tini` from `https://github.com/krallin/tini` release archive to `/opt/tini` executable       
