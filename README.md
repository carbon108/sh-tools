# Shell Tools

Shell tools for automaiting Docker image builds

## Installation

Get and run installer `install-sh-tools-rc`:

    curl -Lk $INSTALL_SH_TOOLS_URL --output /tmp/install-sh-tools \
        && chmod +x /tmp/install-sh-tools \
        && /tmp/install-sh-tools $SHTOOLS_VERSION "$SCRIPTS_DIR" \
        && rm /tmp/install-sh-tools      
  
Source it in Dockerfile:

    +++ GOOD: see https://stackoverflow.com/questions/20635472/using-the-run-instruction-in-a-dockerfile-with-source-does-not-work
    --- BAD: source "$scripts_dir/sh-tools-rc"

## Usage

### curl_exists

Return `0` if URL response header reports no errors, returns `1` otherwise 

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