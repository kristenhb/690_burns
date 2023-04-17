# Install and Setup a LAMP Stack

### Perform System Update

```
sudo apt update
sudo apt -y upgrade
```

### Clients, Servers, and URLs
+ Addresses on the Web are expressed with URLs - Uniform Resource Locators -
which specify a protocol (e.g. http), a servername (e.g. www.apache.org),
a URL-path (e.g. /docs/current/getting-started.html), and possibly a query 
string (e.g. ?arg=value) used to pass additional arguments to the server.

+ A client (e.g., a web browser) connects to a server (e.g., your Apache HTTP Server),
with the specified protocol, and makes a request for a resource using the URL-path.

+ The URL-path may represent any number of things on the server. It may be a file 
(like getting-started.html) a handler (like server-status) or some kind of program
file (like index.php). We'll discuss this more below in the Web Site Content section.

+ The server will send a response consisting of a status code and, optionally, 
a response body. The status code indicates whether the request was successful,
and, if not, what kind of error condition there was. This tells the client what
it should do with the response. You can read about the possible response codes 
in HTTP Server wiki.

### Hostnames and DNS
In order to connect to a server, the client will first have to resolve the servername 
to an IP address - the location on the Internet where the server resides. Thus, in
order for your web server to be reachable, it is necessary that the servername be in DNS.
(If you don't know how to do this, you'll need to contact your network administrator, or 
Internet service provider, to perform this step for you.)

Read more about the hosts file at Wikipedia.org/wiki/Hosts_(file),
and more about DNS at Wikipedia.org/wiki/Domain_Name_System.

### Configuration Files and Directives
The Apache HTTP Server is configured via simple text files. These files may be located 
any of a variety of places, depending on how exactly you installed the server. Common 
locations for these files may be found in the httpd wiki. If you installed httpd from source,
the default location of the configuration files is /usr/local/apache2/conf. The default 
configuration file is usually called httpd.conf. This, too, can vary in third-party distributions of the server.

In addition to the main configuration files, certain directives may go in 
.htaccess files located in the content directories. .htaccess files are primarily
for people who do not have access to the main server configuration file(s). You 
can read more about .htaccess files in the .htaccess howto.

### Web Site Content

Web site content can take many different forms, but may be broadly divided into static
and dynamic content.

Static content is things like HTML files, image files, CSS files, and other files that 
reside in the filesystem. The DocumentRoot directive specifies where in your filesystem 
you should place these files. This directive is either set globally, or per virtual host.
Look in your configuration file(s) to determine how this is set for your server.

Typically, a document called index.html will be served when a directory is requested without
a file name being specified. For example, if DocumentRoot is set to /var/www/html and a 
request is made for http://www.example.com/work/, the file /var/www/html/work/index.html 
will be served to the client.

Dynamic content is anything that is generated at request time, and may change from one 
request to another. There are numerous ways that dynamic content may be generated. 
Various handlers are available to generate content. CGI programs may be written to 
generate content for your site.

Log Files and Troubleshooting

As an Apache HTTP Server administrator, your most valuable assets are the log files,
and, in particular, the error log. Troubleshooting any problem without the error log
is like driving with your eyes closed.

The location of the error log is defined by the ErrorLog directive, which may be set
globally, or per virtual host. Entries in the error log tell you what went wrong, and
when. They often also tell you how to fix it. Each error log message contains an error
code, which you can search for online for even more detailed descriptions of how to address
the problem. You can also configure your error log to contain a log ID which you can then
correlate to an access log entry, so that you can determine what request caused the error
condition.

What's next?
Once you have the prerequisites under your belt, it's time to move on.

This document covers only the bare basics. We hope that this gets you started,
but there are many other things that you might need to know.

##### Download
https://httpd.apache.org/download.cgi
##### Install
https://httpd.apache.org/docs/2.4/install.html
##### Configure
https://httpd.apache.org/docs/2.4/configuring.html
##### Start
https://httpd.apache.org/docs/2.4/invoking.html
##### Frequently Asked Questions
https://cwiki.apache.org/confluence/display/HTTPD/FAQ

##### VM Instances
http://34.123.18.251/

##### OPAC
```
create user 'opacuser'@'localhost' identified by 'opac2023';
```
