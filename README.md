# Shell Tools

Shell tools for automating Docker image builds

## Installation

Follow instructions below in your Dockerfile.

Set environment variables:

```dockerfile
ENV SHTOOLS_VERSION 0.1.3
ENV SHTOOLS_DIR /opt/carbon108   
```

Get and run installer `install-sh-tools`:

```dockerfile
RUN curl -Lk https://github.com/carbon108/sh-tools/releases/download/get/install-sh-tools \
    --output /tmp/install-sh-tools && chmod +x /tmp/install-sh-tools \
    && /tmp/install-sh-tools "$SHTOOLS_VERSION" "$SHTOOLS_DIR" && rm /tmp/install-sh-tools     
```

Add `sh-tools` directory to PATH:

```dockerfile
ENV PATH "$SHTOOLS_DIR:$PATH"  
```

Call `sh-tools` scripts in Dockerfile or in running container:

```dockerfile
RUN curl-tini /opt 
```

## Usage

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
RUN curl-github "${repo_path}" "${release_tag}" "$download_dir" file_1 file_2...
```
File attributes are preserved. Zip-file archive paths are ignored in output.
     
#### Example

Extract two files `curl-exists` and `test_curl-exists` from `sh-tools` 
release `0.1.4` into `/opt` directory

```dockerfile
RUN curl-github carbon108/sh-tools 0.1.4 /opt curl-exists tests/test_curl-exists 
``` 
 
### `curl-tini`

Get `krallin/tini` executable from GitHub into destination directory `${tini_dir}`

```dockerfile
RUN curl-tini ${tini_dir}
```    

#### Environment

`${TINI_VERSION}` the archive release version. If not set defaults to `v0.18.XX`
    
#### Example 

```dockerfile
ENV TINI_VERSION 'v0.18.0'
RUN curl-tini /opt
```
creates `/opt/tini` executable       

