Cloudflare has two authentication methods. One is a global API key and the other is "account" or "user" tokens. Account or user tokens are considered more secure because you can limit their scope and function so if your key leaks there's much less that can be done with it. What we are going to do, then is use tokens rather than the global API key for our authentication needs. 

In order to generate a token for yourself you can follow [this](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/). You need to create a token with **DNS edit** scope.

Lets imagine you have generated a token with DNS edit scope and its value is: **EJUj0k4faJxERLyLUwEJ00lxwqw8et6olUi2a-x3**

You also need your account ID which you can find by by logging in to your cloudflare account and going to you domain name. The address in the browser will then look something like:

>     https://dash.cloudflare.com/8bbdhjf84jfbdu8gfhb4i8fjbndkk92/wintersys.uk

The account ID is: **8bbdhjf84jfbdu8gfhb4i8fjbndkk92**

What you now need to do is in your template provide the account ID and the token you have generated as the DNS_SECURITY_KEY setting in your template. The way you do this is by going to your template and setting

>     DNS_SECURITY_KEY="\<account-id\>:::\<api-token\>"

In our case this will be:

>     export DNS_SECURITY_KEY="8bbdhjf84jfbdu8gfhb4i8fjbndkk92:::EJUj0k4faJxERLyLUwEJ00lxwqw8et6olUi2a-x3"

You need to make sure you provide both your account ID and your API token separated by ':::' and the toolkit will unpack both and use them for its needs
