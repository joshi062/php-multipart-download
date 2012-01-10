#Multi-Part PHP Download Script

This script provides a download path for files that are not within the web root. For example, you could
host files from /w/media when your web root is /w/web, therefor giving you more control over your media
file downloads (security, statistics, etc.). It supports multi-part downloads.

##FEATURES

- Multipart download support (allows multi-threaded client downloads). 
- Download resume after failure.
- Very large file support. Files are split into chunks for download, limiting memory usage.
- Key based download. Hides file path information from browser. 

##DEPENDENCIES

- Apache setup and configured with PHP and .htaccess support.
- Apache must support URL rewrites.
- $content_base and $default_content must be included in PHP open_base() path.
- PHP must be able to run code via exec() [PHP filesize() int overflow workaround]
- The 'file' command must be installed (on most Linux systems by default) [mime type detection]

##USAGE

This script tricks the web browser/downloader into thinking that the file being downloaded
is a simple directory based download. Here's how it works.

1. Define your $content_base. If the file you want to download is in /w/media/file.mp4, then your 
$content_base would be /w/media/.

2. Setup your download directory. Drop the entire download directory into your web root. For example, if
your web root is /w/web then your download directory could be /w/web/download. So, now you can call the 
download script by pointing your browser at http://{hostname}/download.

3. Create a download key. Within the page you are going to link from, take the full path of the file you
want to download, remove the $content_base (i.e. /w/media/file.mp4 becomes file.mp4) and encode the string 
in PHP with base64_encode($string). This is your download key. 

4. Create a link to the download script. You can down combine everything into a working download link. 
It should be in the following format. You can change this by modifying the rewrite rule in the .htaccess
file.
`````````````````````````````````````````````````````````````````````````````
http://{hostname}/download/key/{download_key}/name/{file_name}.{file_extension}
`````````````````````````````````````````````````````````````````````````````

5. Display the link to a user, pass it to a media player, or do whatever else you need to do. 