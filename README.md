# Shell Tools

Shell tools for automaiting Docker builds

## Installation

Get resource file `sh-tools-rc`:

    sh_tools_version=0.1.1
    download_dir=/opt/carbon108
    url=https://github.com/carbon108/sh-tools-docker/archive/${sh_tools_version}.zip
    zip_file=/tmp/carbon108_sh-tools-${sh_tools_version}.zip
    
    curl -Lk $url --output $zip_file 
    mkdir -p "$download_dir"
    unzip -j -o -q $zip_file "sh-tools-docker-${sh_tools_version}/sh-tools-rc" -d "${download_dir}"
    rm $zip_file

Source it:

    source $download_dir/sh-tools-rc

## Usage

### curl_github

Extract specified file(s) from GitHub repo:

    curl_github ${repo_path} ${release_tag} $download_dir ${file_list}
     
For example 
 
    curl_github krallin/tini v0.18.0 /tmp/tini "ddist.sh src/tini.c" 
 
downloads two files `ddist.sh` and `tini.c` from `https://github.com/krallin/tini` repository into `/tmp/tini` 
directory
 