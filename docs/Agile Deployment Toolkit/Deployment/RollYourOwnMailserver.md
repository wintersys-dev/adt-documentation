#### ROLL YOUR OWN MAILSERVER

The way that this toolkit is designed, if you want to use the authentication servers, for example, it expects you to issue your users with an email address with a specified custom domain so that only email addresses for that custom domain are considered valid. Now you can use "Cloudflare Email Routing" to route emails for your custom domain to the user's end account which could be a gmail address, or a yahoo address and so on. But alternatively you could give someone a job in your organisation of running and maintaining an email server for your domain and this would be "rolling your own solution" in terms of email provision because you can provide custom domain email addresses to your users and that means that because you control their email addresses you can issue them with security sensitive items based on their email address domain because no one should have one of your email addresses without them being considered as above board by you. So, I made a reconnoiter of the different self hosted email solutions that are out there and this the offerings that I liked the look of 

There are a few solutions to providing email services for your users so, in no particular order:

1. [iRedmail](https://www.iredmail.org)
2. [Mail in a box](https://mailinabox.email)
3. [Modoba](https://modoboa.org/en)
4. [Postie](https://poste.io)
5. [Stalwart](https://stalw.art)

And so some of these solutions provide commercial offerings where they will host your email servers for you using their product but a) you have less control if you do that and b) it costs £

So, the advice if you are serious about using my software for secured systems is to set aside some resources for running your own mail server and if you do that you will be able to make fuller use of what the ADT is offering you in terms of secured systems because the authentication servers will be able to be included in your build chain.  

If you would rather pay for an email service with unlimited domains and unlimited mailboxes you could try "mxroute.com" or "migadu" although I haven't used either and cannot vouch for security, quality of service or reliability for either of them. 

