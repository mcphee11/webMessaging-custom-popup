# webMessaging-custom-popup

This is designed as an example on how you can build out your own "widget extension" to offer a popup based on a conversational trigger in realtime. This popup im my example is going to render an iFrame of your choice but the same method can be used for various different use cases. The example is also using [Bootstrap](https://getbootstrap.com/docs/5.0/getting-started/introduction/) for rendering the CSS elements, the rest is native JavaScript with the use of the Genesys WebMessaging SDK used in this example details on this SDK can be found [here](https://developer.genesys.cloud/api/digital/webmessaging/messengersdk/).

    I would recommend having an understanding of how to deploy the default WebMessaging widget before trying to use this example.

The Example HTML page is designed to work with your existing Genesys Cloud WebMessaging "DeploymentId" you need to of already set this up in Genesys as well as the "Flow" this readMe is NOT going to cover these setup items.

To connect the HTML to your environment simply put parameters in the URL to pass them into the code. I have not hard coded them to make it easier to share this example.

    iframe
    deploymentId

for example:

# https://localhost:8080/index.html?iframe=https://www.wikipedia.org&deploymentId=1234567890987654321

You need to enter your own region im based in Australia so I have used the .com.au in the WebMessenger snippet if your in a different region you will need to change this a list of regions can be found [here](https://developer.genesys.cloud/api/rest/). The deploymentId needs to be that of YOUR webMessaging deployment in your Genesys Cloud ORG.

Once you have done that you should be good to go... here is an example screen shot of it working with a payment gateway iFrame i have inserted.


![](/docs/images/example1.png?raw=true)

This widget can then of course be changed to suit themes, logos and complete CSS look and feel as required.


I have placed comments inline in the html file at locations to help explain it. I have also added buttons on the HTML for examples and testing on how to interact with the Genesys SDK. I have also created a function to update and add "customAttributes" to an interaction. This may be useful to append metadata to the interaction as well as make it visual to the agent in the "agent script" inside the Genesys Cloud agent interface.

Rather then listening for the string of "iFrame" you may want to change this. To do so you simply need to change this part of the function to look for the String you are after:

    Genesys("subscribe", "MessagingService.messagesReceived", function (o) {
            try {
                if (o.data.messages[0].text === 'iFrame') {
                    console.log('iFrame popup detected')
                    popIframe()
                }
            } catch (err) {
                console.error(err)
            }
        });

All you need to do is change the "if" statement check to the String you require. You can also do a check on the "direction" as well to ensure the String is coming from either the Agent or the Customer as an option.

If you would prefer the "trigger" to not be based on the conversation transcript you can also use the SDK to subscribe to the customAttributes on the interaction. This way if a key changes you could make this the trigger. I have put an example on this below:

    Genesys("subscribe", "Database.updated", function(){});

Other examples can be embedding a Google Maps based on an address request or any other 3rd party view you would like to show.