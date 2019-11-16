# Shell Tools

Shell tools for automaiting Docker builds

## Installation

Get resource file `sh-tools-rc`:

    sh_tools_version=0.1.1
    download_dir=/opt/carbon108
    archive_url=https://github.com/carbon108/sh-tools-docker/archive/${sh_tools_version}.zip
    zip_file=/tmp/carbon108_sh-tools-${sh_tools_version}.zip
    
    curl -Lk $archive_url --output $zip_file 
    mkdir -p "$download_dir"
    unzip -j -o -q $zip_file "sh-tools-docker-${sh_tools_version}/sh-tools-rc" -d "${download_dir}"
    rm $zip_file

Source it:

    source $download_dir/sh-tools-rc

## Usage

### curl_github

Extract file(s) from specified release sources archive at GitHub repository:

    curl_github "${repo_path}" "${release_tag}" "$download_dir" file_1 file_2...

File attributes are preserved.
     
For example 
 
    curl_github krallin/tini v0.18.0 /opt/tini ddist.sh src/tini.c 
 
extracts two files `ddist.sh` and `tini.c` from repository release 
`https://github.com/krallin/tini/archive/v0.18.0.zip` into `/opt/tini` directory
 