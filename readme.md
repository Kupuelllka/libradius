# Radius library

a Go (golang) [RADIUS](https://tools.ietf.org/html/rfc2865) client and server implementation

[![Go Reference](https://pkg.go.dev/badge/libradius/radius.svg)](https://pkg.go.dev/libradius/radius)

## Installation

    go get -u libradius/radius

## Client example

```go
package main

import (
	"context"
	"log"

	"libradius/radius"
	"libradius/radius/rfc2865"
)

func main() {
	packet := radius.New(radius.CodeAccessRequest, []byte(`secret`))
	rfc2865.UserName_SetString(packet, "tim")
	rfc2865.UserPassword_SetString(packet, "12345")
	response, err := radius.Exchange(context.Background(), packet, "localhost:1812")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Code:", response.Code)
}
```

## Server example

```go
package main

import (
	"log"

	"libradius/radius"
	"libradius/radius/rfc2865"
)

func main() {
	handler := func(w radius.ResponseWriter, r *radius.Request) {
		username := rfc2865.UserName_GetString(r.Packet)
		password := rfc2865.UserPassword_GetString(r.Packet)

		var code radius.Code
		if username == "tim" && password == "12345" {
			code = radius.CodeAccessAccept
		} else {
			code = radius.CodeAccessReject
		}
		log.Printf("Writing %v to %v", code, r.RemoteAddr)
		w.Write(r.Response(code))
	}

	server := radius.PacketServer{
		Handler:      radius.HandlerFunc(handler),
		SecretSource: radius.StaticSecretSource([]byte(`secret`)),
	}

	log.Printf("Starting server on :1812")
	if err := server.ListenAndServe(); err != nil {
		log.Fatal(err)
	}
}
```
Библиотека для работы с протоколом радиус
- **Список поддерживаемых RFC спецификаций:**
    - [x] rfc2759
    - [x] rfc2865
    - [x] rfc2866
    - [x] rfc2867
    - [x] rfc2867
    - [x] rfc2868
    - [x] rfc2869
    - [x] rfc3079
    - [x] rfc3162
    - [x] rfc3576
    - [x] rfc3850
    - [x] rfc4072
    - [x] rfc4372
    - [x] rfc4603
    - [x] rfc4675
    - [x] rfc4679
    - [x] rfc4818
    - [x] rfc5090
    - [x] rfc5176
    - [x] rfc5447
    - [x] rfc5580
    - [x] rfc5904
    - [x] rfc6519
    - [x] rfc6572
    - [x] rfc6677
    - [x] rfc6911
    - [x] rfc6929
    - [x] rfc7055
    - [x] rfc7268
- **Список поддерживаемых VENDOR спецификаций:**
    - [x]3com
    - [x]3gpp
    - [x]acc
    - [x]acme
    - [x]airespace
    - [x]alcatel
    - [x]alteon
    - [x]altiga
    - [x]alvarion
    - [x]apc
    - [x]aptis
    - [x]aruba
    - [x]ascend
    - [x]asn
    - [x]audiocodes
    - [x]azaira
    - [x]bay
    - [x]bintec
    - [x]bristol
    - [x]cablelabs
    - [x]chillispot
    - [x]cisco
    - [x]clavister
    - [x]colubris
    - [x]columbia
    - [x]compat
    - [x]cosine
    - [x]dhcp
    - [x]digium
    - [x]eltex
    - [x]eepygi
    - [x]extreme
    - [x]fortinet
    - [x]foundry
    - [x]freeradius
    - [x]freeswitch
    - [x]gandalf
    - [x]garderos
    - [x]gemtek
    - [x]h3c
    - [x]huawei
    - [x]iea
    - [x]infonet
    - [x]inpuplugged
    - [x]issanni
    - [x]itk
    - [x]jradius
    - [x]juniper
    - [x]karlnet
    - [x]lancom
    - [x]livingston
    - [x]localweb
    - [x]lucent
    - [x]manzara
    - [x]merit
    - [x]microsoft
    - [x]mikrotik
    - [x]motorola
    - [x]navini
    - [x]netscreen
    - [x]networkphysics
    - [x]nexans
    - [x]nokia
    - [x]nomadix
    - [x]nortel
    - [x]ntua
    - [x]openser
    - [x]packeteer
    - [x]patton
    - [x]propel
    - [x]prosoft
    - [x]quiconnect
    - [x]redback
    - [x]redcreek
    - [x]riverstone
    - [x]roaringpenguin
    - [x]shasta
    - [x]shiva
    - [x]slipstream
    - [x]sonicwall
    - [x]springtide
    - [x]starent
    - [x]system_nova
    - [x]telebit
    - [x]telkom
    - [x]tropos
    - [x]ukerna
    - [x]unix
    - [x]usr
    - [x]ustarcom
    - [x]valemount
    - [x]versanet
    - [x]vqp
    - [x]walabi
    - [x]waverider
    - [x]wichorus
    - [x]wimax
    - [x]wispr
    - [x]xedia
    - [x]zyxel
