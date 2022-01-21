---
layout    : post
title     : "Not Very ProtectedText.com 🔏"
date      : 2020-11-05 22:50:01 -0700
categories: infosec encryption bruteforce
published : True
---

The website [protectedtext.com](https://www.protectedtext.com/) has been a convenient tool that I've used on occasion. I'm writing this article because I recently forgot my password to one of the encrypted sites that I had used to write a rough draft for a flash fiction with my friend a few years ago. After awhile of trying passwords I eventually began to wonder if maybe I could recover the password myself. I immediately started searching for low hanging fruit by looking for clues about the site and it's functionality.

The FAQ revealed the following:

> "Passwords are never sent to our servers. We only store encrypted text - which is useless data once a password is lost. Also, we don't know who this text belongs to."

> "ProtectedText.com uses AES algorithm for encrypting/decrypting the content, together with 'salts' and other known good practices to achieve exceptional security; and SHA512 algorithm for hashing. On top of that, all data is only provided through SSL."

> "It's simple: Open your site with Google Chrome or Mozilla Firefox and save the site before decrypting it (Ctrl + S should work). Make sure to save the site while 'Password required' dialog is still visible. To open your encrypted backup, open saved .html file and type in your password."

## **Deciphering The Encryption**

# Decoding Scheme

This was about all the information I needed to come to the conclusion that the encrypted data must be in the source code. I created a site and inspected the code and sure enough there was the encrypted data encoded in the base64 encoding scheme.

```html
<script type="text/javascript">
    var state;
    var createState = function() {
        /* data from server is stored in 'state' object */
        state = new ClientState(
        "/px41",
        "U2FsdGVkX1+54Y3sTYk98vcirFqiJXApVYMUefKajTvXawfOdTuUqgPv42yOpP2HMvYtFWnzjJX8AL1
        JbgcRYd3vDjPCHVUycwFSACr2ucVBLf7IeddbP/z3xbjaxYf6VadTNBXnXzfkIXEB0/72NDgdA+A9aCUG
        Lq228R0tZYYbX1EBFiVgjb277zNNzTmERNpN/LczcJdJtLVli/OIQiPQU3t/nFmGscwlF/wbIQ4=",
        false,2,2);
    }
</script>
```

My next inclination was to decode the base64 encoding, using `python`, to see if there were any clues hidden within. It would seem at the very least that this chunk of data contained a salt of unknown length and the encrypted data. 

```python
from base64 import b64decode

decoded = b64decode(
    "U2FsdGVkX1+54Y3sTYk98vcirFqiJXApVYMUefKajTvXawfOdTuUqgPv42yOpP2HMvYtFWnzjJX8AL1
    JbgcRYd3vDjPCHVUycwFSACr2ucVBLf7IeddbP/z3xbjaxYf6VadTNBXnXzfkIXEB0/72NDgdA+A9aCUG
    Lq228R0tZYYbX1EBFiVgjb277zNNzTmERNpN/LczcJdJtLVli/OIQiPQU3t/nFmGscwlF/wbIQ4="
)

print(decoded)

b'Salted__\xb9\xe1\x8d\xecM\x89=\xf2\xf7"\xacZ\xa2%p)U\x83\x14y\xf2\x9a\x8d;\xd7k\x07
\xceu;\x94\xaa\x03\xef\xe3l\x8e\xa4\xfd\x872\xf6-\x15i\xf3\x8c\x95\xfc\x00\xbdIn\x07
\x11a\xdd\xef\x0e3\xc2\x1dU2s\x01R\x00*\xf6\xb9\xc5A-\xfe\xc8y\xd7[?\xfc\xf7\xc5\xb8
\xda\xc5\x87\xfaU\xa7S4\x15\xe7_7\xe4!q\x01\xd3\xfe\xf648\x1d\x03\xe0=h%\x06.\xad\xb6
\xf1\x1d-e\x86\x1b_Q\x01\x16%`\x8d\xbd\xbb\xef3M\xcd9\x84D\xdaM\xfc\xb73p\x97I\xb4
\xb5e\x8b\xf3\x88B#\xd0S{\x7f\x9cY\x86\xb1\xcc%\x17\xfc\x1b!\x0e'
```

# Trust the source

At this point there was no way to know where the salt ended and the ciphertext began, so I decided to examine the source code a bit more. The comment `<!-- main JS file for ProtectedText.com -->` was intriguing so I followed it.

```html
<script type="text/javascript" src="/js/sha512.js"></script> <!-- lib SHA-512 -->
<script type="text/javascript" src="/js/aes.js"></script> <!-- lib for AES -->
<script type="text/javascript" src="/js/main.js?v=9"></script> <!-- main JS file for ProtectedText.com -->
```

There were two functions that stood out, and they revealed that the website was using the `CryptoJS library`. It looked as though the decryption function handled all the details we needed to find.

```js
newContent = CryptoJS.AES.decrypt(eOrigContent, pass).toString(CryptoJS.enc.Utf8); // try decrypting content

var eContent = CryptoJS.AES.encrypt(String(content + siteHash), passwordToUse).toString(); // encrypt(content + siteHash, password)
```

I went back to the source code that linked to `aes.js` and took a look around. The source had obviously been obfuscated for the sake of minimalism. I did a few string searches for some of the details I was looking for. From what I could glean the default mode of operation in use was `Cipher Block Chaining (CBC)`. The key-derivation function implemented was a non-standard and obsolete KDF called `EvpKDF` and the use of `PKCS#7` padding became evident. 

```
var CryptoJS=CryptoJS||function(u,p){var d={},l=d.lib={},s=function(){},t=l.Base={extend:function(a){s.prototype=this;var c=new s;a&&c.mixIn(a);c.hasOwnProperty("init")||(c.init=function(){c.$super.init.apply(this,arguments)});c.init.prototype=c;c.$super=this;return c},create:function(){var a=this.extend();a.init.apply(a,arguments);return a},init:function(){},mixIn:function(a){for(var c in a)a.hasOwnProperty(c)&&(this[c]=a[c]);a.hasOwnProperty("toString")&&(this.toString=a.toString)},clone:function(){return this.init.prototype.extend(this)}},
r=l.WordArray=t.extend({init:function(a,c){a=this.words=a||[];this.sigBytes=c!=p?c:4*a.length},toString:function(a){return(a||v).stringify(this)},concat:function(a){var c=this.words,e=a.words,j=this.sigBytes;a=a.sigBytes;this.clamp();if(j%4)for(var k=0;k<a;k++)c[j+k>>>2]|=(e[k>>>2]>>>24-8*(k%4)&255)<<24-8*((j+k)%4);else if(65535<e.length)for(k=0;k<a;k+=4)c[j+k>>>2]=e[k>>>2];else c.push.apply(c,e);this.sigBytes+=a;return this},clamp:function(){var a=this.words,c=this.sigBytes;a[c>>>2]&=4294967295<<
32-8*(c%4);a.length=u.ceil(c/4)},clone:function(){var a=t.clone.call(this);a.words=this.words.slice(0);return a},random:function(a){for(var c=[],e=0;e<a;e+=4)c.push(4294967296*u.random()|0);return new r.init(c,a)}}),w=d.enc={},v=w.Hex={stringify:function(a){var c=a.words;a=a.sigBytes;for(var e=[],j=0;j<a;j++){var k=c[j>>>2]>>>24-8*(j%4)&255;e.push((k>>>4).toString(16));e.push((k&15).toString(16))}return e.join("")},parse:function(a){for(var c=a.length,e=[],j=0;j<c;j+=2)e[j>>>3]|=parseInt(a.substr(j,
2),16)<<24-4*(j%8);return new r.init(e,c/2)}},b=w.Latin1={stringify:function(a){var c=a.words;a=a.sigBytes;for(var e=[],j=0;j<a;j++)e.push(String.fromCharCode(c[j>>>2]>>>24-8*(j%4)&255));return e.join("")},parse:function(a){for(var c=a.length,e=[],j=0;j<c;j++)e[j>>>2]|=(a.charCodeAt(j)&255)<<24-8*(j%4);return new r.init(e,c)}},x=w.Utf8={stringify:function(a){try{return decodeURIComponent(escape(b.stringify(a)))}catch(c){throw Error("Malformed UTF-8 data");}},parse:function(a){return b.parse(unescape(encodeURIComponent(a)))}},
q=l.BufferedBlockAlgorithm=t.extend({reset:function(){this._data=new r.init;this._nDataBytes=0},_append:function(a){"string"==typeof a&&(a=x.parse(a));this._data.concat(a);this._nDataBytes+=a.sigBytes},_process:function(a){var c=this._data,e=c.words,j=c.sigBytes,k=this.blockSize,b=j/(4*k),b=a?u.ceil(b):u.max((b|0)-this._minBufferSize,0);a=b*k;j=u.min(4*a,j);if(a){for(var q=0;q<a;q+=k)this._doProcessBlock(e,q);q=e.splice(0,a);c.sigBytes-=j}return new r.init(q,j)},clone:function(){var a=t.clone.call(this);
a._data=this._data.clone();return a},_minBufferSize:0});l.Hasher=q.extend({cfg:t.extend(),init:function(a){this.cfg=this.cfg.extend(a);this.reset()},reset:function(){q.reset.call(this);this._doReset()},update:function(a){this._append(a);this._process();return this},finalize:function(a){a&&this._append(a);return this._doFinalize()},blockSize:16,_createHelper:function(a){return function(b,e){return(new a.init(e)).finalize(b)}},_createHmacHelper:function(a){return function(b,e){return(new n.HMAC.init(a,
e)).finalize(b)}}});var n=d.algo={};return d}(Math);
(function(){var u=CryptoJS,p=u.lib.WordArray;u.enc.Base64={stringify:function(d){var l=d.words,p=d.sigBytes,t=this._map;d.clamp();d=[];for(var r=0;r<p;r+=3)for(var w=(l[r>>>2]>>>24-8*(r%4)&255)<<16|(l[r+1>>>2]>>>24-8*((r+1)%4)&255)<<8|l[r+2>>>2]>>>24-8*((r+2)%4)&255,v=0;4>v&&r+0.75*v<p;v++)d.push(t.charAt(w>>>6*(3-v)&63));if(l=t.charAt(64))for(;d.length%4;)d.push(l);return d.join("")},parse:function(d){var l=d.length,s=this._map,t=s.charAt(64);t&&(t=d.indexOf(t),-1!=t&&(l=t));for(var t=[],r=0,w=0;w<
l;w++)if(w%4){var v=s.indexOf(d.charAt(w-1))<<2*(w%4),b=s.indexOf(d.charAt(w))>>>6-2*(w%4);t[r>>>2]|=(v|b)<<24-8*(r%4);r++}return p.create(t,r)},_map:"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/="}})();
(function(u){function p(b,n,a,c,e,j,k){b=b+(n&a|~n&c)+e+k;return(b<<j|b>>>32-j)+n}function d(b,n,a,c,e,j,k){b=b+(n&c|a&~c)+e+k;return(b<<j|b>>>32-j)+n}function l(b,n,a,c,e,j,k){b=b+(n^a^c)+e+k;return(b<<j|b>>>32-j)+n}function s(b,n,a,c,e,j,k){b=b+(a^(n|~c))+e+k;return(b<<j|b>>>32-j)+n}for(var t=CryptoJS,r=t.lib,w=r.WordArray,v=r.Hasher,r=t.algo,b=[],x=0;64>x;x++)b[x]=4294967296*u.abs(u.sin(x+1))|0;r=r.MD5=v.extend({_doReset:function(){this._hash=new w.init([1732584193,4023233417,2562383102,271733878])},
_doProcessBlock:function(q,n){for(var a=0;16>a;a++){var c=n+a,e=q[c];q[c]=(e<<8|e>>>24)&16711935|(e<<24|e>>>8)&4278255360}var a=this._hash.words,c=q[n+0],e=q[n+1],j=q[n+2],k=q[n+3],z=q[n+4],r=q[n+5],t=q[n+6],w=q[n+7],v=q[n+8],A=q[n+9],B=q[n+10],C=q[n+11],u=q[n+12],D=q[n+13],E=q[n+14],x=q[n+15],f=a[0],m=a[1],g=a[2],h=a[3],f=p(f,m,g,h,c,7,b[0]),h=p(h,f,m,g,e,12,b[1]),g=p(g,h,f,m,j,17,b[2]),m=p(m,g,h,f,k,22,b[3]),f=p(f,m,g,h,z,7,b[4]),h=p(h,f,m,g,r,12,b[5]),g=p(g,h,f,m,t,17,b[6]),m=p(m,g,h,f,w,22,b[7]),
f=p(f,m,g,h,v,7,b[8]),h=p(h,f,m,g,A,12,b[9]),g=p(g,h,f,m,B,17,b[10]),m=p(m,g,h,f,C,22,b[11]),f=p(f,m,g,h,u,7,b[12]),h=p(h,f,m,g,D,12,b[13]),g=p(g,h,f,m,E,17,b[14]),m=p(m,g,h,f,x,22,b[15]),f=d(f,m,g,h,e,5,b[16]),h=d(h,f,m,g,t,9,b[17]),g=d(g,h,f,m,C,14,b[18]),m=d(m,g,h,f,c,20,b[19]),f=d(f,m,g,h,r,5,b[20]),h=d(h,f,m,g,B,9,b[21]),g=d(g,h,f,m,x,14,b[22]),m=d(m,g,h,f,z,20,b[23]),f=d(f,m,g,h,A,5,b[24]),h=d(h,f,m,g,E,9,b[25]),g=d(g,h,f,m,k,14,b[26]),m=d(m,g,h,f,v,20,b[27]),f=d(f,m,g,h,D,5,b[28]),h=d(h,f,
m,g,j,9,b[29]),g=d(g,h,f,m,w,14,b[30]),m=d(m,g,h,f,u,20,b[31]),f=l(f,m,g,h,r,4,b[32]),h=l(h,f,m,g,v,11,b[33]),g=l(g,h,f,m,C,16,b[34]),m=l(m,g,h,f,E,23,b[35]),f=l(f,m,g,h,e,4,b[36]),h=l(h,f,m,g,z,11,b[37]),g=l(g,h,f,m,w,16,b[38]),m=l(m,g,h,f,B,23,b[39]),f=l(f,m,g,h,D,4,b[40]),h=l(h,f,m,g,c,11,b[41]),g=l(g,h,f,m,k,16,b[42]),m=l(m,g,h,f,t,23,b[43]),f=l(f,m,g,h,A,4,b[44]),h=l(h,f,m,g,u,11,b[45]),g=l(g,h,f,m,x,16,b[46]),m=l(m,g,h,f,j,23,b[47]),f=s(f,m,g,h,c,6,b[48]),h=s(h,f,m,g,w,10,b[49]),g=s(g,h,f,m,
E,15,b[50]),m=s(m,g,h,f,r,21,b[51]),f=s(f,m,g,h,u,6,b[52]),h=s(h,f,m,g,k,10,b[53]),g=s(g,h,f,m,B,15,b[54]),m=s(m,g,h,f,e,21,b[55]),f=s(f,m,g,h,v,6,b[56]),h=s(h,f,m,g,x,10,b[57]),g=s(g,h,f,m,t,15,b[58]),m=s(m,g,h,f,D,21,b[59]),f=s(f,m,g,h,z,6,b[60]),h=s(h,f,m,g,C,10,b[61]),g=s(g,h,f,m,j,15,b[62]),m=s(m,g,h,f,A,21,b[63]);a[0]=a[0]+f|0;a[1]=a[1]+m|0;a[2]=a[2]+g|0;a[3]=a[3]+h|0},_doFinalize:function(){var b=this._data,n=b.words,a=8*this._nDataBytes,c=8*b.sigBytes;n[c>>>5]|=128<<24-c%32;var e=u.floor(a/
4294967296);n[(c+64>>>9<<4)+15]=(e<<8|e>>>24)&16711935|(e<<24|e>>>8)&4278255360;n[(c+64>>>9<<4)+14]=(a<<8|a>>>24)&16711935|(a<<24|a>>>8)&4278255360;b.sigBytes=4*(n.length+1);this._process();b=this._hash;n=b.words;for(a=0;4>a;a++)c=n[a],n[a]=(c<<8|c>>>24)&16711935|(c<<24|c>>>8)&4278255360;return b},clone:function(){var b=v.clone.call(this);b._hash=this._hash.clone();return b}});t.MD5=v._createHelper(r);t.HmacMD5=v._createHmacHelper(r)})(Math);
(function(){var u=CryptoJS,p=u.lib,d=p.Base,l=p.WordArray,p=u.algo,s=p.EvpKDF=d.extend({cfg:d.extend({keySize:4,hasher:p.MD5,iterations:1}),init:function(d){this.cfg=this.cfg.extend(d)},compute:function(d,r){for(var p=this.cfg,s=p.hasher.create(),b=l.create(),u=b.words,q=p.keySize,p=p.iterations;u.length<q;){n&&s.update(n);var n=s.update(d).finalize(r);s.reset();for(var a=1;a<p;a++)n=s.finalize(n),s.reset();b.concat(n)}b.sigBytes=4*q;return b}});u.EvpKDF=function(d,l,p){return s.create(p).compute(d,
l)}})();
CryptoJS.lib.Cipher||function(u){var p=CryptoJS,d=p.lib,l=d.Base,s=d.WordArray,t=d.BufferedBlockAlgorithm,r=p.enc.Base64,w=p.algo.EvpKDF,v=d.Cipher=t.extend({cfg:l.extend(),createEncryptor:function(e,a){return this.create(this._ENC_XFORM_MODE,e,a)},createDecryptor:function(e,a){return this.create(this._DEC_XFORM_MODE,e,a)},init:function(e,a,b){this.cfg=this.cfg.extend(b);this._xformMode=e;this._key=a;this.reset()},reset:function(){t.reset.call(this);this._doReset()},process:function(e){this._append(e);return this._process()},
finalize:function(e){e&&this._append(e);return this._doFinalize()},keySize:4,ivSize:4,_ENC_XFORM_MODE:1,_DEC_XFORM_MODE:2,_createHelper:function(e){return{encrypt:function(b,k,d){return("string"==typeof k?c:a).encrypt(e,b,k,d)},decrypt:function(b,k,d){return("string"==typeof k?c:a).decrypt(e,b,k,d)}}}});d.StreamCipher=v.extend({_doFinalize:function(){return this._process(!0)},blockSize:1});var b=p.mode={},x=function(e,a,b){var c=this._iv;c?this._iv=u:c=this._prevBlock;for(var d=0;d<b;d++)e[a+d]^=
c[d]},q=(d.BlockCipherMode=l.extend({createEncryptor:function(e,a){return this.Encryptor.create(e,a)},createDecryptor:function(e,a){return this.Decryptor.create(e,a)},init:function(e,a){this._cipher=e;this._iv=a}})).extend();q.Encryptor=q.extend({processBlock:function(e,a){var b=this._cipher,c=b.blockSize;x.call(this,e,a,c);b.encryptBlock(e,a);this._prevBlock=e.slice(a,a+c)}});q.Decryptor=q.extend({processBlock:function(e,a){var b=this._cipher,c=b.blockSize,d=e.slice(a,a+c);b.decryptBlock(e,a);x.call(this,
e,a,c);this._prevBlock=d}});b=b.CBC=q;q=(p.pad={}).Pkcs7={pad:function(a,b){for(var c=4*b,c=c-a.sigBytes%c,d=c<<24|c<<16|c<<8|c,l=[],n=0;n<c;n+=4)l.push(d);c=s.create(l,c);a.concat(c)},unpad:function(a){a.sigBytes-=a.words[a.sigBytes-1>>>2]&255}};d.BlockCipher=v.extend({cfg:v.cfg.extend({mode:b,padding:q}),reset:function(){v.reset.call(this);var a=this.cfg,b=a.iv,a=a.mode;if(this._xformMode==this._ENC_XFORM_MODE)var c=a.createEncryptor;else c=a.createDecryptor,this._minBufferSize=1;this._mode=c.call(a,
this,b&&b.words)},_doProcessBlock:function(a,b){this._mode.processBlock(a,b)},_doFinalize:function(){var a=this.cfg.padding;if(this._xformMode==this._ENC_XFORM_MODE){a.pad(this._data,this.blockSize);var b=this._process(!0)}else b=this._process(!0),a.unpad(b);return b},blockSize:4});var n=d.CipherParams=l.extend({init:function(a){this.mixIn(a)},toString:function(a){return(a||this.formatter).stringify(this)}}),b=(p.format={}).OpenSSL={stringify:function(a){var b=a.ciphertext;a=a.salt;return(a?s.create([1398893684,
1701076831]).concat(a).concat(b):b).toString(r)},parse:function(a){a=r.parse(a);var b=a.words;if(1398893684==b[0]&&1701076831==b[1]){var c=s.create(b.slice(2,4));b.splice(0,4);a.sigBytes-=16}return n.create({ciphertext:a,salt:c})}},a=d.SerializableCipher=l.extend({cfg:l.extend({format:b}),encrypt:function(a,b,c,d){d=this.cfg.extend(d);var l=a.createEncryptor(c,d);b=l.finalize(b);l=l.cfg;return n.create({ciphertext:b,key:c,iv:l.iv,algorithm:a,mode:l.mode,padding:l.padding,blockSize:a.blockSize,formatter:d.format})},
decrypt:function(a,b,c,d){d=this.cfg.extend(d);b=this._parse(b,d.format);return a.createDecryptor(c,d).finalize(b.ciphertext)},_parse:function(a,b){return"string"==typeof a?b.parse(a,this):a}}),p=(p.kdf={}).OpenSSL={execute:function(a,b,c,d){d||(d=s.random(8));a=w.create({keySize:b+c}).compute(a,d);c=s.create(a.words.slice(b),4*c);a.sigBytes=4*b;return n.create({key:a,iv:c,salt:d})}},c=d.PasswordBasedCipher=a.extend({cfg:a.cfg.extend({kdf:p}),encrypt:function(b,c,d,l){l=this.cfg.extend(l);d=l.kdf.execute(d,
b.keySize,b.ivSize);l.iv=d.iv;b=a.encrypt.call(this,b,c,d.key,l);b.mixIn(d);return b},decrypt:function(b,c,d,l){l=this.cfg.extend(l);c=this._parse(c,l.format);d=l.kdf.execute(d,b.keySize,b.ivSize,c.salt);l.iv=d.iv;return a.decrypt.call(this,b,c,d.key,l)}})}();
(function(){for(var u=CryptoJS,p=u.lib.BlockCipher,d=u.algo,l=[],s=[],t=[],r=[],w=[],v=[],b=[],x=[],q=[],n=[],a=[],c=0;256>c;c++)a[c]=128>c?c<<1:c<<1^283;for(var e=0,j=0,c=0;256>c;c++){var k=j^j<<1^j<<2^j<<3^j<<4,k=k>>>8^k&255^99;l[e]=k;s[k]=e;var z=a[e],F=a[z],G=a[F],y=257*a[k]^16843008*k;t[e]=y<<24|y>>>8;r[e]=y<<16|y>>>16;w[e]=y<<8|y>>>24;v[e]=y;y=16843009*G^65537*F^257*z^16843008*e;b[k]=y<<24|y>>>8;x[k]=y<<16|y>>>16;q[k]=y<<8|y>>>24;n[k]=y;e?(e=z^a[a[a[G^z]]],j^=a[a[j]]):e=j=1}var H=[0,1,2,4,8,
16,32,64,128,27,54],d=d.AES=p.extend({_doReset:function(){for(var a=this._key,c=a.words,d=a.sigBytes/4,a=4*((this._nRounds=d+6)+1),e=this._keySchedule=[],j=0;j<a;j++)if(j<d)e[j]=c[j];else{var k=e[j-1];j%d?6<d&&4==j%d&&(k=l[k>>>24]<<24|l[k>>>16&255]<<16|l[k>>>8&255]<<8|l[k&255]):(k=k<<8|k>>>24,k=l[k>>>24]<<24|l[k>>>16&255]<<16|l[k>>>8&255]<<8|l[k&255],k^=H[j/d|0]<<24);e[j]=e[j-d]^k}c=this._invKeySchedule=[];for(d=0;d<a;d++)j=a-d,k=d%4?e[j]:e[j-4],c[d]=4>d||4>=j?k:b[l[k>>>24]]^x[l[k>>>16&255]]^q[l[k>>>
8&255]]^n[l[k&255]]},encryptBlock:function(a,b){this._doCryptBlock(a,b,this._keySchedule,t,r,w,v,l)},decryptBlock:function(a,c){var d=a[c+1];a[c+1]=a[c+3];a[c+3]=d;this._doCryptBlock(a,c,this._invKeySchedule,b,x,q,n,s);d=a[c+1];a[c+1]=a[c+3];a[c+3]=d},_doCryptBlock:function(a,b,c,d,e,j,l,f){for(var m=this._nRounds,g=a[b]^c[0],h=a[b+1]^c[1],k=a[b+2]^c[2],n=a[b+3]^c[3],p=4,r=1;r<m;r++)var q=d[g>>>24]^e[h>>>16&255]^j[k>>>8&255]^l[n&255]^c[p++],s=d[h>>>24]^e[k>>>16&255]^j[n>>>8&255]^l[g&255]^c[p++],t=
d[k>>>24]^e[n>>>16&255]^j[g>>>8&255]^l[h&255]^c[p++],n=d[n>>>24]^e[g>>>16&255]^j[h>>>8&255]^l[k&255]^c[p++],g=q,h=s,k=t;q=(f[g>>>24]<<24|f[h>>>16&255]<<16|f[k>>>8&255]<<8|f[n&255])^c[p++];s=(f[h>>>24]<<24|f[k>>>16&255]<<16|f[n>>>8&255]<<8|f[g&255])^c[p++];t=(f[k>>>24]<<24|f[n>>>16&255]<<16|f[g>>>8&255]<<8|f[h&255])^c[p++];n=(f[n>>>24]<<24|f[g>>>16&255]<<16|f[h>>>8&255]<<8|f[k&255])^c[p++];a[b]=q;a[b+1]=s;a[b+2]=t;a[b+3]=n},keySize:8});u.AES=p._createHelper(d)})();
```

I knew this obfuscated javascript was not going to be very clear and that I needed to download and sift through the CryptoJS library. The cipher core components file cipher-core.js contained the OpenSSL key-derivation function, which revealed another vital piece of information. `The salt`, if omitted, defaulted to a `random 8-byte chunk` of data. Not only this but the `KDF` seems to be responsible for generating our decryption `key` and  `initialization vector`.

```js
/**
 * OpenSSL key derivation function.
 */
var OpenSSLKdf = C_kdf.OpenSSL = {
    /**
     * Derives a key and IV from a password.
     *
     * @param {string} password The password to derive from.
     * @param {number} keySize The size in words of the key to generate.
     * @param {number} ivSize The size in words of the IV to generate.
     * @param {WordArray|string} salt (Optional) A 64-bit salt to use. If omitted, a salt will be generated randomly.
     *
     * @return {CipherParams} A cipher params object with the key, IV, and salt.
     *
     * @static
     *
     * @example
     *
     *     var derivedParams = CryptoJS.kdf.OpenSSL.execute('Password', 256/32, 128/32);
     *     var derivedParams = CryptoJS.kdf.OpenSSL.execute('Password', 256/32, 128/32, 'saltsalt');
     */
    execute: function (password, keySize, ivSize, salt) {
        // Generate random salt
        if (!salt) {
            salt = WordArray.random(64/8);
        }

        // Derive key and IV
        var key = EvpKDF.create({ keySize: keySize + ivSize }).compute(password, salt);

        // Separate key and IV
        var iv = WordArray.create(key.words.slice(keySize), ivSize * 4);
        key.sigBytes = keySize * 4;

        // Return params
        return CipherParams.create({ key: key, iv: iv, salt: salt });
    }
};
```

The CryptoJS file cipher-core.js also contained the OpenSSL formatting strategy. The line that is of particular interest is the extraction of the salt which instructs us to skip the first 8 bytes 'Salted__' then capture the next. We now know that our base64 decoded ciphertext begins with 8 bytes of unnecessary string, followed by 8 bytes which is our salt, followed by the ciphertext.

```js
/**
 * OpenSSL formatting strategy.
 */
var OpenSSLFormatter = C_format.OpenSSL = {
    /**
     * Converts a cipher params object to an OpenSSL-compatible string.
     *
     * @param {CipherParams} cipherParams The cipher params object.
     *
     * @return {string} The OpenSSL-compatible string.
     *
     * @static
     *
     * @example
     *
     *     var openSSLString = CryptoJS.format.OpenSSL.stringify(cipherParams);
     */
    stringify: function (cipherParams) {
        // Shortcuts
        var ciphertext = cipherParams.ciphertext;
        var salt = cipherParams.salt;

        // Format
        if (salt) {
            var wordArray = WordArray.create([0x53616c74, 0x65645f5f]).concat(salt).concat(ciphertext);
        } else {
            var wordArray = ciphertext;
        }

        return wordArray.toString(Base64);
    },

    /**
     * Converts an OpenSSL-compatible string to a cipher params object.
     *
     * @param {string} openSSLStr The OpenSSL-compatible string.
     *
     * @return {CipherParams} The cipher params object.
     *
     * @static
     *
     * @example
     *
     *     var cipherParams = CryptoJS.format.OpenSSL.parse(openSSLString);
     */
    parse: function (openSSLStr) {
        // Parse base64
        var ciphertext = Base64.parse(openSSLStr);

        // Shortcut
        var ciphertextWords = ciphertext.words;

        // Test for salt
        if (ciphertextWords[0] == 0x53616c74 && ciphertextWords[1] == 0x65645f5f) {
            // Extract salt
            var salt = WordArray.create(ciphertextWords.slice(2, 4));

            // Remove salt from ciphertext
            ciphertextWords.splice(0, 4);
            ciphertext.sigBytes -= 16;
        }

        return CipherParams.create({ ciphertext: ciphertext, salt: salt });
    }
};
```

# Decryption & Key-Derivation

Now that we know that the mode of operation is `CBC`, `EvpKDF is used to generate the key and iv`, the `salt` is the second `8-bytes before the ciphertext`, the padding utilized `PKCS#7`, and any hashing & integrity checking seemed to be limited to the plaintext data, we could now begin building our decryption routine in python. The only thing missing was a functional recreation of the EvpKDF for which no python library seemed to exist.

The comments in cipher-core.js OpenSSL key derivation function `var derivedParams = CryptoJS.kdf.OpenSSL.execute('Password', 256/32, 128/32);` revealed a `256-bit Key`, followed by an `IV size of 128-bits`. I still needed to know what one-way cryptographic hash algorithm was being used. I found what I was looking for in evpkdf.js, MD5 with only 1 iteration was used to derive the key and IV.

```js
// CryptoJS evpkdf.js
var EvpKDF = C_algo.EvpKDF = Base.extend({
    /**
     * Configuration options.
     *
     * @property {number} keySize The key size in words to generate. Default: 4 (128 bits)
     * @property {Hasher} hasher The hash algorithm to use. Default: MD5
     * @property {number} iterations The number of iterations to perform. Default: 1
     */
    cfg: Base.extend({
        keySize: 128/32,
        hasher: MD5,
        iterations: 1
    }),

    ...
```

The Python implementation of EvpKDF was probably the most difficult thing about this process only because it's non-standard and obsolete with almost no code examples in the language i'm familiar with:

```python
@staticmethod
def _key_derivation_evp(password, salt, keySize=8, ivSize=4, iterations=1, hashAlgorithm=MD5):
    """
    If the total key and IV length is less than the digest length and MD5 is used then the derivation algorithm is compatible with PKCS#5 v1.5 otherwise a non standard extension is used to derive the extra data. 
    https://www.openssl.org/docs/manmaster/man3/EVP_BytesToKey.html
    https://github.com/CryptoStore/crypto-js/blob/3.1.2/src/evpkdf.js
    https://gist.github.com/adrianlzt/d5c9657e205b57f687f528a5ac59fe0e
    """
    targetKeySize     = keySize + ivSize
    derivedBytes      = b""
    derivedWordsCount = 0
    block             = None
    hasher            = hashAlgorithm.new()
    while derivedWordsCount < targetKeySize:
        if block: 
            hasher.update(block)

        hasher.update(password)
        hasher.update(salt)
        block  = hasher.digest()
        hasher = hashAlgorithm.new()

        for _ in range(1, iterations):
            hasher.update(block)
            block  = hasher.digest()
            hasher = hashAlgorithm.new()

        derivedBytes += block[: min(len(block), (targetKeySize - derivedWordsCount) * 4)]

        derivedWordsCount += len(block) / 4

    # Password & IV Tuple
    return derivedBytes[0: keySize * 4], derivedBytes[keySize * 4:]
```

The decryption function followed naturally, but my goal for this project was to recover a lost password. This means that I need to bruteforce the solution. If everything decrypted without issue I needed a way to check for common english words to verify that I had actually decrypted something intelligible. 

```python
def _decrypt(self, password, ciphertext):
    try:
        decoded    = b64decode(ciphertext)
        salt       = decoded[8:16]
        ciphertext = decoded[16:]
        key, iv    = self._key_derivation_evp(password, salt)
        decipher   = AES.new(key, AES.MODE_CBC, iv)
        decrypted  = decipher.decrypt(ciphertext)
        unpadded   = unpad(decrypted, AES.block_size)
        plaintext  = unpadded[:-128] #SHA512 (128-bytes)
        return plaintext.decode('utf-8')
    except:
        pass
```

I copied some stop words from NLTK and created a small function to alert me if they were found in the decrypted plaintext.

```python
# https://gist.github.com/sebleier/554280
STOPWORDS = [ 
    'i', 'me', 'my', 'myself', 'we', 'our', 'ours', 
    'ourselves', 'you', 'your', 'yours', 'yourself', 
    'yourselves', 'he', 'him', 'his', 'himself', 'she', 
    'her', 'hers', 'herself', 'it', 'its', 'itself', 
    'they', 'them', 'their', 'theirs', 'themselves', 
    'what', 'which', 'who', 'whom', 'this', 'that', 
    'these', 'those', 'am', 'is', 'are', 'was', 'were', 
    'be', 'been', 'being', 'have', 'has', 'had', 'having', 
    'do', 'does', 'did', 'doing', 'a', 'an', 'the', 'and', 
    'but', 'if', 'or', 'because', 'as', 'until', 'while', 
    'of', 'at', 'by', 'for', 'with', 'about', 'against', 
    'between', 'into', 'through', 'during', 'before', 
    'after', 'above', 'below', 'to', 'from', 'up', 'down', 
    'in', 'out', 'on', 'off', 'over', 'under', 'again', 
    'further', 'then', 'once', 'here', 'there', 'when', 
    'where', 'why', 'how', 'all', 'any', 'both', 'each', 
    'few', 'more', 'most', 'other', 'some', 'such', 'no', 
    'nor', 'not', 'only', 'own', 'same', 'so', 'than', 
    'too', 'very', 's', 't', 'can', 'will', 'just', 'don', 
    'should', 'now'
]

@staticmethod
def _detect_text(decrypted):
    # test decrypted data
    for stop in STOPWORDS:
        if stop in decrypted:
            return True
    return False
```

My next thought was whether or not to use a dictionary attack or bruteforce up to maxLength 8 characters which would take entirely too long. I decided to go with the dictionary attack first. I grabbed some wordlists from [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Common-Credentials) and created the following function to parse any specific file into a list.

```python
@staticmethod
def _wordlist(filePath):
    filePath = os.path.expanduser(
        os.path.expandvars(filePath)
    )
    
    if not os.path.isfile(filePath):
        print("Invalid wordlist!")
        return False

    with open(filePath, 'r') as f:
        internal = ""
        while True:
            buffer = f.read(1024)
            # bufLen = len(buffer)

            if not buffer:
                break

            internal += buffer
        
        return internal.split()
```

Now that the wordlists were handled I needed to create a function to handle the dictionary attack. I hardcoded the smallest dictionary in as the default if no wordlist was designated / supplied. Please forgive the profanity, just know that these passwords rate at the top 100 of all passwords ever used and it comes with the wordlists.

```python
# https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/10-million-password-list-top-100.txt
TOP100 = [
    '123456', 'password', etc ...
]

@ctrl_c
def dictionary_attack(self, ciphertext, dictionary=None, hybrid=False):
    print(f"[*] Analyzing Ciphertext:\n\n{ciphertext}\n")
    print(f"[*] Initiating Dictionary Attack ...\n")

    dictionary = self._wordlist(dictionary) if dictionary else TOP100
    dictSize10 = len(dictionary) * 0.1
    show       = dictSize10 if dictSize10 < 1000 else 1000

    for c, password in enumerate(dictionary):
        bytesPass = bytes(password, 'utf-8')
        decrypted = self._decrypt(bytesPass, ciphertext)

        if not decrypted:
            if self.__verbose:
                print(f"[Attempt] <{c}> password: {password}")
            elif c % show == 0 and c != 0:
                print(f"[Attempt] <{c}> password: {password}")
            continue

        if self._detect_text(decrypted):
            print(f"[!!!!!!!] password: {password}")
            return password, decrypted

    print("Password Not Found! Press Enter or Ctrl+C to Exit ...")
```

After adding the known password into the default and testing this upon my newly created site, I was pleased to see that it had successfully decrypted the contents. However I was unable to recover the password that I originally lost even using the top 1,000,000 passwords. This made me realize that whatever password I had set on my other site was not a dictionary word. I would need to create a bruteforce of all printable characters.

```python
@ctrl_c
def bruteforce_attack(self, ciphertext, characterSet, minKeyLen=1, maxKeyLen=6):
    print(f"[*] Analyzing Ciphertext:\n\n{ciphertext}\n")
    print(f"[*] Initiating Bruteforce Attack ...\n")

    c = 0
    for r in range(minKeyLen, maxKeyLen+1):
        for password in product(characterSet, repeat=r):
            c+=1
            password  = ''.join(password)
            bytesPass = bytes(password, 'utf-8')
            decrypted = self._decrypt(bytesPass, ciphertext)

            if not decrypted:
                if self.__verbose:
                    print(f"[Attempt] <{c}> password: {password}")
                elif c % 1000 == 0 and c != 0:
                    print(f"[Attempt] <{c}> password: {password}")
                continue

            if self._detect_text(decrypted):
                print(f"[!!!!!!!] password: {password}")
                return password, decrypted

    print("Password Not Found! Press Enter or Ctrl+C to Exit ...")
```

## **Convenience Functions & Decorators**

I created this to extract the encrypted content for faster processing. This works by using the requests module to remotely scrape the encrypted data via a combination of BeautifulSoup and regular expressions.

```python
@property
def ciphertext(self):
    page = requests.get(self.__url)
    if not page.ok:
        return None

    soup    = BeautifulSoup(page.text, "html.parser")
    scripts = soup.find_all('script')
    for script in scripts:
        scriptString = str(script)
        if "ClientState" in scriptString:
            try:
                regex = re.search(rf'"/{self.__link}",\s+"(.+)"', scriptString, re.I)
                return bytes(regex.group(1), "utf-8")
            except AttributeError:
                return None
```

This decorator has proven to be extremely useful. Essentially it backgrounds the main program and listens for EOFError generated by Ctrl+C.

```python
def ctrl_c(function):
    @wraps(function)
    def handler(*args, **kwargs):
        return function(*args, **kwargs)
    
    def handle_ctrl_c():
        try:
            input()
        except EOFError:
            print("ctrl+c")
            os._exit(1)
    
    Thread(target=handle_ctrl_c).start()
    return handler
```

## **Multithreading**

We could also implement a multithreaded version of our dictionary attack function which would first require us to queue up our wordlist using pythons built in synchronized queue class.

```python
@staticmethod
def _queueList(words):
    q = Queue()
    for word in words:
        q.put(word)

    return q
```

```python
@ctrl_c
def dictionary_attack_multithreaded(self, ciphertext, dictionary, threads):
    print(f"[*] Analyzing Ciphertext:\n\n{ciphertext}\n")
    print(f"[*] Initiating Multithreaded Dictionary Attack ...\n")

    dictionary = self._wordlist(dictionary)
    queued     = self._queueList(dictionary)
        
    def worker(tid):
        while not queued.empty():
            password  = queued.get_nowait()
            bytesPass = bytes(password, 'utf-8')
            decrypted = self._decrypt(bytesPass, ciphertext)

            if not decrypted:
                if self.__verbose:
                    print(f"[Attempt] Thread<{tid}> password: {password}")
                continue

            if self._detect_text(decrypted):
                print(f"[!!!!!!!] Thread<{tid}> password: {password}")
                queued.task_done()
                os._exit(0)

        queued.task_done()

    threadHandles = []
    for tid in range(threads):
        tHandle = Thread(target=worker, args=(tid,), daemon=True)
        tHandle.start()
        threadHandles.append(tHandle)
    
    for tHandle in threadHandles:
        tHandle.join()

    print("Password Not Found! Press Enter or Ctrl+C to Exit ...")
```

If you'd like to see the working tool check out my github page.