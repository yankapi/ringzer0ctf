---
Autors: Hüseyin ALTUNKAYNAK, Süleyman ERGEN
Date: 05.03.2019
License: CC BY-NC-SA
---

# JAVASCRIPT

## Client side validation is so secure?

Bu ilk seviyede javascript ile client tarafta bir doğrulama yapmamamız gerektiğini anlıyoruz. Doğrulama için kullanılan js kodu aşağıda.

```javascript
// Look's like weak JavaScript auth script :)
$(".c_submit").click(function(event) {
    event.preventDefault()
    var u = $("#cuser").val();
    var p = $("#cpass").val();
    if(u == "admin" && p == String.fromCharCode(74,97,118,97,83,99,114,105,112,116,73,115,83,101,99,117,114,101)) {
        if(document.location.href.indexOf("?p=") == -1) {
            document.location = document.location.href + "?p=" + p;
        }
    } else {
        $("#cresponse").html("<div class='alert alert-danger'>Wrong password sorry.</div>");
    }
});
```

Buradan kullanıcı isminin `admin` olduğu anlaşılıyor. Parolanın ise charcode olarak saklandığını görüyoruz. Hemen tarayıcıdan geliştirici seçeneklerinden kodu çalıştıralım.

```javascript
String.fromCharCode(74,97,118,97,83,99,114,105,112,116,73,115,83,101,99,117,114,101)
"JavaScriptIsSecure"
```

Aptık parolayıda biliyoruz. Bilgileri girdikten sonra flag şu şekilde.

```text
FLAG-66Jq5u688he0y46564481WRh
```

## Is hashing more secure?

Bu sefer doğrulama sha1 ile yapılmış. Eğer hash'i alınmış değer yani bizim parolamız çok yaygın kullanılan bir değder ise bulmamız zor olmaz.

```javascript
// Look's like weak JavaScript auth script :)
$(".c_submit").click(function(event) {
    event.preventDefault();
    var p = $("#cpass").val();
    if(Sha1.hash(p) == "b89356ff6151527e89c4f3e3d30c8e6586c63962") {
        if(document.location.href.indexOf("?p=") == -1) {   
            document.location = document.location.href + "?p=" + p;
        }
    } else {
        $("#cresponse").html("<div class='alert alert-danger'>Wrong password sorry.</div>");
    }
});
```

Bu hash değerini google'da aratın. Karşınıza hemen çıkacaktır.

```text
adminz
```

Çünkü bu değer daha önceden kırılmış ve internette paylaşılmıştır. Bu sayede şifreyi kırabildir. Fakat daha önce kırılmamış ve zor bir parola kullansaydık o zaman internette bulamazdık. İşte flag:

```text
FLAG-bXNsYg9tLCaIX6h1UiQMmMYB
```

## Then obfuscation is more secure?

Bu seviyede js kodu biraz karışık verilmiş. Bu kodu biraz düzenlememiz gerekecek.

```javascript
// Look's like weak JavaScript auth script :)
var _0xc360=["\x76\x61\x6C","\x23\x63\x70\x61\x73\x73","\x61\x6C\x6B\x33","\x30\x32\x6C\x31","\x3F\x70\x3D","\x69\x6E\x64\x65\x78\x4F\x66","\x68\x72\x65\x66","\x6C\x6F\x63\x61\x74\x69\x6F\x6E","\x3C\x64\x69\x76\x20\x63\x6C\x61\x73\x73\x3D\x27\x65\x72\x72\x6F\x72\x27\x3E\x57\x72\x6F\x6E\x67\x20\x70\x61\x73\x73\x77\x6F\x72\x64\x20\x73\x6F\x72\x72\x79\x2E\x3C\x2F\x64\x69\x76\x3E","\x68\x74\x6D\x6C","\x23\x63\x72\x65\x73\x70\x6F\x6E\x73\x65","\x63\x6C\x69\x63\x6B","\x2E\x63\x5F\x73\x75\x62\x6D\x69\x74"];$(_0xc360[12])[_0xc360[11]](function (){var _0xf382x1=$(_0xc360[1])[_0xc360[0]]();var _0xf382x2=_0xc360[2];if(_0xf382x1==_0xc360[3]+_0xf382x2){if(document[_0xc360[7]][_0xc360[6]][_0xc360[5]](_0xc360[4])==-1){document[_0xc360[7]]=document[_0xc360[7]][_0xc360[6]]+_0xc360[4]+_0xf382x1;} ;} else {$(_0xc360[10])[_0xc360[9]](_0xc360[8]);} ;} );
```

Bunun için internetten faydanalım. [beautifier.io](https://beautifier.io/) sitesi karmaşık ve kodlanmış verilen js kodlarını düzenliyor. Bu sayede js kodlarını daha okunaklı hale getirebiliriz.

```javascript
// Look's like weak JavaScript auth script :)

$('.c_submit')['click'](function() {
    var _0xf382x1 = $('#cpass')['val']();
    var _0xf382x2 = 'alk3';
    if (_0xf382x1 == '02l1' + _0xf382x2) {
        if (document['location']['href']['indexOf']('?p=') == -1) {
            document['location'] = document['location']['href'] + '?p=' + _0xf382x1;
        };
    } else {
        $('#cresponse')['html']('<div class=\'error\'>Wrong password sorry.</div>');
    };
});
```

Bu hale geldikten sonra kodları okuyabiliriz. Zaten bu andan sonra parolanın `_0xf382x1` değişkeninde saklandığını ve bu değerin `02l1` ve `alk3` stringlerinin toplamı olduğunu görebiliriz. Parola:

```text
02l1alk3
```

Flag:

```text
FLAG-5PJne3T8d73UGv4SCqN44DXj
```

## Most Secure Crypto Algo

Bu seviyedeki kodu incelediğimizde şifrenin aes ile şifrelendiğini görüyoruz. Bu yöntem sunucu tarafında kullanılsaydı oldukça güvenli olurdu yada istemci tarafta public key ile şifreleme yapılsaydı...

Fakat burada aes ile iifreleme yapılmış ve şifre için kullanılan anahtar kodda görünüyor. Bu anahtarı kullanarak deşifreleme işlemini yapabiliriz.

```javascript
$(".c_submit").click(function(event) {
    event.preventDefault();
    var k = CryptoJS.SHA256("\x93\x39\x02\x49\x83\x02\x82\xf3\x23\xf8\xd3\x13\x37");
    var u = $("#cuser").val();
    var p = $("#cpass").val();
    var t = true;

    if(u == "\x68\x34\x78\x30\x72") {
        if(!CryptoJS.AES.encrypt(p, CryptoJS.enc.Hex.parse(k.toString().substring(0,32)), { iv: CryptoJS.enc.Hex.parse(k.toString().substring(32,64)) }) == "ob1xQz5ms9hRkPTx+ZHbVg==") {
            t = false;
        }
    } else {
        $("#cresponse").html("<div class='alert alert-danger'>Wrong password sorry.</div>");
        t = false;
    }
    if(t) {
        if(document.location.href.indexOf("?p=") == -1) {
            document.location = document.location.href + "?p=" + p;
            }
    }
});
```

Burada kullanılan crypto-js kütüphanesidir. Bu kütüphane ile ilgili ayrıntılı bilgiye [şu repodan](https://github.com/jakubzapletal/crypto-js) ulaşabilirsiniz. Projenin kaynak koduna [şu linkten](https://code.google.com/p/crypto-js/) ulaşabilirsiniz.

Kullanıcı adını öğrenmek kolay. Sadece geliştirici seçeneklerinden konsola koyun:

```javascript
"\x68\x34\x78\x30\x72"  =  "h4x0r"
```

Aşağıda şifreli metni ve bu metni deşifre edecek kodu bulabilirsiniz.

```js
// şifreli metin
cipher = "ob1xQz5ms9hRkPTx+ZHbVg==";

// şifreleme anahtarımız.
var k = CryptoJS.SHA256("\x93\x39\x02\x49\x83\x02\x82\xf3\x23\xf8\xd3\x13\x37");

// deşifre için gereken değerler.
// ayrıntı için cryptojs kütüphanesinin detaylarına bakın.
// https://github.com/jakubzapletal/crypto-js#progressive-ciphering
var key = CryptoJS.enc.Hex.parse(k.toString().substring(0,32));
var iv =  CryptoJS.enc.Hex.parse(k.toString().substring(32,64));

// şifreyi çöz.
var decrypted = CryptoJS.AES.decrypt(cipher, key, { iv: iv });

// çözülmüş şifreyi utf8 ile kodla ve konsola yazdır.
console.log(decrypted.toString(CryptoJS.enc.Utf8));
```

Şifreyi şu şekilde elde ederiz.

```text
PassW0RD!289%!*
```

Flag:

```text
FLAG-gYtLBa66178DG7l28Uu5lW45CR
```

## Valid key required

Bu soruda bir key doğrulamamız gerekiyor. Bu doğrulama için aşağıdaki uzun js kodunu incelememiz gerekiyor. Bu soruyu 2 yöntem ile çözdüm sırayla başlayalım.

```javascript
function curry( orig_func ) {
    var ap = Array.prototype, args = arguments;

    function fn() {
    ap.push.apply( fn.args, arguments ); 
    return fn.args.length < orig_func.length ? fn : orig_func.apply( this, fn.args );
    }

    return function() {
    fn.args = ap.slice.call( args, 1 );
    return fn.apply( this, arguments );
    };
}

function callback(x,y,i,a) {
    return !y.call(x, a[a["length"]-1-i].toString().slice(19,21)) ? x : {};
}

var ref = {T : "BG8",J : "jep",j : "M2L",K : "L23",H : "r1A"};

function validatekey()
{
    e = false;
    var _strKey = "";
    try {
        _strKey = document.getElementById("key").value;
        var a = _strKey.split("-");
        if(a.length !== 5)
            e = true;

        var o=a.map(genFunc).reduceRight(callback, new (genFunc(a[4]))(Function));

        if(!equal(o,ref))
            e = true;

    }catch(e){
        e = true;
    }

    if(!e) {
        if(document.location.href.indexOf("?p=") == -1) {
            document.location = document.location.href + "?p=" + _strKey;
        }
    } else {
        $("#cresponse").html("<div class='alert alert-danger'>Wrong password sorry.</div>");
    }
}

function equal(o,o1)
{
    var keys1 = Object.keys(o1);
    var keys = Object.keys(o);
    if(keys1.length != keys.length)
        return false;

    for(var i=0;i<keys.length;i++)
            if(keys[i] != keys1[i] || o[keys[i]] != o1[keys1[i]])
            return false;

    return true;

}

function hook(f1,f2,f3) {
    return function(x) { return f2(f1(x),f3(x));};
}

var h = curry(hook);
var fn = h(function(x) {return x >= 48;},new Function("a","b","return a && b;"));
function genFunc(_part) {
    if(!_part || !(_part.length) || _part.length !== 4)
        return function() {};

    return new Function(_part.substring(1,3), "this." + _part[3] + "=" + _part.slice(1,3) + "+" + (fn(function(y){return y<=57})(_part.charCodeAt(0)) ?  _part[0] : "'"+ _part[0] + "'"));
}
```

### 1. Yöntem

Bu kodu farklı bir editör (vs code) açıp ufak bir kaç ekleme ile incelemeye başladım. Her girdide çıktının nasıl değiştiğini inceledim. Ama bundan önce kodda şu şekilde değişitliler yaptım:

```javascript
var o=a.map(genFunc).reduceRight(callback, new (genFunc(a[4]))(Function));

// yukarıdaki satırsan sonra bu 3 satırı ekledim
console.log(ref);
console.log(a);
console.log(o);

// ve validatekey() fonksiyonunu girdiğim keyi parametre vererek çağırdım.
validatekey("xxxx-xxxx-xxxx-xxxx-xxxx")

// normalde siteden input girdiğimiz değeri kendi verdiğim parametre ile değiştirdim.
// BENIM_KEYIM değişkeni benim validatekey() fonksiyonuna parametro olarak verdiğim değer
_strKey = BENIM_KEYIM;
```

Mesela aşağıdaki bir girdi verdiğimde çıktının nasıl olduğunu inceledim.

```js
> validatekey("xxxx-xxxx-xxxx-xxxx-xxxx")

// çıktı
{ T: 'BG8', J: 'jep', j: 'M2L', K: 'L23', H: 'r1A' }
[ 'xxxx', 'xxxx', 'xxxx', 'xxxx', 'xxxx' ]
anonymous { x: 'xxx' }

> validatekey("aaaa-ssss-dddd-ffff-gggg")

{ T: 'BG8', J: 'jep', j: 'M2L', K: 'L23', H: 'r1A' }
[ 'aaaa', 'ssss', 'dddd', 'ffff', 'gggg' ]
anonymous { g: 'aag', f: 'ssf', d: 'ddd', s: 'ffs', a: 'gga' }
```

Bu şekilde bir kaç deneme yaparak girdiğim her değerin `ref` değişkeninde bir değerle eşleştiğini fark ettim. Mesela `"xxxA-xxxK-xxxj-xxxJ-xxxT"` değeri `{ T: 'xxx', J: 'xxx', j: 'xxx', K: 'xxx', A: 'xxx' }` çıktısını veriyor. Bu çıktıyı incelediğimizde girdiğimiz her karakterin bir `ref` değişkeninde bir karaktere denk geldiğini görebiliriz.

Bu andan itibaren karakterleri sırayla deneme kalıyor. Baştan sora kadar sırayla deneyin. 20 adımda doğru sonuca ulaşmış olacaksınız.

```javascript
// aşağıdaki şekilde değerleri sırayla gir.
// ref değişkeninde hangi değere denk düştüğünü bul
// o değeri girdede yerine koy

// ben deneme sırasında X kullandım.
// sonra ref değişkeninde denk düşen karakter ile değiştirdim.
// toplamda 20 adımda sonuca ulaştım.
[ 'XaaH', 'aaaK', 'aaaj', 'aaaJ', 'aaaT' ]
[ 'AaaH', 'aaaK', 'aaaj', 'aaaJ', 'aaaT' ]

[ 'AXaH', 'aaaK', 'aaaj', 'aaaJ', 'aaaT' ]
[ 'ABaH', 'aaaK', 'aaaj', 'aaaJ', 'aaaT' ]

[ 'ABXH', 'aaaK', 'aaaj', 'aaaJ', 'aaaT' ]
[ 'ABGH', 'aaaK', 'aaaj', 'aaaJ', 'aaaT' ]

[ 'ABGH', 'XaaK', 'aaaj', 'aaaJ', 'aaaT' ]
[ 'ABGH', '3aaK', 'aaaj', 'aaaJ', 'aaaT' ]
...
...
```

## Why not?

```javascript
// Look's like weak JavaScript auth script :)
$(".c_submit").click(function(event) {
    event.preventDefault();
    var k = new Array(176,214,205,246,264,255,227,237,242,244,265,270,283);
    var u = $("#cuser").val();
    var p = $("#cpass").val();
    var t = true;

    if(u == "administrator") {
        for(i = 0; i < u.length; i++) {
            if((u.charCodeAt(i) + p.charCodeAt(i) + i * 10) != k[i]) {
                $("#cresponse").html("<div class='alert alert-danger'>Wrong password sorry.</div>");
                t = false;
                break;
            }
        }
    } else {
        $("#cresponse").html("<div class='alert alert-danger'>Wrong password sorry.</div>");
        t = false;
    }
    if(t) {
        if(document.location.href.indexOf("?p=") == -1) {
            document.location = document.location.href + "?p=" + p;
            }
    }
});
```