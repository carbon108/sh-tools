# Shell Tools

Shell tools for automating Docker image builds

## Installation

Follow instructions below in your Dockerfile.

Set environment variables:

    ENV SHTOOLS_VERSION 0.1.3
    ENV SCRIPTS_DIR /opt/carbon108    

Get and run installer `install-sh-tools-rc`:

    RUN curl -Lk https://github.com/carbon108/sh-tools-docker/releases/download/get/install-sh-tools \
        --output /tmp/install-sh-tools && chmod +x /tmp/install-sh-tools \
        && /tmp/install-sh-tools "$SHTOOLS_VERSION" "$SCRIPTS_DIR" && rm /tmp/install-sh-tools     

Add `sh-tools` directory to PATH:

    ENV PATH "$SCRIPTS_DIR:$PATH"  

Call `sh-tools` scripts in Dockerfile or in running container:

    RUN curl_tini ${TINI_VERSION} /opt  

More info on shell in Docker: https://github.com/moby/moby/issues/7281#issuecomment-389440503

## Usage

### curl_exists

Return `true` if URL response header reports no errors, returns `false` otherwise 

    curl_exists ${url}

Page content is not retrieved

Example:

    if curl_exists "https://google.com/"
    then
        echo "google.com is up and running"
    else
        echo "google.com is down"
    fi

### curl_github

Extract file(s) from GitHub release archive:

    curl_github "${repo_path}" "${release_tag}" "$download_dir" file_1 file_2...

File attributes are preserved, archive paths ignored in output.
     
Example: 
 
    curl_github krallin/tini v0.18.0 /opt/tini ddist.sh ci/run_build.sh 
 
extracts two files `ddist.sh` and `run_build.sh` from repository release 
`https://github.com/krallin/tini/archive/v0.18.0.zip` into `/opt/tini` directory
 
### curl_tini

Get `tini` executable into destination directory
    
    curl_tini ${tini_path}
    
`${TINI_VERSION}` should be specified in environment, default version is used otherwise
    
Example: 

    curl_tini ./bin
    
creates `./bin/tini` binary file from `https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini`       