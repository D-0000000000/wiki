For users that run qBittorrent via Microsoft IIS as a reverse proxy an extra header is needed. You must install the [URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) addon first. Reverse proxy support will be enabled when creating the first rule.

1. On the URL Rewrite Page open **View Server Variables** on the right side
2. Click **Add** and in the box that appears enter `HTTP_X-Forwarded-Host`
3. Return to the rules page
4. Open **Add Rules** and select **Reverse Proxy**
5. Enter the server IP:Port without `http://` (for example `127.0.0.1:8080`)
6. Open the new rule and change the path to a subdirectory if needed (for example **qbweb/(.*)** = http://domain.tld/qbweb/)
7. Under **Server Variables** add a rule with the following:
  
    Server variable name: **HTTP_X-Forwarded-Host**

    Value: `{HTTP_HOST}:{SERVER_PORT}`

The code is automatically created in the web.config file and looks like this (you must use the GUI first so reverse proxy support is enabled):
```xml
<rule name="Qbittorrent" stopProcessing="true">
  <match url="qbweb/(.*)" />
  <action type="Rewrite" url="http://127.0.0.1:8080/{R:1}" />
  <serverVariables>
    <set name="HTTP_X-Forwarded-Host" value="{HTTP_HOST}:{SERVER_PORT}" />
  </serverVariables>
</rule>
```

You can use HTTPS to access the URL via IIS and it will use HTTP to communicate with qBittorrent. There is no need for HTTPS on localhost. 
 
The tutorial is based on the assistance of Chocobo1 in [this thread](https://github.com/qbittorrent/qBittorrent/issues/7311).