# Ecotricity-API
These are my findings when exploring the Ecotricity API for their new 'charging for charging' system.

<br/>

####/checkDuplicateEmail
This one doesn't look like it's a particularly good idea, it tells you whether or not a particular email address has an account.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/checkDuplicateEmail HTTP/1.1
    email=scotthelme%40hotmail.com

Response:

    {"result":true}

<br/>

The API returns true which seems to indicate the email address is free to be registered. Once I've registered my account I get the following.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/checkDuplicateEmail HTTP/1.1
    email=scotthelme%40hotmail.com

Response:

    {"result":false}

<br/>
####/validateUserName
This is pretty much the same as the email address endpoint above. Provide it a username and it will tell you if an account exists with that username.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/validateUserName HTTP/1.1
    name=ScottHelme

Response:

    {"result":true}

<br/>

Then after registering my username I get the following.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/validateUserName HTTP/1.1
    name=ScottHelme

Response:

    {"result":false}

<br/>
####/getAddresses
Just noting this down but not much to see here, you can look up addresses with postcodes if that's useful.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getAddresses HTTP/1.1
    postcode=m14wb

Response:

    {"result":[{"summaryline":"Premier Inn, 112 Portland Street, Manchester, Greater Manchester, M1 4WB","organisation":"Premier Inn","number":"112","premise":"112","street":"Portland Street","posttown":"Manchester","county":"Greater Manchester","postcode":"M1 4WB","line1":"Portland Street","line2":"Premier Inn","line3":"","town":"Manchester"}]}

<br/>
####/verifyEmail
I can't figure out what this endpoint does. Any combination of valid or invalid values just returns the same thing.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/verifyEmail HTTP/1.1
    email=scotthelme%40hotmail.com
    &username=ScottHelme

Response:

    {"result":true}

<br/>
####/createUserEV
This is called at the end of the account creation process to actually create your user account with the service.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/createUserEV HTTP/1.1
    vehicleSpecification=%282011-%29
    &vehicleRegistration=*snip*
    &password=*snip*
    &deviceId=*snip*
    &lastDigits=test
    &address2=*snip*
    &cardType=test
    &token=test
    &city=*snip*
    &address1=
    &email=scotthelme%40hotmail.com
    &county=*snip*
    &village=
    &vehicleMake=Nissan
    &firstName=scott
    &street=*snip*
    &country=GB
    &houseName=
    &houseNumber=
    &name=ScottHelme
    &lastName=helme
    &hasEnergyAccount=0
    &postcode=*snip*
    &vehicleModel=Leaf

Response:

    {"result":true,"data":{"id":"*snip*","name":"ScottHelme","email":"scotthelme@hotmail.com","title":null,"firstname":"scott","lastname":"helme","phone":"","vehicleAdded":{"result":true},"tokenAdded":{"result":true}}}

<br/>

What's *really* interesting in here is the third from last parameter of the POST request to create the account, hasEnergyAccount=0. I wonder if that was change to 1 or true to indicate that I do have an energy account if I'd get the free charging?..

<br/>
####/login
Called upon login.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/login HTTP/1.1
    electricHighway=true
    &identifier=ScottHelme
    &password=*snip*

Response:

    {"result":true,"data":{"id":"*snip*","token":"","name":"ScottHelme","email":"scotthelme@hotmail.com","firstname":"scott","lastname":"helme","verified":"1","businessPartnerId":"*snip*","phone":"","electricHighwayAccount":true,"accountDetails":{"businessPartnerId":"*snip*","type":"1","firstName":"scott","lastName":"helme","emailAddresses":[{"address":"scotthelme@hotmail.com"},{"address":"scotthelme@hotmail.com","primary":"X"}],"street":"*snip*","village":"*snip*","city":"*snip*","postcode":"*snip*","telephoneNumbers":null},"googleAPIkey":"*snip*"}}

<br/>
####/registerNotifications
Called after login.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/registerNotifications HTTP/1.1
    deviceId=*snip*
    &password=*snip*
    &app=com.ecotricity.electrichighway
    &identifier=ScottHelme
    &deviceType=android

Response:

    {"result":false}

<br/>
####/getCardList
This simply lists out the payment cards associated on your account.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getCardList HTTP/1.1
    identifier=ScottHelme
    &password=*snip*

Response:

    {"result":[{"lastDigits":"*snip*","cardType":"Visa (VISA)","cardId":"000001","cardIcon":"visa"}]}

<br/>
####/getTransactionList
Viewing historic transactions on the account.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getTransactionList HTTP/1.1
    identifier=ScottHelme
    &password=*snip*

Response:

    {"result":{}}

<br/>
####/getUserVehicleList
Same again, this just lists out the vehicles registered to your account. It seems to be used for selecting chargers with the appropriate connector for your car.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getUserVehicleList HTTP/1.1
    identifier=ScottHelme
    &password=*snip*

Response:

    {"result":[{"id":"0000001069","registration":"*snip*","specification":"(2011-)","model":"Leaf","make":"Nissan"}]}

<br/>
####/getLocationDetails
Does what it says on the tin.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getLocationDetails HTTP/1.1
    vehicleSpecification=%282011-%29
    &vehicleModel=Leaf
    &locationId=147
    &vehicleMake=Nissan

Response:

    {"result":{"pump":[{"status":"Swipe card only","latitude":"53.72297","longitude":"-2.486165","name":"BP Ewood","postcode":"BB2 4LA","location":"Bolton Road Blackburn","locationId":"147","pumpId":"1257","lastHeartbeat":"","pumpModel":"AC (RAPID) \/ DC (CHAdeMO)","connector":[{"compatible":"X","type":"DC (CHAdeMO)","status":"Swipe card only","name":"DC (CHADEMO)","connectorId":"1","sessionDuration":"20"},{"compatible":"","type":"AC (RAPID)","status":"Swipe card only","name":"AC (RAPID)","connectorId":"2","sessionDuration":"20"}]}]}}

<br/>
####/getPumpList
Lists pumps close to your location using lat/lon.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getPumpList HTTP/1.1
    latitude=55.378051
    &vehicleSpec=%282011-%29
    &vehicleMake=Nissan
    &vehicleModel=Leaf
    &longitude=-3.435973

Response:

    {"result":[{"latitude":"55.219691","longitude":"-3.410582","name":"Annandale Water Services","postcode":"DG11 1HD","location":"A74(M) Jct 16","locationId":"111","pumpId":["1201"],"pumpModel":"AC (RAPID) \/ DC (CHAdeMO)","available":false,"swipeOnly":true,"distance":17.682033646467}, ... }

<br/>

That response is a much larger list that I've cut down. It also seems to tell you your distance from the charger.

<br/>
####/getPumpConnectors
This lists the available connectors on a charger and seems to require auth so that it can tell which which (if any) connector is for your car.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getPumpConnectors HTTP/1.1
    password=*snip*
    &deviceId=*snip*
    &vehicleId=*snip*
    &vehicleMake=Nissan
    &identifier=ScottHelme
    &vehicleModel=Leaf
    &pumpId=1257

Response:

    {"result":{"status":"Swipe card only","latitude":"53.72297","longitude":"-2.486165","name":"BP Ewood","postcode":"BB2 4LA","location":"Bolton Road Blackburn","pumpId":"1257","connector":[{"compatible":"X","type":"DC (CHAdeMO)","status":"Swipe card only","name":"DC (CHADEMO)","connectorId":"1","sessionDuration":"20"},{"compatible":"","type":"AC (RAPID)","status":"Swipe card only","name":"AC (RAPID)","connectorId":"2","sessionDuration":"20"}],"connectorCost":[{"connectorId":"1","totalCost":"5.00 ","baseCost":"5.00 ","discountEcoGrp":"0.00 ","discountMultiChg":"0.00 ","surcharge":"0.00 ","freecost":"","currency":"GBP","sessionId":"00000095","sessionDuration":"20"},{"connectorId":"2","totalCost":"5.00 ","baseCost":"5.00 ","discountEcoGrp":"0.00 ","discountMultiChg":"0.00 ","surcharge":"0.00 ","freecost":"","currency":"GBP","sessionId":"00000096","sessionDuration":"20"}]}}

<br/>

If the charger can be used by your car you can see the "compatible":"X" in the response for the appropriate charger. It's interesting to note there is also a "discountEcoGrp" and "discountMultiChg" value set to 0.

<br/>
####/getSettings
This POST request has no params.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getSettings HTTP/1.1

Response:

    {"result":true,"autocomplete":"1","amplitude":"1","google_maps_key":"*snip*","amplitude_key":"*snip*"}

<br/>
####/forgottenUsername
Triggered when you click forgotten username in the app.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/forgottenUsername HTTP/1.1
    email=scotthelme%40hotmail.com

Response:

    {"result":true}

<br/>
####/forgottenPassword
Triggered when you request a password reset.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/forgottenPassword HTTP/1.1
    platform=android
    &email=ScottHelme

Response:

    {"result":{"hashkey":"*snip*"}}

<br/>

You need to hit the following URL to activate the token for the next step: https://www.ecotricity.co.uk/ecovalidate/token/(hashkey)

<br/>
####/getPasswordToken
Triggered when you hit forgotten password in the app, the hashkey value is returned in the previous API call.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getPasswordToken HTTP/1.1
    platform=android
    &hashkey=*snip*

Response:

    {"result":{"success":false,"error":"TOKEN_NOT_VALIDATED"}}

<br/>

Once you have clicked the activation link in the email.

Response:

    {"result":{"success":true,"hashkey":"*snip*"}}

<br/>

Note: The hashkey provided here is a new value.

<br/>
####/usePasswordToken
Final step in the password reset process, provide the new password.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/usePasswordToken HTTP/1.1
    platform=android
    &hashkey=*snip-hashkey-forgottenPassword*
    &password=*snip*
    &forgotpassword_hash_key=*snip-hashkey-getPasswordToken*
    &confirm_password=*snip*

<br/>
####/changeEmail
Used to change email address on account.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/changeEmail HTTP/1.1
    newEmail=*snip*
    &identifier=ScottHelme
    &password=*snip*

Response:

    {"result":true}

<br/>

The email validation link is https://www.ecotricity.co.uk/ecovalidate/change-email/(token)

<br/>
####/registerVehicle
Add a new car to your account.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/registerVehicle HTTP/1.1
    vehicleModel=Leaf
    &password=*snip*
    &vehicleMake=Nissan
    &identifier=ScottHelme
    &vehicleRegistration=MY64ABC
    &vehicleSpecification=%282011-%29

Response:

    {"result":true}

<br/>
####/unregisterVehicle
Remove a vehicle from your account.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/unregisterVehicle HTTP/1.1
    vehicleSpecification=%282011-%29
    &password=*snip*
    &vehicleModel=Leaf
    &identifier=ScottHelme
    &vehicleRegistration=MY64ABC
    &vehicleMake=Nissan
    &vehicleId=*snip*

Response:

    {"result":true}

<br/>
####/changePassword
Used to change password in your account. Just provide old password and new password.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/changePassword HTTP/1.1
    newPassword=*snip*
    &identifier=ScottHelme
    &password=*snip*

Response:

    {"result":true}

<br/>
####/startChargeSession
Used to actually start a charging session.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/startChargeSession HTTP/1.1
    password=*snip*
    &deviceId=*snip*
    &identifier=ScottHelme
    &pumpConnector=1
    &pumpId=1263
    &cv2=*snip*
    &cardId=000001
    &sessionId=00000228

Response:

    {"result":true}

<br/>
####/getChargeStatus
Used to get the status of a charge session.

Request:

    POST https://www.ecotricity.co.uk/api/ezx/v1/getChargeStatus HTTP/1.1
    deviceId=*snip*
    &vehicleMake=Nissan
    &sessionId=00000228
    &vehicleModel=Leaf
    &pumpConnector=1
    &pumpId=1263

Response:

    {"result":{"status":"Awaiting Start","message":"Awaiting Start","completed":false,"cost":"","sessionId":"00000228","pumpId":"1263","pumpConnector":"1"}}

<br/>

The charge here was never actually initiated. I assume there are other status messages we can receive.

Response:

    {"result":{"status":"Retry","message":"Sorry there's been problem please disconnect your vehicle and try again.","completed":false,"cost":"","sessionId":"00000228","pumpId":"1263","pumpConnector":"1"}}

<br/>
####Other bits
There is a list of what seems to be all supported cars here:
https://www.ecotricity.co.uk/api/ezx/v1/getVehicleList

You can read the T&C's if you *really* want to here:
https://www.ecotricity.co.uk/api/ezx/v1/terms?eh=true

The app seems to load the T&C doc every time you open it, bit of a waste.
