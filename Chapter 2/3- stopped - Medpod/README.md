# Medpod
Web page contains:
```
<!-------------------------------------------------------->
<!--                                                    -->
<!--  Congratulations!                                  -->
<!--  You found an ADV Glitch! 4ISP #5                  -->
<!--                                                    -->
<!--  We take care of every aspect of the               -->
<!--  infrastructure on which to base your business.    -->
<!--                                                    -->
<!--  https://www.4isp.it                               -->
<!--                                                    -->
<!--  4ISP{ff4446e3-2b24-400f-a07a-b52522f11c6d}        -->
<!--                                                    -->
<!-------------------------------------------------------->
```

JWT contains:
```
{
  "fresh": false,
  "iat": 1720359020,
  "jti": "af3289f2-0098-49bf-ac0e-3f6363aef8d7",
  "type": "access",
  "sub": "940f8687-e53c-4011-acd9-fd60a3314726",
  "nbf": 1720359020,
  "csrf": "c362c5d6-c038-42d9-84df-cea887300065",
  "exp": 33256359020,
  "glitch1": "Congratulations! You found an ADV Glitch! Result Consulting #1",
  "glitch2": "Systematically acquire new customers with a digital, replicable and effective business development model.",
  "glitch3": "Find out more on https://www.resultconsulting.it/",
  "glitch4": "RESULT{c988a66b-2f5e-414e-af62-7af07788a49b}"
}
```

We stoped here.

# Officil solution

We need to start the therapy, but the feature is disabled for our user.

When we open the webpage, an automatic login is performed and the server returns a JWT token, which the frontend application stores in the browser's session storage.

It turns out that this token has a weak secret. Let's crack it using hashcat and the rockyou wordlist:

```shell
hashcat -m 16500 -a 0 jwt_token.txt rockyou.txt
```
Ah wow, the secret is `NUKENUKE`.

The user id is represented by the `sub` claim in the token. The server will most surely use that value to check for permissions or to perform various database queries. Let's see if we can perform a SQL injection. We can leverage https://jwt.io to edit, sign again the token and put it into the session storage.
It turns out that the following payload, when used against the `sub` claim works:

```sql
' or '1'='1
```
The therapies list now shows a new entry linked to another doctor, of which we can retrieve the uuid by inspecting the data sent from the server.
Let's put that uuid as the value of the `sub` claim, sign and put the new token in the browser's session token.

We are finally able to start the therapy and retrieve the flag.