# Forensics

***

## The First Strike

## My Solve:

* Provided PCAP can be opened using wireshark. 
* We can use `FTP` as a filter and go through different protocols to find multiple failed login attempts.

<img width="1302" height="114" alt="image" src="https://github.com/user-attachments/assets/aa9f638c-dc50-43b6-bd64-ec70882d88ad" />

* Using `frame contains "logged in"` helps us find our required login attempt.

<img width="1144" height="114" alt="image" src="https://github.com/user-attachments/assets/900bdf8e-fc3f-46fd-83ee-8493c42cd743" />


Flag : `csd{Elf67_snowball}`

***

# Web Exploitation

***

## Kramazon

> Description
Investigate Kramazon’s ordering workflow. If you can exploit this flaw to obtain Santa Priority Delivery for your order, Kramazon will reveal its restricted Priority Route Manifest, which contains the flag.

Good luck, Operative.

https://kramazon.csd.lol/

## My solve:

Provided is an amazon-like website where orders can be placed. I first tried clicking around to find different endpoints and found that orders could be placed for the Advent calender. Our goal (as stated by the description) is to find a way to change the priority order. 

These were the requests that were sent while creating an order: 

<img width="686" height="70" alt="image" src="https://github.com/user-attachments/assets/17465c63-a475-4c9b-ba63-c91662f2322a" />

Each of the requests had a request header called `Priority: u= 1, i` which I tried modifying with different values only to keep getting `priority":false` as the response. Then upon inspecting the source code I found `script.js` with the following part which I found interesting:

```javascript
 const createRes = await fetch("/create-order", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
    });
    const order = await createRes.json();

    console.log("[*] Waiting for status callback...");
    setTimeout(async () => {
      const callbackRes = await fetch(order.callback_url);
      const status = await callbackRes.json();

      function santaMagic(n) {
        return n ^ 0x37; // TODO: remove in production
      }

      if (status.internal.user === 1) {
        alert("Welcome, Santa! Allowing priority finalize...");
      }

      setTimeout(async () => {
        const finalizeRes = await fetch("/finalize", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            user: status.internal.user,
            order: order.order_id,
          }),
        });
```

This means that setting the value for `user` to `1` would be our next step. Also on looking at the `/finalize` request we can see the cookie value ` auth=BA4FBg%3D%3D` provides us with the response `"message":"Order 64c77513 finalized for user 3921.",`.

The hint for this challenge states that:
```
There is a Set-Cookie header being sent when you request the homepage. Perhaps the server is not verifying this part of the authentication flow properly.

Using any clues that you can find in script.js (do any parts of the code look off?), can you reverse engineer the cookie and send one that can exploit the challenge?
```

I then noticed the `santaMagic` function and realized that it used XOR encoding. I then tried using a web base64 to ascii decoder to decode the value of the cookie `auth:BA4FBg==` only to get some white boxes. After this I tried using cyberchef and converted the value to hex to get `04 0e 05 06`, stacking this with XOR using the key as 37 gives us our original user `3921`: 

 <img width="927" height="609" alt="image" src="https://github.com/user-attachments/assets/e667879f-9297-4a58-9ba0-928ef0b132ac" />


* Now, all I had to do was reverse engineer this to get the output as `Bg==`. THen on intercepting our original cookie with `auth=Bg==` gives us the request: 

 <img width="693" height="172" alt="image" src="https://github.com/user-attachments/assets/31f7e4c3-394b-44bb-aee8-49986a891dcb" />

* Then on heading over to the provided path I was able to find the flag: `csd{npld_async_callback_idor_mastery}`

## Concepts Learned

* Reverse engineering ciphers on CyberChef
* Identifying and exploiting a different kind of IDOR vulnerability.

***

# Multifactorial

>Description
Analyze the authentication flow end-to-end. If multifactor authentication is only as strong as its weakest link, can you find the point where identity becomes a matter of belief rather than proof?

https://multifactorial.csd.lol/

## My Solve:

We are provided with the login screen:

 <img width="816" height="436" alt="image" src="https://github.com/user-attachments/assets/6ee727ab-55be-4e80-83ee-ffde62566154" />

I started off by going through the source code to find this: `'You\x27re\x20really\x20lucky!\x20Here\x27s\x20my\x20hash\x20as\x20a\x20reward.\x20bf33632dd9668787878890cb4fbb54261b6b7571'`

* Then I headed over to [hash identifier](https://hashes.com/en/tools/hash_identifier) to find that it was SHA1 and [decrypted](https://md5decrypt.net/en/Sha1/) it to:

`bf33632dd9668787878890cb4fbb54261b6b7571 : {"Plain":"northpole123","Algo":"sha1"}`

Sure enough, northpole123 was our first password. We now have a different authentication screen which asks for an OTP: 

 <img width="666" height="381" alt="image" src="https://github.com/user-attachments/assets/d459eb40-e016-4b7c-ac18-588bb3a65bb5" />

Going through the source code again I came across this,

 ```
 const ORACLE_KEY = "17_w0Uld_83_V3Ry_fUNnY_1f_y0U_7H0u9H7_7H15_W45_4_Fl49";

      async function verifyCode() {
        const statusEl = document.getElementById("status");
        const code = document.getElementById("code").value.trim();

        statusEl.textContent = "";

        if (!code) {
          statusEl.textContent = "Please enter a TOTP code.";
          return;
        }

        const resp = await fetch("/api/something-you-have-verify?debug=0", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ code }),
        });

        const data = await resp.json();
        if (!resp.ok) {
          statusEl.textContent = `Error: ${data.error || "Unknown error"}`;
          return;
        }

        statusEl.textContent = `${data.message}\nRedirecting to Step 3...`;
        setTimeout(() => {
          window.location.href = "/register-passkey";
        }, 800);

```

* However, I was unable to figure out how to use the `ORACLE_KEY` for this so I used the hint to find https://www.okta.com/identity-101/hmac/. 

This website is about Hash-based message authentication code (HMAC) and how it works. 

## Concepts Learned:
* SHA stands for Secure hashing algorithm. I can identify SHA1 hashes by remembering that they are always 40 characters. 
