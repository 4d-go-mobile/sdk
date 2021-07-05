# 4d go mobile (iOS) and OTP

Some throuth about OTP to authenticate with 4d go mobile

## What we need

- server side code to generate OTP code and valide it : https://github.com/mesopelagique/OTP (bugged for some 32bite encoding)
- a way store/retrieve or regenerate the OTP secret by user to be able to check the code send later
- (mobile app) client side code to open a "form" to enter the otp and re-send the authentication request with this otp

## Workflow to login with an OTP

* Mobile app client request the authentication after enter the email and press validating
  * the rest request to authenticate is send and will trigger database method `On Mobile Authentication`
* Server receive the request, and 4D code could be executed  
  * to compute the secret otp key and keep it according to user agent session
  * to send an OTP (generated with secret) by email (or sms it have a user database with mail/phone)
  * to respond by `$0.success:=True` and `$0.verify:=True` and optionnaly some description of OTP mobile form interface wanted like the number of character expected
(`$0.password:=New object("digitOnly": True, "length": 6)`)
* Mobile app client receive the data 
  * and display a form to enter OTP
  * the use enter a code and validate   
    * _(it could also come back en request a new code)_
  * a new request (on validate or when enter the number of otp character is entered) with the otp as additional info (there is `parameters` or `customParameters` to send it, *name to check*)
* Server receive request with the code (and other user info)
  * in `On Mobile Authentication` (or elsewhere if technically if not possible)
  * validate the otp, and respond with `success` equal to `True` or `False` according to the check.
* Mobile app client receive the response from server
 * if success, the mobile client app must show main form with the data etc..
 * if failure, show the message, a new code could be entered to retry, and user could return (or maybe ask also to generate a new otp code? if totp if could be important if expired)
 
## TOTP workflow

If we want to use app like google authenticator, we do not send an OTP, we send the user secret as URL https://github.com/mesopelagique/OTP#scan-a-qr-code-with-the-app (it could be textual or better with QRCode)

Then the secret/URL must be enter in authenticator app and a new code is show every 30s according to secret in this app.

The user could use it directly to authenticate, no need each time to send new OTP code or the URL. 
* so if there is already a secrey for the user, we respond without sending code, but just ask mobile app to show form to fill the otp
* But if the user loose it's secret a way to reset it, a website where you are authenticated could provide it.

## Thing to verify

- [ ] Is `$0.verify:=True` workflow allow us to enter two times in `On Mobile Authentication` or we must do another web entry point to verify the code
