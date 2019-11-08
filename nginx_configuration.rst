.. code-block:: nginx

    server {
        listen 80 default_server;
        listen [::]:80 default_server;

        return 301 https://$host$request_uri;
    }

    server {
        listen 443 ssl http2;
        listen [::]:443 ssl http2;

        server_name plone-saml-demo.office.quintagroup.com;

        ssl_certificate     /etc/pki/tls/certs/plone-saml-demo.office.quintagroup.com.crt;
        ssl_certificate_key /etc/pki/tls/private/plone-saml-demo.office.quintagroup.com.key;

        access_log /var/log/nginx/plone-saml-demo.log;
        error_log  /var/log/nginx/plone-saml-demo_error.log;

        location / {

            proxy_pass http://127.0.0.1:8080/VirtualHostBase/https/$host:443/auth0demo/VirtualHostRoot/$request_uri;
            proxy_set_header   Host            $host;
            proxy_set_header   X-Real-IP       $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
