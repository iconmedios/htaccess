- https://developer.mozilla.org/en-US/docs/Learn/Server-side/Apache_Configuration_htaccess

- https://www.webjrcode.com/trucos-consejos-htaccess/

## Cambiar página establecida por defecto
DirectoryIndex inicio.html

## Forzar a www.
RewriteEngine on
RewriteCond %{HTTP_HOST} ^midominio\.com [NC]
RewriteRule ^(.*)$ http://www.midominio.com/$1 [L,R=301,NC]


## Forzar urls quitar el www
RewriteEngine on
RewriteCond %{HTTP_HOST} ^www\.midominio\.com [NC]
RewriteRule ^(.*)$ http://midominio.com/$1 [L,R=301]

## Denegar acceso a archivos comprometidos
Puede que tengamos archivos delicados que guarden contraseñas o manejen directrices importantes de la web como .htaccess o .htpasswd. Si queremos que no caiga en malas manos ponemos el siguiente código:

'''
<FilesMatch $^*(.)^ini|log|htpasswd|htaccess>
    Order allow,deny
    Deny from all
    Satisfy All
</FilesMatch>
'''



# Caching einrichten FileETag MTime Size
ExpiresActive On
ExpiresByType text/css "access plus 1 weeks"
ExpiresByType application/javascript "access plus 1 weeks"
ExpiresByType application/x-javascript "access plus 1 weeks"
ExpiresByType image/gif "access plus 1 months"
ExpiresByType image/jpeg "access plus 1 months"
ExpiresByType image/png "access plus 1 months"
ExpiresByType image/x-icon "access plus 1 months"

# BEGIN Expire headers
<ifModule mod_expires.c>
  ExpiresActive On
  ExpiresDefault "access plus 5 seconds"
  ExpiresByType image/x-icon "access plus 12 months"
  ExpiresByType image/jpeg "access plus 2592000 seconds"
  ExpiresByType image/png "access plus 2592000 seconds"
  ExpiresByType image/gif "access plus 2592000 seconds"
  ExpiresByType application/x-shockwave-flash "access plus 2592000 seconds"
  ExpiresByType text/css "access plus 604800 seconds"
  ExpiresByType text/javascript "access plus 216000 seconds"
  ExpiresByType application/javascript "access plus 216000 seconds"
  ExpiresByType application/x-javascript "access plus 216000 seconds"
  ExpiresByType text/html "access plus 600 seconds"
  ExpiresByType application/xhtml+xml "access plus 600 seconds"
</ifModule>
# END Expire headers

# BEGIN Cache-Control Headers
<ifModule mod_headers.c>
  <filesMatch "\.(ico|jpe?g|png|gif|swf)$">
    Header set Cache-Control "public"
  </filesMatch>
  <filesMatch "\.(css)$">
    Header set Cache-Control "public"
  </filesMatch>
  <filesMatch "\.(js)$">
    Header set Cache-Control "private"
  </filesMatch>
  <filesMatch "\.(x?html?|php)$">
    Header set Cache-Control "private, must-revalidate"
  </filesMatch>
</ifModule>
# END Cache-Control Headers


# Habilitar compresión deflate o Gzip

# Compresión deflate por tipo de archivo
<IfModule mod_deflate.c>
 AddOutputFilterByType DEFLATE text/plain
 AddOutputFilterByType DEFLATE text/html
 AddOutputFilterByType DEFLATE text/xml
 AddOutputFilterByType DEFLATE text/css
 AddOutputFilterByType DEFLATE text/javascript
 AddOutputFilterByType DEFLATE application/xml
 AddOutputFilterByType DEFLATE application/xhtml+xml
 AddOutputFilterByType DEFLATE application/rss+xml
 AddOutputFilterByType DEFLATE application/atom_xml
 AddOutputFilterByType DEFLATE application/javascript
 AddOutputFilterByType DEFLATE application/x-javascript
 AddOutputFilterByType DEFLATE application/x-shockwave-flash
</IfModule>

#Compresión por Gzip
<IfModule mod_gzip.c>
 mod_gzip_on Yes
 mod_gzip_dechunk Yes
 mod_gzip_item_include file\.(html?|txt|css|js|php|pl)$
 mod_gzip_item_include handler^cgi-script$
 mod_gzip_item_include mime^text/.*
 mod_gzip_item_include mime^application/x-javascript.*
 mod_gzip_item_exclude mime^image/.*
 mod_gzip_item_exclude rspheader^Content-Encoding:.*gzip.*
</IfModule>