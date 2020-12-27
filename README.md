# OpenMage Docker Setup

## Supported Frameworks

| Framework | Source |
| ----------- | ----------- |
| Magento 1 | [Source](https://github.com/OpenMage/magento-lts) |
| Magento 2  | [Source](https://github.com/magento/magento2/) | 
| Sylius  | [Source](https://github.com/Sylius/Sylius) | 
| Sulu CMS  | [Source](https://github.com/sulu/sulu) |    

### Sample Configs

This Docker Container works with the following nginx config in k8s setup.

Sample Config for Magento 2:
```
config:
    nginx: |-
      upstream fastcgi_backend {
        server  127.0.0.1:9000;
        keepalive 8;
      }

      map $http_host $magento2_mode {
       default production;
      }

      map $http_host $magento2_run_code {
       default admin;
      }

      # LOCAL AND TEMPORARY IP WHITELIST START
      # insert temp ips here
      geo $http_x_real_ip $is_ip_first_allowed {
        default        0;
      # #ip reason
      # 1.2.3.4  1;
      }
      # LOCAL AND TEMPORARY IP WHITELIST END
        
      # IP WHITELIST AND USER AGENT WHITELIST CONFIG START
      include geo.d/*.conf;
      include map.d/*.conf;
      # IP WHITELIST AND USER AGENT WHITELIST CONFIG END

      ## different stores
      server {
        # PARAM AND BASIC AUTH CONFIG START
        include access.d/*.conf;
        # PARAM AND BASIC AUTH CONFIG END

        # DEBUG START
        # comment this in if you need access vars in header
        # include debug.d/*.conf;
        # DEBUG END

        set $MAGE_ROOT /var/www/html;
        set $MAGE_MODE $magento2_mode;
        set $MAGE_RUN_CODE admin;
        listen 80;
        server_name ~^__PROJECT_DOMAIN__(.*\.__DOMAIN__\.__TLD__)?$;
        root $MAGE_ROOT/pub;
        include /etc/nginx/commons.d/magento2.conf;
      }
```
