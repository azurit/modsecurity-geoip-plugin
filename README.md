# OWASP ModSecurity Core Rule Set - GeoIP Plugin

## Description

This is a plugin that brings GeoIP support to CRS.

Plugin is setting country code into variable `tx.geoip-plugin_country_code`
which can be used by other plugins or rules. Plugin is able to use ModSecurity
build-id GeoIP support or another source of GeoIP data accessible to ModSecurity
via environment variables or HTTP headers.

## Prerequisities

 * ModSecurity build-id GeoIP support using [SecGeoLookupDb](https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual-(v2.x)#user-content-SecGeoLookupDb) directive

OR
 * another source of GeoIP data (for example Apache HTTP Server `mod_maxminddb` module)

## Plugin installation

For full and up to date instructions for the different available plugin
installation methods, refer to [How to Install a Plugin](https://coreruleset.org/docs/concepts/plugins/#how-to-install-a-plugin)
in the official CRS documentation.

## Configuration

All settings can be done in file `plugins/geoip-config.conf`.

### tx.geoip-plugin_custom_lookup

This setting can be used to disable ModSecurity build-id GeoIP lookups and use
externally provided GeoIP data (for example mod_geoip2 / mod_maxminddb). See
setting `tx.geoip-plugin_country_code` below.

Values:
 * 0 - disable custom GeoIP lookups and use ModSecurity build-id GeoIP lookups
 * 1 - enable custom GeoIP lookups

Default value: 0

### tx.geoip-plugin_country_code

Variable which holds GeoIP country code. Default value is suitable for Apache
HTTP Server `mod_maxminddb` module.

Default: %{env.geoip_country_code}

## Usage

Here is a simple blocking rule based on GeoIP country code:

```
SecRule TX:GEOIP-PLUGIN_COUNTRY_CODE "@pm RU BY IR" \
    "id:999,\
    phase:1,\
    t:none,\
    msg:'GeoIP blocking',\
    logdata:'GeoIP country: %{tx.geoip-plugin_country_code}',\
    deny"
```

## Testing

After configuration, plugin should be tested, for example, using:  
...

## License

Copyright (c) 2023 OWASP ModSecurity Core Rule Set project. All rights reserved.

The OWASP ModSecurity Core Rule Set and its official plugins are distributed
under Apache Software License (ASL) version 2. Please see the enclosed LICENSE
file for full details.
