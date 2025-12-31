# Forensics

***

## The First Strike

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

## Solve

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
