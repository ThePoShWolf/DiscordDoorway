# DiscordDoorway
_Accept discord interactions in azure with ease_

This repo provides a generalized method to accept messages from a discord interaction. It can provide a response and/or pass the interaction to an Azure storage queue for ingestion from another function app (or even the storage REST API).

## Using This Code
To use this code in your own project:

1. create a .net Azure Function app and deploy this code (I like using github actions as an auto-deployment CI/CD chain from a forked repo).
1. Add your Discord bot's application ID (e.g. 952387339597017129) and public key to an app setting called `APP_PUBLICKEY_<application_id>`. E.g. Name: `APP_PUBLICKEY_952387339597017129` Value: <public key>
1. Set your bot's `Interactions Endpoint URL` to the function endpoint. E.g. `https://myFunctionApp.azurewebsites.net/api/Doorway`.
1. If the last step lets you save the field, then you did it all right! If it says something else, like "interactions_endpoint_url: The specified interactions endpoint url could not be verified," then you need to double check your app and app setting.

## Adding Instant Responses
By default, this app will accept any messages that come in to your specified Discord bots (via the APP_PUBLICKEY_ app settings). To respond to specific commands instantly, create an app setting with the command name (and optional sub command group/name). Ex: `APP_COMMAND_<Command>_<SubCommand>`.

You can place a simple text field in the value to be a static response. You can also paste in a [JSON embed object](https://discord.com/developers/docs/resources/channel#embed-object) to the field to make the response be an embed. Ex: `{"title":"My Site","url":"https://www.reddit.com"}`. Title, url, color, description currently supported.

You can also place a simple message or embed JSON within a JSON content property and also optionally set the `private` or `passthru` true/false properties. Setting private to true sets the response to be only visible to the submitter. Setting passthru makes the function pass the original interaction to the queue (instead of skipping because of the canned command). Some examples:

`{"private":true,"content":{"title":"My Site","url":"https://www.reddit.com"}}`

`{"passthru":true,"content":"Thanks for submitting your request! I'll check this out later."}`
